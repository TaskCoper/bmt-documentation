# Đặc tả Unit Test Tạo dự toán

Người dùng xác nhận “Ok chốt đi” cho TDD-PROJ-001, TDD-PROJ-002 và TDD-PROJ-003 sau khi làm rõ việc lưu và xem lại kết quả bước 2–3. Đây là xác nhận thiết kế trong hội thoại, không phải thao tác import/publish hoặc phê duyệt trên hệ thống quản lý tài liệu.

Phạm vi chốt gồm thiết kế hiện có và các phần chờ tích hợp đã nêu rõ. Chưa có hợp đồng AI, lựa chọn kho tệp (ngày 26/09/2026 đã chốt không có kho tệp riêng, xem mục cập nhật cuối), nguồn địa chỉ hoặc thông số vận hành; không coi xác nhận này là đã bổ sung các dữ liệu còn thiếu. Chưa triển khai mã hoặc chạy test.

## Mốc TDD được chốt

Hash dưới đây ghi nhận nội dung TDD làm căn cứ cho 48 ca ban đầu. Giữ metadata Draft và các trường lịch sử import trống trong TDD; không tự giả lập phê duyệt bằng sửa Status. Ba TDD đã được cập nhật ngày 25/09/2026; mốc mới ghi ở mục [Cập nhật ngày 25/09/2026](#cập-nhật-ngày-25092026).

| TDD | SHA-256 |
|---|---|
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | `c270ada012e651c0902898c0b92e9fdaf3ece98b301276b47ea7a3a65319f564` |
| [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | `cd8175f593ccdc4d742c8ac3eb1ec1a2e6b5a2502bc918156f815848538ad7cf` |
| [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | `8fb511864f3ada842e5db0e425585bd2372b2e472019dca3001fc7a9374b2dd4` |

## Danh sách 66 ca

Tất cả là đặc tả Draft, chưa thực thi. Reviewer/Approver: Tân Trần; Owner chưa xác định. Một file chứa một test và một dòng bảng; các biến thể trong Input là dữ liệu tham số hóa cùng hành vi. UT-PROJ-049 đến UT-PROJ-061 được thêm ngày 25/09/2026; UT-PROJ-062 và UT-PROJ-063 được thêm ngày 26/09/2026; UT-PROJ-064 đến UT-PROJ-066 được thêm cùng ngày khi triển khai TDD-PROJ-002.

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
| [UT-PROJ-026](../unittest/UT-PROJ-026.md) | EstimateResultFileVerifier: Kết quả thiếu hoặc URL tệp không mở được chưa thành công | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-023](../systemtest/ST-PROJ-023.md) |
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
| [UT-PROJ-049](../unittest/UT-PROJ-049.md) | RenameEstimateHandler: Đổi tên không phụ thuộc gói, lượt hay trạng thái AI | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-061](../systemtest/ST-PROJ-061.md) |
| [UT-PROJ-050](../unittest/UT-PROJ-050.md) | RenameEstimateHandler: NameVersion cũ không ghi đè tên đã đổi ở nơi khác | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-068](../systemtest/ST-PROJ-068.md) |
| [UT-PROJ-051](../unittest/UT-PROJ-051.md) | RenameEstimateHandler: Gửi lại đúng tên đang lưu thành công, không tăng NameVersion | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-068](../systemtest/ST-PROJ-068.md) |
| [UT-PROJ-052](../unittest/UT-PROJ-052.md) | RenameEstimateHandler / validator: Từ chối trước khi ghi khi tên sai hoặc không phải tài khoản khách | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-067](../systemtest/ST-PROJ-067.md) |
| [UT-PROJ-053](../unittest/UT-PROJ-053.md) | SaveEstimateInputHandler / validator: PUT /input không nhận tên | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | Chưa liên kết System Test |
| [UT-PROJ-054](../unittest/UT-PROJ-054.md) | EstimateGenerationInputFactory: Snapshot gửi AI không chứa tên bản dự toán | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-064](../systemtest/ST-PROJ-064.md) |
| [UT-PROJ-055](../unittest/UT-PROJ-055.md) | RequestEstimateGenerationHandler: Đổi tên trước khi gửi AI không gây InputVersionConflict | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-064](../systemtest/ST-PROJ-064.md) |
| [UT-PROJ-056](../unittest/UT-PROJ-056.md) | RequestEstimateExportHandler: Đổi tên không tạo export mới | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [UT-PROJ-057](../unittest/UT-PROJ-057.md) | EstimateExportWorker: Lấy tệp không phụ thuộc tên bản dự toán | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [UT-PROJ-058](../unittest/UT-PROJ-058.md) | EstimateFileReader: Tên tệp tải về theo tên hiện tại sau khi đổi tên | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [UT-PROJ-059](../unittest/UT-PROJ-059.md) | EstimateResultReader: Hồ sơ chủ sở hữu và trang chia sẻ dùng tên hiện tại | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [UT-PROJ-060](../unittest/UT-PROJ-060.md) | EstimateEmailWorker: Thư dựng bằng tên hiện tại lúc gửi | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [UT-PROJ-061](../unittest/UT-PROJ-061.md) | GetEstimate query: canRename tách khỏi canEdit | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-061](../systemtest/ST-PROJ-061.md) |
| [UT-PROJ-062](../unittest/UT-PROJ-062.md) | ProvincesOpenApiLocationCatalog: Dùng bản lưu khi nguồn địa chỉ lỗi, 503 khi chưa có bản nào | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-071](../systemtest/ST-PROJ-071.md) |
| [UT-PROJ-063](../unittest/UT-PROJ-063.md) | ProvincesOpenApiLocationCatalog / SaveEstimateInputHandler: Xã thuộc tỉnh theo đúng phiên bản dữ liệu hiện hành | [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | [ST-PROJ-010](../systemtest/ST-PROJ-010.md) |
| [UT-PROJ-064](../unittest/UT-PROJ-064.md) | EstimateAiInfrastructureExtensions / EstimateGenerationAvailability: Chặn adapter giả ngoài Development/Staging, 503 khi chưa có adapter | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-072](../systemtest/ST-PROJ-072.md) |
| [UT-PROJ-065](../unittest/UT-PROJ-065.md) | RequestEstimateGenerationHandler: Kiểm lại địa chỉ theo dữ liệu hiện hành trước khi gửi AI | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-073](../systemtest/ST-PROJ-073.md) |
| [UT-PROJ-066](../unittest/UT-PROJ-066.md) | MockEstimateAiGateway / MockEstimateResultContract: Adapter giả luôn thành công với bộ kết quả mẫu cố định | [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | [ST-PROJ-072](../systemtest/ST-PROJ-072.md) |

## Phạm vi và phần cần kiểm chứng tiếp

- Unit tập trung vào validator/policy, trạng thái, hash/key chống lặp, đối số truyền giữa handler và port, quyết định công bố và quyền đọc. Expected output được viết theo TDD/BR đã chốt, không lấy giá trị fake trả về làm bằng chứng tự đủ cho assertion.
- Việc đọc lại bước 2–3 phải giữ đúng source operation và giá trị dữ liệu, không gọi AI/quota. ST-PROJ-022, 033–038 và 057 kiểm chứng thêm bằng lưu/đọc thật và tệp thật trong môi trường thử.
- Không dùng các fake store ở UT để kết luận đã bảo vệ transaction, FK, unique index hoặc khóa đồng thời. Tranh lượt, chốt-vs-timeout, tạo-vs-sửa danh mục, nhiều yêu cầu tạo link và rollback cần PostgreSQL thật; bám ST-PROJ-021–032, 052, 056 và 060.
- Xác thực, CSRF, HTTP status/envelope, stream HEAD/Range khi backend chuyển tiếp tệp từ URL, header `Content-Disposition`, kiểm tên máy chủ của URL tệp và keyring cần kiểm tra ở adapter/tầng tích hợp. Định dạng và dung lượng ảnh JPG/PNG/HEIC/WebP do frontend kiểm trước khi upload qua presign, backend chỉ kiểm URL (TDD-PROJ-001); backend không giữ bản sao tệp nên không có bước khôi phục tệp riêng. Bám ST-PROJ-006, 011, 018, 034–047, 054–057 và 059–060; các nhánh keyring/fencing bổ sung bộ tích hợp kỹ thuật khi triển khai, không coi ST hiện có đã phủ mọi chi tiết kỹ thuật. Nhánh CSRF dùng lớp chung ở [TDD-AUTH-001](../tdd/TDD-AUTH-001.md), đã có test qua pipeline HTTP trong `test/bmt-be.api.tests/security/` của `bmt-be`; khi có route dự toán, chỉ cần kiểm route đó đi qua `UseApiPipeline`.
- Giữ nội dung chưa lưu, tự lưu lại khi mạng phục hồi, QR và tải tệp từ fragment/header cần trình duyệt thật. Chưa có frontend trong workspace.
- Validator payload chuyên môn AI và nguồn tệp PDF/Excel (`IEstimateExportFileSource`, lấy URL do AI trả) chưa thể đặc tả chi tiết khi thiếu hợp đồng. Ca dùng envelope opaque chỉ kiểm luồng điều phối, không chứng minh dữ liệu AI thực đúng.
- Chưa hoàn tất rà soát ngữ nghĩa toàn bộ chuỗi tài liệu ngoài PROJ như đã ghi trong bảng TDD. Xác nhận thiết kế không thay bằng chứng đã rà soát hoặc chạy kiểm thử. Sau khi tích hợp được bổ sung làm thay đổi TDD, cần đối chiếu lại các ca bị ảnh hưởng.

[Bảng Story/AC → BR → TDD](estimate-technical-design.md#truy-vết-và-kiểm-chứng) và [bảng từng AC/luồng → ST](estimate-system-test-coverage.md) là nguồn truy vết bổ sung; không coi số ca là mức bao phủ mã hoặc kết quả Pass.

## Kiểm tra tài liệu đã thực hiện

Lần bàn giao đầu đã kiểm cấu trúc 48 file: một test/một dòng dữ liệu, đúng 13 cột, mã và giá trị phân loại hợp lệ, Reviewer/Approver đúng phân công, Trace to khớp TEST_LINKS và section đích tồn tại. Các liên kết trong hai bảng bàn giao và ba hash TDD khớp file tại thời điểm đó. Ngày 25/09/2026 đã kiểm lại cả 61 file bằng script đọc TEST_LINKS: không có lỗi cấu trúc, lệch Trace to hoặc section đích thiếu. Chưa chạy importer, mã Unit Test hoặc test tích hợp.

## Cập nhật ngày 25/09/2026

Ba TDD được sửa theo US/BR chốt ngày 25/09/2026 và người dùng xác nhận bản sửa trước khi cập nhật UT: API đổi tên riêng `PATCH /api/v1/estimates/{estimateId}/name` với `NameVersion`, bỏ tên khỏi snapshot gửi AI, xuất lại tệp khi tên đổi (`EstimateExport.NameVersion`, lỗi 409 `ExportOutdated`) và dùng lại link còn hiệu lực kèm `requestedExpiryApplied`. UT-PROJ-001, UT-PROJ-012 và UT-PROJ-038 được sửa theo thiết kế này; UT-PROJ-049 đến UT-PROJ-061 được thêm mới. Hash dưới đây là mốc TDD cho bộ 61 ca, chỉ dùng đối chiếu nội dung, không xác nhận phê duyệt.

| TDD | SHA-256 |
|---|---|
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | `bd43872f877e36ff4887f40aad14198936f9ca0c51ad7d31b3b3e3191d0444a3` |
| [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | `afd9b8dec70d37d103eccc86172dd87c6c44eb9c52f5030cfcf4bcceaa511e15` |
| [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | `adaa88e334833cfbe9241e27ff6497212bfc29883020806388202a8a5f3aeff7` |

## Cập nhật ngày 26/09/2026 (lần 2)

TDD-PROJ-001 được sửa theo bốn quyết định người dùng xác nhận ngày 26/09/2026: URL ảnh mới phải thuộc tên miền kho presign; nhóm lựa chọn tắt vẫn giữ danh sách nhưng khách không chọn được; người vận hành bật cổng tạo bản dự toán; nguồn địa chỉ là provinces.open-api.vn v2 có bản lưu Redis. UT-PROJ-008, 011, 017 và 018 được sửa theo các quyết định này và ghi tên mã test; UT-PROJ-062 và UT-PROJ-063 được thêm cho adapter địa chỉ. TDD-PROJ-002 và TDD-PROJ-003 không đổi trong đợt này. Đã kiểm lại cấu trúc sáu file UT bị đụng tới (một dòng, 13 cột, Trace to khớp TEST_LINKS).

## Cập nhật ngày 26/09/2026 (lần 3)

Người dùng xác nhận ngày 26/09/2026 hai nhóm quyết định làm thay đổi các ca UT về tệp:

- **Lưu URL, không có kho tệp riêng.** Backend lưu URL ảnh đầu vào đã kiểm theo `UploadedFileOption__AllowedHosts`; AI trả URL cho mọi tệp kết quả, kể cả PDF và Excel; backend chuyển tiếp tệp qua route có kiểm quyền, không lộ URL gốc. `EstimateResultStager`, `EstimateAsset` và các khóa AssetId đã bỏ khỏi TDD-PROJ-002/003. `EstimateResultFileVerifier` đọc thử URL trước khi chốt; `EstimateFileReader` kiểm quyền rồi đọc tệp theo URL và chuyển tiếp. UT-PROJ-026, 036, 042, 057 và 058 được sửa theo thiết kế này.
- **Tên tệp sau khi đổi tên.** Tên mới chỉ áp vào tên tệp tải về (`Content-Disposition`), nội dung PDF/Excel giữ như lúc AI tạo (BR-PROJ-007 khoản 7). Export chỉ gắn với nguồn và định dạng; bỏ `EstimateExport.NameVersion`, `isCurrent`, trạng thái Failed `EstimateRenamed` và mã 409 `ExportOutdated` của bản ngày 25/09/2026. UT-PROJ-056, 057, 058 và 059 được sửa theo; ST-PROJ-069 cũng sửa bước kiểm tên.

Đã kiểm lại cấu trúc các UT bị đụng tới (một dòng, 13 cột, Trace to khớp TEST_LINKS). Hash TDD ở các mục trên là mốc cũ, không còn khớp nội dung hiện tại của TDD-PROJ-002/003; chưa ghi mốc mới vì TDD chưa được chốt lại sau đợt sửa này.

## Cập nhật ngày 26/09/2026 (lần 4): triển khai TDD-PROJ-002

TDD-PROJ-002 được triển khai với adapter AI giả `mock-v1` theo quyết định người dùng cùng ngày, trên nhánh `feature/estimate-generation` của `bmt-be` (commit `62a626d`). UT-PROJ-019 đến UT-PROJ-032, UT-PROJ-054 và UT-PROJ-055 nay có mã test: cột Precondition ghi "Đã có mã test", tên test nằm ở cột Rationale; nội dung Expected output không đổi. Ba ca mới UT-PROJ-064 đến UT-PROJ-066 kiểm chế độ adapter, việc kiểm lại địa chỉ trước khi gửi AI và bộ kết quả mẫu của adapter giả. Các phần cần PostgreSQL (tranh lượt cuối, chốt-vs-timeout, đổi/hủy kỳ lúc tiếp nhận, gửi lặp cùng khóa, worker dừng trước/sau gửi, hai worker nhận việc cùng lúc, kết quả tới sau TimedOut) có integration test trong `EstimateGenerationFlowTests` và `EstimateGenerationConstraintTests`. Test với adapter giả chứng minh điều phối nội bộ, không chứng minh hợp đồng AI thật. Đã kiểm cấu trúc các file UT bị đụng tới (một dòng, 13 cột, Trace to khớp TEST_LINKS).

Bổ sung cùng ngày: người dùng xác nhận URL tệp AI trả về sai dạng hoặc ngoài danh sách tên máy chủ thì thất bại ngay với InvalidProviderResult; UT-PROJ-026 được sửa Expected output và tên test theo quyết định này (commit `624e212`). Nhánh đã rebase lên `develop` sau TDD-LIB-002; SHA triển khai nay là `62a626d`, migration `20260926141806_EstimateGeneration`.

## Cập nhật ngày 26/09/2026 (lần 5): triển khai TDD-PROJ-003

TDD-PROJ-003 được triển khai trên nhánh `feature/estimate-sharing` của `bmt-be` (commit `f8a7ffa`). UT-PROJ-033 đến UT-PROJ-048 và UT-PROJ-056 đến UT-PROJ-060 nay có mã test: cột Precondition ghi "Đã có mã test", tên test nằm ở cột Rationale; Unit under test ghi thêm tên lớp trong code; nội dung Expected output không đổi. Kết quả nguồn trong test là của adapter AI giả `mock-v1`, dùng như kết quả thật theo quyết định của người dùng.

Phần cần PostgreSQL có integration test trong `EstimateSharingFlowTests` và `EstimateSharingConstraintTests`: sáu yêu cầu tạo link đồng thời chỉ ra một link, hết hạn theo ngày Việt Nam, chặn tải qua link sau khi thu hồi mà không mở URL tệp, yêu cầu email và message outbox cùng commit hoặc cùng rollback, worker email ghi Unknown/Skipped, gói hết hạn vẫn dùng hồ sơ cũ, yêu cầu PDF đồng thời chỉ ra một export, lease cũ không ghi đè và các ràng buộc database. Chuyển tiếp tệp (Range, không theo chuyển hướng, không lộ URL gốc) kiểm bằng HttpMessageHandler giả; SMTP kiểm bằng `ISmtpClient` giả; header tải tệp, 401/403/404 và chống CSRF kiểm qua pipeline HTTP. Không gọi mạng hay SMTP thật. Đã kiểm cấu trúc các file UT bị đụng tới (một dòng, 13 cột, Trace to khớp TEST_LINKS).

## Cập nhật ngày 27/09/2026: tăng cường TDD-PROJ-003

TDD-PROJ-003 được bổ sung bốn quyết định người dùng xác nhận ngày 27/09/2026 (Problem, Architecture/Notes mục "Tăng cường ngày 27/09/2026"). Ba ca mới đã có mã test trên nhánh `feature/estimate-sharing-hardening` của `bmt-be` (commit `8de4c21`). UT-PROJ-067 và UT-PROJ-068 thuộc một thay đổi khác làm cùng ngày, không nằm trong mục này.

| Ca | Unit / hành vi | TDD | System Test liên quan |
|---|---|---|---|
| [UT-PROJ-069](../unittest/UT-PROJ-069.md) | EstimateShareRateLimits: Giới hạn tần suất riêng theo IP và theo link của route công khai | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-074](../systemtest/ST-PROJ-074.md) |
| [UT-PROJ-070](../unittest/UT-PROJ-070.md) | EstimateEmailWorker / EstimateShareEmailTemplate: Thư ghi tên chủ hồ sơ, không có email hay số điện thoại | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-075](../systemtest/ST-PROJ-075.md) |
| [UT-PROJ-071](../unittest/UT-PROJ-071.md) | EstimateSharingStuckRecovery: Job đóng yêu cầu xuất tệp và email bị kẹt | [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | [ST-PROJ-076](../systemtest/ST-PROJ-076.md), [ST-PROJ-077](../systemtest/ST-PROJ-077.md) |

Giới hạn tần suất kiểm qua pipeline HTTP thật (TestServer), không có proxy thật. Điều kiện kẹt và việc job chạy đồng thời với consumer xuất tệp và consumer gửi email kiểm trên PostgreSQL thật trong `EstimateSharingStuckRecoveryFlowTests`. Đã kiểm cấu trúc ba file UT mới (một dòng, 13 cột, Trace to khớp TEST_LINKS).

## Mốc cập nhật ngày 11/10/2026: luồng bốn chặng, đợt 1

Người dùng chốt TDD-PROJ-006, TDD-PROJ-007 và TDD-PROJ-008 ngày 11/10/2026. Đã soạn 134 đặc tả Unit Test mới:
- UT-PROJ-133 → 173 cho TDD-PROJ-006;
- UT-PROJ-180 → 224 cho TDD-PROJ-007;
- UT-PROJ-240 → 287 cho TDD-PROJ-008.

Các mã 174–179, 225–239 và 288–289 còn trống lúc đó; đã dùng ở mốc 12/10/2026 bên dưới.

Tất cả là unit dự kiến, chưa có mã và chưa chạy. Script kiểm tra cho kết quả:
- mỗi file một heading trùng Test ID;
- đúng 13 cột;
- Trace to khớp TEST_LINKS;
- mọi mã và mục được dẫn đều có thật.

Mọi khoản BR-PROJ-010 đến 015, 017, 018, 019 thuộc đợt 1 đều có ca truy vết. BR-PROJ-016 (luật giá) thuộc đợt 2.

Các hành vi chỉ chứng minh được trên PostgreSQL thật không viết thành unit test. Rationale của ca gần nhất ghi rõ chúng thuộc integration test:
- chỉ mục duy nhất từng phần của `EstimateStageRun`;
- `SKIP LOCKED`;
- tranh chấp giữa finalizer và `ExpireStaleUsageJob`;
- CHECK mới;
- upsert hạn mức soi ảnh đồng thời;
- `xmin` của bảng miền giá;
- migration nạp quyền.

| Test | TDD | Unit | Loại | Ưu tiên |
|---|---|---|---|---|
| [UT-PROJ-133](../unittest/UT-PROJ-133.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Luồng bốn chặng đang tắt | Branch | P0 |
| [UT-PROJ-134](../unittest/UT-PROJ-134.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Chặng chưa được bật trong EnabledStages | Branch | P1 |
| [UT-PROJ-135](../unittest/UT-PROJ-135.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Gửi lặp cùng key, cùng nội dung | Determinism | P0 |
| [UT-PROJ-136](../unittest/UT-PROJ-136.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Cùng key, khác nội dung | Error | P1 |
| [UT-PROJ-137](../unittest/UT-PROJ-137.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Chặng trước chưa thành công | Branch | P0 |
| [UT-PROJ-138](../unittest/UT-PROJ-138.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Chặng đã thành công | Branch | P0 |
| [UT-PROJ-139](../unittest/UT-PROJ-139.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Đang có lần chạy hoạt động | Branch | P0 |
| [UT-PROJ-140](../unittest/UT-PROJ-140.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Chạm giới hạn lỗi và sau khi Admin đặt lại | Boundary | P0 |
| [UT-PROJ-141](../unittest/UT-PROJ-141.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Giới hạn lỗi bị hạ thấp hơn số lần đã lỗi | Boundary | P1 |
| [UT-PROJ-142](../unittest/UT-PROJ-142.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Plan2d gửi phiên bản đầu vào cũ | Error | P1 |
| [UT-PROJ-143](../unittest/UT-PROJ-143.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Plan2d đầu vào chưa đủ | Error | P0 |
| [UT-PROJ-144](../unittest/UT-PROJ-144.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Plan2d tiếp nhận hợp lệ giữ lượt và chụp đầu vào | Happy | P0 |
| [UT-PROJ-145](../unittest/UT-PROJ-145.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Chặng sau 2D không chạm lượt | Branch | P0 |
| [UT-PROJ-146](../unittest/UT-PROJ-146.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Bản dự toán đã xóa hoặc không thuộc khách | Error | P1 |
| [UT-PROJ-147](../unittest/UT-PROJ-147.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Plan2d khi gói không còn hiệu lực hoặc hết lượt | Error | P1 |
| [UT-PROJ-148](../unittest/UT-PROJ-148.md) | TDD-PROJ-006 | RunEstimateStageCommandValidator — Kiểm yêu cầu chạy chặng | Boundary | P1 |
| [UT-PROJ-149](../unittest/UT-PROJ-149.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — 2D thành công trước hạn tính một lượt | Happy | P0 |
| [UT-PROJ-150](../unittest/UT-PROJ-150.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — 2D lỗi hoàn lượt | Branch | P0 |
| [UT-PROJ-151](../unittest/UT-PROJ-151.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — Kết quả tới sau hạn chờ | Boundary | P0 |
| [UT-PROJ-152](../unittest/UT-PROJ-152.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — Lần chạy đã kết thúc | Determinism | P1 |
| [UT-PROJ-153](../unittest/UT-PROJ-153.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — UsageOperation đã bị job chung chốt quá hạn | Quirk | P0 |
| [UT-PROJ-154](../unittest/UT-PROJ-154.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — Thiếu bước bắt buộc | Error | P1 |
| [UT-PROJ-155](../unittest/UT-PROJ-155.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — Chặng sau 2D lỗi không chạm lượt | Branch | P1 |
| [UT-PROJ-156](../unittest/UT-PROJ-156.md) | TDD-PROJ-006 | RecordStageStepCommandHandler — Ghi bước có điều kiện lease | Branch | P0 |
| [UT-PROJ-157](../unittest/UT-PROJ-157.md) | TDD-PROJ-006 | EstimateInputLock.IsInputLocked — Bảng chân trị khóa đầu vào | Branch | P0 |
| [UT-PROJ-158](../unittest/UT-PROJ-158.md) | TDD-PROJ-006 | SaveEstimateInputCommandHandler — Từ chối lưu khi đầu vào khóa theo chặng 2D | Error | P0 |
| [UT-PROJ-159](../unittest/UT-PROJ-159.md) | TDD-PROJ-006 | StageStatusProjector — Trạng thái và số lần chạy lại còn lại | Branch | P1 |
| [UT-PROJ-160](../unittest/UT-PROJ-160.md) | TDD-PROJ-006 | MyEstimateStatusResolver — Năm trạng thái và thứ tự ưu tiên | Branch | P0 |
| [UT-PROJ-161](../unittest/UT-PROJ-161.md) | TDD-PROJ-006 | MyEstimatesQueryValidator — Giá trị lọc trạng thái mới | Boundary | P2 |
| [UT-PROJ-162](../unittest/UT-PROJ-162.md) | TDD-PROJ-006 | ResetStageFailuresCommandHandler — Đặt lại bộ đếm lỗi | Happy | P0 |
| [UT-PROJ-163](../unittest/UT-PROJ-163.md) | TDD-PROJ-006 | ResetStageFailuresCommandHandler — Từ chối khi không có gì để đặt lại | Error | P1 |
| [UT-PROJ-164](../unittest/UT-PROJ-164.md) | TDD-PROJ-006 | EstimatePipelineModeGuard — Kiểm cấu hình lúc khởi động | Error | P0 |
| [UT-PROJ-165](../unittest/UT-PROJ-165.md) | TDD-PROJ-006 | RequestEstimateGenerationCommandHandler — Luồng cũ bị chặn khi luồng mới bật | Branch | P0 |
| [UT-PROJ-166](../unittest/UT-PROJ-166.md) | TDD-PROJ-006 | DeleteMyEstimatesCommandHandler — Chặn xóa bản có lần chạy hoạt động | Branch | P0 |
| [UT-PROJ-167](../unittest/UT-PROJ-167.md) | TDD-PROJ-006 | EstimateStageWorker — Mất lease khi gia hạn | Error | P0 |
| [UT-PROJ-168](../unittest/UT-PROJ-168.md) | TDD-PROJ-006 | EstimatePipelineMaintenanceJob — Quét quá hạn gửi TimedOut cho từng run | Branch | P1 |
| [UT-PROJ-169](../unittest/UT-PROJ-169.md) | TDD-PROJ-006 | EstimateStageWorker — Chạy tiếp bỏ qua bước đã lưu | Happy | P0 |
| [UT-PROJ-170](../unittest/UT-PROJ-170.md) | TDD-PROJ-006 | EstimateStageWorker — Lỗi cuối của executor chốt Failed | Branch | P1 |
| [UT-PROJ-171](../unittest/UT-PROJ-171.md) | TDD-PROJ-006 | EstimateStageWorker — Hủy do mất lease không chốt Failed | Quirk | P1 |
| [UT-PROJ-172](../unittest/UT-PROJ-172.md) | TDD-PROJ-006 | EstimateStageWorker — Token của executor hết hạn tại DeadlineUtc | Boundary | P1 |
| [UT-PROJ-173](../unittest/UT-PROJ-173.md) | TDD-PROJ-006 | EstimatePipelineMaintenanceJob — Cảnh báo lần chạy chờ quá lâu | Branch | P2 |
| [UT-PROJ-180](../unittest/UT-PROJ-180.md) | TDD-PROJ-007 | SaveEstimateInputCommandValidator — kích thước lô dạng chuỗi thập phân | Boundary | P1 |
| [UT-PROJ-181](../unittest/UT-PROJ-181.md) | TDD-PROJ-007 | SaveEstimateInputCommandValidator — cạnh cửa vào, số phòng ngủ, gara | Boundary | P1 |
| [UT-PROJ-182](../unittest/UT-PROJ-182.md) | TDD-PROJ-007 | SaveEstimateInputCommandValidator — mô tả tối đa 4.000 ký tự đếm theo Rune | Boundary | P1 |
| [UT-PROJ-183](../unittest/UT-PROJ-183.md) | TDD-PROJ-007 | SaveEstimateInputCommandHandler — đổi ảnh lô xóa con trỏ kết quả soi | Branch | P0 |
| [UT-PROJ-184](../unittest/UT-PROJ-184.md) | TDD-PROJ-006 | SaveEstimateInputCommandHandler — đầu vào khóa theo lần chạy 2D | Branch | P0 |
| [UT-PROJ-185](../unittest/UT-PROJ-185.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — đủ đầu vào, có và không có mô tả | Happy | P0 |
| [UT-PROJ-186](../unittest/UT-PROJ-186.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — thiếu ảnh, chưa soi Đạt, kết quả soi lệch ảnh | Branch | P0 |
| [UT-PROJ-187](../unittest/UT-PROJ-187.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — số phòng ngủ thiếu | Branch | P1 |
| [UT-PROJ-188](../unittest/UT-PROJ-188.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — kích thước lô một ô và ngoài giới hạn | Boundary | P0 |
| [UT-PROJ-189](../unittest/UT-PROJ-189.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — danh mục chưa gắn mã gốc hoặc thiếu mô tả cho AI | Branch | P0 |
| [UT-PROJ-190](../unittest/UT-PROJ-190.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — tỉnh chưa có miền giá | Branch | P1 |
| [UT-PROJ-191](../unittest/UT-PROJ-191.md) | TDD-PROJ-007 | EstimateInputPolicy.FindMissingForStage(Plan2d) — Căn hộ không đòi tầng, tum, phong cách kiến trúc | Branch | P1 |
| [UT-PROJ-192](../unittest/UT-PROJ-192.md) | TDD-PROJ-007 | CatalogConfigurationPolicy — mã gốc và mã phong cách gốc | Branch | P1 |
| [UT-PROJ-193](../unittest/UT-PROJ-193.md) | TDD-PROJ-007 | CatalogConfigurationPolicy — mô tả cho AI của phong cách nội thất | Boundary | P1 |
| [UT-PROJ-194](../unittest/UT-PROJ-194.md) | TDD-PROJ-007 | CatalogConfigurationPolicy — số tầng 1 đến 4 | Boundary | P1 |
| [UT-PROJ-195](../unittest/UT-PROJ-195.md) | TDD-PROJ-007 | MediaUploadInputValidator — purpose EstimateLotImage cần estimateId và giới hạn 10 MiB | Boundary | P0 |
| [UT-PROJ-196](../unittest/UT-PROJ-196.md) | TDD-PROJ-007 | CreateMediaUploadCommandHandler — chỉ chủ bản dự toán chưa khóa được xin URL ảnh lô | Branch | P0 |
| [UT-PROJ-197](../unittest/UT-PROJ-197.md) | TDD-PROJ-007 | MediaUploadPayload.PrepareAsync(EstimateLotImage) — kích thước, điểm ảnh, tỷ lệ, định dạng thật | Boundary | P0 |
| [UT-PROJ-198](../unittest/UT-PROJ-198.md) | TDD-PROJ-007 | RequestLotVerificationCommandHandler — từ chối, trả yêu cầu đang soi, tạo yêu cầu mới | Error | P1 |
| [UT-PROJ-199](../unittest/UT-PROJ-199.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx B) — dùng lại kết quả theo tài khoản, hash, prompt, model | Branch | P0 |
| [UT-PROJ-200](../unittest/UT-PROJ-200.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx B, lỗi) — hạn mức 30 lần mỗi ngày và trả suất khi AI lỗi | Boundary | P0 |
| [UT-PROJ-201](../unittest/UT-PROJ-201.md) | TDD-PROJ-007 | LotVerificationVerdictMapper — ánh xạ PlotQuickCheck sang Đạt/Không đạt | Branch | P0 |
| [UT-PROJ-202](../unittest/UT-PROJ-202.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer — model soi không khả dụng thì thử một lần bằng model văn bản | Branch | P2 |
| [UT-PROJ-203](../unittest/UT-PROJ-203.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx C) — khách đã thay ảnh trong lúc soi | Quirk | P1 |
| [UT-PROJ-204](../unittest/UT-PROJ-204.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx C) — kết quả Đạt không tăng InputVersion và báo kết quả sau commit | Happy | P0 |
| [UT-PROJ-205](../unittest/UT-PROJ-205.md) | TDD-PROJ-006 | EstimateGenerationInputFactory (SchemaVersion 3) — nội dung bản chụp | Happy | P0 |
| [UT-PROJ-206](../unittest/UT-PROJ-206.md) | TDD-PROJ-007 | EstimateGenerationInputFactory (SchemaVersion 3) — mã phong cách hiệu lực khi tắt nhóm kiến trúc | Branch | P1 |
| [UT-PROJ-207](../unittest/UT-PROJ-207.md) | TDD-PROJ-007 | MediaTextExtractor — đọc URL ảnh từ bản chụp InputV3 | Happy | P1 |
| [UT-PROJ-208](../unittest/UT-PROJ-208.md) | TDD-PROJ-006 | Plan2dStageExecutor — thứ tự bước và ghi điểm lưu | Happy | P0 |
| [UT-PROJ-209](../unittest/UT-PROJ-209.md) | TDD-PROJ-006 | Plan2dStageExecutor — chạy tiếp bỏ qua bước đã xong | Branch | P0 |
| [UT-PROJ-210](../unittest/UT-PROJ-210.md) | TDD-PROJ-007 | Plan2dStageExecutor — khổ giấy chọn một lần, bản nội thất dùng khổ của bản kỹ thuật | Determinism | P1 |
| [UT-PROJ-211](../unittest/UT-PROJ-211.md) | TDD-PROJ-007 | Plan2dStageExecutor — ảnh tham chiếu theo tầng | Branch | P1 |
| [UT-PROJ-212](../unittest/UT-PROJ-212.md) |  | Plan2dStageExecutor — AI không trả ảnh: vẽ lại một lần rồi lỗi | Error | P0 |
| [UT-PROJ-213](../unittest/UT-PROJ-213.md) |  | Plan2dStageExecutor — viền nhà lệch: vẽ lại một lần, giữ bản tốt hơn | Branch | P0 |
| [UT-PROJ-214](../unittest/UT-PROJ-214.md) |  | Plan2dStageExecutor — cờ vẽ lại viền tắt thì chỉ ghi lại | Branch | P1 |
| [UT-PROJ-215](../unittest/UT-PROJ-215.md) | TDD-PROJ-007 | Plan2dStageExecutor — các trường hợp miễn kiểm viền | Quirk | P2 |
| [UT-PROJ-216](../unittest/UT-PROJ-216.md) | TDD-PROJ-008 | Plan2dStageExecutor — lỗi hệ thống AI và nội dung bị chặn | Error | P0 |
| [UT-PROJ-217](../unittest/UT-PROJ-217.md) | TDD-PROJ-007 | Plan2dStageExecutor — HouseSpec sai schema sau hai lần | Error | P1 |
| [UT-PROJ-218](../unittest/UT-PROJ-218.md) | TDD-PROJ-007 | Plan2dResultBuilder — tách phần gửi khách và phần nội bộ | Branch | P0 |
| [UT-PROJ-219](../unittest/UT-PROJ-219.md) |  | Plan2dStageExecutor — đọc quy tắc từ phiên bản đã chốt | Determinism | P1 |
| [UT-PROJ-220](../unittest/UT-PROJ-220.md) | TDD-PROJ-007 | Parity BriefReader — đọc mô tả tiếng Việt | Determinism | P0 |
| [UT-PROJ-221](../unittest/UT-PROJ-221.md) | TDD-PROJ-007 | Parity CustomerLot và luật lô đất | Determinism | P0 |
| [UT-PROJ-222](../unittest/UT-PROJ-222.md) | TDD-PROJ-007 | Parity SpecRuleEngine — 17 luật theo đúng thứ tự | Determinism | P0 |
| [UT-PROJ-223](../unittest/UT-PROJ-223.md) | TDD-PROJ-007 | Parity Plan2dPrompts (2d-13) — prompt trùng từng ký tự | Determinism | P0 |
| [UT-PROJ-224](../unittest/UT-PROJ-224.md) | TDD-PROJ-007 | Parity OutlineMeasurer — đo viền nhà trên ảnh | Determinism | P1 |
| [UT-PROJ-240](../unittest/UT-PROJ-240.md) | TDD-PROJ-008 | AiCallRunner — gọi thành công ngay lần đầu | Happy | P0 |
| [UT-PROJ-241](../unittest/UT-PROJ-241.md) | TDD-PROJ-008 | AiCallRunner — thử lại khi 503 với thời gian chờ 2 rồi 4 giây có ngẫu nhiên ±25% | Branch | P0 |
| [UT-PROJ-242](../unittest/UT-PROJ-242.md) | TDD-PROJ-008 | AiCallRunner — hết 4 lần thử khi lỗi 500 liên tục | Boundary | P1 |
| [UT-PROJ-243](../unittest/UT-PROJ-243.md) | TDD-PROJ-008 | AiCallRunner — dùng Retry-After của 429 thay cho lịch chờ, không cộng ngẫu nhiên | Branch | P1 |
| [UT-PROJ-244](../unittest/UT-PROJ-244.md) | TDD-PROJ-008 | AiCallRunner — dừng ngay khi thời gian còn lại không đủ cho lần chờ kế tiếp | Boundary | P0 |
| [UT-PROJ-245](../unittest/UT-PROJ-245.md) | TDD-PROJ-008 | AiCallRunner — phân loại mã 5xx: 501 không thử lại, 502 và 504 thử lại | Branch | P2 |
| [UT-PROJ-246](../unittest/UT-PROJ-246.md) | TDD-PROJ-008 | AiCallRunner — lỗi mạng và hết thời gian một lần gọi được thử lại | Branch | P1 |
| [UT-PROJ-247](../unittest/UT-PROJ-247.md) | TDD-PROJ-008 | AiCallRunner — 401 và 403 không thử lại, báo lỗi hệ thống | Error | P0 |
| [UT-PROJ-248](../unittest/UT-PROJ-248.md) | TDD-PROJ-008 | AiCallRunner — hết tiền theo HTTP 402 và mã insufficient_quota | Error | P0 |
| [UT-PROJ-249](../unittest/UT-PROJ-249.md) | TDD-PROJ-008 | AiCallRunner — nhận diện hết tiền theo mã lỗi trước, không bắt nhầm chữ billing | Quirk | P1 |
| [UT-PROJ-250](../unittest/UT-PROJ-250.md) | TDD-PROJ-008 | AiCallRunner — cảnh báo tài khoản AI tối đa một lần mỗi 10 phút mỗi nhà cung cấp | Determinism | P2 |
| [UT-PROJ-251](../unittest/UT-PROJ-251.md) | TDD-PROJ-008 | AiCallRunner — bỏ tham số ảnh không được hỗ trợ và nhớ theo model | Branch | P1 |
| [UT-PROJ-252](../unittest/UT-PROJ-252.md) | TDD-PROJ-008 | AiCallRunner — bỏ temperature, reasoning hoặc strict khi model từ chối | Branch | P2 |
| [UT-PROJ-253](../unittest/UT-PROJ-253.md) | TDD-PROJ-008 | AiCallRunner — không bao giờ bỏ size, quality hoặc background | Error | P1 |
| [UT-PROJ-254](../unittest/UT-PROJ-254.md) | TDD-PROJ-008 | AiCallRunner — không gửi temperature cùng reasoning | Quirk | P2 |
| [UT-PROJ-255](../unittest/UT-PROJ-255.md) | TDD-PROJ-008 | AiCallRunner — chỗ trong giới hạn chung không bị giữ trong lúc chờ thử lại | Determinism | P1 |
| [UT-PROJ-256](../unittest/UT-PROJ-256.md) | TDD-PROJ-008 | AiCallRunner — timeout một lần gọi bằng min(thời gian còn lại, timeout + 5 giây) | Boundary | P1 |
| [UT-PROJ-257](../unittest/UT-PROJ-257.md) | TDD-PROJ-008 | AiCallRunner — JSON sai được gọi lại một lần rồi mới báo AiOutputIncomplete | Error | P0 |
| [UT-PROJ-258](../unittest/UT-PROJ-258.md) | TDD-PROJ-008 | AiCallRunner — nội dung bị chặn không thử lại | Error | P1 |
| [UT-PROJ-259](../unittest/UT-PROJ-259.md) | TDD-PROJ-008 | AiCallRunner — ảnh 200 không có ảnh được gọi lại tối đa 2 lần với câu nhắc chỉ trả ảnh | Branch | P0 |
| [UT-PROJ-260](../unittest/UT-PROJ-260.md) | TDD-PROJ-008 | AiCallRunner — loại ảnh đọc từ byte trả về | Quirk | P2 |
| [UT-PROJ-261](../unittest/UT-PROJ-261.md) | TDD-PROJ-008 | OpenAiImagesClient — chọn edits hay generations và giới hạn 16 ảnh tham chiếu | Branch | P2 |
| [UT-PROJ-262](../unittest/UT-PROJ-262.md) | TDD-PROJ-008 | AnthropicMessagesClient — luồng bị đứt được thử lại, max_tokens và refusal không thử lại (đợt 2) | Branch | P2 |
| [UT-PROJ-263](../unittest/UT-PROJ-263.md) | TDD-PROJ-008 | Soi ảnh — model soi không khả dụng thì thử lại một lần bằng FallbackModel | Branch | P2 |
| [UT-PROJ-264](../unittest/UT-PROJ-264.md) | TDD-PROJ-008 | AiCallRecorder — một dòng nhật ký cho một lời gọi logic | Happy | P0 |
| [UT-PROJ-265](../unittest/UT-PROJ-265.md) | TDD-PROJ-008 | AiCallRecorder — công thức chi phí có token đệm và token ghi đệm | Boundary | P1 |
| [UT-PROJ-266](../unittest/UT-PROJ-266.md) | TDD-PROJ-008 | AiCallRecorder — model không có giá thì chi phí NULL | Branch | P2 |
| [UT-PROJ-267](../unittest/UT-PROJ-267.md) | TDD-PROJ-008 | AiCallRecorder — bản thô không có ảnh và không có khóa API | Quirk | P0 |
| [UT-PROJ-268](../unittest/UT-PROJ-268.md) | TDD-PROJ-008 | AiCallRecorder — ghi kết quả Error và Cancelled | Branch | P2 |
| [UT-PROJ-269](../unittest/UT-PROJ-269.md) | TDD-PROJ-008 | FakeAiProvider — phát lại theo task và stepKey, rơi về mặc định của task | Happy | P1 |
| [UT-PROJ-270](../unittest/UT-PROJ-270.md) | TDD-PROJ-008 | FakeAiProvider — lỗi theo kịch bản đi qua đúng logic của AiCallRunner | Error | P1 |
| [UT-PROJ-271](../unittest/UT-PROJ-271.md) | TDD-PROJ-008 | FakeAiProvider — model ghi là fake và chi phí bằng 0 | Quirk | P2 |
| [UT-PROJ-272](../unittest/UT-PROJ-272.md) | TDD-PROJ-008 | EstimatePipeline startup guard — từ chối cấu hình không hợp lệ | Error | P0 |
| [UT-PROJ-273](../unittest/UT-PROJ-273.md) | TDD-PROJ-008 | PermissionNames — có năm quyền mới và được gán cho Admin | Happy | P1 |
| [UT-PROJ-274](../unittest/UT-PROJ-274.md) | TDD-PROJ-008 | SavePipelineConfigCommandHandler — lưu tạo phiên bản mới, chép toàn bộ dòng con | Happy | P0 |
| [UT-PROJ-275](../unittest/UT-PROJ-275.md) | TDD-PROJ-008 | SavePipelineConfigCommandHandler — expectedVersion cũ bị từ chối | Error | P0 |
| [UT-PROJ-276](../unittest/UT-PROJ-276.md) | TDD-PROJ-008 | SavePipelineConfigCommandHandler — gửi lặp cùng key trả phiên bản cũ, khác nội dung thì 409 | Determinism | P1 |
| [UT-PROJ-277](../unittest/UT-PROJ-277.md) | TDD-PROJ-008 | PipelineConfigValidator — model ngoài danh sách hỗ trợ | Error | P0 |
| [UT-PROJ-278](../unittest/UT-PROJ-278.md) | TDD-PROJ-008 | PipelineConfigValidator — tham số phải khớp cờ khả năng của model | Branch | P1 |
| [UT-PROJ-279](../unittest/UT-PROJ-279.md) | TDD-PROJ-008 | PipelineConfigValidator — biên của các số cấu hình | Boundary | P1 |
| [UT-PROJ-280](../unittest/UT-PROJ-280.md) | TDD-PROJ-008 | IPipelineConfigReader — lần chạy đọc đúng phiên bản đã chốt | Determinism | P0 |
| [UT-PROJ-281](../unittest/UT-PROJ-281.md) | TDD-PROJ-008 | PromptAssembler — ghép khung mã với chỗ chèn nạp sẵn trùng nguyên văn app demo | Happy | P0 |
| [UT-PROJ-282](../unittest/UT-PROJ-282.md) | TDD-PROJ-008 | PromptAssembler — chọn lời dặn chung theo phạm vi, thứ tự và trạng thái | Branch | P1 |
| [UT-PROJ-283](../unittest/UT-PROJ-283.md) | TDD-PROJ-008 | AiRuleSetValidator — lời dặn có nội dung, tối đa 2.000 ký tự | Boundary | P2 |
| [UT-PROJ-284](../unittest/UT-PROJ-284.md) | TDD-PROJ-008 | IAiRuleSetReader — lần chạy đọc đúng phiên bản quy tắc đã chốt | Determinism | P0 |
| [UT-PROJ-285](../unittest/UT-PROJ-285.md) | TDD-PROJ-008 | IAiRuleSetReader — nạp đủ quy tắc theo mã gốc như app demo | Happy | P1 |
| [UT-PROJ-286](../unittest/UT-PROJ-286.md) | TDD-PROJ-008 | SavePricingRegionsCommandHandler — lưu tăng TableVersion, rowVersion cũ bị từ chối | Branch | P1 |
| [UT-PROJ-287](../unittest/UT-PROJ-287.md) | TDD-PROJ-008 | PricingRegionValidator — tỉnh không tồn tại, thiếu miền, miền không hợp lệ | Error | P1 |

## Mốc cập nhật ngày 12/10/2026: sửa theo bản rà soát TDD và câu trả lời của người dùng

Người dùng chốt phần sửa TDD-PROJ-006/007/008 theo bản rà soát độc lập, rồi trả lời thêm 11 điểm (lỗi hệ thống không tính giới hạn, hàng chờ đầy, tạm ngắt dịch vụ AI, cảnh báo chi phí, trần soi chung, tệp riêng tư, quyền vận hành, ghi vết đọc nhật ký thô…). Đặc tả Unit Test được cập nhật như sau.

**Sửa ca đã có** (giữ mã):
- UT-PROJ-140, 144, 151, 153, 155: chỉ lỗi do kết quả AI mới có `CountsTowardLimit=true`; quá hạn và lỗi hệ thống luôn false.
- UT-PROJ-162: người đặt lại bộ đếm dùng quyền `estimate.pipeline.operate`.
- UT-PROJ-168, 173: quét quá hạn và cảnh báo chuyển sang job `EstimatePipelineMaintenanceJob`, chạy cả khi `Mode=Off`.
- UT-PROJ-198 → 204 (trừ 201): soi ảnh bất đồng bộ. 198 là handler gửi yêu cầu; 199, 200, 202, 203, 204 là `LotVerificationRequestedConsumer` theo ba transaction ngắn.
- UT-PROJ-255: chỗ trong giới hạn chung qua Redis thay cho semaphore theo instance.
- UT-PROJ-264: token và chi phí cộng mọi lần thử đã bị tính tiền.
- UT-PROJ-267: khóa bản thô dưới `media/site-private/`.
- UT-PROJ-273: năm quyền, thêm `estimate.pipeline.operate`.
- UT-PROJ-274, 279: các cột cấu hình mới và biên của chúng.

**Thêm 29 ca mới**, dùng hết các mã trống 174–179, 225–239, 288–289 và thêm 290–295. Mã 290–295 thuộc TDD-PROJ-006 vì dải 174–179 không đủ.

| Test | TDD | Unit | Loại | Ưu tiên |
|---|---|---|---|---|
| [UT-PROJ-174](../unittest/UT-PROJ-174.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Hàng chờ đầy | Boundary | P0 |
| [UT-PROJ-175](../unittest/UT-PROJ-175.md) | TDD-PROJ-006 | RunEstimateStageCommandHandler — Nhà cung cấp AI đang tạm ngắt | Branch | P0 |
| [UT-PROJ-176](../unittest/UT-PROJ-176.md) | TDD-PROJ-006 | StageFailureClassifier — phân nhóm mã lỗi lần chạy | Branch | P0 |
| [UT-PROJ-177](../unittest/UT-PROJ-177.md) | TDD-PROJ-006 | FinalizeStageRunCommandHandler — đặt CountsTowardLimit theo nhóm lỗi | Branch | P0 |
| [UT-PROJ-178](../unittest/UT-PROJ-178.md) | TDD-PROJ-006 | StageRunQueuedConsumer — chỉ đánh thức worker, không chạm DB | Determinism | P0 |
| [UT-PROJ-179](../unittest/UT-PROJ-179.md) | TDD-PROJ-006 | EstimateStageWorker — nhận theo tín hiệu và nhận việc kế tiếp theo thứ tự tiếp nhận | Branch | P1 |
| [UT-PROJ-290](../unittest/UT-PROJ-290.md) | TDD-PROJ-006 | EstimateStageWorker — vòng quét dự phòng khi không có tin | Branch | P1 |
| [UT-PROJ-291](../unittest/UT-PROJ-291.md) | TDD-PROJ-006 | EstimateStageWorker — dừng êm khi ứng dụng tắt | Branch | P1 |
| [UT-PROJ-292](../unittest/UT-PROJ-292.md) | TDD-PROJ-006 | StageRunFinishedConsumer — báo trình duyệt và cảnh báo chi phí | Branch | P1 |
| [UT-PROJ-293](../unittest/UT-PROJ-293.md) | TDD-PROJ-006 | GetStageFileQueryHandler — link tải riêng tư có hạn 15 phút | Branch | P0 |
| [UT-PROJ-294](../unittest/UT-PROJ-294.md) | TDD-PROJ-006 | EstimatePipelineMaintenanceJob — đối soát chặng 2D với sổ lượt | Branch | P2 |
| [UT-PROJ-295](../unittest/UT-PROJ-295.md) | TDD-PROJ-006 | StageArtifactLookup — dùng lại phần đã xong dù cấu hình đổi | Branch | P1 |
| [UT-PROJ-225](../unittest/UT-PROJ-225.md) | TDD-PROJ-007 | RequestLotVerificationCommandHandler — tạm ngắt và trần soi chung | Branch | P0 |
| [UT-PROJ-226](../unittest/UT-PROJ-226.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx A) — nhận việc có điều kiện | Branch | P0 |
| [UT-PROJ-227](../unittest/UT-PROJ-227.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer — không giữ transaction khi tải ảnh và gọi AI | Determinism | P0 |
| [UT-PROJ-228](../unittest/UT-PROJ-228.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx B) — trần soi chung đầy và cảnh báo 80% | Boundary | P0 |
| [UT-PROJ-229](../unittest/UT-PROJ-229.md) | TDD-PROJ-007 | LotVerificationRequestedConsumer (Tx C) — yêu cầu đã bị job bảo trì chốt | Quirk | P1 |
| [UT-PROJ-230](../unittest/UT-PROJ-230.md) | TDD-PROJ-007 | EstimatePipelineMaintenanceJob — yêu cầu soi quá hạn | Branch | P1 |
| [UT-PROJ-231](../unittest/UT-PROJ-231.md) | TDD-PROJ-007 | LotVerificationCompletedConsumer — báo kết quả soi | Happy | P2 |
| [UT-PROJ-232](../unittest/UT-PROJ-232.md) | TDD-PROJ-007 | Cấu hình endpoint `lot-verification-requested` của MassTransit | Determinism | P1 |
| [UT-PROJ-233](../unittest/UT-PROJ-233.md) | TDD-PROJ-008 | AiConcurrencyLimiter — Redis lỗi thì dùng giới hạn trong tiến trình | Error | P1 |
| [UT-PROJ-234](../unittest/UT-PROJ-234.md) | TDD-PROJ-008 | AiCircuitBreaker — ngắt sau đủ lỗi liên tiếp | Boundary | P0 |
| [UT-PROJ-235](../unittest/UT-PROJ-235.md) | TDD-PROJ-008 | AiCircuitBreaker — hết tiền ngắt ngay, mỗi lần ngắt một cảnh báo | Branch | P0 |
| [UT-PROJ-236](../unittest/UT-PROJ-236.md) | TDD-PROJ-008 | AiCircuitBreaker — mở lại và Redis lỗi | Branch | P1 |
| [UT-PROJ-237](../unittest/UT-PROJ-237.md) | TDD-PROJ-008 | GetAiCallRawQueryHandler — đọc bản thô có ghi vết | Branch | P0 |
| [UT-PROJ-238](../unittest/UT-PROJ-238.md) | TDD-PROJ-008 | Chính sách quyền của API vận hành luồng AI | Branch | P0 |
| [UT-PROJ-239](../unittest/UT-PROJ-239.md) | TDD-PROJ-008 | Đăng ký nhà cung cấp AI giả theo Mode | Branch | P1 |
| [UT-PROJ-288](../unittest/UT-PROJ-288.md) | TDD-PROJ-008 | AiConcurrencyLimiter — giữ chỗ chung qua Redis | Boundary | P0 |
| [UT-PROJ-289](../unittest/UT-PROJ-289.md) | TDD-PROJ-008 | AiConcurrencyLimiter — chỗ tự hết hạn khi instance chết | Quirk | P1 |

Tất cả vẫn là unit dự kiến, chưa có mã và chưa chạy. Script kiểm tra cho kết quả:
- 249 file UT/ST của luồng bốn chặng đều có một dòng 13 cột;
- Trace to khớp TEST_LINKS;
- mọi mã và mục được dẫn đều có thật.

Phần chỉ chứng minh được trên hạ tầng thật vẫn để cho integration test:
- không có kết nối `idle in transaction` quá 1 giây khi 10 lần chạy và 20 yêu cầu soi chạy cùng lúc;
- upsert đồng thời bộ đếm soi chung;
- script Lua của limiter và breaker trên Redis thật;
- endpoint SignalR qua nhiều instance.

## Mốc cập nhật ngày 12/10/2026: chặng Dự toán, đợt 2

Người dùng chốt TDD-PROJ-009 ngày 12/10/2026 bằng lựa chọn "Chốt TDD, viết Unit Test (Đề xuất)", sau khi TDD được rà lại theo đúng app demo `design-estimate` ở commit `059e876`. Đây là xác nhận thiết kế trong hội thoại, không phải phê duyệt trên hệ thống quản lý tài liệu; Status của TDD vẫn là Draft.

### Mốc TDD được chốt

Hash dưới đây ghi nhận nội dung TDD làm căn cứ cho 133 ca của đợt 2, chỉ dùng để đối chiếu nội dung. TDD-PROJ-006, 007 là phần dùng chung đã chốt ở đợt 1; các ca đợt 2 chỉ dẫn tới chúng, không sửa chúng.

**Cập nhật mốc chốt, cùng ngày 12/10/2026.** Sau khi viết Unit Test, TDD-PROJ-009 và TDD-PROJ-008 được sửa để hết mâu thuẫn và chốt các điểm còn mở của bản bàn giao đầu. Căn cứ là nguyên tắc "bám sát app demo" người dùng đã đặt: điểm nào app demo có cách làm tương ứng thì làm theo app demo, điểm còn lại theo thiết kế sẵn có của BMT (TDD-PROJ-009 mục R, quyết định 8). Đây là quyết định kỹ thuật, chưa có xác nhận riêng của người dùng cho bản sửa. Những gì thay đổi:
- Chỗ sửa 1 của văn bản luật giá nạp sẵn chỉ bỏ cụm "Tây Nguyên / ", giữ phần tỉnh lẻ, khớp BR-PROJ-016 Notes (mục O).
- Bản gửi khách dùng ngưỡng 8 ký tự cho mọi ô như `fill.py`; 25 ký tự chỉ là mặc định khi gọi thẳng hàm lọc (mục G).
- Anthropic chờ thử lại đúng 2 → 4 → 8 giây, không cộng ngẫu nhiên; ±25% chỉ áp dụng cho OpenAI (TDD-PROJ-009 mục C, TDD-PROJ-008 mục B). Quy tắc gọi lại một lần khi JSON sai chỉ thuộc lời gọi OpenAI theo JSON schema (TDD-PROJ-008 mục B).
- Mã lỗi cuối khi có lượt hỏi lại lỗi: lượt hỏi lại không tự đặt mã lỗi; nhiều lượt lỗi hệ thống thì lấy lượt đầu tiên; lượt hỏi lại bị từ chối nội dung tính như thiếu kết quả (mục E).
- Thêm các chi tiết còn thiếu: khóa `prompt_fingerprint` (mục B); `vung_re` luôn `false` (mục F); làm tròn `pricePerM2Vnd` nửa xa số 0 như ô D22 của `template.xlsx` (mục G); thông báo lỗi 422, cảnh báo xem trước, khôi phục cả giá neo và hệ số gói của bản cũ, phân trang theo `PagedResult` (mục H); danh sách 9 chỗ chèn (mục I); câu cảnh báo `EstimateNeedsEngineerReview` (mục M); `MaxOutputTokens` và khoảng hợp lệ của bốn tham số giới hạn đầu ra (mục C, O).
- STORY-PROJ-011 References/TDDs thêm TDD-PROJ-009.

16 ca được sửa theo: UT-PROJ-299, 312, 313, 321, 362, 364, 384, 392, 394, 395, 396, 403, 408, 413, 416, 419. Số ca, mã ca và phân bổ theo mục không đổi.

**Cập nhật lần hai, cùng ngày 12/10/2026.** Ba điểm còn mở sau lần cập nhật trên đã được chốt:
- **Người dùng chốt** bằng câu trả lời "Không tính, lỗi hệ thống (Đề xuất)": lượt chính thiếu kết quả, lượt hỏi lại lỗi vì hệ thống (bị cắt, dịch vụ lỗi, hết tiền, `AiRequestInvalid`), rồi kiểm trường bắt buộc vẫn thất bại thì lần chạy lỗi không tính vào giới hạn lỗi; mã lỗi là mã hệ thống của lượt hỏi lại lỗi đầu tiên theo thứ tự chạy. Lượt hỏi lại bị từ chối nội dung hoặc không trả `tool_use` thì lần chạy lỗi `AiOutputIncomplete` có tính. Ghi ở TDD-PROJ-009 mục R, quyết định 9; UT-PROJ-362 khẳng định đúng các nhánh này.
- **Quyết định kỹ thuật:** giữ câu lỗi 422 "Không đọc được khoảng giá của dòng '{tên dòng}' ở mục {N}." cho ô giá không có chữ số; ô ghi "0" vẫn hợp lệ (UT-PROJ-392).
- **Quyết định kỹ thuật:** lưu cả hai tài liệu luật giá đều trả `warnings` không chặn khi thiếu tiêu đề mục mà bộ rút gọn cần, cùng câu như xem trước. Quy tắc bóc tách kiểm tiêu đề mục 2, 5, 6, 9; Khung giá kiểm mục 2, 7, 8, 9, 10 (UT-PROJ-389, 393, 394).

Tính cả hai lần cập nhật, 18 ca đã sửa: UT-PROJ-299, 312, 313, 321, 362, 364, 384, 389, 392, 393, 394, 395, 396, 403, 408, 413, 416, 419. TDD-PROJ-008 không đổi ở lần hai.

| TDD | Bản | SHA-256 |
|---|---|---|
| [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Bản người dùng chốt bằng lựa chọn "Chốt TDD, viết Unit Test (Đề xuất)" | `5d7afb5fdfee169e34c637ff036cd2fedf3c4ff1a5952f2c0fd92d2af9322251` |
| [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Bản cập nhật lần một cùng ngày | `078924b9e1fd851d518f127dfaf998f61fe3770985fc8e42b466786674e28aec` |
| [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Bản cập nhật lần hai cùng ngày (quyết định 9 của người dùng), căn cứ hiện hành của 133 ca | `4a0a22165746b7007d6f67abb5874159a2af312e5c975a3503eabe7eb51ed0dc` |
| [TDD-PROJ-008](../tdd/TDD-PROJ-008.md) | Bản cập nhật cùng ngày (lịch chờ theo nhà cung cấp, `MaxOutputTokens`) | `3985dd8bd1fc1bb0bc80ba4c0baa24063aa16a343de53efa84522298159a31c7` |

### Danh sách 133 ca

Đã soạn 133 đặc tả mới, UT-PROJ-296 → 428: 125 unit test và 8 test tích hợp trên PostgreSQL thật (ràng buộc database, khóa đồng thời, migration). Tất cả là Draft, chưa có mã và chưa chạy. Reviewer/Approver: Tân Trần; Owner chưa xác định. Một file chứa một test và một dòng bảng; các biến thể của cùng hành vi nằm trong Input.

Số ca theo mục của TDD-PROJ-009:
- A. Tiếp nhận và chốt phiên bản: 3 ca (UT-PROJ-296 → 298)
- B. Executor, thứ tự xử lý và điểm lưu: 11 ca (UT-PROJ-299 → 309)
- C. Lời gọi Anthropic: 12 ca (UT-PROJ-310 → 321)
- D. Dựng đầu vào cho AI: 21 ca (UT-PROJ-322 → 342)
- E. Lưới an toàn, hỏi lại và phân loại lỗi: 20 ca (UT-PROJ-343 → 362)
- F. Giá neo, dải giá và làm tròn: 17 ca (UT-PROJ-363 → 379)
- G. Phần gửi khách và phần nội bộ: 9 ca (UT-PROJ-380 → 388)
- H. Luật giá có phiên bản: 12 ca (UT-PROJ-389 → 400)
- I. Bộ quy tắc AI bổ sung: 3 ca (UT-PROJ-401 → 403)
- J. Excel dự toán: 2 ca (UT-PROJ-404 → 405)
- K. Đối chiếu với app demo: 4 ca (UT-PROJ-406 → 409)
- L. Bảo mật và quyền: 3 ca (UT-PROJ-410 → 412)
- M. Quan sát và cảnh báo: 2 ca (UT-PROJ-413 → 414)
- O. Cấu hình, triển khai và migration: 4 ca (UT-PROJ-415 → 418)
- P. Hợp đồng màn hình khách: 1 ca (UT-PROJ-419)
- Internal API — mã lỗi: 1 ca (UT-PROJ-420)
- Data Model — test tích hợp PostgreSQL: 8 ca (UT-PROJ-421 → 428)

| Ca | Unit / hành vi | TDD | System Test liên quan |
|---|---|---|---|
| [UT-PROJ-296](../unittest/UT-PROJ-296.md) | RunEstimateStageCommandHandler (chặng Estimate) — chốt hai phiên bản luật giá, không kiểm gói và lượt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-140](../systemtest/ST-PROJ-140.md) |
| [UT-PROJ-297](../unittest/UT-PROJ-297.md) | RunEstimateStageCommandHandler (chặng Estimate) — chạy lại sau lỗi chốt phiên bản đang áp dụng lúc chạy lại | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-145](../systemtest/ST-PROJ-145.md) |
| [UT-PROJ-298](../unittest/UT-PROJ-298.md) | RunEstimateStageCommandHandler (chặng Estimate) — bước 7a kiểm tạm ngắt theo nhà cung cấp của việc estimate | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-199](../systemtest/ST-PROJ-199.md), [ST-PROJ-147](../systemtest/ST-PROJ-147.md) |
| [UT-PROJ-299](../unittest/UT-PROJ-299.md) | EstimateStageExecutor.ExecuteAsync — đọc đầu vào đã chốt, ghi bước context và tiến độ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-140](../systemtest/ST-PROJ-140.md) |
| [UT-PROJ-300](../unittest/UT-PROJ-300.md) | EstimateStageExecutor.ExecuteAsync — thiếu kết quả 2D, thiếu phiên bản luật giá, lỗi đọc kho ảnh | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-301](../unittest/UT-PROJ-301.md) | EstimateRunner.RunAsync — gọi các lượt AI và các lưới theo đúng thứ tự của run_estimate_pipeline | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-302](../unittest/UT-PROJ-302.md) | CheckpointedEstimateTurns.AskAsync — bước đã có thì phát lại, chưa có thì gọi AI rồi ghi bước | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-303](../unittest/UT-PROJ-303.md) | CheckpointedEstimateTurns.AskAsync — lượt hỏi lại lỗi được ghi thành bước đã xong; lượt chính lỗi không ghi bước | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-304](../unittest/UT-PROJ-304.md) | EstimateStageExecutor — nhận lại sau khi worker chết chỉ gọi lượt chưa có kết quả | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-305](../unittest/UT-PROJ-305.md) | EstimateRunner — khóa bước của lưới hai khuôn giá theo lần chạy lưới | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-306](../unittest/UT-PROJ-306.md) | PriceSplitNet — sửa ngay trên dòng của kết quả AI, lượt rà giá gửi bản đã sửa | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-307](../unittest/UT-PROJ-307.md) | EstimateComposer.Compose — ngày lập theo ngày tiếp nhận giờ Việt Nam, hạn báo giá 90 ngày | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-308](../unittest/UT-PROJ-308.md) | EstimateStageExecutor — ghi EstimateCostResult có điều kiện lease, ghi bước result rồi chốt Succeeded | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-309](../unittest/UT-PROJ-309.md) | EstimateResultRecord — cột có kiểu khớp phần JSON và hai cờ cần rà | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-310](../unittest/UT-PROJ-310.md) | AnthropicMessagesClient — thân yêu cầu Messages streaming và ghép tool input từ SSE | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-311](../unittest/UT-PROJ-311.md) | AiCallRunner.CallToolAsync — phân loại phản hồi 200 theo stop_reason và khối tool_use | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-312](../unittest/UT-PROJ-312.md) | AiCallRunner.CallToolAsync — lỗi tạm thời của Anthropic được thử lại, tối đa 4 lần gửi | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-313](../unittest/UT-PROJ-313.md) | AiCallRunner.CallToolAsync — chờ theo retry-after-ms rồi retry-after khi Anthropic trả 429 | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-314](../unittest/UT-PROJ-314.md) | AnthropicMessagesClient — sự kiện error giữa luồng đổi sang mã HTTP theo error.type | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-315](../unittest/UT-PROJ-315.md) | AiCallRunner.CallToolAsync — hết tiền, sai khóa và bị từ chối quyền với Anthropic | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-147](../systemtest/ST-PROJ-147.md) |
| [UT-PROJ-316](../unittest/UT-PROJ-316.md) | AiCallRunner.CallToolAsync — 400 có chữ "cache" khi đang gắn bộ nhớ đệm prompt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-317](../unittest/UT-PROJ-317.md) | AiCallRunner — hạn một lần gọi Anthropic theo max_tokens và thời hạn chỗ giữ của nhóm anthropic | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-318](../unittest/UT-PROJ-318.md) | Bộ nhớ đệm prompt của việc estimate — mặc định tắt; khi bật chỉ gắn ở lượt chính và lượt rà giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-319](../unittest/UT-PROJ-319.md) | AiCallRecorder — token và chi phí của lượt Anthropic có đọc và ghi bộ nhớ đệm | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-320](../unittest/UT-PROJ-320.md) | AiCircuitBreaker — lời gọi dò Anthropic khi khóa ngắt do hết tiền hoặc sai khóa vừa hết hạn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-321](../unittest/UT-PROJ-321.md) | PipelineConfigValidator và AnthropicMessagesClient — model, temperature và khoảng hợp lệ của tham số việc estimate | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-322](../unittest/UT-PROJ-322.md) | EstimateInputs.Build — mã gốc, nhãn danh mục của app demo và mã gói | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-323](../unittest/UT-PROJ-323.md) | EstimateInputs.Build — số tầng và tum lấy từ bản chụp trước, HouseSpec sau; căn hộ luôn 1 tầng | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-324](../unittest/UT-PROJ-324.md) | EstimateInputs.Build và EstimatePrompts — yêu cầu thêm của khách và cách bọc dữ liệu khách | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-325](../unittest/UT-PROJ-325.md) | EstimateInputs.Build — vùng miền lấy từ miền giá đã chốt lúc 2D, không dò địa chỉ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-326](../unittest/UT-PROJ-326.md) | EstimateInputs.Build và EstimatePrompts — không gửi tên khách cho AI | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-327](../unittest/UT-PROJ-327.md) | EstimateInputs.Build — số liệu lấy từ HouseSpec: diện tích xây dựng, lô, số phòng ngủ, số WC, tổng sàn tham chiếu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-328](../unittest/UT-PROJ-328.md) | PriceTypeResolver.Resolve — sáu loại neo giá theo mã gốc và mã phong cách | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-329](../unittest/UT-PROJ-329.md) | RoofResolver — kiểu mái mặc định tra ma trận mái của bộ quy tắc đã chốt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-330](../unittest/UT-PROJ-330.md) | RoofResolver — mái do phương án nêu: trùng kiểu mặc định thì dùng dòng mặc định | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-331](../unittest/UT-PROJ-331.md) | QuantityTakeoff.RoofFromSpec — đọc mái từ phần chữ của phương án | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-332](../unittest/UT-PROJ-332.md) | QuantityTakeoff.BaseQuantities — khối lượng cơ sở theo tầng và toàn nhà | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-333](../unittest/UT-PROJ-333.md) | QuantityTakeoff — lô không vuông vức đo theo đường bao thật | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-334](../unittest/UT-PROJ-334.md) | EstimatePrompts.TailorRules — rút gọn Khung giá tham khảo theo dự án | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-335](../unittest/UT-PROJ-335.md) | EstimatePrompts.TailorRules — rút gọn Quy tắc bóc tách; không có dự án thì giữ nguyên văn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-336](../unittest/UT-PROJ-336.md) | EstimatePrompts.TailorRules — chọn cột và dòng theo miền đã chốt, không có khóa Tây Nguyên | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-337](../unittest/UT-PROJ-337.md) | EstimatePrompts — ghép prompt từ khung trong mã và chỗ chèn AiPromptPart nạp sẵn trùng app demo | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-338](../unittest/UT-PROJ-338.md) | EstimateImages.Collect — mỗi tầng một ảnh nội thất, ảnh kỹ thuật chỉ thay khi thiếu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-339](../unittest/UT-PROJ-339.md) | EstimateImages.Fit — trần base64 mỗi ảnh và cách nén ảnh vượt trần | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-340](../unittest/UT-PROJ-340.md) | EstimateImages.Arrange — tối đa 9 ảnh, nhãn "Ảnh i/N", lượt chạy không có ảnh | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-341](../unittest/UT-PROJ-341.md) | EstimateTools — schema của bốn công cụ và biến thể theo dự án | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-342](../unittest/UT-PROJ-342.md) | EstimatePrompts.Fingerprint và hash phương án — ổn định và đổi theo nội dung | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-343](../unittest/UT-PROJ-343.md) | ShapeNormalizer.RecoverParts — khôi phục các phần bóc tách nằm sai chỗ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-344](../unittest/UT-PROJ-344.md) | ShapeNormalizer — không ghi đè phần đã có dòng và không chuyển nhóm nhầm vào phần đã có | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-345](../unittest/UT-PROJ-345.md) | ShapeNormalizer.NormalizeAiShape — gom trường phân tích bị trải phẳng, đọc phan_tich là chuỗi JSON | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-346](../unittest/UT-PROJ-346.md) | EstimateMerge.MergeAiResults — chỉ ghi đè phần có dòng, ghép phân tích theo từng trường | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-347](../unittest/UT-PROJ-347.md) | TakeoffNet — điều kiện hỏi lại, công cụ chỉ khai báo phần thiếu, giới hạn token, chỉ ghép phần được hỏi | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-348](../unittest/UT-PROJ-348.md) | TakeoffNet — không hỏi lại, vẫn thiếu sau lượt hỏi lại, lượt hỏi lại lỗi | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-349](../unittest/UT-PROJ-349.md) | RoughPartGuards.Regroup — chia lại nhóm phần thô khi có từ 3 dòng sai nhóm | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-350](../unittest/UT-PROJ-350.md) | RoughPartGuards.DropNewBuildRoughLines — căn hộ bỏ dòng xây mới khỏi phần thô | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-351](../unittest/UT-PROJ-351.md) | AnalysisNet — điều kiện hỏi lại, 4.000 token, chỉ ghép trường thiếu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-352](../unittest/UT-PROJ-352.md) | PriceSplitNet.SplitByGroupCode — tách VT/NC bằng 24 tỷ lệ theo mã nhóm vật tư | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-353](../unittest/UT-PROJ-353.md) | PriceSplitNet.Run — phần hỏng, hỏi lại tách giá, ràng buộc ±1.000 đ và tách theo tỷ lệ tên nhóm | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-354](../unittest/UT-PROJ-354.md) | PriceSplitNet.SplitByRatio — làm tròn khi tách VT/NC theo tỷ lệ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-355](../unittest/UT-PROJ-355.md) | EstimateNormalizer.Normalize — loại dòng 0 đồng, làm tròn khối lượng, đơn vị đếm | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-356](../unittest/UT-PROJ-356.md) | QuantityTakeoff.CheckQuantities — kiểm khối lượng theo hình học, chỉ ghi nội bộ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-357](../unittest/UT-PROJ-357.md) | QuantityTakeoff.CheckMinRows — số dòng tối thiểu theo loại công trình | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-358](../unittest/UT-PROJ-358.md) | EstimateValidator.Validate — 24 ô bắt buộc, tong_san_so là số dương, đủ ba phần | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-144](../systemtest/ST-PROJ-144.md) |
| [UT-PROJ-359](../unittest/UT-PROJ-359.md) | MaterialBrands.Summarize — đếm dòng thiếu hãng và ngưỡng cần nhân viên rà | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-360](../unittest/UT-PROJ-360.md) | EstimateRunner — lượt hỏi lại lỗi vì bất kỳ lý do gì được bỏ qua, lần chạy đi tiếp | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-361](../unittest/UT-PROJ-361.md) | EstimateStageExecutor — mã lỗi cuối khi lượt chính lỗi | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-147](../systemtest/ST-PROJ-147.md) |
| [UT-PROJ-362](../unittest/UT-PROJ-362.md) | EstimateStageExecutor — mã lỗi cuối khi kiểm trường bắt buộc thất bại | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-144](../systemtest/ST-PROJ-144.md), [ST-PROJ-201](../systemtest/ST-PROJ-201.md) |
| [UT-PROJ-363](../unittest/UT-PROJ-363.md) | PriceAnchors — sàn giá, chỉ tiêu từng phần và trần của 18 ô loại neo giá × gói | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-364](../unittest/UT-PROJ-364.md) | PriceAnchors.CheckFloor — công thức theo loại, ngưỡng 0,95 sàn, trần và trường hợp không đủ dữ liệu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-365](../unittest/UT-PROJ-365.md) | PriceAnchors.Allocate — một vị trí chung trên dải, phần cuối hấp thụ phần làm tròn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-366](../unittest/UT-PROJ-366.md) | PriceAnchors.Targets — chỉ tiêu dự toán của nhà phố hiện đại ba gói | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-367](../unittest/UT-PROJ-367.md) | PriceAnchors.Targets — chỉ tiêu của mọi ô cộng lại bằng neo và nằm cùng một vị trí trên dải | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-368](../unittest/UT-PROJ-368.md) | PriceFloorNet — giá trong khoảng: trên đầu cao dải thì cần kỹ sư rà, trên dải × 1,8 và trên sàn × 1,05 thì nghi bóc trùng | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-143](../systemtest/ST-PROJ-143.md) |
| [UT-PROJ-369](../unittest/UT-PROJ-369.md) | PriceFloorNet — câu lưu ý chỉ nói đã đạt sàn khi số đã đạt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-370](../unittest/UT-PROJ-370.md) | PriceFloorNet và EstimateRunner — trên trần hỏi lại AI đúng một lần, dưới sàn không hỏi lại | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-371](../unittest/UT-PROJ-371.md) | EstimateRunner — ghép lại câu trả lời của lượt rà giá như lượt chính | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-372](../unittest/UT-PROJ-372.md) | PriceFloorNet — lượt rà giá làm giá giảm được ghi lại và luôn cần kỹ sư rà | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-373](../unittest/UT-PROJ-373.md) | PriceFloorNet.RaisePrices — tự nâng đơn giá tới 100% neo, giữ nguyên khối lượng | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-142](../systemtest/ST-PROJ-142.md) |
| [UT-PROJ-374](../unittest/UT-PROJ-374.md) | PriceFloorNet.RaisePrices — giới hạn ×6 tổng, ×3 mỗi dòng và dòng vật liệu cơ bản | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-375](../unittest/UT-PROJ-375.md) | PriceFloorNet.RaisePrices — thiếu do đơn giá hay do khối lượng; không có dòng nào nâng được | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-376](../unittest/UT-PROJ-376.md) | PriceFloorNet — không bao giờ hạ giá và không đổi khối lượng trên mọi ô | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-377](../unittest/UT-PROJ-377.md) | Common/JsMath và EstimateTotals — các quy tắc làm tròn trộn JavaScript và Python | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-378](../unittest/UT-PROJ-378.md) | EstimateNormalizer.ParseNumber (ep_so) — đọc số AI trả theo quy ước dấu phẩy Việt Nam | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-379](../unittest/UT-PROJ-379.md) | StaffReviewBuilder — ghi chú cho kỹ sư theo từng nhánh của lưới sàn giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-380](../unittest/UT-PROJ-380.md) | CustomerCopy.Reasons — nội dung nội bộ và câu viết lại không dùng từ cấm đều bị từ chối | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-143](../systemtest/ST-PROJ-143.md) |
| [UT-PROJ-381](../unittest/UT-PROJ-381.md) | CustomerCopy — câu trung thực và phép tính bóc tách được giữ nguyên từng byte | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-382](../unittest/UT-PROJ-382.md) | CustomerCopy.Reasons — luật tiền chạy trên chữ có dấu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-383](../unittest/UT-PROJ-383.md) | CustomerCopy.StripInternal — cắt nhỏ nhất theo thứ tự ngoặc, mệnh đề, vế dấu phẩy, cả câu | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-384](../unittest/UT-PROJ-384.md) | CustomerCopy.AdmitCell — ngưỡng 8 ký tự cho mọi ô gửi khách, 25 chỉ khi gọi thẳng hàm lọc, bỏ mảnh vụn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-385](../unittest/UT-PROJ-385.md) | CustomerCopy.Build — thứ tự 11 ô gửi khách và nội dung soạn lại khi phải bỏ cả ô | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-386](../unittest/UT-PROJ-386.md) | CustomerCopy.Build — nhận xét dự toán luôn do hệ thống soạn, đoạn của AI chỉ được nối khi sạch cả đoạn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-387](../unittest/UT-PROJ-387.md) | CustomerCopy.Build — bỏ ghi_chu_noi_bo theo tên, khối phân tích khớp ô in ra, báo cáo các ô đã cắt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-388](../unittest/UT-PROJ-388.md) | CustomerCopy.CleanForExcel — bỏ ký tự điều khiển trước khi dựng bản gửi khách | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-389](../unittest/UT-PROJ-389.md) | SavePriceRuleDocumentCommandHandler — lưu Khung giá tạo phiên bản mới và tính lại dải giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-183](../systemtest/ST-PROJ-183.md) |
| [UT-PROJ-390](../unittest/UT-PROJ-390.md) | SavePriceRuleDocumentCommandHandler — expectedVersion cũ bị từ chối | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-184](../systemtest/ST-PROJ-184.md) |
| [UT-PROJ-391](../unittest/UT-PROJ-391.md) | SavePriceRuleDocumentCommandHandler — gửi lặp cùng Idempotency-Key | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-392](../unittest/UT-PROJ-392.md) | SavePriceRuleDocumentCommandHandler — nội dung không hợp lệ bị từ chối, chỉ rõ mục và dòng | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-184](../systemtest/ST-PROJ-184.md) |
| [UT-PROJ-393](../unittest/UT-PROJ-393.md) | SavePriceRuleDocumentCommandHandler — lưu Quy tắc bóc tách không tính lại dải giá, cảnh báo khi thiếu tiêu đề mục | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-183](../systemtest/ST-PROJ-183.md) |
| [UT-PROJ-394](../unittest/UT-PROJ-394.md) | PreviewPriceFrameworkQueryHandler — xem trước dải giá không ghi gì, kèm cảnh báo không chặn | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-183](../systemtest/ST-PROJ-183.md) |
| [UT-PROJ-395](../unittest/UT-PROJ-395.md) | RestorePriceRuleVersionCommandHandler — khôi phục đầy đủ luật giá của bản cũ thành phiên bản mới | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-185](../systemtest/ST-PROJ-185.md) |
| [UT-PROJ-396](../unittest/UT-PROJ-396.md) | Truy vấn luật giá — tài liệu hiện hành, danh sách phiên bản, một phiên bản, không tìm thấy | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-182](../systemtest/ST-PROJ-182.md), [ST-PROJ-185](../systemtest/ST-PROJ-185.md) |
| [UT-PROJ-397](../unittest/UT-PROJ-397.md) | PriceFrameworkTables.Parse — đọc khoảng giá mục 3–5 từ văn bản | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-398](../unittest/UT-PROJ-398.md) | PriceBandDeriver — 41 dải giá tính từ văn bản gốc và từ bản nạp sẵn trùng các dải đang ghim của app demo | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-399](../unittest/UT-PROJ-399.md) | PriceBandDeriver — hình học ghim, kết cấu biệt thự, mái và nội thất theo gói | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-400](../unittest/UT-PROJ-400.md) | PriceBandDeriver — ví dụ tăng 10% đơn giá bê tông M250 | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-401](../unittest/UT-PROJ-401.md) | AiRoofRuleValidator — kiểm ma trận mái khi nạp | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-402](../unittest/UT-PROJ-402.md) | AiRoofRule → kiểu mái của app demo — câu "Kéo theo", tên thành phần mái và một hệ mái trong cả prompt | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-403](../unittest/UT-PROJ-403.md) | Dữ liệu nạp sẵn bộ quy tắc RS2 — ma trận mái và chỗ chèn lời dặn của prompt dự toán | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-404](../unittest/UT-PROJ-404.md) | ClosedXmlEstimateWorkbookWriter — dựng sheet, công thức, named range và chiều cao ô như fill_template.py | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-405](../unittest/UT-PROJ-405.md) | ClosedXmlEstimateWorkbookWriter — tính lại tổng như dung_sheet và đối chiếu với tổng của kết quả Dự toán | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-406](../unittest/UT-PROJ-406.md) | Parity Estimate — phát lại 7 hồ sơ nhật ký thô của app demo | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-407](../unittest/UT-PROJ-407.md) | Parity Estimate theo hàm — các hàm thuần trùng app demo ở các biên | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-408](../unittest/UT-PROJ-408.md) | Văn bản luật giá nạp sẵn chỉ khác bản gốc đúng bốn chỗ và cho cùng dải giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-409](../unittest/UT-PROJ-409.md) | FakeAiProvider (IToolCallModel) — phát lại tool input theo việc và khóa bước | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-410](../unittest/UT-PROJ-410.md) | GetStageResultQueryHandler (chặng Estimate) — chỉ chủ bản đọc, chỉ khi Succeeded, chỉ đọc CustomerJson | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-143](../systemtest/ST-PROJ-143.md) |
| [UT-PROJ-411](../unittest/UT-PROJ-411.md) | Chính sách quyền của API luật giá, bộ quy tắc AI và phần nội bộ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-182](../systemtest/ST-PROJ-182.md) |
| [UT-PROJ-412](../unittest/UT-PROJ-412.md) | GetEstimatePipelineQueryHandler — khối estimate của lần chạy Dự toán cho người có quyền xem | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-142](../systemtest/ST-PROJ-142.md), [ST-PROJ-143](../systemtest/ST-PROJ-143.md) |
| [UT-PROJ-413](../unittest/UT-PROJ-413.md) | StageRunFinishedConsumer — cảnh báo EstimateNeedsEngineerReview cho Admin | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-414](../unittest/UT-PROJ-414.md) | Nhật ký của chặng Dự toán — không ghi mô tả, địa chỉ, prompt hay tool input | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-415](../unittest/UT-PROJ-415.md) | EstimatePriceRuleSeedData — phiên bản 1 của hai tài liệu luật giá, giá neo, gói và dải giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-416](../unittest/UT-PROJ-416.md) | Migration EstimatePipelineEstimateStage — phiên bản cấu hình mới cho việc estimate | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-417](../unittest/UT-PROJ-417.md) | Hằng số và công tắc của app demo được ghim trong mã ở giá trị mặc định | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-418](../unittest/UT-PROJ-418.md) | Kiểm cấu hình lúc khởi động — khóa Anthropic và AnthropicOption__BaseUrl | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-419](../unittest/UT-PROJ-419.md) | EstimateResultProjection — dựng hợp đồng estimate-v1 từ CustomerJson | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-140](../systemtest/ST-PROJ-140.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [UT-PROJ-420](../unittest/UT-PROJ-420.md) | ExceptionHandlingMiddleware — mã lỗi của chặng Dự toán nằm ở messageCode, title và code theo loại ngoại lệ | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-421](../unittest/UT-PROJ-421.md) | EstimateStageRun — CHECK hai cột phiên bản luật giá theo chặng | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-422](../unittest/UT-PROJ-422.md) | PriceRuleDocument và PriceRuleDocumentVersion — khóa, FK ghép theo loại và CHECK | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-423](../unittest/UT-PROJ-423.md) | PriceAnchor, PricePackage, PriceBand — ràng buộc theo phiên bản Khung giá | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-424](../unittest/UT-PROJ-424.md) | EstimateCostResult và AiRoofRule — ràng buộc database | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-425](../unittest/UT-PROJ-425.md) | SavePriceRuleDocumentCommandHandler — hai Admin lưu đồng thời từ cùng một phiên bản | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-184](../systemtest/ST-PROJ-184.md) |
| [UT-PROJ-426](../unittest/UT-PROJ-426.md) | RecordEstimateResult và đọc kết quả — lần chạy đã TimedOut hoặc chưa Succeeded | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | [ST-PROJ-148](../systemtest/ST-PROJ-148.md) |
| [UT-PROJ-427](../unittest/UT-PROJ-427.md) | Migration EstimatePipelineEstimateStage — Up trên dữ liệu có sẵn và điều kiện Down | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |
| [UT-PROJ-428](../unittest/UT-PROJ-428.md) | RunEstimateStageCommandHandler — hai cột phiên bản trỏ đúng loại tài liệu khi Admin lưu cùng lúc | [TDD-PROJ-009](../tdd/TDD-PROJ-009.md) | Chưa liên kết System Test |

