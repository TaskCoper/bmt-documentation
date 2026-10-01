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

# STORY-MAP-001

## Metadata

- **Story**: Là người dùng, tôi muốn tìm địa chỉ rồi chọn một kết quả để lấy kinh độ và vĩ độ.
- **Context**: Người dùng yêu cầu backend tích hợp VietMap cho FE và xác nhận trả danh sách để chọn. Chưa có API key; cần tạo biến cấu hình trống để điền sau. Bản nháp còn thiếu thông tin người review và phê duyệt.
- **Sprint**:
- **Priority**: Must
- **Status**: In Progress
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- FE có phiên đăng nhập đã xác minh theo policy mặc định của backend.
- Backend được cấu hình API key VietMap có quyền dùng Search v4 và Place v4.

### Trigger

Người dùng yêu cầu tra cứu một địa chỉ đầy đủ trên FE.

## Flow

### Main Flow

1. FE gửi địa chỉ đến backend.
2. Backend gọi Search v4 và trả danh sách theo thứ tự VietMap cung cấp, kèm mã `refId` của từng kết quả.
3. Người dùng chọn một kết quả; FE gửi nguyên `refId` đó đến backend.
4. Backend gọi Place v4 và trả địa chỉ hiển thị, vĩ độ `latitude` và kinh độ `longitude`.

### Alternative Flow

#### ALT-01

VietMap không tìm thấy địa chỉ phù hợp.

1. Backend trả danh sách rỗng.
2. FE cho phép người dùng sửa địa chỉ và tìm lại.

### Exception Flow

#### EXC-01

Đầu vào thiếu hoặc quá dài, phiên không hợp lệ, chưa có key hoặc VietMap không trả dữ liệu hợp lệ.

1. Backend trả lỗi theo contract trong TDD-MAP-001/Internal API; không tự đặt tọa độ để coi là thành công.

## Acceptance Criteria

#### AC-001

- **Given**: Địa chỉ có nhiều kết quả phù hợp.
- **When**: FE tra cứu rồi người dùng chọn kết quả thứ hai.
- **Then**: Danh sách giữ nguyên thứ tự; tọa độ trả về thuộc đúng `refId` đã chọn.
- **And**: Backend không tự chọn kết quả đầu tiên, không gọi Place cho toàn bộ danh sách.

#### AC-002

- **Given**: Search trả danh sách rỗng.
- **When**: FE nhận phản hồi.
- **Then**: HTTP 200 với `value: []`, không có tọa độ giả.

#### AC-003

- **Given**: Đầu vào không hợp lệ hoặc dịch vụ không sẵn sàng.
- **When**: FE gọi API.
- **Then**: Nhận lỗi phân biệt được theo contract; API key và nội dung lỗi thô của VietMap không được trả cho FE.

## References

### TDDs

- TDD-MAP-001

### Rules

- BR-MAP-001

### Dependencies

## Non-Functional

- API key chỉ được cấu hình ở backend; dùng cơ chế xác thực, validation và rate limit hiện có.
- FE bỏ kết quả của request cũ khi người dùng đã đổi địa chỉ, bỏ tọa độ đã chọn khi sửa địa chỉ và chỉ lưu khi kết quả còn khớp lựa chọn hiện tại.

## Out of Scope

- Triển khai giao diện FE, hiển thị bản đồ, gợi ý theo từng phím gõ và tìm ngược từ tọa độ.
- Lưu hoặc sửa công trình trong thao tác tra cứu; không thêm bảng hay migration.
