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

# STORY-CTR-002

## Metadata

- **Story**: Là admin, tôi muốn quản lý các dự án nhà thầu đã thực hiện để khách xem kinh nghiệm thi công của nhà thầu.
- **Context**: Dự án đã thực hiện là phần hồ sơ năng lực của nhà thầu, lưu riêng với bảng công trình của khách. Mỗi dự án có một loại công trình, một phạm vi thi công và bộ ảnh. Toàn bộ dự án hiện theo trạng thái công khai của hồ sơ nhà thầu. Nghiệp vụ đã được người dùng chốt trong hội thoại; chưa triển khai.
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

- Admin đã đăng nhập và nhà thầu đã có hồ sơ.
- Có danh mục loại công trình dùng chung và danh mục phạm vi thi công để chọn.

### Trigger

Admin mở phần dự án đã thực hiện trong hồ sơ nhà thầu để thêm, sửa hoặc xóa dự án.

## Flow

### Main Flow

1. Admin nhập tên dự án, chọn đúng một loại công trình và một phạm vi thi công, thêm ít nhất một ảnh.
2. Admin có thể bổ sung kích thước, diện tích, số tầng, tum, địa điểm, thời gian/năm hoàn thành, vai trò và hạng mục chính.
3. Hệ thống kiểm tra các trường bắt buộc và lưu dự án thuộc hồ sơ nhà thầu, riêng với công trình của khách.
4. Khi khách xem hồ sơ nhà thầu đang hiển thị, hệ thống cung cấp toàn bộ dự án của nhà thầu; không có ẩn hoặc nổi bật riêng.

### Alternative Flow

#### ALT-01

Admin cập nhật một dự án đã có.

1. Admin sửa thông tin, ảnh hoặc danh mục của dự án.
2. Hệ thống kiểm tra lại trường bắt buộc và lưu; không phát sinh xác minh riêng.

#### ALT-02

Admin xóa một dự án đã thực hiện.

1. Admin chọn dự án cần xóa trong hồ sơ nhà thầu.
2. Hệ thống kiểm tra quyền và xóa dự án; dự án không còn xuất hiện trong hồ sơ nhà thầu.

### Exception Flow

#### EXC-01

Dự án thiếu tên, loại công trình, phạm vi thi công hoặc ảnh.

1. Từ chối lưu dự án không đáp ứng BR-CTR-003; chỉ rõ thông tin cần bổ sung.

#### EXC-02

Người thao tác không có quyền admin.

1. Từ chối và không tạo, cập nhật hoặc xóa dự án.

## Acceptance Criteria

#### AC-001

- **Given**: Nhà thầu đã có hồ sơ và admin có quyền.
- **When**: Admin lưu dự án có tên, đúng một loại công trình, đúng một phạm vi và ít nhất một ảnh.
- **Then**: Dự án được lưu trong hồ sơ nhà thầu.
- **And**: Không tạo hoặc thay một công trình của khách để đại diện cho dự án này.

#### AC-002

- **Given**: Admin thêm hoặc sửa dự án.
- **When**: Dự án thiếu ảnh hoặc thiếu một trường bắt buộc.
- **Then**: Từ chối lưu và chỉ rõ phần còn thiếu.
- **And**: Không tự tạo ảnh hoặc tự chọn danh mục.

#### AC-003

- **Given**: Nhà thầu có nhiều dự án và hồ sơ đang hiển thị.
- **When**: Khách mở phần dự án đã thực hiện.
- **Then**: Khách được xem toàn bộ dự án và mở thông tin chi tiết của từng dự án.
- **And**: Không có bước xác minh hoặc trạng thái hiển thị riêng của dự án.

#### AC-004

- **Given**: Nhà thầu có dự án thuộc loại công trình dùng chung và phạm vi admin cấu hình.
- **When**: Khách mở chi tiết dự án.
- **Then**: Hiển thị tên, bộ ảnh và hai danh mục; các thông tin bổ sung đã nhập cũng được cung cấp.
- **And**: Mỗi dự án chỉ có một loại công trình và một phạm vi thi công.

#### AC-005

- **Given**: Nhà thầu có dự án P và admin có quyền quản lý.
- **When**: Admin xóa riêng dự án P thành công.
- **Then**: P không còn xuất hiện trong danh sách dự án của nhà thầu.
- **And**: Hồ sơ nhà thầu và các dự án khác vẫn được giữ.

## References

### TDDs

- [TDD-CTR-001](../tdd/TDD-CTR-001.md)

### Rules

- BR-CTR-001
- BR-CTR-003
- BR-CTR-007

### Dependencies

- STORY-CTR-001: Hồ sơ nhà thầu và trạng thái công khai.
- STORY-CTR-003: Danh mục phạm vi thi công.

## Non-Functional

- Đặc tả kiểm thử liên quan: [ST-CTR-011](../systemtest/ST-CTR-011.md), [ST-CTR-012](../systemtest/ST-CTR-012.md), [ST-CTR-013](../systemtest/ST-CTR-013.md), [ST-CTR-014](../systemtest/ST-CTR-014.md), [ST-CTR-015](../systemtest/ST-CTR-015.md), [ST-CTR-016](../systemtest/ST-CTR-016.md). Các ca chưa chạy.

- Quyền quản trị được kiểm tra tại backend.
- Giới hạn ảnh và thứ tự dự án theo TDD-CTR-001 đã được người dùng chốt. Không bổ sung bộ lọc riêng cho dự án trong phạm vi này. Metadata phân công chưa xác định.
- Đã soạn đặc tả System Test theo nghiệp vụ được người dùng chốt; chưa chạy kiểm thử. Người dùng đã chốt TDD và đã soạn đặc tả Unit Test liên kết trong TDD; chưa triển khai mã ứng dụng hoặc chạy bộ test của tính năng.

## Out of Scope

- Ẩn/Hiển thị từng dự án, chọn Nổi bật hoặc xác minh từng dự án.
- Dùng dự án đã làm làm điều kiện lọc danh sách nhà thầu.
