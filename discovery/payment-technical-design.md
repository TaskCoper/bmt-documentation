# Bàn giao thiết kế thanh toán và gói đã mua

Đã lưu 4 TDD và 74 đặc tả Unit Test cho 4 User Story mới. Bộ System Test thanh toán hiện có 70 ca, gồm 7 ca bổ sung kiểm tra transaction và xử lý đồng thời. Tất cả là bản nháp thiết kế, chưa nhập vào Document First. Kiểm code ngày 25/09/2026: phần gán, hủy và khôi phục gói (TDD-SUB-004, TDD-SUB-005) đã có handler và migration. Từ commit `182e2a8` của `bmt-be`, gói giám sát dùng cột `ConstructionSiteId` gắn với công trình và thao tác đổi công trình đã bỏ. Phần đơn, giao dịch và tra cứu (TDD-PAY-001, TDD-PAY-002) chưa có code. Chưa chạy các test trong bộ đặc tả này.

## Cập nhật ngày 25/09/2026

Bốn TDD được sửa theo US/BR người dùng chốt ngày 25/09/2026. Các thay đổi chính:

- Gói giám sát gắn với **công trình**, một thực thể riêng khác bản dự toán do khách tự tạo. Tên kỹ thuật là `ConstructionSite`: cột `ConstructionSiteId`, `OfferKey = ConstructionSite` và cổng đọc `IConstructionSiteOwnershipReader` thay `IProjectOwnershipReader`. Story, BR và TDD của Công trình đã có (STORY-SITE-001/002, BR-SITE-001..003, TDD-SITE-001) và đã có code ở commit `182e2a8`.
- Tài khoản nhân viên không được tạo hoặc hủy đơn mua gói, kể cả khi có quyền tra cứu; hệ thống trả 403 theo BR-RBAC-005 (STORY-PAY-001/EXC-08, AC-028).
- Mọi luồng đụng tới gói và lượt của một khách khóa `AccountCommerceState` trước, rồi mới tới kỳ, lượt và dữ liệu khác (TDD-PAY-001).
- Gói giám sát chốt tên, giá và mô tả dịch vụ theo đơn qua revision, không dùng danh mục quyền lợi (BR-SUB-004 khoản 5, BR-SUB-008 khoản 7).
- Thao tác nhân viên kiểm theo mã quyền trong danh mục 13 mã của TDD-RBAC-001: `commerce.read`, `package.cancel`, `package.restore`. Mã `supervision.reassign` đã bỏ ngày 25/09/2026 cùng với việc bỏ đổi công trình của gói (BR-SUB-009).

Test bổ sung: ST-PAY-069 (thu hồi `commerce.read` có hiệu lực theo hạn token hoặc ngay khi buộc đăng xuất), ST-PAY-070 và UT-PAY-071 đến UT-PAY-074. Sau khi bỏ đổi công trình và chuyển sang phân công theo gói (25/09/2026, lần 2): UT-PAY-043, UT-PAY-045 đã rút; UT-PAY-040, 042, 044, 046, 048, 049, 064 đã sửa; thêm UT-PAY-075 đến UT-PAY-081. Các mục bên dưới giữ nội dung bàn giao ban đầu; chỗ nào nói “dự án” của gói giám sát thì đọc là công trình.

## Tài liệu theo chức năng

