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

<!-- Thay mã TDD-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Sơ đồ dùng mermaid, plantuml hoặc URL. Giữ heading Architecture, Sequence Diagram, Activity Diagram, State Diagram, Data Model.
Ví dụ API phải có heading trùng METHOD /path trong Endpoints. Xoá phần API/sơ đồ không dùng.
Version, Updated At và Change Log do lịch sử phiên bản quản lý, để trống khi nhập mới.
Author/Reviewer là tên hiển thị; gán tài khoản, phê duyệt, giả định và câu hỏi mở trên giao diện sau import.
Thiết kế phải truy vết về Story và Business Rule được cung cấp. Phân biệt thiết kế đề xuất với implementation đã kiểm chứng; không mô tả API/schema/sơ đồ ví dụ như hệ thống thật hoặc tự quyết định nghiệp vụ còn thiếu.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Feature tối đa 300 ký tự và được dùng làm tiêu đề tài liệu. Status được parser nhận: Draft / In Review / Approved / Deprecated; tài liệu nhập mới vẫn có trạng thái phê duyệt Draft.
- Endpoint: đường dẫn tối đa 500 ký tự; tên/mô tả endpoint được lưu trong Name tối đa 300. HTTP method: GET / POST / PUT / PATCH / DELETE / HEAD / OPTIONS.
- Error Codes: mã tối đa 100 ký tự, không trùng trong tài liệu; giữ dạng - **CODE** (400): mô tả. HTTP status trong Error Codes và Response phải là số nguyên đọc được bằng Int32; không tự đặt status khác hợp đồng API.
- URL sơ đồ tối đa 1.000 ký tự. Mục sơ đồ được sử dụng phải có khối mermaid/plantuml hoặc URL theo mẫu; không để một mục sơ đồ rỗng. Nhãn mục được tách từ bullet như tên External API Fields tối đa 300 ký tự.
- Heading Examples phải khớp METHOD /path ở Endpoints. Giữ các nhãn Request:, Response 200: (thay mã theo contract) và Error Response: để parser tách đúng dữ liệu.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
ĐỐI CHIẾU FORM TDD (src/features/tdds/validations.ts; form có thể chặt hơn import):
- Điền Feature, Author, Reviewer, Problem và ít nhất một Goal; các Goal/Non-goal/Notes đã thêm không được rỗng. API đã khai báo phải có endpoint và mô tả/mục đích; Fields phải có tên và ý nghĩa; Error Codes phải có mã, HTTP status và điều kiện.
- Form còn kiểm tra Version, Updated At và nội dung Change Log khi khai báo; với file nhập mới, giữ các trường lịch sử này trống theo hợp đồng import, không tự tạo dữ liệu lịch sử.
-->

# TDD-SUB-004

## Document Info

- **Feature**: Gán gói giám sát đã mua và sửa liên kết dự án
- **Author**: Codex
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

STORY-SUB-004 cho phép mua giám sát trước khi có dự án. Thiết kế cũ TDD-SUB-003 bắt buộc ProjectId lúc cấp và cấm thay đổi nên không thể dùng nguyên bản. Cần quản lý hạn gán một năm, quyền sở hữu, giới hạn một gói hiệu lực mỗi dự án và quyền riêng của nhân viên sửa liên kết.

Backend đã kiểm tra chưa có Project, module nhân viên hoặc SupervisionGrant. Thiết kế dưới đây là phương án mới; không coi mock hay interface là tích hợp đã hoạt động. Không quản lý trạng thái khảo sát/giám sát.

### Goals

- Khách tự gán một gói chưa gán cho đúng dự án của mình trước hạn; không phát sinh đơn hoặc thanh toán mới.
- Nhân viên có quyền riêng sửa liên kết sang dự án khác cùng khách, có lý do, kể cả sau một năm nếu đã gán đúng hạn.
- Chống gán hai gói cùng dự án, gán một gói sang hai dự án và xung đột với hủy/khôi phục.

### Non-goals

- Khách tự gỡ/đổi dự án; chuyển chủ gói; nhân viên gỡ về chưa gán hoặc sửa gói đang bị hủy chưa được giao.
- Lịch khảo sát, phân công kỹ sư, số lượt kiểm tra hoặc xác nhận công trình đủ điều kiện trước khi mua.
- Tạo module dự án đầy đủ hoặc phục hồi luồng hoàn thành lịch sử trong đợt này.

