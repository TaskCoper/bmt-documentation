# Đối chiếu đặc tả kiểm thử tư vấn KTS

Nội dung ba User Story và năm Business Rule đã được người dùng chốt trong hội thoại. Đây là đặc tả kiểm thử, chưa phải kết quả thực thi. Reviewer và Approver của các ST là Tân Trần theo xác nhận ngày 25/09/2026; Owner kiểm thử chưa được phân công. Không có quy trình phê duyệt hồ sơ KTS trong sản phẩm.

**Cập nhật 25/09/2026:** quyền quản trị tư vấn KTS theo STORY-RBAC-001 và BR-CONSULT-001 có mã kỹ thuật `consultation.manage` trong TDD-RBAC-001, dùng chung cho hồ sơ, category và xử lý yêu cầu; hệ thống kiểm theo mã quyền, không theo tên vai trò. Yêu cầu tư vấn miễn phí là kênh riêng, không thay cam kết tư vấn offline của gói (BR-CONSULT-002). Đã thêm ST-CONSULT-031 và ST-CONSULT-032 cho phần quyền này.

| Tiêu chí nghiệm thu | System Test |
| --- | --- |
| [STORY-CONSULT-001/AC-001](../userstory/STORY-CONSULT-001.md#ac-001) | [ST-CONSULT-001](../systemtest/ST-CONSULT-001.md), [ST-CONSULT-031](../systemtest/ST-CONSULT-031.md) |
| [STORY-CONSULT-001/AC-002](../userstory/STORY-CONSULT-001.md#ac-002) | [ST-CONSULT-002](../systemtest/ST-CONSULT-002.md) |
| [STORY-CONSULT-001/AC-003](../userstory/STORY-CONSULT-001.md#ac-003) | [ST-CONSULT-003](../systemtest/ST-CONSULT-003.md) |
| [STORY-CONSULT-001/AC-004](../userstory/STORY-CONSULT-001.md#ac-004) | [ST-CONSULT-004](../systemtest/ST-CONSULT-004.md), [ST-CONSULT-031](../systemtest/ST-CONSULT-031.md) |
| [STORY-CONSULT-001/AC-005](../userstory/STORY-CONSULT-001.md#ac-005) | [ST-CONSULT-005](../systemtest/ST-CONSULT-005.md) |
| [STORY-CONSULT-001/AC-006](../userstory/STORY-CONSULT-001.md#ac-006) | [ST-CONSULT-006](../systemtest/ST-CONSULT-006.md) |
| [STORY-CONSULT-001/AC-007](../userstory/STORY-CONSULT-001.md#ac-007) | [ST-CONSULT-007](../systemtest/ST-CONSULT-007.md) |
| [STORY-CONSULT-001/AC-008](../userstory/STORY-CONSULT-001.md#ac-008) | [ST-CONSULT-008](../systemtest/ST-CONSULT-008.md) |
| [STORY-CONSULT-001/AC-009](../userstory/STORY-CONSULT-001.md#ac-009) | [ST-CONSULT-009](../systemtest/ST-CONSULT-009.md) |
| [STORY-CONSULT-001/AC-010](../userstory/STORY-CONSULT-001.md#ac-010) | [ST-CONSULT-010](../systemtest/ST-CONSULT-010.md) |
| [STORY-CONSULT-002/AC-001](../userstory/STORY-CONSULT-002.md#ac-001) | [ST-CONSULT-011](../systemtest/ST-CONSULT-011.md) |
| [STORY-CONSULT-002/AC-002](../userstory/STORY-CONSULT-002.md#ac-002) | [ST-CONSULT-012](../systemtest/ST-CONSULT-012.md) |
| [STORY-CONSULT-002/AC-003](../userstory/STORY-CONSULT-002.md#ac-003) | [ST-CONSULT-013](../systemtest/ST-CONSULT-013.md) |
| [STORY-CONSULT-002/AC-004](../userstory/STORY-CONSULT-002.md#ac-004) | [ST-CONSULT-014](../systemtest/ST-CONSULT-014.md) |
| [STORY-CONSULT-002/AC-005](../userstory/STORY-CONSULT-002.md#ac-005) | [ST-CONSULT-015](../systemtest/ST-CONSULT-015.md) |
| [STORY-CONSULT-002/AC-006](../userstory/STORY-CONSULT-002.md#ac-006) | [ST-CONSULT-016](../systemtest/ST-CONSULT-016.md) |
| [STORY-CONSULT-002/AC-007](../userstory/STORY-CONSULT-002.md#ac-007) | [ST-CONSULT-017](../systemtest/ST-CONSULT-017.md) |
| [STORY-CONSULT-002/AC-008](../userstory/STORY-CONSULT-002.md#ac-008) | [ST-CONSULT-018](../systemtest/ST-CONSULT-018.md) |
| [STORY-CONSULT-002/AC-009](../userstory/STORY-CONSULT-002.md#ac-009) | [ST-CONSULT-019](../systemtest/ST-CONSULT-019.md), [ST-CONSULT-030](../systemtest/ST-CONSULT-030.md) |
| [STORY-CONSULT-002/AC-010](../userstory/STORY-CONSULT-002.md#ac-010) | [ST-CONSULT-020](../systemtest/ST-CONSULT-020.md) |
| [STORY-CONSULT-002/AC-011](../userstory/STORY-CONSULT-002.md#ac-011) | [ST-CONSULT-021](../systemtest/ST-CONSULT-021.md) |
| [STORY-CONSULT-002/AC-012](../userstory/STORY-CONSULT-002.md#ac-012) | [ST-CONSULT-022](../systemtest/ST-CONSULT-022.md) |
| [STORY-CONSULT-003/AC-001](../userstory/STORY-CONSULT-003.md#ac-001) | [ST-CONSULT-023](../systemtest/ST-CONSULT-023.md), [ST-CONSULT-032](../systemtest/ST-CONSULT-032.md) |
| [STORY-CONSULT-003/AC-002](../userstory/STORY-CONSULT-003.md#ac-002) | [ST-CONSULT-024](../systemtest/ST-CONSULT-024.md), [ST-CONSULT-029](../systemtest/ST-CONSULT-029.md) |
| [STORY-CONSULT-003/AC-003](../userstory/STORY-CONSULT-003.md#ac-003) | [ST-CONSULT-025](../systemtest/ST-CONSULT-025.md) |
| [STORY-CONSULT-003/AC-004](../userstory/STORY-CONSULT-003.md#ac-004) | [ST-CONSULT-026](../systemtest/ST-CONSULT-026.md) |
| [STORY-CONSULT-003/AC-005](../userstory/STORY-CONSULT-003.md#ac-005) | [ST-CONSULT-027](../systemtest/ST-CONSULT-027.md) |
| [STORY-CONSULT-003/AC-006](../userstory/STORY-CONSULT-003.md#ac-006) | [ST-CONSULT-028](../systemtest/ST-CONSULT-028.md), [ST-CONSULT-032](../systemtest/ST-CONSULT-032.md) |
| [STORY-CONSULT-001/ALT-01](../userstory/STORY-CONSULT-001.md#alt-01) | [ST-CONSULT-001](../systemtest/ST-CONSULT-001.md) |
| [STORY-CONSULT-001/ALT-02](../userstory/STORY-CONSULT-001.md#alt-02) | [ST-CONSULT-002](../systemtest/ST-CONSULT-002.md), [ST-CONSULT-003](../systemtest/ST-CONSULT-003.md) |
| [STORY-CONSULT-001/ALT-03](../userstory/STORY-CONSULT-001.md#alt-03) | [ST-CONSULT-007](../systemtest/ST-CONSULT-007.md), [ST-CONSULT-010](../systemtest/ST-CONSULT-010.md) |
| [STORY-CONSULT-001/EXC-01](../userstory/STORY-CONSULT-001.md#exc-01) | [ST-CONSULT-004](../systemtest/ST-CONSULT-004.md), [ST-CONSULT-031](../systemtest/ST-CONSULT-031.md) |
| [STORY-CONSULT-001/EXC-02](../userstory/STORY-CONSULT-001.md#exc-02) | [ST-CONSULT-008](../systemtest/ST-CONSULT-008.md) |
| [STORY-CONSULT-001/EXC-03](../userstory/STORY-CONSULT-001.md#exc-03) | [ST-CONSULT-009](../systemtest/ST-CONSULT-009.md) |
| [STORY-CONSULT-002/ALT-01](../userstory/STORY-CONSULT-002.md#alt-01) | [ST-CONSULT-013](../systemtest/ST-CONSULT-013.md), [ST-CONSULT-014](../systemtest/ST-CONSULT-014.md) |
| [STORY-CONSULT-002/ALT-02](../userstory/STORY-CONSULT-002.md#alt-02) | [ST-CONSULT-016](../systemtest/ST-CONSULT-016.md) |
| [STORY-CONSULT-002/EXC-01](../userstory/STORY-CONSULT-002.md#exc-01) | [ST-CONSULT-021](../systemtest/ST-CONSULT-021.md) |
| [STORY-CONSULT-002/EXC-02](../userstory/STORY-CONSULT-002.md#exc-02) | [ST-CONSULT-014](../systemtest/ST-CONSULT-014.md), [ST-CONSULT-017](../systemtest/ST-CONSULT-017.md), [ST-CONSULT-018](../systemtest/ST-CONSULT-018.md) |
| [STORY-CONSULT-002/EXC-03](../userstory/STORY-CONSULT-002.md#exc-03) | [ST-CONSULT-020](../systemtest/ST-CONSULT-020.md) |
| [STORY-CONSULT-003/ALT-01](../userstory/STORY-CONSULT-003.md#alt-01) | [ST-CONSULT-025](../systemtest/ST-CONSULT-025.md) |
| [STORY-CONSULT-003/ALT-02](../userstory/STORY-CONSULT-003.md#alt-02) | [ST-CONSULT-026](../systemtest/ST-CONSULT-026.md) |
| [STORY-CONSULT-003/ALT-03](../userstory/STORY-CONSULT-003.md#alt-03) | [ST-CONSULT-027](../systemtest/ST-CONSULT-027.md) |
| [STORY-CONSULT-003/ALT-04](../userstory/STORY-CONSULT-003.md#alt-04) | [ST-CONSULT-029](../systemtest/ST-CONSULT-029.md) |
| [STORY-CONSULT-003/EXC-01](../userstory/STORY-CONSULT-003.md#exc-01) | [ST-CONSULT-028](../systemtest/ST-CONSULT-028.md), [ST-CONSULT-032](../systemtest/ST-CONSULT-032.md) |

Ba mươi hai ca kiểm thử bao phủ 28 tiêu chí nghiệm thu và 16 luồng thay thế, luồng lỗi của ba User Story (mỗi luồng có ít nhất một ca, mã luồng đã ghi trong TEST_LINKS của ca tương ứng), gồm cả gửi trùng giờ bằng hai phiên, thời gian biên, KTS bị ẩn trong lúc khách đang điền form, phân quyền theo `consultation.manage`, email lỗi rồi phục hồi và bảo vệ ghi chú nội bộ.

Phần đã kiểm tra trong mã nguồn: `User` có email và số điện thoại tùy chọn; `SendEmailEvent`, `SendEmailConsumer` và cấu hình message bus đã có luồng gửi nền và thử lại. Đây là phần dự kiến tái sử dụng, chưa chứng minh tính năng tư vấn đã triển khai.

Cách chọn và kiểm số liên lạc, kiểm giờ mong muốn theo UTC+7, endpoint, schema và cách thử lại email nay được mô tả trong [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md); quyền quản trị đã chốt là `consultation.manage`. Không đặt thêm ngưỡng ngoài yêu cầu đã chốt.

Bộ tài liệu có US → BR → System Test, [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md) và 48 đặc tả UT-CONSULT-001 đến UT-CONSULT-048. UT-CONSULT-048 kiểm quyết định ngày 26/09/2026: ảnh đại diện phải thuộc kho ảnh của hệ thống (BR-CONSULT-001 khoản 9).

**Cập nhật 26/09/2026:** mã ứng dụng đã có ở commit `4d6c386` và `9e4f220` trên nhánh `feature/consultation` của `bmt-be`, chưa merge. Unit test theo 48 đặc tả UT-CONSULT và integration test trên PostgreSQL 15 (ràng buộc, khóa, gửi lại đồng thời cùng Idempotency-Key, đơn và email cùng commit hoặc cùng rollback) đã chạy qua. Các System Test trong bảng trên vẫn chưa thực thi, và migration chưa chạy trên môi trường đã triển khai.
