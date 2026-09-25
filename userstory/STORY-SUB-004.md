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

# STORY-SUB-004

## Metadata

- **Story**: Là khách hàng, tôi muốn gán gói giám sát đã mua cho công trình của mình để gói phục vụ đúng công trình đó.
- **Context**: Tách việc mua/cấp gói khỏi gán công trình. Gói chưa gán có hạn một năm; đã gán đúng hạn tiếp tục phục vụ sau mốc này. Chỉ quản lý liên kết gói–công trình, chưa quản lý hoạt động khảo sát/giám sát. Theo quyết định người dùng xác nhận ngày 25/09/2026, không ai đổi thẳng gói đã gắn sang công trình khác, kể cả nhân viên và Admin; quyền `supervision.reassign` và BR-SUB-023 đã bỏ. Cùng ngày, người dùng chốt: khi khách gán nhầm, nhân viên có quyền `supervision.unassign` gỡ gói về chưa gán theo STORY-SUB-006, rồi khách gán lại theo Story này trước hạn gán ban đầu.
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

- Khách có gói giám sát đã cấp và có công trình của mình. Công trình là thực thể riêng, khác với bản dự toán; khách tự tạo công trình miễn phí theo STORY-SITE-001. Chưa coi nguồn dữ liệu công trình đã triển khai.

### Trigger

Khách yêu cầu gán gói chưa gán cho một công trình của mình.

## Flow

### Main Flow

1. Khách mở một gói giám sát đã mua đang chưa gán, kể cả gói vừa được nhân viên gỡ theo STORY-SUB-006.
2. Khách chọn công trình thuộc mình.
3. Hệ thống kiểm tra gói chưa bị hủy, còn hạn gán ban đầu và công trình chưa có gói giám sát hiệu lực.
4. Hệ thống gán gói cho công trình và ghi nhận đã sử dụng; không thu tiền thêm.
5. Gói tiếp tục phục vụ công trình sau mốc một năm nếu đã gán đúng hạn.

### Alternative Flow

#### ALT-01

Gán đúng hạn vẫn phục vụ sau một năm.

1. Đến ngày 20/09/2027, gói chưa bị hủy.
2. Gói tiếp tục phục vụ A.
3. Không tự kết thúc hoặc đòi mua lại vì đã qua một năm từ lúc cấp.

#### ALT-02

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ việc đổi công trình của gói đã gán, kể cả nhân viên; xem EXC-07. Nội dung giữ để tra lịch sử.

Đổi công trình sau một năm.

1. Ngày 02/10/2027 nhân viên có quyền đổi sang B và nhập lý do.
2. Cho đổi công trình.
3. Không tạo gói mới hoặc làm mới hạn; không yêu cầu dữ liệu khảo sát/giám sát.

### Exception Flow

#### EXC-01

Quá hạn chưa từng gán.

1. Ngày 02/10/2027 khách yêu cầu gán lần đầu.
2. Từ chối; gói đã mất quyền sử dụng do quá một năm.
3. Không tự hoàn tiền hoặc tạo gói mới.

#### EXC-02

Chặn công trình đã có gói.

1. Khách gán B vào A.
2. Từ chối; A giữ gói cũ, B vẫn chưa gán.
3. B còn được gán cho công trình khác trong hạn ban đầu.

#### EXC-03

Khách không tự gỡ hoặc đổi.

1. Khách gửi yêu cầu gỡ gói hoặc đổi sang B, kể cả yêu cầu trực tiếp.
2. Từ chối; khách không tự gỡ hoặc đổi công trình của gói đã gắn. Muốn sửa gán nhầm, khách liên hệ tổng đài để nhân viên gỡ gói theo STORY-SUB-006.
3. Gói vẫn gắn với A, không tạo thêm gói.

#### EXC-04

**Không nghiệm thu trong phạm vi hiện tại:** thay bằng EXC-07 vì đã bỏ việc đổi công trình theo quyết định ngày 25/09/2026. Nội dung giữ để tra lịch sử.

Từ chối sửa thiếu quyền hoặc lý do.

1. Nhân viên thiếu quyền thử sửa; nhân viên có quyền thử sửa với lý do trống.
2. Cả hai yêu cầu đều bị từ chối.
3. Liên kết gói không thay đổi.

#### EXC-05

