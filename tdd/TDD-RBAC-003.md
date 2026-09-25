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

# TDD-RBAC-003

## Document Info

- **Feature**: Phân công gói giám sát cho nhân viên, chuyển giao và điều kiện sửa hẹp
- **Author**: Claude
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

`BR-SUB-011` và `BR-SUB-012` chỉ cho Admin hoặc nhân viên **đang phụ trách gói giám sát** hoàn thành hoặc mở lại gói đó. Vòng đời gói giám sát đã cấp nằm ở [TDD-SUB-004](TDD-SUB-004.md) (gán gói vào công trình), [TDD-SUB-005](TDD-SUB-005.md) (hủy, khôi phục) và [TDD-SUB-006](TDD-SUB-006.md) (hoàn thành, mở lại và đọc phân công). TDD-SUB-003 đã bị thay thế, chỉ giữ để tra cứu.

`STORY-RBAC-003` chốt mô hình chung: phân công ghi theo loại tài nguyên, để sau này thêm lead mà không phải dựng cơ chế thứ hai. Theo quyết định người dùng xác nhận ngày 25/09/2026, đợt này chỉ có **một loại tài nguyên là gói giám sát**; không có phân công mức khách hàng hay mức công trình. Gói gắn cố định với một công trình theo `BR-SUB-009`, nên người phụ trách gói cũng là người lo công trình đó trong thời gian gói còn ở đó. Một bản thiết kế trước trong cùng ngày chọn phân công theo công trình; bản đó đã được thay khi chuẩn bị tính năng Công trình (`STORY-SITE-001`, `STORY-SITE-002`).

Các yêu cầu khó của thiết kế:

- Mỗi gói tại một thời điểm chỉ có một phân công đang hiệu lực. Giao một gói đang có người cho người khác phải bị từ chối và hướng sang chuyển giao (`BR-RBAC-013` khoản 4), kể cả khi hai yêu cầu giao chạy song song.
- Chỉ giao hoặc chuyển giao được gói đang giữ chỗ trên công trình, tức gói đã gán hoặc đã hoàn thành (`BR-RBAC-013` khoản 8), kể cả khi việc hủy gói chạy song song với việc giao.
- Người nhận khi giao và khi chuyển giao phải là nhân viên đang hoạt động, không bị khóa và đang có `supervision.complete` (`BR-RBAC-013` khoản 2).
- Hủy và khôi phục gói không đổi phân công (`BR-RBAC-013` khoản 9). Gói mới gắn vào cùng công trình không kế thừa người phụ trách của gói cũ (khoản 3).
- Thu hồi vai trò hoặc bỏ quyền khỏi vai trò không được làm người đang phụ trách gói mất `supervision.complete` (`BR-RBAC-007`). Khóa tài khoản thì không bị chặn, và các gói đang gán của người bị khóa vào danh sách cần chia lại (`BR-RBAC-008` khoản 4, `BR-RBAC-013` khoản 6).

Hiện trạng code trước đợt thay đổi ngày 25/09/2026:

- Bảng `Assignment` được tạo ở migration `20260923152830_InitialRbac`, với CHECK `ResourceType IN ('Customer', 'Project')` và index duy nhất có lọc trên `(StaffUserId, ResourceType, ResourceId)`.
- Bốn endpoint phân công, `AssignmentAuthorizer` (gồm nhánh kế thừa từ khách hàng xuống dự án qua `IResourceHierarchyReader`), bản tạm `UnavailableResourceHierarchyReader` và `AssignmentRowLocker` đã có. `AssignmentRowLocker` khóa dòng phân công bằng `FOR UPDATE` khi chuyển giao và bằng `FOR SHARE` khi kiểm phân công.
- Code chưa kiểm quyền của người nhận, chưa chặn hai người cùng phụ trách một tài nguyên và chưa kiểm tài nguyên có tồn tại hay không.
- Bảng `SupervisionGrant` đã có theo TDD-SUB-004, với cột `State` nhận `Unassigned`, `Assigned`, `CanceledByStaff`, `Completed`. Cột công trình của gói hiện còn tên `ProjectId`. Gói không có đường xóa: nghiệp vụ chỉ hủy, và cấu hình EF bỏ qua `IsDeleted` (`src/bmt-be.persistence/configurations/SupervisionGrantConfiguration.cs`).
- `SupervisionCompletionAccess` gọi `IsDirectlyAssignedAsync` với `ResourceTypes.Project` và `grant.ProjectId`.

Các thay đổi ngày 25/09/2026 dưới đây **đã có trong code**, ở commit `182e2a8` trên nhánh `feature/construction-site` của `bmt-be`, cùng migration `20260925074152_ConstructionSiteAndPackageAssignment`. Chỗ nào code còn khác thiết kế thì ghi ngay tại mục đó.

### Goals

- Một bảng phân công chung dùng được cho nhiều loại tài nguyên, để tính năng chia lead sau này không cần bảng mới. Đợt này bảng chỉ nhận loại `SupervisionGrant`.
- Database bảo đảm mỗi gói có tối đa một phân công đang hiệu lực, kể cả khi hai yêu cầu giao chạy cùng lúc.
- Không tạo được phân công mới cho gói không tồn tại, gói chưa gán công trình hoặc gói đang bị hủy, kể cả khi việc hủy gói chạy song song.
- Trả lời được câu hỏi "người này có đang phụ trách gói kia tại thời điểm thao tác không" bằng một truy vấn dùng index.
- Không để lọt trường hợp gói có người phụ trách nhưng người đó không có `supervision.complete`, dù giao việc, chuyển giao, thu hồi vai trò và sửa quyền vai trò chạy song song.
- Tính được danh sách gói cần chia lại khi đọc, không cần bảng hay cột cờ riêng.
- Chuyển giao và gỡ phân công giữ đủ dấu vết để tra được ai từng phụ trách gói nào.

### Non-goals

- Tính năng chia lead, gồm cả chia thủ công và chia tự động. Tài liệu này chỉ bảo đảm cơ chế dùng lại được.
- Phân công theo khách hàng hoặc theo công trình; đợt này chỉ giao từng gói giám sát.
- Vòng đời gói giám sát: gán vào công trình, hủy, khôi phục, hoàn thành và mở lại thuộc TDD-SUB-004/005/006. Dữ liệu và API của Công trình thuộc thiết kế kỹ thuật của Công trình; tài liệu này chỉ cung cấp truy vấn phạm vi xem theo phân công.
- Tự động chia lại gói khi nhân viên bị khóa, và quy tắc cân bằng số lượng giữa các nhân viên.
- Mô hình vai trò – quyền và vòng đời tài khoản. Xem [TDD-RBAC-001](TDD-RBAC-001.md) và [TDD-RBAC-002](TDD-RBAC-002.md).

## Architecture

