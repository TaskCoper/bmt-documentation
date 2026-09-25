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

# STORY-SUB-006

## Metadata

- **Story**: Là nhân viên được phân quyền, tôi muốn gỡ gói giám sát khỏi công trình khi khách gán nhầm để khách tự gán lại đúng công trình.
- **Context**: Khách không tự gỡ hoặc đổi công trình của gói đã gán. Người dùng xác nhận ngày 25/09/2026: khách gán nhầm thì gọi tổng đài; nhân viên có quyền `supervision.unassign` gỡ gói về chưa gán; khách tự gán lại trước hạn gán ban đầu. Không có thao tác đổi thẳng gói sang công trình khác.
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

- Khách có gói giám sát đang ở trạng thái đã gán trên một công trình của mình theo STORY-SUB-004.
- Nhân viên có tài khoản đang hoạt động và có quyền `supervision.unassign`; không cần được phân công gói.

### Trigger

Khách báo tổng đài đã gán nhầm công trình; nhân viên mở gói của khách và chọn gỡ gói khỏi công trình.

## Flow

### Main Flow

1. Khách gọi tổng đài báo đã gán nhầm gói vào công trình.
2. Nhân viên có quyền gỡ tìm đúng khách hàng và gói đang ở trạng thái đã gán, chọn gỡ gói và nhập lý do.
3. Hệ thống kiểm tra quyền, lý do có nội dung, gói đang ở trạng thái đã gán và chưa đến hạn gán ban đầu.
4. Hệ thống gỡ gói khỏi công trình: gói về chưa gán, công trình cũ không còn gói này giữ chỗ và phân công của gói kết thúc. Hệ thống lưu người gỡ, thời điểm, lý do, cùng tên và địa chỉ công trình cũ tại lúc gỡ.
5. Khách thấy gói ở trạng thái chưa gán kèm hạn gán, rồi tự gán lại cho đúng công trình theo STORY-SUB-004, trước hạn gán ban đầu.
6. Sau khi khách gán lại, gói chưa có người phụ trách và vào danh sách cần chia lại để người quản trị giao nhân viên phụ trách.

### Alternative Flow

#### ALT-01

Khách đổi ý và gán gói lại vào chính công trình cũ.

1. Sau khi gỡ, khách gán gói vào công trình cũ trước hạn gán ban đầu.
2. Hệ thống cho gán như lần gán thông thường.
3. Gói chưa có người phụ trách, kể cả người từng phụ trách gói trên công trình này, và vào danh sách cần chia lại.

#### ALT-02

Khách sửa hoặc xóa công trình cũ sau khi gói bị gỡ.

1. Công trình cũ không còn gói nào giữ chỗ.
2. Khách sửa tên, địa chỉ hoặc xóa công trình theo STORY-SITE-001.
3. Lịch sử gỡ của gói vẫn giữ tên và địa chỉ công trình tại lúc gỡ.

#### ALT-03

Khách lại gán nhầm sau khi đã được gỡ một lần.

1. Khách gán lại nhầm công trình và báo tổng đài lần nữa.
2. Nhân viên gỡ tiếp nếu gói đang ở trạng thái đã gán và chưa đến hạn gán; không giới hạn số lần gỡ.
3. Hệ thống lưu riêng từng lần gỡ.

### Exception Flow

#### EXC-01

Khách tự gỡ gói.

1. Khách gửi yêu cầu gỡ gói, kể cả gửi thẳng tới API.
2. Hệ thống từ chối; gói vẫn gắn công trình cũ với trạng thái, hạn và phân công như trước.

#### EXC-02

Nhân viên thiếu quyền hoặc thiếu lý do.

1. Nhân viên không có quyền `supervision.unassign` yêu cầu gỡ, kể cả khi đang phụ trách gói; hoặc nhân viên có quyền gửi lý do rỗng hay chỉ có khoảng trắng.
2. Hệ thống từ chối; gói giữ nguyên.

#### EXC-03

Gói không ở trạng thái đã gán.

1. Nhân viên yêu cầu gỡ gói chưa gán, đã hoàn thành hoặc đã hủy.
2. Hệ thống từ chối; gói giữ nguyên trạng thái.

#### EXC-04

Đã đến hoặc đã qua hạn gán ban đầu.

1. Nhân viên yêu cầu gỡ gói đã gán khi thời điểm hiện tại đã đến hoặc đã qua hạn gán ban đầu.
2. Hệ thống từ chối và báo gỡ lúc này sẽ làm khách không gán lại được; gói vẫn gắn công trình cũ.
3. Nếu cần chấm dứt gói, nhân viên dùng thao tác hủy theo STORY-SUB-005.