## Architecture

**Giải thích kỹ thuật trong luồng gán**

| Kỹ thuật | Áp dụng ở đây | Ví dụ và giới hạn |
| --- | --- | --- |
| Adapter kiểm tra sở hữu | IProjectOwnershipReader nối luồng gói với dữ liệu Project thật. | Client gửi PR1; server đọc chủ PR1 và so với AccountId của gói. Interface không chứng minh module dự án đã được xây. |
| Khóa và UNIQUE theo trạng thái | Khóa khách/dự án/gói; UNIQUE chỉ tính grant Assigned trên một ProjectId. | Hai gói cùng tranh PR1 thì chỉ một gói được gán. Hủy gói giải phóng chỗ sau commit, không xóa lịch sử liên kết. |
| Optimistic concurrency — kiểm phiên bản khách đang sửa | ExpectedVersion phải bằng Version hiện tại; sau thay đổi tăng Version. | Khách đang xem v1 nhưng nhân viên đã sửa lên v2 thì yêu cầu cũ bị từ chối; client đọc lại. Khóa server và kiểm version giải quyết hai nguy cơ khác nhau. |
| Transaction với audit và receipt | Liên kết hiện tại, sự kiện giải thích và kết quả chống lặp được ghi cùng nhau. | Ghi sự kiện lỗi thì gán cũng rollback; gửi lại sau mất phản hồi không tạo hai sự kiện. |
| Thời hạn theo lịch địa phương | Đổi mốc cấp sang giờ Việt Nam, cộng một năm theo lịch rồi đổi về UTC. | Ngày 29/02 sang 28/02 năm không nhuận; không thay bằng cộng cố định 365 ngày. Không suy gói đã gán thành hết phục vụ từ deadline này. |
| Trạng thái hiệu lực tính khi đọc | Dựa trên State, FirstAssignedAtUtc và deadline để trả EffectiveState. | Gói chưa gán đúng mốc hạn bị chặn dù tác vụ nền chưa chạy; gói đã gán đúng hạn tiếp tục Assigned. |


