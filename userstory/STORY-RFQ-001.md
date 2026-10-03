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

# STORY-RFQ-001

## Metadata

- **Story**: Là khách hàng, tôi muốn gửi lời mời khảo sát từ hồ sơ của mình đến nhà thầu và theo dõi tiến độ để được admin hỗ trợ sắp lịch nhận báo giá.
- **Context**: Nhà thầu không đăng nhập. Admin liên hệ nhà thầu và khách bên ngoài hệ thống. Hồ sơ trong Story này là hồ sơ công trình của khách; không bắt buộc đã hoàn tất thiết kế hoặc dự toán. Đây là bản nháp tổng hợp các quyết định trong hội thoại ngày 03/10/2026; người dùng đã chốt cả bộ US/BR và giao triển khai. Đã soạn đặc tả System Test; chưa chạy các ca. Reviewer và Approver đã được xác nhận; các metadata phân công còn thiếu.
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

- Khách đăng nhập bằng tài khoản của mình; hồ sơ thuộc khách và còn tồn tại.
- Nhà thầu được chọn còn tồn tại và đang hiển thị.

### Trigger

Khách chọn nhà thầu và mở chức năng mời báo giá từ một hồ sơ.

## Flow

### Main Flow

1. Khách chọn hồ sơ của mình và nhà thầu đang hiển thị.
2. Khách chọn ngày và khung giờ mong muốn; kiểm tra số điện thoại được điền từ tài khoản hoặc nhập số khác. Khách có thể nhập ghi chú khảo sát hoặc để trống.
3. Khách gửi. Hệ thống kiểm tra quyền sở hữu, nhà thầu, dữ liệu bắt buộc, điều kiện không gửi lại và giới hạn hiện tại theo BR-RFQ-001 và BR-RFQ-002.
4. Hệ thống tiếp nhận một lời mời ở trạng thái Đã gửi, giữ thông tin và tệp đính kèm của hồ sơ tại thời điểm gửi theo BR-RFQ-003; gửi email tiếp nhận theo BR-RFQ-005.
5. Khách mở danh sách lời mời của hồ sơ để xem nhà thầu, thông tin đã gửi, trạng thái và lịch hẹn mới nhất. Khách không có thao tác sửa hoặc hủy.
6. Khi người quản lý đổi lịch hoặc trạng thái, khách thấy dữ liệu mới nhất và nhận email tương ứng. Ghi chú nội bộ không hiển thị cho khách.

### Alternative Flow

#### ALT-01

Nhà thầu đang hiển thị nhưng ngừng nhận dự án.

1. Tiếp nhận nếu các điều kiện còn lại hợp lệ; admin liên hệ xác nhận với nhà thầu.

#### ALT-02

Khách sửa hồ sơ hoặc gỡ tệp sau khi đã gửi.

1. Thao tác với hồ sơ gốc theo quyền sửa hiện có; bản lưu của lời mời vẫn giữ nguyên cả thông tin và tệp.
2. Lời mời mới tới nhà thầu khác lưu bản hồ sơ tại thời điểm mới.

#### ALT-03

Nhà thầu bị ẩn sau khi nhận lời mời.

1. Khách vẫn xem được lời mời và tiến độ. Lời mời vẫn tính vào giới hạn của hồ sơ.

### Exception Flow

#### EXC-01

Hồ sơ không thuộc khách hoặc không còn tồn tại.

1. Từ chối gửi hoặc đọc lời mời qua hồ sơ đó; không tạo lời mời hay tiết lộ dữ liệu của khách khác.

#### EXC-02

Nhà thầu không tồn tại hoặc đã bị ẩn trước khi tiếp nhận.

1. Từ chối tạo lời mời; không trừ thêm lượt.

#### EXC-03

Hồ sơ đã mời nhà thầu này hoặc đã đạt/vượt giới hạn hiện hành.

1. Từ chối tạo thêm lời mời theo BR-RFQ-002; giữ nguyên các lời mời cũ.

#### EXC-04

