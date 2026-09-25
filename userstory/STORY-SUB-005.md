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

# STORY-SUB-005

## Metadata

- **Story**: Là nhân viên được phân quyền, tôi muốn hủy gói đã cấp có ghi lý do để chấm dứt đúng gói của khách khi cần hoàn tiền.
- **Context**: Áp dụng cho thiết kế và giám sát, gồm giám sát chưa gán. Tiền hoàn/bù trừ xử lý bên ngoài, chẳng hạn qua điện thoại. Người dùng xác nhận ngày 25/09/2026: bỏ thao tác khôi phục cho cả hai loại gói; hủy không hoàn tác được và có hộp xác nhận trước khi hủy; hủy gói giám sát kết thúc phân công; hủy nhầm thì khách mua lại nếu cần. Các nhánh và AC về khôi phục giữ để tra lịch sử, không nghiệm thu.
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

- Có gói đã cấp và nhân viên có thể được xác minh quyền riêng cho thao tác. Không mặc định vai trò nhân viên đồng nghĩa có quyền.

### Trigger

Nhân viên gửi yêu cầu hủy gói.

## Flow

### Main Flow

1. Nhân viên chọn đúng khách hàng và gói đã cấp.
2. Nhân viên chọn hủy. Giao diện hiện hộp xác nhận nói rõ việc hủy không hoàn tác được; nhân viên nhập lý do trong hộp này rồi xác nhận.
3. Hệ thống kiểm tra quyền riêng và lý do.
4. Hủy hợp lệ có hiệu lực ngay và không hoàn tác được. Hệ thống lưu lịch sử cùng lý do. Với gói giám sát, gói nhả chỗ trên công trình, phân công của gói kết thúc, và hệ thống lưu tên, địa chỉ công trình tại lúc hủy.
5. Giữ nguyên các gói khác và không thực hiện hoặc ghi nhận hoàn tiền; nhân viên xử lý tiền với khách bên ngoài hệ thống.

### Alternative Flow

#### ALT-01

Hủy giám sát đã gán giải phóng công trình.

1. Nhân viên có quyền hủy G1 với lý do; khách gán G2 vào A.
2. G1 bị hủy và A được nhận G2.
3. Không xóa lịch sử G1 hoặc tự hoàn tiền.

#### ALT-02

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Khôi phục giám sát đã gán sau một năm.

1. Ngày 02/10/2027 nhân viên có quyền khôi phục và nhập lý do.
2. Gói phục vụ A trở lại.
3. Giữ nguyên quyền lợi và liên kết A dù đã qua một năm từ lúc cấp.

#### ALT-03

Nhân viên hủy nhầm và khách vẫn cần gói.

1. Nhân viên hủy nhầm gói của khách.
2. Hệ thống không có thao tác khôi phục; nhân viên xử lý tiền với khách bên ngoài hệ thống, chẳng hạn qua điện thoại.
3. Nếu khách vẫn cần, khách mua gói mới theo STORY-PAY-001; gói mới có hạn và quyền lợi theo lần mua mới.

#### ALT-04

Công trình chỉ còn gói giám sát đã hủy.

1. Gói giám sát của công trình A bị hủy; A không còn gói giữ chỗ.
2. Khách sửa hoặc xóa được A theo STORY-SITE-001.
3. Gói đã hủy vẫn hiện trong danh sách gói của khách, kèm tên công trình tại lúc hủy.

### Exception Flow

#### EXC-01

Chặn hủy thiếu quyền hoặc lý do.

1. Người thiếu quyền yêu cầu hủy; người có quyền hủy với lý do trống.
2. Từ chối cả hai.
3. Gói vẫn hiệu lực, không ghi thao tác hủy thành công.

#### EXC-02

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Chặn khôi phục xung đột thiết kế.

1. Nhân viên có quyền và lý do khôi phục A.
2. Từ chối khôi phục.
3. Giữ B hiệu lực, không tự hủy B.

#### EXC-03

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Chặn khôi phục xung đột giám sát.

1. Nhân viên có quyền và lý do khôi phục G1.
2. Từ chối khôi phục.
3. Giữ nguyên G1 bị hủy và G2 hiệu lực.

#### EXC-04

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Chặn khôi phục thiết kế hết kỳ.

1. Ngày 01/10 nhân viên có quyền và lý do khôi phục.
2. Từ chối vì kỳ đã hết.
3. Không gia hạn hoặc tạo kỳ mới.

#### EXC-05

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Chặn khôi phục giám sát quá hạn chưa gán.

1. Ngày 02/10/2027 nhân viên có quyền và lý do khôi phục.
2. Từ chối vì quá hạn gán lần đầu.
3. Không dùng khôi phục để làm mới hạn một năm.

#### EXC-06

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Không khôi phục gói thiết kế đã bị thay thế.

1. Nhân viên thử khôi phục A.
2. Từ chối vì A bị thay thế bởi lần mua mới, không phải bị nhân viên hủy.
3. Không tự quay lại A khi hủy B; B chỉ được khôi phục nếu còn đủ điều kiện.

