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


# STORY-SUB-003

## Metadata

- **Story**: Là người có quyền quản lý gói giám sát, tôi muốn ghi nhận gói của đúng khách hàng và dự án là đã hoàn thành khi dự án xong.
- **Context**: Phạm vi hiện tại chỉ quản lý gói theo dự án và nút hoàn thành, không có chu kỳ hay ngày hết hạn. Lịch và lượt vận hành offline. AC-004/AC-005 là nội dung đã HOÃN, không thuộc nghiệm thu đợt này; xem nợ nghiệp vụ. Admin và nhân viên được phân công phụ trách dự án được bấm hoàn thành hoặc mở lại; không phân công riêng theo gói; khách hàng và nhân viên không phụ trách không được bấm. Gói giám sát gắn với một công trình, không chia lượt sang công trình khác. Một lượt kiểm tra thực tế là một buổi kỹ sư đến kiểm tra tại công trình. Quyền lợi thiết kế vẫn dùng chung theo tài khoản. Mỗi công trình có tối đa một gói giám sát đang thực hiện; các công trình khác nhau của tài khoản được dùng gói riêng cùng lúc.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Tài khoản có gói giám sát hợp lệ gắn với công trình A.
- Tài khoản có quyền truy cập công trình đang yêu cầu giám sát.

### Trigger

Người có quyền mở gói của khách hàng theo dự án hoặc bấm Hoàn thành gói giám sát khi dự án xong.

## Flow

**Cập nhật phạm vi 19/09/2026:** [STORY-SUB-004](STORY-SUB-004.md) bổ sung mua trước/gán sau, hạn gán lần đầu một năm và quyền nhân viên sửa dự án; thay các mô tả cũ cấm mọi chuyển dự án. [STORY-SUB-005](STORY-SUB-005.md) quy định hủy/khôi phục bằng quyền riêng. Các bước hoàn thành/mở lại dưới đây là thiết kế lịch sử riêng, không phải quản lý khảo sát/giám sát hoặc hủy/khôi phục. AC-007 chỉ áp dụng gói đã gán đúng hạn, không áp dụng gói chưa từng gán.

### Main Flow

1. Xác định gói giám sát và công trình được gắn.
2. Đối chiếu công trình yêu cầu với công trình được gắn.
3. Admin hoặc nhân viên phụ trách chọn đúng khách hàng, dự án và gói đang thực hiện; hệ thống kiểm tra quyền của người thao tác theo phân công hiện tại của dự án, hoặc quyền Admin.
4. Khi dự án đã xong, người có quyền bấm Hoàn thành gói giám sát.
5. Hệ thống ghi nhận gói được chọn đã hoàn thành; không thay đổi gói của dự án khác hoặc subscription thiết kế. Không yêu cầu dữ liệu lượt hoặc lịch offline.

### Alternative Flow

#### ALT-01

Khách muốn dùng gói giám sát đã gắn với dự án A cho dự án khác.

1. Khách không tự gỡ/đổi gói sang dự án khác; yêu cầu sử dụng sai dự án xử lý theo EXC-01. Nhân viên có quyền riêng được sửa dự án theo STORY-SUB-004, phải có lý do và không vi phạm giới hạn gói hiệu lực.

#### ALT-02

Tài khoản có nhiều công trình cần giám sát cùng lúc.

1. Mỗi công trình được có gói giám sát riêng còn hiệu lực khi đáp ứng các điều kiện hợp lệ.
2. Quyền lợi và lượt của mỗi subscription vẫn chỉ dùng cho công trình được gắn.

#### ALT-03

Admin hoặc nhân viên phụ trách mở lại gói đã hoàn thành do thao tác nhầm.

1. Chọn đúng khách hàng, dự án và gói đã hoàn thành.
2. Nhập lý do mở lại.
3. Hệ thống kiểm tra quyền, lý do và dự án không có gói giám sát khác đang thực hiện.
4. Chuyển gói về đang thực hiện và lưu lý do; giữ nguyên các gói khác, không tạo lịch hoặc lượt giám sát.

