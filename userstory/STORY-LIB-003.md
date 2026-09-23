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

# STORY-LIB-003

## Metadata

- **Story**: Là khách hàng, tôi muốn mở mẫu theo lượt và xem lại từng phiên bản đã mở để tham khảo mà không bị tính lượt lặp.
- **Context**: Thay quy tắc mỗi lần mở đều tính lượt bằng quyền xem theo tài khoản và phiên bản.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Mở chi tiết cần đăng nhập; mở lần đầu cần đủ điều kiện BR-LIB-003.

### Trigger

Khách chọn mẫu từ thư viện hoặc một phiên bản trong lịch sử.

## Flow

### Main Flow

1. Xác định đúng phiên bản khách yêu cầu và quyền xem của tài khoản.
2. Nếu chưa xem, kiểm tra gói, quyền và lượt; hiển thị xác nhận dùng một lượt. Khách hủy thì không mở có tính lượt.
3. Sau xác nhận, xử lý mở thành công, tính đúng một lượt và ghi nhận quyền xem phiên bản; lỗi trước thành công không tính lượt.
4. Cho xem mọi ảnh và tải tệp của phiên bản không tính thêm.
5. Lưu mỗi phiên bản đã xem thành một dòng lịch sử; khi mở lại đúng phiên bản thì mở thẳng miễn lượt, kể cả gói đã hết hạn.

### Alternative Flow

#### ALT-01

Khách đã xem bản cũ, thư viện có bản mới.

1. Lịch sử mở bản cũ; mở bản mới phải kiểm tra điều kiện và xác nhận lượt mới. Sửa tại chỗ cùng phiên bản không tính lượt mới.

### Exception Flow

#### EXC-01

Thiếu quyền mở lần đầu, mẫu bị ẩn, hoặc xử lý lỗi.

1. Không cấp quyền xem mới hoặc tính lượt cho yêu cầu bị từ chối; người đã có quyền vẫn được xem lại mẫu ẩn. Mất mạng sau ghi nhận thành công không được tính trùng khi gửi lại.

## Acceptance Criteria

#### AC-001

- **Given**: Khách có 20 lượt, chưa xem phiên bản A
- **When**: Xác nhận mở A thành công rồi mở lại
- **Then**: Còn 19 lượt sau cả hai lần; quyền xem chỉ thuộc tài khoản đó.

#### AC-002

- **Given**: Đã xem A, chưa xem phiên bản mới B
- **When**: Mở A từ lịch sử rồi xác nhận mở B đủ điều kiện
- **Then**: A miễn lượt; B tính một lượt. Lịch sử có hai dòng riêng.

#### AC-003

- **Given**: Đã xem một phiên bản
- **When**: Gói hết hạn, hết lượt hoặc mất quyền tra cứu trong gói
- **Then**: Vẫn xem/tải phiên bản đã mở khi đăng nhập đúng tài khoản; phiên bản chưa mở vẫn cần điều kiện hiện hành.

#### AC-004

- **Given**: Khách chưa xem mẫu
- **When**: Hủy xác nhận hoặc xử lý lỗi trước thành công
- **Then**: Không tính lượt, không ghi nhận quyền xem; giải phóng lượt tạm giữ nếu có.

#### AC-005

- **Given**: Đã ghi nhận mở thành công nhưng mất phản hồi
- **When**: Gửi lại hoặc mở đồng thời cùng phiên bản
- **Then**: Không tính trùng lượt; không cho tài khoản khác dùng quyền xem đó.

#### AC-006

- **Given**: Đã mở chi tiết
- **When**: Chuyển ảnh hoặc tải PDF/CAD
- **Then**: Không trừ thêm; gọi trực tiếp tài nguyên vẫn kiểm tra quyền xem.

#### AC-007

- **Given**: Khách có quyền tra cứu không giới hạn
- **When**: Mở phiên bản hợp lệ rồi gói hết hạn
- **Then**: Vẫn có quyền xem lại phiên bản đã mở; không tự cấp quyền xem phiên bản khác.

## References

### TDDs

- TDD-LIB-002
- TDD-LIB-001: Nội dung và vòng đời phiên bản được dùng lại.

### Rules

- BR-LIB-002
- BR-LIB-003
- BR-SUB-017
- BR-SUB-005

### Dependencies

## Non-Functional

- Kiểm tra quyền tại backend, kể cả yêu cầu trực tiếp; không chỉ ẩn nút trên giao diện.
- Chưa đặt ngưỡng hiệu năng hay giới hạn hạ tầng khi chưa có căn cứ. Chưa triển khai hoặc thực thi test.

## Out of Scope

- Lưu mẫu yêu thích, mô hình 3D xoay trực tiếp, tìm kiếm kích thước, lựa chọn sắp xếp.
- Sửa phiên bản đã được thay thế; xóa phiên bản đã công bố.
