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

# STORY-SITE-002

## Metadata

- **Story**: Là nhân viên phụ trách phân công hoặc giám sát, tôi muốn xem công trình của khách trong phạm vi quyền của mình để giao gói đúng người và giám sát đúng nơi.
- **Context**: Công trình do khách tự tạo và quản lý theo STORY-SITE-001; nhân viên chỉ xem, không tạo, sửa hay xóa hộ. Admin và nhân viên có quyền `assignment.manage` xem mọi công trình để phân công gói giám sát. Nhân viên có quyền `supervision.complete` chỉ xem công trình của những gói mình đang phụ trách. Nhân viên được phân công theo từng gói giám sát, không theo công trình. Không thêm mã quyền mới. Người dùng xác nhận các quyết định này ngày 25/09/2026, gồm quyết định bổ sung: hủy gói hoặc gỡ gói khỏi công trình đều kết thúc phân công, nên nhân viên mất quyền xem công trình qua gói đó.
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

- Nhân viên dùng tài khoản nhân viên đang hoạt động; tập quyền được xác định theo BR-RBAC-001.
- Phân công nhân viên cho gói giám sát theo STORY-RBAC-003 và BR-RBAC-013.

### Trigger

Nhân viên mở danh sách, chi tiết công trình hoặc xem/tải tệp đính kèm.

## Flow

### Main Flow

1. Admin hoặc nhân viên có quyền `assignment.manage` mở danh sách công trình.
2. Hệ thống kiểm tra quyền theo BR-SITE-003.
3. Hệ thống trả mọi công trình của mọi khách. Mỗi công trình có tên, địa chỉ, khách hàng sở hữu và các gói giám sát đã gắn, kèm trạng thái gói.
4. Nhân viên chỉ xem; không có thao tác tạo, sửa hay xóa công trình.

### Alternative Flow

#### ALT-01

Nhân viên có quyền `supervision.complete` nhưng không có `assignment.manage`.

1. Nhân viên mở danh sách, chi tiết công trình hoặc xem/tải tệp đính kèm.
2. Hệ thống chỉ trả công trình của các gói đang được phân công cho nhân viên đó tại thời điểm xem.
3. Khi phân công kết thúc hoặc được chuyển giao cho người khác, công trình đó ra khỏi phạm vi xem của nhân viên.

#### ALT-02

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 hủy gói kết thúc phân công theo BR-RBAC-013 khoản 9; thay bằng ALT-03. Nội dung giữ để tra lịch sử.

Gói do nhân viên phụ trách bị hủy nhưng phân công vẫn còn hiệu lực.

1. Nhân viên mở chi tiết công trình của gói đang bị hủy.
2. Hệ thống vẫn cho xem vì nhân viên vẫn phụ trách gói đó.

#### ALT-03

Gói do nhân viên phụ trách bị hủy hoặc bị gỡ khỏi công trình.

1. Hủy theo STORY-SUB-005 hoặc gỡ theo STORY-SUB-006 kết thúc phân công của nhân viên trên gói đó.
2. Từ yêu cầu xem tiếp theo, nhân viên chỉ có quyền `supervision.complete` không còn xem được công trình qua gói đó.
3. Admin và nhân viên có quyền `assignment.manage` vẫn xem được công trình, kèm gói đã hủy nếu gói còn gắn với công trình.

#### ALT-04

Nhân viên xem chi tiết hồ sơ mở rộng và tệp đính kèm.

1. Kiểm phạm vi công trình theo BR-SITE-003; chi tiết hiện thông tin mới, nguồn nếu có và danh sách tệp.
2. Mỗi yêu cầu mở/tải tệp tiếp tục kiểm quyền hiện tại theo BR-SITE-007; không công khai URL gốc.
3. Nhân viên không thêm/thay/xóa tệp. Quyền công trình không tự mở quyền đọc kết quả dự toán riêng của khách.

### Exception Flow

#### EXC-01

Nhân viên chỉ có quyền `supervision.complete` mở công trình của gói do người khác phụ trách, hoặc công trình không có gói nào của mình.

1. Hệ thống từ chối và không tiết lộ tên, địa chỉ hay gói của công trình.

#### EXC-02

Nhân viên không có quyền `assignment.manage` hay `supervision.complete`, ví dụ chỉ có quyền `commerce.read`, mở danh sách hoặc chi tiết công trình.

