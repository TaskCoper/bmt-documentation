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

# STORY-HB-004

## Metadata

- **Story**: Là người có quyền quản lý Tin tức, tôi muốn sửa các khối chữ cố định của trang Cẩm nang để đổi lời giới thiệu mà không cần nhờ lập trình viên.
- **Context**: Trang Cẩm nang có vài khối chữ cố định: tên hai tab, phần mở đầu, tiêu đề nhóm ba bước, tiêu đề và mô tả khối bài viết, tiêu đề khối Bản tin, banner của tab Thư viện mẫu. Hiện các chữ này nằm trong mã frontend. Bản nháp soạn từ các quyết định người dùng xác nhận ngày 07/10/2026 và 08/10/2026.
- **Sprint**:
- **Priority**: Should
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác đăng nhập và có quyền quản lý tin tức theo STORY-RBAC-001.

### Trigger

Người quản lý mở phần quản lý nội dung trang để sửa một khối chữ.

## Flow

### Main Flow

1. Mở danh sách các khối của trang Cẩm nang.
2. Chọn một khối; hệ thống hiện các trường đúng theo loại khối đó.
3. Nhập nội dung rồi lưu; hệ thống kiểm cấu trúc theo loại khối trước khi ghi.
4. Nội dung mới hiển thị ngay trên trang công khai ở ngôn ngữ tương ứng.

### Alternative Flow

#### ALT-01

Khối chưa từng được sửa.

1. Trang công khai dùng bản dịch sẵn có trong giao diện; không để trống chữ.
2. Người quản lý sửa khối bất cứ lúc nào để thay bản mặc định đó.

#### ALT-02

Quay về nội dung mặc định.

1. Người quản lý xóa bản đã lưu của khối.
2. Trang công khai quay lại dùng bản dịch sẵn có trong giao diện.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý tin tức.

1. Từ chối thao tác và giữ nguyên nội dung hiện tại.

#### EXC-02

Nội dung gửi lên không đúng cấu trúc của loại khối.

1. Từ chối lưu, chỉ rõ phần sai và giữ nguyên nội dung đang hiển thị.

#### EXC-03

Ảnh trong khối không phải đường dẫn https thuộc tên miền kho ảnh đã cấu hình.

1. Từ chối lưu và giữ nguyên nội dung đang hiển thị.

#### EXC-04

Một trường chữ trong khối dài hơn 1.000 ký tự.

1. Từ chối lưu, chỉ rõ trường vượt giới hạn và giữ nguyên nội dung đang hiển thị; không tự cắt bớt chữ.

## Acceptance Criteria

#### AC-001

- **Given**: Người thao tác không có quyền quản lý tin tức
- **When**: Gọi trực tiếp thao tác sửa khối
- **Then**: Bị từ chối và nội dung không thay đổi.

#### AC-002

- **Given**: Người quản lý đang sửa khối phần mở đầu của trang Cẩm nang
- **When**: Lưu nội dung đúng cấu trúc
- **Then**: Trang công khai hiện nội dung mới ngay ở ngôn ngữ tương ứng.

#### AC-003

- **Given**: Một khối chưa từng được sửa
- **When**: Người đọc mở trang Cẩm nang
- **Then**: Trang hiện bản dịch sẵn có trong giao diện, không để trống chữ.

#### AC-004

- **Given**: Người quản lý đang sửa một khối
- **When**: Gửi nội dung thiếu trường bắt buộc hoặc sai cấu trúc của loại khối
- **Then**: Từ chối lưu, chỉ rõ phần sai và giữ nguyên nội dung đang hiển thị.

#### AC-005

- **Given**: Một khối đã có bản tiếng Việt
- **When**: Người đọc mở trang ở ngôn ngữ chưa có bản riêng
- **Then**: Trang dùng bản dịch sẵn có trong giao diện cho ngôn ngữ đó, không hiện nhầm bản tiếng Việt.

#### AC-006

- **Given**: Người quản lý đang sửa một khối
- **When**: Lưu một trường chữ dài đúng 1.000 ký tự, rồi lưu một trường chữ dài 1.001 ký tự
- **Then**: Bản 1.000 ký tự được lưu và hiển thị đủ chữ; bản 1.001 ký tự bị từ chối, chỉ rõ trường vượt giới hạn.
- **And**: Hệ thống không tự cắt bớt chữ và giữ nguyên nội dung đang hiển thị.

#### AC-007

- **Given**: Người quản lý đang sửa banner của tab Thư viện mẫu
- **When**: Lưu tiêu đề gồm hai dòng và một mô tả mới, không chọn ảnh
- **Then**: Tab Thư viện mẫu hiện tiêu đề đúng chỗ xuống dòng và mô tả mới ở ngôn ngữ tương ứng.
- **And**: Banner vẫn dùng ảnh mặc định vì không chọn ảnh.

#### AC-008

- **Given**: Người quản lý đang sửa tên hai tab của trang Cẩm nang
- **When**: Đổi tên tab Tin tức và tab Thư viện mẫu rồi lưu
- **Then**: Hai nút chuyển tab hiện tên mới ở ngôn ngữ tương ứng và việc chuyển tab vẫn hoạt động như trước.

## References

### TDDs

- TDD-HB-003

### Rules

- BR-HB-003

### Dependencies

## Non-Functional

- Kiểm quyền tại backend cho mọi thao tác quản trị.
- Nội dung khối phải hiển thị an toàn, không thực thi mã do người soạn chèn.

## Out of Scope

- Quản lý nội dung cho các trang khác ngoài Cẩm nang.
- Lịch sử sửa, bản nháp và hẹn giờ xuất bản cho khối chữ.
- Kéo thả để thêm, xóa hoặc đổi thứ tự khối trên trang.
- Chữ chức năng của tab Thư viện mẫu (nhãn bộ lọc, gợi ý ô tìm kiếm, thông báo không có mẫu) và danh sách mẫu bản vẽ; danh sách mẫu có màn quản lý riêng.
