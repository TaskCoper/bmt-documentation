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


# STORY-CUSTOMER-001

## Metadata

- **Story**: Là nhân viên được cấp quyền quản lý khách hàng, tôi muốn tra cứu danh sách và hồ sơ khách hàng để tìm đúng tài khoản cần hỗ trợ.
- **Context**: Backend chưa có API danh sách hoặc chi tiết khách hàng cho nhân viên. Người dùng chốt phiên bản chỉ đọc ngày 06/10/2026; chưa nối màn hình frontend.
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

- Người gọi có phiên thông thường hợp lệ, đã xác minh email và được cấp quyền riêng `customer.manage`.

### Trigger

Nhân viên gọi API danh sách hoặc chi tiết tài khoản khách hàng.

## Flow

### Main Flow

1. Nhân viên yêu cầu danh sách khách hàng, có thể tìm theo tên, email hoặc số điện thoại.
2. Backend kiểm phiên và quyền, giới hạn bản ghi vào Customer chưa bị xóa, rồi phân trang.
3. Nhân viên chọn ID để đọc hồ sơ một khách hàng; backend kiểm cùng quyền và phạm vi tài khoản.
4. Backend trả ID, họ, tên, email, số điện thoại, avatar, trạng thái, xác minh email và ngày tạo.

### Alternative Flow

#### ALT-01

Không có khách hàng phù hợp tìm kiếm hoặc trang yêu cầu không có bản ghi.

1. Trả danh sách rỗng cùng thông tin phân trang; không đổi sang dữ liệu mẫu.

### Exception Flow

#### EXC-01

Không có phiên hoặc thiếu quyền.

1. Trả 401 nếu phiên không hợp lệ; trả 403 nếu phiên hợp lệ nhưng thiếu quyền.

#### EXC-02

ID chi tiết không tồn tại, đã bị xóa hoặc thuộc Staff.

1. Trả 404 `CustomerNotFound`, không tiết lộ tài khoản nhân viên.

## Acceptance Criteria

#### AC-001

- **Given**: Có Customer hoạt động, Customer bị khóa, Staff và Customer đã xóa.
- **When**: Nhân viên đủ quyền lấy danh sách.
- **Then**: Chỉ trả Customer chưa bị xóa, gồm cả hoạt động và bị khóa, theo trang yêu cầu.
- **And**: Tìm tên/email/SĐT trả đúng bản ghi; không có kết quả thì danh sách rỗng.

#### AC-002

- **Given**: Một Customer chưa bị xóa tồn tại.
- **When**: Nhân viên đủ quyền đọc chi tiết.
- **Then**: Trả đúng các trường đã chốt, giữ null nếu chưa có thông tin.
- **And**: ID không tồn tại, đã xóa hoặc thuộc Staff đều trả 404 `CustomerNotFound`.

#### AC-003

- **Given**: Người gọi chưa đăng nhập hoặc chỉ có quyền quản lý nhân viên.
- **When**: Gọi một trong hai API.
- **Then**: Backend trả 401 hoặc 403 tương ứng trước khi chạy query.
- **And**: Nhân viên chỉ có `customer.manage` đọc được, không cần vai trò Admin.

#### AC-004

- **Given**: Tài khoản có mật khẩu băm, mã xác minh và dấu phiên.
- **When**: Đọc danh sách hoặc chi tiết.
- **Then**: Không trả các trường nhạy cảm; không sửa hồ sơ, phiên hoặc trạng thái.

## References

### TDDs

- TDD-CUSTOMER-001

### Rules

- BR-CUSTOMER-001

### Dependencies

## Non-Functional

- Kiểm quyền ở server trên cả hai API; tối đa 100 bản ghi mỗi trang; truyền cancellation token xuống database.

## Out of Scope

- Tạo/sửa/xóa tài khoản, khóa/mở khóa, buộc đăng xuất, lịch sử quản trị, gói, quota, đơn, giao dịch và nối frontend.
- Người phụ trách chưa được cung cấp; bộ tài liệu giữ bản nháp, tên Reviewer/Approver không thay thế phê duyệt.