### Exception Flow

#### EXC-01

Yêu cầu dùng quyền giám sát của công trình A cho công trình B.

1. Hệ thống không cho dùng gói gắn với A để phục vụ B.
2. Không thay đổi dữ liệu gói của A và không tự chuyển gói sang B.

#### EXC-02

Yêu cầu làm một công trình có hai gói giám sát cùng hiệu lực.

1. Hệ thống từ chối thay đổi gây chồng hiệu lực cho công trình này.
2. Không cấp thêm quyền hoặc lượt và không tự thay thế subscription hiện có.

#### EXC-03

Khách hàng hoặc nhân viên không phụ trách yêu cầu hoàn thành gói.

1. Hệ thống từ chối yêu cầu, kể cả khi gửi trực tiếp mà không dùng nút trên giao diện.
2. Giữ nguyên trạng thái và liên kết khách hàng/dự án của gói.

#### EXC-04

Yêu cầu mở lại thiếu quyền, thiếu lý do hoặc dự án đã có gói giám sát khác đang thực hiện.

1. Hệ thống từ chối yêu cầu; lý do rỗng hoặc chỉ có khoảng trắng được xem là thiếu lý do.
2. Giữ nguyên trạng thái gói được chọn và các gói khác.

## Acceptance Criteria

#### AC-001

- **Given**: Tài khoản có công trình A và B; gói giám sát hợp lệ chỉ gắn với A.
- **When**: Hệ thống kiểm tra dùng quyền của subscription này cho B.
- **Then**: Yêu cầu không đạt điều kiện công trình; không được dùng gói của A cho B.
- **And**: Dữ liệu gói của A không thay đổi và gói vẫn gắn với A.

#### AC-002

- **Given**: Tài khoản có công trình A và B; A đã có gói giám sát đang thực hiện, B chưa có.
- **When**: Một gói giám sát hợp lệ được tiếp nhận cho B.
- **Then**: Subscription của A và B được phép cùng có hiệu lực.
- **And**: Mỗi gói chỉ gắn với công trình đã chọn; không dùng chung gói giữa A và B.

#### AC-003

- **Given**: Công trình A đã có gói giám sát đang thực hiện.
- **When**: Một thay đổi làm gói giám sát thứ hai có hiệu lực chồng lên subscription hiện có của A.
- **Then**: Thay đổi bị từ chối; A vẫn chỉ có một gói giám sát đang thực hiện.
- **And**: Không cấp thêm quyền hoặc lượt, không tự thay thế subscription hiện có.

#### AC-004

**HOÃN — không thuộc nghiệm thu hiện tại; giữ làm tham khảo cho phần nợ quản lý lượt.**

- **Given**: Công trình có gói giám sát hợp lệ với quyền lợi số lần kỹ sư kiểm tra thực tế.
- **When**: Hệ thống xác định số lượt tương ứng cho một buổi kỹ sư đến kiểm tra thực tế tại công trình.
- **Then**: Buổi kiểm tra đó tương ứng với một lượt kiểm tra thực tế.
- **And**: Thời điểm giữ/trừ lượt được mô tả ở AC-005; chính sách hủy hoặc đổi lịch còn cần chốt.

#### AC-005

**HOÃN — không triển khai; giả định về kỳ trong ca cũ đã bị thay thế bởi gói theo dự án không thời hạn.**

- **Given**: Công trình có gói giám sát còn hiệu lực và 6 lượt kiểm tra thực tế sẵn dùng, không có lịch khác; toàn bộ tình huống diễn ra trong cùng kỳ.
- **When**: Một lịch được xác nhận, sau đó buổi kiểm tra hoàn thành.
- **Then**: Khi xác nhận lịch, công trình có 5 lượt sẵn dùng và 1 lượt đang giữ, chưa tính là đã dùng.
- **And**: Khi hoàn thành, công trình có 5 lượt sẵn dùng, 1 lượt đã dùng và không còn lượt giữ cho buổi đó; không trừ thêm lần nữa.