#### EXC-07

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng EXC-08. Nội dung giữ để tra lịch sử.

Chặn khôi phục thiếu quyền hoặc lý do.

1. Người thiếu quyền thử khôi phục; người có quyền thử khôi phục không có lý do.
2. Từ chối cả hai yêu cầu.
3. Giữ nguyên trạng thái bị hủy và quyền lợi.

#### EXC-08

Có yêu cầu khôi phục gói đã hủy.

1. Nhân viên, kể cả người có quyền hủy, gửi yêu cầu khôi phục gói đã hủy, kể cả gửi thẳng tới API.
2. Hệ thống không có thao tác khôi phục; gói vẫn ở trạng thái đã hủy.

## Acceptance Criteria

#### AC-001

- **Given**: Nhân viên có quyền hủy; khách có gói thiết kế hiệu lực.
- **When**: Nhân viên hủy với lý do có nội dung.
- **Then**: Gói ngừng cấp quyền cho thao tác mới ngay; lưu lịch sử và lý do.
- **And**: Không chuyển tiền, không đánh dấu hoàn tiền hoặc tự quay lại gói cũ.

#### AC-002

- **Given**: Nhân viên có quyền; khách có gói giám sát chưa gán còn hạn.
- **When**: Nhân viên hủy và nhập lý do.
- **Then**: Gói bị hủy, không được dùng để gán công trình.
- **And**: Giữ lịch sử; tiền xử lý ngoài hệ thống.

#### AC-003

- **Given**: Công trình A có gói G1; khách còn gói G2 chưa gán còn hạn.
- **When**: Nhân viên có quyền hủy G1 với lý do; khách gán G2 vào A.
- **Then**: G1 bị hủy và A được nhận G2.
- **And**: Không xóa lịch sử G1 hoặc tự hoàn tiền.

#### AC-004

- **Given**: Có gói hiệu lực để kiểm tra riêng từng trường hợp.
- **When**: Người thiếu quyền yêu cầu hủy; người có quyền hủy với lý do trống.
- **Then**: Từ chối cả hai.
- **And**: Gói vẫn hiệu lực, không ghi thao tác hủy thành công.

#### AC-005

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói có 10 lượt, đã dùng 3, hết hạn 30/9, bị nhân viên hủy 20/9; không có gói khác hiệu lực.
- **When**: Ngày 22/9 nhân viên có quyền khôi phục và nhập lý do.
- **Then**: Gói có hiệu lực lại, vẫn hết hạn 30/9, vẫn đã dùng 3 trên 10 lượt.
- **And**: Không cộng hai ngày bị hủy hoặc lấy bản quyền lợi mới trong danh mục.

#### AC-006

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói cấp 01/10/2026, chưa từng gán, bị nhân viên hủy; chưa quá một năm.
- **When**: Ngày 01/11/2026 nhân viên có quyền khôi phục với lý do.
- **Then**: Gói trở về chưa gán và có thể gán trong hạn ban đầu.
- **And**: Không tính lại một năm từ ngày khôi phục.

#### AC-007

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói cấp 01/10/2026, gán A đúng hạn, sau đó bị nhân viên hủy; A chưa có gói khác.
- **When**: Ngày 02/10/2027 nhân viên có quyền khôi phục và nhập lý do.
- **Then**: Gói phục vụ A trở lại.
- **And**: Giữ nguyên quyền lợi và liên kết A dù đã qua một năm từ lúc cấp.

#### AC-008

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói A bị nhân viên hủy; tài khoản hiện có gói B đang hiệu lực.
- **When**: Nhân viên có quyền và lý do khôi phục A.
- **Then**: Từ chối khôi phục.
- **And**: Giữ B hiệu lực, không tự hủy B.

#### AC-009

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói G1 gán A bị hủy; A hiện có G2 hiệu lực.
- **When**: Nhân viên có quyền và lý do khôi phục G1.
- **Then**: Từ chối khôi phục.
- **And**: Giữ nguyên G1 bị hủy và G2 hiệu lực.

#### AC-010

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói thiết kế bị nhân viên hủy, hết kỳ 30/9; không có gói khác.
- **When**: Ngày 01/10 nhân viên có quyền và lý do khôi phục.
- **Then**: Từ chối vì kỳ đã hết.
- **And**: Không gia hạn hoặc tạo kỳ mới.

#### AC-011

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói cấp 01/10/2026, chưa từng gán, bị nhân viên hủy.
- **When**: Ngày 02/10/2027 nhân viên có quyền và lý do khôi phục.
- **Then**: Từ chối vì quá hạn gán lần đầu.
- **And**: Không dùng khôi phục để làm mới hạn một năm.

#### AC-012

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Khách mua A rồi B; B đã thay thế A. Sau đó nhân viên hủy B.
- **When**: Nhân viên thử khôi phục A.
- **Then**: Từ chối vì A bị thay thế bởi lần mua mới, không phải bị nhân viên hủy.
- **And**: Không tự quay lại A khi hủy B; B chỉ được khôi phục nếu còn đủ điều kiện.

