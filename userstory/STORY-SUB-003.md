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

- **Story**: Là người có quyền quản lý gói giám sát, tôi muốn ghi nhận gói của đúng khách hàng và công trình là đã hoàn thành khi công trình xong.
- **Context**: Phạm vi hiện tại chỉ quản lý gói theo công trình và nút hoàn thành, không có chu kỳ hay ngày hết hạn. Lịch và lượt vận hành offline. AC-004/AC-005 là nội dung đã HOÃN, không thuộc nghiệm thu đợt này; xem nợ nghiệp vụ. Người bấm hoàn thành hoặc mở lại phải có quyền `supervision.complete`. Admin có quyền này thì không cần phân công; nhân viên có quyền này phải đang được phân công công trình đó; đợt này không có phân công mức khách hàng theo BR-RBAC-013. Không phân công riêng theo gói; khách hàng và nhân viên không phụ trách không được bấm. Gói giám sát gắn với một công trình, không chia lượt sang công trình khác. Một lượt kiểm tra thực tế là một buổi kỹ sư đến kiểm tra tại công trình. Quyền lợi thiết kế vẫn dùng chung theo tài khoản. Mỗi công trình có tối đa một gói giám sát giữ chỗ, tức là đang gán hoặc đã hoàn thành; các công trình khác nhau của tài khoản được dùng gói riêng cùng lúc.
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

- Tài khoản có gói giám sát hợp lệ gắn với công trình A.
- Tài khoản có quyền truy cập công trình đang yêu cầu giám sát.

### Trigger

Người có quyền mở gói của khách hàng theo công trình hoặc bấm Hoàn thành gói giám sát khi công trình xong.

## Flow

**Cập nhật phạm vi 19/09/2026:** [STORY-SUB-004](STORY-SUB-004.md) bổ sung mua trước/gán sau, hạn gán lần đầu một năm và quyền nhân viên đổi công trình; thay các mô tả cũ cấm mọi chuyển công trình. [STORY-SUB-005](STORY-SUB-005.md) quy định hủy/khôi phục bằng quyền riêng. AC-007 chỉ áp dụng gói đã gán đúng hạn, không áp dụng gói chưa từng gán.

**Cập nhật phạm vi 24/09/2026:** Hoàn thành và mở lại vẫn thuộc phạm vi, áp dụng trên vòng đời mới của STORY-SUB-004/005. "Đang thực hiện" trong Story này nghĩa là gói đã gán công trình. Chỉ gói đã gán mới được hoàn thành; mở lại đưa gói về đã gán trên đúng công trình cũ. Gói đã hoàn thành vẫn giữ chỗ trên công trình, không được đổi công trình, được hủy và khi khôi phục thì về lại đã hoàn thành. Hoàn thành/mở lại vẫn khác hủy/khôi phục và không quản lý hoạt động khảo sát/giám sát.

### Main Flow

1. Xác định gói giám sát và công trình được gắn.
2. Đối chiếu công trình yêu cầu với công trình được gắn.
3. Admin hoặc nhân viên phụ trách chọn đúng khách hàng, công trình và gói đã gán công trình đó. Hệ thống kiểm tra người thao tác có quyền `supervision.complete`; với nhân viên, kiểm tra thêm người đó đang được phân công công trình này.
4. Khi công trình đã xong, người có quyền bấm Hoàn thành gói giám sát.
5. Hệ thống ghi nhận gói được chọn đã hoàn thành, không bắt nhập lý do và lưu người thao tác cùng thời điểm. Gói vẫn gắn với công trình và vẫn giữ chỗ trên công trình đó. Không thay đổi gói của công trình khác hoặc subscription thiết kế; không yêu cầu dữ liệu lượt hoặc lịch offline.

### Alternative Flow

#### ALT-01

Khách muốn dùng gói giám sát đã gắn với công trình A cho công trình khác.