#### EXC-05

Khách không gán lại kịp trước hạn.

1. Gói đã bị gỡ và khách chưa gán lại cho tới hạn gán ban đầu.
2. Từ mốc hạn, hệ thống không cho gán gói; gói mất quyền sử dụng và hiện với khách ở trạng thái quá hạn gán, giống gói chưa từng gán đã quá hạn.
3. Hệ thống không tự hoàn tiền và không tạo gói thay thế.

## Acceptance Criteria

#### AC-001

- **Given**: Gói G của khách U1 được cấp 01/10/2026 lúc 10:00, đang gán công trình A và do nhân viên N phụ trách; nhân viên S có quyền `supervision.unassign` và không được phân công G.
- **When**: Ngày 15/11/2026, S gỡ G với lý do "Khách gán nhầm công trình".
- **Then**: G về chưa gán và A không còn gói giữ chỗ; hệ thống lưu người gỡ là S, thời điểm, lý do, cùng tên và địa chỉ của A tại lúc gỡ.
- **And**: Phân công của N trên G kết thúc và được ghi nhật ký; hạn gán của G vẫn là 01/10/2027 lúc 10:00; chủ gói, mô tả dịch vụ và quyền lợi không đổi; không thu thêm và không hoàn tiền.

#### AC-002

- **Given**: G vừa bị gỡ khỏi A như AC-001; công trình B của U1 chưa có gói giữ chỗ.
- **When**: Ngày 20/11/2026, U1 gán G vào B.
- **Then**: Gán thành công; G ở trạng thái đã gán trên B.
- **And**: G chưa có người phụ trách và nằm trong danh sách cần chia lại; không phát sinh thanh toán.

#### AC-003

- **Given**: G bị gỡ khỏi A; A chưa có gói giữ chỗ khác; chưa đến hạn gán của G.
- **When**: U1 gán G lại vào A.
- **Then**: Gán thành công; G ở trạng thái đã gán trên A.
- **And**: G chưa có người phụ trách, kể cả N là người từng phụ trách G trên A.

#### AC-004

- **Given**: G đã bị gỡ khỏi A và chưa được gán lại.
- **When**: U1 xem danh sách gói, chi tiết G và chi tiết công trình A.
- **Then**: U1 thấy G ở trạng thái chưa gán kèm hạn gán 01/10/2027 lúc 10:00; A không còn hiện G.
- **And**: U1 không thấy công trình cũ, thời điểm gỡ, lý do gỡ hay người gỡ của G.

#### AC-005

- **Given**: G đã bị gỡ khỏi A, sau đó U1 xóa A.
- **When**: Nhân viên K có quyền `commerce.read` mở lịch sử của G khi tra cứu gói; nhân viên S chỉ có `supervision.unassign`, không có `commerce.read`, cũng thử mở lịch sử của G.
- **Then**: K thấy lần gỡ với người gỡ, thời điểm, lý do, cùng tên và địa chỉ của A tại lúc gỡ; lịch sử vẫn đầy đủ dù A đã bị xóa.
- **And**: Yêu cầu xem lịch sử của S bị từ chối.

#### AC-006

- **Given**: G của U1 đang gán công trình A.
- **When**: U1 gửi yêu cầu gỡ G, kể cả gửi thẳng tới API.
- **Then**: Yêu cầu bị từ chối.
- **And**: G vẫn gắn A với trạng thái, hạn và phân công như trước.

#### AC-007

- **Given**: G đang gán công trình A; nhân viên N có quyền `supervision.complete`, đang phụ trách G nhưng không có `supervision.unassign`; nhân viên S có `supervision.unassign`.
- **When**: N yêu cầu gỡ G với lý do có nội dung; S yêu cầu gỡ G với lý do chỉ có khoảng trắng.
- **Then**: Cả hai yêu cầu bị từ chối.
- **And**: G vẫn gắn A, phân công của N giữ nguyên.

#### AC-008

- **Given**: Khách có gói G1 chưa gán, gói G2 đã hoàn thành trên công trình B và gói G3 đã hủy; nhân viên S có `supervision.unassign`.
- **When**: S lần lượt yêu cầu gỡ G1, G2 và G3 với lý do có nội dung.
- **Then**: Cả ba yêu cầu bị từ chối.
- **And**: Ba gói giữ nguyên trạng thái; G2 vẫn giữ chỗ trên B.

#### AC-009