Truy vết nhanh tới tiêu chí nghiệm thu và luật:
- STORY-PROJ-008: AC-001 → UT-PROJ-296; AC-002 → UT-PROJ-137 của đợt 1 (bước 6 của TDD-PROJ-006, chặng Dự toán dùng lại); AC-003 → UT-PROJ-299, 308, 419; AC-004 → UT-PROJ-364, 373; AC-005 → UT-PROJ-380, 385, 386; AC-006 → UT-PROJ-358, 361, 362; AC-007 → UT-PROJ-297; AC-008 → UT-PROJ-296, 389 và UT-PROJ-138 của đợt 1; AC-009 → UT-PROJ-315, 361.
- STORY-PROJ-011: AC-001 → UT-PROJ-396, 411; AC-002 → UT-PROJ-394; AC-003 → UT-PROJ-389; AC-004 → UT-PROJ-392; AC-005 → UT-PROJ-390, 425; AC-006 → UT-PROJ-395; AC-008 → UT-PROJ-393. AC-007 (miền giá) đã có ở đợt 1 (UT-PROJ-286, 287).
- BR-PROJ-016 khoản 1 → UT-PROJ-408, 415; khoản 2 → UT-PROJ-396; khoản 3 → UT-PROJ-394; khoản 4 → UT-PROJ-392; khoản 5 → UT-PROJ-389; khoản 6 → UT-PROJ-390, 425; khoản 7 → UT-PROJ-395; khoản 8 → UT-PROJ-296, 297, 428; khoản 9 → UT-PROJ-411; Except → UT-PROJ-393.
- BR-PROJ-017 khoản 1 → UT-PROJ-309; khoản 2 → UT-PROJ-380 → 387; khoản 3 → UT-PROJ-410, 419; khoản 4 → UT-PROJ-309, 359, 413; khoản 5 → UT-PROJ-410, 412; Except → UT-PROJ-373.