1. Hệ thống từ chối.
2. Việc thấy tên công trình khi tra cứu gói đã mua thuộc STORY-PAY-002, không mở quyền xem công trình.

#### EXC-03

Tài khoản nhân viên, kể cả Admin, gửi yêu cầu tạo công trình hoặc sửa, xóa công trình của khách.

1. Hệ thống từ chối theo BR-SITE-002.
2. Không tạo hoặc thay đổi dữ liệu.

## Acceptance Criteria

#### AC-001

- **Given**: Khách U1 có công trình A với gói G1 đã gán; khách U2 có công trình C chưa có gói.
- **When**: Admin mở danh sách công trình.
- **Then**: Admin thấy A và C, mỗi công trình có tên, địa chỉ, khách hàng sở hữu và các gói đã gắn kèm trạng thái.
- **And**: Không có thao tác tạo, sửa hay xóa công trình.

#### AC-002

- **Given**: Nhân viên M có quyền `assignment.manage`, không có vai trò Admin; dữ liệu như AC-001.
- **When**: M mở danh sách công trình.
- **Then**: M thấy cả A và C như AC-001.

#### AC-003

- **Given**: Nhân viên N có quyền `supervision.complete`, không có `assignment.manage`, đang phụ trách gói G1 gắn công trình A; gói G3 gắn công trình B do nhân viên N2 phụ trách; công trình C chưa có gói.
- **When**: N mở danh sách công trình.
- **Then**: N chỉ thấy A.

#### AC-004

- **Given**: Dữ liệu như AC-003.
- **When**: N mở chi tiết B, rồi chi tiết C, kể cả gửi yêu cầu trực tiếp tới API.
- **Then**: Cả hai yêu cầu bị từ chối.
- **And**: Không tiết lộ tên, địa chỉ hay gói của B và C.

#### AC-005

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 hủy gói kết thúc phân công theo BR-RBAC-013 khoản 9; thay bằng AC-010. Nội dung giữ để tra lịch sử.

- **Given**: N đang phụ trách gói G1 gắn công trình A; G1 bị hủy nhưng phân công của N với G1 vẫn còn hiệu lực.
- **When**: N mở chi tiết A.
- **Then**: N xem được A và thấy G1 với trạng thái đang bị hủy.

#### AC-006

- **Given**: N đang phụ trách gói G1 gắn công trình A; lúc 10:00 người quản trị chuyển giao G1 cho nhân viên N2.
- **When**: Lúc 10:01, N và N2 lần lượt mở chi tiết A.
- **Then**: Yêu cầu của N bị từ chối; N2 xem được A.

#### AC-007

- **Given**: Nhân viên K chỉ có quyền `commerce.read`.
- **When**: K mở danh sách công trình, rồi mở chi tiết công trình A.
- **Then**: Cả hai yêu cầu bị từ chối.

#### AC-008

- **Given**: Nhân viên N có cả quyền `supervision.complete` và `assignment.manage`; dữ liệu như AC-003.
- **When**: N mở danh sách công trình.
- **Then**: N thấy A, B và C theo phạm vi rộng hơn của quyền `assignment.manage`.

#### AC-009

- **Given**: Công trình C của U2 chưa từng có gói gắn vào; công trình A của U1 có gói G1.
- **When**: Admin và nhân viên M có quyền `assignment.manage` lần lượt yêu cầu tạo công trình mới, đổi tên A và xóa C.
- **Then**: Mọi yêu cầu đều bị từ chối.
- **And**: Không có công trình mới; A giữ tên cũ và C vẫn còn.

#### AC-010

- **Given**: N chỉ có quyền `supervision.complete`, đang phụ trách gói G1 gắn công trình A và gói G2 gắn công trình B; nhân viên M có quyền `assignment.manage`.
- **When**: G1 bị hủy, G2 bị nhân viên gỡ khỏi B; sau đó N mở chi tiết A và B, rồi M mở chi tiết A.
- **Then**: Cả hai yêu cầu của N bị từ chối vì phân công của N trên G1 và G2 đã kết thúc; M xem được A và thấy G1 với trạng thái đã hủy.
- **And**: Không tiết lộ tên, địa chỉ hay gói của A và B cho N.

#### AC-011

