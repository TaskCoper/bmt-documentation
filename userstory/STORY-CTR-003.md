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

# STORY-CTR-003

## Metadata

- **Story**: Là admin, tôi muốn cấu hình danh mục phạm vi thi công để nhà thầu và dự án dùng thống nhất, và có thể tái sử dụng cho tính năng sau.
- **Context**: Loại công trình tái sử dụng danh mục đã có. Phạm vi thi công là danh mục admin cấu hình, khác với khu vực địa lý phục vụ của công ty. Người dùng đã chốt tên bắt buộc, mô tả tùy chọn, thứ tự hiển thị, trạng thái Đang dùng/Ngừng dùng, giữ liên kết cũ và không xóa mục đang được sử dụng. Nghiệp vụ đã được người dùng chốt trong hội thoại; chưa triển khai.
- **Sprint**: [Chưa xác định]
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

- Người thao tác đã đăng nhập với quyền admin.

### Trigger

Admin mở phần cấu hình phạm vi thi công.

## Flow

### Main Flow

1. Admin tạo hoặc sửa một mục trong danh mục phạm vi thi công, nhập tên bắt buộc, mô tả tùy chọn, thứ tự và trạng thái Đang dùng/Ngừng dùng.
2. Hệ thống kiểm tra tên không trùng với mục khác rồi lưu cấu hình để hồ sơ nhà thầu và dự án chọn cùng danh mục.
3. Các mục Đang dùng được chọn để gắn mới; GUID được dùng cho query lọc phạm vi thi công.

### Alternative Flow

#### ALT-01

Admin ngừng sử dụng một mục đã được gắn vào nhà thầu hoặc dự án.

1. Admin chuyển phạm vi sang Ngừng dùng.
2. Hệ thống không cho gắn mới phạm vi đó; vẫn giữ các liên kết đã có.

#### ALT-02

Admin xóa mục chưa được sử dụng.

1. Admin chọn mục cần xóa.
2. Hệ thống kiểm tra mục chưa được sử dụng rồi xóa khỏi danh mục.

#### ALT-03

Admin đổi tên mục đã gắn vào hồ sơ hoặc dự án.

1. Admin nhập tên mới không trùng với mục khác và lưu.
2. Các hồ sơ và dự án đang gắn mục đó hiển thị tên mới; các liên kết được giữ nguyên.

### Exception Flow

#### EXC-01

Người thao tác không có quyền admin.

1. Từ chối cập nhật danh mục.

#### EXC-02

Admin yêu cầu xóa một mục đang được nhà thầu hoặc dự án sử dụng.

1. Từ chối xóa mục đang được sử dụng; giữ mục và các liên kết.

#### EXC-03

Mục phạm vi thi công thiếu tên.

1. Từ chối lưu mục không có tên.

#### EXC-04

Admin tạo hoặc sửa mục với tên trùng một mục khác.

1. Từ chối lưu và thông báo tên bị trùng.
2. Giữ nguyên dữ liệu đã có.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý danh mục.
- **When**: Admin tạo hoặc sửa mục có tên, mô tả tùy chọn, thứ tự và trạng thái rồi lưu thành công.
- **Then**: Nhà thầu và dự án có thể dùng cùng danh mục đã lưu.
- **And**: Không tạo danh mục phạm vi riêng cho từng nhà thầu hoặc từng dự án.

#### AC-002

- **Given**: Hệ thống có danh mục loại công trình do admin đã cấu hình.
- **When**: Admin chọn loại cho nhà thầu hoặc dự án đã thực hiện.
- **Then**: Dùng danh mục loại công trình đã có.
- **And**: Không tạo danh mục loại công trình độc lập cho tính năng nhà thầu.

#### AC-003

- **Given**: Phạm vi S đang được gắn vào nhà thầu A và dự án P.
- **When**: Admin chuyển S sang Ngừng dùng.
- **Then**: Các liên kết của A và P với S được giữ; S không được gắn mới.
- **And**: Không tự xóa dữ liệu đã có để ngừng dùng danh mục.

#### AC-004

- **Given**: Một mục phạm vi đang được nhà thầu hoặc dự án sử dụng.
- **When**: Admin yêu cầu xóa mục đó.
- **Then**: Từ chối xóa.
- **And**: Danh mục và các liên kết giữ nguyên.

#### AC-005

- **Given**: Phạm vi S chưa được sử dụng.
- **When**: Admin xóa S.
- **Then**: Xóa thành công; S không còn trong danh mục để chọn.
- **And**: Các mục khác giữ nguyên.

#### AC-006

- **Given**: Danh mục đã có phạm vi tên “Phần thô”.
- **When**: Admin tạo mục mới hoặc đổi tên một mục khác thành “Phần thô”.
- **Then**: Từ chối vì trùng tên.
- **And**: Giữ nguyên dữ liệu trước yêu cầu.

#### AC-007

- **Given**: Phạm vi S đang gắn với hồ sơ nhà thầu A và dự án P.
- **When**: Admin đổi tên S thành một tên hợp lệ không trùng mục khác.
- **Then**: A và P hiển thị tên mới của S.
- **And**: Giữ nguyên các liên kết với S, không yêu cầu admin gắn lại danh mục.

## References

### TDDs

- [TDD-CTR-001](../tdd/TDD-CTR-001.md)

### Rules

- BR-CTR-001
- BR-CTR-007

### Dependencies

## Non-Functional

- Đặc tả kiểm thử liên quan: [ST-CTR-017](../systemtest/ST-CTR-017.md), [ST-CTR-018](../systemtest/ST-CTR-018.md), [ST-CTR-019](../systemtest/ST-CTR-019.md), [ST-CTR-020](../systemtest/ST-CTR-020.md), [ST-CTR-021](../systemtest/ST-CTR-021.md), [ST-CTR-022](../systemtest/ST-CTR-022.md), [ST-CTR-023](../systemtest/ST-CTR-023.md), [ST-CTR-024](../systemtest/ST-CTR-024.md). Các ca chưa chạy.

- Quyền quản trị được kiểm tra tại backend.
- Giới hạn tên và xử lý cập nhật đồng thời theo TDD-CTR-001 đã được người dùng chốt. Metadata phân công chưa xác định.
- Đã soạn đặc tả System Test theo nghiệp vụ được người dùng chốt; chưa chạy kiểm thử. Người dùng đã chốt TDD và đã soạn đặc tả Unit Test liên kết trong TDD; chưa triển khai mã ứng dụng hoặc chạy bộ test của tính năng.

## Out of Scope

- Triển khai các tính năng tương lai tái sử dụng phạm vi thi công.
- Thay đổi nghiệp vụ cấu hình tầng/tum/phong cách của danh mục loại công trình đã có.