### Đối chiếu với test của app demo

TDD-PROJ-009 mục K chuyển 14 tệp `tests/estimate/test_*.py` sang `test/bmt-be.estimatepipeline.tests/Ported/Estimate/`, giữ tên ca. Đặc tả Unit Test không lặp lại từng ca đó. Khi một hành vi có test của app demo, đặc tả lấy lại đầu vào và assertion của test đó và ghi tên `tệp::test` ở cột Rationale. Ca không có đặc tả riêng được phủ theo một trong ba cách ghi ở cột cuối. Đếm theo hàm test, chưa tính các biến thể tham số hóa.

| Tệp test của app demo | Số ca | Có đặc tả riêng | Ca còn lại: cách phủ hoặc lý do |
|---|---|---|---|
| `test_estimate.py` | 24 | 14 | 7 ca không áp dụng (Excel qua tiến trình con, chế độ demo, event loop, đọc tệp dự án, khách ngắt kết nối). 2 ca là điều kiện tiếp nhận của TDD-PROJ-006 (UT-PROJ-137, 139). 1 ca khác có chủ đích: app demo cho chạy dự toán khi 2D xong một phần, hệ thống thật chỉ mở chặng khi 2D thành công (BR-PROJ-010 khoản 1). |
| `test_estimate_checklist.py` | 20 | 4 | 16 ca kiểm chữ của prompt, schema dt-14 và khối lượng tham chiếu in trong prompt: chuyển nguyên; schema có ở UT-PROJ-341, chữ của prompt được UT-PROJ-337, 406 chứng minh trùng từng ký tự. |
| `test_estimate_customer_cells.py` | 16 | 13 | 2 ca kiểm chữ của prompt: như trên. 1 ca chạy tiến trình con điền Excel và thư mục staff của app demo: không áp dụng; phần ranh giới gửi khách đã có ở UT-PROJ-387. |
| `test_estimate_live_path.py` | 18 | 17 | 1 ca hủy khi khách ngắt kết nối: không áp dụng vì lần chạy chạy nền và dừng theo lease (TDD-PROJ-006). |
| `test_estimate_pipeline_nets.py` | 10 | 6 | 3 ca về tệp nhật ký thô và chế độ demo: không áp dụng (bản thô theo TDD-PROJ-008 mục C). 1 ca mỗi lượt một dòng metrics: phủ ở UT-PROJ-264 của đợt 1 và UT-PROJ-321. |
| `test_estimate_price_invariants.py` | 17 | 11 | 4 ca kiểm chữ của dòng giá neo và chỉ tiêu trong prompt: chuyển nguyên, phủ bằng UT-PROJ-406. 2 ca tra gói với giá trị kiểu JavaScript: chuyển nguyên; ở hệ thống thật gói là enum có kiểu trong bản chụp v3. |
| `test_estimate_prompt_cache.py` | 9 | 9 | — |
| `test_estimate_prompt_tailoring.py` | 45 | 6 | 39 ca kiểm chữ của prompt đã rút gọn: chuyển nguyên, phủ bằng UT-PROJ-334, 335, 337, 406. Ca dùng địa chỉ Tây Nguyên (ví dụ `test_a_highland_project_is_told_which_end_of_the_band_each_column_takes`) phải sửa theo quyết định 5 của mục R. |
| `test_estimate_quantity.py` | 40 | 25 | 4 ca không áp dụng (cấu hình qua biến môi trường, chế độ demo, Excel của bản demo, ranh giới module Python). 3 ca kiểm chữ của prompt và schema: chuyển nguyên, phủ bằng UT-PROJ-341, 406. 8 ca hình học lô không vuông và lô chữ nhật: chuyển nguyên, hành vi chính có ở UT-PROJ-333. |
| `test_estimate_quote_window.py` | 3 | 2 | 1 ca qua HTTP và tệp dự án: không áp dụng; công thức ngày đã có ở UT-PROJ-307. |
| `test_estimate_recovery.py` | 12 | 12 | — |
| `test_estimate_regroup.py` | 2 | 2 | — |
| `test_estimate_roof.py` | 9 | 9 | — |
| `test_price_band_derivation.py` | 12 | 12 | — |
| Tổng | 237 | 142 | |

