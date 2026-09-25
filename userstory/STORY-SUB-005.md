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

- **Story**: Là nhân viên được phân quyền, tôi muốn hủy hoặc khôi phục gói đã cấp có ghi lý do để xử lý đúng hiệu lực gói của khách.
- **Context**: Áp dụng cho thiết kế và giám sát, gồm giám sát chưa gán. Tiền hoàn/bù trừ xử lý bên ngoài. Khôi phục không phải cấp gói mới và không áp dụng cho kỳ thiết kế đã bị thay thế bởi lần mua sau.
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

- Có gói đã cấp và nhân viên có thể được xác minh quyền riêng cho thao tác. Không mặc định vai trò nhân viên đồng nghĩa có quyền.

### Trigger

Nhân viên gửi yêu cầu hủy hoặc khôi phục gói.

## Flow

### Main Flow

1. Nhân viên chọn đúng khách hàng và gói đã cấp.
2. Nhân viên chọn hủy hoặc khôi phục và nhập lý do.
3. Hệ thống kiểm tra quyền riêng tương ứng; khi khôi phục còn kiểm tra nguồn hủy, thời hạn và xung đột với gói hiệu lực khác.
4. Hủy hợp lệ có hiệu lực ngay; khôi phục hợp lệ giữ nguyên hạn, quyền lợi và lượt. Lưu lịch sử cùng lý do.
5. Giữ nguyên các gói khác và không thực hiện hoặc ghi nhận hoàn tiền.

### Alternative Flow

#### ALT-01

Hủy giám sát đã gán giải phóng công trình.

1. Nhân viên có quyền hủy G1 với lý do; khách gán G2 vào A.
2. G1 bị hủy và A được nhận G2.
3. Không xóa lịch sử G1 hoặc tự hoàn tiền.

#### ALT-02

Khôi phục giám sát đã gán sau một năm.

1. Ngày 02/10/2027 nhân viên có quyền khôi phục và nhập lý do.
2. Gói phục vụ A trở lại.
3. Giữ nguyên quyền lợi và liên kết A dù đã qua một năm từ lúc cấp.

### Exception Flow

#### EXC-01

Chặn hủy thiếu quyền hoặc lý do.

1. Người thiếu quyền yêu cầu hủy; người có quyền hủy với lý do trống.
2. Từ chối cả hai.
3. Gói vẫn hiệu lực, không ghi thao tác hủy thành công.

#### EXC-02

Chặn khôi phục xung đột thiết kế.

1. Nhân viên có quyền và lý do khôi phục A.
2. Từ chối khôi phục.
3. Giữ B hiệu lực, không tự hủy B.

#### EXC-03

Chặn khôi phục xung đột giám sát.

1. Nhân viên có quyền và lý do khôi phục G1.
2. Từ chối khôi phục.
3. Giữ nguyên G1 bị hủy và G2 hiệu lực.

#### EXC-04

Chặn khôi phục thiết kế hết kỳ.

1. Ngày 01/10 nhân viên có quyền và lý do khôi phục.
2. Từ chối vì kỳ đã hết.
3. Không gia hạn hoặc tạo kỳ mới.

#### EXC-05

Chặn khôi phục giám sát quá hạn chưa gán.

1. Ngày 02/10/2027 nhân viên có quyền và lý do khôi phục.
2. Từ chối vì quá hạn gán lần đầu.
3. Không dùng khôi phục để làm mới hạn một năm.

#### EXC-06

Không khôi phục gói thiết kế đã bị thay thế.

1. Nhân viên thử khôi phục A.
2. Từ chối vì A bị thay thế bởi lần mua mới, không phải bị nhân viên hủy.
3. Không tự quay lại A khi hủy B; B chỉ được khôi phục nếu còn đủ điều kiện.

#### EXC-07

Chặn khôi phục thiếu quyền hoặc lý do.

1. Người thiếu quyền thử khôi phục; người có quyền thử khôi phục không có lý do.
2. Từ chối cả hai yêu cầu.
3. Giữ nguyên trạng thái bị hủy và quyền lợi.

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

- **Given**: Gói có 10 lượt, đã dùng 3, hết hạn 30/9, bị nhân viên hủy 20/9; không có gói khác hiệu lực.
- **When**: Ngày 22/9 nhân viên có quyền khôi phục và nhập lý do.
- **Then**: Gói có hiệu lực lại, vẫn hết hạn 30/9, vẫn đã dùng 3 trên 10 lượt.
- **And**: Không cộng hai ngày bị hủy hoặc lấy bản quyền lợi mới trong danh mục.

#### AC-006

