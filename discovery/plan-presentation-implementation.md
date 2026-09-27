# Bàn giao phần hiển thị gói và danh sách quản trị

Backend đã bổ sung thông tin hiển thị gói Design và API lấy mọi gói cho admin theo [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-008](../businessrule/BR-SUB-008.md) và [TDD-SUB-001](../tdd/TDD-SUB-001.md). Code và tài liệu được bàn giao trên nhánh `feature/plan-presentation`; chưa triển khai lên môi trường đang dùng. Frontend chưa sửa vì workspace chỉ có backend và tài liệu, chưa có repository giao diện.

## Body tạo gói và lưu nháp

`POST /api/v1/admin/plans` nhận năm trường mới ở gốc body. Ví dụ một gói đủ điều kiện công bố; URL ảnh phải được thay bằng URL thật do kho upload đã cấu hình trả về:

```json
{
  "code": "design-plus",
  "kind": "Design",
  "name": "PLUS",
  "description": "Gói thiết kế PLUS",
  "consultationText": "Tư vấn phương án thiết kế",
  "coverImageUrl": "https://files.example.test/plans/plus.jpg",
  "isHighlighted": true,
  "highlightLabel": "PHỔ BIẾN NHẤT",
  "giftDescription": "Nội dung quà do admin nhập",
  "giftConditions": "Điều kiện áp dụng do admin nhập",
  "offers": [
    {
      "offerKey": "Month",
      "price": 100000,
      "currency": "VND",
      "quotas": [{ "code": "design.generate", "isUnlimited": false, "limit": 5 }]
    },
    {
      "offerKey": "Year",
      "price": 900000,
      "currency": "VND",
      "quotas": [{ "code": "design.generate", "isUnlimited": true, "limit": null }]
    }
  ],
  "displayBenefits": [
    { "code": "design.render3d", "enabled": false, "displayText": "Phối cảnh 3D", "sortOrder": 0 }
  ]
}
```

`PUT /api/v1/admin/plans/{planId}/draft` dùng cùng nội dung cấu hình, bỏ `code` và `kind`, thêm `expectedVersion`. PUT thay toàn bộ bản nháp: bỏ trường mới khỏi payload sẽ đặt trường đó về `null` hoặc `false`. Frontend quản trị cần đọc detail rồi gửi lại đầy đủ các trường muốn giữ.

| Trường | Quy tắc |
|---|---|
| `coverImageUrl` | Trim, rỗng thành null, tối đa 2048. URL tuyệt đối HTTPS, host DNS, không khoảng trắng, userinfo hoặc cổng khác mặc định. URL mới phải thuộc `UploadedFileOption:AllowedHosts`. |
| `isHighlighted` | Mặc định false. Nhiều gói có thể cùng nổi bật. |
| `highlightLabel` | Trim, rỗng thành null, tối đa 200. Công bố gói nổi bật phải có nhãn; false vẫn được lưu nhãn. |
| `giftDescription` | Văn bản tùy chọn, trim, tối đa 4000. |
| `giftConditions` | Văn bản tùy chọn độc lập với mô tả quà, trim, tối đa 4000. |

Design lưu nháp khi thiếu ảnh/nhãn được; công bố cần ảnh, đủ Month/Year và ít nhất một quyền lợi. Giá phải nguyên đồng và lớn hơn 0. Hạn mức hữu hạn từ 1; không giới hạn dùng `limit: null`. Quyền 3D dạng Boolean, kể cả `enabled: false`, được tính là một quyền lợi cấu hình; không sinh quota hoặc logic sử dụng 3D.

Supervision giữ giá theo ConstructionSite và mô tả dịch vụ. Năm trường mới chỉ nhận null/false; không bắt ảnh khi công bố.

Ảnh giữ nguyên từ Draft hoặc PublishedRevision hiện hành của chính gói được dùng lại mà không kiểm lại host. Ảnh từ bản lịch sử hoặc gói khác vẫn được coi là URL mới. Backend không tải URL để kiểm nội dung.

## API đọc và cách dùng trên frontend

- `GET /api/v1/plans?kind=Design`: công khai, chỉ trả gói OnSale và revision đang công bố. Năm trường mới nằm trong `revision`.
- `GET /api/v1/admin/plans/{planId}`: cần `plan.manage`, trả đủ cấu hình trong `publishedRevision` và `draft` riêng biệt, kèm `version`.
- `GET /api/v1/admin/plans`: cần `plan.manage`; mặc định lấy cả Design/Supervision và NotPublished/OnSale/Stopped. Bộ lọc tùy chọn `kind`, `saleState`; null/chuỗi rỗng nghĩa là không lọc. Giá trị khác danh mục trả 422 `PlanListFilterInvalid`.
- Phân trang admin: mặc định `pageIndex=1`, `pageSize=20`; index dưới 1 thành 1, size ngoài 1–100 thành 20. Sắp theo Plan.Id tăng dần, lọc trước khi đếm/phân trang, mỗi Plan một mục. Trang vượt phạm vi trả danh sách rỗng.
- Mỗi mục admin gồm `planId`, `code`, `kind`, `saleState`, `version`, `publishedRevision`, `draft`. Hai revision có thể null; mỗi tóm tắt gồm `id`, `number`, `name`, `coverImageUrl`, `isHighlighted`, `highlightLabel`. Mở detail để sửa quà, giá và quyền lợi.

