# Tọa độ dự toán — kết quả triển khai backend

Ngày kiểm tra: 01/10/2026. Mã nguồn đã được commit tại `5e396fc` và đẩy lên `origin/develop` của `bmt-be`. Pipeline tự triển khai Dev được kích hoạt khi push; việc push thành công chưa xác nhận migration hoặc deploy đã hoàn tất.

Backend đã nhận, kiểm tra và lưu `latitude` (vĩ độ), `longitude` (kinh độ) theo [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) và [TDD-PROJ-002](../tdd/TDD-PROJ-002.md). Người dùng đã xác nhận hai trường bắt buộc; FE lấy từ bản đồ trước khi tạo và lấy lại khi đổi địa chỉ. Chưa có dữ liệu thật cần giữ.

## Điều chỉnh tạo nhanh — 01/10/2026

Theo yêu cầu mới, `POST /api/v1/estimates` chỉ cần body `{"name":"Nhà của tôi"}`; giữ xác thực và `Idempotency-Key`. Bỏ cả tọa độ hoặc gửi cả hai null đều được; nếu gửi số thì cần đủ cặp hợp lệ. `additionalProp1/2/3` trong ví dụ Swagger là trường lạ, cần bỏ khỏi request.

Backend lưu tọa độ chưa có là NULL, GET trả null và báo thiếu hai trường trong `missingFields`. Bản này vẫn tự lưu mô tả, diện tích hoặc các trường khác khi không đổi địa chỉ. Khi nhập/đổi địa chỉ vẫn phải gửi đủ cặp và khai báo trong `changedFields`; trước AI phải có đủ tọa độ. Không tự điền (0,0), không xóa tọa độ của bản cũ.

Migration mới `20261001105408_OptionalEstimateCoordinates` bỏ NOT NULL, giữ CHECK miền và thêm `CK_Estimate_CoordinatePair`. Up giữ nguyên dữ liệu; Down dừng nếu còn bản chưa có tọa độ. Chỉ chạy migration trong PostgreSQL tạm, chưa triển khai API hoặc áp migration lên môi trường dùng chung. Khi triển khai, dừng writer cũ, áp migration rồi chạy toàn bộ API/worker mới trước khi mở tạo nhanh; code cũ không đọc được các dòng NULL.

Kiểm chứng bản sửa bằng `dotnet test --no-restore`, bộ lọc dự toán: application 367 đạt; API 69 đạt; PostgreSQL/migration 27 đạt; không lỗi, không bỏ qua. Log local lần lượt là `/private/tmp/bmt-quick-create-app.log`, `/private/tmp/bmt-quick-create-api.log`, `/private/tmp/bmt-quick-create-postgres.log`. API test dùng HTTP binding và validator thật với spy ở bước ghi; PostgreSQL test gọi handler thật với store/transaction thật, giả lập quyền ghi. Chưa chạy browser hoặc HTTP tới API đang triển khai. Không coi các ca này là E2E toàn hệ thống.

## Mô tả dùng chung khi tạo nhanh — 01/10/2026

Theo xác nhận của người dùng, tạo nhanh nhận `description` tùy chọn, dùng chung Mô tả chi tiết hiện có. Ví dụ body:

```json
{"name":"Nhà của tôi","description":"Nhà hai tầng, có sân"}
```

Không gửi `description` hoặc gửi null đều được. Mô tả tối đa 500 Unicode scalar; backend giữ nguyên nội dung và lưu vào `Estimate.Description`. GET trả qua `input.description`; PUT /input sửa cùng trường. Không thêm cột hay migration cho mô tả. Nội dung quá dài trả 422 `InvalidEstimateInput` tại `description`; sai kiểu JSON trả 400.

Mô tả tham gia kiểm tra `Idempotency-Key`: cùng key nhưng đổi mô tả trả 409; gửi lại yêu cầu tạo ban đầu không ghi đè mô tả đã sửa. Yêu cầu không có mô tả giữ cách tính hash cũ để replay được receipt trước thay đổi.