- **Given**: Gói cấp 01/10/2026, chưa từng gán, bị nhân viên hủy; chưa quá một năm.
- **When**: Ngày 01/11/2026 nhân viên có quyền khôi phục với lý do.
- **Then**: Gói trở về chưa gán và có thể gán trong hạn ban đầu.
- **And**: Không tính lại một năm từ ngày khôi phục.

#### AC-007

- **Given**: Gói cấp 01/10/2026, gán A đúng hạn, sau đó bị nhân viên hủy; A chưa có gói khác.
- **When**: Ngày 02/10/2027 nhân viên có quyền khôi phục và nhập lý do.
- **Then**: Gói phục vụ A trở lại.
- **And**: Giữ nguyên quyền lợi và liên kết A dù đã qua một năm từ lúc cấp.

#### AC-008

- **Given**: Gói A bị nhân viên hủy; tài khoản hiện có gói B đang hiệu lực.
- **When**: Nhân viên có quyền và lý do khôi phục A.
- **Then**: Từ chối khôi phục.
- **And**: Giữ B hiệu lực, không tự hủy B.

#### AC-009

- **Given**: Gói G1 gán A bị hủy; A hiện có G2 hiệu lực.
- **When**: Nhân viên có quyền và lý do khôi phục G1.
- **Then**: Từ chối khôi phục.
- **And**: Giữ nguyên G1 bị hủy và G2 hiệu lực.

#### AC-010

- **Given**: Gói thiết kế bị nhân viên hủy, hết kỳ 30/9; không có gói khác.
- **When**: Ngày 01/10 nhân viên có quyền và lý do khôi phục.
- **Then**: Từ chối vì kỳ đã hết.
- **And**: Không gia hạn hoặc tạo kỳ mới.

#### AC-011

- **Given**: Gói cấp 01/10/2026, chưa từng gán, bị nhân viên hủy.
- **When**: Ngày 02/10/2027 nhân viên có quyền và lý do khôi phục.
- **Then**: Từ chối vì quá hạn gán lần đầu.
- **And**: Không dùng khôi phục để làm mới hạn một năm.

#### AC-012

- **Given**: Khách mua A rồi B; B đã thay thế A. Sau đó nhân viên hủy B.
- **When**: Nhân viên thử khôi phục A.
- **Then**: Từ chối vì A bị thay thế bởi lần mua mới, không phải bị nhân viên hủy.
- **And**: Không tự quay lại A khi hủy B; B chỉ được khôi phục nếu còn đủ điều kiện.

#### AC-013

- **Given**: Gói bị nhân viên hủy, còn hạn và không có xung đột.
- **When**: Người thiếu quyền thử khôi phục; người có quyền thử khôi phục không có lý do.
- **Then**: Từ chối cả hai yêu cầu.
- **And**: Giữ nguyên trạng thái bị hủy và quyền lợi.

#### AC-014

- **Given**: Một tác vụ AI đã bắt đầu hợp lệ và giữ 1 lượt của kỳ còn hiệu lực.
- **When**: Nhân viên hủy gói đúng quyền/lý do; tác vụ cũ hoàn tất trước timeout, khách thử tạo tác vụ mới.
- **Then**: Tác vụ cũ được hoàn tất theo quyền đã tiếp nhận, quyết toán lượt vào kỳ đã giữ; tác vụ mới bị chặn.
- **And**: Không hoàn tác tác vụ cũ hoặc chuyển lượt sang kỳ khác. Restore nếu hợp lệ giữ bộ đếm hiện tại sau quyết toán.

## References

### TDDs

- [TDD-SUB-005](../tdd/TDD-SUB-005.md): Thiết kế kỹ thuật bản nháp và đặc tả Unit Test liên quan.

### Rules

- BR-SUB-024/Then
- BR-SUB-025/Then
- BR-SUB-006/Then

### Dependencies

- [Quyết định, hiện trạng và các điểm còn mở](../discovery/payment-packages.md).

## Non-Functional

- Giữ các ràng buộc về số gói hiệu lực, quyền riêng và một lần ghi nhận/cấp gói cả khi yêu cầu lặp hoặc đồng thời. Cơ chế kỹ thuật và ca tích hợp chi tiết bổ sung trong TDD.
- Chưa chốt chỉ tiêu định lượng về hiệu năng hoặc thời gian phản hồi. Không tự đặt SLA cho webhook SePay.

## Out of Scope

- Chuyển tiền hoặc đánh dấu đã hoàn tiền; khôi phục kỳ thiết kế đã bị thay thế; tự gia hạn/bù thời gian; mở lại gói đã hoàn thành không thuộc Story này; thao tác đó theo STORY-SUB-003 và BR-SUB-012.
- Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority, Creator và người thực hiện chưa được phân công; không lấy ví dụ trong template làm giá trị thật.
