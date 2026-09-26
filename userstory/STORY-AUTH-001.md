<!-- HƯỚNG DẪN CHO AI KHI ĐIỀN HOẶC CHỈNH SỬA MẪU
- Đọc toàn bộ mẫu, các comment hướng dẫn và tài liệu nguồn được cung cấp trước khi viết. Tuân thủ các quy tắc nghiệp vụ, điều kiện, ngoại lệ và giới hạn đã được xác nhận.
- Không bịa, tự chế hoặc suy đoán thành sự thật: yêu cầu, quy tắc, số liệu, API, schema, mã lỗi, nguồn tham khảo, người phụ trách và kết quả kiểm thử phải có căn cứ. Nội dung ví dụ trong mẫu không phải dữ kiện của dự án.
- Khi thiếu thông tin hoặc các nguồn mâu thuẫn, hỏi người dùng để làm rõ; giữ nguyên placeholder ở phần chưa xác định. Không tự chọn đáp án, lấp chỗ trống hay tuyên bố tài liệu đã hoàn tất.
- Chỉ thay nội dung cần điền hoặc được yêu cầu sửa. Không tự mở rộng phạm vi, sửa nghĩa quy tắc, bỏ điều kiện/ngoại lệ, xoá hay ghi đè nội dung hợp lệ đã có.
- Giữ nguyên tên, cấp và thứ tự heading, nhãn in đậm, tiền tố bullet, cấu trúc danh sách, tên/thứ tự/số cột bảng và cú pháp code fence. Không dịch, đổi tên, gộp hoặc thêm mục ngoài cấu trúc mẫu.
- Chỉ lặp khối luồng, AC hoặc API theo đúng khuôn khi có căn cứ; chỉ bỏ phần tuỳ chọn khi hướng dẫn riêng của mẫu cho phép và đã xác định không áp dụng.
- Mã tham chiếu và section phải trỏ tới tài liệu có thật đã được cung cấp hoặc kiểm chứng. Không giữ mã ví dụ như tham chiếu thật và không tự tạo tài liệu liên quan để hợp thức hoá liên kết.
- Giữ các comment hướng dẫn trong file. Xuất Markdown UTF-8, mỗi file một tài liệu; không bọc toàn bộ file trong code fence, không chèn lời dẫn hay giải thích ngoài mẫu.
- Trước khi bàn giao, đối chiếu nội dung với nguồn, kiểm tra cấu trúc, giá trị cho phép, giới hạn độ dài và tính nhất quán của mã/tham chiếu. Báo rõ phần chưa đủ thông tin; không tuyên bố đã chạy test hoặc import khi chưa thực hiện.
-->

<!-- Mỗi file chứa một tài liệu. Thay mã STORY-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Priority: Must / Should / Could / Won't. Status: Todo / In Progress / Blocked / Done.
Heading cấp 4 trong Alternative Flow và Exception Flow chỉ chứa mã ngắn, duy nhất như ALT-01 hoặc EXC-01, tối đa 50 ký tự; không nối mô tả vào mã. Đặt mô tả ở đoạn bên dưới heading, tối đa 500 ký tự, trước danh sách bước.
AC dùng mã duy nhất như AC-001; giữ nhãn Given/When/Then/And và bám sát luồng, điều kiện, ngoại lệ cùng Business Rule đã xác nhận.
Tham chiếu dùng mã tài liệu, có thể thêm /section và : ghi chú. Xoá dòng tham chiếu không dùng.
Creator và Assignee chỉ là tên trong Markdown; phân công tài khoản và phê duyệt thực hiện trên giao diện sau import.
Sơ đồ thuộc TDD. Các trường quản trị, giả định và câu hỏi mở bổ sung trên giao diện.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Story được dùng làm tiêu đề tài liệu: 1–500 ký tự. Assignee: mỗi tên tối đa 200; vai trò được đọc: Frontend / Backend / Fullstack / Mobile / QA / DevOps / Designer / Reviewer / Other.
- ALT/EXC: mã không rỗng, tối đa 50 ký tự, duy nhất trên cả hai nhóm; mô tả bên dưới mã tối đa 500. Main Flow không có mã nhánh. Mỗi AC dùng mã duy nhất, tối đa 1.000 ký tự; mã AC được dùng trong tham chiếu section vẫn phải nằm trong giới hạn section 100 ký tự.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
ĐỐI CHIẾU FORM USER STORY (src/features/user-stories/validations.ts; form có thể chặt hơn import):
- Điền Story, Context, Creator, Trigger; ít nhất một Assignee có tên, một bước Main Flow và một AC có ít nhất một điều kiện. Mỗi nhánh ALT/EXC đã khai báo có mã và ít nhất một bước. Không để rỗng bước, điều kiện AC hoặc mục danh sách đã thêm.
- Sprint dùng số nguyên dương để đồng thời đáp ứng form yêu cầu số dương và parser đọc Int32 (tối đa 2.147.483.647). Không tự đặt Sprint hoặc số liệu khi chưa có nguồn.
-->

