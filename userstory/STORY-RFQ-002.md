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

# STORY-RFQ-002

## Metadata

- **Story**: Là admin hoặc người có quyền, tôi muốn quản lý lời mời báo giá, xem hồ sơ đã gửi và cập nhật lịch, trạng thái để liên hệ nhà thầu và khách.
- **Context**: Bốn trạng thái do người quản lý tự điều chỉnh, không bị ràng buộc vào trình tự hay bằng chứng khảo sát. Bản hồ sơ đã gửi và lịch khách đề nghị ban đầu được giữ nguyên. Đây là bản nháp tổng hợp các quyết định trong hội thoại ngày 03/10/2026; người dùng đã chốt cả bộ US/BR và giao triển khai. Đã soạn đặc tả System Test; chưa chạy các ca. Reviewer và Approver đã được xác nhận; các metadata phân công còn thiếu.
- **Sprint**:
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

- Người thao tác là admin hoặc đã được cấp quyền quản lý lời mời.
- Có lời mời được khách gửi thành công.

### Trigger

Người quản lý mở danh sách và chi tiết lời mời báo giá.

## Flow

### Main Flow

1. Người quản lý xem danh sách lời mời và mở một yêu cầu.
2. Hệ thống kiểm quyền và hiển thị khách, nhà thầu, số điện thoại, ghi chú khảo sát, lịch đề nghị ban đầu, lịch hẹn hiện tại, trạng thái và ghi chú nội bộ.
3. Người quản lý xem bản hồ sơ và các tệp được giữ tại thời điểm gửi; liên hệ khách và nhà thầu để sắp lịch.
4. Người quản lý có thể đổi ngày giờ gặp, chọn một trong bốn trạng thái và sửa ghi chú nội bộ.
5. Hệ thống lưu thay đổi, giữ nguyên lịch đề nghị ban đầu và bản hồ sơ đã gửi. Khách thấy lịch và trạng thái mới nhất; gửi email nếu lịch hoặc trạng thái thực sự thay đổi.

### Alternative Flow

#### ALT-01

Người quản lý muốn chuyển ngược hoặc bỏ qua trạng thái trung gian.

1. Cho chuyển trực tiếp giữa bốn trạng thái, kể cả từ Hoàn tất về Đã gửi; không yêu cầu chứng từ hoặc báo giá.

#### ALT-02

Chỉ sửa ghi chú nội bộ.

1. Lưu ghi chú cho người quản lý; không cho khách xem và không gửi email vì thay đổi ghi chú.

#### ALT-03

Nhà thầu của yêu cầu đã bị ẩn.

1. Vẫn cho người có quyền xem và xử lý lời mời cũ.

#### ALT-04

Admin muốn gỡ nhà thầu đã có lời mời khỏi danh sách công khai.

1. Cho ẩn nhà thầu; giữ các yêu cầu cũ và không hoàn lại lượt của hồ sơ.

### Exception Flow

#### EXC-01

Người gọi không có quyền quản lý lời mời.

1. Từ chối xem dữ liệu quản trị hoặc cập nhật yêu cầu.

#### EXC-02

Yêu cầu cập nhật dùng trạng thái ngoài bốn giá trị đã chốt.

1. Từ chối thay đổi; giữ trạng thái đã lưu.

#### EXC-03

Admin yêu cầu xóa nhà thầu đã có lời mời.

1. Từ chối xóa theo BR-RFQ-006; vẫn cho ẩn nhà thầu.

#### EXC-04

Gửi email thông báo thất bại sau khi đã cập nhật lịch hoặc trạng thái thành công.

1. Giữ lịch và trạng thái mới đã lưu; khách vẫn xem được các giá trị này.
2. Hệ thống tự thử gửi lại email theo BR-RFQ-005; không yêu cầu người quản lý lưu lại thay đổi.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản lý có quyền và lời mời đã lưu hồ sơ cùng tệp.
- **When**: Mở chi tiết sau khi khách thay đổi hồ sơ gốc.
- **Then**: Xem đúng hồ sơ và tệp tại thời điểm gửi.
- **And**: Quyền quản lý lời mời không cho sửa hồ sơ gốc hoặc bản đã gửi.

#### AC-002

- **Given**: Lời mời đang Đã gửi.
- **When**: Người có quyền chuyển trực tiếp sang Hoàn tất rồi sang Nhà thầu đã nhận.
- **Then**: Cả hai chuyển trạng thái đều được chấp nhận.
- **And**: Không yêu cầu đi lần lượt hoặc nộp bằng chứng; gửi email cho khách về trạng thái mới.

#### AC-003

- **Given**: Lịch đề nghị ban đầu là D1/T1.
- **When**: Người có quyền đổi cả ngày và giờ thành D2/T2.
- **Then**: Lịch hiện tại là D2/T2 và khách xem được.
- **And**: Lịch đề nghị ban đầu D1/T1 được giữ; khách nhận email về lịch mới.

