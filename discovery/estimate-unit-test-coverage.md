# Đặc tả Unit Test Tạo dự toán

Người dùng xác nhận “Ok chốt đi” cho TDD-PROJ-001, TDD-PROJ-002 và TDD-PROJ-003 sau khi làm rõ việc lưu và xem lại kết quả bước 2–3. Đây là xác nhận thiết kế trong hội thoại, không phải thao tác import/publish hoặc phê duyệt trên hệ thống quản lý tài liệu.

Phạm vi chốt gồm thiết kế hiện có và các phần chờ tích hợp đã nêu rõ. Chưa có hợp đồng AI, lựa chọn kho tệp, nguồn địa chỉ hoặc thông số vận hành; không coi xác nhận này là đã bổ sung các dữ liệu còn thiếu. Chưa triển khai mã hoặc chạy test.

## Mốc TDD được chốt

Hash ghi nhận đúng nội dung TDD làm căn cứ cho các ca bên dưới. Giữ metadata Draft và các trường lịch sử import trống trong TDD; không tự giả lập phê duyệt bằng sửa Status.

| TDD | SHA-256 |
|---|---|
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | `c270ada012e651c0902898c0b92e9fdaf3ece98b301276b47ea7a3a65319f564` |
| [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | `cd8175f593ccdc4d742c8ac3eb1ec1a2e6b5a2502bc918156f815848538ad7cf` |
| [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | `8fb511864f3ada842e5db0e425585bd2372b2e472019dca3001fc7a9374b2dd4` |

## Danh sách 48 ca

Tất cả là đặc tả Draft, chưa thực thi. Reviewer/Approver: Tân Trần; Owner chưa xác định. Một file chứa một test và một dòng bảng; các biến thể trong Input là dữ liệu tham số hóa cùng hành vi.

| Ca | Unit / hành vi | TDD | System Test liên quan |
|---|---|---|---|
| [UT-PROJ-001](../unittest/UT-PROJ-001.md) | EstimateInputPolicy: Biên tên bản dự toán | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-002](../systemtest/ST-PROJ-002.md) |
| [UT-PROJ-002](../unittest/UT-PROJ-002.md) | EstimateInputPolicy: Đếm đúng giới hạn mô tả Unicode | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-007](../systemtest/ST-PROJ-007.md) |
| [UT-PROJ-003](../unittest/UT-PROJ-003.md) | EstimateInputPolicy: Diện tích không bị làm tròn hoặc nhân tầng | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-004](../systemtest/ST-PROJ-004.md) |
| [UT-PROJ-004](../unittest/UT-PROJ-004.md) | EstimateInputPolicy: Phân biệt lưu nháp và đủ đầu vào AI | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-005](../systemtest/ST-PROJ-005.md) |
| [UT-PROJ-005](../unittest/UT-PROJ-005.md) | EstimateInputPolicy: Đổi loại giữ phần hợp lệ và xóa đúng phần sai | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-009](../systemtest/ST-PROJ-009.md) |
| [UT-PROJ-006](../unittest/UT-PROJ-006.md) | EstimateInputPolicy: Không nhận thay đổi ngầm hoặc phong cách sai nhóm | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-008](../systemtest/ST-PROJ-008.md) |
| [UT-PROJ-007](../unittest/UT-PROJ-007.md) | EstimateInputPolicy: Đổi tỉnh đúng quan hệ và giữ chi tiết | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-010](../systemtest/ST-PROJ-010.md) |
| [UT-PROJ-008](../unittest/UT-PROJ-008.md) | CatalogConfigurationPolicy: Cấu hình bật không được thiếu lựa chọn | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-053](../systemtest/ST-PROJ-053.md) |
| [UT-PROJ-009](../unittest/UT-PROJ-009.md) | CatalogConfigurationPolicy: Giới hạn tên danh mục và cho phép trùng | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-058](../systemtest/ST-PROJ-058.md) |
| [UT-PROJ-010](../unittest/UT-PROJ-010.md) | EstimateInputPolicy: Không thay danh mục của bản cũ | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-020](../systemtest/ST-PROJ-020.md) |
| [UT-PROJ-011](../unittest/UT-PROJ-011.md) | EstimateInputPolicy: Yêu cầu đủ từng nhóm đang bật | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-051](../systemtest/ST-PROJ-051.md) |
| [UT-PROJ-012](../unittest/UT-PROJ-012.md) | CreateEstimateHandler: Tạo bản nháp không giữ lượt | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-001](../systemtest/ST-PROJ-001.md) |
| [UT-PROJ-013](../unittest/UT-PROJ-013.md) | CreateEstimateHandler / SaveEstimateInputHandler: Không bỏ qua quyền lợi khi tạo hoặc tự lưu | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-015](../systemtest/ST-PROJ-015.md) |
| [UT-PROJ-014](../unittest/UT-PROJ-014.md) | SaveEstimateInputHandler: Chặn lưu đến muộn và bản đã thành công | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md) |
| [UT-PROJ-015](../unittest/UT-PROJ-015.md) | SaveEstimateInputHandler: Gửi lại không ghi đè dữ liệu mới | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-014](../systemtest/ST-PROJ-014.md) |
| [UT-PROJ-016](../unittest/UT-PROJ-016.md) | CreateEstimateHandler / SaveEstimateInputHandler: Receipt không bỏ qua quyền sở hữu | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-018](../systemtest/ST-PROJ-018.md) |
| [UT-PROJ-017](../unittest/UT-PROJ-017.md) | SaveEstimateInputHandler: Thay ảnh không hợp lệ giữ ảnh cũ | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-011](../systemtest/ST-PROJ-011.md) |
| [UT-PROJ-018](../unittest/UT-PROJ-018.md) | SaveBuildingTypeHandler / SaveStyleHandler: Sửa danh mục không sửa lịch sử | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-052](../systemtest/ST-PROJ-052.md) |
| [UT-PROJ-019](../unittest/UT-PROJ-019.md) | RequestEstimateGenerationHandler: Tiếp nhận AI giữ đúng một lượt | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md) |
| [UT-PROJ-020](../unittest/UT-PROJ-020.md) | RequestEstimateGenerationHandler: Từ chối trước khi giữ lượt | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-031](../systemtest/ST-PROJ-031.md) |
| [UT-PROJ-021](../unittest/UT-PROJ-021.md) | RequestEstimateGenerationHandler: Lặp yêu cầu không trở thành thử lại AI | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-024](../systemtest/ST-PROJ-024.md) |
| [UT-PROJ-022](../unittest/UT-PROJ-022.md) | RequestEstimateGenerationHandler: Thử lại giữ đúng lịch sử và kiểm quyền mới | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-025](../systemtest/ST-PROJ-025.md) |
| [UT-PROJ-023](../unittest/UT-PROJ-023.md) | EstimateGenerationInputFactory: Đầu vào từng lần AI bất biến | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-051](../systemtest/ST-PROJ-051.md) |
| [UT-PROJ-024](../unittest/UT-PROJ-024.md) | UsageMaintenanceWorker: Hết lease không tự gửi AI lại | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md) |
| [UT-PROJ-025](../unittest/UT-PROJ-025.md) | UsageMaintenanceWorker: Worker cũ không ghi đè lần nhận việc mới | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md) |
| [UT-PROJ-026](../unittest/UT-PROJ-026.md) | EstimateResultStager: Kết quả thiếu hoặc không đọc được chưa thành công | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-023](../systemtest/ST-PROJ-023.md) |
| [UT-PROJ-027](../unittest/UT-PROJ-027.md) | FinalizeEstimateGenerationHandler: Chốt đúng kết quả và kỳ quota | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-022](../systemtest/ST-PROJ-022.md) |
| [UT-PROJ-028](../unittest/UT-PROJ-028.md) | FinalizeEstimateGenerationHandler: Đúng mốc timeout không nhận kết quả muộn | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md) |
| [UT-PROJ-029](../unittest/UT-PROJ-029.md) | FinalizeEstimateGenerationHandler: Thành công không bị tính hoặc trả lượt lần nữa | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-027](../systemtest/ST-PROJ-027.md) |
| [UT-PROJ-030](../unittest/UT-PROJ-030.md) | FinalizeEstimateGenerationHandler: Không lẫn kết quả giữa hai lần thử | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md) |
| [UT-PROJ-031](../unittest/UT-PROJ-031.md) | FinalizeEstimateGenerationHandler: Đổi hoặc hết hạn gói không làm lệch kỳ quyết toán | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-029](../systemtest/ST-PROJ-029.md) |
| [UT-PROJ-032](../unittest/UT-PROJ-032.md) | IDesignUsageCoordinator — Fail/Expire: Trả lượt đúng một lần khi thất bại | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-024](../systemtest/ST-PROJ-024.md) |
| [UT-PROJ-033](../unittest/UT-PROJ-033.md) | EstimateResultReader: Xem lại bước 2–3 dùng kết quả đã lưu | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-033](../systemtest/ST-PROJ-033.md) |
| [UT-PROJ-034](../unittest/UT-PROJ-034.md) | EstimateResultReader: Có dữ liệu tạm không đồng nghĩa được xem kết quả | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-037](../systemtest/ST-PROJ-037.md) |
| [UT-PROJ-035](../unittest/UT-PROJ-035.md) | RequestEstimateExportHandler: Xuất tệp cũ không tạo thiết kế mới | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-035](../systemtest/ST-PROJ-035.md) |
| [UT-PROJ-036](../unittest/UT-PROJ-036.md) | EstimateExportWorker: Lỗi hoặc worker cũ không thay nguồn hồ sơ | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-038](../systemtest/ST-PROJ-038.md) |
| [UT-PROJ-037](../unittest/UT-PROJ-037.md) | CreateEstimateShareHandler: Ngày hết hạn theo Việt Nam | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-059](../systemtest/ST-PROJ-059.md) |
| [UT-PROJ-038](../unittest/UT-PROJ-038.md) | CreateEstimateShareHandler: Dùng lại một link khi còn hiệu lực | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-060](../systemtest/ST-PROJ-060.md) |
| [UT-PROJ-039](../unittest/UT-PROJ-039.md) | CreateEstimateShareHandler: Link mới không phục hồi link cũ | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-060](../systemtest/ST-PROJ-060.md) |
| [UT-PROJ-040](../unittest/UT-PROJ-040.md) | EstimateShareAccessPolicy: Kiểm đủ điều kiện trên mỗi lần truy cập | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-041](../systemtest/ST-PROJ-041.md) |
| [UT-PROJ-041](../unittest/UT-PROJ-041.md) | RevokeEstimateShareHandler: Thu hồi không sửa hồ sơ nguồn | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md) |
| [UT-PROJ-042](../unittest/UT-PROJ-042.md) | EstimateFileReader: Không có đường tải vượt quyền chia sẻ | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md) |
| [UT-PROJ-043](../unittest/UT-PROJ-043.md) | QueueEstimateEmailHandler: Mỗi yêu cầu email chỉ một người nhận | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-044](../systemtest/ST-PROJ-044.md) |
| [UT-PROJ-044](../unittest/UT-PROJ-044.md) | QueueEstimateEmailHandler: Gửi lại request không gửi trùng email | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-045](../systemtest/ST-PROJ-045.md) |
| [UT-PROJ-045](../unittest/UT-PROJ-045.md) | EstimateEmailWorker: Email dùng cùng link và chỉ xác nhận tiếp nhận | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-043](../systemtest/ST-PROJ-043.md) |
| [UT-PROJ-046](../unittest/UT-PROJ-046.md) | EstimateEmailWorker: Không coi trạng thái chưa rõ là gửi thất bại chắc chắn | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-045](../systemtest/ST-PROJ-045.md) |
| [UT-PROJ-047](../unittest/UT-PROJ-047.md) | EstimateEmailWorker: Không tự gửi link thay thế cho yêu cầu cũ | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md) |
| [UT-PROJ-048](../unittest/UT-PROJ-048.md) | IEstimateMailSender — adapter dự kiến: Lỗi đóng kết nối không làm mất bằng chứng gửi | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-045](../systemtest/ST-PROJ-045.md) |