- **Given**: Gói G được cấp 01/10/2026 lúc 10:00, đang gán công trình A; nhân viên S có `supervision.unassign`.
- **When**: S yêu cầu gỡ G đúng 10:00 ngày 01/10/2027, rồi thử lại ngày 05/10/2027.
- **Then**: Cả hai yêu cầu bị từ chối và S được báo gói đã qua hạn gán nên gỡ sẽ làm khách không gán lại được.
- **And**: G vẫn gắn A; trạng thái và phân công của G giữ nguyên.

#### AC-010

- **Given**: Gói G được cấp 01/10/2026 lúc 10:00, bị gỡ khỏi A ngày 25/09/2027 và chưa được gán lại.
- **When**: Ngày 02/10/2027, U1 gán G vào công trình B chưa có gói giữ chỗ.
- **Then**: Yêu cầu bị từ chối vì đã quá hạn gán; G mất quyền sử dụng.
- **And**: Hệ thống không tự hoàn tiền và không tạo gói thay thế.

#### AC-011

- **Given**: G bị gỡ khỏi A, sau đó U1 gán G vào B nhầm lần nữa; chưa đến hạn gán.
- **When**: S gỡ G khỏi B với lý do có nội dung.
- **Then**: Gỡ thành công; G về chưa gán.
- **And**: Lịch sử của G có hai lần gỡ riêng biệt, mỗi lần kèm công trình cũ tương ứng.

#### AC-012

- **Given**: Gói G được cấp 01/10/2026 lúc 10:00, bị gỡ khỏi A ngày 25/09/2027 và chưa được gán lại; U1 còn gói G9 chưa từng gán, cũng cấp 01/10/2026 lúc 10:00.
- **When**: Ngày 02/10/2027, U1 xem danh sách gói và chi tiết G.
- **Then**: G hiện trạng thái quá hạn gán và không có thao tác gán.
- **And**: G và G9 hiển thị cùng một trạng thái; U1 không thấy công trình cũ, thời điểm gỡ hay lý do gỡ của G.

## References

### TDDs

- [TDD-SUB-007](../tdd/TDD-SUB-007.md): Thiết kế thao tác gỡ gói, bản lưu công trình trong lịch sử, kết thúc phân công và migration gộp của đợt thay đổi.
- [TDD-SUB-004](../tdd/TDD-SUB-004.md): Gán lại gói đã gỡ và cột `AssignedAtUtc`.

### Rules

- BR-SUB-026/Then: Quy tắc gỡ gói đã gán khỏi công trình.
- BR-SUB-022/Then: Gán lại theo điều kiện gán và hạn gán ban đầu; quá hạn thì hiện trạng thái quá hạn gán.
- BR-PAY-005/Then: Nhân viên có quyền tra cứu xem lịch sử gỡ gói.
- BR-SUB-009/Except: Không đổi thẳng công trình; chỉ gỡ theo BR-SUB-026.
- BR-SUB-006/Statement: Mỗi công trình có tối đa một gói giữ chỗ.
- BR-RBAC-013/Then: Gỡ gói kết thúc phân công; gói gán lại vào danh sách cần chia lại.
- BR-SITE-002/Then: Công trình cũ sửa hoặc xóa được khi không còn gói giữ chỗ.

### Dependencies

- STORY-SUB-004: Khách gán và gán lại gói vào công trình.
- STORY-SITE-001: Khách sửa hoặc xóa công trình cũ.
- STORY-RBAC-003: Giao lại nhân viên phụ trách cho gói được gán lại.
- STORY-PAY-002: Nhân viên có quyền tra cứu xem lịch sử gỡ gói.
- [Quyết định, hiện trạng và các điểm còn mở](../discovery/payment-packages.md).

## Non-Functional

- Giữ ràng buộc một gói giữ chỗ trên mỗi công trình và chỉ ghi nhận một lần cho mỗi yêu cầu gỡ khi yêu cầu lặp hoặc chạy đồng thời với thao tác khác trên cùng gói hay cùng công trình. Cơ chế kỹ thuật bổ sung trong TDD.
- Chưa chốt chỉ tiêu định lượng về hiệu năng hoặc thời gian phản hồi.

## Out of Scope

- Đổi thẳng gói sang công trình khác; khách tự gỡ gói; chuyển gói sang khách khác.
- Gỡ gói đã hoàn thành hoặc đã hủy; làm mới hoặc kéo dài hạn gán khi gỡ.
- Hoàn tiền hoặc ghi nhận hoàn tiền trong hệ thống.
- Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority và người thực hiện chưa được phân công; không lấy ví dụ trong template làm giá trị thật.
