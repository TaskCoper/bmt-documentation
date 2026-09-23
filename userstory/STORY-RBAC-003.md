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


# STORY-RBAC-003

## Metadata

- **Story**: Là người quản trị có quyền phân công, tôi muốn giao khách hàng hoặc dự án cho nhân viên phụ trách và chuyển giao khi cần, để mỗi tài nguyên luôn có người chịu trách nhiệm và chỉ người đó thao tác được.
- **Context**: BR-SUB-011 và BR-SUB-012 đã chốt chỉ Admin hoặc nhân viên phụ trách dự án mới được hoàn thành hoặc mở lại gói giám sát, nhưng ghi rõ cách quản lý phân công để lại cho bước thiết kế. Story này dựng cơ chế phân công chung theo loại tài nguyên, dùng được cho khách hàng và dự án ngay bây giờ và cho lead sau này mà không phải thêm cơ chế mới.
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

- Người thao tác đã đăng nhập bằng tài khoản nhân viên đang hoạt động và có quyền `assignment.manage`.
- Nhân viên nhận phân công là tài khoản nhân viên đang hoạt động, đã kích hoạt và không bị khóa.
- Tài nguyên được phân công đã tồn tại. Đợt này có hai loại tài nguyên là khách hàng và dự án.

### Trigger

Người quản trị mở màn hình phân công để giao một tài nguyên cho nhân viên, chuyển giao sang người khác, hoặc gỡ phân công.

## Flow

### Main Flow

1. Người quản trị chọn một tài nguyên và chọn nhân viên phụ trách.
2. Hệ thống kiểm tra người nhận là tài khoản nhân viên đang hoạt động theo BR-RBAC-013; từ chối nếu là tài khoản khách hàng hoặc tài khoản đang bị khóa.
3. Hệ thống ghi phân công gồm nhân viên, loại tài nguyên, định danh tài nguyên và thời điểm bắt đầu hiệu lực.
4. Từ thời điểm đó, nhân viên thao tác được trên tài nguyên đó với các quyền có gắn phân công mà mình đang có, theo BR-RBAC-010.
5. Hệ thống ghi nhật ký người thao tác, người nhận, tài nguyên và thời điểm theo BR-RBAC-012.

### Alternative Flow

#### ALT-01

Người quản trị chuyển giao một phân công đang hiệu lực sang nhân viên khác.

1. Người quản trị chọn phân công đang hiệu lực và chọn người nhận.
2. Hệ thống kết thúc hiệu lực phân công cũ và tạo phân công mới cho người nhận tại cùng thời điểm.
3. Người bàn giao mất quyền thao tác trên tài nguyên đó kể từ thời điểm chuyển giao; người nhận có quyền thao tác từ thời điểm đó.
4. Hệ thống ghi nhật ký cả việc kết thúc phân công cũ và việc tạo phân công mới.

#### ALT-02

Người quản trị chuyển giao để gỡ vướng khi việc thu hồi vai trò bị chặn.

1. Việc thu hồi vai trò ở STORY-RBAC-002 bị từ chối vì nhân viên còn phân công đang hiệu lực theo BR-RBAC-007.
2. Người quản trị xem danh sách tài nguyên người đó đang phụ trách.
3. Người quản trị chuyển từng phần hoặc toàn bộ sang nhân viên khác theo ALT-01, hoặc gỡ phân công theo ALT-04.
4. Khi người đó không còn phân công đang hiệu lực dựa trên vai trò bị thu hồi, việc thu hồi vai trò thực hiện được.

#### ALT-03

Nhân viên được phân công ở mức khách hàng thao tác trên dự án của khách hàng đó.

1. Hệ thống xác định dự án đích thuộc khách hàng nào.
2. Nếu nhân viên đang được phân công chính khách hàng đó, coi như đã được phân công dự án đó theo BR-RBAC-013; không đòi phân công riêng từng dự án.
3. Nếu dự án đó còn có phân công riêng cho một nhân viên khác, cả hai người đều thao tác được; hệ thống không tự gỡ phân công nào.

#### ALT-04

Người quản trị gỡ một phân công mà không chuyển cho ai.

1. Hệ thống kết thúc hiệu lực phân công và ghi nhật ký.
2. Tài nguyên chuyển về danh sách chưa có người phụ trách để người quản trị chia lại.
3. Người vừa bị gỡ mất quyền thao tác trên tài nguyên đó ngay khi phân công hết hiệu lực.

#### ALT-05

Người quản trị chia lại các tài nguyên của một nhân viên đã bị khóa tài khoản.

1. Các phân công của người bị khóa vẫn còn bản ghi theo BR-RBAC-008, nên người quản trị tra được người đó đang phụ trách những gì.
2. Người quản trị chuyển từng tài nguyên sang nhân viên khác theo ALT-01.
3. Việc khóa tài khoản trước đó không bị hủy bỏ vì thao tác chia lại này.

### Exception Flow

#### EXC-01

Người quản trị phân công cho tài khoản đang bị khóa.

1. Hệ thống từ chối theo BR-RBAC-013 và không tạo bản ghi phân công.
2. Các phân công đang có của những người khác trên tài nguyên đó giữ nguyên.
3. Tài khoản bị khóa vẫn giữ các phân công cũ theo BR-RBAC-008; quy tắc này chỉ chặn phân công mới.

#### EXC-02

Người quản trị phân công cho một tài khoản khách hàng.

1. Hệ thống từ chối theo BR-RBAC-005; tài khoản khách hàng không nhận phân công.

#### EXC-03

Người quản trị chuyển giao một phân công cho chính người đang phụ trách.

