# Chống CSRF cho API dùng cookie đăng nhập

**Trạng thái: Đã trả phần code ngày 26/09/2026, chưa đóng — còn chờ merge và đặt cấu hình trên các môi trường thật.** Thiết kế chính thức nằm ở [TDD-AUTH-001](../tdd/TDD-AUTH-001.md); code ở commit `50ed2f3` trên nhánh `feature/csrf-protection` của `bmt-be`, chưa merge vào `develop`. Phần còn lại của ghi chú này là bản nháp phân tích ngày 26/09/2026, giữ lại làm lịch sử: số dòng, hiện trạng và các câu hỏi bên dưới là trước khi chốt, không còn là yêu cầu.

Người dùng đã chốt ngày 26/09/2026 (chi tiết ở TDD-AUTH-001/Context & Goals):

- Phương án A, một middleware kiểm `Origin`/`Referer` cho mọi request ghi; request có `Authorization` được cho qua; chỉ miễn webhook SePay bằng `.SkipCsrfCheck()`; mã lỗi chung `CsrfInvalid` (403).
- Câu 1: frontend khác site với API. Câu 2: chọn (a), request mang cookie phiên mà thiếu cả `Origin` lẫn `Referer` bị chặn. Câu 3: cookie phiên `SameSite=None; Secure` ngoài Development, SameSite đọc từ cấu hình. Câu 4: `http://localhost:3000` chỉ tự thêm ở Development; cấu hình sai thì từ chối khởi động. Câu 5: bỏ antiforgery token, `X-CSRF-Token`, `GET /api/v1/antiforgery/token`, `X-BMT-Request`; thiết kế chính thức ở TDD nền tảng. Câu 6: chọn (a), bật chặn ngay ở mọi môi trường, **không** làm chế độ `ReportOnly` (tùy chọn `Csrf:Mode` ở mục Cấu hình bên dưới đã bỏ).
- Khác bản nháp khi triển khai: forwarded headers phải bật cả `X-Forwarded-For`, vì ASP.NET Core 8 chỉ kiểm proxy tin cậy trong nhánh đó (TDD-AUTH-001/Architecture Notes).

Điều kiện đóng nợ: (1) nhánh `feature/csrf-protection` được merge; (2) mỗi môi trường đã đặt `CORS_ALLOWED_ORIGINS` đúng và `FORWARDED_HEADERS_KNOWN_NETWORKS` hoặc `FORWARDED_HEADERS_KNOWN_PROXIES` cho `cloudflared`; (3) đã thử trên môi trường thử rằng request ghi từ origin lạ nhận 403 `CsrfInvalid` và frontend thật vẫn hoạt động. Khi đạt cả ba thì đổi trạng thái ở đầu tài liệu thành đã đóng.

## Tóm tắt

Backend nhận access token từ cookie khi request không có header `Authorization`. Vì vậy trình duyệt tự gửi kèm phiên đăng nhập khi một trang khác gửi request tới API. Kiểu tấn công này gọi là CSRF (giả mạo request từ trang khác). Hiện code chưa có lớp chống CSRF nào.

Hầu hết endpoint ghi đang được che chắn một cách tình cờ, vì chúng chỉ nhận body JSON. Tuy vậy, 5 endpoint không cần body vẫn gửi giả được: khóa, mở khóa và buộc đăng xuất nhân viên, đăng xuất, và làm mới phiên. Tài liệu cũng đang mô tả ba cơ chế và ba mã lỗi khác nhau cho cùng một việc.

Đề xuất: thêm **một middleware dùng chung** kiểm header `Origin` (dự phòng bằng `Referer`) theo danh sách origin được phép, áp cho mọi request ghi. Không dùng antiforgery token của ASP.NET. Frontend chạy trên trình duyệt không phải sửa gì, miễn origin của nó nằm trong danh sách. Request bị chặn nhận 403 với mã `CsrfInvalid`.

## Vì sao cần

CSRF xảy ra khi người dùng đang đăng nhập BMT mở một trang khác, và trang đó tự gửi request tới API BMT. Trình duyệt gửi kèm cookie `accessToken`, nên backend coi request đó như do chính người dùng gửi. Trang lạ không đọc được phản hồi, nhưng thao tác ghi đã xảy ra.

CORS không ngăn được việc này. CORS chỉ quyết định trang lạ có được **đọc** phản hồi hay không. Với "request đơn giản" (POST từ form hoặc `fetch` không có header tùy chỉnh, kiểu nội dung `text/plain`, form hoặc multipart), trình duyệt gửi đi luôn mà không hỏi trước (preflight).

## Hiện trạng đã kiểm tra

### Code backend

