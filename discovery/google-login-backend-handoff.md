# Bàn giao backend đăng nhập Google

Backend đã có luồng Google cho khách hàng và API riêng cho web, Android/iOS. Tính năng mặc định tắt. Phạm vi lần này là mã nguồn backend, migration và kiểm thử cục bộ; chưa cấu hình Google Cloud, tích hợp giao diện hay triển khai dịch vụ.

Hiện đang chờ khách hàng cung cấp các biến cấu hình Google. Theo yêu cầu ngày 01/10/2026, mã nguồn được đẩy lên backend `develop` và tài liệu lên `main` trước; giữ `GOOGLE_AUTH_ENABLED=false` cho đến khi có cấu hình và hoàn tất kiểm tra môi trường.

Ngày cập nhật: 01/10/2026. Nghiệp vụ và TDD đã được người dùng chốt trong hội thoại; Reviewer/Approver: Tân Trần. Căn cứ: [STORY-AUTH-002](../userstory/STORY-AUTH-002.md), [TDD-AUTH-003](../tdd/TDD-AUTH-003.md), BR-AUTH-003–007 và các phần phiên đăng nhập trong TDD-AUTH-001/002.

## Hành vi đã triển khai

- Khách mới được tạo tài khoản Customer/Active và vai trò `customer`. Google thiếu tên thì lưu NULL; không tự đặt tên thay khách.
- Liên kết đã có được tìm bằng Google `sub`. Google đổi email không làm đổi email hay hồ sơ BMT. Khi chưa có liên kết, chỉ tự đối chiếu email nếu Google cung cấp đủ bằng chứng; email ngoài Gmail/Workspace cần nhập mã gửi đến hộp thư đó.
- Tài khoản đã có bằng chứng xác minh đúng email hiện tại giữ mật khẩu và dữ liệu. Nếu thiếu bằng chứng, hệ thống thay credential bằng mật khẩu tạm, đổi security stamp, vô hiệu mã cũ và thu hồi phiên cũ.
- Mật khẩu tạm không tự hết hạn. Khách dùng mật khẩu tạm phải đổi mật khẩu trước khi tiếp tục; phiên Google vẫn dùng bình thường. Đổi hoặc đặt lại mật khẩu thành công thu hồi mọi phiên cũ, kể cả Google.
- Email BMT bất biến trên API hồ sơ dùng chung. Gửi email tương đương sau chuẩn hóa được chấp nhận nhưng không ghi lại cột Email; các thông tin khác vẫn sửa được theo quyền của phiên.
- Mật khẩu tạm chỉ gửi ở lần tạo hoặc thay credential cần thiết. Không có API xem lại/gửi lại mật khẩu tạm; khách dùng Quên mật khẩu khi cần phục hồi.

## API để tích hợp giao diện sau này

| Client | Bắt đầu | Hoàn tất | Nhập mã email |
| --- | --- | --- | --- |
| Web | `POST /api/v1/users/google/start` | `POST /api/v1/users/google/complete` | `POST /api/v1/users/google/verify-email` |
| Android/iOS | `POST /api/v1/mobile/auth/google/start` | `POST /api/v1/mobile/auth/google/complete` | `POST /api/v1/mobile/auth/google/verify-email` |

