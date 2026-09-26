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

# STORY-PAY-003

## Metadata

- **Story**: Là Admin hoặc nhân viên có quyền quản lý kết nối nhận tiền, tôi muốn khai báo tài khoản nhận tiền SePay và chọn tài khoản đang dùng cho từng môi trường để đổi tài khoản nhận mà không phải sửa database hay biến môi trường chọn tài khoản.
- **Context**: Trước đây mỗi kết nối nhận tiền (PaymentConnection) do vận hành tự chèn vào database, còn kết nối dùng cho đơn mới được chọn bằng biến môi trường `SePayOption__ActiveConnectionId`. Người dùng quyết định ngày 26/09/2026 chuyển việc này sang API quản trị có quyền riêng và đã trả lời đủ các câu hỏi Q1–Q7 cùng ngày. Secret HMAC vẫn nằm ở biến môi trường theo Id kết nối. Máy chủ biết mình thuộc môi trường Test hay Live qua biến `SePayOption__Environment`; biến này chỉ chọn môi trường, không chọn kết nối.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác đã đăng nhập và có quyền `payment.connection.manage`. Admin có quyền này mặc định.
- Tài khoản ngân hàng và webhook trên SePay đã được đăng ký bên ngoài hệ thống.
- Máy chủ đã đặt `SePayOption__Environment` là Test hoặc Live.

### Trigger

Người có quyền mở phần quản lý kết nối nhận tiền.

## Flow

### Main Flow

1. Người dùng xem danh sách kết nối nhận tiền của các môi trường Test và Live.
2. Người dùng tạo kết nối mới: chọn môi trường, ngân hàng, số tài khoản, tài khoản ảo (VA) nếu có và mã ngân hàng dùng cho QR. Không nhập secret.
3. Hệ thống lưu kết nối, ghi lịch sử và trả Id để vận hành đặt secret vào biến môi trường theo Id đó.
4. Người dùng chọn kết nối đang dùng cho một môi trường.
5. Đơn mới tạo sau đó dùng kết nối vừa chọn. Đơn đã tạo trước đó giữ kết nối lúc tạo và vẫn nhận tiền qua kết nối đó.

### Alternative Flow

#### ALT-01

Đổi tài khoản nhận khi kết nối cũ đã có đơn hoặc giao dịch webhook.

1. Người dùng tạo một kết nối mới với tài khoản nhận mới.
2. Người dùng chọn kết nối mới làm kết nối đang dùng của môi trường.
3. Đơn cũ vẫn đối chiếu và nhận webhook theo kết nối cũ; đơn mới dùng kết nối mới.

#### ALT-02

Sửa kết nối chưa có đơn và chưa có giao dịch webhook.

1. Người dùng sửa ngân hàng, số tài khoản, VA, mã ngân hàng QR hoặc bật/tắt kết nối.
2. Hệ thống lưu thay đổi và ghi lịch sử giá trị trước/sau. Môi trường của kết nối không đổi được sau khi tạo.

#### ALT-03

Xem lịch sử thao tác trên một kết nối.

1. Người dùng mở lịch sử của kết nối.
2. Hệ thống hiện từng lần tạo, sửa, chọn làm kết nối đang dùng: ai làm, lúc nào, giá trị trước và sau. Lịch sử không chứa secret.

### Exception Flow

#### EXC-01

Người không có quyền `payment.connection.manage` gọi các thao tác quản lý kết nối, kể cả gửi trực tiếp tới API.

1. Hệ thống từ chối và không trả thông tin kết nối.

#### EXC-02

Người dùng sửa tài khoản nhận của kết nối đã có đơn hoặc giao dịch webhook.

1. Hệ thống từ chối; tài khoản nhận của kết nối giữ nguyên.
2. Muốn đổi tài khoản nhận thì tạo kết nối mới theo ALT-01.

#### EXC-03

Người dùng tắt kết nối đang được chọn cho môi trường.

1. Hệ thống từ chối; kết nối vẫn bật và vẫn được chọn.
2. Muốn tắt thì chọn kết nối khác cho môi trường trước. Kết nối không được chọn thì tắt được kể cả khi còn đơn chờ; webhook của nó vẫn được nhận.

#### EXC-04

Người dùng chọn làm kết nối đang dùng một kết nối đang tắt, thuộc môi trường khác hoặc máy chủ chưa có secret của nó.

1. Hệ thống từ chối; kết nối đang dùng của môi trường giữ nguyên.

#### EXC-05

Môi trường của máy chủ chưa có kết nối đang dùng hợp lệ khi khách tạo đơn.

1. Hệ thống trả lỗi thanh toán chưa sẵn sàng (503 `PaymentUnavailable`) và không tạo đơn.
2. Không có thao tác bỏ chọn kết nối; người quản trị chỉ đổi sang kết nối khác.

#### EXC-06

Máy chủ thiếu hoặc đặt sai `SePayOption__Environment`.

1. Ứng dụng không khởi động và báo cấu hình môi trường sai.

## Acceptance Criteria

#### AC-001

- **Given**: Admin đã đăng nhập; môi trường Test chưa có kết nối nào.
- **When**: Admin tạo kết nối Test cho tài khoản TESTACCOUNT của Vietcombank, không có VA.
- **Then**: Kết nối được lưu và phản hồi có Id kết nối.
- **And**: Phản hồi không chứa secret HMAC; lịch sử có một dòng tạo kết nối ghi người tạo và thời điểm.

#### AC-002