1. Hệ thống từ chối và giữ nguyên phân công hiện tại.
2. Không tạo bản ghi phân công thứ hai trùng người và trùng tài nguyên.

#### EXC-04

Người gọi không có quyền `assignment.manage` yêu cầu tạo, chuyển giao hoặc gỡ phân công.

1. Hệ thống từ chối với mã 403 theo BR-RBAC-011, kể cả yêu cầu gửi trực tiếp tới API.
2. Không tạo, không đổi và không gỡ phân công nào.

## Acceptance Criteria

#### AC-001

- **Given**: Nhân viên A đang hoạt động, có quyền `supervision.complete` và chưa được phân công dự án P.
- **When**: Người quản trị phân công dự án P cho A, rồi A yêu cầu hoàn thành gói giám sát của P.
- **Then**: Hệ thống chấp nhận yêu cầu của A.
- **And**: Trước khi được phân công, cùng yêu cầu đó của A bị từ chối dù A có quyền `supervision.complete`.

#### AC-002

- **Given**: Nhân viên B có quyền `supervision.complete` nhưng không được phân công dự án P và cũng không phụ trách khách hàng sở hữu P.
- **When**: B yêu cầu hoàn thành gói giám sát của dự án P.
- **Then**: Hệ thống từ chối với mã 403.
- **And**: B vẫn xem được dữ liệu nằm trong phạm vi các quyền xem mà B có.

#### AC-003

- **Given**: Nhân viên A đang phụ trách dự án P; nhân viên C đang hoạt động và có quyền `supervision.complete`.
- **When**: Người quản trị chuyển giao phân công dự án P từ A sang C.
- **Then**: C thao tác được trên P kể từ thời điểm chuyển giao.
- **And**: A không còn thao tác được trên P kể từ thời điểm đó.

#### AC-004

- **Given**: Khách hàng K có hai dự án P1 và P2; nhân viên A được phân công phụ trách khách hàng K và có quyền `supervision.complete`.
- **When**: A yêu cầu thao tác trên gói giám sát của P1 và của P2.
- **Then**: Cả hai yêu cầu đều được chấp nhận.
- **And**: Không cần tạo thêm phân công riêng cho P1 hoặc P2.

#### AC-005

- **Given**: Nhân viên A đang phụ trách dự án P và không có người nào khác phụ trách P.
- **When**: Người quản trị gỡ phân công của A trên P mà không chuyển cho ai.
- **Then**: P nằm trong danh sách tài nguyên chưa có người phụ trách.
- **And**: A không còn thao tác được trên P.

#### AC-006

- **Given**: Nhân viên E đang bị khóa tài khoản và trước đó đã được phân công dự án Q.
- **When**: Người quản trị phân công thêm dự án P cho E.
- **Then**: Hệ thống từ chối yêu cầu và không tạo bản ghi phân công cho E trên P.
- **And**: Phân công cũ của E trên dự án Q vẫn còn hiệu lực; khóa tài khoản chỉ chặn phân công mới.

#### AC-007

- **Given**: Nhân viên A giữ vai trò "Nhân viên giám sát" và đang phụ trách hai dự án dựa trên vai trò đó; việc thu hồi vai trò đã bị từ chối.
- **When**: Người quản trị chuyển cả hai dự án sang nhân viên khác, rồi thu hồi lại vai trò của A.
- **Then**: Việc thu hồi vai trò thực hiện được.
- **And**: Hai dự án đó có người phụ trách mới; không có dự án nào rơi vào trạng thái chưa có người phụ trách.

#### AC-008

- **Given**: Người quản trị có quyền `assignment.manage`.
- **When**: Người quản trị tạo, chuyển giao hoặc gỡ một phân công.
- **Then**: Mỗi thao tác tạo ra một bản ghi nhật ký có người thao tác, nhân viên liên quan, tài nguyên, thời điểm và nội dung thay đổi.
- **And**: Chuyển giao tạo bản ghi cho cả việc kết thúc phân công cũ và việc tạo phân công mới.

## References

### TDDs

- [TDD-RBAC-003](../tdd/TDD-RBAC-003.md): Phân công tài nguyên, chuyển giao và điều kiện sửa hẹp.

### Rules

- BR-RBAC-005
- BR-RBAC-007
- BR-RBAC-008
- BR-RBAC-010
- BR-RBAC-011
- BR-RBAC-012
- BR-RBAC-013

### Dependencies

- STORY-RBAC-002/Exception Flow: thu hồi vai trò bị chặn khi còn phân công đang hiệu lực.
- BR-SUB-011/Then: quyền hoàn thành gói giám sát dựa trên phân công hiện tại của dự án.
- BR-SUB-012/Then: quyền mở lại gói giám sát dựa trên phân công hiện tại của dự án.

## Non-Functional

- Việc kiểm tra một người có đang được phân công một tài nguyên hay không phải cho kết quả theo đúng thời điểm thao tác, không dùng dữ liệu phân công đã hết hiệu lực.
- Bản ghi phân công đã kết thúc hiệu lực vẫn phải tra cứu được để biết ai từng phụ trách tài nguyên nào.

## Out of Scope

- Tính năng chia lead, gồm cả chia thủ công và chia tự động. Story này chỉ bảo đảm cơ chế phân công dùng lại được cho loại tài nguyên mới.
- Tự động chia lại tài nguyên khi nhân viên bị khóa hoặc nghỉ việc.
- Quy tắc cân bằng số lượng tài nguyên giữa các nhân viên.
- Định nghĩa schema của module khách hàng và module dự án. Phân công chỉ tham chiếu tới chúng theo định danh.
