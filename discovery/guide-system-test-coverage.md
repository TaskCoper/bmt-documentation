# Phạm vi System Test cho video hướng dẫn

Đã soạn 34 đặc tả System Test, liên kết tới 26 tiêu chí nghiệm thu của hai User Story. Các tài liệu đang Draft; chưa viết mã hoặc chạy các ca này. Có tham chiếu không đồng nghĩa đã kiểm chứng hành vi.

## Đối chiếu tiêu chí nghiệm thu

| Story / AC | System Test |
|---|---|
| [STORY-GUIDE-001/AC-001](../userstory/STORY-GUIDE-001.md#ac-001) | [ST-GUIDE-002](../systemtest/ST-GUIDE-002.md), [ST-GUIDE-003](../systemtest/ST-GUIDE-003.md) |
| [STORY-GUIDE-001/AC-002](../userstory/STORY-GUIDE-001.md#ac-002) | [ST-GUIDE-001](../systemtest/ST-GUIDE-001.md), [ST-GUIDE-004](../systemtest/ST-GUIDE-004.md) |
| [STORY-GUIDE-001/AC-003](../userstory/STORY-GUIDE-001.md#ac-003) | [ST-GUIDE-001](../systemtest/ST-GUIDE-001.md), [ST-GUIDE-021](../systemtest/ST-GUIDE-021.md) |
| [STORY-GUIDE-001/AC-004](../userstory/STORY-GUIDE-001.md#ac-004) | [ST-GUIDE-001](../systemtest/ST-GUIDE-001.md), [ST-GUIDE-014](../systemtest/ST-GUIDE-014.md) |
| [STORY-GUIDE-001/AC-005](../userstory/STORY-GUIDE-001.md#ac-005) | [ST-GUIDE-005](../systemtest/ST-GUIDE-005.md), [ST-GUIDE-006](../systemtest/ST-GUIDE-006.md), [ST-GUIDE-007](../systemtest/ST-GUIDE-007.md), [ST-GUIDE-008](../systemtest/ST-GUIDE-008.md), [ST-GUIDE-009](../systemtest/ST-GUIDE-009.md) |
| [STORY-GUIDE-001/AC-006](../userstory/STORY-GUIDE-001.md#ac-006) | [ST-GUIDE-010](../systemtest/ST-GUIDE-010.md) |
| [STORY-GUIDE-001/AC-007](../userstory/STORY-GUIDE-001.md#ac-007) | [ST-GUIDE-013](../systemtest/ST-GUIDE-013.md) |
| [STORY-GUIDE-001/AC-008](../userstory/STORY-GUIDE-001.md#ac-008) | [ST-GUIDE-017](../systemtest/ST-GUIDE-017.md) |
| [STORY-GUIDE-001/AC-009](../userstory/STORY-GUIDE-001.md#ac-009) | [ST-GUIDE-015](../systemtest/ST-GUIDE-015.md), [ST-GUIDE-016](../systemtest/ST-GUIDE-016.md) |
| [STORY-GUIDE-001/AC-010](../userstory/STORY-GUIDE-001.md#ac-010) | [ST-GUIDE-018](../systemtest/ST-GUIDE-018.md) |
| [STORY-GUIDE-001/AC-011](../userstory/STORY-GUIDE-001.md#ac-011) | [ST-GUIDE-019](../systemtest/ST-GUIDE-019.md), [ST-GUIDE-021](../systemtest/ST-GUIDE-021.md) |
| [STORY-GUIDE-001/AC-012](../userstory/STORY-GUIDE-001.md#ac-012) | [ST-GUIDE-011](../systemtest/ST-GUIDE-011.md) |
| [STORY-GUIDE-001/AC-013](../userstory/STORY-GUIDE-001.md#ac-013) | [ST-GUIDE-012](../systemtest/ST-GUIDE-012.md) |
| [STORY-GUIDE-001/AC-014](../userstory/STORY-GUIDE-001.md#ac-014) | [ST-GUIDE-020](../systemtest/ST-GUIDE-020.md) |
| [STORY-GUIDE-001/AC-015](../userstory/STORY-GUIDE-001.md#ac-015) | [ST-GUIDE-031](../systemtest/ST-GUIDE-031.md), [ST-GUIDE-032](../systemtest/ST-GUIDE-032.md) |
| [STORY-GUIDE-001/AC-016](../userstory/STORY-GUIDE-001.md#ac-016) | [ST-GUIDE-033](../systemtest/ST-GUIDE-033.md), [ST-GUIDE-034](../systemtest/ST-GUIDE-034.md) |
| [STORY-GUIDE-002/AC-001](../userstory/STORY-GUIDE-002.md#ac-001) | [ST-GUIDE-022](../systemtest/ST-GUIDE-022.md) |
| [STORY-GUIDE-002/AC-002](../userstory/STORY-GUIDE-002.md#ac-002) | [ST-GUIDE-022](../systemtest/ST-GUIDE-022.md) |
| [STORY-GUIDE-002/AC-003](../userstory/STORY-GUIDE-002.md#ac-003) | [ST-GUIDE-023](../systemtest/ST-GUIDE-023.md) |
| [STORY-GUIDE-002/AC-004](../userstory/STORY-GUIDE-002.md#ac-004) | [ST-GUIDE-024](../systemtest/ST-GUIDE-024.md) |
| [STORY-GUIDE-002/AC-005](../userstory/STORY-GUIDE-002.md#ac-005) | [ST-GUIDE-026](../systemtest/ST-GUIDE-026.md) |
| [STORY-GUIDE-002/AC-006](../userstory/STORY-GUIDE-002.md#ac-006) | [ST-GUIDE-027](../systemtest/ST-GUIDE-027.md) |
| [STORY-GUIDE-002/AC-007](../userstory/STORY-GUIDE-002.md#ac-007) | [ST-GUIDE-025](../systemtest/ST-GUIDE-025.md) |
| [STORY-GUIDE-002/AC-008](../userstory/STORY-GUIDE-002.md#ac-008) | [ST-GUIDE-028](../systemtest/ST-GUIDE-028.md) |
| [STORY-GUIDE-002/AC-009](../userstory/STORY-GUIDE-002.md#ac-009) | [ST-GUIDE-029](../systemtest/ST-GUIDE-029.md) |
| [STORY-GUIDE-002/AC-010](../userstory/STORY-GUIDE-002.md#ac-010) | [ST-GUIDE-030](../systemtest/ST-GUIDE-030.md) |

## Dữ liệu và môi trường cần có

- PostgreSQL thật để đối chiếu trạng thái, xóa và tính toàn vẹn; tài khoản có quyền, không có quyền và phiên khách.
- Video thường và Shorts thật đã đăng, có thời lượng và cho nhúng; người kiểm thử cung cấp link. Không dùng video ID minh họa trong TDD làm bằng chứng phát được.
- Bộ giả lập YouTube có thể trả timeout, lỗi dịch vụ, video không có, không cho nhúng, live/upcoming. Bộ giả lập chỉ kiểm nhánh tích hợp; ST-GUIDE-024 và ST-GUIDE-033 cần trình duyệt/video thật.

## Phần thiết kế cần kiểm chứng thêm

[TDD-GUIDE-001](../tdd/TDD-GUIDE-001.md) đề xuất Version/OrderVersion, cache YouTube 24 giờ và cách đếm Unicode. Sau khi chốt TDD cần bổ sung kiểm thử kỹ thuật cho tranh chấp đồng thời, rollback, CHECK/FK, metadata hết hạn, Redis lỗi, phân trang và Unicode; các ca nghiệp vụ hiện tại không tự chứng minh các thuộc tính này. Chưa soạn đặc tả Unit Test trước khi chốt TDD.

## Giới hạn khảo sát nguồn

Đã khảo sát bộ GUIDE, code nền về policy/transaction/cache/lỗi và tài liệu nền RBAC/CSRF. Việc đi tiếp mọi tham chiếu giữa RBAC, AUTH và các module khác mở rộng tới hàng trăm tài liệu; chưa đọc hết chuỗi này. Vì vậy TDD đang là bản nháp để rà soát, chưa được coi là đã kiểm chứng đầy đủ tác động liên module. Không phát hiện mã GUIDE đích bị thiếu trong kiểm tra cấu trúc và liên kết của bộ mới.

Chưa có mã frontend trong workspace, chưa kiểm credentials/quota YouTube của dự án, chưa render sơ đồ hoặc chạy importer Document First. Author/Creator/Assignee/Owner, Sprint và ngày hiệu lực chưa được cung cấp; Reviewer và Approver đã xác nhận là Tân Trần.

## Kiểm tra tài liệu

Kiểm tra tự động tại workspace: mỗi System Test có một mã đúng tên file, một dòng dữ liệu với 13 cột; Trace to khớp TEST_LINKS; các section AC/BR đích tồn tại; 26/26 AC có đặc tả liên kết. Kiểm tra thêm heading theo template, tham chiếu Story–TDD hai chiều, JSON ví dụ API và đường dẫn Markdown trong bộ GUIDE. Đây là kiểm tra tài liệu, không phải kết quả kiểm thử ứng dụng.