Đã chạy `dotnet test --no-restore`: application 370 đạt (`~estimate`), API 75 đạt (`~Estimate`), PostgreSQL/migration 28 đạt (`~EstimateCoordinate|~IdempotentMigrationScriptTests`); tổng 473, không lỗi hoặc bỏ qua. Log: `/private/tmp/bmt-description-app.log`, `/private/tmp/bmt-description-api.log`, `/private/tmp/bmt-description-postgres.log`. Các ca mới kiểm Unicode, HTTP binding/validation, tạo–đọc–sửa cùng trường, replay sau khi sửa và receipt cũ. Phạm vi HTTP dùng spy ở bước ghi; PostgreSQL gọi handler/store/transaction thật với quyền ghi giả lập. Chưa chạy browser hoặc API đang triển khai. Backend tạo nhanh và mô tả đã đẩy lên `develop` tại commit `fe85d5c`, sau khi ghép với `689e36e`. Đã chạy lại trên bản ghép: application 370, API 75 và PostgreSQL/migration 28 test đạt; không lỗi hoặc bỏ qua. Log tương ứng: `/private/tmp/bmt-quick-push-app.log`, `/private/tmp/bmt-quick-push-api.log`, `/private/tmp/bmt-quick-push-integration.log`. Model đích của migration đã giữ đầy đủ schema công trình mới nhất. Push không xác nhận deploy hoặc migration trên Dev đã hoàn tất.

## Contract dành cho FE — lịch sử trước điều chỉnh tạo nhanh

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

Migration mặc định dừng nếu bảng còn dòng thiếu tọa độ, không tự điền (0,0). Bản sửa phục hồi Dev bên dưới bổ sung ngoại lệ có cờ bật riêng cho dữ liệu thử. Down từ chối khi bảng có dữ liệu để tránh xóa cặp đã lưu. Trước khi triển khai phải chuẩn bị FE/BE cùng contract, dừng writer cũ và kiểm dữ liệu đích theo kế hoạch trong TDD-PROJ-001/Data Model.

## Phục hồi migration trên Dev — 01/10/2026

[Job 110316252815](https://github.com/TaskCoper/bmt-be/actions/runs/36845593860/job/110316252815) dừng khi thêm `Latitude NOT NULL` vì bảng `Estimate` đã có dữ liệu. Người dùng xác nhận đây là dữ liệu thử và yêu cầu điền tọa độ ngẫu nhiên.

Bản sửa giữ nguyên mã migration `20261001093238_EstimateCoordinates`: thêm hai cột nullable, điền giá trị thử khi phiên PostgreSQL có `bmt.seed_estimate_test_coordinates=on`, rồi áp NOT NULL và CHECK. Workflow chỉ truyền cờ `on` qua `PGOPTIONS` vào phiên psql khi profile và environment đều là `dev`; các môi trường khác dùng `off`. Không thêm default vào schema, không xóa dự toán hay các bảng liên quan. Tọa độ ngẫu nhiên nằm trong miền hợp lệ nhưng không khớp địa chỉ thực tế. Database đã áp migration thành công sẽ bỏ qua bản sửa và giữ nguyên tọa độ.

Cần build lại từ bản sửa để có artifact SQL mới; chạy lại job cũ vẫn dùng SQL bị lỗi. Không thêm cột thủ công hoặc tự ghi migration vào `__EFMigrationsHistory`. Bản sửa backend ở commit `815e7ee` trên nhánh `develop`; việc push kích hoạt pipeline Dev nhưng chưa chứng minh migration hoặc deploy đã thành công.

Kiểm chứng bản sửa: chạy project `bmt-be.integration.tests` với bộ lọc `EstimateCoordinateMigrationTests|IdempotentMigrationScriptTests|EstimateCoordinateFlowTests` bằng `dotnet test --no-restore`, đạt 25 test, 0 lỗi, 0 bỏ qua trên PostgreSQL 15 tạm. Hai ca mới kiểm cả `MigrateAsync` và SQL idempotent với cờ Dev: giữ bản đang dùng, bản xóa mềm, version và receipt; tọa độ nằm trong miền; chạy lại giữ nguyên cặp đã lưu; schema vẫn NOT NULL, CHECK và không có default. Ca mặc định tiếp tục kiểm thiếu tọa độ thì rollback và có dữ liệu thì chặn Down. Log: `/private/tmp/bmt-coordinate-migration-fix-tests.log`.

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
