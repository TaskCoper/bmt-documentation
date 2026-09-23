# Đối chiếu đặc tả kiểm thử tư vấn KTS

Nội dung ba User Story và năm Business Rule đã được người dùng chốt trong hội thoại. Đây là đặc tả kiểm thử, chưa phải kết quả thực thi. Thông tin người phụ trách, reviewer và approver chưa được phân công; không có quy trình phê duyệt hồ sơ KTS trong sản phẩm.

| Tiêu chí nghiệm thu | System Test |
| --- | --- |
| [STORY-CONSULT-001/AC-001](../userstory/STORY-CONSULT-001.md#ac-001) | [ST-CONSULT-001](../systemtest/ST-CONSULT-001.md) |
| [STORY-CONSULT-001/AC-002](../userstory/STORY-CONSULT-001.md#ac-002) | [ST-CONSULT-002](../systemtest/ST-CONSULT-002.md) |
| [STORY-CONSULT-001/AC-003](../userstory/STORY-CONSULT-001.md#ac-003) | [ST-CONSULT-003](../systemtest/ST-CONSULT-003.md) |
| [STORY-CONSULT-001/AC-004](../userstory/STORY-CONSULT-001.md#ac-004) | [ST-CONSULT-004](../systemtest/ST-CONSULT-004.md) |
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
| [STORY-CONSULT-003/AC-001](../userstory/STORY-CONSULT-003.md#ac-001) | [ST-CONSULT-023](../systemtest/ST-CONSULT-023.md) |
| [STORY-CONSULT-003/AC-002](../userstory/STORY-CONSULT-003.md#ac-002) | [ST-CONSULT-024](../systemtest/ST-CONSULT-024.md), [ST-CONSULT-029](../systemtest/ST-CONSULT-029.md) |
| [STORY-CONSULT-003/AC-003](../userstory/STORY-CONSULT-003.md#ac-003) | [ST-CONSULT-025](../systemtest/ST-CONSULT-025.md) |
| [STORY-CONSULT-003/AC-004](../userstory/STORY-CONSULT-003.md#ac-004) | [ST-CONSULT-026](../systemtest/ST-CONSULT-026.md) |
| [STORY-CONSULT-003/AC-005](../userstory/STORY-CONSULT-003.md#ac-005) | [ST-CONSULT-027](../systemtest/ST-CONSULT-027.md) |
| [STORY-CONSULT-003/AC-006](../userstory/STORY-CONSULT-003.md#ac-006) | [ST-CONSULT-028](../systemtest/ST-CONSULT-028.md) |

Ba mươi ca kiểm thử bao phủ 28 tiêu chí nghiệm thu, gồm cả gửi trùng giờ bằng hai phiên, thời gian biên, KTS bị ẩn trong lúc khách đang điền form, phân quyền, email lỗi rồi phục hồi và bảo vệ ghi chú nội bộ.

Phần đã kiểm tra trong mã nguồn: `User` có email và số điện thoại tùy chọn; `SendEmailEvent`, `SendEmailConsumer` và cấu hình message bus đã có luồng gửi nền và thử lại. Đây là phần dự kiến tái sử dụng, chưa chứng minh tính năng tư vấn đã triển khai.

Khi thiết kế kỹ thuật cần xác định định dạng số liên lạc, giới hạn dữ liệu, danh sách giờ cụ thể trên frontend, quyền quản trị tương ứng và cấu hình thử lại email. Chưa đặt endpoint, schema hoặc ngưỡng ngoài yêu cầu đã chốt.

Chưa sửa mã ứng dụng, chạy migration hoặc chạy kiểm thử. Phần chuẩn bị nghiệp vụ đã có US → BR → System Test; chưa có TDD hay Unit Test cho tính năng này.