| User Story | Quy tắc chính | TDD | Unit Test |
| --- | --- | --- | --- |
| [STORY-PAY-001](../userstory/STORY-PAY-001.md) | BR-PAY-001–004, BR-RBAC-005 | [TDD-PAY-001](../tdd/TDD-PAY-001.md) | UT-PAY-001–036, UT-PAY-071–074, UT-PAY-111–113, UT-PAY-120 |
| [STORY-SUB-004](../userstory/STORY-SUB-004.md) | BR-SUB-022, BR-SUB-009 (BR-SUB-023 đã bỏ) | [TDD-SUB-004](../tdd/TDD-SUB-004.md) | UT-PAY-037–042, 044, 046–048, 075–077 (043, 045 đã rút) |
| [STORY-SUB-005](../userstory/STORY-SUB-005.md) | BR-SUB-024–025 | [TDD-SUB-005](../tdd/TDD-SUB-005.md) | UT-PAY-049–062, 078–079 |
| [STORY-PAY-002](../userstory/STORY-PAY-002.md) | BR-PAY-005 | [TDD-PAY-002](../tdd/TDD-PAY-002.md) | UT-PAY-063–070, 080–081 |
| [STORY-PAY-003](../userstory/STORY-PAY-003.md) (chốt 26/09/2026) | BR-PAY-006 | [TDD-PAY-001](../tdd/TDD-PAY-001.md), mục Quản trị connection | UT-PAY-114–127 (trừ 120); System Test ST-PAY-089–101 |

## Đã xác nhận

- Khoản tiền phải phát sinh trước hạn 15 phút và trước lúc hủy; đúng mốc không hợp lệ. Các khoản chuyển hợp lệ được cộng dồn, không kéo dài hạn.
- Hai đơn thiết kế cùng thời điểm đủ tiền theo độ chính xác SePay: đơn tạo sau được xem là lần mua sau.
- Nếu A đã thay B rồi giao dịch đến muộn chứng minh A thực ra đủ tiền trước B, giữ A. Ghi sai lệch để nhân viên xử lý bên ngoài; không tự khôi phục B hoặc làm mới hạn/lượt.
- Hạn gán giám sát là cùng ngày và giờ năm sau theo giờ Việt Nam; 29/02 chuyển thành 28/02 nếu cần. Phải gán trước hạn; đã gán đúng hạn thì tiếp tục phục vụ sau hạn.
- Hủy gói thiết kế chỉ chặn tác vụ mới. Tác vụ AI đã bắt đầu hợp lệ được hoàn tất, quyết toán vào kỳ và lượt đã giữ.

## Phương án kỹ thuật

Lưu webhook vào cơ sở dữ liệu trước khi trả thành công. Worker xử lý dữ liệu đã lưu, tự thử lại khi lỗi. Khóa theo tài khoản và ràng buộc duy nhất của PostgreSQL bảo vệ tiền, cấp gói và liên kết công trình khi nhiều yêu cầu cùng đến. Tách trạng thái nhận tiền, cấp gói và hiệu lực gói để không nhầm “đã trả tiền” với “đang được sử dụng”.

Đơn giữ bản giá/quyền lợi tại lúc tạo. Giao dịch lưu thời điểm thực tế riêng với thời điểm thứ tự đã áp dụng. Các thao tác nhân viên phải có quyền hiện hành, lý do và lịch sử trong cùng transaction. Tra cứu quản trị bao gồm giao dịch chưa khớp đơn, không có thao tác hoàn tiền hoặc gán giao dịch thủ công.

Đây là lựa chọn kỹ thuật cho các nghiệp vụ đã chốt. Không cần duyệt lại từng tên lớp hoặc trường dữ liệu.

## Thứ tự triển khai và điều kiện còn thiếu

1. Hoàn thiện danh mục gói/revision/giá, kỳ và quota theo các TDD subscription, áp dụng phần thay đổi được ghi trong TDD thanh toán. Các bảng này chưa có trong backend đã khảo sát.
2. Xây quyền nhân viên, transaction và nguồn dữ liệu công trình có kiểm tra chủ sở hữu. Chưa bật gán công trình nếu chưa có module Công trình thật và cơ chế khóa tương thích.
3. Triển khai đơn, tiếp nhận SePay, worker cấp gói và các thao tác gói; sau đó xây tra cứu quản trị.
4. Cung cấp tài khoản ngân hàng, connection SePay, secret và cấu hình webhook của môi trường triển khai. Đối chiếu payload/chữ ký thực tế trước khi bật nhận tiền thật.
5. Viết và chạy unit/integration/system test; không dùng mock để kết luận transaction PostgreSQL đã đúng.