- **Given**: Môi trường Test có kết nối C1 đang dùng; khách đã tạo đơn O1 bằng C1; Admin đã tạo kết nối C2 và vận hành đã đặt secret cho C2.
- **When**: Admin chọn C2 làm kết nối đang dùng của môi trường Test, rồi khách tạo đơn O2.
- **Then**: O2 dùng C2.
- **And**: O1 vẫn giữ C1 và vẫn nhận webhook qua C1.

#### AC-003

- **Given**: Kết nối C1 đã có ít nhất một đơn.
- **When**: Người có quyền gửi yêu cầu đổi số tài khoản hoặc ngân hàng của C1.
- **Then**: Hệ thống từ chối.
- **And**: Tài khoản nhận của C1 giữ nguyên; đơn của C1 không đổi.

#### AC-004

- **Given**: Nhân viên N không có quyền `payment.connection.manage`, khách hàng K và một phiên chưa đăng nhập.
- **When**: Từng người gọi danh sách, tạo, sửa kết nối, chọn kết nối đang dùng hoặc xem lịch sử, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối mọi yêu cầu.
- **And**: Không trả thông tin kết nối và không thay đổi dữ liệu.

#### AC-005

- **Given**: Vận hành đã đặt secret cho kết nối C1 trong biến môi trường.
- **When**: Người có quyền xem danh sách, chi tiết hoặc lịch sử của C1.
- **Then**: Phản hồi không chứa secret.
- **And**: Không có thao tác nào của API nhận secret để lưu.

#### AC-006

- **Given**: Kết nối C3 chưa có đơn và chưa có giao dịch webhook.
- **When**: Người có quyền sửa số tài khoản, VA và tắt C3.
- **Then**: C3 lưu giá trị mới.
- **And**: Lịch sử có một dòng sửa với giá trị trước và sau; môi trường của C3 không đổi.

#### AC-007

- **Given**: Kết nối C4 chưa có đơn nhưng đã nhận một giao dịch webhook.
- **When**: Người có quyền đổi số tài khoản của C4.
- **Then**: Hệ thống từ chối như kết nối đã có đơn.
- **And**: Người có quyền vẫn bật/tắt được C4.

#### AC-008

- **Given**: C2 đang được chọn cho môi trường Test; C1 không còn được chọn nhưng còn đơn O1 đang chờ thanh toán.
- **When**: Người có quyền tắt C2, rồi tắt C1, rồi SePay gửi webhook của O1 tới C1.
- **Then**: Tắt C2 bị từ chối; C2 vẫn bật và vẫn được chọn.
- **And**: Tắt C1 thành công và webhook của O1 tới C1 vẫn được nhận.

#### AC-009

- **Given**: Kết nối C1 đã tạo.
- **When**: Người có quyền tìm thao tác xóa C1, kể cả gửi yêu cầu xóa trực tiếp tới API.
- **Then**: Hệ thống không có thao tác xóa kết nối; yêu cầu xóa không thành công.
- **And**: C1 và lịch sử của nó giữ nguyên; muốn ngừng dùng thì tắt C1.

#### AC-010

- **Given**: Máy chủ chạy môi trường Live; Live chưa có kết nối đang dùng, còn Test đã có.
- **When**: Khách tạo đơn mua gói.
- **Then**: Hệ thống trả 503 `PaymentUnavailable`.
- **And**: Không tạo đơn hay QR.

#### AC-011

- **Given**: Máy chủ không đặt `SePayOption__Environment`, hoặc đặt giá trị khác Test và Live.
- **When**: Khởi động ứng dụng.
- **Then**: Ứng dụng không khởi động.
- **And**: Thông báo lỗi nêu biến `SePayOption__Environment` cần là Test hoặc Live.

#### AC-012

- **Given**: Admin A1 đã tạo C2, chọn C2 cho Test, và nhân viên N2 có quyền đã sửa VA của C2 trước khi C2 có đơn.
- **When**: Người có quyền xem lịch sử của C2.
- **Then**: Có ba dòng tạo, chọn, sửa, mới nhất trước, mỗi dòng ghi người làm và thời điểm.
- **And**: Dòng sửa có VA trước và sau; dòng chọn có kết nối cũ và mới; không dòng nào chứa secret.

#### AC-013

- **Given**: Môi trường Test đang dùng C1; có C5 đang tắt, C6 thuộc Live và C7 máy chủ chưa có secret.
- **When**: Người có quyền lần lượt chọn C5, C6, C7 làm kết nối đang dùng của Test.
- **Then**: Cả ba yêu cầu bị từ chối.
- **And**: Test vẫn dùng C1.

## References

### TDDs

- [TDD-PAY-001](../tdd/TDD-PAY-001.md): Thiết kế kỹ thuật của API quản trị kết nối, mục Quản trị connection.

### Rules

- BR-PAY-006/Then

### Dependencies

- STORY-PAY-001: Đơn thanh toán dùng kết nối đang dùng lúc tạo.
- [TDD-RBAC-001](../tdd/TDD-RBAC-001.md): Quyền theo mã và claim `perm`.

## Non-Functional

- Kiểm tra quyền tại nơi xử lý yêu cầu, không chỉ ẩn nút trên giao diện.
- Secret HMAC không xuất hiện trong phản hồi API, lịch sử, log hay database.

## Out of Scope

- Nhập, lưu hoặc xoay vòng secret qua API; tạo hay đổi cấu hình webhook trên SePay.
- Xóa kết nối; bỏ chọn kết nối đang dùng của một môi trường.
- Chưa phân công người thực hiện; chưa có kết quả chạy System Test.
