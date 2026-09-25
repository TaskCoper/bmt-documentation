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


# STORY-RBAC-002

## Metadata

- **Story**: Là người quản trị có quyền quản lý người dùng, tôi muốn tạo tài khoản nhân viên, gán vai trò, khóa tài khoản và buộc đăng xuất để kiểm soát ai đang làm được việc gì trong hệ thống.
- **Context**: Hiện không có đường nào tạo tài khoản nhân viên: đăng ký công khai luôn gán vai trò Khách hàng, và muốn có Admin phải sửa dữ liệu thủ công. Story này bổ sung luồng người quản trị tạo tài khoản nhân viên kèm mật khẩu do hệ thống sinh, gán và thu hồi vai trò, khóa và mở khóa tài khoản, cùng chức năng buộc đăng xuất để thay đổi quyền có hiệu lực ngay khi cần. Người quản trị chuyển mật khẩu cho nhân viên bằng kênh riêng ngoài hệ thống; nhân viên phải đổi mật khẩu ở lần đăng nhập đầu.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác đã đăng nhập bằng tài khoản nhân viên đang hoạt động. Tạo tài khoản, khóa, mở khóa và buộc đăng xuất cần quyền `user.manage`; gán và thu hồi vai trò cần quyền `role.manage`.
- Các vai trò định gán đã tồn tại, tạo theo STORY-RBAC-001.
- Người quản trị có sẵn một kênh riêng ngoài hệ thống để chuyển mật khẩu cho nhân viên.

### Trigger

Người quản trị mở màn hình quản lý tài khoản nhân viên để tạo tài khoản mới, thay đổi vai trò, khóa hoặc mở khóa một tài khoản.

## Flow

### Main Flow

1. Người quản trị nhập địa chỉ email của nhân viên mới và chọn các vai trò nhân viên người đó sẽ giữ.
2. Hệ thống kiểm tra email chưa thuộc tài khoản nào, và các vai trò được chọn đều là vai trò nhân viên theo BR-RBAC-005.
3. Hệ thống kiểm tra người quản trị đang có tất cả các quyền nằm trong những vai trò định gán, theo BR-RBAC-004.
4. Hệ thống sinh một mật khẩu ngẫu nhiên đủ mạnh, tạo tài khoản ở trạng thái đang hoạt động kèm dấu hiệu phải đổi mật khẩu, và ghi nhận các vai trò đã chọn theo BR-RBAC-006.
5. Hệ thống hiển thị mật khẩu vừa sinh đúng một lần cho người quản trị. Người quản trị chuyển mật khẩu cho nhân viên bằng kênh riêng ngoài hệ thống.
6. Nhân viên đăng nhập bằng mật khẩu đó. Hệ thống chỉ cho đổi mật khẩu và từ chối mọi yêu cầu khác cho tới khi đổi xong.
7. Nhân viên đổi mật khẩu thành công. Hệ thống bỏ dấu hiệu phải đổi mật khẩu, hủy các phiên đang mở, và nhân viên đăng nhập lại bằng mật khẩu mới để bắt đầu dùng quyền của mình.
8. Hệ thống ghi nhật ký việc tạo tài khoản, danh sách vai trò đã gán và thời điểm nhân viên đổi mật khẩu lần đầu theo BR-RBAC-012.

### Alternative Flow

#### ALT-01

Người quản trị gán thêm vai trò cho một nhân viên đang làm việc.

1. Hệ thống kiểm tra vai trò được chọn là vai trò nhân viên và kiểm tra rào chắn cấp quyền theo BR-RBAC-004.
2. Hệ thống lưu thay đổi và ghi nhật ký. Quyền của nhân viên là hợp các quyền của tất cả vai trò đang giữ theo BR-RBAC-001.
3. Quyền mới có hiệu lực chậm nhất khi access token của người đó hết hạn theo BR-RBAC-009.

#### ALT-02

Người quản trị thu hồi một vai trò mà sau khi thu hồi, nhân viên vẫn làm được việc trên các gói giám sát đang phụ trách.

1. Hệ thống kiểm tra theo BR-RBAC-007: sau khi thu hồi, nhân viên không còn phụ trách gói nào, hoặc vẫn còn quyền `supervision.complete` từ vai trò khác.
2. Hệ thống thu hồi vai trò và ghi nhật ký.
3. Các vai trò còn lại và các phân công gói giữ nguyên.