1. Khách không tự gỡ/đổi gói sang công trình khác; yêu cầu sử dụng sai công trình xử lý theo EXC-01. Nhân viên có quyền riêng được đổi công trình theo STORY-SUB-004, phải có lý do và không vi phạm giới hạn gói hiệu lực.

#### ALT-02

Tài khoản có nhiều công trình cần giám sát cùng lúc.

1. Mỗi công trình được có gói giám sát riêng còn hiệu lực khi đáp ứng các điều kiện hợp lệ.
2. Quyền lợi và lượt của mỗi subscription vẫn chỉ dùng cho công trình được gắn.

#### ALT-03

Admin hoặc nhân viên phụ trách mở lại gói đã hoàn thành do thao tác nhầm.

1. Chọn đúng khách hàng, công trình và gói đã hoàn thành.
2. Nhập lý do mở lại.
3. Hệ thống kiểm tra quyền, lý do và công trình không có gói giám sát khác đang thực hiện.
4. Chuyển gói về đã gán trên đúng công trình cũ và lưu lý do; không đổi hạn gán, quyền lợi hay chủ gói. Giữ nguyên các gói khác, không tạo lịch hoặc lượt giám sát.

#### ALT-04

Nhân viên có quyền hủy gói đã hoàn thành, sau đó khôi phục.

1. Hủy theo STORY-SUB-005; gói nhả chỗ trên công trình để công trình có thể nhận gói khác.
2. Khi khôi phục, nếu công trình chưa có gói khác giữ chỗ, gói về lại trạng thái đã hoàn thành trên đúng công trình cũ.

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
2. Giữ nguyên trạng thái và liên kết khách hàng/công trình của gói.

#### EXC-04

Yêu cầu mở lại thiếu quyền, thiếu lý do hoặc công trình đã có gói giám sát khác đang thực hiện.

1. Hệ thống từ chối yêu cầu; lý do rỗng hoặc chỉ có khoảng trắng được xem là thiếu lý do.
2. Giữ nguyên trạng thái gói được chọn và các gói khác.

#### EXC-05

Yêu cầu hoàn thành gói chưa gán công trình hoặc gói đang bị hủy.

1. Hệ thống từ chối; gói giữ nguyên trạng thái.

#### EXC-06

Yêu cầu gán hoặc sửa một gói khác sang công trình đang có gói đã hoàn thành.

1. Hệ thống từ chối vì gói đã hoàn thành vẫn giữ chỗ trên công trình.
2. Muốn giám sát tiếp công trình đó thì mở lại gói đã hoàn thành.

#### EXC-07

Nhân viên có quyền đổi công trình yêu cầu đổi công trình của gói đã hoàn thành.

1. Hệ thống từ chối; phải mở lại gói trước rồi mới đổi công trình.

#### EXC-09

Khôi phục gói đã hoàn thành bị hủy khi công trình đã có gói khác giữ chỗ.

1. Hệ thống từ chối; không tự hủy gói khác và không tự chuyển gói sang công trình khác.

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

**HOÃN — không triển khai; giả định về kỳ trong ca cũ đã bị thay thế bởi gói theo công trình không thời hạn.**

- **Given**: Công trình có gói giám sát còn hiệu lực và 6 lượt kiểm tra thực tế sẵn dùng, không có lịch khác; toàn bộ tình huống diễn ra trong cùng kỳ.
- **When**: Một lịch được xác nhận, sau đó buổi kiểm tra hoàn thành.
- **Then**: Khi xác nhận lịch, công trình có 5 lượt sẵn dùng và 1 lượt đang giữ, chưa tính là đã dùng.
- **And**: Khi hoàn thành, công trình có 5 lượt sẵn dùng, 1 lượt đã dùng và không còn lượt giữ cho buổi đó; không trừ thêm lần nữa.

#### AC-006