**Không nghiệm thu trong phạm vi hiện tại:** thay bằng EXC-07 vì đã bỏ việc đổi công trình theo quyết định ngày 25/09/2026. Nội dung giữ để tra lịch sử.

Từ chối đổi sang công trình khác khách hoặc đã có gói.

1. Nhân viên có quyền và lý do lần lượt thử đổi sang B và C.
2. Cả hai yêu cầu bị từ chối.
3. Gói vẫn gắn A; các gói khác giữ nguyên.

#### EXC-06

Không gán lần đầu sang công trình người khác.

1. U1 yêu cầu gán gói cho B.
2. Từ chối vì không cùng chủ sở hữu.
3. Gói vẫn chưa gán, giữ hạn ban đầu.

#### EXC-07

Không ai đổi thẳng công trình của gói đã gán.

1. Admin hoặc nhân viên có bất kỳ quyền nào yêu cầu đổi gói đang gắn A sang công trình B của cùng khách, kể cả có lý do và gửi trực tiếp tới API.
2. Từ chối theo BR-SUB-009; hệ thống không có thao tác đổi thẳng công trình. Nhân viên chỉ gỡ được gói theo STORY-SUB-006, sau đó khách tự gán lại.
3. Gói vẫn gắn A; trạng thái, hạn và lịch sử gói giữ nguyên.

## Acceptance Criteria

#### AC-001

- **Given**: Khách có gói giám sát đã cấp 01/10/2026, chưa gán; công trình A thuộc khách và chưa có gói.
- **When**: Ngày 01/11/2026 khách gán gói cho A.
- **Then**: Gán thành công, gói được coi là đã sử dụng.
- **And**: Không tạo đơn hoặc yêu cầu thanh toán lại.

#### AC-002

- **Given**: Gói cấp 01/10/2026, chưa từng gán.
- **When**: Ngày 02/10/2027 khách yêu cầu gán lần đầu.
- **Then**: Từ chối; gói đã mất quyền sử dụng do quá một năm.
- **And**: Không tự hoàn tiền hoặc tạo gói mới.

#### AC-003

- **Given**: Gói cấp 19/09/2026, đã gán cho A ngày 01/09/2027.
- **When**: Đến ngày 20/09/2027, gói chưa bị hủy.
- **Then**: Gói tiếp tục phục vụ A.
- **And**: Không tự kết thúc hoặc đòi mua lại vì đã qua một năm từ lúc cấp.

#### AC-004

- **Given**: Công trình A đang có gói hiệu lực; khách có gói B chưa gán và còn hạn.
- **When**: Khách gán B vào A.
- **Then**: Từ chối; A giữ gói cũ, B vẫn chưa gán.
- **And**: B còn được gán cho công trình khác trong hạn ban đầu.

#### AC-005

- **Given**: Gói đã gán cho A; B là công trình khác của cùng khách.
- **When**: Khách gửi yêu cầu gỡ gói hoặc đổi sang B, kể cả yêu cầu trực tiếp.
- **Then**: Từ chối; khách không tự gỡ hoặc đổi công trình của gói đã gắn. Muốn sửa gán nhầm, khách liên hệ tổng đài để nhân viên gỡ gói theo STORY-SUB-006.
- **And**: Gói vẫn gắn với A, không tạo thêm gói.

#### AC-006

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ việc đổi công trình của gói đã gán, kể cả nhân viên; thay bằng AC-013. Nội dung giữ để tra lịch sử.

- **Given**: Nhân viên có quyền riêng; gói đã gán A; B cùng khách và chưa có gói hiệu lực.
- **When**: Nhân viên đổi sang B với lý do “Gán nhầm công trình”.
- **Then**: Gói chuyển liên kết sang B, lưu lý do.
- **And**: Giữ chủ gói, quyền lợi và hạn ban đầu; không kiểm tra trạng thái khảo sát.

#### AC-007

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ việc đổi công trình của gói đã gán, kể cả nhân viên; thay bằng AC-013. Nội dung giữ để tra lịch sử.

- **Given**: Gói cấp 01/10/2026, gán A đúng hạn; B cùng khách chưa có gói.
- **When**: Ngày 02/10/2027 nhân viên có quyền đổi sang B và nhập lý do.
- **Then**: Cho đổi công trình.
- **And**: Không tạo gói mới hoặc làm mới hạn; không yêu cầu dữ liệu khảo sát/giám sát.

