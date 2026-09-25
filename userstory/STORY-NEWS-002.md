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

# STORY-NEWS-002

## Metadata

- **Story**: Là người có quyền quản lý Tin tức, tôi muốn quản lý cây danh mục riêng để tổ chức và phân loại bài viết.
- **Context**: Danh mục đa cấp không giới hạn độ sâu; một bài gắn được nhiều danh mục.
- **Sprint**:
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

- Người thao tác đăng nhập và có cùng quyền quản lý tin tức theo STORY-RBAC-001 dùng cho bài viết.

### Trigger

Người quản lý tạo, đổi tên, đổi thứ tự, chuyển cha hoặc xóa danh mục.

## Flow

### Main Flow

1. Nhập tên và chọn vị trí gốc hoặc cha cho danh mục.
2. Kiểm tên bắt buộc, tối đa 200 ký tự và không trùng trong cùng cha sau khi bỏ khoảng trắng đầu/cuối, không phân biệt hoa/thường.
3. Lưu danh mục trong cây riêng của Tin tức để người quản lý gắn bài và khách lọc tin.

### Alternative Flow

#### ALT-01

Cập nhật danh mục hiện có.

1. Cho đổi tên, thứ tự hoặc chuyển cha khi hợp lệ.
2. Giữ liên kết với bài; kết quả lọc dùng cây sau thay đổi.

#### ALT-02

Xóa danh mục không còn được sử dụng.

1. Kiểm không có danh mục con và không có bài liên kết.
2. Xóa danh mục khi cả hai điều kiện thỏa mãn.

### Exception Flow

#### EXC-01

Tên trống, dài quá 200 ký tự, trùng tên cùng cha hoặc chuyển cha tạo vòng lặp.

1. Từ chối thay đổi và giữ cây hiện tại.

#### EXC-02

Xóa danh mục có con hoặc có bài nháp, công bố hay ẩn liên kết.

1. Từ chối xóa; yêu cầu xử lý bài và danh mục con trước.

#### EXC-03

Người thao tác thiếu quyền quản lý.

1. Từ chối thay đổi.

## Acceptance Criteria

#### AC-001

- **Given**: Có cây nhiều cấp
- **When**: Tạo danh mục con ở cấp tiếp theo
- **Then**: Không bị chặn bởi giới hạn số cấp nghiệp vụ.

#### AC-002

- **Given**: Có danh mục tên “Sơn”
- **When**: Tạo “ sơn ” dưới cùng cha
- **Then**: Từ chối trùng tên; dưới cha khác được phép; các danh mục gốc cùng xét một cấp.

#### AC-003

- **Given**: Danh mục có nhánh con
- **When**: Chuyển cha vào chính nó hoặc một hậu duệ
- **Then**: Từ chối, không tạo vòng lặp.

#### AC-004

- **Given**: Danh mục đang gắn bài
- **When**: Đổi tên, thứ tự hoặc chuyển cha hợp lệ
- **Then**: Liên kết bài giữ nguyên và bộ lọc phản ánh cây mới.

#### AC-005

- **Given**: Cha đích có danh mục cùng tên
- **When**: Chuyển danh mục tới cha đó
- **Then**: Từ chối trùng tên theo cùng quy tắc chuẩn hóa.

#### AC-006

- **Given**: Danh mục còn con hoặc bài ở bất kỳ trạng thái nào
- **When**: Xóa danh mục
- **Then**: Từ chối; chỉ xóa được khi không còn cả con lẫn bài liên kết.

#### AC-007

- **Given**: Người không có quyền quản lý Tin tức
- **When**: Thay đổi danh mục
- **Then**: Bị từ chối, không đổi dữ liệu.

#### AC-008

- **Given**: Người có quyền quản lý tin tức tạo hoặc đổi tên danh mục.
- **When**: Tên sau khi bỏ khoảng trắng đầu/cuối dài 200 ký tự, rồi thử lại với 201 ký tự.
- **Then**: Tên 200 ký tự được lưu; tên 201 ký tự bị từ chối và không tự cắt ngắn.
- **And**: Yêu cầu bị từ chối không đổi tên hoặc vị trí của danh mục.

## References

### TDDs

- TDD-NEWS-002
- TDD-NEWS-001

### Rules

- BR-NEWS-001
- BR-NEWS-002

### Dependencies



## Non-Functional

- Kiểm tra quyền quản lý tại backend, kể cả yêu cầu trực tiếp; quyền đọc công khai không cấp quyền sửa dữ liệu.
- Rich text phải hiển thị an toàn, không thực thi mã do người soạn chèn. Chi tiết kiểm soát thuộc bước thiết kế kỹ thuật.
- Chưa chốt ngưỡng hiệu năng hoặc giới hạn truyền tải. Chưa triển khai hoặc chạy kiểm thử.

## Out of Scope

- Dùng chung danh mục loại công trình hoặc thư viện mẫu.
- Giới hạn số cấp và phân quyền quản lý danh mục riêng với quản lý bài.
