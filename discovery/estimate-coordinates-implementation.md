# Tọa độ dự toán — kết quả triển khai backend

Ngày kiểm tra: 01/10/2026. Mã nguồn đã được commit tại `5e396fc` và đẩy lên `origin/develop` của `bmt-be`. Pipeline tự triển khai Dev được kích hoạt khi push; việc push thành công chưa xác nhận migration hoặc deploy đã hoàn tất.

Backend đã nhận, kiểm tra và lưu `latitude` (vĩ độ), `longitude` (kinh độ) theo [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) và [TDD-PROJ-002](../tdd/TDD-PROJ-002.md). Người dùng đã xác nhận hai trường bắt buộc; FE lấy từ bản đồ trước khi tạo và lấy lại khi đổi địa chỉ. Chưa có dữ liệu thật cần giữ.

## Contract dành cho FE

Tạo bằng `POST /api/v1/estimates`, kèm xác thực và header `Idempotency-Key` như hiện tại:

```json
{
  "name": "Nhà của tôi",
  "latitude": 21.033,
  "longitude": 105.814
}
```

Hai trường phải là JSON number hữu hạn: latitude trong [-90, 90], longitude trong [-180, 180]. Giá trị 0 hợp lệ. Thiếu, null hoặc ngoài miền trả 422; chuỗi số và kiểu JSON sai bị từ chối ở bước đọc body với HTTP 400.

`PUT /api/v1/estimates/{estimateId}/input` tiếp tục gửi toàn bộ `input`, luôn có đủ cặp tọa độ. Khi đổi tỉnh, xã hoặc địa chỉ chi tiết, FE lấy lại tọa độ và đưa cả `latitude`, `longitude` vào `changedFields`, cùng các trường địa chỉ chủ động sửa. Cặp mới có thể bằng cặp cũ. Khi chỉ sửa trường khác, gửi lại cặp đang lưu; không tự đổi số ngoài `changedFields`.

Ví dụ đổi địa chỉ chi tiết của bản nháp chưa chọn các thông tin khác:

```json
{
  "expectedInputVersion": 1,
  "changedFields": ["addressDetail", "latitude", "longitude"],
  "input": {
    "buildingTypeId": null,
    "areaM2": null,
    "description": null,
    "provinceCode": null,
    "wardCode": null,
    "locationDatasetVersion": null,
    "addressDetail": "Địa chỉ mới",
    "finishPackage": null,
    "floorCount": null,
    "hasTum": null,
    "architectureStyleId": null,
    "interiorStyleId": null,
    "inputImageUrl": null,
    "latitude": 21.034,
    "longitude": 105.815
  }
}
```

Với bản đã có dữ liệu, FE giữ các giá trị đang lưu ở những trường không sửa. GET chi tiết trả tọa độ tại `value.input.latitude` và `value.input.longitude`. Lấy version và cặp này để chuẩn bị lần lưu tiếp theo. Dùng key mới cho thao tác mới; cùng key nhưng khác tọa độ trả `IdempotencyConflict`.

FE chịu trách nhiệm chờ bản đồ và bỏ kết quả cũ đến muộn. Backend kiểm dữ liệu và việc khai báo cặp, không xác minh được FE đã gọi bản đồ hay tọa độ có khớp thực địa. Tích hợp bản đồ trên FE nằm ngoài phần mã đã triển khai này.

## Dữ liệu và snapshot AI

- `Estimate.Latitude/Longitude` dùng `double precision`, NOT NULL, CHECK miền giá trị, không có default và không thêm index không gian.
- Địa chỉ, cặp tọa độ, `InputVersion` và receipt được lưu trong cùng transaction hiện có. Gửi lại yêu cầu đã thành công không ghi đè dữ liệu của lần sửa sau.
- Hash tạo/lưu bao gồm cả tọa độ, dùng biểu diễn số không phụ thuộc culture và coi -0 bằng 0.
- Snapshot mới có `SchemaVersion=2`, chứa cặp đã lưu trong `address.latitude/longitude`. Worker gửi đúng payload và schema của từng snapshot; snapshot v1 cũ vẫn được chuyển nguyên vẹn. Chưa xác nhận payload này với dịch vụ AI thật.

Migration: `bmt-be/src/bmt-be.persistence/Migrations/20261001093238_EstimateCoordinates.cs`. Đã chạy trên PostgreSQL 15 trong container kiểm thử riêng. Chưa áp dụng vào database dùng chung.

