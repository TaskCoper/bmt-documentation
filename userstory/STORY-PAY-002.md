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

# STORY-PAY-002

## Metadata

- **Story**: Là Admin hoặc nhân viên có quyền tra cứu, tôi muốn biết khách nào mua gói nào và xem các giao dịch để đối chiếu thông tin mua gói trong hệ thống.
- **Context**: Bổ sung phần quản trị còn thiếu của thanh toán. Tra cứu gói đã mua, đơn và giao dịch gồm cả giao dịch chưa khớp đơn; không mở rộng quyền xem thành quyền sửa dữ liệu hoặc xử lý tiền. Ba danh sách liên kết là đề xuất trình bày, không bắt buộc ba màn hình riêng.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác đã đăng nhập; hệ thống có thể kiểm tra vai trò Admin hoặc quyền tra cứu riêng của nhân viên.
- Có dữ liệu gói, đơn và giao dịch theo nghiệp vụ thanh toán. Nguồn dữ liệu và quyền riêng chưa được coi là đã triển khai.

### Trigger

Người có quyền mở phần quản trị gói đã mua hoặc giao dịch.

## Flow

### Main Flow

1. Người dùng mở danh sách gói đã mua, đơn hoặc giao dịch.
2. Hệ thống kiểm tra quyền tra cứu trước khi trả dữ liệu.
3. Người dùng xem ai đã mua gói nào và chọn gói cần đối chiếu.
4. Hệ thống cung cấp liên kết đơn mua và các giao dịch thực tế của đơn, giữ đúng thông tin đã ghi nhận.
5. Người dùng xem chi tiết; thao tác xem không thay đổi tiền, liên kết công trình hoặc hiệu lực gói.

### Alternative Flow

#### ALT-01

Giao dịch chưa khớp đơn.

1. Hiển thị giao dịch với nhãn “Chưa xác định đơn” và thông tin giao dịch đã nhận.
2. Không tự suy đoán người mua hoặc gói; chưa hỗ trợ gán thủ công.

#### ALT-02

Đơn có nhiều khoản chuyển, hoặc đơn/gói không còn hiệu lực.

1. Hiển thị từng giao dịch thực tế của đơn, không nhân bản tiền do webhook lặp.
2. Giữ khả năng tra cứu đơn chờ, nhận thiếu, hết hạn, đã hủy và gói lịch sử với trạng thái đúng.

### Exception Flow

#### EXC-01

Người truy cập không có quyền tra cứu.

1. Từ chối danh sách/chi tiết và không trả dữ liệu quản trị, kể cả khi gọi trực tiếp.

#### EXC-02

Nhân viên có quyền xem nhưng không có quyền thực hiện thao tác ghi.

1. Cho xem dữ liệu theo quyền tra cứu.
2. Từ chối đổi công trình, hủy hoặc khôi phục nếu thiếu quyền riêng tương ứng; dữ liệu giữ nguyên.

## Acceptance Criteria

#### AC-001

- **Given**: Admin đã đăng nhập; U1 mua gói thiết kế D1, U2 mua gói giám sát G1 chưa gán.
- **When**: Admin mở danh sách gói đã mua và xem từng gói.
- **Then**: Xác định đúng U1 mua D1 và U2 mua G1, cùng đơn mua tương ứng.
- **And**: G1 chưa gán vẫn xuất hiện; xem không thay đổi hiệu lực gói.

#### AC-002

- **Given**: Nhân viên đã đăng nhập và có quyền tra cứu riêng; hệ thống có đơn, gói và giao dịch của khách.
- **When**: Nhân viên mở danh sách và chi tiết gói, đơn, giao dịch.
- **Then**: Được tra cứu thông tin người mua, gói và giao dịch trong phạm vi quản trị đã chốt.
- **And**: Không yêu cầu nhân viên phải có quyền đổi công trình, hủy hoặc khôi phục chỉ để xem.

#### AC-003

- **Given**: Có nhân viên không được cấp quyền tra cứu, khách hàng thông thường và phiên chưa đăng nhập.
- **When**: Từng người thử truy cập danh sách và chi tiết quản trị, kể cả gửi yêu cầu trực tiếp.
- **Then**: Từ chối truy cập dữ liệu quản trị.
- **And**: Không trả thông tin người mua hoặc giao dịch từ các yêu cầu bị từ chối.