Các test của app demo được lấy lại có điều chỉnh theo quyết định đã chốt, và đặc tả ghi rõ chỗ điều chỉnh:
- Không hạ 5% giá neo theo vùng và không có khóa Tây Nguyên (quyết định 5 của mục R, BR-PROJ-013): bỏ các vòng dùng địa chỉ Buôn Ma Thuột ở UT-PROJ-366, 376, 379; UT-PROJ-325, 336 khẳng định hành vi mới.
- Không gửi tên khách cho AI (quyết định 1): UT-PROJ-326 thay assertion `ten_khach_hang` của `test_form_from_project_maps_every_field_and_derives_area_with_roof_component`.
- Ngày lập theo ngày tiếp nhận (quyết định 2): UT-PROJ-307.
- `stop_reason=refusal` thành `AiContentBlocked` (khác app demo có chủ đích ở mục C): UT-PROJ-311.
- Các hằng số và công tắc ghim ở giá trị mặc định (quyết định 3, mục O): bỏ nửa "tắt cờ" của các test đổi cấu hình, xem UT-PROJ-353, 417.

### Phần cần kiểm chứng ngoài unit test

- Ràng buộc database, khóa FOR UPDATE khi hai Admin lưu đồng thời, ghi kết quả sau khi lần chạy đã TimedOut và migration có test tích hợp riêng trên PostgreSQL thật: UT-PROJ-421 → 428. Không dùng store giả hay EF InMemory để tuyên bố đã chứng minh các hành vi này.
- Đầu-cuối ở chế độ Fake theo ST-PROJ-140 → 148, ST-PROJ-182 → 186, ST-PROJ-199, ST-PROJ-201 và phần Dự toán của ST-PROJ-057. Chạy thật có trả phí trên dev để so số dòng, tổng, thời gian và chi phí với số đo của app demo (TDD-PROJ-009 mục N) không thuộc unit test.
- Test với nhà cung cấp AI giả chứng minh điều phối và các lưới, không chứng minh chất lượng câu trả lời của Anthropic.