# STORY-AUTH-001

## Metadata

- **Story**: Là khách hàng dùng app BMT trên điện thoại, tôi muốn đăng nhập, đăng ký và lấy lại mật khẩu ngay trong app để dùng BMT mà không phải đăng nhập lại mỗi lần mở app.
- **Context**: Web BMT giữ phiên đăng nhập bằng cookie HttpOnly. App mobile (Android, iOS) không dùng cookie nên cần nhận token trực tiếp. Backend đã nhận access token qua header `Authorization: Bearer`, và request có header này không bị lớp chống CSRF kiểm Origin. Tuy vậy, các chức năng cấp phiên hiện chỉ trả token qua cookie: `login`, `verify_account` và `verify_change_password_code` xóa token khỏi nội dung phản hồi, `refresh_token` trả token mới qua cookie, còn `logout` chỉ đọc refresh token từ cookie (`bmt-be.presentation/apis/user/UserApi.cs`). Vì vậy app hiện không lấy được token để gọi API. Phạm vi do người dùng chốt ngày 26/09/2026. Đây là bản nháp tài liệu hóa phạm vi đã chốt, chưa triển khai.
- **Sprint**:
- **Priority**: Must
- **Status**: Todo
- **Creator**: Claude
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa phân công]
  - Mobile: [Chưa phân công]
  - QA: [Chưa phân công]

## Conditions

### Preconditions

- Khách hàng dùng app BMT trên Android hoặc iOS.
- Với luồng đăng nhập: khách hàng đã có tài khoản khách hàng, tạo qua đăng ký công khai theo BR-RBAC-005.
- App gọi API qua HTTPS.

### Trigger

Khách hàng mở app rồi chọn đăng nhập, đăng ký hoặc quên mật khẩu. Ngoài ra, app tự làm mới phiên khi access token hết hạn và gửi yêu cầu đăng xuất khi khách hàng chọn đăng xuất.

## Flow

### Main Flow

1. Khách hàng nhập email và mật khẩu trên app.
2. Hệ thống kiểm tra mật khẩu, trạng thái tài khoản và nhóm tài khoản theo BR-AUTH-001.
3. Với tài khoản khách hàng hợp lệ, hệ thống tạo một phiên mobile và trả access token cùng refresh token trong nội dung phản hồi. Hệ thống không đặt cookie.
4. App lưu refresh token trong kho bảo mật của hệ điều hành và gửi access token qua header `Authorization: Bearer` ở mỗi yêu cầu gọi API.
5. Khi access token hết hạn, app gửi refresh token trong nội dung yêu cầu để lấy cặp token mới. Hạn của phiên được tính lại 30 ngày kể từ lần làm mới đó, theo BR-AUTH-002.
6. Khi khách hàng chọn đăng xuất, app gửi refresh token của phiên. Hệ thống thu hồi đúng phiên đó, rồi app xóa token khỏi máy và quay về màn đăng nhập.