Callback chung: `GET /api/v1/auth/google/callback`. Ví dụ request/response đầy đủ nằm trong [Internal API của TDD](../tdd/TDD-AUTH-003.md#internal-api).

Client tự sinh và giữ `codeVerifier` đúng RFC 7636: 43–128 ký tự trong tập chữ ASCII, số, `-._~`. Start gửi S256 `codeChallenge` dài 43 ký tự và `codeChallengeMethod="S256"`; mobile thêm `platform="Android"` hoặc `"iOS"`. Backend trả `attemptId` dạng UUID, URL Google và hạn của lượt đăng nhập. URI trả về lấy từ cấu hình, không nhận từ request.

Callback chỉ xác thực Google và chuyển về URI đã lưu với fragment `#attemptId=...&completionCode=...`; chưa tạo User, chưa cấp cookie. Client gửi mã này cùng verifier của chính lượt đăng nhập để hoàn tất. HTTP 202 có `status="EmailVerificationRequired"`, `attemptId`, `verificationTicket`, `maskedEmail`, `expiresAtUtc`; gửi lại `verificationTicket`, verifier và `code` dạng chuỗi sáu chữ số ở verify-email. Không dùng tên trường `ticket`.

HTTP 200 của web cấp cookie HttpOnly và bỏ hẳn accessToken/refreshToken khỏi JSON. Mobile trả token trong JSON, không đặt cookie. HTTP 202 không cấp phiên mới; UI phải tiếp tục bước nhập mã dù trình duyệt đang có cookie cũ. Web start/complete/verify-email bắt buộc Origin trong danh sách cho phép, kể cả request có Bearer token. DTO Google từ chối trường lạ.

Giao diện cần xóa fragment sau khi đọc, không tải analytics trên trang nhận mã, dùng verified HTTPS App Links/Universal Links cho mobile và giữ verifier ngoài URL/log. Những phần giao diện này chưa được triển khai trong workspace backend.

## Tên cấu hình thực tế

Implementation gộp `GoogleSignInOption` và `AuthMailProtectionOption` của bản thiết kế vào **`GoogleAuthOptions`**. `RedirectUri` của thiết kế là `CallbackUri` trong code; `EmailCodeHmacActiveKeyId` là `EmailCodeKeyId`. Không đọc các biến giữ chỗ `GOOGLE_OAUTH_*` cũ.

Khi chạy trực tiếp, dùng biến môi trường dạng `GoogleAuthOptions__ClientId`. Hai file compose đã ánh xạ các biến `GOOGLE_AUTH_*` trong [mẫu môi trường](../../bmt-be/.docker/.env.sample). Giá trị secret phải cung cấp riêng cho từng môi trường.

| Option | Giá trị/mục đích |
| --- | --- |
| `Enabled` | Mặc định false; route Google trả 503 khi tắt. |
| `ClientId`, `ClientSecret` | OAuth Web Client của Google; backend đổi authorization code. |
| `CallbackUri` | URI HTTPS callback API, khớp chính xác trong Google Cloud. |
| `WebReturnUri`, `AndroidReturnUri`, `IosReturnUri` | URI HTTPS cố định, không userinfo/query/fragment. |
| `EmailCodeKeyId`, `EmailCodeKeys__<id>` | HMAC key hiện dùng và tập khóa base64, mỗi khóa ít nhất 32 byte ngẫu nhiên, tách khỏi khóa JWT. |
| `KeyRingPath` | Thư mục lưu Data Protection keyring bền vững; compose dùng `/app/data-protection-keys`. |
| `CertificatePath`, `CertificatePassword` | Certificate có private key; compose mount `/run/secrets/bmt-auth/keyring.pfx` từ `AUTH_SECRETS_DIRECTORY`. |
| `PreviousCertificatePaths__0`, `__1`… | Certificate cũ còn cần để giải mã keyring sau xoay khóa; dùng cùng cấu hình mật khẩu PFX hiện có. |
| `AttemptLifetimeMinutes` | 10 phút. |
| `CompletionCodeLifetimeSeconds` | 120 giây, không vượt hạn attempt hoặc token Google. |
| `EmailVerificationLifetimeMinutes` | 10 phút, không vượt hạn attempt hoặc token Google. |
| `MaxEmailAttempts` | 5; lần cuối nhập đúng vẫn thành công. |
| `EmailSendLimitPerHour` | 3 thư/email đã chuẩn hóa/giờ. |
| `StartLimitPerIp`, `StartWindowMinutes` | 30 lượt start/IP/10 phút. |
| `TokenHttpTimeoutSeconds`, `CallbackTimeoutSeconds` | 10 và 15 giây; POST đổi code không tự thử lại. |
| `TemporaryPasswordDeliveryLifetimeHours` | 24 giờ để xử lý yêu cầu gửi thư; không phải hạn của mật khẩu. |
| `ErrorQueueRetentionDays` | 7 ngày cho queue `send-protected-auth-email_error`. |
| `MaxRequestBodyBytes`, `MaxCallbackQueryBytes` | 4096 và 8192 byte. |
| `MaxGoogleResponseBytes`, `MaxIdTokenBytes` | 65536 và 16384 byte. |

Các giới hạn số phải dương. Khi bật Google, thiếu cấu hình bắt buộc, certificate/private key, quyền ghi keyring hoặc khả năng giải mã khóa cũ sẽ làm khởi động thất bại. Startup kiểm cả keyring đã lưu và một lần mã hóa/giải mã thử. Backend giữ application name `bmt-be` để tương thích dữ liệu Data Protection đang có.

Lần đầu bật bảo vệ certificate, backend mã hóa phần khóa đang lưu rõ trong các file keyring hiện có, giữ nguyên ID và vật liệu khóa để link cũ tiếp tục dùng được. Phải sao lưu keyring và certificate theo quy trình môi trường trước bước này. File khóa được thay bằng file tạm đã mã hóa; không tạo bản sao rõ mới. Không chạy nhiều instance cùng chuyển đổi keyring. Workspace hiện dùng volume cho một instance; trước khi mở rộng cần một nơi lưu khóa dùng chung.

Khi xoay HMAC, thêm `EmailCodeKeys__v2`, chuyển `EmailCodeKeyId=v2` và giữ khóa v1 đến khi mọi attempt dùng v1 hết hạn. Compose mẫu chỉ khai khóa v1: bổ sung mapping cho v2 khi xoay. Khi xoay certificate, mount certificate cũ và khai `PreviousCertificatePaths`; chưa bỏ khóa cũ khi thư hoặc link còn cần nó. Tắt Google vẫn phải giữ certificate/keyring đã sử dụng.

## Dữ liệu và thứ tự đưa vào môi trường

Migration mới: `20260930162840_AddGoogleCustomerAuthentication`. Migration thêm EmailKey sinh bằng PostgreSQL `lower(btrim(Email))`, bằng chứng VerifiedEmailKey, liên kết UserGoogleLogin với unique sub, tên User cho phép NULL và concurrency token `xmin`. Unique EmailKey tính cả tài khoản đã xóa; không gộp dấu chấm hoặc alias `+` của Gmail. Mã verify/reset cũ về 0; hash mật khẩu cũ giữ nguyên; không suy bằng chứng email từ cờ IsEmailVerified cũ.

Preflight dừng khi email trống/không hợp lệ hoặc trùng khóa chuẩn hóa. Không tự xóa hay gộp tài khoản. Migration dùng PostgreSQL `xmin` có sẵn, không thêm cột hệ thống này. Hàm Down chủ động từ chối rollback schema làm mất liên kết hoặc thu hẹp email; cần migration sửa tiến hoặc phục hồi bản sao lưu đã kiểm.

1. Kiểm bản migration cùng các migration đang có trong nhánh; chạy thử trên bản sao dữ liệu. Lần làm này chỉ chạy DB/container dùng riêng cho kiểm thử.
2. Cấp cấu hình OAuth, URI, CORS, SMTP, HMAC, certificate và keyring bền vững; giữ Enabled=false trong bước chuẩn bị.
3. Áp dụng schema và triển khai các writer User/credential cùng phiên mới theo kế hoạch phát hành. Không trộn writer cũ không khóa User với writer mới trong thời gian chuyển đổi.
4. Kiểm startup, proxy không ghi query callback, SMTP thử nghiệm, đường quay về web/mobile và Google test project. Chỉ bật Enabled=true khi môi trường đạt các kiểm tra này.

Tài khoản, role, liên kết và event thư mã hóa nằm trong cùng transaction PostgreSQL. Cấp phiên Redis diễn ra sau commit. Nếu Redis lỗi sau commit, API trả 503 nhưng User/link/event vẫn tồn tại; khách bắt đầu lượt mới sẽ dùng lại tài khoản, không tạo thêm mật khẩu tạm. API đọc stamp từ DB mỗi request; Redis cleanup chỉ dọn dữ liệu cũ và không quyết định phiên có hợp lệ hay không.

Outbox và RabbitMQ chỉ lưu ciphertext cho thư mới này. Consumer kiểm hạn, credential hiện tại hoặc mã HMAC đang chờ trước SMTP. SMTP có thể gửi trùng nếu tiến trình chết sau gửi; bản sao vẫn là cùng mật khẩu/mã. Queue lỗi chỉ được phát lại khi event còn hạn và còn đúng credential. Quy tắc mã hóa này không tự chuyển đổi các event email cũ của tính năng khác.

## Kiểm chứng và phần còn lại

Xem [kết quả kiểm thử backend](google-login-backend-validation.md). Bộ kiểm local dùng Google identity giả ở bài HTTP và JWT RSA tự ký ở bài verifier; PostgreSQL, Redis và RabbitMQ chạy thật trong container riêng. Không có thư gửi đến khách thật.

Còn cần nghiệm thu với Google project thật, hộp thư thử nghiệm, cấu hình proxy/Cloudflare và giao diện web/Android/iOS. Chưa coi 40 System Test hoặc toàn bộ đặc tả UT-AUTH-038–110 là đã đạt chỉ từ số lượng xUnit chạy được. Các trường Owner/Sprint/Assignee/Effective Date đang để mở trong tài liệu nguồn không được tự điền bằng tên Reviewer.