### Cần làm rõ

Không còn điểm cần làm rõ. Mười hai điểm của bản bàn giao đầu được chốt ở lần cập nhật thứ nhất (TDD-PROJ-009 mục R, quyết định 8). Ba điểm còn lại được chốt ở lần cập nhật thứ hai:
- **Đã xác nhận (12/10/2026):** người dùng chọn "Không tính, lỗi hệ thống (Đề xuất)" cho trường hợp lượt hỏi lại lỗi vì hệ thống rồi kiểm trường bắt buộc thất bại (mục R, quyết định 9; UT-PROJ-362).
- **Quyết định kỹ thuật:** câu lỗi 422 cho ô giá không có chữ số (UT-PROJ-392 ca g).
- **Quyết định kỹ thuật:** cảnh báo thiếu tiêu đề khi lưu, kể cả với Quy tắc bóc tách (UT-PROJ-393).

Các mâu thuẫn giữa tài liệu ghi ở bản bàn giao đầu đều đã được sửa trong bản cập nhật cùng ngày:
- Chỗ sửa 1 ở mục O của TDD-PROJ-009 nay chỉ bỏ cụm "Tây Nguyên / ", khớp BR-PROJ-016 Notes (UT-PROJ-408).
- Mục G của TDD-PROJ-009 nay ghi ngưỡng 8 ký tự cho mọi ô gửi khách như `fill.py` (UT-PROJ-384).
- TDD-PROJ-008 mục B tách lịch chờ theo nhà cung cấp, khớp TDD-PROJ-009 mục C (UT-PROJ-312, 313).
- TDD-PROJ-008 mục B ghi rõ quy tắc gọi lại một lần khi JSON sai chỉ thuộc OpenAI; lời gọi công cụ của Anthropic theo TDD-PROJ-009 mục C.
- STORY-PROJ-011 References/TDDs đã có TDD-PROJ-009.