- **Given**: Khách hàng có gói giám sát đã gán công trình A và công trình đã xong; người thao tác là Admin có quyền `supervision.complete`, hoặc nhân viên có quyền này và được phân công trực tiếp công trình A.
- **When**: Người đó chọn đúng khách hàng, công trình A và bấm Hoàn thành gói giám sát.
- **Then**: Chỉ gói của A được ghi nhận đã hoàn thành, vẫn gắn với đúng khách hàng và công trình A, đồng thời vẫn giữ chỗ trên A; không bắt nhập lý do.
- **And**: Không thay đổi gói của công trình khác hoặc subscription thiết kế; không yêu cầu dữ liệu lịch hay lượt đang vận hành offline.

#### AC-007

- **Given**: Gói giám sát của công trình A đang thực hiện và chưa được bấm hoàn thành.
- **When**: Thời gian trôi qua một tháng hoặc một năm.
- **Then**: Gói không tự hết hạn, hoàn thành hoặc tạo kỳ mới chỉ vì thời gian trôi qua.
- **And**: Không chạy làm mới lượt giám sát theo chu kỳ.

#### AC-008

- **Given**: Gói giám sát đã gán công trình A; người thao tác là khách hàng hoặc nhân viên không phụ trách A.
- **When**: Người đó gửi yêu cầu hoàn thành gói, kể cả qua API trực tiếp.
- **Then**: Hệ thống từ chối vì không có quyền.
- **And**: Trạng thái gói và liên kết khách hàng/công trình không thay đổi.

#### AC-009

- **Given**: Gói giám sát đã hoàn thành trên công trình A; A không có gói giám sát khác giữ chỗ; người thao tác là Admin có quyền `supervision.complete`, hoặc nhân viên có quyền này và được phân công trực tiếp công trình A.
- **When**: Người đó mở lại gói và nhập lý do có nội dung.
- **Then**: Gói chuyển về đã gán trên công trình A, lý do mở lại được lưu; hạn gán và quyền lợi không đổi.
- **And**: Liên kết khách hàng/công trình và các gói khác giữ nguyên; không tạo lịch hoặc giao dịch lượt giám sát.

#### AC-010

- **Given**: Gói giám sát đã hoàn thành và có người gửi yêu cầu mở lại.
- **When**: Người đó không có quyền, hoặc không nhập lý do có nội dung, hoặc công trình đã có một gói giám sát khác đang thực hiện.
- **Then**: Yêu cầu bị từ chối.
- **And**: Gói được chọn và các gói khác giữ nguyên trạng thái; không tự đóng gói khác để mở lại gói cũ.

#### AC-011

- **Given**: Gói giám sát chưa gán công trình, hoặc đang bị nhân viên hủy.
- **When**: Người có quyền gửi yêu cầu hoàn thành gói.
- **Then**: Yêu cầu bị từ chối.
- **And**: Gói giữ nguyên trạng thái.

#### AC-012

- **Given**: Gói G1 đã hoàn thành trên công trình A; khách có gói G2 chưa gán.
- **When**: Khách gán G2 vào A, hoặc nhân viên đổi công trình của một gói khác sang A.
- **Then**: Yêu cầu bị từ chối vì G1 vẫn giữ chỗ trên A.
- **And**: G1 và G2 giữ nguyên trạng thái và liên kết.

#### AC-013

- **Given**: Gói G1 đã hoàn thành trên công trình A; nhân viên có quyền đổi công trình.
- **When**: Nhân viên yêu cầu đổi công trình của G1 sang công trình B.
- **Then**: Yêu cầu bị từ chối; phải mở lại G1 trước.
- **And**: G1 vẫn đã hoàn thành trên A.

#### AC-014

- **Given**: Gói G1 đã hoàn thành trên công trình A; nhân viên có quyền hủy và khôi phục.
- **When**: Nhân viên hủy G1, rồi khôi phục G1 khi A chưa có gói khác giữ chỗ.
- **Then**: Sau khi hủy, A được nhận gói giám sát khác; sau khi khôi phục, G1 về lại đã hoàn thành trên A.
- **And**: Hạn gán, quyền lợi và lịch sử của G1 được giữ nguyên.