#### AC-006

- **Given**: Khách hàng có gói giám sát đang thực hiện gắn với dự án A đã xong; người thao tác là Admin hoặc nhân viên phụ trách dự án A.
- **When**: Người đó chọn đúng khách hàng, dự án A và bấm Hoàn thành gói giám sát.
- **Then**: Chỉ gói của A được ghi nhận đã hoàn thành, vẫn gắn với đúng khách hàng và A.
- **And**: Không thay đổi gói của dự án khác hoặc subscription thiết kế; không yêu cầu dữ liệu lịch hay lượt đang vận hành offline.

#### AC-007

- **Given**: Gói giám sát của dự án A đang thực hiện và chưa được bấm hoàn thành.
- **When**: Thời gian trôi qua một tháng hoặc một năm.
- **Then**: Gói không tự hết hạn, hoàn thành hoặc tạo kỳ mới chỉ vì thời gian trôi qua.
- **And**: Không chạy làm mới lượt giám sát theo chu kỳ.

#### AC-008

- **Given**: Gói giám sát đang thực hiện; người thao tác là khách hàng hoặc nhân viên không phụ trách dự án đó.
- **When**: Người đó gửi yêu cầu hoàn thành gói, kể cả qua API trực tiếp.
- **Then**: Hệ thống từ chối vì không có quyền.
- **And**: Trạng thái gói và liên kết khách hàng/dự án không thay đổi.

#### AC-009

- **Given**: Gói giám sát đã hoàn thành; dự án không có gói giám sát khác đang thực hiện; người thao tác là Admin hoặc nhân viên phụ trách dự án.
- **When**: Người đó mở lại gói và nhập lý do có nội dung.
- **Then**: Gói chuyển về đang thực hiện, lý do mở lại được lưu.
- **And**: Liên kết khách hàng/dự án và các gói khác giữ nguyên; không tạo lịch hoặc giao dịch lượt giám sát.

#### AC-010

- **Given**: Gói giám sát đã hoàn thành và có người gửi yêu cầu mở lại.
- **When**: Người đó không có quyền, hoặc không nhập lý do có nội dung, hoặc dự án đã có một gói giám sát khác đang thực hiện.
- **Then**: Yêu cầu bị từ chối.
- **And**: Gói được chọn và các gói khác giữ nguyên trạng thái; không tự đóng gói khác để mở lại gói cũ.

## References

### TDDs

- [TDD-SUB-003](../tdd/TDD-SUB-003.md): Thiết kế kỹ thuật bản nháp; Reviewer/Approver Tân Trần.

### Rules

- BR-SUB-009/Statement: Giám sát chỉ dùng cho công trình được gắn.

- BR-SUB-006/Statement: Một gói giám sát đang thực hiện cho mỗi công trình.

- BR-SUB-011/Statement: Gói theo dự án, không thời hạn và hoàn thành thủ công.

- BR-SUB-012/Statement: Mở lại gói cần đúng quyền, có lý do và không chồng gói đang thực hiện.

### Dependencies

- STORY-SUB-001: Quy tắc subscription và quyền lợi thiết kế dùng chung.

## Non-Functional

- [Chưa chốt yêu cầu đo được.]

## Out of Scope

- Luồng nhận gói và gia hạn thiết kế cùng thanh toán sau.
- Hoãn quản lý lịch và lượt trên nền tảng: số dư, giữ/trừ/hoàn lượt, đặt/hủy/đổi lịch, điều phối kỹ sư. Các phần này tiếp tục offline theo [nợ nghiệp vụ](../debt/supervision-offline.md).
- Sửa dự án đã gán được đặc tả riêng trong STORY-SUB-004; không cho khách tự chuyển. Chưa thêm cơ chế tự động đóng dự án.