## Phạm vi và phần cần kiểm chứng tiếp

- Unit tập trung vào validator/policy, trạng thái, hash/key chống lặp, đối số truyền giữa handler và port, quyết định công bố và quyền đọc. Expected output được viết theo TDD/BR đã chốt, không lấy giá trị fake trả về làm bằng chứng tự đủ cho assertion.
- Việc đọc lại bước 2–3 phải giữ đúng source operation và giá trị dữ liệu, không gọi AI/quota. ST-PROJ-022, 033–038 và 057 kiểm chứng thêm bằng lưu/đọc thật và tệp thật trong môi trường thử.
- Không dùng các fake store ở UT để kết luận đã bảo vệ transaction, FK, unique index hoặc khóa đồng thời. Tranh lượt, chốt-vs-timeout, tạo-vs-sửa danh mục, nhiều yêu cầu tạo link và rollback cần PostgreSQL thật; bám ST-PROJ-021–032, 052, 056 và 060.
- Xác thực, CSRF, HTTP status/envelope, stream HEAD/Range, nội dung thật của ảnh JPG/PNG/HEIC/WebP, giới hạn byte, keyring và khôi phục tệp cần kiểm tra ở adapter/tầng tích hợp. Bám ST-PROJ-006, 011, 018, 034–047, 054–057 và 059–060; các nhánh CSRF/keyring/fencing bổ sung bộ tích hợp kỹ thuật khi triển khai, không coi ST hiện có đã phủ mọi chi tiết kỹ thuật.
- Giữ nội dung chưa lưu, tự lưu lại khi mạng phục hồi, QR và tải tệp từ fragment/header cần trình duyệt thật. Chưa có frontend trong workspace.
- Validator payload chuyên môn AI và exporter PDF/Excel chi tiết chưa thể đặc tả khi thiếu hợp đồng. Ca dùng envelope opaque chỉ kiểm luồng điều phối, không chứng minh dữ liệu AI thực đúng.
- Chưa hoàn tất rà soát ngữ nghĩa toàn bộ chuỗi tài liệu ngoài PROJ như đã ghi trong bảng TDD. Xác nhận thiết kế không thay bằng chứng đã rà soát hoặc chạy kiểm thử. Sau khi tích hợp được bổ sung làm thay đổi TDD, cần đối chiếu lại các ca bị ảnh hưởng.

[Bảng Story/AC → BR → TDD](estimate-technical-design.md#truy-vết-và-kiểm-chứng) và [bảng từng AC/luồng → ST](estimate-system-test-coverage.md) là nguồn truy vết bổ sung; không coi số ca là mức bao phủ mã hoặc kết quả Pass.

## Kiểm tra tài liệu đã thực hiện

Đã kiểm cấu trúc 48 file: một test/một dòng dữ liệu, đúng 13 cột, mã và giá trị phân loại hợp lệ, Reviewer/Approver đúng phân công, Trace to khớp TEST_LINKS và section đích tồn tại. Các liên kết trong hai bảng bàn giao và ba hash TDD khớp file hiện tại. Không phát hiện lỗi trong các kiểm tra này; chưa chạy importer, mã Unit Test hoặc test tích hợp.