### Kiểm tra tài liệu đã thực hiện

Script kiểm tra trên 133 file mới cho kết quả:
- mỗi file một heading trùng Test ID, đúng một dòng dữ liệu 13 cột;
- giá trị Loại, Suite, Priority, Trạng thái hợp lệ;
- Trace to khớp TEST_LINKS;
- mọi mã và mục được dẫn đều có thật;
- mọi tên `tệp::test` của app demo được dẫn đều có trong `tests/estimate/` ở commit `059e876`.

Một số kết quả của bộ lọc gửi khách (UT-PROJ-382, 384, 385) đã chạy thử trên bản chép các hàm thuần của `fill.py`, không nạp cấu hình của app demo. Chưa chạy importer, mã Unit Test hay test tích hợp.

Sau bản cập nhật cùng ngày đã chạy lại script trên cả 133 file và không có lỗi. Năm sơ đồ mermaid của TDD-PROJ-008 và năm sơ đồ của TDD-PROJ-009 đều đọc được bằng mermaid 10.9.1. Ô D22 của `template.xlsx` (công thức và định dạng) được đọc trực tiếp từ tệp của app demo. Các số làm tròn ở UT-PROJ-419 được tính lại bằng phép chia thập phân. Sau lần cập nhật thứ hai, script lại báo không lỗi trên 133 file và cả mười sơ đồ vẫn đọc được bằng mermaid 10.9.1.


