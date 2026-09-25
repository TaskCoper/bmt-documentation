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

# STORY-CONSULT-003

## Metadata

- **Story**: Là admin, tôi muốn xem và cập nhật yêu cầu tư vấn để chủ động liên hệ khách và theo dõi việc xử lý.
- **Context**: Admin gọi khách ngoài hệ thống để sắp xếp lịch; Đã xử lý không có nghĩa buổi tư vấn đã kết thúc. Nghiệp vụ được xác nhận qua hội thoại ngày 2026-09-23; người dùng đã chốt bộ US/BR trong hội thoại. Metadata chưa đầy đủ. Quyền xem và xử lý yêu cầu là quyền quản lý tư vấn KTS theo STORY-RBAC-001 (người dùng xác nhận ngày 25/09/2026); tên mã đặt ở bước thiết kế kỹ thuật.
- **Sprint**:
- **Priority**: Must
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác có quyền quản lý tư vấn KTS theo STORY-RBAC-001; vai trò Admin có quyền này.

### Trigger

Admin mở danh sách yêu cầu tư vấn.

## Flow

### Main Flow

1. Admin mở danh sách, lọc Chưa xử lý và chọn một yêu cầu.
2. Hệ thống hiển thị thông tin khách gửi: KTS, ngày giờ mong muốn, số liên lạc, nội dung tư vấn nếu có, trạng thái và ghi chú nội bộ.
3. Admin chủ động gọi số liên lạc trên yêu cầu để thống nhất lịch.
4. Khi đã liên hệ và xử lý xong, admin có thể cập nhật ghi chú và đánh dấu Đã xử lý.
5. Hệ thống lưu thay đổi; yêu cầu xuất hiện khi lọc Đã xử lý. Không gửi email cập nhật trạng thái.

### Alternative Flow

#### ALT-01

Chưa liên lạc được với khách.

1. Admin giữ trạng thái Chưa xử lý, có thể ghi chú việc liên hệ.

#### ALT-02

Khách không còn nhu cầu.

1. Admin ghi lý do và đánh dấu Đã xử lý.

#### ALT-03

Đánh dấu nhầm hoặc cần liên hệ lại.

1. Admin chuyển từ Đã xử lý về Chưa xử lý và có thể sửa ghi chú.

#### ALT-04

KTS của yêu cầu đã bị ẩn.

1. Admin vẫn xem và xử lý yêu cầu đã gửi trước đó.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý tư vấn KTS.

1. Hệ thống từ chối đọc danh sách, chi tiết hoặc cập nhật yêu cầu và ghi chú.

## Acceptance Criteria

#### AC-001

- **Given**: Có yêu cầu ở cả hai trạng thái.
- **When**: Admin lọc Chưa xử lý hoặc Đã xử lý.
- **Then**: Danh sách chỉ chứa yêu cầu đúng trạng thái chọn.
- **And**: Admin mở được chi tiết và thấy số liên lạc trên đơn, kể cả số đã thay khác tài khoản.

#### AC-002

- **Given**: Admin đã liên hệ và xử lý xong yêu cầu.
- **When**: Đánh dấu Đã xử lý.
- **Then**: Trạng thái và ghi chú được lưu.
- **And**: Không phải đợi buổi tư vấn kết thúc và không gửi thêm email.

#### AC-003

- **Given**: Admin chưa liên lạc được.
- **When**: Theo dõi yêu cầu.
- **Then**: Yêu cầu vẫn ở Chưa xử lý.
- **And**: Admin được lưu ghi chú nội bộ.

#### AC-004

- **Given**: Khách không còn nhu cầu.
- **When**: Admin ghi lý do và đánh dấu Đã xử lý.
- **Then**: Lưu trạng thái Đã xử lý cùng lý do.
- **And**: Không thêm trạng thái hủy.

#### AC-005

- **Given**: Yêu cầu đã được đánh dấu Đã xử lý.
- **When**: Admin mở lại và sửa ghi chú khi cần.
- **Then**: Yêu cầu trở về Chưa xử lý và lưu ghi chú đã sửa.
- **And**: Có thể tiếp tục theo dõi để liên hệ lại.

#### AC-006

- **Given**: Yêu cầu có ghi chú nội bộ.
- **When**: Khách nhận email hoặc người không có quyền quản lý tư vấn KTS truy cập chức năng quản trị.
- **Then**: Ghi chú không có trong email; truy cập quản trị bị từ chối.
- **And**: Không cung cấp trang khách theo dõi yêu cầu.

## References

### TDDs

- TDD-CONSULT-001

### Rules

- BR-CONSULT-001
- BR-CONSULT-004
- BR-CONSULT-005

### Dependencies

- STORY-CONSULT-002

## Non-Functional

- Chức năng quản trị chỉ dành cho người có quyền quản lý tư vấn KTS theo STORY-RBAC-001; ghi chú nội bộ không được công khai hoặc gửi cho khách. Chưa xác định chỉ tiêu hiệu năng định lượng.

## Out of Scope

- Email thông báo cho admin; email khi cập nhật trạng thái.
- Quản lý buổi tư vấn, cuộc gọi, lịch chính thức hoặc theo dõi kết thúc buổi tư vấn.