#### AC-015

- **Given**: Gói G1 đã hoàn thành trên công trình A và bị hủy; sau đó gói G2 được gán vào A.
- **When**: Nhân viên khôi phục G1.
- **Then**: Yêu cầu bị từ chối.
- **And**: G1 vẫn bị hủy, G2 vẫn gắn A; không tự hủy G2 hoặc chuyển G1 sang công trình khác.

#### AC-017

- **Given**: Khách có một gói giám sát đã hoàn thành.
- **When**: Khách xem danh sách hoặc chi tiết gói giám sát của mình.
- **Then**: Gói hiện trạng thái đã hoàn thành cùng công trình đã gắn.
- **And**: Khách không được hoàn thành hoặc mở lại gói, kể cả khi gửi yêu cầu trực tiếp.

## References

### TDDs

- [TDD-SUB-003](../tdd/TDD-SUB-003.md): Thiết kế cũ đã bị thay, chỉ giữ để tra cứu; không dùng để triển khai. Phần gán/đổi công trình thay bằng TDD-SUB-004, hủy/khôi phục thay bằng TDD-SUB-005, hoàn thành/mở lại thay bằng TDD-SUB-006.
- [TDD-SUB-004](../tdd/TDD-SUB-004.md): Gán gói và đổi công trình; gói đã hoàn thành vẫn giữ chỗ và không được đổi công trình (AC-012, AC-013).
- [TDD-SUB-005](../tdd/TDD-SUB-005.md): Hủy gói đã hoàn thành và khôi phục về lại đã hoàn thành (AC-014, AC-015).
- [TDD-SUB-006](../tdd/TDD-SUB-006.md): Thiết kế hoàn thành và mở lại trên vòng đời gán công trình; thay phần hoàn thành/mở lại của TDD-SUB-003.

### Rules

- BR-SUB-009/Statement: Giám sát chỉ dùng cho công trình được gắn.

- BR-SUB-006/Statement: Một gói giám sát đang thực hiện cho mỗi công trình.

- BR-SUB-011/Statement: Gói theo công trình, không thời hạn và hoàn thành thủ công.

- BR-SUB-012/Statement: Mở lại gói cần đúng quyền, có lý do và không chồng gói đang thực hiện.

- BR-SUB-023/Then: Không đổi công trình của gói đã hoàn thành.

- BR-SUB-024/Then: Hủy gói đã hoàn thành và nhả chỗ trên công trình.

- BR-SUB-025/Then: Khôi phục gói đã hoàn thành về lại trạng thái đã hoàn thành.

- BR-RBAC-013/Then: Chỉ phân công theo công trình, mỗi công trình một nhân viên phụ trách.

### Dependencies

- STORY-SUB-001: Quy tắc subscription và quyền lợi thiết kế dùng chung.
- STORY-SUB-004: Gán gói và đổi công trình.
- STORY-SUB-005: Hủy và khôi phục gói.

## Non-Functional

- [Chưa chốt yêu cầu đo được.]

## Out of Scope

- Luồng mua và nhận gói theo STORY-PAY-001.
- Tạo và quản lý công trình: công trình là thực thể riêng, khác với bản dự toán; Story và BR của phần này sẽ chuẩn bị riêng.
- EXC-08 và AC-016 về phân công mức khách hàng đã rút ngày 25/09/2026 vì đợt này không còn loại phân công này; không dùng lại hai mã.
- Hoãn quản lý lịch và lượt trên nền tảng: số dư, giữ/trừ/hoàn lượt, đặt/hủy/đổi lịch, điều phối kỹ sư. Các phần này tiếp tục offline theo [nợ nghiệp vụ](../debt/supervision-offline.md).
- Đổi công trình đã gán được đặc tả riêng trong STORY-SUB-004; không cho khách tự chuyển. Chưa thêm cơ chế tự động đóng công trình.