#### ALT-03

Người quản trị khóa một tài khoản nhân viên.

1. Hệ thống khóa tài khoản ngay, không kiểm tra người đó còn phân công hay không, theo BR-RBAC-008.
2. Hệ thống hủy toàn bộ phiên đăng nhập của tài khoản đó; các yêu cầu tiếp theo bị từ chối dù access token cấp trước đó chưa hết hạn.
3. Các vai trò và bản ghi phân công của người đó giữ nguyên; người bị khóa không thao tác được trên chúng. Các gói đang gán mà người đó phụ trách hiện trong danh sách cần chia lại theo BR-RBAC-013.
4. Hệ thống ghi nhật ký việc khóa.

#### ALT-04

Người quản trị mở khóa một tài khoản đã bị khóa.

1. Hệ thống mở khóa và ghi nhật ký.
2. Tài khoản dùng lại đúng các vai trò và phân công như trước khi khóa; không tự cấp thêm hoặc bỏ bớt vai trò nào.
3. Nhân viên phải đăng nhập lại để nhận phiên mới.

#### ALT-05

Người quản trị buộc đăng xuất một tài khoản mà không khóa tài khoản đó.

1. Dùng khi vừa thay đổi vai trò hoặc quyền và cần có hiệu lực ngay.
2. Hệ thống hủy phiên đăng nhập của tài khoản đó và ghi nhật ký.
3. Lần đăng nhập tiếp theo, người đó nhận bộ quyền mới nhất. Tài khoản vẫn hoạt động bình thường.

#### ALT-06

Nhân viên làm mất mật khẩu được giao trước khi kịp đổi.

1. Hệ thống không cho xem lại mật khẩu đã sinh, kể cả với người quản trị đã tạo tài khoản.
2. Nhân viên dùng luồng quên mật khẩu sẵn có để nhận mã qua email đã đăng ký và đặt mật khẩu mới.
3. Sau khi đặt mật khẩu mới thành công, hệ thống bỏ dấu hiệu phải đổi mật khẩu; nhân viên không phải đổi thêm một lần nữa.

### Exception Flow

#### EXC-01

Người quản trị tạo tài khoản nhân viên cho một địa chỉ email đã thuộc tài khoản khách hàng.

1. Hệ thống từ chối theo BR-RBAC-005 và cho biết email đã được dùng.
2. Không nâng tài khoản khách hàng đó thành tài khoản nhân viên và không tạo tài khoản thứ hai cùng email.

#### EXC-02

Người quản trị thu hồi một vai trò khiến nhân viên mất quyền `supervision.complete` trong khi vẫn đang phụ trách gói giám sát.

1. Hệ thống từ chối theo BR-RBAC-007, cho biết số gói người đó đang phụ trách.
2. Hệ thống hướng người quản trị sang luồng chuyển giao ở STORY-RBAC-003; không tự chuyển giao hoặc tự gỡ phân công.
3. Nếu cần cắt quyền gấp, người quản trị khóa tài khoản theo ALT-03; thao tác đó không đòi chuyển giao.

#### EXC-03

Thao tác làm hệ thống không còn tài khoản Admin nào đang hoạt động.

1. Hệ thống từ chối thu hồi vai trò Admin cuối cùng và từ chối khóa tài khoản Admin cuối cùng, theo BR-RBAC-004.
2. Giữ nguyên vai trò và trạng thái tài khoản; ghi nhật ký yêu cầu bị từ chối.

#### EXC-04

Nhân viên chưa đổi mật khẩu lần đầu nhưng gửi yêu cầu tới một chức năng khác.

1. Hệ thống từ chối yêu cầu theo BR-RBAC-006, kể cả khi tài khoản đã được gán vai trò có quyền và kể cả yêu cầu gửi trực tiếp tới API.
2. Chỉ yêu cầu đổi mật khẩu được chấp nhận trong lúc tài khoản còn dấu hiệu phải đổi mật khẩu.

#### EXC-05

Người quản trị tự gán thêm vai trò cho chính mình.

1. Hệ thống từ chối theo BR-RBAC-004.
2. Vai trò của người thao tác giữ nguyên; ghi nhật ký yêu cầu bị từ chối.

