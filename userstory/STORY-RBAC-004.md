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


# STORY-RBAC-004

## Metadata

- **Story**: Là người vận hành BMT, tôi muốn hệ thống kiểm tra quyền và phân công trên mọi yêu cầu và cho tra cứu nhật ký thay đổi quyền, để không ai thao tác ngoài phạm vi được giao và luôn truy được ai đã cấp quyền cho ai.
- **Context**: Các quy tắc đã chốt về thanh toán và gói dịch vụ đều yêu cầu kiểm quyền trên yêu cầu xử lý chứ không chỉ ẩn nút trên giao diện, nhưng mỗi quy tắc lại nói riêng cho tính năng của mình. Story này gom thành một cách kiểm tra chung cho mọi tính năng, làm rõ khác nhau giữa quyền xem và quyền thao tác, và bổ sung màn hình tra cứu nhật ký thay đổi quyền.
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

- Hệ thống đã có vai trò và quyền theo STORY-RBAC-001, tài khoản nhân viên theo STORY-RBAC-002 và phân công theo STORY-RBAC-003.
- Người gọi đã đăng nhập bằng tài khoản đang hoạt động. Yêu cầu chưa đăng nhập hoặc phiên không hợp lệ bị từ chối với mã 401 trước khi xét tới quyền.
- Việc tra cứu nhật ký cần quyền `audit.read`.

### Trigger

Hệ thống nhận một yêu cầu tới chức năng có yêu cầu quyền, hoặc người có quyền `audit.read` mở màn hình nhật ký thay đổi quyền.

## Flow

### Main Flow

1. Hệ thống xác định người gọi và tập quyền hiện có, bằng hợp các quyền của tất cả vai trò người đó đang giữ theo BR-RBAC-001.
2. Hệ thống xác định yêu cầu cần quyền nào. Người gọi không có quyền đó thì từ chối với mã 403 theo BR-RBAC-011.
3. Nếu yêu cầu là thao tác thay đổi trên một tài nguyên và quyền tương ứng thuộc nhóm có gắn phân công, hệ thống kiểm tra thêm người gọi đang được phân công tài nguyên đó tại thời điểm thao tác, theo BR-RBAC-010.
4. Không đạt điều kiện phân công thì từ chối với mã 403, dù người gọi có quyền.
5. Đạt cả hai điều kiện thì hệ thống thực hiện yêu cầu.

### Alternative Flow

#### ALT-01

Yêu cầu chỉ để xem dữ liệu.

1. Hệ thống kiểm tra người gọi có quyền xem tương ứng.
2. Có quyền thì cho xem toàn bộ dữ liệu trong phạm vi của quyền đó, không đòi thêm phân công theo BR-RBAC-010.
3. Quyền xem không cấp thêm quyền sửa, hủy hay khôi phục; các thao tác đó vẫn kiểm tra riêng.

#### ALT-02

Quyền của người gọi vừa được thay đổi trong lúc họ đang đăng nhập.

1. Phiên đang mở tiếp tục dùng bộ quyền trong access token đã cấp cho tới khi token hết hạn, theo BR-RBAC-009.
2. Lần phát hành token tiếp theo, gồm cả lần làm mới token, dùng bộ quyền mới nhất.
3. Người quản trị buộc đăng xuất theo STORY-RBAC-002 để thay đổi có hiệu lực ngay.

#### ALT-03

Người có quyền `audit.read` tra cứu nhật ký thay đổi quyền.

1. Người đó mở màn hình nhật ký và lọc theo người thao tác, đối tượng bị tác động hoặc khoảng thời gian.
2. Hệ thống trả các bản ghi thay đổi vai trò, quyền và phân công theo BR-RBAC-012, gồm cả các yêu cầu đã bị từ chối vì rào chắn quyền.
3. Người đó không sửa và không xóa được bản ghi nhật ký; quyền `audit.read` cũng không cho thay đổi vai trò hay phân công.

### Exception Flow

#### EXC-01

Người gọi gửi yêu cầu thẳng tới API, không qua giao diện.

1. Hệ thống vẫn kiểm quyền đầy đủ theo BR-RBAC-011 và từ chối nếu thiếu quyền.
2. Việc giao diện ẩn hoặc tắt nút không được coi là cơ chế bảo vệ.
3. Yêu cầu bị từ chối không ghi dữ liệu và không tạo bản ghi một phần.

#### EXC-02

Tài khoản đã bị khóa nhưng access token cấp trước đó chưa hết hạn.

1. Hệ thống từ chối yêu cầu vì phiên đã bị hủy khi khóa tài khoản theo BR-RBAC-008.
2. Không chờ token hết hạn mới từ chối.

#### EXC-03

Người gọi có quyền nhưng không được phân công tài nguyên đích.

1. Hệ thống từ chối thao tác thay đổi với mã 403 theo BR-RBAC-010.
2. Người gọi vẫn xem được tài nguyên đó nếu có quyền xem tương ứng.

#### EXC-04

Người gọi không có quyền `audit.read` yêu cầu xem nhật ký.

1. Hệ thống từ chối với mã 403 và không trả bản ghi nào.
2. Áp dụng cho cả danh sách và chi tiết, kể cả yêu cầu gửi trực tiếp tới API.

#### EXC-05

Người gọi yêu cầu sửa hoặc xóa một bản ghi nhật ký.

