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

# TDD-SITE-001

## Document Info

- **Feature**: Công trình — khách tạo, xem, sửa, xóa công trình; nhân viên xem công trình theo phạm vi quyền
- **Author**: Claude
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

Người dùng đã chốt nghiệp vụ Công trình ngày 25/09/2026 trong STORY-SITE-001, STORY-SITE-002 và BR-SITE-001 đến BR-SITE-003. Công trình là nơi thi công thật mà khách muốn được giám sát, là thực thể riêng, khác bản dự toán. Khách tự tạo công trình miễn phí, chỉ nhập tên và địa chỉ, sửa được lúc nào cũng được và chỉ xóa được công trình chưa từng có gói giám sát gắn vào. Nhân viên không tạo, sửa hay xóa hộ; họ chỉ xem theo phạm vi quyền.

Công trình là điểm neo của hai tính năng đã có code: gói giám sát gắn cố định vào một công trình (BR-SUB-009, BR-SUB-022) và nhân viên phụ trách theo từng gói (BR-RBAC-013). Hiện chưa có bảng công trình nào, nên hai tính năng đó đang tham chiếu tới một định danh không kiểm được.

Hiện trạng code đã kiểm ngày 25/09/2026:

| Thành phần | Hiện trạng và ảnh hưởng |
|---|---|
| `SupervisionGrant` (`src/bmt-be.domain/entities/SupervisionGrant.cs`, `SupervisionGrantConfiguration.cs`) | Cột `ProjectId uuid NULL`, không có khóa ngoại. Index duy nhất có lọc `UX_SupervisionGrant_ProjectHolder` trên `ProjectId` với `State IN ('Assigned','Completed')`. CHECK `CK_SupervisionGrant_AssignedColumns` cho phép gói `CanceledByStaff` giữ nguyên công trình cũ. |
| `SupervisionAssignmentEvent` | Cột `OldProjectId`, `NewProjectId`, không có khóa ngoại tới công trình. [TDD-SUB-004](TDD-SUB-004.md#data-model) bỏ bảng này vì gói không còn đổi công trình; tài liệu này không dùng bảng đó. |
| `IProjectOwnershipReader`, `UnavailableProjectOwnershipReader` (`src/bmt-be.application/abstractions/`, `services/`) | Cổng đọc chủ công trình, hiện đăng ký bản tạm luôn ném `DependencyUnavailableException` với mã `ProjectModuleUnavailable`. Vì vậy `POST /api/v1/me/supervision-grants/{grantId}/assign` luôn trả 503. |
| `SupervisionAssignmentFlow` | Khóa tài khoản chủ gói bằng `IDesignSubscriptionStore.LockAccountAsync`, tức `SELECT ... FROM "User" ... FOR UPDATE`, rồi mới đọc chủ công trình qua cổng trên. TDD-SUB-004 đổi khóa này sang `AccountCommerceState` theo TDD-PAY-001. |
| Route và policy (`src/bmt-be.presentation/apis/`, `JwtExtensions.cs`) | Tài nguyên của khách dùng tiền tố `/api/v1/me/...`; màn hình nhân viên dùng `/api/v1/admin/...`. Policy mặc định đòi phiên đã xác minh; mỗi mã quyền có một policy cùng tên. Chưa có policy "có một trong hai quyền". |
| Lỗi và ánh xạ (`ExceptionHandlingMiddleware.cs`, `ApiEndpoint.cs`) | Validator trả 422 dạng ProblemDetails. `NotFoundException` → 404, `NotPermissionException` → 403, `ConflictException` → 409. Vi phạm unique `23505` rơi vào một nhánh 409 chung với mã `ServerError`; chưa có ánh xạ theo tên ràng buộc và chưa bắt lỗi khóa ngoại `23503`. |
| Quy ước chuẩn hóa tên | `User.NormalizedFirstName`, `NormalizedLastName` lưu bản chữ hoa do ứng dụng tính. |
| `Assignment` (`AssignmentConfiguration.cs`) | CHECK `ResourceType IN ('Customer','Project')`. Việc đổi sang loại tài nguyên gói giám sát thuộc TDD-RBAC-003. |

Các yêu cầu khó của thiết kế:

- Hai yêu cầu tạo, hoặc sửa, cùng một tên của cùng khách gửi song song thì chỉ một yêu cầu được thành công (STORY-SITE-001/Non-Functional).
- Yêu cầu xóa công trình và yêu cầu gắn gói vào chính công trình đó gửi cùng lúc không được để lại gói trỏ vào công trình đã xóa.
- "Công trình từng có gói" phải đúng cả khi gói đã hoàn thành hoặc đang bị hủy (BR-SITE-002 khoản 4).
- Nhân viên chỉ có `supervision.complete` xem công trình theo phân công gói, không theo quyền xem thông thường (BR-SITE-003 khoản 3).
- Yêu cầu ngoài phạm vi không được lộ tên, địa chỉ hay gói của công trình (BR-SITE-003 khoản 7).

### Goals

- Có bảng `ConstructionSite` làm nguồn sự thật duy nhất về công trình; `SupervisionGrant` trỏ vào bảng này bằng khóa ngoại ghép theo chủ sở hữu.
- Database bảo đảm tên không trùng trong cùng khách và không xóa được công trình đã từng có gói, kể cả khi các yêu cầu chạy song song.
- Thay bản tạm `UnavailableProjectOwnershipReader` bằng cổng đọc thật `IConstructionSiteOwnershipReader`, để luồng gán gói lần đầu của TDD-SUB-004 chạy được.
- API khách đủ tạo, xem danh sách, xem chi tiết, sửa, xóa; API nhân viên chỉ đọc và lọc đúng phạm vi của BR-SITE-003.
- Mỗi lỗi nghiệp vụ có một mã riêng, đọc được ở phía client.

### Non-goals

- Trạng thái công trình, nhân viên tạo, sửa hoặc xóa hộ, hiện tên nhân viên phụ trách cho khách, địa chỉ tách cấp hành chính, liên kết với bản dự toán, giới hạn số công trình. Các mục này nằm trong Out of Scope của STORY-SITE-001 và STORY-SITE-002.
- Phân trang, thứ tự sắp xếp, bộ lọc và tìm kiếm theo nghiệp vụ: chưa được chốt. Phân trang dưới đây là quyết định kỹ thuật theo khuôn `PagedResult` hiện có.
- Gán gói vào công trình và đổi tên cột của `SupervisionGrant`: thuộc [TDD-SUB-004](TDD-SUB-004.md). Phân công theo gói và danh sách gói cần chia lại: thuộc [TDD-RBAC-003](TDD-RBAC-003.md). Tài liệu này chỉ mô tả phần các thiết kế đó cần từ bảng công trình.
- Chưa triển khai code và chưa tạo migration. Người dùng chốt TDD ngày 25/09/2026; đã có đặc tả Unit Test UT-SITE-001 đến UT-SITE-025, chưa có mã test hoặc kết quả chạy.

## Architecture

```mermaid
flowchart LR
    KH[Khach hang] --> MAPI[ConstructionSiteApi<br/>/me/construction-sites]
    NV[Nhan vien] --> AAPI[ConstructionSiteAdminApi<br/>/admin/construction-sites]
    MAPI --> CH[Handler tao, sua, xoa<br/>query cua khach]
    AAPI --> SQ[Query nhan vien<br/>xac dinh pham vi xem]
    CH --> PG[(PostgreSQL<br/>ConstructionSite)]
    SQ --> PG
    SQ --> AS[(Assignment<br/>SupervisionGrant)]
    CH --> SG[(SupervisionGrant)]
    GA[SupervisionAssignmentFlow<br/>TDD-SUB-004] --> OR[IConstructionSiteOwnershipReader]
    OR --> PG
    CH -.->|23505, 23503| TR[Anh xa loi theo ten rang buoc<br/>TDD-RBAC-003]
```

### Thành phần và trách nhiệm

| Thành phần (dự kiến) | Trách nhiệm |
|---|---|
| `ConstructionSiteApi` (`presentation/apis/construction-site/`) | Năm route của khách dưới `/api/v1/me/construction-sites`, policy mặc định. |
| `ConstructionSiteAdminApi` | Hai route chỉ đọc cho nhân viên dưới `/api/v1/admin/construction-sites`, policy mặc định; phạm vi xem được xác định trong handler. |
| `CreateConstructionSiteCommandHandler`, `UpdateConstructionSiteCommandHandler`, `DeleteConstructionSiteCommandHandler` | Kiểm người gọi là tài khoản khách hàng, kiểm chủ sở hữu, chuẩn hóa dữ liệu, kiểm trùng tên, kiểm version, kiểm điều kiện xóa. |
| `GetMyConstructionSitesQueryHandler`, `GetMyConstructionSiteQueryHandler` | Chỉ đọc công trình của chính khách, kèm các gói đã gắn và trạng thái gói; không đọc bảng phân công. |
| `GetConstructionSitesForStaffQueryHandler`, `GetConstructionSiteForStaffQueryHandler` | Xác định phạm vi từ claim quyền, rồi đọc danh sách hoặc chi tiết trong phạm vi đó. |
| `ConstructionSiteText` (`contract/services/construction-site/`) | Một hàm chuẩn hóa dùng chung cho validator và handler: chuẩn Unicode NFC, bỏ khoảng trắng đầu và cuối, tính `NormalizedName`. |
| `IConstructionSiteOwnershipReader` (`application/abstractions/`) và bản cài ở `persistence/repositories/` | Đọc chủ công trình và giữ khóa để gói không gắn vào công trình đang bị xóa. Thay `IProjectOwnershipReader`. |
| `ConstructionSiteErrorCodes` (`contract/constants/`) | Các mã lỗi ở mục Error Codes. |

Tên thư mục tính năng là `construction-site` ở cả bốn tầng contract, application, presentation và test, theo quy ước một tính năng một tên thư mục.

### Chuẩn hóa tên và kiểm trùng

BR-SITE-001 đòi tên không trùng giữa các công trình của cùng khách, so sau khi bỏ khoảng trắng đầu và cuối, không phân biệt chữ hoa, chữ thường. Thiết kế lưu thêm cột `NormalizedName` và đặt index duy nhất `UX_ConstructionSite_OwnerNormalizedName` trên `(OwnerUserId, NormalizedName)`.

`ConstructionSiteText` tính giá trị theo đúng một thứ tự, dùng cho cả validator lẫn handler:

1. Đưa chuỗi về dạng Unicode NFC. Bàn phím tiếng Việt có thể gửi "à" thành một ký tự hoặc thành "a" cộng dấu huyền rời; hai cách gõ trông giống nhau nhưng khác mã. NFC gộp về một dạng để hai tên nhìn giống nhau thì so bằng nhau, và độ dài đếm đúng số chữ người dùng thấy.
2. Bỏ khoảng trắng đầu và cuối bằng `string.Trim()`. Khoảng trắng ở giữa giữ nguyên, nên "Nhà  phố" (hai dấu cách) khác "Nhà phố", đúng BR-SITE-001/Notes.
3. Kiểm có nội dung và độ dài: tên 1–200, địa chỉ 1–500, tính theo `string.Length` sau hai bước trên. Không tự cắt ngắn.
4. `NormalizedName = Name.ToUpperInvariant()`, theo cùng cách `User.NormalizedFirstName` đang dùng. Chữ có dấu vẫn giữ dấu: "Nhà phố" thành "NHÀ PHỐ", còn "Nha pho" thành "NHA PHO", nên hai tên này khác nhau.

Vì sao không dùng index trên biểu thức `upper("Name")` của PostgreSQL: hàm đó phụ thuộc collation của database, còn phép kiểm trước ở handler chạy bằng .NET. Hai bên có thể lệch nhau ở một vài ký tự, và khi đó handler báo "chưa trùng" nhưng index lại chặn, hoặc ngược lại. Lưu kết quả do ứng dụng tính thì hai lớp kiểm dùng đúng một hàm. Đánh đổi là cột `NormalizedName` là dữ liệu suy ra từ `Name`; nó chỉ được ghi ở một chỗ là phương thức đặt tên của entity, và mỗi lần đổi `Name` đều tính lại.

Kiểm trùng có hai lớp, giống cách TDD-RBAC-003 làm với phân công:

1. Handler tạo và sửa truy vấn trước xem khách đã có công trình khác cùng `NormalizedName` chưa. Có thì trả 409 `ConstructionSiteNameTaken`. Khi sửa, điều kiện loại chính công trình đang sửa (`Id <> @siteId`), nên khách đổi "nhà phố" thành "Nhà Phố" của chính công trình đó vẫn được (STORY-SITE-001/AC-013).
2. Index duy nhất là lớp chặn cuối. Tình huống: khách bấm tạo "Nhà mẹ" hai lần liên tiếp trên hai thiết bị. Cả hai handler cùng thấy chưa có tên này và cùng chèn. PostgreSQL chặn câu `INSERT` đến sau bằng lỗi `23505`. Lớp ánh xạ lỗi theo tên ràng buộc mà TDD-RBAC-003 dự kiến đặt bao ngoài `TransactionPipelineBehavior` nhận tên `UX_ConstructionSite_OwnerNormalizedName` và trả 409 `ConstructionSiteNameTaken`, thay vì nhánh 409 chung của middleware. Kết quả đúng ST-SITE-019: chỉ một công trình "Nhà mẹ".

Công trình bị xóa là xóa cứng, nên dòng biến mất khỏi index và khách tạo lại được tên cũ, đúng BR-SITE-001/Except. Khách khác nhau được trùng tên vì `OwnerUserId` là cột đầu của index.

### Điều kiện xóa và khóa ngoại ghép RESTRICT

BR-SITE-002 khoản 4 chỉ cho xóa công trình chưa từng có gói giám sát gắn vào. Thiết kế không lưu thêm cờ "đã từng có gói" trên công trình, vì cờ đó là một sự thật thứ hai phải đồng bộ với bảng gói. Sự thật này suy ra từ `SupervisionGrant.ConstructionSiteId`: sau khi bỏ việc đổi công trình (BR-SUB-009; BR-SUB-023 đã bỏ), gói đã gắn thì liên kết không bao giờ đổi, kể cả khi bị hủy, hoàn thành hay khôi phục. Vậy công trình X "từng có gói" khi và chỉ khi có ít nhất một dòng `SupervisionGrant` với `ConstructionSiteId = X`.

Bảng `SupervisionAssignmentEvent` không còn là căn cứ: [TDD-SUB-004](TDD-SUB-004.md#data-model) bỏ bảng này vì mỗi gói chỉ có một lần gán, và dữ kiện của lần gán đã nằm ở `SupervisionGrant` và `PackageMutationReceipt`.

**Khóa ngoại ghép theo chủ sở hữu.** TDD-SUB-004 đổi `ProjectId` thành `ConstructionSiteId` và thêm khóa ngoại `FK_SupervisionGrant_ConstructionSite` từ `SupervisionGrant(ConstructionSiteId, AccountId)` tới `ConstructionSite(Id, OwnerUserId)` với `ON DELETE RESTRICT`. Để khóa ngoại này trỏ được, tài liệu này thêm ràng buộc duy nhất `AK_ConstructionSite_Id_OwnerUserId` trên `(Id, OwnerUserId)` của `ConstructionSite`. Riêng `Id` đã là khóa chính nên cặp này luôn duy nhất; ràng buộc chỉ tồn tại để làm đích cho khóa ngoại ghép.

Vì sao ghép thêm chủ sở hữu thay vì chỉ trỏ tới `Id`: khóa ngoại một cột chỉ bảo đảm công trình tồn tại, còn khóa ngoại ghép bảo đảm công trình đó thuộc đúng chủ gói. Nếu handler gán gói có lỗi và ghi gói của U1 vào công trình CS9 của U2, cặp (CS9, U1) không khớp dòng nào của `ConstructionSite(Id, OwnerUserId)`, nên PostgreSQL từ chối bằng lỗi `23503`. Như vậy database tự chặn gói trỏ vào công trình của khách khác, không chỉ dựa vào phép so chủ sở hữu trong handler. Công trình không bao giờ đổi chủ, nên khóa này không bao giờ phải cập nhật theo. Đây là cùng kỹ thuật mà `SupervisionGrant(RevisionId, Kind)` đang dùng để trỏ tới `PlanRevision(Id, Kind)`.

`MATCH SIMPLE`, mặc định của PostgreSQL, bỏ qua dòng có `ConstructionSiteId` NULL, nên gói chưa gán không bị khóa ngoại xét.

Tài liệu này dựa vào khóa ngoại đó theo ba cách:

1. **Kiểm trước ở handler xóa** để trả mã lỗi rõ nghĩa 409 `ConstructionSiteInUse`.
2. **Khóa ngoại là lớp chặn cuối.** Nếu một đường ghi nào đó bỏ qua bước kiểm, PostgreSQL vẫn từ chối câu `DELETE` bằng lỗi `23503`. Cùng một khóa ngoại cho ra lỗi `23503` ở hai tình huống khác nhau: khi xóa công trình còn gói (công trình đang được dùng) và khi ghi gói vào công trình không tồn tại hoặc khác chủ (không thấy công trình). Vì vậy lớp ánh xạ lỗi phải xét cả tên ràng buộc lẫn lệnh đang chạy: `DeleteConstructionSiteCommand` đổi thành 409 `ConstructionSiteInUse`, còn lệnh gán gói đổi thành 404 `ConstructionSiteNotFound` theo TDD-SUB-004.
3. **Khóa ngoại tự tạo khóa dòng giữa xóa và gắn gói.** Khi một câu lệnh ghi `ConstructionSiteId = X` vào `SupervisionGrant`, PostgreSQL kiểm dòng cha còn tồn tại và giữ khóa `FOR KEY SHARE` trên dòng X tới hết transaction. Khóa này xung đột với khóa mà `DELETE` cần, nên hai thao tác không thể cùng thành công.

**Thứ tự khóa.** Luồng gán gói ở TDD-SUB-004 khóa theo thứ tự `AccountCommerceState` của chủ gói → dòng `ConstructionSite` đích (`FOR KEY SHARE`, qua `IConstructionSiteOwnershipReader`) → `SupervisionGrant` (`FOR UPDATE`) → biên nhận. Handler xóa công trình chỉ khóa đúng một dòng `ConstructionSite` (`FOR UPDATE`), không khóa `AccountCommerceState` và không khóa gói; việc kiểm gói tham chiếu do câu kiểm và khóa ngoại làm. Hai chuỗi khóa chỉ gặp nhau ở dòng công trình, nên không tạo vòng chờ.

Tình huống của ST-SITE-020: công trình B của U1 chưa từng có gói; U1 xóa B trong lúc gắn gói G4 vào B.

- Nếu gắn gói chạy trước: luồng gán khóa `AccountCommerceState` của U1, rồi cổng `IConstructionSiteOwnershipReader` đọc B kèm `FOR KEY SHARE`. Handler xóa khóa B bằng `FOR UPDATE` thì phải chờ. Sau khi gắn gói commit, câu kiểm của handler xóa chạy sau khi lấy được khóa, thấy G4 đã trỏ vào B, và trả 409 `ConstructionSiteInUse`.
- Nếu xóa chạy trước: handler xóa giữ `FOR UPDATE` trên B. Cổng đọc chủ công trình phải chờ; khi xóa commit, dòng B không còn, cổng trả "không có công trình", và luồng gán trả 404 `ConstructionSiteNotFound`. G4 vẫn chưa gán.

Ở mức cô lập `READ COMMITTED` mặc định, mỗi câu lệnh thấy dữ liệu đã commit tại lúc câu lệnh bắt đầu. Vì vậy câu kiểm "có gói nào trỏ vào B không" phải chạy **sau** khi handler xóa đã lấy khóa dòng B, không chạy trước.

Khóa ngoại `RESTRICT` cần index trên cột con để PostgreSQL tìm nhanh các dòng tham chiếu khi xóa. Index giữ chỗ `UX_SupervisionGrant_ConstructionSiteHolder` chỉ chứa gói `Assigned` và `Completed` nên không dùng được cho việc này. TDD-SUB-004 thêm index thường `IX_SupervisionGrant_ConstructionSiteId_AccountId` trên `(ConstructionSiteId, AccountId)`; câu kiểm của handler xóa và danh sách công trình kèm gói cũng dùng index này.

### Cổng đọc chủ công trình

`IConstructionSiteOwnershipReader` thay `IProjectOwnershipReader`. Cổng chỉ có một hàm:

```
Task<Guid?> LockOwnerAccountIdAsync(Guid constructionSiteId, CancellationToken cancellationToken = default);
```

- Trả `OwnerUserId` của công trình, hoặc `null` nếu không có công trình đó.
- Chạy `SELECT "OwnerUserId" FROM "ConstructionSite" WHERE "Id" = @id FOR KEY SHARE` trên cùng `DbContext` và cùng transaction của handler gọi. Khóa giữ tới hết transaction.
- Dùng `FOR KEY SHARE` chứ không dùng `FOR SHARE`. `FOR KEY SHARE` chỉ chặn xóa dòng hoặc sửa cột khóa; khách đổi địa chỉ công trình cùng lúc không phải chờ. Chủ sở hữu không bao giờ đổi, nên chặn xóa là đủ để kết quả đọc đúng tới lúc commit.
- Bản cài `ConstructionSiteOwnershipReader` đặt ở `persistence/repositories/` vì dùng SQL thô, giống `DesignSubscriptionStore.LockAccountAsync`. Đăng ký thay `UnavailableProjectOwnershipReader` và xóa bản tạm đó.

Luồng gán gói vẫn tự so `OwnerUserId` với chủ gói và trả cùng một mã 404 cho "không có công trình" và "công trình của người khác", như `SupervisionAssignmentFlow` đang làm. Cổng không nhận hay tin `ownerId` do client gửi.

### Phạm vi xem của khách và nhân viên

Hai nhóm route tách riêng vì hai nhóm người dùng xem dữ liệu khác nhau.

**Route của khách `/api/v1/me/construction-sites`.** Mọi handler đọc dòng `User` của người gọi và kiểm `AccountKind = 'Customer'`. Tài khoản nhân viên, kể cả Admin, nhận 403 `AccessForbidden`, đúng BR-SITE-002 khoản 1–2 và STORY-SITE-002/EXC-03. Đọc từ database chứ không từ token vì token không mang loại tài khoản. Truy vấn luôn có điều kiện `OwnerUserId = người gọi`. Công trình không tồn tại và công trình của khách khác trả cùng 404 `ConstructionSiteNotFound`, cùng một thông báo. Nếu tách thành hai mã, người gọi dò được công trình nào có thật chỉ bằng cách so hai câu trả lời; `SupervisionAssignmentFlow` đang theo đúng cách này với gói.

**Route của nhân viên `/api/v1/admin/construction-sites`.** Policy mặc định cho phép mọi phiên đã xác minh đi tiếp; handler xác định phạm vi từ claim `perm`:

| Quyền của người gọi | Phạm vi | Công trình ngoài phạm vi |
|---|---|---|
| Có `assignment.manage` | Mọi công trình của mọi khách | Không có; công trình không tồn tại trả 404 `ConstructionSiteNotFound` |
| Không có `assignment.manage`, có `supervision.complete` | Công trình của những gói đang được phân công cho người gọi tại lúc đọc | 403 `ConstructionSiteNotInScope`, kể cả khi công trình không tồn tại |
| Không có cả hai | Không có | 403 `AccessForbidden` cho cả danh sách lẫn chi tiết |

Vai trò hệ thống Admin có đủ mọi quyền và không sửa được (BR-RBAC-002), nên Admin luôn rơi vào dòng đầu. Không cần kiểm thêm mã vai trò `admin` như ở luồng hoàn thành gói. Có nhiều quyền thì lấy phạm vi rộng nhất, đúng BR-SITE-003 khoản 4.

Dòng thứ hai trả 403 cho cả công trình không tồn tại, vì BR-RBAC-011 khoản 3 quy định thiếu phân công trả 403, và vì trả 404 riêng cho công trình không tồn tại sẽ để người không phụ trách dò ra công trình nào có thật. ST-SITE-024 kiểm đúng hành vi này.

Phạm vi của dòng thứ hai được tính bằng điều kiện:

```
EXISTS (
  SELECT 1 FROM "Assignment" a
  JOIN "SupervisionGrant" g ON g."Id" = a."ResourceId"
  WHERE a."StaffUserId" = @me
    AND a."ResourceType" = 'SupervisionGrant'
    AND a."EffectiveToUtc" IS NULL
    AND g."ConstructionSiteId" = s."Id")
```

Điều kiện không lọc theo trạng thái gói. Gói đang bị hủy mà phân công còn hiệu lực thì nhân viên vẫn xem được công trình (STORY-SITE-002/ALT-02, BR-RBAC-013 khoản 9). Khi phân công kết thúc hoặc được chuyển giao, dòng phân công có `EffectiveToUtc`, nên yêu cầu xem tiếp theo không còn thấy công trình đó (ST-SITE-026). Giá trị loại tài nguyên `SupervisionGrant` là giá trị dự kiến; tên cuối cùng do TDD-RBAC-003 chốt.

Phạm vi xem theo phân công là trường hợp riêng mà BR-RBAC-010/Notes đã ghi: `supervision.complete` là quyền thao tác, không phải quyền xem, nên phạm vi đọc công trình của người chỉ có quyền này bị giới hạn theo gói được giao.

### Sửa có kiểm version

Khách gửi tên, địa chỉ và `expectedVersion`. Handler đọc công trình của khách, so `Version` với `expectedVersion`; lệch thì trả 409 `ConstructionSiteVersionConflict`. Khớp thì gán giá trị mới, tính lại `NormalizedName`, tăng `Version` và đặt `UpdatedAtUtc`. `Version` được cấu hình là concurrency token của EF Core, nên câu `UPDATE` có thêm điều kiện `"Version" = @old`. Nếu một yêu cầu khác đã sửa giữa lúc đọc và lúc lưu, `UPDATE` không khớp dòng nào, EF ném `DbUpdateConcurrencyException`, và handler đổi thành cùng mã 409.

Kiểm version giúp tránh mất dữ liệu khi khách mở hai màn hình: màn hình A sửa địa chỉ, màn hình B vẫn giữ địa chỉ cũ rồi sửa tên. Không có version thì lần lưu của B ghi đè địa chỉ A vừa sửa. Sửa không đụng tới gói hay phân công (BR-SITE-002 khoản 3).

### Không dùng khóa chống gửi lặp cho các thao tác của khách

Các thao tác gói hiện đòi header `Idempotency-Key` và lưu biên nhận vì gửi lặp có thể gán hoặc hủy gói hai lần. Với công trình, gửi lặp không gây hại dữ liệu:

- Tạo lặp cùng tên: lần sau bị index trùng tên chặn, trả 409 `ConstructionSiteNameTaken`; client tải lại danh sách để thấy công trình đã tạo.
- Sửa lặp: lần sau có `expectedVersion` cũ nên trả 409 `ConstructionSiteVersionConflict`; dữ liệu đã đúng theo lần đầu.
- Xóa lặp: lần sau trả 404 `ConstructionSiteNotFound`.

Đánh đổi là client phải hiểu ba phản hồi này sau khi mất kết nối, thay vì nhận lại đúng phản hồi lần đầu. Nếu sau này cần phản hồi y hệt, có thể thêm `Idempotency-Key` mà không đổi schema công trình.

### Nơi từng Business Rule được thực hiện

| Quy tắc | Nơi thực hiện | Đặc tả Unit Test |
|---|---|---|
| BR-SITE-001 khoản 1–3 | `ConstructionSiteText` (NFC, trim) dùng trong validator 422 và trong phương thức đặt tên của entity; CHECK không rỗng trong database | UT-SITE-001 đến UT-SITE-008 |
| BR-SITE-001 khoản 4 | Kiểm trước ở handler tạo và sửa; index `UX_ConstructionSite_OwnerNormalizedName`; ánh xạ `23505` theo tên index | UT-SITE-002, UT-SITE-011, UT-SITE-013, UT-SITE-018 |
| BR-SITE-001 khoản 5–6, Except | Không có kiểm số lượng; xóa cứng nên tên cũ dùng lại được; lỗi ném exception để transaction rollback | UT-SITE-009; tạo lại tên đã xóa kiểm ở ST-SITE-017 |
| BR-SITE-002 khoản 1–2 | Handler khách kiểm `User.AccountKind = 'Customer'` từ database; `OwnerUserId` lấy từ phiên, không nhận từ body; route nhân viên không có thao tác ghi | UT-SITE-009, UT-SITE-010, UT-SITE-015 |
| BR-SITE-002 khoản 3 | Handler sửa chỉ đổi `Name`, `NormalizedName`, `Address`, `Version`, `UpdatedAtUtc` | UT-SITE-012, UT-SITE-014 |
| BR-SITE-002 khoản 4–5 | Handler xóa khóa dòng rồi kiểm `SupervisionGrant` tham chiếu; khóa ngoại ghép `RESTRICT` từ `SupervisionGrant(ConstructionSiteId, AccountId)`; ánh xạ `23503` theo lệnh xóa | UT-SITE-016, UT-SITE-017, UT-SITE-019 |
| BR-SITE-003 khoản 1 | Query của khách lọc `OwnerUserId`; DTO của khách không có trường nhân viên | UT-SITE-020 |
| BR-SITE-003 khoản 2–5 | Bảng phạm vi ở mục trên, xác định từ claim `perm` trong query của nhân viên | UT-SITE-021 đến UT-SITE-024 |
| BR-SITE-003 khoản 6 | Route nhân viên chỉ có `GET` | UT-SITE-022 |
| BR-SITE-003 khoản 7, BR-RBAC-011 | 404 chung cho khách; 403 chung cho nhân viên ngoài phạm vi; thông báo lỗi không chứa dữ liệu công trình | UT-SITE-015, UT-SITE-023 |
| BR-SITE-003/Except, BR-PAY-005 | Không cài ở đây: màn hình tra cứu gói của TDD-PAY-002 đọc `ConstructionSite.Name` qua join; quyền `commerce.read` không mở route công trình | UT-SITE-021 (commerce.read không có phạm vi xem) |
| BR-SUB-022, BR-SUB-006 | Không cài ở đây: luồng gán ở TDD-SUB-004 dùng `IConstructionSiteOwnershipReader`; ràng buộc `AK_ConstructionSite_Id_OwnerUserId` làm đích cho khóa ngoại ghép chặn gói trỏ vào công trình khác chủ | UT-SITE-019, UT-SITE-025 |

**Notes**:
- Công trình nằm trong cùng database và cùng monolith với gói và phân công, nên đọc chéo bằng join và cùng transaction, không qua message bus. Bảng `ConstructionSite` chỉ được ghi bởi các handler của tính năng này; tính năng khác chỉ đọc hoặc khóa qua cổng.
- Không thêm policy "có một trong hai quyền". Phạm vi xem phụ thuộc quyền nào người gọi có, nên phải quyết định trong handler; một policy chỉ cho biết đạt hay không.
- Các tên lớp, route và mã lỗi ở tài liệu này là thiết kế dự kiến, chưa có trong code.

## Sequence Diagram

Khách tạo công trình, rồi yêu cầu xóa công trình chạy cùng lúc với yêu cầu gắn gói vào chính công trình đó (nhánh gắn gói chạy trước).

```mermaid
sequenceDiagram
    actor KH as Khach U1
    participant API as ConstructionSiteApi
    participant CH as Create handler
    participant DH as Delete handler
    participant GA as SupervisionAssignmentFlow
    participant OR as IConstructionSiteOwnershipReader
    participant PG as PostgreSQL

    KH->>API: POST /me/construction-sites {name, address}
    API->>CH: CreateConstructionSiteCommand
    CH->>PG: Doc User cua nguoi goi, kiem AccountKind = Customer
    CH->>PG: Co cong trinh khac cua U1 cung NormalizedName khong
    alt Da co
        CH-->>KH: 409 ConstructionSiteNameTaken
    else Chua co
        CH->>PG: INSERT ConstructionSite (Version = 1)
        Note over CH,PG: Index trung ten chan yeu cau song song, anh xa 23505
        CH-->>KH: 201 {constructionSiteId, version}
    end

    par Gan goi G4 vao B
        GA->>PG: Khoa AccountCommerceState cua chu goi
        GA->>OR: LockOwnerAccountIdAsync(B)
        OR->>PG: SELECT OwnerUserId ... FOR KEY SHARE
        GA->>PG: Khoa SupervisionGrant G4 FOR UPDATE
        GA->>PG: UPDATE SupervisionGrant SET ConstructionSiteId = B, ghi bien nhan
        GA->>PG: COMMIT
    and Xoa B
        KH->>DH: DELETE /me/construction-sites/B
        DH->>PG: Doc B cua U1, khoa FOR UPDATE (cho khoa cua luong gan)
        DH->>PG: Co SupervisionGrant nao tro vao B khong
        DH-->>KH: 409 ConstructionSiteInUse
    end
```

## Activity Diagram

Xử lý yêu cầu xóa của khách và yêu cầu xem chi tiết của nhân viên.

```mermaid
flowchart TD
    A[DELETE /me/construction-sites/id] --> B{Nguoi goi la tai khoan Customer?}
    B -->|Khong| B1[403 AccessForbidden]
    B -->|Co| C{Cong trinh ton tai va thuoc nguoi goi?}
    C -->|Khong| C1[404 ConstructionSiteNotFound]
    C -->|Co| D[Khoa dong cong trinh FOR UPDATE]
    D --> E{Co SupervisionGrant tro vao cong trinh?}
    E -->|Co| E1[409 ConstructionSiteInUse]
    E -->|Khong| F[DELETE va commit]
    F -->|Loi 23503 tu khoa ngoai| E1
    F -->|Thanh cong| G[204]

    H[GET /admin/construction-sites/id] --> I{Co assignment.manage?}
    I -->|Co| J{Cong trinh ton tai?}
    J -->|Khong| J1[404 ConstructionSiteNotFound]
    J -->|Co| K[Tra chi tiet, chu so huu va cac goi]
    I -->|Khong| L{Co supervision.complete?}
    L -->|Khong| L1[403 AccessForbidden]
    L -->|Co| M{Co goi tren cong trinh dang phan cong cho nguoi goi?}
    M -->|Khong, ke ca cong trinh khong ton tai| M1[403 ConstructionSiteNotInScope]
    M -->|Co| K
```

## State Diagram

Công trình không có cột trạng thái (BR-SITE-001, STORY-SITE-001/Out of Scope). Sơ đồ dưới đây chỉ minh họa điều kiện xóa, được suy ra khi đọc từ việc có hay không có dòng `SupervisionGrant` trỏ vào công trình. Không lưu hai trạng thái này thành dữ liệu.

```mermaid
stateDiagram-v2
    [*] --> ChuaTungCoGoi: Khach tao cong trinh
    ChuaTungCoGoi --> ChuaTungCoGoi: Khach sua ten hoac dia chi
    ChuaTungCoGoi --> DaTungCoGoi: Goi giam sat duoc gan lan dau
    DaTungCoGoi --> DaTungCoGoi: Khach sua ten hoac dia chi, goi doi trang thai
    ChuaTungCoGoi --> [*]: Khach xoa
```

`DaTungCoGoi` không có đường ra: gói đã gắn không đổi công trình (BR-SUB-009), và hủy gói vẫn giữ `ConstructionSiteId`, nên dòng tham chiếu không bao giờ biến mất và công trình không bao giờ xóa được nữa. Gói đi qua các trạng thái đã gán, đã hoàn thành, đang bị hủy mà không ảnh hưởng điều này.

## Data Model

### ConstructionSite — bảng mới

Bảng lưu các công trình khách tự tạo. **Một dòng là một công trình của một khách**, ví dụ "Nhà phố Quận 7" của khách U1. Dòng được tạo khi khách gọi API tạo, được cập nhật khi khách sửa tên hoặc địa chỉ, và bị xóa hẳn khi khách xóa công trình chưa từng có gói.

- `Id`, `OwnerUserId`, `Name`, `Address` là dữ kiện gốc do khách nhập hoặc do hệ thống cấp. `OwnerUserId` lấy từ phiên của người tạo và không bao giờ đổi.
- `NormalizedName` là giá trị suy ra từ `Name`, lưu lại chỉ để kiểm trùng bằng index. Không hiển thị cột này.
- `Version` tăng mỗi lần sửa, dùng để phát hiện hai lần sửa chồng nhau.
- Danh sách gói của công trình không lưu ở bảng này. Khi đọc, hệ thống lấy từ `SupervisionGrant` theo `ConstructionSiteId`.

| Cột | Kiểu | Ràng buộc | Ý nghĩa |
|---|---|---|---|
| Id | uuid | PK, ứng dụng sinh | Định danh công trình |
| OwnerUserId | uuid | NOT NULL, FK `User(Id)` ON DELETE RESTRICT | Khách sở hữu. Handler bảo đảm đây là tài khoản `Customer`; database không kiểm được điều kiện này vì nó nằm ở bảng khác |
| Name | varchar(200) | NOT NULL, CHECK có ký tự khác khoảng trắng | Tên đã chuẩn NFC và bỏ khoảng trắng đầu/cuối |
| NormalizedName | varchar(200) | NOT NULL, CHECK có ký tự khác khoảng trắng | `Name` viết hoa bằng `ToUpperInvariant()` |
| Address | varchar(500) | NOT NULL, CHECK có ký tự khác khoảng trắng | Địa chỉ ô chữ tự do đã chuẩn NFC và bỏ khoảng trắng đầu/cuối |
| CreatedAtUtc | timestamptz | NOT NULL | Lúc tạo, UTC |
| UpdatedAtUtc | timestamptz | NOT NULL, CHECK `UpdatedAtUtc >= CreatedAtUtc` | Lúc sửa gần nhất; bằng lúc tạo khi chưa sửa |
| Version | bigint | NOT NULL DEFAULT 1, CHECK `Version >= 1`, concurrency token | Tăng 1 mỗi lần sửa |

Index:

| Index | Cột | Phục vụ |
|---|---|---|
| `AK_ConstructionSite_Id_OwnerUserId` | `(Id, OwnerUserId)` UNIQUE, ràng buộc khóa thay thế | Đích của khóa ngoại ghép `FK_SupervisionGrant_ConstructionSite` từ `SupervisionGrant(ConstructionSiteId, AccountId)` |
| `UX_ConstructionSite_OwnerNormalizedName` | `(OwnerUserId, NormalizedName)` UNIQUE | BR-SITE-001 khoản 4; cũng là index cho khóa ngoại `OwnerUserId` nhờ cột đầu |
| `IX_ConstructionSite_Owner_CreatedAt` | `(OwnerUserId, CreatedAtUtc, Id)` | Danh sách của khách, sắp mới nhất trước |
| `IX_ConstructionSite_CreatedAt` | `(CreatedAtUtc, Id)` | Danh sách mọi công trình cho người có `assignment.manage` |

Không có cột xóa mềm. Entity kế thừa `Entity<Guid>` và cấu hình `Ignore(x => x.IsDeleted)` như các bảng mới khác.

### Bảng dùng lại và phần thay đổi thuộc tài liệu khác

- `SupervisionGrant` ([TDD-SUB-004](TDD-SUB-004.md#data-model)): một dòng là một quyền dùng gói giám sát. Cặp `(ConstructionSiteId, AccountId)` (code hiện là `ProjectId`, chưa có khóa ngoại) trỏ tới `ConstructionSite(Id, OwnerUserId)` bằng `FK_SupervisionGrant_ConstructionSite` với `ON DELETE RESTRICT`, có index `IX_SupervisionGrant_ConstructionSiteId_AccountId`. `ConstructionSiteId` NULL khi gói chưa gán; hủy gói vẫn giữ giá trị này. Quan hệ: một công trình có nhiều gói theo thời gian, nhưng tối đa một gói giữ chỗ cùng lúc theo `UX_SupervisionGrant_ConstructionSiteHolder`.
- `SupervisionAssignmentEvent`: bị bỏ trong migration của [TDD-SUB-004](TDD-SUB-004.md#data-model); không được dùng làm căn cứ ở tài liệu này.
- `Assignment` ([TDD-RBAC-003](TDD-RBAC-003.md#data-model)): một dòng là một khoảng thời gian một nhân viên phụ trách một gói. Bảng này không trỏ trực tiếp tới công trình; phạm vi xem của nhân viên đi qua gói.
- `User`: chủ công trình. Không thêm cột.

### Dữ liệu mẫu

Dữ liệu giả định, không phải dữ liệu production và không phải bản ghi đầy đủ; chỉ trích các cột cần giải thích. U1, U2 là khách hàng; NV1 là nhân viên chỉ có `supervision.complete`; QL1 là nhân viên có `assignment.manage`; CS1, CS2, CS3, G1, A1 là bí danh UUID. Mọi giờ lưu là UTC.

| Bước | Bảng | Dữ liệu | Giải thích |
|---|---|---|---|
| 1 | ConstructionSite | Id=CS1; OwnerUserId=U1; Name=Nhà phố Quận 7; NormalizedName=NHÀ PHỐ QUẬN 7; Address=12 Nguyễn Thị Thập, Quận 7, TP.HCM; CreatedAtUtc=2026-10-01T02:00:00Z; UpdatedAtUtc=2026-10-01T02:00:00Z; Version=1 | U1 tạo công trình đầu tiên, không cần gói nào. |
| 2 | ConstructionSite | Id=CS2; OwnerUserId=U1; Name=Nhà vườn; NormalizedName=NHÀ VƯỜN; Address=Củ Chi, TP.HCM; CreatedAtUtc=2026-10-01T02:05:00Z; UpdatedAtUtc=2026-10-01T02:05:00Z; Version=1 | U1 gửi "  Nhà vườn  "; hệ thống lưu bản đã bỏ khoảng trắng đầu và cuối. |
| 3 | ConstructionSite | Id=CS3; OwnerUserId=U2; Name=Nhà phố Quận 7; NormalizedName=NHÀ PHỐ QUẬN 7; Version=1 | U2 được đặt trùng tên với CS1 vì khác chủ. |
| 4 | (không ghi) | U1 tạo " nhà phố quận 7 " | Sau chuẩn hóa ra NHÀ PHỐ QUẬN 7, trùng CS1 cùng chủ U1: trả 409 `ConstructionSiteNameTaken`, không có dòng mới. |
| 5 | SupervisionGrant | Id=G1; AccountId=U1; State=Assigned; ConstructionSiteId=CS1 | Luồng gán của TDD-SUB-004 gắn G1 vào CS1; từ đây CS1 "đã từng có gói". |
| 6 | Assignment | Id=A1; StaffUserId=NV1; ResourceType=SupervisionGrant; ResourceId=G1; EffectiveToUtc=NULL | Người quản trị giao G1 cho NV1 theo TDD-RBAC-003. |
| 7 | ConstructionSite | Id=CS1; Name=Nhà phố mới; NormalizedName=NHÀ PHỐ MỚI; UpdatedAtUtc=2026-10-05T03:00:00Z; Version=2 | U1 đổi tên với expectedVersion=1. G1 vẫn trỏ CS1 và A1 không đổi. |
| 8 | ConstructionSite | Dòng CS2 bị xóa | CS2 chưa từng có gói nên xóa được; U1 tạo lại "Nhà vườn" sau đó cũng được. |
| 9 | (không ghi) | U1 xóa CS1 | G1 trỏ CS1 nên trả 409 `ConstructionSiteInUse`. Dù G1 bị hủy sau này, CS1 vẫn không xóa được. |

Kết quả đọc, không phải dữ liệu lưu:

- U1 xem danh sách: thấy CS1 kèm G1 trạng thái `Assigned` và tên gói lấy từ `PlanRevision.Name` của G1; không thấy CS3 và không thấy NV1.
- NV1 xem danh sách qua route nhân viên: chỉ thấy CS1, vì A1 còn hiệu lực và G1 trỏ CS1. NV1 mở CS3 thì nhận 403 `ConstructionSiteNotInScope`.
- QL1 xem danh sách: thấy CS1 và CS3, mỗi công trình kèm chủ sở hữu và các gói.

### Sơ đồ quan hệ

```mermaid
erDiagram
    User ||--o{ ConstructionSite : "so huu"
    ConstructionSite |o--o{ SupervisionGrant : "duoc gan goi, FK ghep"
    User ||--o{ SupervisionGrant : "so huu goi"
    SupervisionGrant ||..o{ Assignment : "phan cong, khong co FK"
    ConstructionSite {
        uuid Id PK
        uuid OwnerUserId FK
        varchar Name
        varchar NormalizedName
        varchar Address
        timestamptz CreatedAtUtc
        timestamptz UpdatedAtUtc
        bigint Version
    }
    SupervisionGrant {
        uuid Id PK
        uuid AccountId FK "cung la cot thu hai cua FK ghep"
        uuid ConstructionSiteId FK "NULL khi chua gan"
        varchar State
    }
    Assignment {
        uuid Id PK
        uuid StaffUserId FK
        varchar ResourceType
        uuid ResourceId
        timestamptz EffectiveToUtc "NULL khi con hieu luc"
    }
```

| Cha | Con | Bản số | Cột khóa ngoại | Con có thể không có cha | Khi xóa cha | Quy tắc |
|---|---|---|---|---|---|---|
| User | ConstructionSite | 1 – 0..n | ConstructionSite.OwnerUserId | Không | RESTRICT | BR-SITE-002 khoản 1 |
| ConstructionSite | SupervisionGrant | 0..1 – 0..n | `(ConstructionSiteId, AccountId)` → `(Id, OwnerUserId)` | Có, khi gói chưa gán (`MATCH SIMPLE` bỏ qua dòng có cột NULL) | RESTRICT | BR-SITE-002 khoản 4, BR-SUB-009; công trình phải cùng chủ với gói |
| SupervisionGrant | Assignment | 1 – 0..n (logic) | Assignment.ResourceId khi ResourceType là gói | Không có FK | Không áp dụng; gói không bị xóa | BR-RBAC-013 |

Mermaid ER không biểu diễn được khóa ngoại ghép, hành vi khi xóa và index có lọc; bảng trên và mục Index là nguồn chính cho hai phần này.

**Notes**:

- **DDL dự kiến, chưa chạy:**

```sql
CREATE TABLE "ConstructionSite" (
    "Id" uuid PRIMARY KEY,
    "OwnerUserId" uuid NOT NULL REFERENCES "User"("Id") ON DELETE RESTRICT,
    "Name" varchar(200) NOT NULL,
    "NormalizedName" varchar(200) NOT NULL,
    "Address" varchar(500) NOT NULL,
    "CreatedAtUtc" timestamptz NOT NULL,
    "UpdatedAtUtc" timestamptz NOT NULL,
    "Version" bigint NOT NULL DEFAULT 1,
    CONSTRAINT "CK_ConstructionSite_Name" CHECK ("Name" ~ '[^[:space:]]'),
    CONSTRAINT "CK_ConstructionSite_NormalizedName" CHECK ("NormalizedName" ~ '[^[:space:]]'),
    CONSTRAINT "CK_ConstructionSite_Address" CHECK ("Address" ~ '[^[:space:]]'),
    CONSTRAINT "CK_ConstructionSite_Version" CHECK ("Version" >= 1),
    CONSTRAINT "CK_ConstructionSite_UpdatedAt" CHECK ("UpdatedAtUtc" >= "CreatedAtUtc"),
    CONSTRAINT "AK_ConstructionSite_Id_OwnerUserId" UNIQUE ("Id", "OwnerUserId")
);
CREATE UNIQUE INDEX "UX_ConstructionSite_OwnerNormalizedName"
    ON "ConstructionSite" ("OwnerUserId", "NormalizedName");
CREATE INDEX "IX_ConstructionSite_Owner_CreatedAt"
    ON "ConstructionSite" ("OwnerUserId", "CreatedAtUtc", "Id");
CREATE INDEX "IX_ConstructionSite_CreatedAt"
    ON "ConstructionSite" ("CreatedAtUtc", "Id");
```

- CHECK không rỗng dùng biểu thức so khớp `~ '[^[:space:]]'` như `CK_SupervisionAssignmentEvent_Reason` trong code hiện tại, vì `btrim` mặc định chỉ cắt dấu cách nên chuỗi toàn tab vẫn lọt. Database không kiểm được "đã bỏ khoảng trắng đầu/cuối" theo đúng định nghĩa của .NET; phần đó do `ConstructionSiteText` bảo đảm.
- Độ dài: `string.Length` của .NET đếm đơn vị UTF-16, còn `varchar(n)` của PostgreSQL đếm ký tự. Chữ tiếng Việt sau NFC là một đơn vị UTF-16 nên hai cách đếm trùng nhau; với emoji, .NET đếm 2 còn PostgreSQL đếm 1. Validator vì vậy chặt hơn database, không có trường hợp validator cho qua mà database từ chối.
- `AK_ConstructionSite_Id_OwnerUserId` khai báo bằng `HasAlternateKey(x => new { x.Id, x.OwnerUserId })`, tên theo quy ước `AK_<Bảng>_<Cột>` mà EF đang sinh cho `AK_PlanRevision_Id_Kind`. Ràng buộc này dư về mặt duy nhất (đã có khóa chính `Id`) nhưng bắt buộc để khóa ngoại ghép trỏ được, vì PostgreSQL chỉ cho khóa ngoại trỏ tới khóa chính hoặc ràng buộc duy nhất khớp đúng các cột.
- **Rà soát chuẩn hóa:** khóa ứng viên là `{Id}` và `{OwnerUserId, NormalizedName}`. `NormalizedName` phụ thuộc hàm vào `Name`, một cột không phải khóa, nên về hình thức là phụ thuộc bắc cầu vi phạm 3NF. Đây là dư thừa có chủ đích để index kiểm trùng dùng đúng hàm của ứng dụng, với một đường ghi duy nhất ở entity; cùng cách với `User.NormalizedFirstName`. Không có cờ "đã từng có gói" hay số gói trên công trình, nên không có sự thật nào bị lưu hai nơi.
- **Kế hoạch migration** (chưa tạo, chưa chạy). Thứ tự thống nhất với [TDD-SUB-004](TDD-SUB-004.md#data-model); có thể gộp thành một migration nếu giữ đúng thứ tự:
  1. Migration của tài liệu này tạo bảng `ConstructionSite` cùng CHECK, `AK_ConstructionSite_Id_OwnerUserId`, `UX_ConstructionSite_OwnerNormalizedName` và hai index danh sách. Bảng mới nên không cần kiểm dữ liệu trước. Down: xóa bảng.
  2. Migration của TDD-SUB-004 chạy sau: kiểm trước `SELECT count(*) FROM "SupervisionGrant" WHERE "ProjectId" IS NOT NULL` và `SELECT count(*) FROM "SupervisionAssignmentEvent"`. Dự kiến cả hai bằng 0 vì endpoint gán luôn trả 503. Nếu khác 0 thì dừng migration; người chạy chọn xóa dữ liệu test đó hoặc dựng lại database dev. Migration không tự xóa dữ liệu.
  3. Vẫn trong migration của TDD-SUB-004: `DROP TABLE "SupervisionAssignmentEvent"`, đổi `ProjectId` thành `ConstructionSiteId`, đổi tên index giữ chỗ, tạo `IX_SupervisionGrant_ConstructionSiteId_AccountId`, rồi thêm `FK_SupervisionGrant_ConstructionSite` tới `ConstructionSite(Id, OwnerUserId)` với `ON DELETE RESTRICT`. Khóa ngoại cần bảng và ràng buộc của bước 1 nên phải chạy sau bước đó.
  4. Kiểm sau migration: `pg_constraint` có `AK_ConstructionSite_Id_OwnerUserId` và khóa ngoại ghép với `confdeltype = 'r'`; trong môi trường test, xóa một công trình còn gói phải nhận lỗi `23503`, và ghi gói của U1 vào công trình của U2 cũng phải nhận lỗi `23503`.
- Đây là bảng mới trong database chỉ có dữ liệu dev/test (xác nhận ngày 25/09/2026), nên không cần chia lô hay tạo index `CONCURRENTLY`.

## Internal API

### Endpoints

Tất cả route dùng `NewVersionedApi` và `HasApiVersion(1)`, policy mặc định (phiên đã xác minh, không phải phiên quên mật khẩu, không bị bắt đổi mật khẩu). Phản hồi thành công bọc trong `Result` như các API hiện có. Phân trang theo `pageIndex` (từ 1) và `pageSize` (1–100, ngoài khoảng thì dùng 20) là quyết định kỹ thuật; nghiệp vụ chưa chốt phân trang, sắp xếp hay tìm kiếm. Thứ tự mặc định là mới tạo trước (`CreatedAtUtc DESC, Id DESC`).

- **POST** `/api/v1/me/construction-sites` — Khách tạo công trình. Body `{name, address}`. Trả 201 với `{constructionSiteId, name, address, version, createdAtUtc}`. Tài khoản nhân viên nhận 403.
- **GET** `/api/v1/me/construction-sites` — Danh sách công trình của chính khách. Mỗi mục có `constructionSiteId`, `name`, `address`, `version`, `createdAtUtc`, `updatedAtUtc` và `supervisionGrants` gồm mọi gói từng gắn vào công trình, mỗi gói có `grantId`, `planName`, `state` (`Assigned`, `Completed` hoặc `CanceledByStaff`), `firstAssignedAtUtc`. Không có trường nào về nhân viên.
- **GET** `/api/v1/me/construction-sites/{siteId}` — Chi tiết một công trình của khách, cùng dữ liệu như một mục của danh sách.
- **PUT** `/api/v1/me/construction-sites/{siteId}` — Sửa tên và địa chỉ. Body `{name, address, expectedVersion}`; gửi đủ cả hai trường, kể cả khi chỉ đổi một trường. Trả 200 với dữ liệu mới và `version` mới.
- **DELETE** `/api/v1/me/construction-sites/{siteId}` — Xóa công trình chưa từng có gói. Trả 204.
- **GET** `/api/v1/admin/construction-sites` — Danh sách cho nhân viên theo phạm vi quyền. Mỗi mục có thêm `owner` gồm `userId`, `fullName` và `email` (người dùng xác nhận hiện `email` ngày 25/09/2026 vì lịch và lượt giám sát đang làm offline, nhân viên cần cách liên hệ khách), cùng các gói như route của khách. Người có `supervision.complete` mà không có `assignment.manage` chỉ nhận công trình trong phạm vi; không có cả hai quyền thì nhận 403.
- **GET** `/api/v1/admin/construction-sites/{siteId}` — Chi tiết cho nhân viên, cùng dữ liệu như một mục của danh sách nhân viên.

### Examples

#### POST /api/v1/me/construction-sites

```
Request:
{"name":"  Nhà vườn  ","address":"  Củ Chi, TP.HCM  "}

Response 201:
{"value":{"constructionSiteId":"a1a1a1a1-0000-4000-8000-000000000002","name":"Nhà vườn","address":"Củ Chi, TP.HCM","version":1,"createdAtUtc":"2026-10-01T02:05:00+00:00"},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Conflict","code":"ConstructionSiteNameTaken","status":409,"detail":"Bạn đã có một công trình khác cùng tên.","messageCode":"ConstructionSiteNameTaken","errors":null}
```

#### GET /api/v1/me/construction-sites

```
Request:
GET /api/v1/me/construction-sites?pageIndex=1&pageSize=20

Response 200:
{"value":{"items":[{"constructionSiteId":"a1a1a1a1-0000-4000-8000-000000000001","name":"Nhà phố Quận 7","address":"12 Nguyễn Thị Thập, Quận 7, TP.HCM","version":1,"createdAtUtc":"2026-10-01T02:00:00+00:00","updatedAtUtc":"2026-10-01T02:00:00+00:00","supervisionGrants":[{"grantId":"b2b2b2b2-0000-4000-8000-000000000001","planName":"Giám sát cơ bản","state":"Assigned","firstAssignedAtUtc":"2026-10-02T01:00:00+00:00"}]}],"pageIndex":1,"pageSize":20,"totalCount":1,"hasNextPage":false,"hasPreviousPage":false},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Forbidden","code":"AccessForbidden","status":403,"detail":"Chức năng này dành cho tài khoản khách hàng.","messageCode":"AccessForbidden","errors":null}
```

#### PUT /api/v1/me/construction-sites/{siteId}

```
Request:
{"name":"Nhà phố mới","address":"12 Nguyễn Thị Thập, Quận 7, TP.HCM","expectedVersion":1}

Response 200:
{"value":{"constructionSiteId":"a1a1a1a1-0000-4000-8000-000000000001","name":"Nhà phố mới","address":"12 Nguyễn Thị Thập, Quận 7, TP.HCM","version":2,"updatedAtUtc":"2026-10-05T03:00:00+00:00"},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Validation Error","type":"Validation Error","status":422,"detail":"A validation error occured","errors":[{"code":"Name","message":"Tên công trình tối đa 200 ký tự.","messageCode":"ConstructionSiteNameTooLong"}]}
```

#### DELETE /api/v1/me/construction-sites/{siteId}

```
Request:
DELETE /api/v1/me/construction-sites/a1a1a1a1-0000-4000-8000-000000000001

Response 204:
(không có thân phản hồi)

Error Response:
{"title":"Conflict","code":"ConstructionSiteInUse","status":409,"detail":"Công trình đã từng có gói giám sát gắn vào nên không xóa được.","messageCode":"ConstructionSiteInUse","errors":null}
```

#### GET /api/v1/admin/construction-sites/{siteId}

```
Request:
GET /api/v1/admin/construction-sites/a1a1a1a1-0000-4000-8000-000000000001

Response 200:
{"value":{"constructionSiteId":"a1a1a1a1-0000-4000-8000-000000000001","name":"Nhà phố mới","address":"12 Nguyễn Thị Thập, Quận 7, TP.HCM","version":2,"createdAtUtc":"2026-10-01T02:00:00+00:00","updatedAtUtc":"2026-10-05T03:00:00+00:00","owner":{"userId":"c3c3c3c3-0000-4000-8000-000000000001","fullName":"Khách U1","email":"u1@example.test"},"supervisionGrants":[{"grantId":"b2b2b2b2-0000-4000-8000-000000000001","planName":"Giám sát cơ bản","state":"Assigned","firstAssignedAtUtc":"2026-10-02T01:00:00+00:00"}]},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Forbidden","code":"ConstructionSiteNotInScope","status":403,"detail":"Bạn không phụ trách gói nào trên công trình này.","messageCode":"ConstructionSiteNotInScope","errors":null}
```

Ví dụ dùng UUID và dữ liệu giả định; tên gói "Giám sát cơ bản" chỉ minh họa, không phải danh mục gói thật.

### Error Codes

- **Unauthorized** (401): Phiên thiếu hoặc không hợp lệ; dùng cơ chế xác thực hiện có.
- **AccessForbidden** (403): Tài khoản nhân viên gọi route của khách; hoặc người gọi route nhân viên không có `assignment.manage` lẫn `supervision.complete`.
- **ConstructionSiteNotInScope** (403): Người chỉ có `supervision.complete` xem công trình không có gói nào đang được phân công cho mình, kể cả khi công trình không tồn tại.
- **ConstructionSiteNotFound** (404): Route của khách: công trình không tồn tại hoặc thuộc khách khác, cùng một thông báo. Route nhân viên với `assignment.manage`: công trình không tồn tại. Mã này dùng chung với luồng gán gói của TDD-SUB-004, thay `ProjectNotFound`.
- **ConstructionSiteNameTaken** (409): Khách đã có công trình khác cùng tên sau chuẩn hóa, gồm cả trường hợp index `UX_ConstructionSite_OwnerNormalizedName` chặn yêu cầu song song.
- **ConstructionSiteVersionConflict** (409): `expectedVersion` khác `Version` hiện tại, hoặc công trình vừa được sửa bởi yêu cầu khác trước lúc lưu.
- **ConstructionSiteInUse** (409): Xóa công trình đã từng có gói giám sát gắn vào, gồm cả trường hợp khóa ngoại `RESTRICT` chặn câu `DELETE`.
- **ConstructionSiteNameRequired** (422): Tên trống hoặc chỉ có khoảng trắng.
- **ConstructionSiteNameTooLong** (422): Tên dài hơn 200 ký tự sau chuẩn hóa.
- **ConstructionSiteAddressRequired** (422): Địa chỉ trống hoặc chỉ có khoảng trắng.
- **ConstructionSiteAddressTooLong** (422): Địa chỉ dài hơn 500 ký tự sau chuẩn hóa.
- **ConstructionSiteVersionRequired** (422): `expectedVersion` thiếu hoặc nhỏ hơn 1 khi sửa.

Các mã 422 là `messageCode` của từng lỗi trong mảng `errors`, do validator trả qua `ValidationPipelineBehavior`. Các mã 403, 404, 409 là mã mới dự kiến trong `ConstructionSiteErrorCodes`, trừ `AccessForbidden` đã có ở cơ chế phân quyền.

## References

### User Stories

- STORY-SITE-001
- STORY-SITE-001/AC-001
- STORY-SITE-001/AC-002
- STORY-SITE-001/AC-003
- STORY-SITE-001/AC-004
- STORY-SITE-001/AC-005
- STORY-SITE-001/AC-006
- STORY-SITE-001/AC-007
- STORY-SITE-001/AC-008
- STORY-SITE-001/AC-009
- STORY-SITE-001/AC-010
- STORY-SITE-001/AC-011
- STORY-SITE-001/AC-012
- STORY-SITE-001/AC-013
- STORY-SITE-001/AC-014
- STORY-SITE-001/AC-015
- STORY-SITE-001/AC-016
- STORY-SITE-002
- STORY-SITE-002/AC-001
- STORY-SITE-002/AC-002
- STORY-SITE-002/AC-003
- STORY-SITE-002/AC-004
- STORY-SITE-002/AC-005
- STORY-SITE-002/AC-006
- STORY-SITE-002/AC-007
- STORY-SITE-002/AC-008
- STORY-SITE-002/AC-009

### Business Rules

- BR-SITE-001/Then
- BR-SITE-001/Except
- BR-SITE-002/Then
- BR-SITE-002/Except
- BR-SITE-003/Then
- BR-SITE-003/Except
- BR-RBAC-005/Then
- BR-RBAC-010/Then
- BR-RBAC-011/Then
- BR-RBAC-013/Then
- BR-SUB-006/Then
- BR-SUB-009/Except
- BR-SUB-022/Then

### Use Cases

- STORY-SITE-001/Main Flow
- STORY-SITE-001/ALT-01
- STORY-SITE-001/ALT-02
- STORY-SITE-001/ALT-03
- STORY-SITE-001/EXC-01
- STORY-SITE-001/EXC-02
- STORY-SITE-001/EXC-03
- STORY-SITE-001/EXC-04
- STORY-SITE-002/Main Flow
- STORY-SITE-002/ALT-01
- STORY-SITE-002/ALT-02
- STORY-SITE-002/EXC-01
- STORY-SITE-002/EXC-02
- STORY-SITE-002/EXC-03

### Others

- Unit test: đặc tả UT-SITE-001 đến UT-SITE-025, gồm `ConstructionSiteText` và validator (UT-SITE-001 đến UT-SITE-008); handler tạo, sửa, xóa với repository giả (UT-SITE-009 đến UT-SITE-017); ánh xạ `23505`, `23503` theo tên ràng buộc và lệnh đang chạy (UT-SITE-018, UT-SITE-019); query của khách (UT-SITE-020); phạm vi xem của nhân viên (UT-SITE-021 đến UT-SITE-024); cổng đọc chủ công trình (UT-SITE-025). Đặc tả chưa thực thi, chưa có mã test. Unit test không chứng minh hành vi của index, khóa ngoại và khóa dòng PostgreSQL; các phần đó thuộc integration test và System Test bên dưới.
- Integration test với PostgreSQL thật, không dùng EF InMemory: index trùng tên với hai connection chèn song song; khóa ngoại ghép `RESTRICT` chặn xóa công trình còn gói và chặn gói trỏ vào công trình khác chủ, cùng cách ánh xạ `23503` theo lệnh đang chạy; tranh chấp xóa và gắn gói theo cả hai thứ tự; `FOR KEY SHARE` không chặn sửa địa chỉ; concurrency token của `Version`; truy vấn phạm vi của nhân viên trên dữ liệu `Assignment` thật.
- System test: ST-SITE-001 đến ST-SITE-018 cho STORY-SITE-001; ST-SITE-019 (tạo trùng tên đồng thời) và ST-SITE-020 (xóa và gắn gói đồng thời); ST-SITE-021 đến ST-SITE-029 cho STORY-SITE-002.
- Tài liệu liên quan: [TDD-SUB-004](TDD-SUB-004.md) cho gán gói và đổi tên cột, [TDD-RBAC-003](TDD-RBAC-003.md) cho phân công theo gói và cơ chế ánh xạ lỗi theo tên ràng buộc, [TDD-PAY-002](TDD-PAY-002.md) cho màn hình tra cứu gói hiện tên công trình.
- Không gọi dịch vụ bên ngoài nên không có External API trong TDD này.

## Change Log