#### AC-004

- **Given**: Người có quyền sửa ghi chú nội bộ.
- **When**: Lưu ghi chú rồi khách mở chi tiết yêu cầu.
- **Then**: Người quản lý thấy ghi chú; dữ liệu trả cho khách không chứa ghi chú.
- **And**: Không tạo email chỉ vì thay đổi ghi chú; mọi email khác cũng không chứa nội dung này.

#### AC-005

- **Given**: Nhà thầu đã có ít nhất một lời mời, kể cả đã Hoàn tất.
- **When**: Admin yêu cầu xóa rồi chọn ẩn nhà thầu.
- **Then**: Xóa bị từ chối; ẩn được phép theo quyền quản lý nhà thầu hiện có.
- **And**: Lời mời vẫn xem, sửa lịch và đổi trạng thái được; không hoàn lại lượt.

#### AC-006

- **Given**: Khách hoặc nhân viên không có quyền quản lý biết mã lời mời.
- **When**: Gọi thao tác đổi lịch, trạng thái hoặc ghi chú nội bộ.
- **Then**: Từ chối thao tác.
- **And**: Không đổi dữ liệu và không gửi email thông báo thành công.

#### AC-007

- **Given**: Lời mời đã Hoàn tất.
- **When**: Người có quyền sửa lịch hoặc ghi chú nội bộ.
- **Then**: Vẫn được sửa theo BR-RFQ-004.
- **And**: Không thay đổi hồ sơ, nhà thầu, số điện thoại và ghi chú khảo sát do khách đã gửi.

#### AC-008

- **Given**: Người có quyền quản lý đang sửa một lời mời.
- **When**: Trong cùng một lần lưu, người đó thay cả lịch hẹn và trạng thái.
- **Then**: Hệ thống gửi một email tổng hợp đến email tài khoản khách, thể hiện cả lịch và trạng thái mới đã lưu.
- **And**: Không tạo hai email riêng cho hai trường thay đổi và không đưa ghi chú nội bộ vào email.

#### AC-009

- **Given**: Lịch hoặc trạng thái mới đã được lưu thành công.
- **When**: Gửi email thông báo thất bại.
- **Then**: Giữ nguyên thay đổi đã lưu và khách vẫn xem được lịch, trạng thái mới nhất.
- **And**: Hệ thống tự thử gửi lại email, không yêu cầu người quản lý thực hiện lại thao tác cập nhật.

## References

### TDDs

- TDD-RFQ-001

### Rules

- BR-RFQ-003
- BR-RFQ-004
- BR-RFQ-005
- BR-RFQ-006

### Dependencies

- STORY-RFQ-001: Khách gửi và theo dõi lời mời.

## Non-Functional

- Đặc tả kiểm thử: [ST-RFQ-013](../systemtest/ST-RFQ-013.md), [ST-RFQ-014](../systemtest/ST-RFQ-014.md), [ST-RFQ-015](../systemtest/ST-RFQ-015.md), [ST-RFQ-016](../systemtest/ST-RFQ-016.md), [ST-RFQ-017](../systemtest/ST-RFQ-017.md), [ST-RFQ-018](../systemtest/ST-RFQ-018.md), [ST-RFQ-019](../systemtest/ST-RFQ-019.md), [ST-RFQ-020](../systemtest/ST-RFQ-020.md), [ST-RFQ-021](../systemtest/ST-RFQ-021.md), [ST-RFQ-031](../systemtest/ST-RFQ-031.md), [ST-RFQ-033](../systemtest/ST-RFQ-033.md), [ST-RFQ-034](../systemtest/ST-RFQ-034.md). Chưa chạy; đây không phải kết quả Pass.

- Quyền quản lý và bảo vệ ghi chú nội bộ phải kiểm tại backend; không chỉ ẩn nút hoặc trường trên giao diện.
- Tệp trong bản lưu không được công khai; quyền xem lời mời không tự mở quyền đọc mọi hồ sơ hoặc dự toán của khách.
- Chưa chốt bộ lọc, phân trang, thứ tự danh sách hoặc chỉ tiêu hiệu năng; sẽ xác định khi thiết kế.
- Gộp email khi một lần lưu đổi cả lịch và trạng thái; gửi lỗi thì giữ thay đổi và tự thử lại theo BR-RFQ-005 đã được xác nhận trong hội thoại.

## Out of Scope

- Tài khoản nhà thầu, lịch rảnh nhà thầu, tự liên hệ hoặc tự xác nhận lịch.
- Tự thêm trạng thái hủy hoặc từ chối; bắt buộc bằng chứng để đổi trạng thái.
- Quyền quản lý lời mời không tự cấp quyền xóa nhà thầu, sửa hồ sơ khách hoặc sửa cấu hình giới hạn.
