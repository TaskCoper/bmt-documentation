# Kết quả kiểm thử backend đăng nhập Google

Phần backend đã qua kiểm thử cục bộ ngày 01/10/2026: **1.913 ca đạt, không có ca lỗi hoặc bị bỏ qua trong các lượt cuối dưới đây**. Kết quả này gồm các bộ hồi quy dùng chung và kiểm thử Google; không tương đương nghiệm thu giao diện hay Google Cloud thật.

| Bộ kiểm | Phạm vi lượt cuối | Đạt | Lỗi / bỏ qua |
| --- | --- | ---: | --- |
| Application | Toàn bộ `bmt-be.application.tests` | 1.607 | 0 / 0 |
| Infrastructure | Toàn bộ `bmt-be.infrastructure.tests` | 275 | 0 / 0 |
| API | Các lớp có tên Google, gồm HTTP với DB/Redis thật | 16 | 0 / 0 |
| Integration | Các lớp Google và SessionTokenStoreRedis | 15 | 0 / 0 |

Một lượt toàn bộ API trước các chỉnh sửa cuối đã đạt 436 ca; số này không cộng vào bảng trên. Sau chỉnh sửa cuối đã chạy lại nhóm Google, chưa chạy lại toàn bộ API. Hai lỗi thư viện từng xuất hiện trong lần chạy trung gian đã không còn ở lượt application cuối; không giữ kết luận lỗi từ lượt cũ làm kết quả hiện tại.

Bằng chứng bền vững: [tóm tắt lượt chạy](../../bmt-be/results/google-login-test-summary.json) và [kết quả database-flow-validator](../../bmt-be/results/database-flow-validator.json). Build/test dùng `--no-restore`; không đổi dependency. Hai file compose đã qua `config --quiet` với `.env.sample`; `git diff --check` không báo lỗi tại thời điểm kiểm.

## Những hành vi đã quan sát

| Nhóm | Bằng chứng thực thi |
| --- | --- |
| Google và client proof | JWT RSA kiểm chữ ký, issuer, audience/azp, nonce, thời gian và email; PKCE có UUID attempt, verifier 43–128 ký tự; callback chưa gọi handler tạo tài khoản; code chỉ hoàn tất một lần và không tráo web/mobile. POST token không tự thử lại; GET JWKS lỗi mạng tối đa hai lần gọi; kiểm giới hạn response. |
| Xác minh email | Mã giữ được số 0 đầu, HMAC gắn attempt/sub/email/code; lần thứ năm đúng được chấp nhận, giới hạn tùy cấu hình; lỗi xếp thư đóng attempt. Consumer chỉ gửi mã còn khớp HMAC hiện tại. |
| Tài khoản và dữ liệu | PostgreSQL thật kiểm unique/FK, `xmin`, đọc lại dưới khóa User, tám lệnh cùng sub không tạo trùng, hai sub cùng email dùng một User. Tài khoản, link, role và outbox cùng transaction; lỗi publish rollback. |
| Migration | Chạy từ schema trước Google; dừng và rollback khi email không hợp lệ hoặc trùng kể cả tài khoản đã xóa; giữ hash cũ, xóa mã cũ, không tự tạo bằng chứng xác minh email. |
| Phiên và credential | Mật khẩu tạm giữ giới hạn; Google không mang giới hạn đó; refresh giữ method/stamp; reset cần proof email; đổi mật khẩu đổi stamp. Redis thật kiểm dọn tác vụ cũ không xóa phiên thuộc thế hệ mới. DB xác thực lỗi trả 503 và không xóa cookie. |
| HTTP đến dữ liệu lưu | `GoogleBackendFlowTests`: start/callback chưa có User; complete có đúng User/link/role/thư mã hóa; temp login không sửa được hồ sơ; Google cũng không đổi được email; đổi mật khẩu thu hồi token Google cũ; lượt Google mới không sinh thêm thư. |
| Thư và khóa | RabbitMQ thật nhận ciphertext, retry lỗi SMTP giả rồi đưa vào queue lỗi có TTL bảy ngày; không có To/mật khẩu/diagnostic SMTP rõ trong event lỗi. Keyring thật với certificate thử nghiệm giữ link cũ, giải mã sau restart/rotation và từ chối thiếu certificate cũ. |
| Hợp đồng API và log | Origin web bắt buộc kể cả Bearer, DTO lạ bị chặn, web bỏ token JSON/mobile không cookie, 202 chỉ trả dữ liệu bước email. Bộ lọc callback loại event chứa query nhạy cảm tại API. |

Các lớp test nằm trong `test/bmt-be.application.tests/usecases/user/Google*.cs`, `ProtectedAuthMailTests.cs`, `test/bmt-be.infrastructure.tests/authentication/`, `test/bmt-be.api.tests/security/Google*.cs` và `test/bmt-be.integration.tests/Google*.cs`. Đây là danh mục bằng chứng theo nhóm; không tự đánh dấu từng đặc tả UT-AUTH-038–110 là Pass nếu chưa đối chiếu hết từng assertion của đặc tả.

## Điều kiện thử và dọn dữ liệu

PostgreSQL, Redis và RabbitMQ chạy trong container riêng. HTTP test dùng pipeline Carter/MediatR/transaction/session thật, thay Google verifier bằng identity thử nghiệm; bài verifier riêng dùng JWT có chữ ký RSA thật do test sinh. SMTP dùng test double, không gửi thư cho khách. Test không đọc dữ liệu khách thật và không chạy migration lên database đang dùng.

HTTP test lưu UserId và DeliveryId để nối request với bản ghi trong cùng lượt. Hai container `bmt-google-http-postgres` và `bmt-google-http-redis` chạy với `--rm`, không mount dữ liệu vào workspace; đã dừng và xác nhận không còn trong `docker ps -a`. Container của các bài integration do fixture Testcontainers tự dọn. File bằng chứng chỉ giữ ID thử nghiệm và kết quả, không giữ token, email hay mật khẩu.

## Chưa nghiệm thu

- Google test project, consent, callback qua internet và App Links/Universal Links trên thiết bị thật; giao diện web, Android và iOS nằm ngoài phạm vi backend lần này.
- SMTP thật, sự cố tiến trình đúng thời điểm sau gửi, phục hồi backup theo quy trình vận hành và quyền truy cập khóa của môi trường triển khai.
- Proxy/Cloudflare không ghi query callback, log của hạ tầng bên ngoài API và tải thực tế sau khi mỗi request đọc security stamp từ DB.
- Mọi biến thể cạnh tranh và gây lỗi tại từng điểm trong [12 nhóm integration của đặc tả](google-login-unit-test-coverage.md), gồm race với đăng ký/Staff và crash tại từng ranh giới commit. Bộ hiện có kiểm các tình huống nêu ở bảng, chưa chứng minh mọi tổ hợp trong kế hoạch.

Google vẫn mặc định tắt. Xem [bàn giao backend](google-login-backend-handoff.md) để chuẩn bị cấu hình và migration trước lần nghiệm thu môi trường.