| Thành phần dự kiến | Trách nhiệm |
| --- | --- |
| SupervisionGrantApi ở presentation/apis/subscription/ | Đọc gói của khách, gán lần đầu; endpoint quản trị riêng cho sửa dự án. |
| AssignSupervisionGrantHandler / ReassignSupervisionGrantHandler | Kiểm quyền, khóa, version, lý do; cập nhật liên kết và audit trong cùng transaction. |
| SupervisionAssignmentPolicy | Hàm thuần kiểm lần gán đầu, thời hạn, cùng chủ sở hữu và gói khác đang hiệu lực. |
| AssignmentDeadlineCalculator | Convert UTC→Asia/Ho_Chi_Minh, AddYears(1), giữ giờ/phút/giây, 29/02→28/02 nếu cần, convert về UTC. |
| IProjectOwnershipReader | Đọc/khóa dự án thật và AccountId; không tin ownerId do client gửi. Adapter phải dùng cùng DB/transaction khi triển khai monolith. |
| Policy `supervision.reassign` | Gắn ở endpoint quản trị sửa dự án; kiểm claim `perm` trong access token theo [TDD-RBAC-001](TDD-RBAC-001.md#architecture). Thay cho `IStaffPermissionReader` của bản trước, vì quyền nay gắn vào vai trò chứ không gắn thẳng vào người. |
| SupervisionStore | Truy vấn/ghi gói, unique index dự án và lịch sử liên kết. |

```mermaid
flowchart LR
    C[Khách hoặc nhân viên] --> A[SupervisionGrantApi]
    A --> H[Assign hoặc Reassign Handler]
    H --> P[Ownership và permission readers]
    H --> R[AssignmentPolicy]
    H --> D[(Grant và audit trong PostgreSQL)]
```

**Notes**:

- Dùng transaction pipeline hiện có; validation lỗi sau mutation phải ném exception để rollback. Quy ước chung từ TDD-PAY-001. Không gọi dịch vụ dự án bên ngoài trong transaction rồi coi dữ liệu đó được khóa an toàn.
- Thứ tự khóa: bản ghi quyền của actor (nếu là nhân viên) → AccountCommerceState của chủ gói → các dự án cũ/mới theo UUID tăng dần → SupervisionGrant → mutation receipt/audit. Cấp, hủy, restore cũng khóa AccountCommerceState nên không chạy xen làm sai liên kết. Chức năng đổi chủ/xóa dự án phải dùng quy ước khóa tương thích; nếu module tương lai không bảo đảm được thì chưa bật gán/sửa.
- Reread gói sau khóa; đối chiếu expectedVersion. Sai version trả 409, không tự thay version rồi lặp lại quyết định người dùng. Idempotency-Key có scope actor+operation+grant; cùng request replay không sửa lần hai, quyền được kiểm lại trước replay.
- Khách: lấy AccountId từ session; kiểm gói thuộc khách trước khi lộ dữ liệu; request chỉ projectId/expectedVersion. Nhân viên: kiểm `supervision.reassign` và tư cách nhân viên; Admin không tự có quyền sửa chỉ vì được xem quản trị.
- Gán lần đầu: State=Unassigned, FirstAssignedAtUtc=NULL, now<AssignmentDeadlineUtc; dự án cùng chủ và chưa có Assigned grant khác. Ghi FirstAssignedAtUtc=now, ProjectId và State=Assigned. Đây là mốc đã sử dụng, không gọi thanh toán.
- Sửa: State=Assigned, FirstAssignedAtUtc đã hợp lệ; không xét hạn lần đầu một lần nữa; lý do sau khi bỏ khoảng trắng đầu/cuối dài 1–2000 ký tự; project đích khác project cũ, cùng khách và chưa có gói Assigned khác. Cùng project trả 409 NoProjectChange (không tạo lịch sử giả); đây là quy tắc kỹ thuật của thao tác sửa, không tự thu phí.
- Không cung cấp route gỡ về chưa gán, không dùng trường nullable để cho client gửi null rồi xóa liên kết. Không có điều kiện đã/chưa khảo sát.
- Chạy đến hạn chỉ làm gói chưa từng gán mất quyền gán. GET tính EffectiveState=ExpiredUnassigned khi now>=deadline; không cần job đúng giây. Gói từng gán đúng hạn tiếp tục Assigned sau deadline. Hủy/restore theo TDD-SUB-005 giữ FirstAssignedAtUtc và deadline.

## Sequence Diagram

```mermaid
sequenceDiagram
    actor U as Người thao tác
    participant A as API
    participant H as Handler
    participant D as PostgreSQL
    U->>A: projectId, expectedVersion, key, reason nếu sửa
    A->>H: Command với actor từ phiên
    H->>D: Khóa quyền, account, dự án và grant
    H->>H: Kiểm ownership, hạn hoặc quyền sửa, version
    H->>D: Kiểm dự án đích chưa có gói Assigned
    alt Hợp lệ
      H->>D: Ghi ProjectId, version, audit và receipt
      D-->>A: Commit
      A-->>U: 200 trạng thái mới
    else Không hợp lệ
      H-->>A: Exception, rollback
      A-->>U: 403/404/409/422
    end
```

## Activity Diagram

```mermaid
flowchart TD
    A[Yêu cầu gán hoặc sửa] --> B{Gán lần đầu?}
    B -->|Có| C{Đúng khách, Unassigned, trước hạn?}
    B -->|Không| D{Nhân viên có quyền, Assigned, có lý do?}
    C -->|Không| X[Từ chối không đổi dữ liệu]
    D -->|Không| X
    C -->|Có| E{Dự án cùng chủ và chưa có gói?}
    D -->|Có| E
    E -->|Không| X
    E -->|Có| F[Lưu liên kết và audit cùng transaction]
```

## State Diagram

```mermaid
stateDiagram-v2
    [*] --> Unassigned: Thanh toán hợp lệ cấp gói
    Unassigned --> Assigned: Khách gán trước hạn
    Unassigned --> ExpiredUnassigned: Đến hạn chưa từng gán
    Assigned --> Assigned: Nhân viên đổi dự án hợp lệ
    Unassigned --> CanceledByStaff: Hủy có quyền và lý do
    Assigned --> CanceledByStaff: Hủy có quyền và lý do
    CanceledByStaff --> Unassigned: Restore chưa từng gán và còn hạn
    CanceledByStaff --> Assigned: Restore từng gán và không xung đột
```

ExpiredUnassigned là trạng thái hiệu lực suy ra từ thời gian, không đòi job cập nhật mới chặn được. Không có chuyển Assigned→Unassigned qua API này. Hạn một năm không tạo transition Assigned→ExpiredUnassigned.

## Data Model

**Ý nghĩa các bảng**

| Bảng | Một dòng đại diện cho gì? | Khi ghi và liên kết |
| --- | --- | --- |
| SupervisionGrant | Một quyền sử dụng gói giám sát khách đã mua; không phải dự án hoặc lịch khảo sát. | Cấp khi thanh toán hợp lệ, ban đầu ProjectId=NULL. Gán/sửa/hủy cập nhật cùng gói, giữ RevisionId và hạn đã cấp. |
| SupervisionAssignmentEvent | Một lần gán đầu hoặc sửa dự án của gói. | Ghi cùng transaction đổi liên kết. OldProjectId=NULL nghĩa lần gán đầu; các lần sửa giữ cả dự án cũ/mới và lý do. |
| PackageMutationReceipt | Một kết quả thao tác để xử lý việc client gửi lại yêu cầu. | Ghi cùng transaction với liên kết và sự kiện; khóa RequestKey theo actor/operation/target. Schema chung ở TDD-SUB-005. |
| Project / User | Công trình thật và chủ tài khoản được dùng để kiểm tra sở hữu. | Đọc từ module dự án/tài khoản; không tạo bảng dự án giả trong payment. Project và grant phải cùng khách. |
| PlanRevision / PaymentFulfillment | Bản quyền lợi đã mua và dấu vết lần thanh toán đã cấp gói này. | Đọc từ danh mục/thanh toán; gán dự án không tạo đơn hay fulfillment mới. |

**Dữ liệu lưu trữ minh họa — mua trước, gán sau**

Dữ liệu giả định, trích các cột cần giải thích; G1, U1, NV1, R2, PR1, PR2, E1, E2, M1, M2 là bí danh UUID, không phải giá trị seed. Mọi giờ lưu dưới đây là UTC. Dự án PR1 và PR2 đã tồn tại, cùng thuộc U1 và chưa có gói Assigned khác tại thời điểm gán/sửa. NV1 là nhân viên được cấp quyền supervision.reassign.

Trong ví dụ, Operation=Assign/Reassign là tên minh họa cho thao tác gán/sửa, chưa quy định giá trị enum khi triển khai. H1/H2 là ký hiệu thay cho RequestHash dài 64 ký tự; ResultBody bên dưới chỉ trích các trường kết quả cần giải thích, không phải toàn bộ phản hồi API. RequestKey lấy từ header Idempotency-Key của yêu cầu.

| Bảng / thời điểm | Các giá trị lưu | Ý nghĩa |
| --- | --- | --- |
| SupervisionGrant, vừa cấp | Id=G1; AccountId=U1; RevisionId=R2; State=Unassigned; ProjectId=NULL; GrantedAtUtc=2026-09-19T03:00:00Z; AssignmentDeadlineUtc=2027-09-19T03:00:00Z; FirstAssignedAtUtc=NULL; Version=1 | Có gói nhưng chưa dùng cho công trình nào. Hạn là 10:00 ngày 19/09/2027 theo giờ Việt Nam. |
| SupervisionGrant, gán đầu | Id=G1; State=Assigned; ProjectId=PR1; FirstAssignedAtUtc=2026-10-01T02:00:00Z; Version=2 | Cập nhật cùng dòng; GrantedAtUtc và AssignmentDeadlineUtc không đổi. |
| SupervisionAssignmentEvent | Id=E1; GrantId=G1; ActorId=U1; OldProjectId=NULL; NewProjectId=PR1; AtUtc=2026-10-01T02:00:00Z; Reason=NULL; GrantVersion=2; ReceiptId=M1 | Khách gán lần đầu, chưa có dự án cũ; không bắt khách nhập lý do như thao tác sửa của nhân viên. |
| PackageMutationReceipt, gán đầu | Id=M1; ActorId=U1; Operation=Assign; TargetId=G1; RequestKey=assign-g1-1; RequestHash=H1; ResultVersion=2; ResultBody={"grantId":"G1","projectId":"PR1","state":"Assigned","version":2}; AtUtc=2026-10-01T02:00:00Z | Lưu kết quả yêu cầu gán đầu. E1.ReceiptId=M1 nối lịch sử gán với kết quả yêu cầu đã xử lý. |
| Cả ba bảng, khách gửi lại sau mất phản hồi | G1 vẫn ProjectId=PR1, Version=2; vẫn chỉ có E1 và M1 cho lần gán này | Khách gửi lại key assign-g1-1 với đúng nội dung cũ, gồm expectedVersion=1. Sau kiểm quyền/sở hữu, server đọc M1 và trả kết quả đã lưu; không tăng version hoặc tạo thêm event/receipt. |
| SupervisionGrant, sửa sau một năm | Id=G1; State=Assigned; ProjectId=PR2; FirstAssignedAtUtc=2026-10-01T02:00:00Z; Version=3 | Nhân viên sửa ngày 01/10/2027 vẫn hợp lệ nếu PR2 trống, vì gói đã gán đúng hạn. Deadline không được làm mới. |
| SupervisionAssignmentEvent, lần sửa | Id=E2; GrantId=G1; ActorId=NV1; OldProjectId=PR1; NewProjectId=PR2; AtUtc=2027-10-01T02:00:00Z; Reason=Điều chỉnh công trình theo yêu cầu khách; GrantVersion=3; ReceiptId=M2 | E1 được giữ nguyên; ghi thêm E2 và receipt M2 của nhân viên có quyền. |
| PackageMutationReceipt, lần sửa | Id=M2; ActorId=NV1; Operation=Reassign; TargetId=G1; RequestKey=reassign-g1-1; RequestHash=H2; ResultVersion=3; ResultBody={"grantId":"G1","projectId":"PR2","state":"Assigned","version":3}; AtUtc=2027-10-01T02:00:00Z | Lưu kết quả yêu cầu sửa có expectedVersion=2 và lý do như E2. E2.ReceiptId=M2; M1 vẫn được giữ nguyên. |

Sau hai thao tác thành công, SupervisionGrant có một dòng G1 đang trỏ tới PR2; SupervisionAssignmentEvent có hai dòng E1/E2; PackageMutationReceipt có hai dòng M1/M2. Nhìn G1 biết dự án hiện tại. Đọc E1/E2 biết khách đã gán vào PR1, rồi NV1 chuyển từ PR1 sang PR2 vào lúc nào và vì sao. Đọc M1/M2 biết kết quả của từng yêu cầu để trả lại khi ứng dụng gửi lại yêu cầu đó.

Nếu U1 gửi lại yêu cầu gán đầu với key assign-g1-1 và đúng nội dung cũ sau khi nhân viên đã đổi sang PR2, server kiểm lại quyền/sở hữu rồi trả kết quả M1: PR1, version=2. Đây là kết quả lần gán trước; dữ liệu hiện tại vẫn là PR2, version=3. Ứng dụng gọi GET để lấy trạng thái hiện tại. Nếu U1 giữ nguyên key đó nhưng đổi projectId trong yêu cầu, server trả 409 IdempotencyConflict vì nội dung không còn khớp H1; không thay đổi ba bảng.

Mỗi thao tác mới ghi thay đổi G1, event và receipt trong cùng một transaction: hoặc lưu thành công cả ba, hoặc hoàn tác cả ba. Vì vậy, E1/E2 phục vụ tra cứu lịch sử, còn M1/M2 phục vụ xử lý yêu cầu gửi lại; hai bảng không thay thế nhau trong thiết kế này.

Nhánh hết hạn: G2 có cùng deadline nhưng chưa từng gán. Tại `2027-09-19T03:00:00Z`, DB có thể vẫn lưu State=Unassigned và FirstAssignedAtUtc=NULL; kết quả đọc là EffectiveState=ExpiredUnassigned, yêu cầu gán bị từ chối. EffectiveState là giá trị tính khi đọc, không phải một dòng hay cột trạng thái mới phải được job cập nhật đúng giây.


Dùng lại và điều chỉnh SupervisionGrant của TDD-SUB-003; không tạo bảng giám sát khác cạnh bảng cũ. Mọi thời điểm lưu UTC; FK lịch sử RESTRICT.

| Bảng | Trường và ràng buộc dự kiến |
| --- | --- |
| SupervisionGrant | Id uuid PK; AccountId uuid NN FK User; RevisionId uuid NN FK PlanRevision; Kind varchar(16) NN CHECK='Supervision'; ProjectId uuid NULL; State varchar(24) NN =Unassigned/Assigned/CanceledByStaff; GrantedAtUtc timestamptz NN; AssignmentDeadlineUtc timestamptz NN; FirstAssignedAtUtc timestamptz NULL; Version bigint NN; CancelEventId uuid NULL; UNIQUE(AccountId,Id), FK(RevisionId,Kind) -> UNIQUE(Id,Kind) của PlanRevision. Enum State chỉ có ba giá trị trên; [TDD-SUB-006](TDD-SUB-006.md#data-model) thêm `Completed` và mở rộng partial unique index sang `Assigned`/`Completed`. Cách xử lý dữ liệu theo schema cũ của TDD-SUB-003, nếu có, xem phần Notes. |
| SupervisionAssignmentEvent | Id uuid PK; GrantId uuid NN FK; ActorId uuid NN FK User; OldProjectId uuid NULL; NewProjectId uuid NN; AtUtc timestamptz NN; Reason text NULL chỉ khi first assignment; GrantVersion bigint NN; ReceiptId uuid NN UNIQUE FK PackageMutationReceipt. CHECK reason có nội dung nếu OldProjectId không NULL; UNIQUE(GrantId,GrantVersion). |
| PackageMutationReceipt | Schema dùng chung ở TDD-SUB-005; kết quả thao tác gán/sửa không phải giao dịch tiền. |

Cột `Kind` lặp lại loại gói và luôn bằng `Supervision`. Một mình nó chưa đủ: khóa ngoại ghép `(RevisionId,Kind)` trỏ tới `UNIQUE(Id,Kind)` của `PlanRevision` mới là phần bắt phiên bản được tham chiếu phải thuộc một gói giám sát thật. Nếu chỉ kiểm ở tầng ứng dụng, một lỗi lập trình hoặc một đường ghi khác vẫn có thể gắn gói thiết kế vào grant. Đây là cùng kỹ thuật [TDD-SUB-001](TDD-SUB-001.md#data-model) dùng cho `PlanOffer`.

CHECK: AssignmentDeadlineUtc>GrantedAtUtc; FirstAssignedAtUtc nếu có phải GrantedAtUtc<=first<deadline. Unassigned đòi ProjectId và FirstAssignedAtUtc NULL; Assigned đòi cả hai NOT NULL. CanceledByStaff giữ nguyên liên kết trước hủy, có thể NULL hoặc có dự án; không dùng hủy để xóa FirstAssignedAtUtc.

Partial unique index `UX_Supervision_Project_Assigned(ProjectId) WHERE State='Assigned'`. Repository hiện chưa có schema giám sát nào nên migration đầu tiên tạo mới bảng theo thiết kế này và index áp dụng ngay. Nếu kiểm tra database thật thấy schema theo TDD-SUB-003, phải chuyển `InProgress` thành `Assigned` rồi mới tạo index; nếu tạo index trước, các gói `InProgress` cũ sẽ lọt ngoài ràng buộc và một công trình có thể có hai gói đang hiệu lực. FK(ProjectId,AccountId) tới UNIQUE(Project.Id,Project.AccountId) khi module Project thật tồn tại; không tạo FK giả hoặc tự thêm stub table để migration chạy.

```mermaid
erDiagram
    User ||--o{ SupervisionGrant : owns
    PlanRevision ||--o{ SupervisionGrant : fixed_benefits
    Project o|--o{ SupervisionGrant : assigned_history
    SupervisionGrant ||--o{ SupervisionAssignmentEvent : records
    User ||--o{ SupervisionAssignmentEvent : actor
    SupervisionGrant ||--o{ PackageMutationReceipt : deduplicates
```

**Notes**:

- `GrantVersion` ghi `SupervisionGrant.Version` tại thời điểm xảy ra sự kiện. `PackageLifecycleEvent` của [TDD-SUB-005](TDD-SUB-005.md#data-model) gọi cột tương đương là `PackageVersion` vì bảng đó dùng chung cho cả kỳ thiết kế lẫn gói giám sát, còn bảng này chỉ ghi sự kiện của gói giám sát. Hai tên khác nhau là có chủ ý, không phải trùng lặp.
- Index `(AccountId,GrantedAtUtc DESC,Id)` cho danh sách khách; `(GrantId,AtUtc,Id)` cho audit. Hạn được lưu một lần khi cấp và không tính lại khi sửa, hủy hoặc restore.
- Mapping C# DateTimeOffset UTC; calculator dùng TimeZoneInfo Asia/Ho_Chi_Minh và lịch, không AddDays(365). Clock được inject để test trước/đúng/sau hạn và leap day.
- Không xóa audit khi sửa lại; mỗi lần tăng Version. Hai yêu cầu gán cùng version chỉ một lần thành công; khách retry cùng key nhận kết quả thao tác đã lưu, không đổi firstAssignedAt.
- Nếu schema cũ đã tồn tại, chạy theo thứ tự: nới `ProjectId` thành nullable và thêm `AssignmentDeadlineUtc`, `FirstAssignedAtUtc`, `CancelEventId`; chuyển dữ liệu trạng thái; kiểm tra lại; rồi mới tạo partial unique index và các CHECK. Trước khi chuyển, đếm số dòng theo từng trạng thái cũ để biết phạm vi ảnh hưởng và giữ lại số đếm đó làm căn cứ đối chiếu sau khi chạy.
- `InProgress` cũ chuyển thành `Assigned` vì cả hai đều là gói đang gắn công trình. Cách ánh xạ `Completed` cũ sang ba trạng thái mới chưa được chốt: luồng mới không có trạng thái hoàn thành, và không được tự coi gói đã hoàn tất là `Assigned` hay `CanceledByStaff`. Phải chốt nghiệp vụ trước khi chạy migration trên database có dữ liệu `Completed`.
- `AssignmentDeadlineUtc` là cột bắt buộc nên dữ liệu cũ phải có giá trị trước khi đặt NOT NULL; nguồn tính hạn cho gói cũ cũng chưa chốt. Không lấy GrantedAt làm firstAssignedAt cho lịch sử nếu không có bằng chứng; backfill từ lịch sử đáng tin cậy, nếu không có thì để `FirstAssignedAtUtc` NULL và xử lý riêng. Chưa chạy migration.
- Module Project còn thiếu nên adapter ownership và quy ước khóa là điều kiện tích hợp bắt buộc; mock chỉ kiểm policy. Không thể chứng minh cross-project FK bằng EF InMemory.

## Internal API

### Endpoints

- **GET** `/api/v1/me/supervision-grants` — Verified session; chỉ gói của chính khách, gồm chưa gán/quá hạn/bị hủy. PageIndex/PageSize theo PagedResult, sort GrantedAtUtc DESC,Id DESC.
- **GET** `/api/v1/me/supervision-grants/{grantId}` — Đọc gói thuộc khách, revision, projectId, firstAssignedAtUtc, assignmentDeadlineUtc, effectiveState và version.
- **POST** `/api/v1/me/supervision-grants/{grantId}/assign` — `{projectId,expectedVersion}`, header Idempotency-Key; chỉ gán lần đầu. Phản hồi gồm `grantId`, `projectId`, `state`, `version`, `eventId` (dòng lịch sử vừa ghi) và `wasAlreadyApplied` (true khi trả lại kết quả của lần gửi trước). Muốn biết `firstAssignedAtUtc` và `assignmentDeadlineUtc`, client gọi GET chi tiết gói.
- **POST** `/api/v1/admin/supervision-grants/{grantId}/reassign` — `{projectId,expectedVersion,reason}`, header Idempotency-Key; verified session + staff permission supervision.reassign. Prefix admin không đồng nghĩa chỉ role Admin, nhân viên được phân quyền dùng được.

### Examples

#### POST /api/v1/me/supervision-grants/{grantId}/assign

```
Request:
Idempotency-Key: 793753ea-0520-4a60-ab2f-dfdbec33c2ba
{"projectId":"33333333-3333-3333-3333-333333333333","expectedVersion":1}

Response 200:
{"value":{"grantId":"44444444-4444-4444-4444-444444444444","projectId":"33333333-3333-3333-3333-333333333333","state":"Assigned","version":2,"eventId":"66666666-6666-6666-6666-666666666666","wasAlreadyApplied":false},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Conflict","code":"ProjectAlreadyHasSupervision","status":409,"detail":"Dự án đã có gói giám sát đang hiệu lực.","messageCode":"ProjectAlreadyHasSupervision","errors":null}
```

#### POST /api/v1/admin/supervision-grants/{grantId}/reassign

```
Request:
Idempotency-Key: 721c5b01-74eb-4ad4-be04-1168e3bb4044
{"projectId":"55555555-5555-5555-5555-555555555555","expectedVersion":2,"reason":"Sửa dự án đã gán nhầm"}

Response 200:
{"value":{"grantId":"44444444-4444-4444-4444-444444444444","projectId":"55555555-5555-5555-5555-555555555555","state":"Assigned","version":3,"eventId":"77777777-7777-7777-7777-777777777777","wasAlreadyApplied":false},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Forbidden","code":"AccessForbidden","status":403,"detail":"Không có quyền sửa dự án của gói.","messageCode":"AccessForbidden","errors":null}
```

### Error Codes

- **Unauthorized** (401): phiên không hợp lệ.
- **AccessForbidden** (403): không có quyền nhân viên tương ứng.
- **SupervisionGrantNotFound** (404): không có gói trong phạm vi được phép.
- **ProjectNotFound** (404): không thấy dự án hoặc không thuộc khách của gói.
- **GrantVersionConflict** (409): expectedVersion không khớp.
- **GrantStateConflict** (409): gói không ở trạng thái được phép thao tác.
- **AssignmentDeadlinePassed** (409): now>=deadline với gán lần đầu.
- **ProjectAlreadyHasSupervision** (409): dự án đã có gói hiệu lực.
- **NoProjectChange** (409): dự án đích trùng dự án cũ khi sửa.
- **IdempotencyConflict** (409): cùng key khác nội dung.
- **AssignmentInputInvalid** (422): thiếu projectId, expectedVersion hoặc lý do sửa có nội dung.
- **ProjectModuleUnavailable** (503): chưa có nguồn ownership/locking thật; không gán dựa trên dữ liệu tự khai.

## References

### User Stories

- STORY-SUB-004

### Business Rules

- BR-SUB-022/Then
- BR-SUB-023/Then
- BR-SUB-006/Then
- BR-SUB-009/Statement

### Use Cases

- STORY-SUB-004/Main Flow

### Others

- [Cấp gói từ thanh toán](TDD-PAY-001.md), [quyền riêng và hủy/restore](TDD-SUB-005.md).
- [TDD cũ cần áp dụng thay đổi](TDD-SUB-003.md); không gọi service bên ngoài nên không có External API trong TDD này.
- [Bảng truy vết kiểm thử](../discovery/payment-technical-design.md); ST-PAY-024–033 và ca tích hợp đồng thời được liệt kê tại đó.

## Change Log

- 2026-09-24: Cập nhật Internal API cho khớp code: mã lỗi 503 đổi thành `ProjectModuleUnavailable`; ví dụ phản hồi gán và sửa dự án thêm `eventId`, `wasAlreadyApplied` và bỏ hai mốc thời gian (đọc qua GET chi tiết gói). Nghiệp vụ không đổi.
- 2026-09-20: Đổi nguồn quyền từ `IStaffPermissionReader` sang policy theo mã quyền của [TDD-RBAC-001](TDD-RBAC-001.md). Mã `supervision.reassign` giữ nguyên tên và nghiệp vụ sửa liên kết gói – dự án không đổi.