- **Given**: Nhân viên có quyền xem công trình.
- **When**: Mở chi tiết hồ sơ mở rộng.
- **Then**: Thấy diện tích, hiện trạng, địa chỉ ba phần, ngân sách, khởi công, phân loại theo cấu hình hồ sơ, nguồn nếu có và tệp; không được sửa.

#### AC-012

- **Given**: Nhân viên có quyền xem công trình theo phân công đang hiệu lực.
- **When**: Mở/tải tệp, sau đó thử lại sau khi mất phân công và không có quyền rộng hơn.
- **Then**: Lần đầu được phép, yêu cầu mới sau mất quyền bị từ chối dù giữ đường dẫn.

#### AC-013

- **Given**: Admin hoặc nhân viên có quyền xem công trình.
- **When**: Gửi yêu cầu thêm, thay, xóa tệp của khách.
- **Then**: Từ chối; quyền xem không cấp quyền quản lý tệp.

#### AC-014

- **Given**: Công trình A mã `BUILDX-HS-20261005-T4W8NC` của gói G1 do nhân viên N phụ trách; công trình B mã `BUILDX-HS-20261005-T4W8NA` không thuộc gói nào của N. Quản lý QL có `assignment.manage`; N chỉ có `supervision.complete`.
- **When**: QL và N mở danh sách công trình của nhân viên rồi tìm bằng “T4W8N”.
- **Then**: Mỗi công trình hiển thị mã hồ sơ. QL nhận cả A và B; N chỉ nhận A, vì tìm theo mã vẫn giới hạn trong phạm vi xem theo BR-SITE-003 khoản 14.
- **And**: Biết mã của B không giúp N xem B; tìm không phân biệt hoa/thường và bỏ qua dấu gạch ngang.

## References

### TDDs

- TDD-SITE-003
- TDD-SITE-005

- [TDD-SITE-001](../tdd/TDD-SITE-001.md): API chỉ đọc cho nhân viên và cách xác định phạm vi xem theo quyền và phân công gói.

### Rules

- BR-SITE-003/Then
- BR-SITE-002/Then
- BR-RBAC-010/Then
- BR-RBAC-011/Then
- BR-RBAC-013/Then
- BR-PAY-005/Then
- BR-SUB-024/Then: Hủy gói giám sát kết thúc phân công.
- BR-SUB-026/Then: Gỡ gói kết thúc phân công.

- BR-SITE-001/Then
- BR-SITE-004/Then
- BR-SITE-005/Then
- BR-SITE-007/Then

### Dependencies

- STORY-SITE-001: Khách tạo và quản lý công trình.
- STORY-RBAC-003: Phân công nhân viên cho gói giám sát.
- STORY-PAY-002: Tra cứu gói đã mua bằng quyền `commerce.read`.

- STORY-SITE-004: Quy tắc bản vẽ và ảnh hiện trạng.

## Non-Functional

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Phạm vi xem được kiểm ở server cho cả danh sách và chi tiết, kể cả yêu cầu gửi trực tiếp tới API, theo BR-RBAC-011. Phân công vừa kết thúc hoặc vừa chuyển giao có hiệu lực ngay từ yêu cầu xem tiếp theo.
- Chưa chốt chỉ tiêu hiệu năng, cách phân trang, thứ tự sắp xếp, bộ lọc hay tìm kiếm trong danh sách công trình của nhân viên.

- Phần mở rộng ngày 01/10/2026 đã được người dùng chốt US/BR; System Test đã cập nhật. TDD/UT còn cần cập nhật; chưa chạy để xác nhận các tiêu chí mới đạt.

## Out of Scope

- Nhân viên tạo, sửa hoặc xóa hộ khách; hiện tên nhân viên phụ trách cho khách; thêm mã quyền riêng để xem công trình.
- Thao tác phân công, chuyển giao và danh sách gói cần chia lại thuộc STORY-RBAC-003 và BR-RBAC-013. Tra cứu gói đã mua thuộc STORY-PAY-002.
- Đã có TDD-SITE-001 và bộ System Test ST-SITE. Code đã triển khai ở commit `182e2a8` của `bmt-be`; unit test và integration test chạy đạt ngày 25/09/2026, System Test chưa chạy. Việc kết thúc phân công khi hủy hoặc gỡ gói, chốt cùng ngày, chưa có trong code. Tài liệu chưa được phê duyệt. Sprint, Priority và người thực hiện chưa được phân công.