### Alternative Flow

#### ALT-01

Khách hàng chưa có tài khoản và đăng ký ngay trên app.

1. Khách hàng nhập thông tin đăng ký. App dùng chức năng đăng ký chung với web; hệ thống tạo tài khoản khách hàng và gửi mã xác minh qua email.
2. Khách hàng nhập mã trên app. Nếu cần, khách hàng yêu cầu gửi lại mã theo giới hạn gửi lại đang áp dụng cho web.
3. Mã đúng thì hệ thống đánh dấu email đã xác minh và cấp phiên mobile như bước 3 của Main Flow. Khách hàng vào app mà không phải đăng nhập lại.

#### ALT-02

Khách hàng quên mật khẩu và đặt lại trên app.

1. Khách hàng nhập email. App dùng chức năng gửi mã quên mật khẩu chung với web.
2. Khách hàng nhập mã trên app. Mã đúng và tài khoản là tài khoản khách hàng thì hệ thống cấp phiên quên mật khẩu cho app theo BR-AUTH-002. Như trên web, phiên này dùng để đặt mật khẩu mới và bị từ chối ở các chức năng thông thường.
3. Khách hàng đặt mật khẩu mới. Hệ thống cắt mọi phiên của tài khoản, kể cả phiên quên mật khẩu vừa dùng.
4. App quay về màn đăng nhập để khách hàng đăng nhập bằng mật khẩu mới.

#### ALT-03

Khách hàng đang đăng nhập trên app và đổi mật khẩu.

1. Khách hàng nhập mật khẩu hiện tại và mật khẩu mới. App dùng chức năng đổi mật khẩu chung với web.
2. Đổi thành công thì hệ thống cắt mọi phiên của tài khoản: phiên trên app đang dùng, các thiết bị khác và web.
3. App xóa token và quay về màn đăng nhập.

#### ALT-04

Khách hàng dùng BMT trên nhiều thiết bị hoặc dùng cả app và web.

1. Mỗi lần đăng nhập tạo một phiên riêng.
2. Đăng xuất trên một thiết bị chỉ thu hồi phiên của thiết bị đó; app trên thiết bị khác và web vẫn đăng nhập.

#### ALT-05

Khách hàng đăng nhập bằng tài khoản chưa xác minh email.

1. Hệ thống xử lý như web hiện nay: đăng nhập thành công, nhưng các chức năng yêu cầu email đã xác minh vẫn bị từ chối.
2. App đưa khách hàng sang bước nhập mã xác minh; từ đây tiếp tục như ALT-01 bước 2.

### Exception Flow

#### EXC-01

Email hoặc mật khẩu sai, hoặc tài khoản bị khóa.

1. Email không thuộc tài khoản nào, hoặc mật khẩu sai: hệ thống trả 401 `InvalidCredentials` với cùng một thông báo cho cả hai trường hợp và không tạo phiên.
2. Mật khẩu đúng nhưng tài khoản bị khóa: hệ thống trả lỗi `AccountLocked` như chức năng đăng nhập web và không tạo phiên.
3. Phản hồi không cho biết email thuộc tài khoản khách hàng hay tài khoản nhân viên.

#### EXC-02

Tài khoản nhân viên nhập đúng mật khẩu trên app, hoặc nhập đúng mã quên mật khẩu trên app.

1. Hệ thống trả 403 `MobileLoginNotAllowed` với thông báo "Tài khoản nhân viên chỉ dùng BMT trên web", theo BR-AUTH-001.
2. Hệ thống không tạo phiên và không trả token.

#### EXC-03

App làm mới phiên bằng refresh token sai, đã hết hạn, đã dùng rồi, hoặc thuộc phiên đã bị thu hồi.