#### AC-013

**Không nghiệm thu trong phạm vi hiện tại:** người dùng xác nhận ngày 25/09/2026 bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; thay bằng AC-017. Nội dung giữ để tra lịch sử.

- **Given**: Gói bị nhân viên hủy, còn hạn và không có xung đột.
- **When**: Người thiếu quyền thử khôi phục; người có quyền thử khôi phục không có lý do.
- **Then**: Từ chối cả hai yêu cầu.
- **And**: Giữ nguyên trạng thái bị hủy và quyền lợi.

#### AC-014

- **Given**: Một tác vụ AI đã bắt đầu hợp lệ và giữ 1 lượt của kỳ còn hiệu lực.
- **When**: Nhân viên hủy gói đúng quyền/lý do; tác vụ cũ hoàn tất trước timeout, khách thử tạo tác vụ mới.
- **Then**: Tác vụ cũ được hoàn tất theo quyền đã tiếp nhận, quyết toán lượt vào kỳ đã giữ; tác vụ mới bị chặn.
- **And**: Không hoàn tác tác vụ cũ hoặc chuyển lượt sang kỳ khác.

#### AC-015

- **Given**: Nhân viên có quyền `package.cancel` mở gói giám sát G của khách.
- **When**: Nhân viên bấm Hủy gói, sau đó đóng hộp xác nhận mà không xác nhận; rồi bấm lại, nhập lý do có nội dung và xác nhận.
- **Then**: Mỗi lần bấm Hủy, giao diện hiện hộp xác nhận nói rõ việc hủy không hoàn tác được và có ô nhập lý do. G chỉ bị hủy sau lần xác nhận có lý do.
- **And**: Lần đóng hộp không xác nhận không làm thay đổi G.

#### AC-016

- **Given**: Gói giám sát G1 đã gán công trình A và do nhân viên N phụ trách; nhân viên C có quyền `package.cancel`.
- **When**: C hủy G1 với lý do có nội dung.
- **Then**: G1 ở trạng thái đã hủy và A không còn gói giữ chỗ; phân công của N trên G1 kết thúc và được ghi nhật ký phân công.
- **And**: N không còn xem được A qua G1 và không hoàn thành được G1; G1 không vào danh sách cần chia lại.

#### AC-017

- **Given**: Gói G1 đã bị hủy; nhân viên C có quyền `package.cancel`.
- **When**: C hoặc nhân viên khác gửi yêu cầu khôi phục G1, kể cả gửi thẳng tới API.
- **Then**: Hệ thống không có thao tác khôi phục; yêu cầu không làm thay đổi G1.
- **And**: G1 vẫn ở trạng thái đã hủy; khách muốn dùng tiếp thì mua gói mới.

#### AC-018

- **Given**: Gói giám sát G1 gắn công trình A bị hủy; sau đó khách xóa A.
- **When**: Khách xem danh sách gói của mình.
- **Then**: G1 hiện trạng thái đã hủy, kèm tên công trình A tại lúc hủy.
- **And**: Khách không gán, hoàn thành hay dùng được G1.

## References

### TDDs

- [TDD-SUB-005](../tdd/TDD-SUB-005.md): Thiết kế kỹ thuật bản nháp và đặc tả Unit Test liên quan.

### Rules

- BR-SUB-024/Then
- BR-SUB-006/Then
- BR-RBAC-013/Then: Hủy gói giám sát kết thúc phân công của gói.
- BR-SITE-002/Then: Công trình chỉ còn gói đã hủy thì sửa hoặc xóa được.
- BR-SUB-025/Statement: Đã bỏ ngày 25/09/2026; chỉ để tra lịch sử các nhánh khôi phục.

### Dependencies

- STORY-PAY-001: Khách mua gói mới khi vẫn cần sau khi gói bị hủy.
- STORY-SITE-001: Khách sửa hoặc xóa công trình không còn gói giữ chỗ.
- [Quyết định, hiện trạng và các điểm còn mở](../discovery/payment-packages.md).

## Non-Functional

- Giữ các ràng buộc về số gói hiệu lực, quyền riêng và một lần ghi nhận/cấp gói cả khi yêu cầu lặp hoặc đồng thời. Cơ chế kỹ thuật và ca tích hợp chi tiết bổ sung trong TDD.
- Chưa chốt chỉ tiêu định lượng về hiệu năng hoặc thời gian phản hồi. Không tự đặt SLA cho webhook SePay.

## Out of Scope

- Chuyển tiền hoặc đánh dấu đã hoàn tiền; khôi phục gói đã hủy cho cả gói thiết kế và gói giám sát (đã bỏ ngày 25/09/2026); khôi phục kỳ thiết kế đã bị thay thế; tự gia hạn/bù thời gian. Mở lại gói đã hoàn thành theo STORY-SUB-003 và BR-SUB-012; gỡ gói khỏi công trình theo STORY-SUB-006.
- Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority và người thực hiện chưa được phân công; không lấy ví dụ trong template làm giá trị thật.