#### EXC-06

Người gọi thiếu quyền `user.manage` hoặc `role.manage` cho thao tác đang yêu cầu.

1. Hệ thống từ chối với mã 403 theo BR-RBAC-011, kể cả yêu cầu gửi trực tiếp tới API.
2. Không tạo tài khoản, không đổi vai trò và không đổi trạng thái tài khoản nào.

#### EXC-07

Người quản trị yêu cầu khóa tài khoản của chính mình.

1. Hệ thống từ chối theo BR-RBAC-008, kể cả yêu cầu gửi trực tiếp tới API.
2. Tài khoản giữ nguyên trạng thái đang hoạt động; ghi nhật ký yêu cầu bị từ chối.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản trị có quyền `user.manage`, `role.manage` và `commerce.read`; địa chỉ email chưa thuộc tài khoản nào.
- **When**: Người quản trị tạo tài khoản cho địa chỉ đó với vai trò "Nhân viên tra cứu thanh toán" gồm quyền `commerce.read`.
- **Then**: Hệ thống tạo tài khoản đang hoạt động, sinh một mật khẩu ngẫu nhiên và hiển thị mật khẩu đó đúng một lần trong phản hồi.
- **And**: Hệ thống chỉ lưu bản băm của mật khẩu; không có màn hình hay API nào xem lại được mật khẩu đã sinh.

#### AC-002

- **Given**: Một tài khoản nhân viên vừa được tạo, đã được gán vai trò có quyền `commerce.read` và chưa đổi mật khẩu lần đầu.
- **When**: Nhân viên đăng nhập bằng mật khẩu được giao, gọi một chức năng cần quyền `commerce.read`, rồi đổi mật khẩu thành công và đăng nhập lại.
- **Then**: Đăng nhập lần đầu thành công nhưng yêu cầu dùng quyền `commerce.read` bị từ chối vì chưa đổi mật khẩu.
- **And**: Sau khi đổi mật khẩu và đăng nhập lại, nhân viên dùng được quyền `commerce.read`; các phiên mở bằng mật khẩu cũ đã bị hủy.

#### AC-003

- **Given**: Địa chỉ email đang thuộc một tài khoản khách hàng.
- **When**: Người quản trị tạo tài khoản nhân viên cho địa chỉ đó.
- **Then**: Hệ thống từ chối và cho biết email đã được dùng.
- **And**: Tài khoản khách hàng đó giữ nguyên vai trò Khách hàng và không nhận vai trò nhân viên nào.

#### AC-004

- **Given**: Nhân viên A chỉ có quyền `supervision.complete` từ vai trò "Nhân viên giám sát" và đang phụ trách 2 gói giám sát.
- **When**: Người quản trị thu hồi vai trò "Nhân viên giám sát" khỏi A.
- **Then**: Hệ thống từ chối và cho biết A còn 2 gói đang phụ trách.
- **And**: A vẫn giữ vai trò đó và cả 2 phân công vẫn còn hiệu lực.

#### AC-005

- **Given**: Nhân viên A đang phụ trách 2 gói giám sát ở trạng thái đã gán và đang có phiên đăng nhập với access token chưa hết hạn.
- **When**: Người quản trị khóa tài khoản của A.
- **Then**: Tài khoản bị khóa ngay dù A còn phân công; mọi yêu cầu tiếp theo của A bị từ chối dù token chưa hết hạn.
- **And**: Hai bản ghi phân công của A vẫn còn, và hai gói đó hiện trong danh sách cần chia lại.

#### AC-006

- **Given**: Hệ thống chỉ còn đúng một tài khoản Admin đang hoạt động.
- **When**: Người quản trị yêu cầu thu hồi vai trò Admin của tài khoản đó, hoặc yêu cầu khóa tài khoản đó.
- **Then**: Hệ thống từ chối cả hai yêu cầu.
- **And**: Tài khoản Admin đó giữ nguyên vai trò và vẫn đang hoạt động.

#### AC-007

- **Given**: Nhân viên A vừa bị bỏ quyền `package.cancel` và vẫn đang có phiên đăng nhập.
- **When**: Người quản trị buộc đăng xuất tài khoản của A.
- **Then**: Phiên đăng nhập của A bị hủy; A phải đăng nhập lại.
- **And**: Sau khi đăng nhập lại, A không còn quyền `package.cancel`; tài khoản của A vẫn đang hoạt động, không bị khóa.