1. Hệ thống từ chối theo BR-RBAC-012, kể cả khi người gọi có quyền `audit.read` hoặc các quyền quản trị khác.
2. Bản ghi nhật ký giữ nguyên.

## Acceptance Criteria

#### AC-001

- **Given**: Nhân viên A giữ hai vai trò, một vai trò có quyền `commerce.read` và một vai trò có quyền `package.cancel`.
- **When**: Hệ thống xác định tập quyền của A.
- **Then**: A có cả `commerce.read` lẫn `package.cancel`.
- **And**: A không có quyền nào nằm ngoài hợp các quyền của hai vai trò đó.

#### AC-002

- **Given**: Nhân viên B đang hoạt động và không có quyền `package.cancel`.
- **When**: B gửi yêu cầu hủy một gói, kể cả gửi trực tiếp tới API mà không qua giao diện.
- **Then**: Hệ thống từ chối với mã 403.
- **And**: Gói đó giữ nguyên hiệu lực; không tạo bản ghi thay đổi nào.

#### AC-003

- **Given**: Nhân viên C có quyền `commerce.read` và quyền `supervision.complete`, nhưng không được phân công công trình P.
- **When**: C mở danh sách quản trị gói đã mua, rồi gửi yêu cầu hoàn thành gói giám sát của công trình P.
- **Then**: C xem được danh sách trong phạm vi quyền `commerce.read`, gồm cả gói của công trình P.
- **And**: Yêu cầu hoàn thành gói giám sát của P bị từ chối với mã 403.

#### AC-004

- **Given**: Nhân viên D đang có phiên đăng nhập với access token chưa hết hạn.
- **When**: Người quản trị khóa tài khoản của D, rồi D gửi một yêu cầu bằng token cũ.
- **Then**: Hệ thống từ chối yêu cầu đó.
- **And**: Việc từ chối xảy ra ngay, không chờ access token hết hạn.

#### AC-005

- **Given**: Nhân viên E đang có phiên đăng nhập và đang có quyền `package.restore`.
- **When**: Người quản trị bỏ quyền `package.restore` khỏi vai trò của E nhưng không buộc đăng xuất E.
- **Then**: E còn dùng được quyền đó cho tới khi access token hiện tại hết hạn.
- **And**: Sau khi token hết hạn và E nhận token mới, yêu cầu dùng quyền `package.restore` của E bị từ chối.

#### AC-006

- **Given**: Nhân viên F có quyền `audit.read` và không có quyền `role.manage`.
- **When**: F mở nhật ký thay đổi quyền và lọc theo một người thao tác trong một khoảng thời gian, rồi F gửi yêu cầu gán vai trò cho một người khác.
- **Then**: F xem được các bản ghi khớp bộ lọc, gồm cả các yêu cầu đã bị từ chối vì rào chắn quyền.
- **And**: Yêu cầu gán vai trò của F bị từ chối với mã 403.

#### AC-007

- **Given**: Nhân viên G đang hoạt động và không có quyền `audit.read`.
- **When**: G gửi yêu cầu xem danh sách hoặc chi tiết nhật ký thay đổi quyền, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối với mã 403.
- **And**: Không trả bản ghi nhật ký nào cho G.

#### AC-008

- **Given**: Một bản ghi nhật ký về việc gán vai trò đã tồn tại.
- **When**: Một người có quyền `audit.read` hoặc `role.manage` gửi yêu cầu sửa hoặc xóa bản ghi đó.
- **Then**: Hệ thống từ chối yêu cầu.
- **And**: Bản ghi nhật ký giữ nguyên nội dung và vẫn tra cứu được.

## References

### TDDs

- [TDD-RBAC-001](../tdd/TDD-RBAC-001.md): Ba chặng kiểm quyền, dấu phiên và nhật ký thay đổi quyền.

### Rules

- BR-RBAC-001
- BR-RBAC-008
- BR-RBAC-009
- BR-RBAC-010
- BR-RBAC-011
- BR-RBAC-012

### Dependencies

- STORY-RBAC-001/Main Flow: vai trò và danh sách quyền.
- STORY-RBAC-002/Alternative Flow: khóa tài khoản và buộc đăng xuất.
- STORY-RBAC-003/Main Flow: phân công tài nguyên làm căn cứ cho điều kiện sửa hẹp.
- BR-PAY-005/Then: quyền tra cứu quản trị không kéo theo quyền sửa, hủy hoặc khôi phục.

## Non-Functional

- Kiểm quyền thực hiện ở server cho mọi yêu cầu, gồm cả danh sách và chi tiết, cả yêu cầu đọc và yêu cầu thay đổi.
- Thông báo lỗi khi thiếu quyền chỉ cho biết người gọi không đủ quyền, không tiết lộ nội dung dữ liệu mà người gọi không được xem.
- Nhật ký thay đổi quyền chỉ đọc; không có đường nào sửa hoặc xóa bản ghi qua giao diện hay API.

## Out of Scope

- Nhật ký cho các nghiệp vụ ngoài vai trò, quyền và phân công. Lý do bắt buộc khi hủy hoặc khôi phục gói vẫn theo BR-SUB-024 và BR-SUB-025.
- Cảnh báo tự động khi phát hiện nhiều yêu cầu bị từ chối liên tiếp.
- Xuất nhật ký ra tệp và lưu trữ nhật ký dài hạn.
- Phân quyền theo địa chỉ mạng, theo thiết bị hoặc theo khung giờ làm việc.
