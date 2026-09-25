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

# STORY-NEWS-003

## Metadata

- **Story**: Là người đọc Cẩm nang, tôi muốn tìm, lọc và đọc tin tức để tham khảo thông tin mà không cần tài khoản hoặc gói.
- **Context**: Tin tức công khai, miễn lượt; danh mục đa cấp giúp tìm bài theo chủ đề.
- **Sprint**:
- **Priority**: Must
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Không yêu cầu đăng nhập hoặc sở hữu gói.

### Trigger

Người đọc mở phần Tin tức trong Cẩm nang hoặc đường dẫn bài.

## Flow

### Main Flow

1. Hiển thị các bài đang công bố theo ngày công bố đầu tiên, mới nhất trước, có phân trang.
2. Người đọc nhập từ khóa tiêu đề và có thể chọn một danh mục.
3. Lọc bài khớp từ khóa và thuộc danh mục đã chọn hoặc các danh mục con ở mọi cấp; loại trùng trước khi phân trang.
4. Mở bài và hiển thị nội dung rich text cùng ảnh; không kiểm gói và không trừ lượt.

### Alternative Flow

#### ALT-01

Không chọn danh mục hoặc không nhập từ khóa.

1. Chỉ áp dụng điều kiện được nhập; không có cả hai thì hiển thị danh sách công khai.

#### ALT-02

Không có bài khớp.

1. Hiển thị danh sách rỗng, cho thay đổi điều kiện tìm/lọc.

### Exception Flow

#### EXC-01

Mở bài nháp, ẩn hoặc đã xóa bằng đường dẫn trực tiếp.

1. Không cung cấp nội dung; bài đã xóa báo không tìm thấy.

## Acceptance Criteria

#### AC-001

- **Given**: Người chưa đăng nhập hoặc không có gói
- **When**: Mở một bài đang công bố
- **Then**: Đọc được miễn phí, không trừ lượt hoặc yêu cầu mua gói.

#### AC-002

- **Given**: Bài gắn cả “Vật liệu” và “Vật liệu → Sơn”
- **When**: Lọc “Vật liệu”
- **Then**: Bài xuất hiện một lần; kết quả gồm bài gắn trực tiếp và mọi cấp con.

#### AC-003

- **Given**: Nhập từ khóa và chọn một danh mục
- **When**: Tìm tin
- **Then**: Chỉ trả bài đồng thời khớp tiêu đề và nhánh danh mục, có phân trang; không khớp thì danh sách rỗng.

#### AC-004

- **Given**: Bài cũ được sửa hoặc ẩn rồi công bố lại
- **When**: Xem danh sách
- **Then**: Thứ tự dựa trên ngày công bố đầu tiên, không tự đẩy bài cũ lên đầu.

#### AC-005

- **Given**: Bài đang nháp, ẩn hoặc đã xóa
- **When**: Xem danh sách hoặc mở trực tiếp
- **Then**: Không được đọc nội dung công khai; đường dẫn bài đã xóa báo không tìm thấy.

## References

### TDDs

- TDD-NEWS-001
- TDD-NEWS-002

### Rules

- BR-NEWS-001
- BR-NEWS-002
- BR-NEWS-003

### Dependencies

- STORY-NEWS-001
- STORY-NEWS-002

## Non-Functional

- Kiểm tra quyền quản lý tại backend, kể cả yêu cầu trực tiếp; quyền đọc công khai không cấp quyền sửa dữ liệu.
- Rich text phải hiển thị an toàn, không thực thi mã do người soạn chèn. Chi tiết kiểm soát thuộc bước thiết kế kỹ thuật.
- Chưa chốt ngưỡng hiệu năng hoặc giới hạn truyền tải. Chưa triển khai hoặc chạy kiểm thử.

## Out of Scope

- Bình luận, thích, lưu yêu thích, ghim tin và thống kê lượt đọc.
- Lịch sử xem, quota, tìm trong toàn bộ nội dung hoặc lọc nhiều danh mục cùng lúc.