Migration dừng nếu bảng còn dòng thiếu tọa độ, không tự điền (0,0). Down từ chối khi bảng có dữ liệu để tránh xóa cặp đã lưu. Trước khi triển khai phải chuẩn bị FE/BE cùng contract, dừng writer cũ và kiểm dữ liệu đích theo kế hoạch trong TDD-PROJ-001/Data Model.

## Bằng chứng kiểm thử

Các lệnh chạy từ `bmt-be`, đều dùng `--no-restore -m:1 -nodeReuse:false -p:UseSharedCompilation=false -v minimal`:

| Project test | Bộ lọc FullyQualifiedName | Kết quả | Log local |
|---|---|---|---|
| `test/bmt-be.application.tests/bmt-be.application.tests.csproj` | `~estimate` | 363 đạt, 0 lỗi, 0 bỏ qua | `/private/tmp/bmt-coordinates-app-final3.log` |
| `test/bmt-be.api.tests/bmt-be.api.tests.csproj` | `~Estimate` | 67 đạt, 0 lỗi, 0 bỏ qua | `/private/tmp/bmt-coordinates-api-final.log` |
| `test/bmt-be.infrastructure.tests/bmt-be.infrastructure.tests.csproj` | `~Estimate` | 65 đạt, 0 lỗi, 0 bỏ qua | `/private/tmp/bmt-coordinates-infra.log` |
| `test/bmt-be.integration.tests/bmt-be.integration.tests.csproj` | `~Estimate` | 103 đạt, 0 lỗi, 0 bỏ qua | `/private/tmp/bmt-coordinates-postgres.log` |

Tổng 598 test đạt, gồm các ca hồi quy dự toán và ca tọa độ mới. Số này không phải 598 ca mới. Log nằm ở thư mục tạm, có thể bị dọn; tên lớp test bên dưới là căn cứ để chạy lại.

| Yêu cầu hoặc đặc tả | Mã test và phạm vi đã kiểm |
|---|---|
| STORY-PROJ-001/AC-021, AC-024; UT-PROJ-118/119/120 | `EstimateCoordinateTests`: bắt buộc, miền, NaN/Infinity, biên và số 0. `EstimateCoordinateApiTests`: route thật, đọc JSON thật, validator/pipeline thật; phần ghi sau validation dùng fake để ghi nhận có được gọi hay không. |
| AC-023; BR-PROJ-004 khoản 18; UT-PROJ-121/122/123/129 | `EstimateCoordinateTests`: thiếu một tên tọa độ, đổi tỉnh/xã/chi tiết và chuyển null, cặp mới bằng cũ, thay đổi ngầm, đổi phiên bản hoặc tên địa giới, lưu cặp cùng địa chỉ. |
| AC-022; UT-PROJ-124/125/126; UT-PROJ-015 | `EstimateCoordinateTests`: lưu/đọc/ánh xạ entity và response, hash theo culture, -0/0, thay một tọa độ, replay tạo/lưu không ghi đè lần sửa mới. |
| UT-PROJ-014 | `EstimateCommandHandlerTests.Handle_SaveLateOrLocked_IsRejected`: version cũ, Pending và Succeeded đều giữ nguyên địa chỉ/cặp/version, không tạo receipt. |
| UT-PROJ-023/063/065/127/128 | `EstimateGenerationInputFactoryTests`, `RequestEstimateGenerationCommandHandlerTests`, `EstimateGenerationRunnerTests`: tọa độ sai bị chặn trước giữ lượt, snapshot v2 bất biến, đổi tên địa giới giữ cặp, worker chuyển đúng payload/schema v1 và v2. |
| Phần backend của ST-PROJ-117/121/122/124 | `EstimateCoordinateFlowTests`: PostgreSQL thật kiểm NOT NULL/CHECK, NaN/±Infinity, biên, không default, lưu đồng thời hai kết nối và rollback khi SQL lỗi trước commit. |
| TDD-PROJ-001/Data Model | `EstimateCoordinateMigrationTests`: dòng cũ chặn upgrade và rollback schema; bảng rỗng nâng cấp được; có dữ liệu mới thì Down bị chặn; model khớp snapshot migration. |

Các nhóm trên mô tả phần đã kiểm, không tự chuyển toàn bộ UT/ST sang Pass. Chưa chạy luồng HTTP → handler → PostgreSQL trong cùng một host cho tọa độ; kiểm HTTP và persistence được thực hiện ở hai bộ test riêng. Chưa chạy browser/map, ST-PROJ-116/119, E2E đầy đủ ST-PROJ-115 đến 125 hoặc AI thật. Các đặc tả giữ trạng thái Draft và metadata hiện có.
