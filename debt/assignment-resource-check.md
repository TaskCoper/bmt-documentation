# Nợ kỹ thuật: phân công không kiểm tài nguyên có thật

**Trạng thái: Đã trả phần code ngày 25/09/2026, chưa đóng — còn chờ migration chạy trên các môi trường thật (điều kiện 2).**

Theo mô hình người dùng chốt ngày 25/09/2026, phân công chỉ có một loại tài nguyên là gói giám sát (`ResourceType = 'SupervisionGrant'`, `ResourceId` là `SupervisionGrant.Id`). Mỗi gói có tối đa một người phụ trách, người nhận phải là nhân viên đang hoạt động có quyền `supervision.complete`, và chỉ giao được gói đã gán công trình hoặc đã hoàn thành. Bản thiết kế trước trong cùng ngày phân công theo công trình, nên khoản nợ khi đó phải chờ module Công trình; bản đó đã được thay.

Trước ngày 25/09/2026, code chạy mô hình cũ, với hai loại tài nguyên `Customer` và `Project`, cho nhiều người cùng phụ trách, chưa kiểm quyền của người nhận và **không đối chiếu `resourceId` với tài nguyên có thật**. Đó là khoản nợ của tài liệu này. Mô hình mới của [TDD-RBAC-003](../tdd/TDD-RBAC-003.md) đã được triển khai ở commit `182e2a8` trên nhánh `feature/construction-site` của `bmt-be`; tình trạng từng điều kiện đóng nợ ghi ở mục "Điều kiện đóng nợ".

## Vì sao từng chấp nhận

Bảng `Assignment` cố ý không có khóa ngoại tới tài nguyên, để sau này thêm loại mới, ví dụ lead, chỉ cần một giá trị mới trong `ResourceType` chứ không cần bảng mới. [TDD-RBAC-003](../tdd/TDD-RBAC-003.md) giữ đánh đổi này và đặt việc kiểm tài nguyên tồn tại vào handler.

Khi code được viết ngày 23/09/2026, tài nguyên là khách hàng và dự án, còn module dự án chưa có bảng để tra. Handler vì vậy nhận mọi `resourceId` hợp lệ về định dạng.

## Vì sao nay trả được

Tài nguyên được phân công nay là gói giám sát, và bảng `SupervisionGrant` đã có trong code theo [TDD-SUB-004](../tdd/TDD-SUB-004.md). Gói không bao giờ bị xóa: nghiệp vụ chỉ hủy gói, và cấu hình EF bỏ qua `IsDeleted`. Vì vậy:

- Handler giao việc đọc dòng gói bằng `SELECT "State" FROM "SupervisionGrant" WHERE "Id" = @grantId FOR SHARE`. Không có dòng thì trả 404 `AssignmentResourceNotFound`; gói chưa gán hoặc đang bị hủy thì trả 409 `ResourceNotAssignable`. Chuyển giao đọc cùng dòng này, vì cũng phải kiểm trạng thái gói.
- Phân công đã tạo hợp lệ không thể trở thành dòng mồ côi, vì gói nó trỏ tới không bị xóa.
- Không cần cổng đọc công trình trong luồng phân công. Cổng `IConstructionSiteOwnershipReader` vẫn thuộc luồng gán gói vào công trình ở TDD-SUB-004, không thuộc phân công.

Vẫn không thêm khóa ngoại, vì cột `ResourceId` dùng chung cho nhiều loại tài nguyên. Lý do và mốc xem lại ghi ở mục "Một bảng cho nhiều loại tài nguyên" của TDD-RBAC-003.

## Hệ quả trước khi triển khai

Các hệ quả dưới đây đúng với code trước ngày 25/09/2026, và vẫn đúng với môi trường nào chưa chạy migration mới:

- Gõ nhầm `resourceId` vẫn tạo được dòng phân công trỏ tới tài nguyên không tồn tại, và không có gì báo.
- Phép kiểm quyền vẫn an toàn. `IAssignmentAuthorizer.IsDirectlyAssignedAsync` hỏi "người này có đang phụ trách tài nguyên kia không" chứ không hỏi ngược lại, nên một dòng mồ côi không cấp quyền cho ai trên tài nguyên có thật.
- Dòng mồ côi vẫn tính vào số phân công người đó đang có, nên vẫn chặn thu hồi vai trò hoặc bỏ quyền khỏi vai trò theo BR-RBAC-007.