Chưa mở phạm vi nhân viên gỡ gói về chưa gán hoặc sửa liên kết khi gói đang bị hủy. Chưa phân công Owner/người triển khai. Reviewer và Approver của bản nháp là Tân Trần; metadata này không có nghĩa tài liệu đã được duyệt.

## Căn cứ khảo sát

- [ApplicationDbContext](../../bmt-be/src/bmt-be.persistence/ApplicationDbContext.cs) lúc khảo sát chỉ có Users; không mô tả các bảng đề xuất như thành phần đã triển khai. Kiểm lại ngày 25/09/2026: code đã có các bảng RBAC, danh mục gói, kỳ thiết kế, lượt, vòng đời gói, gói giám sát và phân công; vẫn chưa có bảng đơn, giao dịch hay `AccountCommerceState`.
- [TransactionPipelineBehavior](../../bmt-be/src/bmt-be.application/behaviors/TransactionPipelineBehavior.cs) commit khi handler trả bình thường. Khi đã ghi dữ liệu rồi gặp lỗi cần hoàn tác, phải ném lỗi để transaction rollback, không chỉ trả Result.Failure.
- `RoleNames.cs` lúc khảo sát chưa có quyền nghiệp vụ nhân viên trong thiết kế này. File này nay không còn; danh mục mã quyền nằm ở [PermissionNames](../../bmt-be/src/bmt-be.contract/constants/PermissionNames.cs) và mã vai trò hệ thống ở [RoleCodes](../../bmt-be/src/bmt-be.contract/constants/RoleCodes.cs).
- **Cập nhật 20/09/2026:** mô hình quyền `StaffAccessProfile` và `StaffPermission` ghi trong biên bản này đã được thay bằng mô hình RBAC chuẩn ở [TDD-RBAC-001](../tdd/TDD-RBAC-001.md). Bốn mã quyền giữ nguyên tên, chỗ gắn quyền chuyển từ người sang vai trò. Phần bên dưới giữ nguyên làm biên bản tại thời điểm chốt thanh toán, không dùng làm thiết kế hiện hành.
- Đã đối chiếu [tích hợp webhook](https://developer.sepay.vn/vi/sepay-webhooks/tich-hop-webhook), [xác thực](https://developer.sepay.vn/vi/sepay-webhooks/xac-thuc) và [tạo QR](https://developer.sepay.vn/vi/sepay-webhooks/tao-qr-va-form-thanh-toan) của SePay. Chưa kiểm tra tài khoản hoặc giao dịch thật.

## Phạm vi rà soát

TDD subscription cũ đã có ghi chú chỉ rõ phần bị thay thế và liên kết sang thiết kế mới. Chưa tuyên bố đồng bộ toàn bộ đặc tả Unit Test lịch sử. Việc lập danh sách 263 mã trong chuỗi tham chiếu trước đó chỉ kiểm tra sự tồn tại, không chứng minh đã đọc và đánh giá đầy đủ nội dung của cả 263 tài liệu. Cần rà soát tiếp các tài liệu lịch sử khi triển khai phần phụ thuộc; không dùng kết quả kiểm tra liên kết thay cho kiểm tra nghiệp vụ.

## Danh mục Unit Test

| Mã | Unit dự kiến | Truy vết |
| --- | --- | --- |
| [UT-PAY-001](../unittest/UT-PAY-001.md) | CreatePaymentOrderHandler | BR-PAY-001/Then, STORY-PAY-001/AC-001, TDD-PAY-001/Architecture |
| [UT-PAY-002](../unittest/UT-PAY-002.md) | CreatePaymentOrderHandler | BR-PAY-001/Then, TDD-PAY-001/Architecture |
| [UT-PAY-003](../unittest/UT-PAY-003.md) | CreatePaymentOrderHandler | TDD-PAY-001/Internal API |
| [UT-PAY-004](../unittest/UT-PAY-004.md) | CreatePaymentOrderHandler | STORY-PAY-001/AC-004, TDD-PAY-001/Architecture |
| [UT-PAY-005](../unittest/UT-PAY-005.md) | CreatePaymentOrderHandler | BR-PAY-003/Then, TDD-PAY-001/Architecture |
| [UT-PAY-006](../unittest/UT-PAY-006.md) | CreatePaymentOrderHandler | STORY-PAY-001/AC-005, TDD-PAY-001/Architecture |
| [UT-PAY-007](../unittest/UT-PAY-007.md) | CreatePaymentOrderHandler | BR-PAY-001/Except, TDD-PAY-001/Internal API |
| [UT-PAY-008](../unittest/UT-PAY-008.md) | PaymentOrderValidator | TDD-PAY-001/Data Model |
| [UT-PAY-009](../unittest/UT-PAY-009.md) | PaymentEligibilityPolicy | BR-PAY-002/Then, STORY-PAY-001/AC-007 |
| [UT-PAY-010](../unittest/UT-PAY-010.md) | PaymentEligibilityPolicy | STORY-PAY-001/AC-006, TDD-PAY-001/Architecture |
| [UT-PAY-011](../unittest/UT-PAY-011.md) | PaymentEligibilityPolicy | BR-PAY-002/Then, TDD-PAY-001/Architecture |
| [UT-PAY-012](../unittest/UT-PAY-012.md) | PaymentEligibilityPolicy | STORY-PAY-001/AC-024, BR-PAY-002/Then |
| [UT-PAY-013](../unittest/UT-PAY-013.md) | PaymentEligibilityPolicy | STORY-PAY-001/AC-025, BR-PAY-003/Then |
| [UT-PAY-014](../unittest/UT-PAY-014.md) | PaymentEligibilityPolicy | STORY-PAY-001/AC-012, TDD-PAY-001/Architecture |
| [UT-PAY-015](../unittest/UT-PAY-015.md) | PaymentEligibilityPolicy | STORY-PAY-001/AC-014, TDD-PAY-001/Architecture |
| [UT-PAY-016](../unittest/UT-PAY-016.md) | SePayPayloadMapper | TDD-PAY-001/External API |
| [UT-PAY-017](../unittest/UT-PAY-017.md) | SePayPayloadMapper | TDD-PAY-001/External API, TDD-PAY-001/Data Model |
| [UT-PAY-018](../unittest/UT-PAY-018.md) | SePayEnvelopeVerifier | TDD-PAY-001/External API |
| [UT-PAY-019](../unittest/UT-PAY-019.md) | SePayEnvelopeVerifier | TDD-PAY-001/External API |
| [UT-PAY-020](../unittest/UT-PAY-020.md) | SePayEnvelopeVerifier | TDD-PAY-001/External API |
| [UT-PAY-021](../unittest/UT-PAY-021.md) | RecordBankTransactionHandler | STORY-PAY-001/AC-013, TDD-PAY-001/Architecture |
| [UT-PAY-022](../unittest/UT-PAY-022.md) | RecordBankTransactionHandler — webhook lệch dữ kiện, lưu bản ghi lệch, chỉ báo lần đầu mỗi nội dung lệch (sửa ngày 26/09/2026) | TDD-PAY-001/Architecture, TDD-PAY-001/Data Model |
| [UT-PAY-023](../unittest/UT-PAY-023.md) | PaymentCodeMatcher | BR-PAY-005/Then, TDD-PAY-001/Architecture |
| [UT-PAY-024](../unittest/UT-PAY-024.md) | PaymentCodeMatcher | TDD-PAY-001/Architecture |
| [UT-PAY-025](../unittest/UT-PAY-025.md) | ProcessBankTransactionHandler | TDD-PAY-001/Architecture |
| [UT-PAY-026](../unittest/UT-PAY-026.md) | CancelPaymentOrderHandler | BR-PAY-003/Then, TDD-PAY-001/Architecture |
| [UT-PAY-027](../unittest/UT-PAY-027.md) | CancelPaymentOrderHandler | STORY-PAY-001/AC-015, TDD-PAY-001/Architecture |
| [UT-PAY-028](../unittest/UT-PAY-028.md) | PurchaseOrderingPolicy | STORY-PAY-001/AC-020, TDD-PAY-001/Architecture |
| [UT-PAY-029](../unittest/UT-PAY-029.md) | PurchaseOrderingPolicy | STORY-PAY-001/AC-026, BR-PAY-004/Except |
| [UT-PAY-030](../unittest/UT-PAY-030.md) | PurchaseOrderingPolicy | STORY-PAY-001/AC-027, BR-PAY-004/Except |
| [UT-PAY-031](../unittest/UT-PAY-031.md) | PackageFulfillmentService | STORY-PAY-001/AC-019, TDD-PAY-001/Architecture |
| [UT-PAY-032](../unittest/UT-PAY-032.md) | PackageFulfillmentService | STORY-PAY-001/AC-010, STORY-PAY-001/AC-021, TDD-PAY-001/Architecture |
| [UT-PAY-033](../unittest/UT-PAY-033.md) | PaymentProcessingWorker | TDD-PAY-001/Architecture |
| [UT-PAY-034](../unittest/UT-PAY-034.md) | PaymentProcessingWorker | TDD-PAY-001/Architecture |
| [UT-PAY-035](../unittest/UT-PAY-035.md) | PaymentQrBuilder | STORY-PAY-001/AC-006, TDD-PAY-001/External API |
| [UT-PAY-036](../unittest/UT-PAY-036.md) | PaymentQrBuilder | TDD-PAY-001/Internal API |
| [UT-PAY-037](../unittest/UT-PAY-037.md) | AssignmentDeadlineCalculator | STORY-SUB-004/AC-011, TDD-SUB-004/Architecture |
| [UT-PAY-038](../unittest/UT-PAY-038.md) | AssignmentDeadlineCalculator | BR-SUB-022/Notes, TDD-SUB-004/Architecture |
| [UT-PAY-039](../unittest/UT-PAY-039.md) | SupervisionAssignmentPolicy | STORY-SUB-004/AC-012, TDD-SUB-004/Architecture |
| [UT-PAY-040](../unittest/UT-PAY-040.md) | AssignSupervisionGrantCommandHandler | STORY-SUB-004/AC-001, TDD-SUB-004/Architecture, TDD-SUB-004/Data Model |
| [UT-PAY-041](../unittest/UT-PAY-041.md) | SupervisionAssignmentPolicy | STORY-SUB-004/AC-004, TDD-SUB-004/Architecture |
| [UT-PAY-042](../unittest/UT-PAY-042.md) | AssignSupervisionGrantCommandHandler | STORY-SUB-004/AC-010, STORY-SUB-004/EXC-06, TDD-SUB-004/Architecture, TDD-SUB-004/Internal API |
| [UT-PAY-043](../unittest/UT-PAY-043.md) | ReassignSupervisionGrantHandler — đã rút ngày 25/09/2026 | STORY-SUB-004/AC-007, TDD-SUB-004/Architecture |
| [UT-PAY-044](../unittest/UT-PAY-044.md) | AssignSupervisionGrantCommandHandler | STORY-SUB-004/AC-005, STORY-SUB-004/EXC-03, BR-SUB-009/Except, TDD-SUB-004/Internal API, TDD-SUB-004/State Diagram |
| [UT-PAY-045](../unittest/UT-PAY-045.md) | ReassignSupervisionValidator — đã rút ngày 25/09/2026 | TDD-SUB-004/Internal API |
| [UT-PAY-046](../unittest/UT-PAY-046.md) | AssignSupervisionGrantCommandHandler | TDD-SUB-004/Architecture, TDD-SUB-004/Internal API |
| [UT-PAY-047](../unittest/UT-PAY-047.md) | SupervisionStatusProjector | STORY-SUB-004/AC-003, TDD-SUB-004/State Diagram |
| [UT-PAY-048](../unittest/UT-PAY-048.md) | AssignmentReceiptPolicy | TDD-SUB-004/Architecture |
| [UT-PAY-049](../unittest/UT-PAY-049.md) | Truy vấn gộp quyền từ vai trò khi phát hành token | STORY-PAY-002/AC-007, TDD-RBAC-001/Architecture, TDD-SUB-005/Architecture |
| [UT-PAY-050](../unittest/UT-PAY-050.md) | PackageLifecyclePolicy.Cancel | STORY-SUB-005/AC-001, TDD-SUB-005/Architecture |
| [UT-PAY-051](../unittest/UT-PAY-051.md) | PackageLifecyclePolicy.Cancel | STORY-SUB-005/AC-002, STORY-SUB-005/AC-003, TDD-SUB-005/Architecture |
| [UT-PAY-052](../unittest/UT-PAY-052.md) | PackageMutationValidator | TDD-SUB-005/Internal API |
| [UT-PAY-053](../unittest/UT-PAY-053.md) | PackageLifecyclePolicy.Restore | STORY-SUB-005/AC-005, TDD-SUB-005/Architecture |
| [UT-PAY-054](../unittest/UT-PAY-054.md) | PackageLifecyclePolicy.Restore | BR-SUB-025/Then, TDD-SUB-005/Architecture |
| [UT-PAY-055](../unittest/UT-PAY-055.md) | PackageLifecyclePolicy.Restore | STORY-SUB-005/AC-012, TDD-SUB-005/Architecture |
| [UT-PAY-056](../unittest/UT-PAY-056.md) | PackageLifecyclePolicy.Restore | STORY-SUB-005/AC-008, STORY-SUB-005/AC-009, TDD-SUB-005/Architecture |
| [UT-PAY-057](../unittest/UT-PAY-057.md) | PackageLifecyclePolicy.Restore | STORY-SUB-005/AC-006, STORY-SUB-005/AC-011, TDD-SUB-005/Architecture |
| [UT-PAY-058](../unittest/UT-PAY-058.md) | PackageLifecyclePolicy.Restore | STORY-SUB-005/AC-007, TDD-SUB-005/Architecture |
| [UT-PAY-059](../unittest/UT-PAY-059.md) | UsageSettlementPolicy sau hủy | STORY-SUB-005/AC-014, TDD-SUB-005/Architecture |
| [UT-PAY-060](../unittest/UT-PAY-060.md) | RestorePackageHandler giữ counters | STORY-SUB-005/AC-014, TDD-SUB-005/Architecture |
| [UT-PAY-061](../unittest/UT-PAY-061.md) | PackageMutationReceiptPolicy | TDD-SUB-005/Architecture |
| [UT-PAY-062](../unittest/UT-PAY-062.md) | CancelPackageHandler audit failure | TDD-SUB-005/Architecture |
| [UT-PAY-063](../unittest/UT-PAY-063.md) | CommerceReadAuthorization | STORY-PAY-002/AC-001, TDD-PAY-002/Architecture |
| [UT-PAY-064](../unittest/UT-PAY-064.md) | CommerceReadAuthorization | STORY-PAY-002/AC-002, TDD-PAY-002/Architecture |
| [UT-PAY-065](../unittest/UT-PAY-065.md) | CommerceReadAuthorization | STORY-PAY-002/AC-003, TDD-PAY-002/Architecture |
| [UT-PAY-066](../unittest/UT-PAY-066.md) | TransactionDtoMapper | STORY-PAY-002/AC-004, TDD-PAY-002/Data Model |
| [UT-PAY-067](../unittest/UT-PAY-067.md) | PurchaseDtoMapper | STORY-PAY-002/AC-001, STORY-PAY-002/AC-006, TDD-PAY-002/Architecture |
| [UT-PAY-068](../unittest/UT-PAY-068.md) | CommerceQueryValidator | TDD-PAY-002/Internal API |
| [UT-PAY-069](../unittest/UT-PAY-069.md) | CommerceStatusProjector | STORY-PAY-002/AC-006, TDD-PAY-002/Architecture |
| [UT-PAY-070](../unittest/UT-PAY-070.md) | CommerceDtoProjection contract | TDD-PAY-002/Architecture |
| [UT-PAY-071](../unittest/UT-PAY-071.md) | CreatePaymentOrderHandler | BR-RBAC-005/Then, TDD-PAY-001/Internal API |
| [UT-PAY-072](../unittest/UT-PAY-072.md) | CancelPaymentOrderHandler | BR-RBAC-005/Then, TDD-PAY-001/Architecture |
| [UT-PAY-073](../unittest/UT-PAY-073.md) | CreatePaymentOrderHandler | BR-PAY-001/Then, TDD-PAY-001/Data Model |
| [UT-PAY-074](../unittest/UT-PAY-074.md) | CreatePaymentOrderHandler | TDD-PAY-001/Architecture |
| [UT-PAY-075](../unittest/UT-PAY-075.md) | AssignSupervisionGrantCommandHandler — thứ tự khóa | TDD-SUB-004/Architecture, TDD-SUB-004/Sequence Diagram |
| [UT-PAY-076](../unittest/UT-PAY-076.md) | Ánh xạ lỗi ràng buộc cho lệnh gán | TDD-SUB-004/Internal API, TDD-SUB-004/Architecture |
| [UT-PAY-077](../unittest/UT-PAY-077.md) | AssignSupervisionGrantCommandHandler — tài khoản nhân viên | BR-RBAC-005/Then, TDD-SUB-004/Architecture |
| [UT-PAY-078](../unittest/UT-PAY-078.md) | CancelPackageCommandHandler — giữ phân công | BR-SUB-024/Then, BR-RBAC-013/Then, TDD-SUB-005/Architecture |
| [UT-PAY-079](../unittest/UT-PAY-079.md) | RestorePackageCommandHandler — giữ phân công | BR-SUB-025/Then, BR-RBAC-013/Then, STORY-RBAC-003/ALT-06, TDD-SUB-005/Architecture |
| [UT-PAY-080](../unittest/UT-PAY-080.md) | Projection gói đã mua — tên công trình | STORY-PAY-002/AC-009, BR-PAY-005/Then, TDD-PAY-002/Data Model, TDD-PAY-002/Internal API |
| [UT-PAY-081](../unittest/UT-PAY-081.md) | Projection lịch sử — mốc gán công trình | TDD-PAY-002/Internal API, TDD-PAY-002/Data Model |
| [UT-PAY-111](../unittest/UT-PAY-111.md) | PaymentProcessingRunner — cảnh báo ở lần thử thứ 10, mỗi giao dịch một lần | TDD-PAY-001/Architecture |
| [UT-PAY-112](../unittest/UT-PAY-112.md) | Đánh dấu đã cảnh báo trên PostgreSQL | TDD-PAY-001/Architecture, TDD-PAY-001/Data Model |
| [UT-PAY-113](../unittest/UT-PAY-113.md) | Cảnh báo một lần cho mỗi nội dung webhook lệch (Q6) | TDD-PAY-001/Architecture, TDD-PAY-001/Data Model |
| [UT-PAY-114](../unittest/UT-PAY-114.md) | CreatePaymentConnectionCommandHandler | STORY-PAY-003/AC-001, AC-005, BR-PAY-006/Then, TDD-PAY-001/Internal API |
| [UT-PAY-115](../unittest/UT-PAY-115.md) | Validator API quản trị connection | TDD-PAY-001/Internal API, BR-PAY-006/Then |
| [UT-PAY-116](../unittest/UT-PAY-116.md) | Handler quản trị connection — kiểm quyền | STORY-PAY-003/AC-004, EXC-01, BR-PAY-006/Then |
| [UT-PAY-117](../unittest/UT-PAY-117.md) | UpdatePaymentConnectionCommandHandler — connection chưa dùng | STORY-PAY-003/AC-006, ALT-02, BR-PAY-006/Then |
| [UT-PAY-118](../unittest/UT-PAY-118.md) | UpdatePaymentConnectionCommandHandler — connection đã dùng | STORY-PAY-003/AC-003, AC-007, EXC-02, BR-PAY-006/Then |
| [UT-PAY-119](../unittest/UT-PAY-119.md) | UpdatePaymentConnectionCommandHandler — tắt, version, không tìm thấy | STORY-PAY-003/AC-008, EXC-03, BR-PAY-006/Then |
| [UT-PAY-120](../unittest/UT-PAY-120.md) | CreatePaymentOrderCommandHandler — connection đang dùng theo môi trường | STORY-PAY-003/AC-010, EXC-05, BR-PAY-006/Then |
| [UT-PAY-121](../unittest/UT-PAY-121.md) | SelectActivePaymentConnectionCommandHandler | STORY-PAY-003/AC-002, AC-013, EXC-04, BR-PAY-006/Then |
| [UT-PAY-122](../unittest/UT-PAY-122.md) | GetPaymentConnectionHistoryQueryHandler | STORY-PAY-003/AC-012, ALT-03, BR-PAY-006/Then |
| [UT-PAY-123](../unittest/UT-PAY-123.md) | GetPaymentConnectionsQueryHandler | STORY-PAY-003/Main Flow, AC-005, TDD-PAY-001/Internal API |
| [UT-PAY-124](../unittest/UT-PAY-124.md) | Ràng buộc PostgreSQL của lựa chọn và lịch sử connection | TDD-PAY-001/Data Model, BR-PAY-006/Then |
| [UT-PAY-125](../unittest/UT-PAY-125.md) | Sửa tài khoản khi đơn đang tạo giữ khóa (PostgreSQL) | TDD-PAY-001/Architecture, STORY-PAY-003/AC-003 |
| [UT-PAY-126](../unittest/UT-PAY-126.md) | Đổi connection đang dùng đầu cuối (PostgreSQL) | STORY-PAY-003/AC-002, AC-008, BR-PAY-006/Then |
| [UT-PAY-127](../unittest/UT-PAY-127.md) | SePayOption — kiểm Environment lúc khởi động | STORY-PAY-003/AC-011, EXC-06, BR-PAY-006/Then |

## Kiểm thử tích hợp bổ sung

- [ST-PAY-062](../systemtest/ST-PAY-062.md): Tạo đơn đồng thời.
- [ST-PAY-063](../systemtest/ST-PAY-063.md): Webhook lặp đồng thời.
- [ST-PAY-064](../systemtest/ST-PAY-064.md): Khôi phục sau tiến trình dừng.
- [ST-PAY-065](../systemtest/ST-PAY-065.md): Hoàn tác cấp gói lỗi.
- [ST-PAY-066](../systemtest/ST-PAY-066.md): Hai gói tranh cùng công trình.
- [ST-PAY-067](../systemtest/ST-PAY-067.md): Hủy lỗi ghi lịch sử.
- [ST-PAY-068](../systemtest/ST-PAY-068.md): Tra cứu giao dịch không khớp.
- [ST-PAY-089](../systemtest/ST-PAY-089.md) đến [ST-PAY-101](../systemtest/ST-PAY-101.md): Quản trị kết nối nhận tiền (STORY-PAY-003), mỗi AC một ca.

## Kết quả kiểm tra tài liệu

Lần bàn giao đầu đã kiểm tra cấu trúc và liên kết trong 146 tài liệu: 4 TDD, 4 User Story, 70 Unit Test và 68 System Test. Không phát hiện liên kết file thiếu, tham chiếu test thiếu mã/section, bảng test sai 13 cột hoặc Trace to lệch TEST_LINKS. Các khối JSON trong 4 TDD đọc được; 61/61 AC của 4 Story có System Test truy vết. Ngày 25/09/2026 đã kiểm lại 74 Unit Test và 70 System Test bằng script đọc TEST_LINKS: không có lỗi cấu trúc hoặc liên kết, và 62/62 AC của 4 Story có System Test còn hiệu lực. Đây là kiểm tra tĩnh tài liệu, không thay cho review đầy đủ nghiệp vụ, kiểm tra nhập Document First hoặc chạy test ứng dụng.