#### AC-004

- **Given**: Hệ thống đã nhận giao dịch hợp lệ T1 nhưng chưa xác định được đơn tương ứng.
- **When**: Admin hoặc nhân viên có quyền mở danh sách và chi tiết giao dịch.
- **Then**: T1 xuất hiện với nhãn “Chưa xác định đơn”; xem được mã giao dịch, thời điểm, số tiền và nội dung chuyển khoản đã nhận.
- **And**: Không tự suy đoán người mua/gói; chưa có thao tác gán giao dịch vào đơn bằng tay.

#### AC-005

- **Given**: Đơn O1 giá 2.000.000 VNĐ có T1 là 500.000 và T2 là 1.500.000; webhook T1 đã được gửi lại.
- **When**: Người có quyền xem O1 và các giao dịch liên quan.
- **Then**: Hiển thị hai giao dịch thực tế T1/T2, liên kết đúng O1; tổng nhận 2.000.000 VNĐ.
- **And**: Không hiển thị webhook gửi lại như một lần chuyển tiền mới hoặc cộng tiền hai lần.

#### AC-006

- **Given**: Có đơn đang chờ, nhận thiếu, hết hạn, đã hủy, đã thanh toán; có gói bị nhân viên hủy và gói thiết kế đã bị thay thế.
- **When**: Người có quyền tra cứu các đơn và gói này.
- **Then**: Vẫn tìm được từng bản ghi với trạng thái thực tế và người mua tương ứng.
- **And**: Không coi đơn hết hạn/đã hủy là không tồn tại hoặc gói lịch sử là đang hiệu lực.

#### AC-007

- **Given**: Nhân viên chỉ có quyền tra cứu, không có quyền đổi công trình, hủy hoặc khôi phục gói.
- **When**: Nhân viên xem gói rồi thử gửi các yêu cầu đổi công trình, hủy và khôi phục.
- **Then**: Xem được nhưng tất cả yêu cầu ghi đều bị từ chối.
- **And**: Gói, công trình và lịch sử thao tác thành công không thay đổi.

#### AC-008

- **Given**: Khách U1 có hai đơn hợp lệ O1/O2 và các giao dịch riêng; mỗi đơn cấp gói tương ứng.
- **When**: Người có quyền mở một gói, xem đơn mua rồi các giao dịch của đơn.
- **Then**: Liên kết đúng gói–người mua–đơn–giao dịch; không trộn tiền của O1 vào O2.
- **And**: Thông tin đọc theo đơn và giao dịch đã ghi nhận, không thay bằng giá danh mục hiện tại.

## References

### TDDs

- [TDD-PAY-002](../tdd/TDD-PAY-002.md): Thiết kế kỹ thuật bản nháp và đặc tả Unit Test liên quan.

### Rules

- BR-PAY-005/Then
- BR-PAY-001/Then
- BR-PAY-002/Then
- BR-PAY-004/Then
- BR-SUB-023/Then
- BR-SUB-024/Then
- BR-SUB-025/Then

### Dependencies

- STORY-PAY-001: Nguồn đơn, giao dịch và gói đã mua.
- STORY-SUB-004: Liên kết công trình và quyền sửa riêng.
- STORY-SUB-005: Hủy và khôi phục gói bằng quyền riêng.
- [Tổng hợp quyết định](../discovery/payment-packages.md).

## Non-Functional

- Kiểm tra quyền tại nơi xử lý yêu cầu cho cả danh sách và chi tiết, không chỉ tại giao diện. AC-003 và AC-007 kiểm tra từ chối truy cập/thao tác trái quyền.
- Chưa chốt chỉ tiêu hiệu năng hoặc kích thước trang; các lựa chọn kỹ thuật sẽ được thiết kế sau.

## Out of Scope

- Gán giao dịch vào đơn thủ công; chỉnh sửa số tiền; hoàn tiền hoặc đánh dấu đã hoàn tiền; xác nhận thủ công để cấp gói.
- Báo cáo doanh thu và xuất file chưa nằm trong phạm vi đã chốt. Quản trị cấp quyền nhân viên theo bộ STORY-RBAC-001 đến STORY-RBAC-004.
- Chưa triển khai, chạy test hoặc phê duyệt. Các trường phân công còn thiếu giữ nguyên trạng thái chưa xác định.