## Cách trả nợ

Đã làm cùng đợt triển khai mô hình phân công theo gói của TDD-RBAC-003, ngày 25/09/2026:

1. Handler giao việc (`CreateAssignmentCommandHandler`) và handler chuyển giao (`TransferAssignmentCommandHandler`) gọi `AssignmentLookup.RequireAssignableGrantAsync`: khóa và đọc dòng `SupervisionGrant` qua `IAssignmentRowLocker.LockSupervisionGrantStateForShareAsync` như trên, trước khi kiểm người nhận.
2. Migration `20260925074152_ConstructionSiteAndPackageAssignment` xóa mọi dòng `Assignment` loại `Customer` và `Project`, rồi đổi CHECK sang `('SupervisionGrant')`. Vì các dòng cũ bị xóa chứ không đổi nhãn, không còn dòng nào trỏ tới tài nguyên không có thật.
3. Unit Test ở `test/bmt-be.application.tests/usecases/assignment/AssignmentCommandHandlerTests.cs`: `Create_GrantMissing_Throws404` (UT-RBAC-084), `Create_GrantNotHoldingASite_Throws409ResourceNotAssignable` (UT-RBAC-082) và `Transfer_CanceledGrant_Throws409AndKeepsCurrentHolder` (UT-RBAC-083). Integration test `AssignmentLocker_ReadsGrantStateForShare` kiểm câu khóa `FOR SHARE` trên PostgreSQL thật. Bộ unit 338/338 và integration 149/149 chạy đạt ngày 25/09/2026. ST-RBAC-058 kiểm nhánh gói chưa gán và gói đang bị hủy; chưa chạy.

## Điều kiện đóng nợ

Đóng nợ khi đủ ba điều kiện. Tình trạng ngày 25/09/2026:

1. **Đạt.** Handler giao việc và chuyển giao từ chối gói không tồn tại (404 `AssignmentResourceNotFound`) và gói chưa gán hoặc đang bị hủy (409 `ResourceNotAssignable`), có Unit Test cho cả hai nhánh. Chuyển giao dùng chung bước kiểm với giao việc; nhánh 404 riêng của chuyển giao chưa có test, vì phân công đang có luôn trỏ tới gói có thật.
2. **Chưa đạt.** Migration mới chỉ chạy thử trên PostgreSQL tạm có dữ liệu cũ: sau `Up`, CHECK chỉ nhận `SupervisionGrant` và không còn dòng `Customer` hay `Project`. Chưa chạy trên môi trường dev dùng chung hay production. Đóng điều kiện này khi đã chạy trên mọi môi trường và kiểm lại hai điểm trên.
3. **Đạt.** Code không có đường xóa nào cho `SupervisionGrant`. Nếu sau này thêm đường xóa gói, hoặc thêm một loại tài nguyên có thể bị xóa, phải thiết kế cách xử lý phân công của tài nguyên đó trước khi mở, nếu không khoản nợ mở lại. Thiết kế ngày 25/09/2026 (lần 3, chưa có trong code) thêm hai đường làm gói mất tư cách được phân công là hủy và gỡ gói; cả hai kết thúc phân công của gói trong cùng transaction theo [TDD-RBAC-003](../tdd/TDD-RBAC-003.md) và [TDD-SUB-007](../tdd/TDD-SUB-007.md), nên điều kiện này vẫn đạt.

## Tài liệu liên quan

- [TDD-RBAC-003](../tdd/TDD-RBAC-003.md): mục Architecture nêu đánh đổi, bước kiểm gói và thứ tự khóa; mục Data Model nêu migration đã tạo.
- [TDD-SUB-004](../tdd/TDD-SUB-004.md): bảng `SupervisionGrant` và tập trạng thái giữ chỗ.
- [STORY-RBAC-003](../userstory/STORY-RBAC-003.md) và [BR-RBAC-013](../businessrule/BR-RBAC-013.md): luồng và quy tắc phân công gói giám sát.

Chưa có thời hạn hay người phụ trách được giao cho việc chạy migration trên các môi trường. Khi điều kiện 2 đạt thì đổi trạng thái ở đầu tài liệu thành đã đóng.