## Mốc xác nhận ngày 13/10/2026: Phối cảnh 3D, đợt 3

Người dùng xác nhận trong hội thoại: “Ok hợp lí rồi, làm di”, sau khi xem TDD và phần giải thích luồng 3D so với app demo. Phạm vi được chốt là [TDD-PROJ-010](../tdd/TDD-PROJ-010.md) tại commit tài liệu `f294de1`, gồm kế hoạch ảnh, prompt, kiểm chất lượng, lưu từng ảnh, chạy tiếp, API và giao diện.

- SHA-256 của TDD đã chốt: `0d7c05cb1064dcfa7e9f5e1e7f4402c424430edadc2eac0e1d57f5a65a4aa26c`.
- Xác nhận hội thoại cho phép tiếp tục đặc tả Unit Test và triển khai BE/FE; không thay Status Draft hoặc ghi phê duyệt trên hệ thống tài liệu.
- Đặc tả tiếp theo bắt đầu UT-PROJ-429. Kết quả kiểm thử thực thi được cập nhật sau khi chạy, không coi bản đặc tả là bằng chứng đã đạt.

### Đặc tả đợt 3

Các ca dưới đây là đặc tả; chưa phải kết quả thực thi. Ca có repository/schema/migration phải kiểm trên PostgreSQL; UI tương tác kiểm thêm qua browser theo ST-PROJ-149–154 và ST-PROJ-204.