| Điểm | Vị trí | Ghi chú |
|---|---|---|
| Access token và refresh token nằm trong cookie HttpOnly `accessToken`, `refreshToken` | `src/bmt-be.presentation/abstractions/AuthCookieHelper.cs:10-11, 13-41` | Login, `verify_account`, `verify_change_password_code` và `refresh_token` đặt cookie. |
| Cờ cookie phụ thuộc `Request.IsHttps` | `AuthCookieHelper.cs:63-76` (dòng 65, 70, 71) | HTTPS: `Secure=true`, `SameSite=None`. HTTP: `Secure=false`, `SameSite=Lax`. |
| Lấy token từ cookie khi không có header `Authorization` | `src/bmt-be.api/dependencyInjection/extensions/JwtExtensions.cs:45-56` | Header `Authorization` luôn được ưu tiên hơn cookie. |
| Token sai thì xóa cookie | `JwtExtensions.cs:111-114` | Một request giả mang token hỏng có thể làm người dùng mất cookie; chặn CSRF trước bước xác thực sẽ tránh được. |
| `refresh_token` và `logout` đọc cookie trực tiếp | `src/bmt-be.presentation/apis/user/UserApi.cs:169, 198` | `refresh_token` không gắn `RequireAuthorization` nhưng vẫn dùng cookie. Vì vậy phải xét theo việc request **có cookie**, không theo việc endpoint có yêu cầu đăng nhập. |
| CORS đọc `Cors:AllowedOrigins`, luôn cộng thêm `http://localhost:3000` | `src/bmt-be.api/dependencyInjection/extensions/ServiceCollectionExtensions.cs:16-41` (dòng 18 cộng sẵn localhost, dòng 25 loại `*`) | Dùng `AllowCredentials`, `AllowAnyHeader`, `AllowAnyMethod`. `localhost:3000` được cho phép ở mọi môi trường, kể cả production. |
| Thứ tự pipeline | `src/bmt-be.api/Program.cs:101-125` | ExceptionHandling → Swagger (Development/Staging) → CORS (108) → Localization → Routing (118) → Authentication (119) → Authorization (120) → RateLimiter (122) → Carter (125). |
| Không có `UseForwardedHeaders` | `Program.cs` (cả file), `.docker/compose.yaml:57` | API nghe `http://+:8080` sau Cloudflare Tunnel (`.docker/compose.yaml:47-52, 227-229`). Không có biến `ASPNETCORE_FORWARDEDHEADERS_ENABLED`. Suy ra trên môi trường đã triển khai, `IsHttps` là `false`, nên cookie đang là `Secure=false`, `SameSite=Lax`. **Chưa kiểm trên môi trường thật.** |
| JWT không có claim định danh chuẩn (`sub`, `NameIdentifier`, `upn`) | `src/bmt-be.infrastructure/authentication/AccessClaimsBuilder.cs:60-69` | Có `UserId` riêng và `ClaimTypes.Expired` đổi theo mỗi token (dòng 67). Điều này ảnh hưởng tới antiforgery của ASP.NET, xem phương án C. |
| Swagger UI gửi kèm cookie | `src/bmt-be.api/dependencyInjection/extensions/SwaggerExtensions.cs:101-102`, `options/ConfigureSwaggerOptions.cs:14-18` | Swagger chạy cùng origin với API, nên request ghi có `Origin` là origin của API. |
| Webhook SePay không dùng cookie | `src/bmt-be.presentation/apis/payment/SePayWebhookApi.cs:38-39` | `AllowAnonymous`, xác thực bằng HMAC trên raw body. |
| Header `Idempotency-Key` | `src/bmt-be.presentation/abstractions/IdempotencyKey.cs` | Header này không nằm trong danh sách header "đơn giản", nên trình duyệt phải preflight. Thiếu header thì validator từ chối. |
| Không có code CSRF, antiforgery, kiểm Origin hay Referer | tìm trong `src/`, `test/`, `.docker/` | Chỉ thấy tên DLL antiforgery trong file `*.lscache`, không có lời gọi. |
| Không có test chạy qua pipeline HTTP | `test/bmt-be.api.tests/*.csproj` | Chưa dùng `WebApplicationFactory` hay `TestServer`. Test project chưa tham chiếu `Microsoft.AspNetCore.Mvc.Testing` hay `Microsoft.AspNetCore.TestHost`. |

**Lớp che chắn tình cờ.** Theo cách minimal API của .NET 8 bind body `[FromBody]`, request có `Content-Type` không phải JSON bị trả 415 và không vào handler. Trang lạ muốn gửi JSON thì phải preflight, và CORS chặn ở bước đó. Nhờ vậy các endpoint nhận body JSON khó bị giả mạo. Endpoint `PUT`, `PATCH`, `DELETE` cũng luôn phải preflight. Hành vi 415 này lấy theo mã nguồn ASP.NET Core 8 và **chưa chạy thử trên BMT**. Lớp này không đủ để dựa vào: chỉ cần thêm một endpoint POST không body hoặc đọc form là lỗ hổng xuất hiện, như 5 endpoint dưới đây.

### Tài liệu đang nói về CSRF, Origin, antiforgery, SameSite