#### AC-008

- **Given**: Người quản trị có quyền `role.manage` và đang giữ một vai trò nào đó.
- **When**: Người quản trị tự gán thêm cho chính mình một vai trò khác.
- **Then**: Hệ thống từ chối yêu cầu.
- **And**: Danh sách vai trò của người quản trị đó không thay đổi.

#### AC-009

- **Given**: Một tài khoản nhân viên đang hoạt động nhưng không có quyền `user.manage`.
- **When**: Tài khoản đó gửi yêu cầu tạo tài khoản nhân viên mới hoặc khóa một tài khoản, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối với mã 403.
- **And**: Không tạo tài khoản nào và không đổi trạng thái tài khoản nào.

#### AC-010

- **Given**: Người quản trị có đủ quyền cho các thao tác quản lý tài khoản.
- **When**: Người quản trị tạo tài khoản, gán vai trò, thu hồi vai trò, khóa, mở khóa hoặc buộc đăng xuất một tài khoản.
- **Then**: Mỗi thao tác tạo ra một bản ghi nhật ký có người thao tác, loại thao tác, tài khoản bị tác động, thời điểm và nội dung thay đổi.
- **And**: Các yêu cầu bị từ chối vì rào chắn quyền cũng được ghi nhật ký.

#### AC-011

- **Given**: Nhân viên B có quyền `supervision.complete` từ cả vai trò "Nhân viên giám sát" lẫn vai trò "Trưởng nhóm giám sát", và đang phụ trách 2 gói giám sát.
- **When**: Người quản trị thu hồi vai trò "Nhân viên giám sát" của B.
- **Then**: Hệ thống cho thu hồi vì B vẫn còn quyền `supervision.complete` từ vai trò còn lại.
- **And**: Hai phân công gói của B giữ nguyên.

#### AC-012

- **Given**: Người quản trị có quyền `user.manage` và đang đăng nhập.
- **When**: Người quản trị gửi yêu cầu khóa tài khoản của chính mình, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối yêu cầu.
- **And**: Tài khoản vẫn đang hoạt động, phiên hiện tại không bị hủy; yêu cầu bị từ chối được ghi nhật ký.

## References

### TDDs

- [TDD-RBAC-002](../tdd/TDD-RBAC-002.md): Tài khoản nhân viên, mật khẩu sinh tự động, khóa và buộc đăng xuất.

### Rules

- BR-RBAC-001
- BR-RBAC-004
- BR-RBAC-005
- BR-RBAC-006
- BR-RBAC-007
- BR-RBAC-008
- BR-RBAC-009
- BR-RBAC-011
- BR-RBAC-012

### Dependencies

- STORY-RBAC-001/Main Flow: vai trò phải được tạo trước khi gán cho nhân viên.
- STORY-RBAC-003/Alternative Flow: chuyển giao phân công trước khi thu hồi vai trò.

## Non-Functional

- Khóa tài khoản và buộc đăng xuất phải cắt được phiên đang mở, không chờ access token hết hạn.
- Mật khẩu do hệ thống sinh phải đủ mạnh và chỉ hiển thị một lần; hệ thống không lưu bản rõ và không có đường xem lại.
- Mọi thao tác quản lý tài khoản và vai trò đều kiểm quyền ở server.

## Out of Scope

- Thay đổi luồng đăng ký, xác thực email, đăng nhập, quên mật khẩu và đổi mật khẩu của khách hàng.
- Nâng một tài khoản khách hàng đã có thành tài khoản nhân viên.
- Xóa vĩnh viễn tài khoản nhân viên. Đợt này chỉ khóa và mở khóa.
- Đăng nhập bằng tài khoản của nhà cung cấp bên ngoài cho nhân viên.
- Gửi email khi tạo tài khoản nhân viên. Việc chuyển mật khẩu cho nhân viên diễn ra ngoài hệ thống.
- Chức năng cho người quản trị đặt lại mật khẩu của nhân viên khác. Nhân viên mất mật khẩu dùng luồng quên mật khẩu sẵn có; điểm còn mở này ghi ở BR-RBAC-006.