Thiếu ngày giờ hoặc số điện thoại bắt buộc.

1. Thông báo thông tin cần bổ sung; không tạo lời mời hoặc email tiếp nhận thành công.

#### EXC-05

Khách yêu cầu sửa, hủy lời mời hoặc xóa hồ sơ đã có lời mời.

1. Từ chối thao tác; giữ nguyên lời mời và hồ sơ.

#### EXC-06

Gửi email tiếp nhận thất bại sau khi lời mời đã được lưu thành công.

1. Giữ lời mời, bản hồ sơ đã gửi và kết quả tiếp nhận thành công.
2. Hệ thống tự thử gửi lại email theo BR-RFQ-005; khách không phải gửi lại lời mời.

## Acceptance Criteria

#### AC-001

- **Given**: Khách có hồ sơ của mình, nhà thầu đang hiển thị và còn lượt.
- **When**: Khách gửi ngày giờ, số liên lạc hợp lệ và để trống ghi chú.
- **Then**: Tạo đúng một lời mời ở trạng thái Đã gửi, gắn đúng khách, hồ sơ và nhà thầu.
- **And**: Có bản hồ sơ cùng các tệp tại lúc gửi và email tiếp nhận; không yêu cầu hoàn tất thiết kế hoặc dự toán.

#### AC-002

- **Given**: Số điện thoại trong tài khoản là P1.
- **When**: Khách thay số thành P2 trước khi gửi.
- **Then**: Lời mời lưu P2.
- **And**: Số trong tài khoản vẫn là P1.

#### AC-003

- **Given**: Giới hạn là 3; hồ sơ đã mời A, B và C.
- **When**: Khách gửi tới D.
- **Then**: Từ chối tạo lời mời thứ tư.
- **And**: Giữ nguyên ba lời mời cũ.

#### AC-004

- **Given**: Hồ sơ đã mời A và lời mời đã Hoàn tất; hồ sơ còn lượt.
- **When**: Khách gửi lại cho A.
- **Then**: Không tạo lời mời thứ hai cho A.
- **And**: Kết quả không thay đổi nếu A đã bị ẩn rồi hiện lại.

#### AC-005

- **Given**: Giới hạn 3; hồ sơ đã mời hai nhà thầu.
- **When**: Hai yêu cầu gửi tới hai nhà thầu khác đến đồng thời.
- **Then**: Tối đa một lời mời mới được tiếp nhận.
- **And**: Tổng số nhà thầu được mời không vượt 3; gửi đồng thời cùng một nhà thầu cũng không tạo hai lời mời.

#### AC-006

- **Given**: Nhà thầu đang hiển thị nhưng ngừng nhận dự án.
- **When**: Khách gửi lời mời hợp lệ.
- **Then**: Yêu cầu vẫn được tiếp nhận.
- **And**: Nhà thầu đã bị ẩn thì không nhận lời mời mới.

#### AC-007

- **Given**: Lời mời đã lưu hồ sơ có bản vẽ F và ngân sách B1.
- **When**: Khách sửa hồ sơ thành B2 và gỡ F khi quyền sửa hiện có cho phép.
- **Then**: Bản đã gửi vẫn có ngân sách B1 và xem được bản vẽ F.
- **And**: Gửi tới một nhà thầu mới sau đó dùng hồ sơ tại thời điểm mới.

#### AC-008

- **Given**: Khách có lời mời; người quản lý đã đổi lịch hoặc trạng thái.
- **When**: Khách xem lại lời mời của hồ sơ.
- **Then**: Thấy trạng thái và lịch hẹn mới nhất; nhận email cho thay đổi tương ứng.
- **And**: Không thấy ghi chú nội bộ và không được sửa hoặc hủy lời mời.

#### AC-009

- **Given**: Hồ sơ có ít nhất một lời mời, kể cả tất cả đã Hoàn tất.
- **When**: Khách yêu cầu xóa hồ sơ.
- **Then**: Từ chối xóa.
- **And**: Hồ sơ và các lời mời vẫn xem được; việc có lời mời không tự khóa sửa hồ sơ.

