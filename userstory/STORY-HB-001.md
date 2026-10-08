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

# STORY-HB-001

## Metadata

- **Story**: Là người có quyền quản lý Tin tức, tôi muốn sửa nội dung ba bước xây nhà và sắp xếp chủ đề hướng dẫn trong từng bước để người đọc tìm được kiến thức theo đúng giai đoạn công trình.
- **Context**: Trang Cẩm nang có ba bước cố định: Phần thô, Phần hoàn thiện, Trang trí nội thất. Chủ đề hướng dẫn dùng chung cây danh mục của Tin tức, phân biệt với nhãn tin tức bằng một cờ hiển thị. Bản nháp soạn từ các quyết định người dùng xác nhận ngày 07/10/2026.
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
- Ba bước đã có sẵn trong hệ thống; người quản lý không tạo bước.

### Trigger

Người quản lý mở phần quản lý Cẩm nang để sửa một bước hoặc sắp xếp chủ đề hướng dẫn.

## Flow

### Main Flow

1. Mở danh sách ba bước theo thứ tự Phần thô, Phần hoàn thiện, Trang trí nội thất rồi chọn một bước.
2. Sửa tiêu đề, mô tả ngắn và ảnh đại diện của bước. Ảnh do frontend tải lên kho trước, backend chỉ nhận đường dẫn.
3. Lưu; hệ thống kiểm đủ ba trường rồi cập nhật ngay nội dung công khai.
4. Tạo danh mục gốc làm chủ đề hướng dẫn: nhập tên, bật cờ chủ đề hướng dẫn, chọn bước và chọn một biểu tượng.
5. Sắp xếp thứ tự các chủ đề trong bước; phần công khai hiển thị theo thứ tự đã lưu.

### Alternative Flow

#### ALT-01

Chuyển một nhãn tin tức sẵn có thành chủ đề hướng dẫn.

1. Mở danh mục gốc đó, bật cờ chủ đề hướng dẫn, chọn bước và biểu tượng.
2. Các bài đang gắn danh mục đó xuất hiện trong bước tương ứng; liên kết bài giữ nguyên.

#### ALT-02

Chuyển chủ đề hướng dẫn sang bước khác.

1. Chọn bước mới cho danh mục gốc rồi lưu.
2. Toàn bộ nhánh con và các bài trong nhánh chuyển sang bước mới; không phải sửa từng bài.

#### ALT-03

Ngừng dùng một danh mục làm chủ đề hướng dẫn.

1. Tắt cờ chủ đề hướng dẫn; hệ thống bỏ biểu tượng của danh mục.
2. Danh mục trở lại là nhãn tin tức và không còn hiện trong bước; bài vẫn giữ liên kết danh mục.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý tin tức.

1. Từ chối thao tác quản trị và giữ nguyên dữ liệu, kể cả khi yêu cầu gửi thẳng tới API.

#### EXC-02

Lưu bước nhưng thiếu tiêu đề, mô tả ngắn hoặc ảnh đại diện.

1. Chỉ rõ trường còn thiếu, không lưu và giữ nguyên nội dung đang hiển thị.

#### EXC-03

Bật cờ chủ đề hướng dẫn nhưng không chọn bước.

1. Từ chối lưu và yêu cầu chọn bước; danh mục giữ nguyên trạng thái cũ.

#### EXC-04

Gắn bước, bật cờ chủ đề hướng dẫn hoặc chọn biểu tượng cho một danh mục con.

1. Từ chối thay đổi và nêu rõ ba thuộc tính này chỉ đặt ở danh mục gốc.

#### EXC-05

Ảnh của bước không phải đường dẫn https thuộc tên miền kho ảnh đã cấu hình.

1. Từ chối lưu và giữ nguyên ảnh hiện tại.

#### EXC-06

Yêu cầu thêm bước mới hoặc xóa một bước.

1. Hệ thống không cung cấp thao tác này; số bước và thứ tự bước không đổi.

#### EXC-07

Người quản lý dời một chủ đề khi người khác vừa dời hoặc sửa chính chủ đề đó.

1. Từ chối thao tác dời vì dữ liệu đã cũ và giữ nguyên thứ tự người kia vừa lưu.
2. Màn quản lý tải lại danh sách để người quản lý thấy thứ tự mới nhất rồi tự quyết định có dời tiếp không.

## Acceptance Criteria

