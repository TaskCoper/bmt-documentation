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
| [UT-PROJ-262](../unittest/UT-PROJ-262.md) | TDD-PROJ-008 | AnthropicMessagesClient — luồng bị cắt được thử lại, max_tokens và refusal không thử lại (đợt 2) | Branch | P2 |
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
| [UT-PROJ-287](../unittest/UT-PROJ-287.md) | TDD-PROJ-008 | PricingRegionValidator — tỉnh không tồn tại, thiếu miền, Tây Nguyên ngoài miền Trung | Error | P1 |

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