#### AC-010

- **Given**: Lời mời thuộc hồ sơ của khách A.
- **When**: Khách B gọi thao tác xem hoặc gửi bằng mã hồ sơ của A.
- **Then**: Từ chối và không trả dữ liệu riêng của A.
- **And**: Không tạo lời mời hoặc thay đổi lượt của hồ sơ A.

#### AC-011

- **Given**: Nhà thầu A đã nhận lời mời rồi bị ẩn.
- **When**: Khách xem tiến độ lời mời cũ.
- **Then**: Vẫn xem được yêu cầu và trạng thái hiện tại.
- **And**: Lời mời cho A vẫn tính vào giới hạn.

#### AC-012

- **Given**: Lời mời đã được tiếp nhận thành công và đã lưu bản hồ sơ cùng tệp.
- **When**: Gửi email tiếp nhận đến email tài khoản khách thất bại.
- **Then**: Lời mời vẫn tồn tại và khách vẫn xem được; không báo lần gửi lời mời là thất bại vì lỗi email.
- **And**: Hệ thống tự thử gửi lại email, không tạo lời mời mới hoặc tính thêm lượt và không yêu cầu khách gửi lại.

## References

### TDDs

- TDD-RFQ-001

### Rules

- BR-RFQ-001
- BR-RFQ-002
- BR-RFQ-003
- BR-RFQ-004
- BR-RFQ-005
- BR-RFQ-006

### Dependencies

- STORY-RFQ-002: Quản trị xử lý lời mời và lịch hẹn.
- STORY-RFQ-003: Thay đổi giới hạn dùng chung.

## Non-Functional

- Đặc tả kiểm thử: [ST-RFQ-001](../systemtest/ST-RFQ-001.md), [ST-RFQ-002](../systemtest/ST-RFQ-002.md), [ST-RFQ-003](../systemtest/ST-RFQ-003.md), [ST-RFQ-004](../systemtest/ST-RFQ-004.md), [ST-RFQ-005](../systemtest/ST-RFQ-005.md), [ST-RFQ-006](../systemtest/ST-RFQ-006.md), [ST-RFQ-007](../systemtest/ST-RFQ-007.md), [ST-RFQ-008](../systemtest/ST-RFQ-008.md), [ST-RFQ-009](../systemtest/ST-RFQ-009.md), [ST-RFQ-010](../systemtest/ST-RFQ-010.md), [ST-RFQ-011](../systemtest/ST-RFQ-011.md), [ST-RFQ-012](../systemtest/ST-RFQ-012.md), [ST-RFQ-029](../systemtest/ST-RFQ-029.md), [ST-RFQ-030](../systemtest/ST-RFQ-030.md), [ST-RFQ-032](../systemtest/ST-RFQ-032.md), [ST-RFQ-035](../systemtest/ST-RFQ-035.md), [ST-RFQ-036](../systemtest/ST-RFQ-036.md). Chưa chạy; đây không phải kết quả Pass.

- Quyền sở hữu, điều kiện không trùng và giới hạn phải được kiểm tại backend, kể cả khi gọi API trực tiếp hoặc gửi đồng thời.
- Bản hồ sơ và tệp đã gửi không được thay đổi theo hồ sơ gốc. Chưa chốt chỉ tiêu hiệu năng định lượng.
- Email dùng địa chỉ trong tài khoản khách; lỗi gửi thư không làm mất lời mời và hệ thống tự thử lại theo BR-RFQ-005 đã được xác nhận trong hội thoại.

## Out of Scope

- Khách sửa hoặc hủy lời mời; đăng nhập dành cho nhà thầu.
- Tự gửi lời mời cho nhà thầu qua email/SMS hoặc tự xác nhận nhà thầu đã đồng ý.
- Chưa có yêu cầu quản lý số tiền, file báo giá do nhà thầu trả, hợp đồng hoặc thanh toán trong Story này.