#### AC-001

- **Given**: Người thao tác không có quyền quản lý tin tức
- **When**: Gọi trực tiếp thao tác sửa bước hoặc sửa danh mục
- **Then**: Bị từ chối và dữ liệu không thay đổi.

#### AC-002

- **Given**: Người quản lý đang sửa một bước
- **When**: Lưu với đủ tiêu đề, mô tả ngắn và ảnh đại diện
- **Then**: Nội dung mới hiển thị ngay trên trang Cẩm nang công khai.

#### AC-003

- **Given**: Người quản lý đang sửa một bước
- **When**: Lưu khi thiếu một trong ba trường bắt buộc
- **Then**: Từ chối, chỉ rõ trường thiếu và giữ nguyên nội dung đang hiển thị.
- **And**: Không tự điền giá trị mặc định cho trường thiếu.

#### AC-004

- **Given**: Người quản lý đang tạo hoặc sửa một danh mục gốc
- **When**: Bật cờ chủ đề hướng dẫn nhưng để trống bước
- **Then**: Từ chối lưu và yêu cầu chọn bước.

#### AC-005

- **Given**: Một danh mục con trong cây
- **When**: Gắn bước, bật cờ chủ đề hướng dẫn hoặc chọn biểu tượng cho nó
- **Then**: Bị từ chối; ba thuộc tính này chỉ đặt được ở danh mục gốc.

#### AC-006

- **Given**: Chủ đề hướng dẫn đang thuộc Phần thô và có danh mục con cùng các bài liên kết
- **When**: Chuyển chủ đề đó sang Phần hoàn thiện
- **Then**: Cả nhánh con và các bài trong nhánh hiện ở Phần hoàn thiện, không còn ở Phần thô.
- **And**: Liên kết bài với danh mục giữ nguyên.

#### AC-007

- **Given**: Một danh mục gốc đang là chủ đề hướng dẫn, có bước và biểu tượng
- **When**: Chuyển nó thành con của một danh mục khác
- **Then**: Bước, cờ chủ đề hướng dẫn và biểu tượng của nó bị xóa; nhánh theo danh mục gốc mới.

#### AC-008

- **Given**: Người quản lý mở phần quản lý Cẩm nang
- **When**: Xem danh sách bước
- **Then**: Luôn có đúng ba bước theo thứ tự cố định, không có thao tác thêm hoặc xóa bước.

#### AC-009

- **Given**: Bước Phần thô có ba chủ đề theo thứ tự A, B, C
- **When**: Người quản lý dời B lên một vị trí
- **Then**: Thứ tự trong bước là B, A, C và trang công khai hiện đúng thứ tự này.
- **And**: A (đầu bước) không dời lên được và C (cuối bước) không dời xuống được.
- **And**: Nhãn tin tức hoặc chủ đề của bước khác nằm xen giữa các chủ đề trên vẫn giữ nguyên thứ tự so với nhau.

#### AC-010

- **Given**: Hai người quản lý cùng mở danh sách chủ đề của Phần thô và cùng thấy B ở vị trí cũ
- **When**: Người thứ nhất dời B lên, sau đó người thứ hai dời B xuống theo danh sách cũ
- **Then**: Thao tác của người thứ hai bị từ chối vì dữ liệu đã cũ và thứ tự người thứ nhất vừa lưu giữ nguyên.
- **And**: Màn của người thứ hai tải lại danh sách mới.

## References

### TDDs

- TDD-HB-001

### Rules

- BR-HB-001
- BR-NEWS-002

### Dependencies

- STORY-NEWS-002: quản lý cây danh mục của Tin tức.

## Non-Functional

- Kiểm quyền tại backend cho mọi thao tác quản trị; ẩn nút trên giao diện không thay thế kiểm quyền.
- Ảnh của bước theo cùng quy định đường dẫn đang áp dụng cho ảnh bài tin tức; backend không nhận dữ liệu ảnh.
- Chưa chốt ngưỡng hiệu năng hoặc số lượng chủ đề tối đa trong một bước.

## Out of Scope

- Thêm, xóa hoặc đổi thứ tự ba bước.
- Nhiều bản ngôn ngữ cho tiêu đề bước và tên chủ đề; phần này dùng bản dịch sẵn có trong giao diện.
- Phân quyền riêng cho quản lý Cẩm nang tách khỏi quản lý tin tức.