1. Hệ thống trả 401 `InvalidRefreshToken` và không cấp token mới.
2. App xóa token đang lưu và đưa khách hàng về màn đăng nhập.

#### EXC-04

Mã xác minh email hoặc mã quên mật khẩu không đúng.

1. Hệ thống trả lỗi `InvalidVerificationCode` như web và không cấp phiên.
2. Khách hàng nhập lại mã hoặc yêu cầu gửi mã mới.

## Acceptance Criteria

#### AC-001

- **Given**: Tài khoản khách hàng đã xác minh email, đang hoạt động.
- **When**: Khách hàng đăng nhập trên app bằng email và mật khẩu đúng.
- **Then**: Phản hồi chứa access token, refresh token và thời điểm hết hạn của refresh token.
- **And**: Phản hồi không đặt cookie `accessToken` hoặc `refreshToken`.

#### AC-002

- **Given**: App đã có access token còn hạn từ AC-001.
- **When**: App gửi một yêu cầu ghi dữ liệu kèm header `Authorization: Bearer`, không có cookie và không có header `Origin`.
- **Then**: Yêu cầu không bị chặn 403 `CsrfInvalid`; hệ thống xử lý theo quyền của tài khoản như với web.

#### AC-003

- **Given**: App có refresh token còn hạn của một phiên đăng nhập.
- **When**: App gửi refresh token đó để làm mới phiên.
- **Then**: Hệ thống trả cặp access token và refresh token mới trong nội dung phản hồi; refresh token mới hết hạn sau 30 ngày kể từ lúc làm mới.
- **And**: Gửi lại refresh token cũ lần nữa bị từ chối 401 `InvalidRefreshToken`.

#### AC-004

- **Given**: Refresh token của phiên mobile đã quá 30 ngày kể từ lần cấp hoặc lần làm mới gần nhất.
- **When**: App gửi refresh token đó để làm mới phiên.
- **Then**: Hệ thống trả 401 `InvalidRefreshToken`; khách hàng phải đăng nhập lại.

#### AC-005

- **Given**: Khách hàng đăng nhập trên hai điện thoại và trên web.
- **When**: Khách hàng đăng xuất trên điện thoại thứ nhất.
- **Then**: Refresh token của điện thoại thứ nhất bị từ chối 401 `InvalidRefreshToken` khi làm mới.
- **And**: Điện thoại thứ hai và web vẫn làm mới phiên được.

#### AC-006

- **Given**: Tài khoản nhân viên đang hoạt động.
- **When**: Nhân viên đăng nhập trên app bằng mật khẩu đúng.
- **Then**: Hệ thống trả 403 `MobileLoginNotAllowed` với thông báo "Tài khoản nhân viên chỉ dùng BMT trên web", không trả token và không tạo phiên.
- **And**: Khi nhân viên nhập sai mật khẩu, phản hồi giống hệt phản hồi cho tài khoản khách hàng nhập sai mật khẩu.

#### AC-007

- **Given**: Khách hàng vừa đăng ký trên app và nhận mã xác minh qua email.
- **When**: Khách hàng nhập đúng mã trên app.
- **Then**: Email được đánh dấu đã xác minh; phản hồi chứa access token và refresh token của phiên mobile như AC-001.

#### AC-008

- **Given**: Khách hàng đã nhận mã quên mật khẩu qua email.
- **When**: Khách hàng nhập đúng mã trên app rồi đặt mật khẩu mới bằng phiên được cấp.
- **Then**: Phiên quên mật khẩu đặt được mật khẩu mới mà không cần mật khẩu hiện tại, bị từ chối ở các chức năng thông thường và có thời hạn như phiên quên mật khẩu trên web.
- **And**: Sau khi đặt mật khẩu mới, mọi phiên của tài khoản bị cắt; khách hàng đăng nhập lại bằng mật khẩu mới.

#### AC-009