| Mã | Unit dự kiến |
|---|---|
| [UT-PROJ-429](../unittest/UT-PROJ-429.md) | Render3dPlanner.Build |
| [UT-PROJ-430](../unittest/UT-PROJ-430.md) | Render3dPlanner.Build |
| [UT-PROJ-431](../unittest/UT-PROJ-431.md) | Render3dPlanner.RoomViews |
| [UT-PROJ-432](../unittest/UT-PROJ-432.md) | Render3dPlanner.RoomViews |
| [UT-PROJ-433](../unittest/UT-PROJ-433.md) | Render3dPlanner.RoomViews |
| [UT-PROJ-434](../unittest/UT-PROJ-434.md) | Render3dPlanner.RoomViews |
| [UT-PROJ-435](../unittest/UT-PROJ-435.md) | Render3dPlanner.RoomRenderable |
| [UT-PROJ-436](../unittest/UT-PROJ-436.md) | Render3dPlanner.RoomViews |
| [UT-PROJ-437](../unittest/UT-PROJ-437.md) | Render3dPlanner.Build |
| [UT-PROJ-438](../unittest/UT-PROJ-438.md) | Render3dPlanner.RoomLabel |
| [UT-PROJ-439](../unittest/UT-PROJ-439.md) | Render3dPrompts.Day |
| [UT-PROJ-440](../unittest/UT-PROJ-440.md) | Render3dPrompts.Frontage |
| [UT-PROJ-441](../unittest/UT-PROJ-441.md) | Render3dPrompts.SiteContext |
| [UT-PROJ-442](../unittest/UT-PROJ-442.md) | Render3dPrompts.Roof |
| [UT-PROJ-443](../unittest/UT-PROJ-443.md) | Render3dPrompts.InteriorStyle |
| [UT-PROJ-444](../unittest/UT-PROJ-444.md) | Render3dPrompts.InteriorStyle |
| [UT-PROJ-445](../unittest/UT-PROJ-445.md) | Render3dPrompts.Weather |
| [UT-PROJ-446](../unittest/UT-PROJ-446.md) | Render3dPrompts.Night |
| [UT-PROJ-447](../unittest/UT-PROJ-447.md) | Render3dPrompts.Room |
| [UT-PROJ-448](../unittest/UT-PROJ-448.md) | Render3dPrompts.ExtraView |
| [UT-PROJ-449](../unittest/UT-PROJ-449.md) | Render3dPrompts.Truncate |
| [UT-PROJ-450](../unittest/UT-PROJ-450.md) | Render3dQuality.DHash |
| [UT-PROJ-451](../unittest/UT-PROJ-451.md) | Render3dQuality.Duplicate |
| [UT-PROJ-452](../unittest/UT-PROJ-452.md) | Render3dQuality.Checks |
| [UT-PROJ-453](../unittest/UT-PROJ-453.md) | Render3dOptions.Parse |
| [UT-PROJ-454](../unittest/UT-PROJ-454.md) | Render3dStageExecutor.ExecuteAsync |
| [UT-PROJ-455](../unittest/UT-PROJ-455.md) | Render3dStageExecutor.NightReferences |
| [UT-PROJ-456](../unittest/UT-PROJ-456.md) | Render3dStageExecutor.Resume |
| [UT-PROJ-457](../unittest/UT-PROJ-457.md) | Render3dStageExecutor.Resume |
| [UT-PROJ-458](../unittest/UT-PROJ-458.md) | Render3dStageExecutor.Redraw |
| [UT-PROJ-459](../unittest/UT-PROJ-459.md) | Render3dStageExecutor.Redraw |
| [UT-PROJ-460](../unittest/UT-PROJ-460.md) | Render3dStageExecutor.QcUnavailable |
| [UT-PROJ-461](../unittest/UT-PROJ-461.md) | Render3dStageExecutor.QcRetryUnavailable |
| [UT-PROJ-462](../unittest/UT-PROJ-462.md) | Render3dStageExecutor.ContentBlocked |
| [UT-PROJ-463](../unittest/UT-PROJ-463.md) | Render3dStageExecutor.RequiredReference |
| [UT-PROJ-464](../unittest/UT-PROJ-464.md) | Render3dStageExecutor.StorageRead |
| [UT-PROJ-465](../unittest/UT-PROJ-465.md) | Render3dStageExecutor.Deadline |
| [UT-PROJ-466](../unittest/UT-PROJ-466.md) | Render3dStageExecutor.PinnedInputs |
| [UT-PROJ-467](../unittest/UT-PROJ-467.md) | Render3dResultProjection.Project |
| [UT-PROJ-468](../unittest/UT-PROJ-468.md) | Render3dResultProjection.Project |
| [UT-PROJ-469](../unittest/UT-PROJ-469.md) | Render3dResultProjection.Project |
| [UT-PROJ-470](../unittest/UT-PROJ-470.md) | GetStageResultQueryHandler.Handle |
| [UT-PROJ-471](../unittest/UT-PROJ-471.md) | RunEstimateStageCommandHandler.Handle |
| [UT-PROJ-472](../unittest/UT-PROJ-472.md) | EstimatePipelineStore.RecordStepAsync |
| [UT-PROJ-473](../unittest/UT-PROJ-473.md) | EstimatePipelineStore.Progress |
| [UT-PROJ-474](../unittest/UT-PROJ-474.md) | EstimatePipelineStore.Render3dSnapshot |
| [UT-PROJ-475](../unittest/UT-PROJ-475.md) | AiRegionInstructionConfiguration |
| [UT-PROJ-476](../unittest/UT-PROJ-476.md) | EstimatePipelineRender3dStageMigration |
| [UT-PROJ-477](../unittest/UT-PROJ-477.md) | render3dResultSchema |
| [UT-PROJ-478](../unittest/UT-PROJ-478.md) | render3dViewState |
| [UT-PROJ-479](../unittest/UT-PROJ-479.md) | render3dPolling |

Kết quả chạy đợt 3 và các giới hạn kiểm chứng: [báo cáo Phối cảnh 3D](estimate-render3d-validation.md). Đặc tả vẫn giữ Draft; không tự coi mỗi đặc tả đã được thực thi.


## Mốc xác nhận ngày 13/10/2026: Hồ sơ, đợt 4

Người dùng xác nhận “theo giao diện đã chốt, làm đi” sau khi nhận TDD-PROJ-011 hoàn chỉnh. Tiếp tục đặc tả UT và triển khai BE/FE theo giao diện đã thống nhất.

- SHA-256 TDD-PROJ-011 được chốt: `03016b28cc4e1ffcced853f5d710e2cc115ce6a8b018b9710f6fd3e81e3a6d25`.
- Hiệu chỉnh ngày 13/10/2026 theo yêu cầu “OK sửa lại phần đó đi”: giữ Vật liệu/Hãng trong PDF và Excel; chỉ màn kết quả Dự toán không hiện cột hãng. Sửa TDD và UT-PROJ-484 tương ứng. SHA-256 TDD sau hiệu chỉnh: `ebdb629fda80354a79c7a8529aa723fc2c6d4795aa85bf245add7dbc3b6f4343`. Giữ SHA phê duyệt ban đầu ở trên để truy vết.
- Giữ Status Draft, không giả phê duyệt/import.
- UT-PROJ-480–509 là đặc tả, chưa phải bằng chứng thực thi. Transaction/FK/lease cần PostgreSQL, bố cục/tải/chia sẻ cần kiểm tệp và browser theo ST-PROJ-155–159, ST-PROJ-057.

| Mã | Unit dự kiến |
|---|---|
| [UT-PROJ-480](../unittest/UT-PROJ-480.md) | DossierDocumentComposer.Build |
| [UT-PROJ-481](../unittest/UT-PROJ-481.md) | DossierDocumentComposer.Build |
| [UT-PROJ-482](../unittest/UT-PROJ-482.md) | DossierDocumentComposer.SelectPages |
| [UT-PROJ-483](../unittest/UT-PROJ-483.md) | DossierDocumentComposer.Build |
| [UT-PROJ-484](../unittest/UT-PROJ-484.md) | DossierDocumentComposer.CustomerData |
| [UT-PROJ-485](../unittest/UT-PROJ-485.md) | DossierDocumentComposer.PrintContext |
| [UT-PROJ-486](../unittest/UT-PROJ-486.md) | DossierDocumentComposer.Hero |
| [UT-PROJ-487](../unittest/UT-PROJ-487.md) | DossierDocumentComposer.Branding |
| [UT-PROJ-488](../unittest/UT-PROJ-488.md) | DossierStageExecutor.ExecuteAsync |
| [UT-PROJ-489](../unittest/UT-PROJ-489.md) | DossierStageExecutor.Retry |
| [UT-PROJ-490](../unittest/UT-PROJ-490.md) | DossierStageExecutor.Retry |
| [UT-PROJ-491](../unittest/UT-PROJ-491.md) | DossierStageExecutor.Resume |
| [UT-PROJ-492](../unittest/UT-PROJ-492.md) | DossierStageExecutor.Resume |
| [UT-PROJ-493](../unittest/UT-PROJ-493.md) | DossierStageExecutor.Context |
| [UT-PROJ-494](../unittest/UT-PROJ-494.md) | DossierStageExecutor.SourceValidation |
| [UT-PROJ-495](../unittest/UT-PROJ-495.md) | DossierStageExecutor.Cancellation |
| [UT-PROJ-496](../unittest/UT-PROJ-496.md) | DossierPublishedState.IsComplete |
| [UT-PROJ-497](../unittest/UT-PROJ-497.md) | DossierPublishedState.IsComplete |
| [UT-PROJ-498](../unittest/UT-PROJ-498.md) | FinalizeStageRunCommandHandler.Handle |
| [UT-PROJ-499](../unittest/UT-PROJ-499.md) | FinalizeStageRunCommandHandler.Handle |
| [UT-PROJ-500](../unittest/UT-PROJ-500.md) | FinalizeStageRunCommandHandler.Handle |
| [UT-PROJ-501](../unittest/UT-PROJ-501.md) | DossierResultContract.ProjectDossier |
| [UT-PROJ-502](../unittest/UT-PROJ-502.md) | GetStageFileQueryHandler.Handle |
| [UT-PROJ-503](../unittest/UT-PROJ-503.md) | GetStageFileQueryHandler.Handle |
| [UT-PROJ-504](../unittest/UT-PROJ-504.md) | EstimateFileQueryHandlers.DownloadName |
| [UT-PROJ-505](../unittest/UT-PROJ-505.md) | RequestEstimateExportCommandHandler.Handle |
| [UT-PROJ-506](../unittest/UT-PROJ-506.md) | GetSharedEstimateFileQueryHandler.Handle |
| [UT-PROJ-507](../unittest/UT-PROJ-507.md) | dossierResultSchema |
| [UT-PROJ-508](../unittest/UT-PROJ-508.md) | dossierViewState |
| [UT-PROJ-509](../unittest/UT-PROJ-509.md) | myEstimatesRoute |


### Tiến độ kiểm chứng đợt 4 — 13/10/2026

Frontend W4 đã qua 38 test logic (34 ca pipeline có sẵn và 4 ca Hồ sơ mới), kiểm tra TypeScript, ESLint các tệp thay đổi và build production bằng Webpack. Test mới kiểm hợp đồng đủ bốn vai trò, URL/MIME/size/pageCount không hợp lệ, giữ tổng tiền nguồn trong bản chia sẻ và điều hướng đúng chặng; hỗ trợ UT-PROJ-507/509, chưa thay thế kiểm thử giao diện UT-PROJ-508 hoặc system test.

Backend W4 đã có mã và test đang viết nhưng chưa biên dịch/chạy: build `--no-restore` dừng NETSDK1004 vì thiếu assets NuGet. Đang chờ xác nhận chạy restore theo AGENTS.md backend. Chưa tạo migration, chưa chạy Dossier trên dữ liệu local, chưa kiểm PDF/Excel thực tế, browser, PostgreSQL hoặc quyền chia sẻ. UT vẫn giữ Draft; chưa đánh dấu đạt.

- Hiệu chỉnh format PDF ngày 13/10/2026 theo yêu cầu “chỉnh sửa lại file PDF cho giống với format trong app demo”: bìa/cảm ơn, hai cột, khung tên và bảng giá theo `pdf.py`; không đổi dữ liệu nguồn. SHA-256 TDD-PROJ-011 sau bổ sung chi tiết bố cục: `274bc24e952d4eefc9462a31cc9893931c68c570c7f12244b009ddcbe2b387c7`.


## Đợt Admin — TDD-PROJ-012

Người dùng xác nhận “Ok chốt” và “ok chốt” ngày 13/10/2026. SHA-256 bản TDD được chốt: `66bb1c9539f5fd1f61efc9b33236489e8c65d25574600fe38c96f244b72fe70d`. Phạm vi: STORY-PROJ-011/012/013; chủ đề theo miền chọn chặng 2D/Dự toán/3D. Không đổi Status thành Approved thay cho phê duyệt trên hệ thống.

Đặc tả UT-PROJ-510–548 đã soạn theo bản này và giữ Draft. Kết quả thực thi ngày 13/10/2026 được ghi riêng trong [báo cáo triển khai Admin](estimate-admin-implementation.md): ứng dụng, HTTP, PostgreSQL, worker trial, model EF, frontend và browser Fake. Không suy ra mọi đặc tả đã chạy chỉ từ trạng thái tài liệu.

| Đặc tả | Unit |
|---|---|
| [UT-PROJ-510](../unittest/UT-PROJ-510.md) | AiRuleSetWriteFlow.Save |
| [UT-PROJ-511](../unittest/UT-PROJ-511.md) | AiRuleSetWriteFlow.Save |
| [UT-PROJ-512](../unittest/UT-PROJ-512.md) | AiRuleSetWriteFlow.Save |
| [UT-PROJ-513](../unittest/UT-PROJ-513.md) | AiRuleSetWriteFlow.Save |
| [UT-PROJ-514](../unittest/UT-PROJ-514.md) | AiRuleSetWriteFlow.Restore |
| [UT-PROJ-515](../unittest/UT-PROJ-515.md) | AiRuleSetWriteFlow.RestorePrompt |
| [UT-PROJ-516](../unittest/UT-PROJ-516.md) | AiRuleSetValidator.BaseTypes |
| [UT-PROJ-517](../unittest/UT-PROJ-517.md) | AiRuleSetValidator.Roofs |
| [UT-PROJ-518](../unittest/UT-PROJ-518.md) | AiRuleSetValidator.Instructions |
| [UT-PROJ-519](../unittest/UT-PROJ-519.md) | AiRuleSetValidator.RegionInstructions |
| [UT-PROJ-520](../unittest/UT-PROJ-520.md) | AiRuleMapping.RegionInstructions |
| [UT-PROJ-521](../unittest/UT-PROJ-521.md) | AiRuleMapping.Instructions |
| [UT-PROJ-522](../unittest/UT-PROJ-522.md) | PromptSlotValidator |
| [UT-PROJ-523](../unittest/UT-PROJ-523.md) | PromptSlotValidator |
| [UT-PROJ-524](../unittest/UT-PROJ-524.md) | SaveAiPromptCommandValidator |
| [UT-PROJ-525](../unittest/UT-PROJ-525.md) | AiPromptPreviewComposer |
| [UT-PROJ-526](../unittest/UT-PROJ-526.md) | AiPromptPreviewComposer |
| [UT-PROJ-527](../unittest/UT-PROJ-527.md) | AiPromptPreviewComposer.Hash |
| [UT-PROJ-528](../unittest/UT-PROJ-528.md) | CreateAiPromptTrialCommandHandler |
| [UT-PROJ-529](../unittest/UT-PROJ-529.md) | CreateAiPromptTrialCommandHandler |
| [UT-PROJ-530](../unittest/UT-PROJ-530.md) | CreateAiPromptTrialCommandHandler |
| [UT-PROJ-531](../unittest/UT-PROJ-531.md) | CreateAiPromptTrialCommandHandler |
| [UT-PROJ-532](../unittest/UT-PROJ-532.md) | AiPromptTrialWorker |
| [UT-PROJ-533](../unittest/UT-PROJ-533.md) | AiPromptTrialWorker |
| [UT-PROJ-534](../unittest/UT-PROJ-534.md) | AiPromptTrialWorker |
| [UT-PROJ-535](../unittest/UT-PROJ-535.md) | AiPromptTrialWorker |
| [UT-PROJ-536](../unittest/UT-PROJ-536.md) | AiPromptTrialWorker |
| [UT-PROJ-537](../unittest/UT-PROJ-537.md) | AiPromptTrialCost |
| [UT-PROJ-538](../unittest/UT-PROJ-538.md) | AiRuleSetWriteFlow.Restore |
| [UT-PROJ-539](../unittest/UT-PROJ-539.md) | AiRuleSetQueryProjection |
| [UT-PROJ-540](../unittest/UT-PROJ-540.md) | AdminDraftState |
| [UT-PROJ-541](../unittest/UT-PROJ-541.md) | PromptDraftState |
| [UT-PROJ-542](../unittest/UT-PROJ-542.md) | PromptSaveWarnings |
| [UT-PROJ-543](../unittest/UT-PROJ-543.md) | RevisionComparison |
| [UT-PROJ-544](../unittest/UT-PROJ-544.md) | PriceRuleEditor |
| [UT-PROJ-545](../unittest/UT-PROJ-545.md) | PriceRuleEditor.Conflict |
| [UT-PROJ-546](../unittest/UT-PROJ-546.md) | AdminRouteAccess |
| [UT-PROJ-547](../unittest/UT-PROJ-547.md) | GetAiCallRawQueryHandler |
| [UT-PROJ-548](../unittest/UT-PROJ-548.md) | ResetStageFailuresCommandHandler |
