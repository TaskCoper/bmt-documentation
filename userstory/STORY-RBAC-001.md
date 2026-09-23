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


# STORY-RBAC-001

## Metadata

- **Story**: Là người quản trị có quyền quản lý vai trò, tôi muốn tạo vai trò và chọn quyền cho vai trò đó để cấp đúng quyền cho từng nhóm nhân viên mà không phải chờ sửa mã nguồn.
- **Context**: BMT hiện chỉ có hai vai trò cố định là Khách hàng và Admin. Trong khi đó các quy tắc đã chốt về thanh toán và gói dịch vụ đều nói tới "nhân viên có quyền riêng" mà chưa có chỗ nào lưu quyền đó. Story này dựng lớp vai trò và quyền để các quy tắc kia thực hiện được, và để thêm nhóm nhân viên mới sau này chỉ là việc cấu hình.
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

- Người thao tác đã đăng nhập bằng tài khoản nhân viên đang hoạt động và có quyền `role.manage`.
- Hệ thống đã có sẵn danh sách mã quyền do hệ thống định nghĩa. Người quản trị chọn trong danh sách này, không tự đặt mã quyền mới. Chín mã quyền khởi tạo gồm: `commerce.read` xem người mua, gói đã mua, đơn và giao dịch; `supervision.reassign` sửa liên kết gói giám sát và dự án; `package.cancel` hủy hiệu lực gói; `package.restore` khôi phục gói bị hủy; `supervision.complete` hoàn thành và mở lại gói giám sát; `user.manage` mời, khóa và mở khóa tài khoản nhân viên; `role.manage` tạo, sửa, xóa vai trò và gán vai trò cho người; `assignment.manage` phân công và chuyển giao tài nguyên; `audit.read` xem nhật ký thay đổi quyền.
- Hai vai trò hệ thống Admin và Khách hàng đã tồn tại và không sửa được theo BR-RBAC-002.

### Trigger

Người quản trị mở màn hình quản lý vai trò để tạo mới, sửa hoặc xóa một vai trò.

## Flow

### Main Flow

1. Người quản trị mở danh sách vai trò. Hệ thống hiển thị từng vai trò kèm loại vai trò là hệ thống hay tự tạo, số người đang giữ và danh sách quyền của vai trò đó.
2. Người quản trị chọn tạo vai trò mới, nhập tên vai trò và chọn các quyền trong danh sách quyền hệ thống định nghĩa.
3. Hệ thống kiểm tra tên vai trò chưa trùng với vai trò nào đang có.
4. Hệ thống kiểm tra người quản trị đang có tất cả các quyền định đưa vào vai trò, theo BR-RBAC-004.
5. Hệ thống lưu vai trò và ghi nhật ký người thao tác, thời điểm và danh sách quyền đã chọn theo BR-RBAC-012.
6. Vai trò mới sẵn sàng để gán cho nhân viên. Việc gán thực hiện ở STORY-RBAC-002.

### Alternative Flow

#### ALT-01

Người quản trị sửa danh sách quyền của một vai trò tự tạo đang có người giữ.

1. Người quản trị thêm hoặc bỏ quyền của vai trò.
2. Hệ thống kiểm tra rào chắn cấp quyền theo BR-RBAC-004, rồi lưu thay đổi và ghi nhật ký.
3. Những người đang giữ vai trò đó nhận bộ quyền mới chậm nhất khi access token của họ hết hạn, theo BR-RBAC-009. Hệ thống cho biết thay đổi chưa có hiệu lực ngay với người đang đăng nhập.
4. Muốn có hiệu lực ngay, người quản trị buộc đăng xuất những người đó theo STORY-RBAC-002.

#### ALT-02

Người quản trị xóa một vai trò tự tạo không còn ai giữ.

1. Hệ thống kiểm tra vai trò không phải vai trò hệ thống và không còn người nào giữ.
2. Hệ thống xóa vai trò và ghi nhật ký.
3. Các bản ghi nhật ký cũ vẫn giữ tên vai trò đã xóa để tra cứu được lịch sử.

#### ALT-03

Người quản trị đổi tên một vai trò tự tạo.

1. Hệ thống kiểm tra tên mới chưa trùng, rồi lưu và ghi nhật ký.
2. Quyền của vai trò và danh sách người đang giữ không đổi theo thao tác này.

### Exception Flow

#### EXC-01

Người quản trị yêu cầu đổi quyền, đổi tên hoặc xóa vai trò Admin hay Khách hàng.

1. Hệ thống từ chối theo BR-RBAC-002, kể cả yêu cầu gửi trực tiếp tới API.
2. Giữ nguyên vai trò và danh sách người đang giữ; ghi nhật ký yêu cầu bị từ chối.

#### EXC-02

Người quản trị yêu cầu xóa một vai trò đang có người giữ.

1. Hệ thống từ chối theo BR-RBAC-003 và cho biết còn bao nhiêu người đang giữ vai trò đó.
2. Không tự gỡ vai trò khỏi những người đó để xóa cho nhanh.

#### EXC-03

Người quản trị đưa vào vai trò một quyền mà bản thân đang không có.