Phân công là chặng thứ ba trong ba chặng kiểm tra mô tả ở [TDD-RBAC-001](TDD-RBAC-001.md#architecture). Chặng này nằm trong handler chứ không ở endpoint, vì chỉ trong handler mới biết gói đích là gói nào.

```mermaid
flowchart LR
    H[MediatR handler<br/>hoan thanh, mo lai goi giam sat] --> SV[IAssignmentAuthorizer]
    SV --> PG[(PostgreSQL<br/>Assignment)]
    AD[Nguoi quan tri] --> API[Assignment endpoints]
    API --> AH[MediatR handlers<br/>giao, chuyen giao, go,<br/>danh sach can chia lai]
    AH -->|khoa FOR SHARE<br/>doc trang thai goi| SG[(PostgreSQL<br/>SupervisionGrant)]
    AH -->|khoa dong User<br/>doc quyen nguoi nhan| UR[(PostgreSQL<br/>User UserRole RolePermission)]
    AH --> PG
    AH --> AU[IAccessAuditWriter]
    SC[Handler xem cong trinh<br/>cua nhan vien] -->|cong trinh cua goi<br/>dang phu trach| PG
    SC --> SG
```

### Một bảng cho nhiều loại tài nguyên

Bảng `Assignment` không có khóa ngoại tới tài nguyên. Mỗi dòng mang một cặp `ResourceType` và `ResourceId`. Đợt này `ResourceType` chỉ có một giá trị là `SupervisionGrant`, và `ResourceId` là `SupervisionGrant.Id` của gói được giao. Thêm loại tài nguyên mới, ví dụ lead, chỉ cần một giá trị mới trong `ResourceType`, không cần bảng mới hay cột mới.

Tên `SupervisionGrant` trùng tên thực thể và tên bảng của gói giám sát đã cấp trong code, nên đọc một dòng phân công là biết phải tra bảng nào. Giá trị `ConstructionSite` của bản thiết kế trước trong cùng ngày không còn dùng.

Không có khóa ngoại thì database không tự bảo đảm `ResourceId` trỏ tới một gói có thật. Khác với bản trước, nay handler kiểm được việc này: gói giám sát đã có bảng, nên handler giao việc đọc dòng `SupervisionGrant` dưới khóa và trả 404 `AssignmentResourceNotFound` nếu không có. Gói không bao giờ bị xóa, nên một phân công đã tạo hợp lệ không trở thành dòng mồ côi. [Nợ kỹ thuật](../debt/assignment-resource-check.md) về việc không kiểm tài nguyên tồn tại đóng được khi bước kiểm này được triển khai.

Vẫn không thêm khóa ngoại vì một cột `ResourceId` phục vụ nhiều loại tài nguyên, còn một khóa ngoại chỉ trỏ được tới một bảng. Muốn có khóa ngoại thì phải thêm cột riêng như `SupervisionGrantId` cho từng loại. Chưa làm, vì gói không bị xóa và handler đã kiểm dưới khóa; nên xem lại khi có loại tài nguyên có thể bị xóa.

### Hiệu lực theo thời gian thay vì xóa dòng

Gỡ phân công không xóa dòng mà đặt `EffectiveToUtc`. Chuyển giao là đặt `EffectiveToUtc` cho dòng cũ và chèn dòng mới cho người nhận, tại cùng một mốc thời gian, trong cùng một transaction.

Làm vậy vì `STORY-RBAC-003/Non-Functional` đòi tra được ai từng phụ trách gói nào, và vì `BR-RBAC-008` đòi các phân công của người bị khóa vẫn còn để chia lại. Nếu gỡ bằng cách xóa dòng thì cả hai yêu cầu đều không đáp ứng được. `AccessAuditLog` một mình cũng không đủ, vì nó ghi theo thao tác chứ không trả lời được câu hỏi "trong khoảng thời gian này ai phụ trách gói G".

Một dòng còn hiệu lực khi `EffectiveToUtc IS NULL`. Không dùng `EffectiveToUtc > now()` làm điều kiện chính, vì thiết kế này không có phân công hẹn trước ngày kết thúc; mọi lần kết thúc đều xảy ra ngay tại thời điểm thao tác.

### Một gói, một người phụ trách

`BR-RBAC-013` khoản 4 cho mỗi gói tối đa một phân công đang hiệu lực tại một thời điểm. Thiết kế giữ ràng buộc này ở hai lớp.

1. **Handler giao việc** (`POST /assignments`) kiểm gói đã có dòng đang hiệu lực chưa. Có thì trả 409 `ResourceAlreadyAssigned`, không tạo dòng thứ hai, và phản hồi kèm mã phân công hiện tại để giao diện đưa người quản trị sang chuyển giao (`STORY-RBAC-003/EXC-05`, `AC-004`). Kiểm này áp dụng cả khi người nhận chính là người đang phụ trách.
2. **Index duy nhất có lọc** `UX_Assignment_ActiveResource` trên `(ResourceType, ResourceId) WHERE "EffectiveToUtc" IS NULL` là lớp chặn cuối ở database. "Có lọc" nghĩa là index chỉ chứa các dòng đang hiệu lực, nên một gói vẫn có nhiều dòng đã kết thúc làm lịch sử.

Cần cả hai lớp vì lớp 1 cho mã lỗi rõ nghĩa nhưng không chặn được hai yêu cầu song song. Ví dụ hai người quản trị cùng giao gói G3 cho hai nhân viên khác nhau: cả hai handler đều thấy G3 chưa có ai và cùng cho qua. Khi đó index chặn câu `INSERT` đến sau bằng lỗi PostgreSQL `23505`. Lỗi này có thể ném ra lúc lưu hoặc lúc commit, nên phải bắt ở lớp bao ngoài `TransactionPipelineBehavior`, sau khi transaction đã rollback. Lớp này là `ConstraintViolationPipelineBehavior`, bảng ánh xạ đầy đủ ở [TDD-SITE-001](TDD-SITE-001.md#architecture). Nó nhận đúng tên index `UX_Assignment_ActiveResource` và trả 409 `ResourceAlreadyAssigned` như lớp 1, thay vì mã 409 chung của middleware. Không chạy thêm câu SQL nào trong transaction PostgreSQL đã lỗi.

Index này thay index duy nhất cũ `IX_Assignment_StaffUserId_ResourceType_ResourceId` trên `(StaffUserId, ResourceType, ResourceId)`. Index cũ chỉ chặn một người được giao hai lần cùng một tài nguyên, và cố ý cho nhiều người cùng phụ trách theo bản BR trước; bản BR hiện tại đã bỏ điều đó.

Chuyển giao phải **đóng dòng cũ trước khi chèn dòng mới**. PostgreSQL kiểm index duy nhất ngay ở từng câu lệnh chứ không đợi tới lúc commit. Nếu câu `INSERT` dòng mới chạy trước câu `UPDATE` dòng cũ, chính thao tác chuyển giao hợp lệ sẽ vi phạm index. Handler vì vậy đặt `EffectiveToUtc` cho dòng cũ rồi gọi `SaveChangesAsync`, sau đó mới thêm dòng mới, cả hai trong cùng transaction. Không dựa vào thứ tự câu lệnh do EF Core tự sắp: dòng cũ không đổi giá trị cột của index, nên EF Core không thấy hai câu lệnh phụ thuộc nhau.

Chuyển giao cho chính người đang phụ trách vẫn bị từ chối với `DuplicateAssignment`, đúng `STORY-RBAC-003/EXC-03`.

Phân công gắn vào `Id` của gói, không gắn vào công trình. Vì vậy gói mới gắn vào cùng công trình là một dòng `SupervisionGrant` khác với `Id` khác, và không có sẵn người phụ trách (`BR-RBAC-013` khoản 3, `STORY-RBAC-003/ALT-07`). Ví dụ: gói G1 của công trình P do anh Nam phụ trách bị hủy; khách gắn gói mới G5 vào P. Dòng phân công của anh Nam vẫn trỏ vào G1, còn G5 chưa có dòng nào nên nằm trong danh sách cần chia lại cho tới khi người quản trị giao.

### Chỉ giao gói đang giữ chỗ trên công trình

`BR-RBAC-013` khoản 8 chỉ cho giao mới và chuyển giao gói đang giữ chỗ trên công trình. Handler giao và handler chuyển giao đọc dòng gói bằng câu lệnh có tham số, qua `IAssignmentRowLocker.LockSupervisionGrantStateForShareAsync`:

```sql
SELECT "State" FROM "SupervisionGrant" WHERE "Id" = @grantId FOR SHARE
```

- Không có dòng: trả 404 `AssignmentResourceNotFound`.
- `State` là `Unassigned` hoặc `CanceledByStaff`: trả 409 `ResourceNotAssignable`, không tạo phân công. Gói chưa gán đã quá hạn gán lần đầu vẫn có `State = Unassigned` (trạng thái `ExpiredUnassigned` chỉ tính khi đọc), nên cũng bị từ chối.
- `State` là `Assigned` hoặc `Completed`: đi tiếp. Handler dùng đúng hằng tập giữ chỗ của TDD-SUB-004 (`SupervisionStates.HoldingConstructionSite`) thay vì tự viết lại danh sách, để không lệch với partial unique index của bảng gói.

Dùng 409 vì yêu cầu đúng khuôn và người gọi có quyền, nhưng xung đột với trạng thái hiện tại của gói; cùng yêu cầu đó có thể thành công sau khi gói được khôi phục. Đây cũng là lý do `ResourceAlreadyAssigned` dùng 409. Mã 422 dành cho người nhận không thỏa điều kiện của yêu cầu (`AssigneeLacksPermission`), còn 404 dành cho gói không tồn tại.

**Khóa `FOR SHARE` trên dòng gói** giữ trạng thái gói đứng yên tới hết transaction giao việc. Hủy gói theo TDD-SUB-005 phải `UPDATE` dòng này, và lệnh `UPDATE` phải chờ mọi khóa `FOR SHARE` đang giữ. Hai người quản trị cùng đọc một gói thì không chặn nhau, vì hai khóa `FOR SHARE` không xung đột. Tình huống: chị Lan giao G1 cho anh Tú đúng lúc anh Hùng hủy G1.

- Nếu việc giao lấy khóa trước, việc hủy chờ tới khi giao commit rồi mới đổi `State`. Phân công mới giữ nguyên sau khi hủy theo khoản 9.
- Nếu việc hủy commit trước, câu `FOR SHARE` chờ, rồi ở mức cô lập `READ COMMITTED` đọc lại dòng đã đổi và thấy `CanceledByStaff`, nên việc giao nhận 409.

Không có kết cục nào tạo ra một phân công mới trên gói đã bị hủy trước đó. Khóa `FOR SHARE` không đổi cột `Version` của gói, nên kiểm version của luồng hủy không bị ảnh hưởng.

Gỡ phân công không đọc gói và làm được với gói ở mọi trạng thái (`STORY-RBAC-003/ALT-04`). Hủy và khôi phục gói theo TDD-SUB-005 không đọc hay ghi `Assignment`, nên phân công giữ nguyên qua cả hai thao tác (khoản 9). Khi gói được khôi phục về `Assigned`, gói vào danh sách cần chia lại nếu phân công đã bị gỡ trong lúc gói bị hủy; ngược lại người cũ tiếp tục phụ trách mà không cần giao lại.

### Điều kiện của người nhận

`BR-RBAC-013` khoản 2 áp dụng cho cả giao và chuyển giao: người nhận phải là tài khoản nhân viên đang hoạt động, không bị khóa và đang có `supervision.complete`. Sau khi đã khóa và kiểm dòng gói, handler kiểm theo thứ tự:

1. Khóa dòng `User` của người nhận bằng `SELECT ... FOR NO KEY UPDATE`.
2. Người nhận tồn tại, chưa bị xóa, có `AccountKind = 'Staff'` và `Status = 'Active'`. Sai thì 409 `AssignmentTargetInvalid` (`EXC-01`, `EXC-02`, `AC-006`).
3. Người nhận có `supervision.complete` qua ít nhất một vai trò, đọc bằng truy vấn nối `UserRole` với `RolePermission` trong database. Không có thì 422 `AssigneeLacksPermission` (`EXC-06`, `AC-009`). Khi chuyển giao, phân công của người đang phụ trách giữ nguyên.

Bước 3 đọc từ database chứ không từ token. Người nhận không phải người gọi API nên trong yêu cầu không có token nào của họ; và token của chính họ cũng có thể mang bộ quyền cũ tới lúc hết hạn theo `BR-RBAC-009`. Dùng 422 vì người gọi có đủ quyền, chỉ người nhận được chọn không thỏa điều kiện; 403 dành cho người gọi thiếu quyền.

Bước 1 chặn trường hợp lọt khi thao tác khác chạy song song. Tình huống: chị Lan giao gói G cho anh Hải, cùng lúc một người quản trị khác thu hồi vai trò duy nhất cho anh Hải quyền `supervision.complete`. Không khóa thì việc giao thấy anh Hải còn quyền, việc thu hồi thấy anh Hải chưa phụ trách gói nào; cả hai cùng commit và anh Hải phụ trách G mà không còn quyền. Thu hồi vai trò ([TDD-RBAC-002](TDD-RBAC-002.md#architecture)) và sửa quyền vai trò ([TDD-RBAC-001](TDD-RBAC-001.md#architecture)) khóa cùng dòng `User` đó, nên bên đến sau phải chờ. Sau khi lấy được khóa, handler mới đọc quyền và phân công bằng câu lệnh riêng; ở mức cô lập `READ COMMITTED`, câu đọc này thấy thay đổi vừa commit của bên kia và từ chối đúng.

Dùng `FOR NO KEY UPDATE` thay vì `FOR UPDATE` để không chặn các lệnh chèn có khóa ngoại tới dòng `User` đó, vì các lệnh này chỉ cần khóa `FOR KEY SHARE`.

### Thứ tự khóa giữa các luồng

Các luồng dưới đây có thể chạy cùng lúc trên cùng một gói hoặc cùng một nhân viên. Thứ tự khóa của chúng:

| Luồng | Thứ tự khóa trong transaction |
|---|---|
| Giao (`POST /assignments`) | `SupervisionGrant` `FOR SHARE` → `User` người nhận `FOR NO KEY UPDATE` |
| Chuyển giao | `Assignment` `FOR UPDATE` → `SupervisionGrant` `FOR SHARE` → `User` người nhận `FOR NO KEY UPDATE` |
| Gỡ phân công | `Assignment` `FOR UPDATE` |
| Hoàn thành, mở lại ([TDD-SUB-006](TDD-SUB-006.md)) | tài khoản chủ gói `FOR UPDATE` → `Assignment` `FOR SHARE` (bỏ qua với Admin) → cập nhật `SupervisionGrant` |
| Hủy, khôi phục ([TDD-SUB-005](TDD-SUB-005.md)) | tài khoản chủ gói `FOR UPDATE` → cập nhật `SupervisionGrant`; không đụng `Assignment` |
| Thu hồi vai trò ([TDD-RBAC-002](TDD-RBAC-002.md)) | `User` nhân viên `FOR NO KEY UPDATE` → đếm `Assignment`, không khóa |
| Sửa quyền vai trò ([TDD-RBAC-001](TDD-RBAC-001.md)) | `Role` `FOR UPDATE` → các `User` giữ vai trò theo `Id` tăng dần `FOR NO KEY UPDATE` → đếm `Assignment`, không khóa |

"Tài khoản chủ gói" là dòng `User` của khách trong code hiện tại; TDD-SUB-006 dự kiến chuyển sang `AccountCommerceState` theo TDD-PAY-001.

Ba nguyên tắc giữ cho các luồng không chờ vòng lẫn nhau (deadlock):

1. Luồng nào cần cả `Assignment` lẫn `SupervisionGrant` đều khóa `Assignment` trước. Hoàn thành giữ khóa chia sẻ trên dòng phân công rồi mới cần khóa ghi trên gói; chuyển giao cũng lấy dòng phân công trước. Vì vậy hai luồng xếp hàng ngay ở dòng phân công, không bên nào giữ gói mà chờ dòng phân công.
2. Dòng `User` của nhân viên luôn khóa sau cùng. Thu hồi vai trò và sửa quyền vai trò không khóa `Assignment` hay `SupervisionGrant`, nên chúng chỉ có thể chờ ở dòng `User`, còn luồng phân công chỉ chờ dòng `User` khi đã giữ đủ các khóa khác.
3. Luồng phân công không khóa tài khoản chủ gói. Hủy và khôi phục chỉ chờ khóa trên dòng gói, mà luồng phân công giữ khóa này mà không chờ thêm gì từ luồng hủy.

Không khóa tài khoản chủ gói như các thao tác trên gói vì phân công không đổi dữ liệu thương mại của khách. Khóa `FOR UPDATE` trên dòng tài khoản sẽ chặn cả việc mua gói và các thao tác gói khác của khách đó trong lúc người quản trị giao việc, trong khi khóa `FOR SHARE` trên đúng dòng gói đã đủ để giữ trạng thái gói đứng yên.

### Kiểm phân công khi thao tác trên gói

Đợt này không có phân công mức khách hàng hay mức công trình (`BR-RBAC-013` khoản 3), nên phép kiểm chỉ còn một bước: có dòng phân công đang hiệu lực khớp đúng `('SupervisionGrant', grantId)` và thuộc người gọi hay không.

`IAssignmentAuthorizer.IsDirectlyAssignedAsync(staffUserId, resourceType, resourceId)` làm việc này và giữ khóa `FOR SHARE` trên dòng khớp tới hết transaction, để việc gỡ hoặc chuyển giao song song phải chờ, như [TDD-SUB-006](TDD-SUB-006.md) mô tả. Mỗi gói có tối đa một dòng đang hiệu lực, nên truy vấn dùng index `UX_Assignment_ActiveResource` và đọc nhiều nhất một dòng. Người gọi giữ vai trò hệ thống có mã `admin` thì đạt ngay, xem Activity Diagram.

Các thay đổi so với code ngày 23/09/2026, đã có trong code:

- Bỏ `IsAssignedAsync` cùng nhánh kế thừa từ khách hàng xuống dự án. Hàm này không có nơi gọi.
- Bỏ `IResourceHierarchyReader` và bản tạm `UnavailableResourceHierarchyReader`, vì không còn câu hỏi "dự án thuộc khách hàng nào".
- `SupervisionCompletionAccess` truyền `ResourceTypes.SupervisionGrant` và `grant.Id`, thay cho `ResourceTypes.Project` và `grant.ProjectId`. Không còn trường hợp truyền `Guid.Empty` cho gói chưa gán: gói chưa gán không thể có phân công theo khoản 8, nên người không phải Admin nhận 403 như trước. Mã lỗi thiếu phân công do TDD-SUB-006 đặt.
- `ResourceTypes` chỉ còn `SupervisionGrant`; bỏ `ResourceTypes.Customer` và `ResourceTypes.Project`.
- `AssignmentRowLocker.LockActiveForShareAsync` giữ nguyên điều kiện lọc, nhưng sau migration dùng `UX_Assignment_ActiveResource` thay cho index cũ trên `(StaffUserId, ResourceType, ResourceId)`, rồi lọc `StaffUserId` trên tối đa một dòng.
- Handler giao và chuyển giao khóa dòng gói qua hàm mới `LockSupervisionGrantStateForShareAsync` của `IAssignmentRowLocker`, cài bằng SQL có tham số trong `AssignmentRowLocker`. Các bước kiểm gói, người nhận và người đang phụ trách dùng chung trong `AssignmentLookup`.

### Phạm vi xem công trình theo gói đang phụ trách

`BR-SITE-003` khoản 3 cho nhân viên có `supervision.complete` mà không có `assignment.manage` chỉ xem công trình của những gói đang được phân công cho mình, kể cả gói đang bị hủy mà phân công vẫn còn. Tài liệu này cung cấp truy vấn xác định phạm vi đó; endpoint xem công trình và cách dùng truy vấn nằm ở [TDD-SITE-001](TDD-SITE-001.md).

```sql
SELECT DISTINCT g."ConstructionSiteId"
FROM "Assignment" a
JOIN "SupervisionGrant" g ON g."Id" = a."ResourceId"
WHERE a."ResourceType" = 'SupervisionGrant'
  AND a."StaffUserId" = @staffUserId
  AND a."EffectiveToUtc" IS NULL
  AND g."ConstructionSiteId" IS NOT NULL
```

Câu truy vấn đi từ index `(StaffUserId, EffectiveToUtc) WHERE "EffectiveToUtc" IS NULL` để lấy các gói người đó đang phụ trách, rồi nối khóa chính của `SupervisionGrant`. Không lọc `State`, vì gói bị hủy mà phân công còn thì nhân viên vẫn xem được công trình. Code viết điều kiện này bằng LINQ trong `ConstructionSiteStaffScope.ApplyAssigned` ([TDD-SITE-001](TDD-SITE-001.md#architecture)), cho cùng kết quả. Để kiểm một công trình cụ thể, dùng cùng điều kiện với thêm `g."ConstructionSiteId" = @siteId` trong `EXISTS`. Đây là thao tác đọc nên không khóa: chuyển giao hoặc gỡ vừa commit có hiệu lực từ yêu cầu xem tiếp theo, đúng `STORY-SITE-002/Non-Functional`.

### Thu hồi vai trò và sửa quyền vai trò

`BR-RBAC-007` chặn thu hồi vai trò hoặc bỏ quyền khỏi vai trò khi việc đó làm một người mất `supervision.complete` trong khi vẫn phụ trách ít nhất một gói giám sát. Bản ghi phân công không lưu vai trò nào làm căn cứ, và không cần lưu: phép kiểm chỉ xét bộ quyền còn lại của người đó sau thay đổi. Người còn `supervision.complete` từ vai trò khác thì không bị chặn, đúng `STORY-RBAC-002/AC-011`.

Hai handler nằm ở hai tài liệu khác: thu hồi vai trò ở [TDD-RBAC-002](TDD-RBAC-002.md#architecture), sửa quyền vai trò ở [TDD-RBAC-001](TDD-RBAC-001.md#architecture). Tài liệu này cung cấp truy vấn đếm phân công đang hiệu lực theo người, dùng index `(StaffUserId, EffectiveToUtc) WHERE "EffectiveToUtc" IS NULL`. Mỗi dòng đang hiệu lực ứng với đúng một gói, nên số dòng là số gói người đó đang phụ trách, kể cả gói đang bị hủy (`BR-RBAC-007/Notes`). Gói đang bị hủy không chuyển giao được, nên muốn thu hồi vai trò thì người quản trị gỡ phân công của gói đó.

Cả bốn thao tác giao, chuyển giao, thu hồi vai trò và sửa quyền vai trò đều khóa dòng `User` của nhân viên bị ảnh hưởng trước khi đọc quyền và phân công. Khi một thao tác khóa nhiều người, như sửa quyền vai trò, thứ tự khóa là `Id` tăng dần.

### Danh sách cần chia lại

`BR-RBAC-013` khoản 6 và `BR-RBAC-008` khoản 4 định nghĩa danh sách gói cần chia lại. Danh sách chỉ gồm gói có `State = 'Assigned'`, chia hai nhóm:

- Gói chưa có phân công đang hiệu lực, trạng thái `NoAssignee` (`AC-005`).
- Gói có người phụ trách đang bị khóa tài khoản, trạng thái `AssigneeLocked` (`AC-010`). Phân công của người bị khóa vẫn còn; hệ thống không tự gỡ hay tự chuyển.

Gói chưa gán, đã hoàn thành hoặc đang bị hủy không vào danh sách (`AC-011`). Tên trạng thái `NoAssignee` thay cho `Unassigned` của bản trước, để không trùng với `SupervisionStates.Unassigned`, vốn có nghĩa là gói chưa gán công trình.

Danh sách là **kết quả tính khi đọc**, không lưu thành bảng hay cột cờ. Bảng gói giám sát đã có, nên endpoint này nay triển khai được:

```sql
SELECT g."Id" AS "SupervisionGrantId", g."ConstructionSiteId", g."AccountId",
       a."Id" AS "AssignmentId", a."StaffUserId",
       CASE WHEN a."Id" IS NULL THEN 'NoAssignee' ELSE 'AssigneeLocked' END AS "Status"
FROM "SupervisionGrant" g
LEFT JOIN "Assignment" a
       ON a."ResourceType" = 'SupervisionGrant'
      AND a."ResourceId" = g."Id"
      AND a."EffectiveToUtc" IS NULL
LEFT JOIN "User" u ON u."Id" = a."StaffUserId"
WHERE g."State" = 'Assigned'
  AND (a."Id" IS NULL OR u."Status" = 'Locked')
ORDER BY g."FirstAssignedAtUtc", g."Id"
LIMIT @pageSize OFFSET @offset
```

Phép nối `Assignment` dùng `UX_Assignment_ActiveResource`. Sắp theo `FirstAssignedAtUtc` để gói gán lâu nhất mà chưa có người lên đầu; `Id` giữ thứ tự ổn định khi phân trang. Chưa thêm index riêng cho `SupervisionGrant.State`, vì số gói hiện nhỏ và việc đọc danh sách chỉ do người quản trị thực hiện. Nếu đo thấy chậm thì thêm partial index `(FirstAssignedAtUtc, Id) WHERE "State" = 'Assigned'` ở bảng gói theo TDD-SUB-004. Tên công trình được nối thêm từ bảng công trình lúc đọc, bằng một câu truy vấn cho cả trang. Phản hồi trả `customerUserId`, không kèm tên khách hàng, như ví dụ ở Internal API. Code viết truy vấn bằng LINQ trong `GetNeedsReassignmentQueryHandler`, cho cùng kết quả với câu SQL trên.

Mở khóa tài khoản thì gói ở nhóm thứ hai tự rời danh sách, vì trạng thái được tính từ `User.Status` hiện tại. Gói đã hoàn thành của người bị khóa không vào danh sách nhưng vẫn chuyển giao được theo khoản 8; người quản trị tra các gói của một người qua `GET /assignments?staffUserId=...&activeOnly=true`, đúng `STORY-RBAC-003/ALT-05`.

### Nơi từng Business Rule được thực hiện

| Quy tắc | Nơi thực hiện |
|---|---|
| BR-RBAC-001 | Truy vấn nối `UserRole` với `RolePermission` để biết người nhận có `supervision.complete` hay không |
| BR-RBAC-005 | Kiểm `User.AccountKind = 'Staff'` trong handler giao và chuyển giao |
| BR-RBAC-007 | Truy vấn đếm phân công đang hiệu lực theo người, gọi từ handler thu hồi vai trò và handler sửa quyền vai trò; khóa dòng `User` dùng chung với giao và chuyển giao |
| BR-RBAC-008 | Không đụng tới `Assignment` khi khóa tài khoản; gói đang gán của người bị khóa vào danh sách cần chia lại với trạng thái `AssigneeLocked` |
| BR-RBAC-010 | `IAssignmentAuthorizer.IsDirectlyAssignedAsync` gọi từ handler hoàn thành và mở lại gói giám sát, với `('SupervisionGrant', grantId)` |
| BR-RBAC-011 | Policy `assignment.manage` ở endpoint phân công |
| BR-RBAC-012 | `IAccessAuditWriter` với ba hành động `AssignmentCreated`, `AssignmentTransferred`, `AssignmentEnded` |
| BR-RBAC-013 | Bảng `Assignment` với một loại `SupervisionGrant`, index `UX_Assignment_ActiveResource`; khóa `FOR SHARE` và kiểm trạng thái gói (khoản 8); kiểm người nhận ở handler; danh sách cần chia lại tính khi đọc; luồng hủy và khôi phục không đụng `Assignment` (khoản 9) |
| BR-SUB-006 | Tập trạng thái giữ chỗ `Assigned`/`Completed` dùng lại cho điều kiện của khoản 8 |
| BR-SITE-003 | Truy vấn công trình của các gói đang phụ trách, dùng cho phạm vi xem của nhân viên chỉ có `supervision.complete` |

**Notes**:
- Chặng kiểm phân công đặt trong handler chứ không thành một pipeline behavior của MediatR, vì tài nguyên đích thường phải đọc từ database mới biết. Ví dụ phạm vi xem công trình phải đi từ phân công sang gói rồi mới tới công trình; một behavior chạy trước handler không có sẵn thông tin này mà không tự đi truy vấn thêm.
- `IAssignmentAuthorizer` khai báo ở `src/bmt-be.application/abstractions/`; `AssignmentAuthorizer` nằm ở `src/bmt-be.application/services/`, đã triển khai ngày 23/09/2026. `IsAssignedAsync`, `IResourceHierarchyReader` và `UnavailableResourceHierarchyReader` đã bỏ ngày 25/09/2026.
- Phần hoàn thành/mở lại của `TDD-SUB-003` đã được `TDD-SUB-006` thay thế. `TDD-SUB-006` dùng `IAssignmentAuthorizer.IsDirectlyAssignedAsync` làm nguồn sự thật duy nhất về việc ai phụ trách gói nào.
- Giao và chuyển giao không ghi nhật ký từ chối nào. Theo `BR-RBAC-012/Notes`, người dùng xác nhận ngày 25/09/2026: **không ghi nhật ký** khi phân công bị từ chối vì gói đã có người phụ trách, gói chưa gán công trình hoặc đang bị hủy, hoặc người nhận thiếu `supervision.complete`. Đó là lỗi nghiệp vụ, không phải từ chối vì rào chắn quyền theo `BR-RBAC-012` khoản 2, nên theo `BR-RBAC-011` khoản 4 yêu cầu bị từ chối không để lại dòng nào, kể cả trong `AccessAuditLog`. `AssignmentResourceNotFound` cũng là lỗi đầu vào nên không ghi. Việc ghi nhật ký từ chối trước đây cho `AssignmentTargetInvalid` và `DuplicateAssignment` đã bỏ. Trường hợp index chặn yêu cầu song song cũng không ghi gì, vì transaction đã rollback.
- `TargetLabel` của nhật ký phân công ghi tên công trình của gói tại thời điểm thao tác, ví dụ "Gói giám sát · Nhà phố Quận 7", để nhật ký vẫn đọc được khi khách đổi tên công trình (`BR-RBAC-012` khoản 3). Đã triển khai ở `AssignmentLookup.GrantLabelAsync` (commit `1c58c38`); gói chưa có công trình thì ghi mã gói.

## Sequence Diagram

Chuyển giao một phân công, rồi nhân viên nhận bàn giao dùng quyền có gắn phân công.

```mermaid
sequenceDiagram
    actor AD as Nguoi quan tri
    participant API as Assignment endpoints
    participant AH as Transfer handler
    participant PG as PostgreSQL
    participant AU as IAccessAuditWriter
    actor NA as Nhan vien nhan ban giao
    participant BH as Handler hoan thanh goi giam sat
    participant AZ as IAssignmentAuthorizer

    AD->>API: POST assignments/{id}/transfer {toStaffUserId}
    API->>AH: TransferAssignmentCommand
    AH->>PG: Khoa dong Assignment FOR UPDATE, doc phan cong dang hieu luc
    AH->>PG: Khoa dong SupervisionGrant FOR SHARE, doc State
    alt Goi Unassigned hoac CanceledByStaff
        AH-->>AD: 409 ResourceNotAssignable
    else Goi Assigned hoac Completed
        AH->>PG: Khoa dong User cua nguoi nhan FOR NO KEY UPDATE
        AH->>PG: Doc trang thai va quyen nguoi nhan tu User, UserRole, RolePermission
        alt Nguoi nhan khong phai Staff dang hoat dong
            AH-->>AD: 409 AssignmentTargetInvalid
        else Nguoi nhan thieu supervision.complete
            AH-->>AD: 422 AssigneeLacksPermission
        else Nguoi nhan trung nguoi dang phu trach
            AH-->>AD: 409 DuplicateAssignment
        else Hop le
            AH->>PG: Dat EffectiveToUtc cho dong cu roi SaveChanges
            AH->>PG: Chen dong moi cho nguoi nhan cung moc thoi gian
            AH->>AU: RecordAsync AssignmentTransferred
            Note over AH,PG: Cac buoc tren cung mot transaction
            AH-->>AD: 200 {endedAssignmentId, newAssignmentId}
        end
    end

    NA->>BH: Yeu cau hoan thanh goi giam sat G
    Note over NA,BH: Policy supervision.complete da dat o endpoint
    BH->>AZ: IsDirectlyAssignedAsync(nhan vien, SupervisionGrant, G)
    AZ->>PG: Nguoi goi co giu vai tro Role.Code = admin khong
    AZ->>PG: Tim phan cong dang hieu luc cua G thuoc nguoi goi, khoa FOR SHARE
    alt Khong phai admin va khong co dong khop
        BH-->>NA: 403 theo TDD-SUB-006
    else Dat
        BH-->>NA: 200 Thuc hien thao tac
    end
```

## Activity Diagram

Hai luồng kiểm tra: giao gói cho nhân viên, và kiểm phân công khi hoàn thành hoặc mở lại gói giám sát.

```mermaid
flowchart TD
    subgraph GIAO [Giao goi - POST assignments]
        A1[Da qua policy assignment.manage<br/>va validator resourceType] --> A2[Khoa dong SupervisionGrant<br/>FOR SHARE]
        A2 --> A3{Goi ton tai?}
        A3 -->|Khong| A4[404 AssignmentResourceNotFound]
        A3 -->|Co| A5{State la Assigned<br/>hoac Completed?}
        A5 -->|Khong| A6[409 ResourceNotAssignable]
        A5 -->|Co| A7[Khoa dong User cua nguoi nhan<br/>FOR NO KEY UPDATE]
        A7 --> A8{Staff, chua xoa, dang Active?}
        A8 -->|Khong| A9[409 AssignmentTargetInvalid]
        A8 -->|Co| A10{Co supervision.complete<br/>doc tu UserRole va RolePermission?}
        A10 -->|Khong| A11[422 AssigneeLacksPermission]
        A10 -->|Co| A12{Goi da co phan cong dang hieu luc?}
        A12 -->|Co| A13[409 ResourceAlreadyAssigned<br/>huong sang chuyen giao]
        A12 -->|Khong| A14[Chen Assignment va ghi nhat ky AssignmentCreated]
        A14 --> A15{Index UX_Assignment_ActiveResource<br/>bao trung khi luu?}
        A15 -->|Co| A13
        A15 -->|Khong| A16[Commit, tra 201]
    end
    subgraph KIEM [Kiem phan cong khi hoan thanh hoac mo lai goi giam sat]
        B1[Da qua policy supervision.complete] --> B2{Nguoi goi giu vai tro he thong<br/>co Role.Code = admin?}
        B2 -->|Co| B5[Dat, bo qua chang phan cong]
        B2 -->|Khong| B3[Tim Assignment dang hieu luc cua goi<br/>khoa FOR SHARE]
        B3 --> B4{Dong do thuoc nguoi goi?}
        B4 -->|Co| B5
        B4 -->|Khong| B6[403 theo TDD-SUB-006]
    end
```

Nhánh Admin ở luồng thứ hai thực hiện phần Except của `BR-RBAC-013`: `BR-SUB-011` và `BR-SUB-012` đã chốt Admin thao tác được trên gói giám sát mà không cần phân công. Đây là ngoại lệ **duy nhất** theo vai trò trong phần phân quyền. Handler nhận diện Admin bằng mã vai trò hệ thống `admin` (cột `Role.Code`, hằng số `RoleCodes.Admin`), không bằng tên hiển thị `Role.Name`: tên hiển thị là dữ liệu cho người đọc, còn mã là định danh ổn định mà code tham chiếu. Nhánh này chỉ bỏ qua chặng phân công. Người gọi vẫn phải qua policy `supervision.complete` ở endpoint trước khi tới đây; Admin qua được vì vai trò `admin` có mã này trong `RolePermission`. Hiện trạng code: `AssignmentAuthorizer` đã so `Role.Code == RoleCodes.Admin`, đọc từ database.

Chuyển giao đi qua cùng các bước A2–A11 như luồng giao, nhưng khóa dòng `Assignment` trước bước A2, và thay bước A12 bằng kiểm người nhận khác người đang phụ trách (`DuplicateAssignment`).

## State Diagram

Vòng đời một dòng phân công.

```mermaid
stateDiagram-v2
    [*] --> DangHieuLuc: Nguoi quan tri phan cong goi dang gan hoac da hoan thanh<br/>EffectiveToUtc = NULL
    DangHieuLuc --> DaKetThuc: Go phan cong<br/>dat EffectiveToUtc
    DangHieuLuc --> DaKetThuc: Chuyen giao<br/>dat EffectiveToUtc va tao dong moi
    DaKetThuc --> [*]: Giu lai de tra cuu lich su
```

Dòng đã kết thúc không quay lại `DangHieuLuc`. Muốn giao lại gói cho đúng người cũ thì tạo một dòng mới, để hai khoảng thời gian phụ trách tách bạch khi tra cứu. Hai sự kiện sau **không** làm dòng đổi trạng thái:

- Khóa tài khoản: người bị khóa vẫn còn phân công `DangHieuLuc` nhưng không thao tác được vì phiên đã bị cắt. Nếu gói đang ở trạng thái đã gán, gói hiện trong danh sách cần chia lại với trạng thái `AssigneeLocked`.
- Hủy hoặc khôi phục gói: dòng giữ nguyên `DangHieuLuc` theo `BR-RBAC-013` khoản 9. Trong lúc gói bị hủy, dòng chỉ còn đường ra là gỡ, vì chuyển giao đòi gói đang giữ chỗ.

## Data Model

Một bảng, đã tạo ở migration `20260923152830_InitialRbac`. Thiết kế ngày 25/09/2026 đổi giá trị cho phép của `ResourceType` và index duy nhất. Các bảng `User`, `Role`, `UserRole`, `RolePermission` và `AccessAuditLog` được định nghĩa ở [TDD-RBAC-001](TDD-RBAC-001.md#data-model). Bảng `SupervisionGrant` được dùng lại, schema nguồn ở [TDD-SUB-004](TDD-SUB-004.md#data-model); tài liệu này chỉ đọc hai cột `State` và cột công trình của gói.

### `Assignment` — ai phụ trách gói giám sát nào

Một dòng là **một khoảng thời gian một nhân viên phụ trách một gói giám sát cụ thể**. Dòng được tạo khi người quản trị giao hoặc chuyển giao; được cập nhật đúng một lần trong đời, khi đặt `EffectiveToUtc` lúc gỡ hoặc lúc chuyển giao. Trong vận hành, dòng không bao giờ bị xóa; migration ngày 25/09/2026 là ngoại lệ, chỉ xóa dữ liệu dev/test (xem Notes). Hủy hay khôi phục gói không sửa dòng này.

Bảng này không có khóa ngoại tới gói, vì nó phải phục vụ nhiều loại tài nguyên. Việc kiểm gói có thật nằm ở handler, dưới khóa `FOR SHARE` trên dòng gói.

| Cột | Kiểu | Ràng buộc | Ý nghĩa |
|---|---|---|---|
| `Id` | uuid | PK | |
| `StaffUserId` | uuid | FK `User(Id)` ON DELETE RESTRICT, NOT NULL | Nhân viên phụ trách. `RESTRICT` để không mất lịch sử phụ trách khi xóa tài khoản |
| `ResourceType` | varchar(32) | NOT NULL, CHECK `CK_Assignment_ResourceType` | Đợt này chỉ nhận `SupervisionGrant`. Giá trị mới thêm sau, ví dụ `Lead`, dùng chung bảng này |
| `ResourceId` | uuid | NOT NULL | `SupervisionGrant.Id` của gói được giao. Không có khóa ngoại |
| `EffectiveFromUtc` | timestamptz | NOT NULL | Thời điểm bắt đầu phụ trách |
| `EffectiveToUtc` | timestamptz | NULL | NULL nghĩa là **đang phụ trách**. Có giá trị nghĩa là đã gỡ hoặc đã chuyển giao |
| `CreatedBy` | uuid | FK `User(Id)`, NOT NULL | Người quản trị đã giao |
| `EndedBy` | uuid | FK `User(Id)`, NULL | Người quản trị đã gỡ hoặc chuyển giao. NULL khi dòng còn hiệu lực |
| `EndReason` | varchar(20) | NULL | `Transferred` hoặc `Removed`. NULL khi dòng còn hiệu lực |

Ba cột kết thúc `EffectiveToUtc`, `EndedBy`, `EndReason` cùng NULL hoặc cùng có giá trị; CHECK `CK_Assignment_EndColumnsConsistent` đã có bảo đảm điều này.

`EndReason` phân biệt hai lý do kết thúc dẫn tới hai hệ quả khác nhau: chuyển giao thì gói vẫn có người phụ trách, còn gỡ thì gói không còn ai và, nếu đang ở trạng thái đã gán, vào danh sách cần chia lại với trạng thái `NoAssignee`. Không có cột nào trỏ từ dòng cũ sang dòng mới khi chuyển giao; quan hệ đó tra qua `AccessAuditLog` hoặc qua việc hai dòng có cùng `ResourceType`, `ResourceId` và liền nhau về thời gian.

Trạng thái trong danh sách cần chia lại (`NoAssignee`, `AssigneeLocked`) **không phải cột**. Nó được tính khi đọc từ `SupervisionGrant.State`, `Assignment` và `User.Status`, như mục Architecture mô tả.

### Sơ đồ quan hệ

Quan hệ giữa `SupervisionGrant` và `Assignment` là quan hệ logic qua `ResourceType = 'SupervisionGrant'` và `ResourceId`, không có khóa ngoại trong database.

```mermaid
erDiagram
    User {
        uuid Id PK
        varchar AccountKind "Customer hoac Staff"
        varchar Status "Active hoac Locked"
    }
    SupervisionGrant {
        uuid Id PK
        uuid AccountId FK "chu goi"
        uuid ConstructionSiteId "NULL khi chua gan"
        varchar State "Unassigned Assigned CanceledByStaff Completed"
    }
    Assignment {
        uuid Id PK
        uuid StaffUserId FK
        varchar ResourceType "SupervisionGrant, sau nay Lead"
        uuid ResourceId "SupervisionGrant.Id, khong co khoa ngoai"
        timestamptz EffectiveFromUtc
        timestamptz EffectiveToUtc "NULL khi dang phu trach"
        uuid CreatedBy FK
        uuid EndedBy FK "NULL khi con hieu luc"
        varchar EndReason "Transferred hoac Removed NULL khi con hieu luc"
    }
    User ||--o{ Assignment : "phu trach"
    User ||--o{ Assignment : "phan cong"
    User ||--o{ SupervisionGrant : "so huu"
    SupervisionGrant ||--o{ Assignment : "duoc phan cong, quan he logic"
```

### Dữ liệu mẫu

Toàn bộ mẫu dưới đây là **dữ liệu giả định để giải thích thiết kế**, không phải dữ liệu thật và không phải kết quả đã ghi database. ID viết dạng bí danh; bản ghi thật dùng uuid. Các cột không liên quan được lược bớt. Thời gian theo UTC.

Tình huống xuyên suốt: chị Lan và anh Hùng là người quản trị có `assignment.manage`. Anh Tú, chị Mai và anh Nam là nhân viên đang hoạt động, có `supervision.complete`. Anh Đức là nhân viên đang hoạt động nhưng chỉ có `commerce.read`. Bảng `SupervisionGrant` (dùng lại, trích cột) có bốn gói:

| Id | AccountId | ConstructionSiteId | State | FirstAssignedAtUtc |
|---|---|---|---|---|
| `grant-g1` | `user-u1` | `site-p1` | Assigned | 2026-09-20T01:00:00Z |
| `grant-g2` | `user-u2` | `site-p2` | Assigned | 2026-09-21T01:00:00Z |
| `grant-g3` | `user-u1` | `site-p3` | Completed | 2026-09-10T01:00:00Z |
| `grant-g4` | `user-u1` | NULL | Unassigned | NULL |

**Bước 1 — chị Lan giao việc lúc 02:00 ngày 22/09/2026.** G1 cho anh Tú, G2 cho chị Mai:

| Id | StaffUserId | ResourceType | ResourceId | EffectiveFromUtc | EffectiveToUtc | EndReason |
|---|---|---|---|---|---|---|
| `asg-1` | `user-tu` | SupervisionGrant | `grant-g1` | 2026-09-22T02:00:00Z | NULL | NULL |
| `asg-2` | `user-mai` | SupervisionGrant | `grant-g2` | 2026-09-22T02:05:00Z | NULL | NULL |

Kết quả kiểm khi hoàn thành gói giám sát. Đây là kết quả tính khi đọc, không lưu:

| Người | Gói | Đạt | Vì sao |
|---|---|---|---|
| Anh Tú | G1 | Có | Có `asg-1` đang hiệu lực |
| Anh Tú | G2 | Không | Dòng đang hiệu lực duy nhất của G2 là `asg-2` của chị Mai |
| Chị Mai | G2 | Có | Có `asg-2` đang hiệu lực |

**Bước 2 — bốn yêu cầu bị từ chối lúc 03:00 ngày 23/09/2026.** Bảng `Assignment` không đổi:

- Giao thẳng G1 cho anh Nam, không qua chuyển giao: 409 `ResourceAlreadyAssigned`, vì G1 đã có `asg-1` (`AC-004`).
- Giao G4 cho anh Nam: 409 `ResourceNotAssignable`, vì G4 chưa gán công trình (`AC-011`).
- Giao G3 cho anh Đức: 422 `AssigneeLacksPermission`, vì anh Đức không có `supervision.complete` (`AC-009`). G3 đã hoàn thành nên được giao, chỉ người nhận không đạt.
- Chuyển giao G1 từ anh Tú sang anh Đức: cũng 422 `AssigneeLacksPermission`; `asg-1` vẫn của anh Tú.

**Bước 3 — hai người quản trị cùng giao G3 lúc 04:00 ngày 23/09/2026.** Chị Lan giao G3 cho anh Nam, cùng lúc anh Hùng giao G3 cho chị Mai. Hai khóa `FOR SHARE` trên dòng G3 không chặn nhau, nên cả hai handler đều thấy G3 chưa có ai. Yêu cầu của chị Lan commit trước và tạo `asg-3`. Câu `INSERT` của anh Hùng vi phạm `UX_Assignment_ActiveResource`, transaction rollback, anh Hùng nhận 409 `ResourceAlreadyAssigned`:

| Id | StaffUserId | ResourceType | ResourceId | EffectiveFromUtc | EffectiveToUtc | EndReason |
|---|---|---|---|---|---|---|
| `asg-3` | `user-nam` | SupervisionGrant | `grant-g3` | 2026-09-23T04:00:00Z | NULL | NULL |

**Bước 4 — chuyển giao G1 từ anh Tú sang anh Nam lúc 05:00 ngày 25/09/2026.** Handler đóng `asg-1` và lưu trước, rồi mới chèn `asg-4`:

| Id | StaffUserId | ResourceType | ResourceId | EffectiveFromUtc | EffectiveToUtc | EndedBy | EndReason |
|---|---|---|---|---|---|---|---|
| `asg-1` | `user-tu` | SupervisionGrant | `grant-g1` | 2026-09-22T02:00:00Z | 2026-09-25T05:00:00Z | `user-lan` | Transferred |
| `asg-4` | `user-nam` | SupervisionGrant | `grant-g1` | 2026-09-25T05:00:00Z | NULL | NULL | NULL |

Từ 05:00, anh Tú không còn thao tác được trên G1 vì dòng của anh đã có `EffectiveToUtc`; anh Nam thao tác được. Tại mọi thời điểm G1 chỉ có một dòng đang hiệu lực. Thử chuyển G1 tiếp từ anh Nam sang chính anh Nam thì bị từ chối với `DuplicateAssignment`, bảng không đổi.

**Bước 5 — G2 bị hủy lúc 07:00 ngày 26/09/2026, rồi được khôi phục lúc 09:00.** Luồng hủy của TDD-SUB-005 đổi `grant-g2.State` thành `CanceledByStaff`; `asg-2` **không đổi**. Lúc 07:30, chị Lan thử chuyển giao G2 sang anh Nam và nhận 409 `ResourceNotAssignable`. Lúc 09:00, G2 được khôi phục về `Assigned`; chị Mai tiếp tục phụ trách G2 qua chính `asg-2`, không cần giao lại (`AC-012`).

**Bước 6 — gỡ phân công của chị Mai trên G2 lúc 08:00 ngày 27/09/2026, không chuyển cho ai:**

| Id | StaffUserId | ResourceType | ResourceId | EffectiveToUtc | EndedBy | EndReason |
|---|---|---|---|---|---|---|
| `asg-2` | `user-mai` | SupervisionGrant | `grant-g2` | 2026-09-27T08:00:00Z | `user-lan` | Removed |

G2 không còn ai thao tác được. Bốn dòng `asg-1` tới `asg-4` đều còn trong bảng, nên tra lịch sử cho biết anh Tú phụ trách G1 từ 22/09 tới 25/09 và chị Mai phụ trách G2 từ 22/09 tới 27/09, kể cả hai giờ G2 bị hủy.

**Bước 7 — anh Nam bị khóa tài khoản lúc 10:00 ngày 30/09/2026.** `asg-3` và `asg-4` **không đổi**: `EffectiveToUtc` vẫn NULL. Anh Nam không thao tác được trên G1 và G3 vì phiên đã bị cắt theo [TDD-RBAC-002](TDD-RBAC-002.md), không phải vì phân công bị gỡ. Danh sách cần chia lại lúc này là kết quả tính khi đọc:

| Gói | `State` của gói | Phân công đang hiệu lực | Trạng thái trong danh sách |
|---|---|---|---|
| G2 | Assigned | Không có | `NoAssignee` |
| G1 | Assigned | `asg-4` của `user-nam`, tài khoản `Locked` | `AssigneeLocked` |
| G3 | Completed | `asg-3` của `user-nam`, tài khoản `Locked` | Không có trong danh sách |
| G4 | Unassigned | Không có | Không có trong danh sách |

Chị Lan chuyển G1 sang người khác theo `ALT-05`; mỗi lần chuyển giao đóng một dòng của anh Nam và mở một dòng mới, và gói đó rời danh sách. G3 không nằm trong danh sách nhưng vẫn chuyển giao được, vì gói đã hoàn thành vẫn giữ chỗ trên công trình.

**Notes**:

- **Index duy nhất có lọc** `UX_Assignment_ActiveResource` trên `(ResourceType, ResourceId) WHERE "EffectiveToUtc" IS NULL` giữ đúng `BR-RBAC-013` khoản 4 như Bước 3. Nó cũng là index chính của phép kiểm phân công, của câu hỏi "ai đang phụ trách gói G" và của phép nối trong danh sách cần chia lại, vì mỗi gói có tối đa một dòng trong index. Index này thay `IX_Assignment_StaffUserId_ResourceType_ResourceId`.
- **Index** `(StaffUserId, EffectiveToUtc) WHERE "EffectiveToUtc" IS NULL` giữ nguyên, phục vụ đếm gói đang phụ trách khi thu hồi vai trò hoặc sửa quyền vai trò, liệt kê gói một người đang phụ trách, và truy vấn phạm vi xem công trình. `(ResourceType, ResourceId, EffectiveToUtc)` giữ nguyên, phục vụ tra lịch sử của một gói gồm cả dòng đã kết thúc.
- **Khóa khi chuyển giao**: handler đọc dòng phân công dưới khóa `FOR UPDATE`, để hai yêu cầu chuyển giao cùng một phân công phải nối đuôi nhau. Index đã chặn được hai dòng mới cho hai người nhận khác nhau, nhưng khóa dòng vẫn giữ để yêu cầu đến sau nhận `AssignmentAlreadyEnded` rõ nghĩa thay vì lỗi vi phạm index. Thứ tự khóa đầy đủ ở mục "Thứ tự khóa giữa các luồng".
- **Transaction**: đóng dòng cũ, lưu, rồi chèn dòng mới nằm trong cùng một transaction do `TransactionPipelineBehavior` mở, cùng với dòng nhật ký ghi bằng `RecordAsync`. Không có trạng thái trung gian nào được commit mà gói vừa mất người cũ vừa chưa có người mới.
- **Migration — đã triển khai ngày 25/09/2026**: là bước 5 của migration gộp `20260925074152_ConstructionSiteAndPackageAssignment`; danh sách đủ các bước ở [TDD-SITE-001](TDD-SITE-001.md#data-model). Database hiện chỉ có dữ liệu dev/test, nên không ánh xạ dữ liệu cũ sang gói thật. Thứ tự trong migration:
  1. Bỏ CHECK `CK_Assignment_ResourceType` cũ và index `IX_Assignment_StaffUserId_ResourceType_ResourceId`.
  2. Xóa mọi dòng có `ResourceType` là `Customer` hoặc `Project`. Không đổi nhãn các dòng này thành `SupervisionGrant`: `ResourceId` của chúng là mã khách hàng hoặc mã dự án, không phải mã gói, nên đổi nhãn sẽ tạo dòng trỏ tới gói không tồn tại. Cũng không ánh xạ qua `SupervisionGrant.ProjectId`, vì dòng đã kết thúc không xác định được gói nào đang ở dự án tại thời điểm đó. `AccessAuditLog` giữ nguyên, vì `TargetId` của nhật ký không có khóa ngoại.
  3. Thêm CHECK mới `"ResourceType" IN ('SupervisionGrant')`.
  4. Tạo `UX_Assignment_ActiveResource`.

  Migration không tự đếm dữ liệu trước và sau. Việc xác nhận môi trường chỉ có dữ liệu dev/test là bước vận hành trước khi chạy; có dữ liệu thật thì dừng và lập kế hoạch khác. Ngày 25/09/2026 đã chạy thử `Up` trên PostgreSQL có phân công `Project` cũ: sau migration không còn dòng `Customer` hay `Project`. Chưa áp dụng lên môi trường dev dùng chung hay production.

  Bảng nhỏ nên tạo index ngay trong transaction của migration, không cần `CREATE INDEX CONCURRENTLY`; bảng chỉ bị khóa ghi trong thời gian ngắn. Migration phải triển khai cùng phiên bản code có `ResourceTypes.SupervisionGrant`, vì code cũ còn ghi `Project` sẽ vi phạm CHECK mới. Sau migration, nhân viên trên môi trường dev/test cần được giao lại gói. Down migration xóa các phân công `SupervisionGrant` rồi dựng lại CHECK và index cũ, nhưng không lấy lại được các dòng đã xóa ở bước 2; muốn có lại dữ liệu đó thì khôi phục từ bản sao lưu của môi trường dev/test. Vì cùng một migration với TDD-SUB-004 và TDD-SITE-001, danh sách cần chia lại và truy vấn phạm vi xem công trình luôn có sẵn cột `ConstructionSiteId` và bảng công trình để đọc.
- **Không còn dòng mồ côi do tài nguyên bị xóa**: phân công chỉ được tạo cho gói có thật, và gói không bao giờ bị xóa. Nếu sau này thêm loại tài nguyên có thể bị xóa, phải thiết kế cách xử lý phân công của tài nguyên đó trước khi mở loại mới.

## Internal API

Bốn endpoint `GET`, `POST`, chuyển giao và `DELETE` đã triển khai ngày 23/09/2026 theo mô hình cũ, với hai loại tài nguyên và nhiều người cùng phụ trách. Các thay đổi ngày 25/09/2026, đã có trong code:

- `resourceType` chỉ nhận `SupervisionGrant`.
- Giao và chuyển giao khóa và kiểm trạng thái gói, rồi kiểm `supervision.complete` của người nhận.
- Giao từ chối gói đã có người phụ trách.
- Thêm `GET /assignments/needs-reassignment`, thay cho `GET /assignments/unassigned` của bản trước.

### Endpoints

- **GET** `/api/v1/assignments` — Danh sách phân công, lọc theo `staffUserId`, `resourceType`, `resourceId`, `activeOnly`, có phân trang. Cần quyền `assignment.manage`.
- **POST** `/api/v1/assignments` — Giao một gói giám sát đang giữ chỗ trên công trình và chưa có người phụ trách cho một nhân viên đang hoạt động có `supervision.complete`. Gói đã có người phụ trách thì từ chối và hướng sang chuyển giao. Cần quyền `assignment.manage`.
- **POST** `/api/v1/assignments/{assignmentId}/transfer` — Chuyển giao sang nhân viên khác đang hoạt động có `supervision.complete`; gói phải đang ở trạng thái đã gán hoặc đã hoàn thành. Cần quyền `assignment.manage`.
- **DELETE** `/api/v1/assignments/{assignmentId}` — Gỡ phân công, không chuyển cho ai; được với gói ở mọi trạng thái. Gói đang gán thì vào danh sách cần chia lại. Cần quyền `assignment.manage`.
- **GET** `/api/v1/assignments/needs-reassignment` — Danh sách gói cần chia lại: gói đang ở trạng thái đã gán mà chưa có người phụ trách, hoặc người phụ trách đang bị khóa. Mỗi dòng có trạng thái `NoAssignee` hoặc `AssigneeLocked`, có phân trang theo `pageIndex`, `pageSize`. Cần quyền `assignment.manage`.

### Examples

#### POST /api/v1/assignments

```
Request:
{"staffUserId": "user-tu", "resourceType": "SupervisionGrant", "resourceId": "grant-g1"}

Response 201:
{"id": "asg-1", "staffUserId": "user-tu", "resourceType": "SupervisionGrant", "resourceId": "grant-g1", "effectiveFromUtc": "2026-09-22T02:00:00Z", "effectiveToUtc": null}

Error Response:
{"title": "Conflict", "code": "Conflict", "status": 409, "detail": "Gói này đang có người phụ trách. Dùng chuyển giao để đổi người.", "messageCode": "ResourceAlreadyAssigned", "errors": null, "currentAssignmentId": "asg-1"}
```

`currentAssignmentId` là phân công đang hiệu lực của gói, để giao diện mở thẳng thao tác chuyển giao. Khi index chặn yêu cầu song song, phản hồi vẫn mang mã `ResourceAlreadyAssigned` nhưng không kèm `currentAssignmentId`, vì transaction đã lỗi và `ConstraintViolationPipelineBehavior` không đọc thêm được. Giao gói chưa gán công trình như G4 ở Bước 2 trả `{"title": "Conflict", "code": "Conflict", "status": 409, "detail": "Chỉ giao được gói giám sát đã gán công trình hoặc đã hoàn thành.", "messageCode": "ResourceNotAssignable", "errors": null}`.

#### POST /api/v1/assignments/{assignmentId}/transfer

```
Request:
{"toStaffUserId": "user-nam"}

Response 200:
{"endedAssignmentId": "asg-1", "newAssignmentId": "asg-4", "effectiveAtUtc": "2026-09-25T05:00:00Z"}

Error Response:
{"title": "Conflict", "code": "Conflict", "status": 409, "detail": "Chỉ giao được gói giám sát đã gán công trình hoặc đã hoàn thành.", "messageCode": "ResourceNotAssignable", "errors": null}
```

#### GET /api/v1/assignments?staffUserId=user-nam&activeOnly=true

```
Response 200:
{"items": [{"id": "asg-3", "staffUserId": "user-nam", "resourceType": "SupervisionGrant", "resourceId": "grant-g3", "effectiveFromUtc": "2026-09-23T04:00:00Z", "effectiveToUtc": null}, {"id": "asg-4", "staffUserId": "user-nam", "resourceType": "SupervisionGrant", "resourceId": "grant-g1", "effectiveFromUtc": "2026-09-25T05:00:00Z", "effectiveToUtc": null}], "pageIndex": 1, "pageSize": 20, "totalCount": 2}
```

Màn hình chia lại gói của một nhân viên bị khóa dùng truy vấn này theo `STORY-RBAC-003/ALT-05`, để thấy cả gói đã hoàn thành như G3 vốn không có trong danh sách cần chia lại. Tài khoản bị khóa vẫn xuất hiện trong kết quả vì phân công của họ không bị gỡ.

#### GET /api/v1/assignments/needs-reassignment

```
Response 200:
{"items": [{"supervisionGrantId": "grant-g2", "constructionSiteId": "site-p2", "constructionSiteName": "Nhà phố Gò Vấp", "customerUserId": "user-u2", "status": "NoAssignee", "assignmentId": null, "staffUserId": null}, {"supervisionGrantId": "grant-g1", "constructionSiteId": "site-p1", "constructionSiteName": "Nhà phố Quận 7", "customerUserId": "user-u1", "status": "AssigneeLocked", "assignmentId": "asg-4", "staffUserId": "user-nam"}], "pageIndex": 1, "pageSize": 20, "totalCount": 2}
```

Dữ liệu khớp Bước 7 ở Data Model; tên công trình là dữ liệu giả định. `status` là giá trị tính khi đọc. `constructionSiteName` đọc từ bảng công trình lúc trả phản hồi, nên luôn là tên hiện tại.

### Error Codes

Mỗi mã dưới đây là giá trị `messageCode` trong thân lỗi; trường `code` chỉ là loại lỗi chung như `Forbidden`, `Conflict`, `NotFound`.

- **AccessForbidden** (403): Thiếu quyền `assignment.manage`, hoặc thiếu điều kiện phân công khi dùng quyền có gắn phân công.
- **AssignmentTargetInvalid** (409): Người nhận không phải tài khoản nhân viên đang hoạt động, gồm cả tài khoản khách hàng và tài khoản đang bị khóa.
- **AssigneeLacksPermission** (422): Người nhận khi giao hoặc chuyển giao không có `supervision.complete`, đọc từ `UserRole` và `RolePermission` trong database.
- **ResourceAlreadyAssigned** (409): Gói đã có phân công đang hiệu lực, gồm cả trường hợp hai yêu cầu giao song song và index `UX_Assignment_ActiveResource` chặn yêu cầu đến sau. Người quản trị dùng chuyển giao để đổi người.
- **ResourceNotAssignable** (409): Gói chưa gán công trình hoặc đang bị hủy nên không giao mới hay chuyển giao được. Gỡ phân công vẫn làm được.
- **AssignmentResourceNotFound** (404): `resourceId` không trỏ tới gói giám sát nào.
- **DuplicateAssignment** (409): Chuyển giao cho chính người đang phụ trách gói đó.
- **AssignmentNotFound** (404): Phân công không tồn tại.
- **AssignmentAlreadyEnded** (409): Phân công đã kết thúc hiệu lực, không chuyển giao hoặc gỡ lại được.
- **ResourceTypeUnknown** (422): `resourceType` khác `SupervisionGrant`. Kiểm ở validator nên trả 422, cùng nhánh với các lỗi đầu vào khác.

`AssigneeLacksPermission`, `ResourceAlreadyAssigned`, `ResourceNotAssignable` và `AssignmentResourceNotFound` là mã mới ngày 25/09/2026, đã có trong `AccessErrorCodes.cs`. `AssigneeLacksPermission` ném bằng `ValidationException` của tầng application, nên trả 422 với một phần tử trong `errors` cho trường `StaffUserId`. `ResourceAlreadyAssigned` và `ResourceNotAssignable` dùng `ConflictException`; `ResourceAlreadyAssigned` còn được `ConstraintViolationPipelineBehavior` ánh xạ theo tên index khi lỗi đến từ vi phạm `23505`. `AssignmentResourceNotFound` dùng `NotFoundException`. Số `currentAssignmentId` đi qua `DomainException.Extensions` và thành trường cùng cấp trong thân lỗi.

## References

### User Stories

- STORY-RBAC-003
- STORY-RBAC-003/Exception Flow: EXC-05 gói đã có người phụ trách; EXC-06 người nhận thiếu `supervision.complete`; EXC-07 gói chưa gán công trình hoặc đang bị hủy.
- STORY-RBAC-003/Alternative Flow: ALT-06 gói bị hủy rồi khôi phục; ALT-07 gói mới gắn vào cùng công trình.
- STORY-RBAC-003/Acceptance Criteria: AC-004, AC-009, AC-010, AC-011 và AC-012.
- STORY-SITE-002/Alternative Flow: ALT-01 nhân viên có `supervision.complete` chỉ xem công trình của gói đang phụ trách.

### Business Rules

- BR-RBAC-001/Then
- BR-RBAC-005/Then
- BR-RBAC-007/Then
- BR-RBAC-008/Then
- BR-RBAC-010/Then
- BR-RBAC-011/Then
- BR-RBAC-012/Then
- BR-RBAC-013/Then
- BR-SUB-006/Then
- BR-SUB-011/Then
- BR-SUB-012/Then
- BR-SUB-024/Then
- BR-SUB-025/Then
- BR-SITE-003/Then

### Use Cases

### Others

- Tài liệu kỹ thuật: [TDD-RBAC-001](TDD-RBAC-001.md) mô hình vai trò – quyền, sửa quyền vai trò và nhật ký; [TDD-RBAC-002](TDD-RBAC-002.md) vòng đời tài khoản nhân viên và thu hồi vai trò.
- Tài liệu kỹ thuật: [TDD-SUB-004](TDD-SUB-004.md) bảng `SupervisionGrant` và tập trạng thái giữ chỗ; [TDD-SUB-005](TDD-SUB-005.md) hủy và khôi phục không đụng `Assignment`; [TDD-SUB-006](TDD-SUB-006.md) dùng `IAssignmentAuthorizer.IsDirectlyAssignedAsync` cho hoàn thành/mở lại gói giám sát. [TDD-SUB-003](TDD-SUB-003.md) đã bị TDD-SUB-004/005/006 thay thế.
- Tài liệu kỹ thuật: [TDD-SITE-001](TDD-SITE-001.md) dùng truy vấn phạm vi xem ở mục Architecture và cung cấp bảng công trình cho tên công trình trong danh sách cần chia lại.
- System Test liên quan: ST-RBAC-023 đến ST-RBAC-030 (AC-001 đến AC-008), ST-RBAC-053 (AC-009), ST-RBAC-054 (AC-010), ST-RBAC-055 (giao song song), ST-RBAC-058 (AC-011, EXC-07), ST-RBAC-059 và ST-RBAC-060 (AC-012, ALT-06, ALT-07), ST-RBAC-061 (gói đã hoàn thành), ST-RBAC-062 (thu hồi vai trò khi còn gói bị hủy); ST-SITE-023 đến ST-SITE-026 cho phạm vi xem công trình theo phân công.
- [Nợ kỹ thuật](../debt/assignment-resource-check.md) về việc kiểm tài nguyên tồn tại: đóng được khi handler giao việc kiểm gói theo thiết kế này.
- Đặc tả Unit Test: UT-RBAC-048 đến UT-RBAC-061, UT-RBAC-068 đến UT-RBAC-073 và UT-RBAC-081 đến UT-RBAC-090, UT-RBAC-092. UT-RBAC-051 đã rút khỏi nghiệm thu. Mã test ở `test/bmt-be.application.tests/usecases/assignment/` và `test/bmt-be.application.tests/behaviors/ConstraintViolationPipelineBehaviorTests.cs`; integration test khóa dòng và index ở `test/bmt-be.integration.tests/AssignmentConcurrencyTests.cs`. Chạy đạt ngày 25/09/2026: unit 338/338, integration 149/149. Chưa có mã test cho UT-RBAC-089 (phạm vi xem công trình, nay kiểm qua UT-SITE ở [TDD-SITE-001](TDD-SITE-001.md)) và UT-RBAC-090 (migration). Các ca UT-RBAC-049, UT-RBAC-050, UT-RBAC-052, UT-RBAC-053 chưa được ghi mã truy vết trong test.

## Change Log

- 2026-09-25 (đồng bộ code): Đồng bộ với code đã triển khai ở commit `182e2a8`: phân công theo gói, kiểm gói có thật và đang giữ chỗ, kiểm quyền người nhận, `UX_Assignment_ActiveResource`, danh sách cần chia lại và `ConstraintViolationPipelineBehavior` đã có. Viết lại mục migration theo migration gộp đã chạy thử. Ghi rõ `TargetLabel` chưa đổi sang tên công trình; phản hồi danh sách cần chia lại không kèm tên khách hàng.
- 2026-09-25: Cập nhật theo US/BR chốt lần hai trong ngày 25/09/2026, khi chuẩn bị tính năng Công trình. Đối tượng phân công đổi từ công trình sang gói giám sát: `ResourceType` duy nhất là `SupervisionGrant`, `ResourceId` là `SupervisionGrant.Id`. Giao và chuyển giao khóa dòng gói bằng `FOR SHARE` và chỉ nhận gói `Assigned`/`Completed`, thêm lỗi 409 `ResourceNotAssignable` và 404 `AssignmentResourceNotFound` (`BR-RBAC-013` khoản 8, `EXC-07`, `AC-011`). Hủy và khôi phục gói không đổi phân công; gói mới gắn vào cùng công trình không kế thừa người phụ trách (khoản 3, 9; `ALT-06`, `ALT-07`, `AC-012`). Thêm mục thứ tự khóa giữa các luồng. Danh sách cần chia lại nay triển khai được, chỉ gồm gói `Assigned`, trạng thái đổi tên thành `NoAssignee`/`AssigneeLocked`. Thêm truy vấn phạm vi xem công trình theo gói đang phụ trách cho `BR-SITE-003`. Kế hoạch migration xóa toàn bộ dòng `Customer`/`Project` của dữ liệu dev/test thay vì đổi nhãn. Viết lại dữ liệu mẫu theo gói.
- 2026-09-25: Cập nhật theo US/BR đã chốt ngày 25/09/2026. Chỉ còn một loại tài nguyên `ConstructionSite`; bỏ phân công mức khách hàng, nhánh kế thừa, `IsAssignedAsync` và `IResourceHierarchyReader`. Mỗi công trình một người phụ trách: index duy nhất có lọc chuyển sang `(ResourceType, ResourceId) WHERE EffectiveToUtc IS NULL`; giao công trình đã có người trả 409 `ResourceAlreadyAssigned` (`EXC-05`, `AC-004`), kể cả khi hai yêu cầu chạy song song; chuyển giao đóng dòng cũ trước khi chèn dòng mới. Người nhận phải có `supervision.complete` đọc từ database, thiếu thì 422 `AssigneeLacksPermission` (`EXC-06`, `AC-009`); giao và chuyển giao khóa dòng `User` của người nhận. Thu hồi vai trò và sửa quyền vai trò theo `BR-RBAC-007` mới. Thay `GET /assignments/unassigned` bằng danh sách cần chia lại gồm công trình chưa có người và công trình có người phụ trách bị khóa (`AC-010`), vẫn chờ đặc tả Công trình. Nhánh Admin nhận diện theo mã vai trò hệ thống `admin`; lỗi thiếu phân công khi hoàn thành gói là `ConstructionSiteNotAssignedToActor` theo TDD-SUB-006; tham chiếu chuyển từ TDD-SUB-003 (đã bị thay thế) sang TDD-SUB-004/005/006. Viết lại dữ liệu mẫu và thêm kế hoạch migration schema cho dữ liệu dev/test; bổ sung tham chiếu `BR-RBAC-001`.
- 2026-09-23: Sửa Sequence Diagram cho khớp bảng BR-RBAC-012: chuyển giao ghi một dòng nhật ký `AssignmentTransferred` với người cũ ở `BeforeJson` và người mới ở `AfterJson`, thay vì hai dòng `AssignmentEnded` và `AssignmentCreated`. Hai dòng sẽ làm hành động `AssignmentTransferred` trong bảng không bao giờ được dùng.
- 2026-09-23: Đã triển khai bốn endpoint phân công, `IAssignmentAuthorizer` và khóa dòng khi chuyển giao. `IResourceHierarchyReader` mới có bản tạm ném lỗi, và việc kiểm tài nguyên tồn tại được ghi thành nợ kỹ thuật. `GET /assignments/unassigned` vẫn chưa triển khai được.
- 2026-09-20: Bỏ nhắc tới trạng thái `PendingActivation` theo quyết định bỏ luồng mời qua email ở [TDD-RBAC-002](TDD-RBAC-002.md). Cơ chế phân công và chuyển giao không đổi.