Trang `/vi/plans` cần chọn Month/Year và lấy giá/quota từ đúng offer, bỏ nội dung “Thanh toán 1 lần”. Ảnh null ở gói cũ dùng trạng thái hiển thị không có ảnh; không ẩn cả gói. Nhãn chỉ hiện khi `isHighlighted=true`. Quà và điều kiện được render như văn bản, không chèn HTML từ nội dung admin. Backend chưa quản lý việc cấp/nhận quà hoặc chụp nội dung quà vào đơn hàng.

## Migration và triển khai

Migration: `20260927031814_PlanPresentation`.

1. Thêm bốn cột nullable và `IsHighlighted NOT NULL DEFAULT false` trên PlanRevision. Giữ nguyên các bản đã công bố, con trỏ, trạng thái bán, giá/quota và kỳ đã mua.
2. Seed `design.render3d` dạng Boolean/None/Design. Nếu cùng code đã có đúng ý nghĩa thì giữ ID/nhãn hiện có; nếu sai ý nghĩa thì dừng và rollback migration.
3. Áp dụng schema trước code; phối hợp frontend quản trị để PUT đầy đủ trường. Trong cửa sổ chuyển đổi, tránh để phiên bản API cũ tiếp tục công bố mà bỏ qua quy tắc ảnh.
4. Gói cũ thiếu ảnh tiếp tục bán. Lần công bố tiếp theo phải bổ sung ảnh qua bản nháp.
5. Không tự chạy Down để quay lại code cũ: Down xóa các cột mới và làm mất nội dung. Down giữ định nghĩa 3D vì có thể tồn tại từ trước hoặc đang được tham chiếu. Ưu tiên giữ schema mở rộng và dữ liệu khi quay lại ứng dụng cũ.

Chưa áp dụng migration lên database dev/live đang chạy; kiểm thử dùng PostgreSQL 15 trong container riêng. Chưa đo khóa DDL theo quy mô dữ liệu production.

## Kiểm chứng

Kết quả ngày 27/09/2026, tất cả không có ca bị bỏ qua:

| Bộ kiểm thử | Kết quả | Bằng chứng TRX tại máy chạy |
|---|---|---|
| Toàn bộ application | 1326 đạt, 0 lỗi | `/private/tmp/bmt-plan-results/application-regression.trx` |
| Toàn bộ API, gồm route/policy/OpenAPI | 323 đạt, 0 lỗi | `/private/tmp/bmt-plan-results/api-regression.trx` |
| PostgreSQL: PlanPresentation, PublishedPlanProtection, PlanCatalogConstraint, PlanConcurrency | 52 đạt, 0 lỗi | `/private/tmp/bmt-plan-results/plan-postgres.trx` |

API và các project liên quan đã build thành công khi chạy test. Dependency đã được restore theo sự đồng ý của người dùng; không nâng phiên bản package. Các đặc tả UT/ST giữ trạng thái Draft; trạng thái tài liệu không thay thế kết quả kiểm thử.

Lệnh tái chạy từ `bmt-be/`:

```sh
dotnet test test/bmt-be.application.tests/bmt-be.application.tests.csproj --no-restore -m:1 -nr:false
dotnet test test/bmt-be.api.tests/bmt-be.api.tests.csproj --no-restore -m:1 -nr:false
BMT_REQUIRE_DOCKER_TESTS=1 dotnet test test/bmt-be.integration.tests/bmt-be.integration.tests.csproj --no-restore -m:1 -nr:false --filter 'FullyQualifiedName~PlanPresentation|FullyQualifiedName~PublishedPlanProtection|FullyQualifiedName~PlanCatalogConstraint|FullyQualifiedName~PlanConcurrency'
```

Các file kiểm thử chính:

- [PlanPresentationPolicyTests](../../bmt-be/test/bmt-be.application.tests/usecases/plan/PlanPresentationPolicyTests.cs): UT-SUB-095–100, 109–110, 120; draft/publish, giới hạn sau trim, URL, Supervision, Boolean và quà.
- [PlanPresentationHandlerTests](../../bmt-be/test/bmt-be.application.tests/usecases/plan/PlanPresentationHandlerTests.cs): UT-SUB-098, 101–108, 110, 119–120; host policy, lưu/xóa trường, giữ bản công bố và mapping DTO.
- [AdminPlanQueryTests](../../bmt-be/test/bmt-be.application.tests/usecases/plan/AdminPlanQueryTests.cs): UT-SUB-111–118; mọi loại/trạng thái, lọc, phân trang, summary theo con trỏ, tài khoản không hợp lệ.
- [PlanApiTests](../../bmt-be/test/bmt-be.api.tests/PlanApiTests.cs): route/policy/mapping HTTP và mã lỗi; MediatR giả nên không thay thế kiểm thử handler/database.
- [PlanPresentationFlowTests](../../bmt-be/test/bmt-be.integration.tests/PlanPresentationFlowTests.cs), [PlanPresentationMigrationTests](../../bmt-be/test/bmt-be.integration.tests/PlanPresentationMigrationTests.cs), [PublishedPlanProtectionTests](../../bmt-be/test/bmt-be.integration.tests/PublishedPlanProtectionTests.cs): pipeline/transaction/repository thật, schema cũ, FK kỳ mua, rollback khi seed xung đột và bất biến của năm trường mới.

Phần backend tương ứng ST-SUB-128–134 đã có kiểm thử tự động. Chưa kết luận các System Test này đạt toàn bộ vì còn bước giao diện. ST-SUB-135 về màn hình chọn tháng/năm chưa chạy; cần mã frontend để triển khai và kiểm thử trình duyệt.