- **Given**: Tài khoản nhân viên đã nhận mã quên mật khẩu qua email.
- **When**: Nhân viên nhập đúng mã trên app.
- **Then**: Hệ thống trả 403 `MobileLoginNotAllowed`, không cấp phiên quên mật khẩu cho app.
- **And**: Khi nhân viên nhập sai mã, phản hồi là `InvalidVerificationCode` như tài khoản khách hàng.

#### AC-010

- **Given**: Khách hàng đăng nhập trên app và trên web.
- **When**: Khách hàng đổi mật khẩu thành công trên app.
- **Then**: Các yêu cầu tiếp theo bằng access token cũ của app và của web bị từ chối 401; làm mới bằng refresh token cũ của cả hai nơi bị từ chối 401 `InvalidRefreshToken`.

#### AC-011

- **Given**: Trình duyệt đang giữ cookie phiên web hợp lệ của khách hàng.
- **When**: Có yêu cầu làm mới phiên mobile chỉ mang cookie, không có refresh token trong nội dung yêu cầu.
- **Then**: Hệ thống trả 401 `InvalidRefreshToken` và không trả token nào trong nội dung phản hồi.
- **And**: Đăng nhập và làm mới phiên trên web vẫn chỉ trả token qua cookie như hiện nay.

## References

### TDDs

Thiết kế kỹ thuật của phiên mobile nằm ở TDD-AUTH-002 (bản nháp, chờ chốt). TDD-PUSH-001 đề xuất bảng AuthSession cho phiên mobile; TDD-AUTH-002 giữ phiên trên Redis và để phần đó cho lúc làm push.

- TDD-AUTH-002/Architecture: endpoint, loại phiên, thời hạn và cách xoay refresh token.
- TDD-AUTH-001/Architecture: request có header `Authorization` không bị kiểm Origin.
- TDD-PUSH-001/Architecture: đề xuất AuthSession và API mobile trả token riêng, chưa chốt.

### Rules

- BR-AUTH-001/Then
- BR-AUTH-002/Then
- BR-RBAC-005/Then: phân biệt tài khoản khách hàng và tài khoản nhân viên.
- BR-RBAC-009/Notes: hạn access token do cấu hình quyết định, hiện là 15 phút.

### Dependencies

- STORY-PUSH-001: đăng ký nhận push trên app cần phiên mobile do Story này cấp.

## Non-Functional

- App lưu refresh token trong kho bảo mật của hệ điều hành (Keychain trên iOS, Keystore trên Android), không lưu ở bộ nhớ thường của app. Access token chỉ giữ trong bộ nhớ khi app chạy.
- Backend và app không ghi access token, refresh token hoặc mã xác minh vào log.
- Các chức năng cấp phiên cho app không đọc và không đặt cookie phiên.
- Thời hạn refresh token của phiên mobile phải đổi được bằng cấu hình, không sửa code; giá trị ban đầu là 30 ngày.
- Các route tải file qua backend (ví dụ link chia sẻ hồ sơ, mẫu thư viện được bảo vệ) đòi header `Authorization`. App phải tải bằng HTTP client có gắn header; mở link bằng trình duyệt ngoài hoặc WebView thì yêu cầu không mang phiên và bị từ chối.

## Out of Scope

- Khách hàng tự xóa tài khoản trong app: làm thành một tính năng riêng và phải xong trước khi nộp app lên App Store và Google Play (người dùng xác nhận ngày 26/09/2026).
- Tài khoản nhân viên dùng app.
- Đăng nhập bằng Google, Apple hoặc sinh trắc học; màn hình quản lý thiết bị và phiên đăng nhập.
- Đăng ký nhận push, thuộc STORY-PUSH-001.
- Thay đổi luồng đăng nhập bằng cookie của web.
- Sửa các hành vi đang có của đăng nhập web, như cách báo lỗi khi email không tồn tại.
- Triển khai code, tạo hoặc chạy migration trong bước chuẩn bị tài liệu này.