1. Hệ thống từ chối toàn bộ yêu cầu theo BR-RBAC-004.
2. Không lưu phần quyền hợp lệ rồi bỏ phần còn lại; vai trò giữ nguyên như trước yêu cầu.

#### EXC-04

Người quản trị đặt tên vai trò trùng với một vai trò đang có.

1. Hệ thống từ chối và giữ nguyên dữ liệu; không tạo vai trò thứ hai cùng tên.

#### EXC-05

Người gọi không có quyền `role.manage` yêu cầu xem hoặc thay đổi vai trò.

1. Hệ thống từ chối với mã 403 theo BR-RBAC-011, kể cả yêu cầu gửi trực tiếp tới API.
2. Không trả danh sách vai trò hoặc danh sách quyền cho người gọi.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản trị có quyền `role.manage`, `package.cancel` và `package.restore`; chưa có vai trò nào tên "Nhân viên vận hành gói".
- **When**: Người quản trị tạo vai trò "Nhân viên vận hành gói" với hai quyền `package.cancel` và `package.restore`.
- **Then**: Vai trò được tạo với đúng hai quyền đó.
- **And**: Nhân viên được gán vai trò này có hai quyền đó và không có thêm quyền nào khác từ vai trò này.

#### AC-002

- **Given**: Người quản trị có quyền `role.manage`.
- **When**: Người quản trị yêu cầu đổi danh sách quyền, đổi tên hoặc xóa vai trò Admin hoặc vai trò Khách hàng.
- **Then**: Hệ thống từ chối cả ba loại yêu cầu, kể cả khi gửi trực tiếp tới API.
- **And**: Danh sách quyền, tên vai trò và danh sách người đang giữ không thay đổi.

#### AC-003

- **Given**: Vai trò "Nhân viên tra cứu thanh toán" đang có 3 người giữ.
- **When**: Người quản trị yêu cầu xóa vai trò đó.
- **Then**: Hệ thống từ chối và cho biết còn 3 người đang giữ vai trò này.
- **And**: Ba người đó vẫn giữ nguyên vai trò; hệ thống không tự gỡ vai trò của họ.

#### AC-004

- **Given**: Người quản trị có quyền `role.manage` và `commerce.read`, không có quyền `package.cancel`.
- **When**: Người quản trị tạo một vai trò gồm cả `commerce.read` lẫn `package.cancel`.
- **Then**: Hệ thống từ chối toàn bộ yêu cầu.
- **And**: Không tạo ra vai trò nào chỉ có `commerce.read`.

#### AC-005

- **Given**: Vai trò "Nhân viên vận hành gói" đang có 2 người giữ và đang có quyền `package.cancel`.
- **When**: Người quản trị bỏ quyền `package.cancel` khỏi vai trò này.
- **Then**: Hai người đó mất quyền `package.cancel` chậm nhất khi access token của họ hết hạn.
- **And**: Hệ thống cho biết thay đổi chưa có hiệu lực ngay với người đang đăng nhập và nêu cách buộc đăng xuất.

#### AC-006

- **Given**: Người quản trị có quyền `role.manage`.
- **When**: Người quản trị tạo, sửa, đổi tên hoặc xóa một vai trò tự tạo.
- **Then**: Mỗi thao tác tạo ra một bản ghi nhật ký có người thao tác, loại thao tác, vai trò bị tác động, thời điểm và nội dung thay đổi.
- **And**: Bản ghi nhật ký vẫn còn và vẫn đọc được tên vai trò sau khi vai trò đó bị xóa.

#### AC-007

- **Given**: Một tài khoản nhân viên đang hoạt động nhưng không có quyền `role.manage`.
- **When**: Tài khoản đó gửi yêu cầu xem danh sách vai trò hoặc tạo vai trò mới, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối với mã 403.
- **And**: Không trả danh sách vai trò và không tạo vai trò nào.

## References

### TDDs

- [TDD-RBAC-001](../tdd/TDD-RBAC-001.md): Mô hình vai trò – quyền, thực thi phân quyền và nhật ký.

### Rules

- BR-RBAC-001
- BR-RBAC-002
- BR-RBAC-003
- BR-RBAC-004
- BR-RBAC-009
- BR-RBAC-011
- BR-RBAC-012

### Dependencies

- STORY-RBAC-002/Main Flow: gán vai trò cho nhân viên và buộc đăng xuất để thay đổi quyền có hiệu lực ngay.

## Non-Functional

- Mọi thao tác tạo, sửa, đổi tên và xóa vai trò đều kiểm quyền ở server; giao diện ẩn nút không được coi là cơ chế bảo vệ.
- Nhật ký thay đổi vai trò phải đọc được cả sau khi vai trò liên quan đã bị xóa.

## Out of Scope

- Tự tạo mã quyền mới trên giao diện. Danh sách mã quyền do hệ thống định nghĩa theo từng tính năng.
- Vai trò lồng nhau hoặc vai trò kế thừa quyền của vai trò khác.
- Phân quyền tới từng trường dữ liệu hoặc từng bản ghi cụ thể ngoài cơ chế phân công ở STORY-RBAC-003.
- Gán quyền lẻ trực tiếp cho một người ngoài vai trò.