#### AC-008

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ việc đổi công trình của gói đã gán, kể cả nhân viên; thay bằng AC-013. Nội dung giữ để tra lịch sử.

- **Given**: Gói đang gán A; công trình B đủ điều kiện.
- **When**: Nhân viên thiếu quyền thử sửa; nhân viên có quyền thử sửa với lý do trống.
- **Then**: Cả hai yêu cầu đều bị từ chối.
- **And**: Liên kết gói không thay đổi.

#### AC-009

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ việc đổi công trình của gói đã gán, kể cả nhân viên; thay bằng AC-013. Nội dung giữ để tra lịch sử.

- **Given**: Gói thuộc khách U1 gán A; B thuộc U2, C thuộc U1 nhưng đã có gói hiệu lực.
- **When**: Nhân viên có quyền và lý do lần lượt thử đổi sang B và C.
- **Then**: Cả hai yêu cầu bị từ chối.
- **And**: Gói vẫn gắn A; các gói khác giữ nguyên.

#### AC-010

- **Given**: Khách U1 có gói chưa gán còn hạn; công trình B thuộc U2.
- **When**: U1 yêu cầu gán gói cho B.
- **Then**: Từ chối vì không cùng chủ sở hữu.
- **And**: Gói vẫn chưa gán, giữ hạn ban đầu.

#### AC-011

- **Given**: Gói giám sát được cấp 29/02/2028 lúc 10:00 theo giờ Việt Nam.
- **When**: Hệ thống xác định hạn gán lần đầu.
- **Then**: Hạn là 28/02/2029 lúc 10:00 theo giờ Việt Nam.
- **And**: Không lấy 365 ngày cố định hoặc kéo dài đến hết ngày.

#### AC-012

- **Given**: Gói chưa từng gán, hạn gán là 19/09/2027 lúc 10:00 giờ Việt Nam.
- **When**: Khách gán đúng 10:00 ngày 19/09/2027.
- **Then**: Từ chối vì đã đến hạn.
- **And**: Gói đã gán đúng hạn trước đó vẫn tiếp tục phục vụ sau mốc này.

#### AC-013

- **Given**: Gói của khách U1 đang gắn công trình A; công trình B của U1 chưa có gói.
- **When**: Admin và nhân viên có đủ các quyền lần lượt yêu cầu đổi gói sang B, có lý do “Gán nhầm công trình”, kể cả gửi trực tiếp tới API.
- **Then**: Cả hai yêu cầu bị từ chối; hệ thống không có thao tác đổi thẳng công trình.
- **And**: Gói vẫn gắn A với trạng thái, hạn và lịch sử như cũ; B vẫn chưa có gói.

## References

### TDDs

- [TDD-SUB-004](../tdd/TDD-SUB-004.md): Thiết kế kỹ thuật bản nháp và đặc tả Unit Test liên quan.

### Rules

- BR-SUB-022/Then
- BR-SUB-009/Then
- BR-SUB-006/Then
- BR-SUB-026/Then: Gói bị nhân viên gỡ được khách gán lại theo Story này.

### Dependencies

- STORY-SUB-006: Nhân viên gỡ gói khi khách gán nhầm, trước khi khách gán lại.
- [Quyết định, hiện trạng và các điểm còn mở](../discovery/payment-packages.md).

## Non-Functional

- Giữ các ràng buộc về số gói hiệu lực, quyền riêng và một lần ghi nhận/cấp gói cả khi yêu cầu lặp hoặc đồng thời. Cơ chế kỹ thuật và ca tích hợp chi tiết bổ sung trong TDD.
- Chưa chốt chỉ tiêu định lượng về hiệu năng hoặc thời gian phản hồi. Không tự đặt SLA cho webhook SePay.

## Out of Scope

- Quản lý khảo sát, tiến độ giám sát, lịch và lượt kiểm tra; khách tự gỡ gói; đổi thẳng công trình của gói đã gán, kể cả nhân viên và Admin; chuyển gói sang khách khác. Nhân viên gỡ gói thuộc STORY-SUB-006. Tạo và quản lý công trình thuộc STORY-SITE-001.
- Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority và người thực hiện chưa được phân công; không lấy ví dụ trong template làm giá trị thật.
