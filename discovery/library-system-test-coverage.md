# Truy vết System Test thư viện mẫu

US và BR đã được người dùng chốt trong hội thoại. 28 ca dưới đây là đặc tả, chưa thực thi; không phải kết quả Pass. Reviewer/Approver: Tân Trần. Owner kiểm thử chưa xác định.

**Cập nhật 25/09/2026:** quyền quản lý thư viện mẫu theo STORY-RBAC-001 và BR-LIB-002 có mã kỹ thuật `library.manage` trong TDD-RBAC-001, không gắn phân công; ST-LIB-011 kiểm thêm nhân viên thuộc vai trò tùy chỉnh được cấp mã này. Đã thêm ST-LIB-028 kiểm việc mở mẫu lần đầu và thay đổi gói của cùng khách chạy đồng thời.

| Tiêu chí | System Test |
| --- | --- |
| STORY-LIB-001/AC-001 | [ST-LIB-001](../systemtest/ST-LIB-001.md), [ST-LIB-002](../systemtest/ST-LIB-002.md), [ST-LIB-011](../systemtest/ST-LIB-011.md) |
| STORY-LIB-001/AC-002 | [ST-LIB-006](../systemtest/ST-LIB-006.md), [ST-LIB-010](../systemtest/ST-LIB-010.md) |
| STORY-LIB-001/AC-003 | [ST-LIB-007](../systemtest/ST-LIB-007.md) |
| STORY-LIB-001/AC-004 | [ST-LIB-008](../systemtest/ST-LIB-008.md) |
| STORY-LIB-001/AC-005 | [ST-LIB-009](../systemtest/ST-LIB-009.md) |
| STORY-LIB-001/AC-006 | [ST-LIB-005](../systemtest/ST-LIB-005.md) |
| STORY-LIB-001/AC-007 | [ST-LIB-003](../systemtest/ST-LIB-003.md), [ST-LIB-004](../systemtest/ST-LIB-004.md) |
| STORY-LIB-002/AC-001 | [ST-LIB-012](../systemtest/ST-LIB-012.md), [ST-LIB-016](../systemtest/ST-LIB-016.md) |
| STORY-LIB-002/AC-002 | [ST-LIB-013](../systemtest/ST-LIB-013.md) |
| STORY-LIB-002/AC-003 | [ST-LIB-014](../systemtest/ST-LIB-014.md) |
| STORY-LIB-002/AC-004 | [ST-LIB-015](../systemtest/ST-LIB-015.md) |
| STORY-LIB-003/AC-001 | [ST-LIB-017](../systemtest/ST-LIB-017.md), [ST-LIB-027](../systemtest/ST-LIB-027.md) |
| STORY-LIB-003/AC-002 | [ST-LIB-018](../systemtest/ST-LIB-018.md) |
| STORY-LIB-003/AC-003 | [ST-LIB-019](../systemtest/ST-LIB-019.md), [ST-LIB-028](../systemtest/ST-LIB-028.md) |
| STORY-LIB-003/AC-004 | [ST-LIB-020](../systemtest/ST-LIB-020.md), [ST-LIB-021](../systemtest/ST-LIB-021.md) |
| STORY-LIB-003/AC-005 | [ST-LIB-022](../systemtest/ST-LIB-022.md), [ST-LIB-023](../systemtest/ST-LIB-023.md), [ST-LIB-025](../systemtest/ST-LIB-025.md) |
| STORY-LIB-003/AC-006 | [ST-LIB-024](../systemtest/ST-LIB-024.md) |
| STORY-LIB-003/AC-007 | [ST-LIB-026](../systemtest/ST-LIB-026.md) |

Đã kiểm tra cấu trúc mỗi ca một file, 13 cột, mã/section tham chiếu tồn tại và Trace to khớp TEST_LINKS. Bao phủ tham chiếu không đồng nghĩa đã chứng minh hành vi đúng.

Các ca 004 và phần truyền tải chưa có ngưỡng hạ tầng để nghiệm thu tải lớn; không thể chứng minh “không giới hạn” bằng một bộ dữ liệu hữu hạn. API, fixture, điểm gây lỗi và cách điều phối đồng thời phải được cụ thể hóa trong TDD trước khi chạy. Trong bước thiết kế TDD, người dùng đã xác nhận cho lưu nháp thiếu dữ liệu và kiểm đủ khi công bố; cần bổ sung đặc tả riêng cho việc lưu nháp khi cập nhật bộ kiểm thử.

Đã cập nhật ST-SUB-050–052 theo lượt từng phiên bản. TDD-SUB-002 đã được cập nhật phần tra cứu cùng TDD-LIB-002 để chốt; quản trị nội dung và danh sách theo TDD-LIB-001; các UT về tra cứu cần cập nhật sau khi chốt TDD; chưa dùng thiết kế cũ để triển khai quyền xem lại.