| Tài liệu | Vị trí | Nội dung | Cơ chế | Mã lỗi |
|---|---|---|---|---|
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | dòng 135, 147, 307, 330, 345, 359, 375 | Kiểm antiforgery token và Origin, endpoint `GET /api/v1/antiforgery/token`, header `X-CSRF-Token`. Ghi rõ là việc nền tảng chưa làm. Câu "Cookie hiện có SameSite=None khi HTTPS" đúng với code, nhưng môi trường thật có thể đang là Lax, xem bảng trên. | Antiforgery + Origin | `CsrfInvalid` (403) |
| [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | dòng 256, 267, 276, 305 | Theo TDD-PROJ-001. | Antiforgery | `CsrfInvalid` (403) |
| [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | dòng 281, 309, 326, 356 | CSRF khi dùng cookie; route công khai không dùng cookie. | Request token + Origin | `CsrfInvalid` (403) |
| [TDD-NEWS-001](../tdd/TDD-NEWS-001.md) | dòng 87, 111, 290, 351 | Mutation dùng cookie kiểm antiforgery và Origin. | Antiforgery + Origin | `CsrfRejected` (403) |
| [TDD-NEWS-002](../tdd/TDD-NEWS-002.md) | dòng 279, 324 | Như TDD-NEWS-001. | Token + Origin | `CsrfRejected` (403) |
| [TDD-LIB-001](../tdd/TDD-LIB-001.md) | dòng 84, 104, 144, 323 | Antiforgery và Origin "theo nền tảng PROJ/RBAC". | Antiforgery + Origin | chưa ghi |
| [TDD-LIB-002](../tdd/TDD-LIB-002.md) | dòng 104, 292 | "POST dùng cookie cần CSRF/Origin như nền tảng chung". | chưa ghi | chưa ghi |
| [TDD-SUB-002](../tdd/TDD-SUB-002.md) | dòng 553 | CSRF theo TDD-PROJ-002. | theo PROJ | theo PROJ |
| [TDD-SUB-005](../tdd/TDD-SUB-005.md) | dòng 341 | API hủy gói phải chống CSRF/Origin cho cookie. | chưa ghi | chưa ghi |
| [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md) | dòng 139, 163, 506, 624 | Header bắt buộc `X-BMT-Request: 1`, Origin khớp chính xác khi dùng cookie, thiếu Origin thì từ chối; client Bearer vẫn phải gửi header. | Header tùy chỉnh + Origin | `ConsultationOriginRejected` (403) |
| [UT-CONSULT-017](../unittest/UT-CONSULT-017.md), [UT-CONSULT-018](../unittest/UT-CONSULT-018.md) | bảng test | Đặc tả unit test cho filter của TDD-CONSULT-001. | Header + Origin | — |
| [TDD-PAY-001](../tdd/TDD-PAY-001.md) | dòng 354 | Webhook SePay chỉ xác thực HMAC, không dùng cookie. | — | — |
| [TDD-PUSH-001](../tdd/TDD-PUSH-001.md) | dòng 66, 326, 330 | Mobile dùng Bearer, không dùng cookie. | — | — |
| [TDD-RBAC-001](../tdd/TDD-RBAC-001.md) | dòng 626 | `MissingAccessToken` khi không có token lẫn cookie. | — | — |
| [discovery/estimate-technical-design.md](../discovery/estimate-technical-design.md) | dòng 59, 96 | CSRF cho cookie là việc nền tảng cần thống nhất. | — | — |
| [discovery/estimate-unit-test-coverage.md](../discovery/estimate-unit-test-coverage.md), [discovery/news-unit-test-coverage.md](../discovery/news-unit-test-coverage.md) | dòng 90; dòng 57 | CSRF kiểm ở tầng tích hợp, chưa có test. | — | — |

Không có User Story, Business Rule hay System Test nào nêu CSRF. Đây là yêu cầu kỹ thuật, không phải quy tắc nghiệp vụ.

**Mâu thuẫn cần gỡ:** có ba cơ chế (antiforgery token, header `X-BMT-Request`, chỉ Origin) và ba mã lỗi (`CsrfInvalid`, `CsrfRejected`, `ConsultationOriginRejected`) cho cùng một việc nền tảng.

## Endpoint ghi hiện có

Tổng cộng 44 endpoint ghi. Cột "Có thể gửi giả hôm nay" đánh giá theo code, chưa thử trên môi trường thật.

### Nhóm 1: dùng cookie phiên (36 endpoint)

| Method | Route (`/api/v1/...`) | Quyền | Body | Có thể gửi giả hôm nay |
|---|---|---|---|---|
| PUT | `users/me` | mặc định | JSON | Khó (JSON) |
| POST | `users/change_password` | `ChangePassword` | JSON | Khó (JSON) |
| POST | `users/logout` | `AuthenticatedOnly` | không | **Có**: buộc người dùng đăng xuất |
| POST | `staff` | `user.manage` + `role.manage` | JSON | Khó |
| POST | `staff/{userId}/roles` | `role.manage` | JSON | Khó |
| DELETE | `staff/{userId}/roles/{roleId}` | `role.manage` | không | Không (DELETE phải preflight) |
| POST | `staff/{userId}/lock` | `user.manage` | không | **Có**: khóa tài khoản nhân viên |
| POST | `staff/{userId}/unlock` | `user.manage` | không | **Có**: mở khóa tài khoản đã bị khóa |
| POST | `staff/{userId}/force-logout` | `user.manage` | không | **Có**: đá phiên nhân viên |
| POST, PUT, DELETE | `roles`, `roles/{roleId}` | `role.manage` | JSON / không | Khó / Không |
| POST, POST, DELETE | `assignments`, `assignments/{id}/transfer`, `assignments/{id}` | `assignment.manage` | JSON / JSON / không | Khó / Khó / Không |
| POST, PUT, DELETE | `me/construction-sites`, `.../{siteId}` | mặc định | JSON / JSON / không | Khó / Khó / Không |
| POST, PUT, PATCH | `estimates`, `estimates/{id}/input`, `estimates/{id}/name` | mặc định | JSON (+ `Idempotency-Key` ở POST, PUT) | Khó |
| POST, PUT, POST, PUT | `admin/estimate-catalog/building-types[/{id}]`, `.../styles[/{id}]` | `estimate.catalog.manage` | JSON + `Idempotency-Key` | Khó |
| POST, POST | `payment-orders`, `payment-orders/{orderId}/cancel` | mặc định | JSON + `Idempotency-Key` | Khó |
| POST, PUT, POST, POST | `admin/plans`, `admin/plans/{id}/draft`, `.../publish`, `.../stop-selling` | `plan.manage` | JSON | Khó |
| POST | `admin/packages/{kind}/{packageId}/cancel` | `package.cancel` | JSON + `Idempotency-Key` | Khó |
| POST | `me/supervision-grants/{grantId}/assign` | mặc định | JSON + `Idempotency-Key` | Khó |
| POST, POST | `admin/supervision-grants/{grantId}/complete`, `.../reopen` | `supervision.complete` | JSON + `Idempotency-Key` | Khó |
| POST | `admin/supervision-grants/{grantId}/unassign` | `supervision.unassign` | JSON + `Idempotency-Key` | Khó |

Nguồn: các file `src/bmt-be.presentation/apis/*/*Api.cs`, tìm theo `MapPost`, `MapPut`, `MapPatch`, `MapDelete`.

### Nhóm 2: không cần đăng nhập (7 endpoint)

| Method | Route (`/api/v1/users/...`) | Body | Cookie | Có thể gửi giả hôm nay |
|---|---|---|---|---|
| POST | `login` | JSON | đặt cookie | Khó (JSON). Nếu gửi giả được thì thành "login CSRF": nạn nhân bị đăng nhập vào tài khoản của kẻ tấn công. |
| POST | `refresh_token` | tùy chọn | **đọc** và đặt cookie | **Có**: xoay vòng token. Nếu trình duyệt không nhận cookie mới thì người dùng mất phiên. |
| POST | `register` | JSON | — | Khó |
| POST | `verify_account` | JSON | đặt cookie | Khó |
| POST | `resend_verify_account_code?email=` | không (query) | — | Có. Không phải CSRF theo nghĩa hẹp vì không dùng cookie, nhưng trang lạ có thể lợi dụng trình duyệt người khác để gửi thư hàng loạt. |
| POST | `forgot_password` | JSON | — | Khó |
| POST | `verify_change_password_code` | JSON | đặt cookie | Khó |

### Nhóm 3: webhook (1 endpoint)

| Method | Route | Xác thực | Ghi chú |
|---|---|---|---|
| POST | `/api/v1/payment-webhooks/sepay/{connectionId}` | HMAC (`X-SePay-Timestamp`, `X-SePay-Signature`) | Máy chủ SePay gọi, không có cookie hay Origin. Phải được miễn kiểm CSRF. |

**Mức rủi ro hiện tại.** Nếu môi trường thật đang đặt cookie `SameSite=Lax` như suy luận ở trên, trang **khác site** (khác tên miền gốc) không gửi kèm được cookie trong request POST. Khi đó chỉ trang **cùng site nhưng khác origin**, ví dụ một subdomain khác của cùng tên miền, mới khai thác được 5 endpoint in đậm. Khi bật forwarded headers hoặc chạy HTTPS trực tiếp, cookie chuyển sang `SameSite=None` và mọi trang trên Internet đều khai thác được. Vì vậy phải có lớp chống CSRF **trước hoặc cùng lúc** với việc sửa cờ cookie.

## So sánh phương án

| | A. Kiểm Origin/Referer theo danh sách | B. Double-submit token có ký | C. Antiforgery của ASP.NET cấu hình lại | D. Header tùy chỉnh bắt buộc (`X-BMT-Request`) |
|---|---|---|---|---|
| Cách làm | Request ghi phải có `Origin` (hoặc `Referer`) khớp chính xác danh sách được phép. | Server cấp token ký HMAC; client gửi token qua header, server so với cookie. | `AddAntiforgery`, endpoint cấp token, filter gọi `IAntiforgery.ValidateRequestAsync` cho endpoint JSON. | Mọi request ghi phải có header cố định, dựa vào preflight và CORS để chặn origin lạ. |
| Chặn được | Mọi request ghi từ origin lạ, kể cả subdomain cùng site và request không body. Chặn cả login CSRF. | CSRF kể cả khi trình duyệt không gửi Origin. | Như B. | Mọi request đơn giản từ origin lạ. |
| Không chặn được | XSS trên origin được phép; origin trong danh sách bị chiếm quyền (ví dụ subdomain takeover); request thiếu cả Origin lẫn Referer nếu chọn cho qua. | XSS. Nếu không ký token thì subdomain cùng site có thể ghi đè cookie. | XSS. | XSS; CORS cấu hình sai là mất bảo vệ, vì chỉ dựa vào CORS. |
| Frontend | Không đổi, nếu gọi từ trình duyệt. Phải sửa nếu có server trung gian (SSR) chuyển tiếp cookie mà không gửi Origin. | Lấy token, gửi header ở mọi request ghi, lấy lại sau login/refresh/logout. Nếu frontend khác tên miền với API thì JS không đọc được cookie của API, nên phải thêm endpoint trả token trong body. | Như B. Thêm: cookie antiforgery mặc định `SameSite=Strict`, frontend khác site sẽ không gửi được nếu không chỉnh. | Thêm một header cố định vào interceptor. |
| Swagger | Thêm origin của API vào danh sách. | Phải nhập header thủ công hoặc viết `requestInterceptor`. | Như B. | Phải cấu hình `requestInterceptor`. |
| Client Bearer / mobile | Bỏ qua khi có `Authorization`. | Bỏ qua. | Phải tự viết điều kiện bỏ qua. | TDD-CONSULT-001 bắt cả Bearer gửi header, nên mobile cũng phải sửa. |
| Login / refresh / logout | Áp chung, không cần bước lấy token trước khi đăng nhập. | Login chưa có phiên để gắn token: phải cấp token ẩn danh trước, hoặc miễn login (khi đó không chặn login CSRF). | Token gắn danh tính người dùng. JWT BMT không có `sub`/`NameIdentifier`/`upn`, nên theo mã nguồn ASP.NET Core 8 (`DefaultClaimUidExtractor`), antiforgery lấy **toàn bộ claim** làm danh tính. Claim `ClaimTypes.Expired` và danh sách quyền đổi sau mỗi lần refresh (15 phút) hoặc đổi vai trò, nên token hỏng theo. Muốn dùng phải thêm claim `NameIdentifier` = UserId vào JWT, và frontend vẫn phải lấy token mới ngay sau login. Chưa chạy thử trên BMT. | Áp chung. |
| Hạ tầng | Không cần gì thêm. | Một khóa bí mật chung giữa các instance. | Keyring Data Protection chung giữa các instance (đang có volume cho một instance). | Không cần gì thêm. |
| Chi phí ước tính | Thấp: 1 middleware, 1 options, 1 metadata miễn trừ, test. Khoảng 1–2 ngày backend. | Trung bình: backend + frontend, khoảng 3–5 ngày. | Cao nhất: như B, cộng đổi claim JWT và tự viết filter cho endpoint JSON. | Thấp, nhưng phải sửa frontend, Swagger và mobile. |

## Phương án đề xuất: A, một middleware dùng chung

**Lý do chọn A:**

- Chặn được đúng các đường tấn công đang có, kể cả 5 endpoint không body, mà không bắt frontend trên trình duyệt sửa gì.
- Hệ thống chỉ có API trả JSON, không có trang HTML server tự render. Trình duyệt hiện đại luôn gửi `Origin` với request ghi khác origin, và cả với POST cùng origin. Kiểm Origin vì vậy là đủ tin cậy cho mô hình này.
- Tránh được vướng mắc của phương án C với JWT không có claim định danh ổn định.
- Không phụ thuộc SameSite. Nếu sau này phải chuyển cookie sang `SameSite=None` vì frontend khác site, lớp này vẫn giữ nguyên.
- Lớp JSON-only và `Idempotency-Key` hiện có vẫn còn tác dụng, trở thành lớp phòng thủ thứ hai.

Phương án D chỉ nên thêm khi xuất hiện client dùng cookie mà không gửi được Origin. Khi đó D thay cho việc bắt Origin với riêng client đó.

### Quy tắc quyết định

Middleware xét theo thứ tự dưới đây. Gặp bước nào có kết luận thì dừng ở bước đó.

1. Method là `GET`, `HEAD`, `OPTIONS` hoặc `TRACE` → cho qua. Hệ quả: endpoint `GET` không được thay đổi dữ liệu. Code hiện tại đã đúng quy ước này.
2. Endpoint có metadata miễn trừ (`SkipCsrfCheck`) → cho qua.
3. Request có header `Authorization` không rỗng → cho qua. Trang lạ không đặt được header này nếu không qua preflight, và JWT luôn dùng header thay cho cookie.
4. Lấy origin của request: dùng `Origin` nếu có. Nếu không có `Origin` thì lấy phần origin của `Referer`.
5. Có origin:
   - Khớp danh sách được phép → cho qua.
   - Không khớp, là chuỗi `null` hoặc sai định dạng → **chặn** (`UntrustedOrigin`). Quy tắc này áp cả khi không có cookie, để chặn login CSRF và việc lợi dụng trình duyệt gọi `resend_verify_account_code`.
6. Không có origin:
   - Request mang cookie `accessToken` hoặc `refreshToken` → **chặn** (`MissingOrigin`).
   - Không mang cookie → cho qua. Đây là client không phải trình duyệt và không có phiên để lạm dụng.

**So khớp origin:** so chính xác cả scheme, host và port. Chuẩn hóa trước khi so: chữ thường cho scheme và host, bỏ port mặc định (`:443` với https, `:80` với http), bỏ dấu `/` cuối. Không so theo tiền tố hay hậu tố: `https://app.bmt.vn.evil.test` không khớp `https://app.bmt.vn`. Không nhận wildcard.

### Vị trí trong pipeline

Đặt ngay sau `app.UseRouting()` và trước `app.UseAuthentication()` trong `Program.cs`:

```
ExceptionHandling → Swagger → UseCors → Localization → UseRouting
  → UseCsrfOriginProtection   ← thêm ở đây
  → UseAuthentication → UseAuthorization → UseRateLimiter → MapCarter
```

- Sau `UseRouting` để đọc được metadata miễn trừ của endpoint.
- Trước `UseAuthentication` để request giả bị chặn trước khi backend kiểm JWT và dấu phiên. Việc này tránh tra dấu phiên cho request giả, và tránh nhánh `OnChallenge` xóa cookie của người dùng (`JwtExtensions.cs:111-114`).
- Sau `UseCors` để preflight `OPTIONS` vẫn được CORS xử lý như cũ. Middleware này không xét `OPTIONS`.

Vị trí code dự kiến:

- Middleware `CsrfOriginProtectionMiddleware` trong `src/bmt-be.api/middlewares/`, cạnh `ExceptionHandlingMiddleware`.
- Phần quyết định tách thành một lớp thuần, không phụ thuộc `HttpContext`, để unit test được.
- Metadata `SkipCsrfCheckMetadata` và extension `.SkipCsrfCheck()` trong `src/bmt-be.presentation/abstractions/`, cùng kiểu với `IdempotencyKey.cs`.

### Cấu hình

- Dùng lại `Cors:AllowedOrigins` làm danh sách origin được phép, để CORS và CSRF không lệch nhau. Tách phần đọc và chuẩn hóa danh sách thành một options dùng chung cho cả `ConfigureCors` và middleware.
- Kiểm cấu hình lúc khởi động. Gặp mục sai định dạng, có đường dẫn hoặc có `*` thì từ chối khởi động, thay vì bỏ qua âm thầm như dòng 25 hiện tại.
- Môi trường có Swagger (Development, Staging) phải thêm origin công khai của chính API vào danh sách.
- `http://localhost:3000` chỉ nên được cộng sẵn ở Development. Cần người dùng xác nhận, xem câu hỏi 4.
- Tùy chọn `Csrf:Mode` với hai giá trị `Enforce` (mặc định) và `ReportOnly` (chỉ ghi log, không chặn), dùng khi đưa lên môi trường thử. Cần người dùng xác nhận, xem câu hỏi 6.

### Danh sách miễn trừ

| Endpoint | Lý do |
|---|---|
| `POST /api/v1/payment-webhooks/sepay/{connectionId}` | Máy chủ SePay gọi, xác thực bằng HMAC, không dùng cookie. |

Chỉ miễn trừ bằng metadata gắn trên từng endpoint, không miễn theo tiền tố đường dẫn. Thêm một test liệt kê mọi endpoint có metadata miễn trừ và so với danh sách trên, để không ai thêm miễn trừ mà không cập nhật tài liệu. Kể cả khi quên gắn metadata, webhook vẫn đi qua được theo bước 6 (không cookie, không Origin). Metadata giữ cho ý định được ghi rõ trong code.

### Phản hồi khi bị chặn

- HTTP 403, không xóa cookie, không gọi handler.
- Body theo dạng lỗi hiện có của `JwtExtensions` và `ExceptionHandlingMiddleware`:

```json
{"title":"Forbidden","code":"CsrfInvalid","status":403,"detail":"Yêu cầu bị từ chối vì không đến từ trang được phép.","messageCode":"CsrfInvalid","errors":null}
```

- Mã `CsrfInvalid` được chọn vì ba TDD của mảng dự toán đã dùng (TDD-PROJ-001/002/003). Frontend chỉ cần biết mã, không cần biết lý do chi tiết.
- Ghi log mức Warning: method, route pattern, lý do (`UntrustedOrigin` hoặc `MissingOrigin`), giá trị `Origin` (cắt tối đa 200 ký tự), request có cookie phiên hay không. **Không** ghi giá trị cookie hay token. Không gửi cảnh báo Discord, vì hiện chỉ lỗi 500 được gửi.
- Middleware tự ghi phản hồi, không ném exception. Làm vậy để `ExceptionHandlingMiddleware` không ghi log mức Error cho mỗi request bị chặn.

### Ảnh hưởng tới các loại client

| Client | Ảnh hưởng |
|---|---|
| Frontend web gọi thẳng từ trình duyệt | Không đổi, miễn origin nằm trong `Cors:AllowedOrigins`. Cần xử lý thêm mã `CsrfInvalid` (403) để hiện thông báo chung. |
| Frontend có tầng server (SSR, route handler) chuyển tiếp cookie tới API | Bị chặn vì không có Origin. Phải gửi `Authorization: Bearer` hoặc đặt `Origin` đúng. Cần xác nhận, xem câu hỏi 2. |
| Swagger UI | Chạy được, sau khi thêm origin của API vào danh sách. Dùng nút Authorize (Bearer) thì không bị kiểm. |
| Postman, curl có cookie | Bị chặn nếu không đặt header `Origin`. Hướng dẫn: dùng Bearer hoặc tự thêm `Origin`. |
| Mobile, script dùng Bearer | Không ảnh hưởng. |
| `login`, `register`, `forgot_password`, `verify_*` | Từ trình duyệt: kiểm Origin như mọi request. Từ client không phải trình duyệt, không cookie, không Origin: cho qua. |
| `refresh_token` | Nhánh dùng cookie: bị kiểm. Nhánh gửi token trong body, không có cookie: cho qua khi không có Origin, bị kiểm khi có Origin. |
| `logout` | Bị kiểm, vì đọc refresh token từ cookie. |
| Webhook SePay | Miễn trừ. |

### Việc liên quan nhưng tách riêng

Các việc dưới đây không bắt buộc cho middleware, nhưng quyết định chung sẽ đỡ phải sửa lại:

- **Forwarded headers:** thêm `UseForwardedHeaders` và tin header của Cloudflare Tunnel, để `IsHttps` phản ánh đúng HTTPS phía trình duyệt. Việc này làm cookie chuyển sang `Secure=true`, `SameSite=None`, nên **phải triển khai cùng hoặc sau** middleware chống CSRF.
- **Cờ cookie theo cấu hình:** đặt SameSite theo cấu hình thay vì suy từ `IsHttps`, và luôn bật `Secure` ngoài Development. Xem câu hỏi 3.

## Kiểm thử

### Unit test (`test/bmt-be.api.tests`)

Kiểm lớp quyết định thuần và phần đọc cấu hình:

| Ca | Kết quả mong đợi |
|---|---|
| `GET` có cookie, Origin lạ | Cho qua |
| `POST` có cookie, Origin đúng | Cho qua |
| `POST` có cookie, Origin lạ | Chặn `UntrustedOrigin` |
| Origin thêm hậu tố (`https://app.example.test.evil.test`), khác scheme (http/https), khác port | Chặn |
| Origin viết hoa, hoặc có port mặc định `:443` | Cho qua sau chuẩn hóa |
| `Origin: null` | Chặn |
| Có cookie, không Origin, Referer đúng | Cho qua |
| Có cookie, không Origin, Referer lạ | Chặn |
| Có cookie, không Origin, không Referer | Chặn `MissingOrigin` |
| Chỉ có cookie `refreshToken`, không Origin | Chặn (trường hợp của `refresh_token`, `logout`) |
| Có header `Authorization` và cookie, Origin lạ | Cho qua |
| Không cookie, không Origin | Cho qua |
| Không cookie, Origin lạ | Chặn |
| `PUT`, `PATCH`, `DELETE` | Giống `POST` |
| Endpoint có `SkipCsrfCheck`, Origin lạ | Cho qua |
| Cấu hình có `*`, có đường dẫn hoặc sai định dạng | Không khởi động được |
| Cấu hình có dấu `/` cuối | Được chuẩn hóa, so khớp đúng |

### Test qua pipeline thật

Cần thêm package `Microsoft.AspNetCore.Mvc.Testing` (hoặc `Microsoft.AspNetCore.TestHost`) vào test project. Có hai mức:

1. **Host tối giản:** dựng `WebApplication` bằng đúng extension method `Program.cs` dùng cho CORS, CSRF và JWT, kèm vài endpoint giả. Không cần database. Mức này chứng minh thứ tự middleware và định dạng phản hồi.
2. **`WebApplicationFactory<Program>`** chạy với PostgreSQL của Testcontainers như bộ integration hiện có. `Program.cs` đối chiếu bảng `Permission` lúc khởi động, và còn phụ thuộc Redis và RabbitMQ. **Cần kiểm khi triển khai** xem có thay được các phụ thuộc này trong test không. Nếu quá nặng thì chỉ làm mức 1, cộng một test liệt kê endpoint từ `EndpointDataSource` của ứng dụng thật.

| Ca | Kết quả mong đợi |
|---|---|
| `POST /api/v1/staff/{id}/lock` có cookie, Origin lạ | 403 `CsrfInvalid`; handler không chạy; trạng thái nhân viên không đổi |
| Cùng request với Origin đúng | Đi tiếp tới bước xác thực (401 với token giả, hoặc 200 với token thật) |
| `POST /api/v1/users/logout` có cookie, không Origin | 403; phản hồi không có `Set-Cookie` |
| `POST /api/v1/users/refresh_token` có cookie, Origin lạ | 403; refresh token không bị xoay vòng |
| `POST /api/v1/payment-webhooks/sepay/{id}` không Origin, và có Origin lạ | Không trả `CsrfInvalid`; đi tới bước kiểm HMAC |
| Preflight `OPTIONS` từ origin được phép | 204 có header CORS, không bị middleware chặn |
| Header `Authorization` với Origin lạ | Qua middleware |
| Request bị chặn | Không có header `IS-TOKEN-EXPIRED`, bộ kiểm dấu phiên không được gọi. Chứng minh middleware đứng trước xác thực. |
| Liệt kê endpoint | Chỉ webhook SePay có metadata `SkipCsrfCheck` |

### Kiểm thử trên môi trường thử (không bắt buộc)

Dùng Playwright mở một trang ở origin khác, tự gửi form POST tới `staff/{id}/lock` khi đang đăng nhập bằng tài khoản quản trị thử. Kỳ vọng 403 và tài khoản không bị khóa. Chỉ chạy trên môi trường thử với dữ liệu thử.

## Cần người dùng quyết định

### Đã xác nhận

Tất cả điểm trong mục này đã được người dùng chốt ngày 26/09/2026, xem đầu tài liệu và TDD-AUTH-001. Nội dung bên dưới giữ nguyên như lúc hỏi.

### Đề xuất chưa chốt

- Dùng phương án A cho toàn hệ thống, không dùng antiforgery token (phương án C).
- Mã lỗi chung `CsrfInvalid` (403).
- Dùng chung `Cors:AllowedOrigins` cho CORS và CSRF.
- Chỉ miễn trừ webhook SePay.

### Cần làm rõ

**1. Frontend chạy ở đâu so với API?**

- (a) Cùng origin, ví dụ API nằm dưới `/api` của cùng tên miền.
- (b) Khác origin nhưng cùng site, ví dụ `app.<tên miền>` và `api.<tên miền>`.
- (c) Khác site, ví dụ frontend trên một nền tảng hosting riêng.

Câu trả lời quyết định giá trị SameSite (câu 3) và các origin phải có trong danh sách. Với (c), cookie bắt buộc `SameSite=None`, nên middleware phải có trước khi sửa cờ cookie. Đề xuất: (b), nếu hạ tầng cho phép.

**2. Có client nào gửi request ghi bằng cookie mà không kèm Origin không?** Ví dụ: tầng server của frontend chuyển tiếp cookie, app mobile dùng cookie, script nội bộ.

- (a) Không có. Chặn mọi request có cookie mà thiếu Origin. **Đề xuất.**
- (b) Có, và client đó chuyển sang dùng Bearer.
- (c) Có, và thêm phương án D (header tùy chỉnh) riêng cho request không Origin. Làm vậy phức tạp hơn và dựa hoàn toàn vào CORS.

**3. Có đổi cách đặt SameSite và Secure không?**

- (a) Giữ như cũ (suy từ `IsHttps`). Môi trường sau tunnel tiếp tục là `Lax` và không `Secure`.
- (b) Đặt theo cấu hình: `Secure` luôn bật ngoài Development; SameSite là `Lax` nếu câu 1 chọn (a) hoặc (b), là `None` nếu chọn (c). Làm cùng việc bật forwarded headers, triển khai sau hoặc cùng middleware. **Đề xuất.**

**4. Danh sách origin**

- Bỏ việc luôn cho phép `http://localhost:3000` ngoài Development? Đề xuất: bỏ. Môi trường nào cần thì khai báo rõ trong `CORS_ALLOWED_ORIGINS`.
- Cho biết origin thật của frontend và của API (cho Swagger) ở dev, staging và production. Hiện giá trị nằm trong secret `CORS_ALLOWED_ORIGINS` của GitHub, tài liệu này không đọc được.

**5. Thống nhất tài liệu**

- Bỏ `X-CSRF-Token` và `GET /api/v1/antiforgery/token` khỏi TDD-PROJ-001/002/003? Đề xuất: bỏ.
- Bỏ `X-BMT-Request` khỏi TDD-CONSULT-001? Đề xuất: bỏ, trừ khi câu 2 chọn (c).
- Đổi `CsrfRejected` (TDD-NEWS) và `ConsultationOriginRejected` (TDD-CONSULT) thành `CsrfInvalid`? Đề xuất: đổi.
- Sau khi chốt, thiết kế chính thức đặt ở đâu? Đề xuất: một mục "Chống CSRF" trong TDD nền tảng về phiên đăng nhập, theo `templates/template-TDD.md`. Các TDD tính năng chỉ dẫn link tới mục đó. Ghi chú này khi ấy chỉ còn giá trị lịch sử.

**6. Cách đưa lên môi trường**

- (a) Bật chặn ngay ở mọi môi trường.
- (b) Chạy `ReportOnly` 1–3 ngày trên staging để tìm client thiếu Origin, sau đó bật chặn; production bật chặn ngay. **Đề xuất.**

## Tài liệu sẽ phải sửa sau khi chốt

Chưa sửa trong đợt này:

- [TDD-PROJ-001](../tdd/TDD-PROJ-001.md), [TDD-PROJ-002](../tdd/TDD-PROJ-002.md), [TDD-PROJ-003](../tdd/TDD-PROJ-003.md): endpoint cấp token, header `X-CSRF-Token` trong ví dụ, câu về SameSite, mục "Phần chưa triển khai".
- [TDD-NEWS-001](../tdd/TDD-NEWS-001.md), [TDD-NEWS-002](../tdd/TDD-NEWS-002.md): cơ chế và mã `CsrfRejected`.
- [TDD-LIB-001](../tdd/TDD-LIB-001.md), [TDD-LIB-002](../tdd/TDD-LIB-002.md), [TDD-SUB-002](../tdd/TDD-SUB-002.md), [TDD-SUB-005](../tdd/TDD-SUB-005.md): dẫn link tới thiết kế chung.
- [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md), [UT-CONSULT-017](../unittest/UT-CONSULT-017.md), [UT-CONSULT-018](../unittest/UT-CONSULT-018.md): header `X-BMT-Request` và mã `ConsultationOriginRejected`.
- [discovery/estimate-technical-design.md](../discovery/estimate-technical-design.md), [discovery/estimate-unit-test-coverage.md](../discovery/estimate-unit-test-coverage.md), [discovery/news-unit-test-coverage.md](../discovery/news-unit-test-coverage.md): cập nhật trạng thái khi đã có code.

## Tham khảo

- OWASP, [Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html): kiểm Origin/Referer, header tùy chỉnh, double-submit có ký.
- Microsoft, [Prevent CSRF attacks in ASP.NET Core 8](https://learn.microsoft.com/en-us/aspnet/core/security/anti-request-forgery?view=aspnetcore-8.0): antiforgery với minimal API chỉ tự kiểm endpoint nhận form.
- Microsoft, [Configure ASP.NET Core to work with proxy servers](https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/proxy-load-balancer?view=aspnetcore-8.0): forwarded headers.
- MDN, [CORS: simple requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests): điều kiện trình duyệt gửi request mà không preflight.
