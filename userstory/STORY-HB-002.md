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

# STORY-HB-002

## Metadata

- **Story**: Là người đọc Cẩm nang, tôi muốn xem nội dung theo ba bước xây nhà và mở từng chủ đề để tìm kiến thức đúng giai đoạn công trình của mình, không cần đăng nhập.
- **Context**: Trang Cẩm nang hiện ba bước kèm ảnh và mô tả; mở một bước thì thấy các chủ đề hướng dẫn và bài viết thuộc chủ đề đó. Bản nháp soạn từ các quyết định người dùng xác nhận ngày 07/10/2026.
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

Người đọc mở trang Cẩm nang, mở một bước hoặc chọn một chủ đề.

## Flow

### Main Flow

1. Mở trang Cẩm nang; thấy phần mở đầu và ba thẻ bước kèm tiêu đề, mô tả ngắn và ảnh đại diện, theo thứ tự Phần thô, Phần hoàn thiện, Trang trí nội thất.
2. Mở một bước; hệ thống hiện các chủ đề hướng dẫn của bước đó theo thứ tự người quản lý đã sắp, kèm biểu tượng của từng chủ đề.
3. Chọn một chủ đề; hệ thống hiện các bài đang công bố gắn chủ đề đó hoặc gắn danh mục con của nó ở mọi cấp, mới công bố trước, mỗi bài một lần.
4. Chọn một bài để đọc theo STORY-NEWS-003.

### Alternative Flow

#### ALT-01

Bước chưa có chủ đề hướng dẫn nào.

1. Hiện trạng thái trống cho bước đó; không lấy chủ đề của bước khác hoặc nhãn tin tức để lấp chỗ.

#### ALT-02

Chủ đề chưa có bài nào đang công bố.

1. Hiện danh sách rỗng; người đọc chọn chủ đề khác hoặc đóng bước.

### Exception Flow

#### EXC-01

Bài trong một chủ đề bị ẩn hoặc xóa trước khi người đọc mở.

1. Không cung cấp nội dung bài; bài đã xóa báo không tìm thấy.
2. Các bài còn lại trong chủ đề vẫn xem được bình thường.

## Acceptance Criteria

#### AC-001

- **Given**: Người đọc chưa đăng nhập và không có gói
- **When**: Mở trang Cẩm nang
- **Then**: Xem được ba bước kèm tiêu đề, mô tả ngắn và ảnh, không bị hỏi đăng nhập hoặc trừ lượt.

#### AC-002

- **Given**: Một bước có nhiều chủ đề hướng dẫn đã được sắp thứ tự
- **When**: Mở bước đó
- **Then**: Các chủ đề hiện đúng thứ tự người quản lý đã lưu, kèm biểu tượng của từng chủ đề.
- **And**: Không hiện nhãn tin tức không thuộc bước nào.

#### AC-003

- **Given**: Chủ đề "Móng" có danh mục con và bài gắn ở cả hai cấp
- **When**: Chọn chủ đề "Móng"
- **Then**: Danh sách gồm bài gắn trực tiếp và bài thuộc mọi cấp con, mỗi bài chỉ xuất hiện một lần.

#### AC-004

- **Given**: Có bài đang nháp, đang ẩn và đang công bố trong một chủ đề
- **When**: Người đọc mở chủ đề đó
- **Then**: Chỉ bài đang công bố được trả về, mới công bố trước.

#### AC-005

- **Given**: Một bài gắn chủ đề của Phần thô và chủ đề của Trang trí nội thất
- **When**: Người đọc mở lần lượt hai bước đó
- **Then**: Bài xuất hiện ở cả hai bước.

#### AC-006

- **Given**: Một bài không gắn danh mục nào
- **When**: Người đọc mở các bước
- **Then**: Bài không xuất hiện trong bước nào.
- **And**: Bài vẫn xuất hiện ở danh sách bài viết chung theo BR-NEWS-003.

#### AC-007

- **Given**: Một bước chưa có chủ đề hướng dẫn nào
- **When**: Người đọc mở bước đó
- **Then**: Hiện trạng thái trống, không hiện dữ liệu minh họa hoặc chủ đề của bước khác.

## References

### TDDs

- TDD-HB-001

### Rules

- BR-HB-001
- BR-NEWS-003

### Dependencies

- STORY-HB-001: người quản lý chuẩn bị nội dung bước và chủ đề hướng dẫn.
- STORY-NEWS-003: đọc chi tiết một bài tin tức.

## Non-Functional

- Phần công khai không được làm lộ bài đang nháp hoặc đang ẩn qua yêu cầu đọc mới.
- Chưa chốt ngưỡng hiệu năng cho việc lấy bài theo cả nhánh chủ đề.

## Out of Scope

- Tìm kiếm trong phạm vi một bước hoặc một chủ đề.
- Lọc nhiều chủ đề cùng lúc và lưu lại chủ đề đang xem giữa các phiên.
- Nhiều bản ngôn ngữ cho tiêu đề bước và tên chủ đề.
