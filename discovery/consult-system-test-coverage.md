# Đối chiếu đặc tả kiểm thử tư vấn KTS

Nội dung ba User Story và năm Business Rule đã được người dùng chốt trong hội thoại. Bảng dưới đối chiếu đặc tả kiểm thử; kết quả backend ngày 29/09/2026 được ghi riêng ở cuối tài liệu. Reviewer và Approver của các ST là Tân Trần theo xác nhận ngày 25/09/2026; Owner kiểm thử chưa được phân công. Không có quy trình phê duyệt hồ sơ KTS trong sản phẩm.

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
| [STORY-CONSULT-001/AC-008](../userstory/STORY-CONSULT-001.md#ac-008) | [ST-CONSULT-008](../systemtest/ST-CONSULT-008.md), [ST-CONSULT-033](../systemtest/ST-CONSULT-033.md) |
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
| [STORY-CONSULT-001/ALT-01](../userstory/STORY-CONSULT-001.md#alt-01) | [ST-CONSULT-001](../systemtest/ST-CONSULT-001.md), [ST-CONSULT-039](../systemtest/ST-CONSULT-039.md) |
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
| [STORY-CONSULT-001/AC-011](../userstory/STORY-CONSULT-001.md#ac-011) | [ST-CONSULT-033](../systemtest/ST-CONSULT-033.md) |
| [STORY-CONSULT-001/AC-012](../userstory/STORY-CONSULT-001.md#ac-012) | [ST-CONSULT-034](../systemtest/ST-CONSULT-034.md), [ST-CONSULT-037](../systemtest/ST-CONSULT-037.md) |
| [STORY-CONSULT-001/AC-013](../userstory/STORY-CONSULT-001.md#ac-013) | [ST-CONSULT-038](../systemtest/ST-CONSULT-038.md), [ST-CONSULT-040](../systemtest/ST-CONSULT-040.md) |
| [STORY-CONSULT-001/AC-014](../userstory/STORY-CONSULT-001.md#ac-014) | [ST-CONSULT-035](../systemtest/ST-CONSULT-035.md), [ST-CONSULT-036](../systemtest/ST-CONSULT-036.md), [ST-CONSULT-037](../systemtest/ST-CONSULT-037.md), [ST-CONSULT-041](../systemtest/ST-CONSULT-041.md) |
| [STORY-CONSULT-001/AC-015](../userstory/STORY-CONSULT-001.md#ac-015) | [ST-CONSULT-038](../systemtest/ST-CONSULT-038.md), [ST-CONSULT-039](../systemtest/ST-CONSULT-039.md) |
| [STORY-CONSULT-001/AC-016](../userstory/STORY-CONSULT-001.md#ac-016) | [ST-CONSULT-040](../systemtest/ST-CONSULT-040.md) |
| [STORY-CONSULT-002/AC-013](../userstory/STORY-CONSULT-002.md#ac-013) | [ST-CONSULT-039](../systemtest/ST-CONSULT-039.md) |
| [STORY-CONSULT-001/EXC-04](../userstory/STORY-CONSULT-001.md#exc-04) | [ST-CONSULT-035](../systemtest/ST-CONSULT-035.md), [ST-CONSULT-036](../systemtest/ST-CONSULT-036.md), [ST-CONSULT-037](../systemtest/ST-CONSULT-037.md) |
| [STORY-CONSULT-001/ALT-04](../userstory/STORY-CONSULT-001.md#alt-04) | [ST-CONSULT-040](../systemtest/ST-CONSULT-040.md) |

Bốn mươi mốt ca kiểm thử bao phủ 35 tiêu chí nghiệm thu và 18 luồng thay thế, luồng lỗi của ba User Story (mỗi luồng có ít nhất một ca, mã luồng đã ghi trong TEST_LINKS của ca tương ứng), gồm cả gửi trùng giờ bằng hai phiên, thời gian biên, KTS bị ẩn trong lúc khách đang điền form, phân quyền theo `consultation.manage`, email lỗi rồi phục hồi và bảo vệ ghi chú nội bộ.

Khảo sát ban đầu trước khi triển khai: `User` có email và số điện thoại tùy chọn; `SendEmailEvent`, `SendEmailConsumer` và cấu hình message bus đã có luồng gửi nền và thử lại. Đây là phần dự kiến tái sử dụng, chưa chứng minh tính năng tư vấn đã triển khai.

Cách chọn và kiểm số liên lạc, kiểm giờ mong muốn theo UTC+7, endpoint, schema và cách thử lại email nay được mô tả trong [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md); quyền quản trị đã chốt là `consultation.manage`. Không đặt thêm ngưỡng ngoài yêu cầu đã chốt.

Bộ tài liệu có US → BR → System Test, [TDD-CONSULT-001](../tdd/TDD-CONSULT-001.md) và 59 đặc tả UT-CONSULT-001 đến UT-CONSULT-059. UT-CONSULT-048 kiểm quyết định ngày 26/09/2026: ảnh đại diện phải thuộc kho ảnh của hệ thống (BR-CONSULT-001 khoản 9).

**Cập nhật 26/09/2026:** mã ứng dụng đã có ở commit `4d6c386` và `9e4f220` trên nhánh `feature/consultation` của `bmt-be`, chưa merge. Unit test theo 48 đặc tả UT-CONSULT và integration test trên PostgreSQL 15 (ràng buộc, khóa, gửi lại đồng thời cùng Idempotency-Key, đơn và email cùng commit hoặc cùng rollback) đã chạy qua. Các System Test trong bảng trên vẫn chưa thực thi, và migration chưa chạy trên môi trường đã triển khai.

**Cập nhật 29/09/2026:** người dùng chốt US/BR bổ sung Công ty bắt buộc nhập tên, Số sao và Số đánh giá nhập thủ công; hiển thị ở cả danh sách và chi tiết KTS. Hồ sơ cũ thiếu Công ty giữ nguyên cùng trạng thái hiển thị, hai số mới bằng 0; phải bổ sung Công ty khi sửa lần tiếp theo. Đã cập nhật ST-CONSULT-001, ST-CONSULT-004, ST-CONSULT-006, ST-CONSULT-008, ST-CONSULT-031 và thêm tám ca ST-CONSULT-033 đến ST-CONSULT-040. Người dùng đã chốt TDD trong hội thoại. Đã thêm 11 đặc tả UT-CONSULT-049 đến UT-CONSULT-059, sửa 9 đặc tả UT-CONSULT-001, 002, 004, 008, 009, 010, 015, 020, 048; thêm ST-CONSULT-041 cho CHECK và ánh xạ kiểu numeric trên PostgreSQL thật. Không có kết quả thực thi mới trong lần cập nhật tài liệu này.

| Quy tắc bổ sung trong BR-CONSULT-001 | System Test |
| --- | --- |
| Then khoản 7: Công ty bắt buộc khi tạo/sửa | ST-CONSULT-008, ST-CONSULT-033 |
| Then khoản 10: hai số nhập thủ công | ST-CONSULT-001, ST-CONSULT-034 |
| Then khoản 11: khoảng, độ chính xác, số nguyên và quan hệ hai số | ST-CONSULT-034, ST-CONSULT-035, ST-CONSULT-036, ST-CONSULT-037 |
| Then khoản 12: mặc định và khởi tạo hai số cho hồ sơ cũ | ST-CONSULT-038, ST-CONSULT-040 |
| Then khoản 13: hiển thị cùng giá trị hiện tại ở hai trang | ST-CONSULT-038, ST-CONSULT-039 |
| Except: giữ hồ sơ cũ thiếu Công ty, bắt buộc bổ sung khi sửa | ST-CONSULT-040 |

## Đối chiếu Unit Test cho ba trường mới

Các đặc tả dưới đây đã có mã kiểm thử cho validator, handler và query; kết quả backend ngày 29/09 ghi riêng ở cuối tài liệu. Số test chạy không bằng số tài liệu UT vì một đặc tả có thể sinh nhiều biến thể kiểm thử. Reviewer/Approver giữ Tân Trần theo bộ tài liệu hiện hành; Owner của các ca mới chưa được phân công. Metadata này không xác nhận phê duyệt trên hệ thống quản lý tài liệu.

| Phạm vi và căn cứ | Unit Test | Kiểm chứng ngoài unit |
| --- | --- | --- |
| Công ty bắt buộc, trim, tối đa 200 đơn vị UTF-16; BR-CONSULT-001/Then khoản 7, TDD/Data Model | UT-CONSULT-001, UT-CONSULT-002, UT-CONSULT-004, UT-CONSULT-008, UT-CONSULT-055, UT-CONSULT-056 | ST-CONSULT-033 kiểm tạo/sửa qua giao diện và API |
| Rating 0–5, tối đa một chữ số thập phân; BR-CONSULT-001/Then khoản 11 | UT-CONSULT-049, UT-CONSULT-050 | ST-CONSULT-034, ST-CONSULT-035, ST-CONSULT-041 kiểm lưu thật và không tự làm tròn |
| ReviewCount nguyên không âm, giới hạn Int32 trong TDD | UT-CONSULT-051 | ST-CONSULT-036 kiểm JSON số lẻ/sai kiểu; ST-CONSULT-041 kiểm CHECK số âm |
| ReviewCount=0 kéo theo Rating=0; BR-CONSULT-001/Then khoản 11 | UT-CONSULT-052, UT-CONSULT-053 | ST-CONSULT-037 và ST-CONSULT-041 |
| POST mặc định 0, PUT bắt buộc gửi đủ hai số; TDD/Endpoints | UT-CONSULT-053, UT-CONSULT-054, UT-CONSULT-056 | ST-CONSULT-038 kiểm mặc định trên form và lưu/đọc thật |
| Lưu số nhập và sửa có version; BR-CONSULT-001/Then khoản 10, TDD/Architecture | UT-CONSULT-008, UT-CONSULT-009, UT-CONSULT-010, UT-CONSULT-055, UT-CONSULT-056 | ST-CONSULT-001 và ST-CONSULT-039 |
| Đọc đủ ba trường ở public/admin, danh sách/chi tiết; BR-CONSULT-001/Then khoản 13, TDD/Endpoints | UT-CONSULT-015, UT-CONSULT-057, UT-CONSULT-058, UT-CONSULT-059 | ST-CONSULT-039 kiểm giao diện/API/database |
| Giữ hồ sơ cũ thiếu Công ty, bắt buộc bổ sung khi sửa; BR-CONSULT-001/Except | UT-CONSULT-002, UT-CONSULT-015, UT-CONSULT-055, UT-CONSULT-057, UT-CONSULT-058, UT-CONSULT-059 | ST-CONSULT-040 kiểm migration và dữ liệu cũ |
| Không tăng số sao/số đánh giá theo yêu cầu tư vấn; BR-CONSULT-001/Then khoản 10 | UT-CONSULT-020 | ST-CONSULT-034 |
| Giữ quyền quản trị và policy ảnh; quy tắc hiện có | UT-CONSULT-046, UT-CONSULT-048 (fixture có đủ ba trường mới) | ST-CONSULT-004, ST-CONSULT-031 |

Không dùng kết quả InMemory hoặc mock để kết luận về CHECK, scale của numeric, khóa hay tính nguyên vẹn khi commit. Các phần đó cần PostgreSQL thật theo ST-CONSULT-040, ST-CONSULT-041 và chiến lược integration trong TDD. Bằng chứng chạy ngày 26/09/2026 chỉ áp dụng cho phiên bản trước khi có ba trường mới.


## Bằng chứng backend ngày 29/09/2026

Đã triển khai Công ty, Số sao và Số đánh giá trong backend. Phần này được ghi trong commit [ae1bc8b](https://github.com/TaskCoper/bmt-be/commit/ae1bc8b32dab77d1218efa20122545f35d9f9706) trên `develop`. Ba trường có mặt ở POST/PUT và cả bốn API GET hồ sơ; các route và quyền `consultation.manage` được giữ nguyên.

| Phạm vi đã chạy | Kết quả | Bằng chứng và giới hạn |
| --- | --- | --- |
| `bmt-be.application.tests`, lọc `FullyQualifiedName~architect` hoặc `FullyQualifiedName~consultationRequest` | 135 qua, 0 lỗi, 0 bỏ qua | Validator, handler, projection, các nhánh tư vấn hiện có. [Test hồ sơ](../../bmt-be/test/bmt-be.application.tests/usecases/architect/ArchitectValidatorTests.cs), [test ánh xạ](../../bmt-be/test/bmt-be.application.tests/usecases/architect/ArchitectQueryTests.cs). Không dùng InMemory để kết luận về CHECK/khóa. |
| `bmt-be.api.tests`, lọc `ConsultationApiPipelineTests` | 34 qua, 0 lỗi, 0 bỏ qua | [HTTP pipeline](../../bmt-be/test/bmt-be.api.tests/security/ConsultationApiPipelineTests.cs) kiểm quyền, CSRF, binding và mapping command; MediatR được giả lập trong bộ test này. |
| `bmt-be.integration.tests`, lọc `ArchitectProfileSummaryTests` hoặc `Consultation`, với `BMT_REQUIRE_DOCKER_TESTS=1` | 22 qua, 0 lỗi, 0 bỏ qua | PostgreSQL 15 trong container tạm, pipeline MediatR và repository thật. [Test ba trường](../../bmt-be/test/bmt-be.integration.tests/ArchitectProfileSummaryTests.cs) kiểm migration hồ sơ cũ, CHECK, không làm tròn 4.85, POST mặc định, PUT bắt buộc và đọc lại cả bốn projection. Bộ test tư vấn kiểm thêm transaction, khóa, outbox và số liệu nhập tay không đổi khi nhận đơn. |

Các lệnh test dùng `--no-restore`; không tải lại dependency. Tổng cộng 191 test chạy qua. Migration [20260929122151_ArchitectProfileSummary](../../bmt-be/src/bmt-be.persistence/Migrations/20260929122151_ArchitectProfileSummary.cs) chỉ thêm ba cột và bốn CHECK của Architect. Test nâng cấp đối chiếu hồ sơ hiện/ẩn, ID, version, thời gian, links và yêu cầu cũ; test chạy lại giữ nguyên số sao/số đánh giá đã nhập.

Đây là bằng chứng cho phần backend của ST-CONSULT-033 đến ST-CONSULT-041, cùng các nhánh hồi quy đã nêu; không đánh dấu toàn bộ 41 System Test đã đạt. Chưa tích hợp frontend, chưa chạy E2E qua trình duyệt hoặc gửi SMTP thật. Chưa áp dụng migration vào database phát triển đang chạy hay môi trường triển khai. Trước khi mở lại thao tác ghi hồ sơ trên môi trường dùng thật, cần triển khai frontend gửi Công ty và đủ hai số theo contract mới.


## Kiểm chứng trước khi push ngày 30/09/2026

Backend tại commit [ae1bc8b](https://github.com/TaskCoper/bmt-be/commit/ae1bc8b32dab77d1218efa20122545f35d9f9706) đã được kiểm trên checkout riêng, dựa trên `develop` có commit Tin tức `580bf16`. Snapshot và model đích của migration KTS đã đồng bộ với schema hiện tại; commit KTS chỉ thêm ba cột và bốn CHECK vào Architect. Test nâng cấp bắt đầu từ migration `20260929090000_NewsArticleReadingTime`.

Lệnh `BMT_REQUIRE_DOCKER_TESTS=1 dotnet test bmt-be.sln -c Release --no-restore -m:1` chạy qua **2.335 test**, không lỗi, không bỏ qua: domain 1, application 1.379, persistence 17, infrastructure 163, API 338, integration 437. Lệnh EF `migrations has-pending-model-changes` xác nhận model khớp migration snapshot. Đây là kết quả trên bản commit cuối sau khi rebase, bổ sung cho bằng chứng kiểm thử theo phạm vi ngày 29/09 ở trên.

Push lên `develop` tự kích hoạt workflow [Deploy Application](https://github.com/TaskCoper/bmt-be/actions/workflows/main.yaml), gồm bước áp dụng migration và triển khai Dev. Kết quả local không xác nhận workflow đã triển khai thành công; theo dõi trạng thái CI của đúng commit. Frontend vẫn chưa được cập nhật vì workspace chưa có mã nguồn giao diện.
