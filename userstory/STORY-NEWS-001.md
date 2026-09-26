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

# STORY-NEWS-001

## Metadata

- **Story**: Là người có quyền quản lý Tin tức, tôi muốn quản lý bài viết để cung cấp tin công khai trong Cẩm nang.
- **Context**: Tin tức dùng rich text và nhiều danh mục riêng; không dùng cơ chế phiên bản hoặc lượt của thư viện mẫu.
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

- Người thao tác đăng nhập và có quyền quản lý tin tức theo STORY-RBAC-001; quyền này không gắn phân công.

### Trigger

Người quản lý tạo hoặc cập nhật bài.

## Flow

### Main Flow

1. Tạo bài và nhập tiêu đề, ảnh đại diện, mô tả ngắn, nội dung rich text; chọn một hoặc nhiều danh mục khi sẵn sàng.
2. Khi chèn ảnh nội dung, FE tải ảnh lên cloud rồi chèn URL vào rich text.
3. Lưu nháp để nhập dần hoặc chọn công bố; hệ thống kiểm đủ dữ liệu trước công bố.
4. Công bố bài hợp lệ trực tiếp, không qua người duyệt; ghi ngày công bố đầu tiên.
5. Khi sửa bài đã công bố, lưu nội dung hợp lệ để cập nhật ngay cùng bài.

### Alternative Flow

#### ALT-01

Chưa đủ dữ liệu công bố.

1. Lưu nháp và tiếp tục nhập sau; bài chưa xuất hiện công khai.

#### ALT-02

Cần ngừng hoặc khôi phục hiển thị.

1. Ẩn bài; danh sách và đường dẫn trực tiếp không cung cấp bài.
2. Công bố lại khi đủ thông tin, giữ ngày công bố đầu tiên.

#### ALT-03

Người quản lý xóa bài ở bất kỳ trạng thái nào.

1. Xóa bài khỏi nội dung phục vụ; đường dẫn cũ báo không tìm thấy.
2. Không có thùng rác hoặc chức năng khôi phục.

### Exception Flow

#### EXC-01

Không có quyền quản lý.

1. Từ chối thao tác, giữ dữ liệu hiện tại.

#### EXC-02

Công bố hoặc lưu sửa bài công bố thiếu trường bắt buộc hay dùng danh mục không còn tồn tại.

1. Thông báo phần không hợp lệ; không công bố hoặc thay bài hiện hành bằng dữ liệu lỗi.

#### EXC-03

Upload ảnh thất bại.

1. Thông báo lỗi, không chèn URL của ảnh chưa upload thành công; cho người quản lý thử lại.

#### EXC-04

Tiêu đề, mô tả ngắn hoặc nội dung vượt giới hạn độ dài.

1. Từ chối lưu, chỉ rõ phần vượt giới hạn; giữ nguyên bài hiện tại và không tự cắt ngắn.

## Acceptance Criteria

#### AC-001

- **Given**: Người có quyền đang tạo bài chưa đủ thông tin
- **When**: Lưu nháp
- **Then**: Lưu được để nhập tiếp, khách không đọc được bài.

#### AC-002

- **Given**: Bài thiếu một trong năm thành phần bắt buộc
- **When**: Công bố
- **Then**: Từ chối; chỉ công bố khi có tiêu đề, ảnh đại diện, mô tả ngắn, nội dung và ít nhất một danh mục hợp lệ.

#### AC-003

- **Given**: Bài chọn các danh mục cha hoặc con ở nhiều nhánh
- **When**: Công bố bài đủ dữ liệu
- **Then**: Giữ các danh mục đã chọn, không bắt chọn cha hoặc danh mục chính.

#### AC-004

- **Given**: Ảnh được FE upload thành công
- **When**: Chèn vào rich text rồi lưu bài
- **Then**: Nội dung lưu tham chiếu URL ảnh; hỗ trợ nhiều ảnh, định dạng chữ, tiêu đề đoạn, danh sách và liên kết.

#### AC-005

- **Given**: Bài đang công bố
- **When**: Lưu sửa hợp lệ
- **Then**: Khách đọc nội dung mới ngay trên cùng bài; không tạo phiên bản hoặc đổi ngày công bố đầu tiên.

#### AC-006

- **Given**: Bài đã công bố
- **When**: Ẩn rồi công bố lại
- **Then**: Khi ẩn không đọc công khai được; khi công bố lại giữ ngày công bố đầu tiên.

#### AC-007

- **Given**: Bài nháp, công bố hoặc ẩn
- **When**: Người quản lý xóa bài
- **Then**: Xóa được ở cả ba trạng thái; bài biến mất và đường dẫn cũ báo không tìm thấy, không khôi phục.

#### AC-008

- **Given**: Người không có quyền quản lý
- **When**: Gọi trực tiếp thao tác quản trị
- **Then**: Bị từ chối, không thay đổi dữ liệu.

#### AC-009

- **Given**: Người có quyền đang lưu nháp hoặc sửa một bài
- **When**: Tiêu đề dài 200 ký tự, mô tả ngắn 500 ký tự sau khi bỏ khoảng trắng đầu/cuối và nội dung 200.000 ký tự, rồi thử lại với 201, 501 hoặc 200.001 ký tự
- **Then**: Lần đầu lưu được; mỗi lần vượt giới hạn bị từ chối, không tự cắt ngắn.
- **And**: Yêu cầu bị từ chối không thay đổi bài hiện tại.

## References

### TDDs

- TDD-NEWS-001
- TDD-NEWS-002

### Rules

- BR-NEWS-001
- BR-NEWS-002

### Dependencies

- STORY-NEWS-002

## Non-Functional

- Kiểm tra quyền quản lý tại backend, kể cả yêu cầu trực tiếp; quyền đọc công khai không cấp quyền sửa dữ liệu.
- Rich text phải hiển thị an toàn, không thực thi mã do người soạn chèn. Chi tiết kiểm soát thuộc bước thiết kế kỹ thuật.
- Chưa chốt ngưỡng hiệu năng hoặc giới hạn truyền tải. Backend đã triển khai ở nhánh `feature/news` của `bmt-be`, chưa merge; chưa chạy System Test.

## Out of Scope

- Duyệt bài, phiên bản nội dung, lịch sử xem, video và tệp đính kèm.
- Thùng rác, khôi phục bài, ghim tin và các tương tác người đọc.
