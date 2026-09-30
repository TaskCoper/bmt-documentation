# Triển khai danh sách và xóa nhiều dự toán

Backend đã được triển khai theo TDD-PROJ-004/005 trên nhánh `feature/my-estimates`, tách từ commit `54af5f4`. Mã nằm trong worktree `bmt-be-my-estimates`; các thay đổi đang làm ở kiến trúc sư và tin tức trong `bmt-be` được giữ nguyên. Đã push và merge vào `develop` qua [PR #10](https://github.com/TaskCoper/bmt-be/pull/10), commit `2eff777817b1fec57b3d7853cc57be2123b9bcc9`, ngày 30/09/2026. Chưa xác nhận triển khai dịch vụ.

## Hành vi đã bổ sung

- `GET /api/v1/estimates`: chỉ trả dự toán chưa xóa của Customer đang đăng nhập, gồm Draft, Processing, Succeeded, Failed. Tìm theo tên không phân biệt hoa/thường và dấu, gồm Đ/đ; lọc trạng thái, phân trang và sắp theo lần sửa gần nhất. Bản nháp chưa chọn loại vẫn xuất hiện. Tổng số và trang dữ liệu dùng cùng snapshot Repeatable Read.
- `POST /api/v1/estimates/bulk-delete`: nhận rõ 1–100 Id trước loại trùng, xử lý từng bản đủ điều kiện và trả Deleted/AlreadyDeleted/Processing/NotFound. Không xóa bản còn Pending, kể cả đã quá deadline. Chỉ gửi các Id được chọn trên trang đang xem; backend không tự mở rộng theo bộ lọc.
- Cùng key và cùng tập Id trả lại đúng biên nhận đã commit; key mới mới kiểm lại bản từng bị chặn. Cả lô và biên nhận dùng một transaction. Gói hoặc số dư không chặn xóa; không hoàn lượt và không khôi phục.
- Bản đã xóa bị chặn ở chi tiết, catalog, trạng thái AI, đổi tên, lưu, replay tạo/gửi AI, hồ sơ, link/QR, export, email và các đường tải owner/public. Lịch sử vẫn được giữ. Email chưa gửi chuyển Skipped; export chưa làm dừng trước I/O, hoặc chuyển Failed/EstimateDeleted nếu xóa xảy ra khi đang chuẩn bị tệp.

## Những điểm kỹ thuật cần biết

`LockOwnedActiveEstimateAsync` dành cho thao tác sản phẩm và kiểm dấu xóa ngay trong câu SQL khóa hàng. `LockOwnedEstimateAsync`/`LockEstimateAsync` vẫn đọc lịch sử cho xóa lặp, finalizer và bước hoàn tất export. Không thêm global query filter hoặc xóa dây chuyền.

`EstimateStore.MyEstimates.cs` thực hiện projection có tham số; không tải toàn bộ bản dự toán hoặc payload AI để tìm kiếm. PostgreSQL `normalize`/`unaccent`/`lower`/`strpos` xử lý tìm chuỗi con; `%`, `_` và `\` là ký tự thường. Không thêm index tìm kiếm khi chưa có số liệu tải đại diện.

Endpoint kiểm tham số lặp/lạ trước khi chuyển chuỗi trang sang Int32 để giữ mã 422 cho tham số lặp, 400 cho binding số lỗi. OpenAPI vẫn khai pageIndex/pageSize là integer và bốn giá trị state hợp lệ.

Sau khi đồng bộ develop ngày 30/09/2026, migration dự toán được đặt sau `20260930004917_AddVideoGuides`. Migration `20260930015648_MyEstimates` thêm `Estimate.DeletedAtUtc`, bảng `EstimateDeletionReceipt` và extension unaccent ở public. Dữ liệu cũ có dấu xóa NULL. Migration dừng nếu encoding không phải UTF8 hoặc unaccent đã ở schema khác. Down bị chặn sau khi có dấu xóa hoặc receipt; giữ extension khi hạ schema. Model dùng metadata schema mặc định theo cách Npgsql 8 sinh snapshot, còn migration kiểm và cài extension rõ ở public.

## Kiểm chứng

[CI trước merge](https://github.com/TaskCoper/bmt-be/actions/runs/36657744965) đã build thành công và qua toàn bộ **2.523 kiểm thử**, không bỏ qua ca nào: domain 1, persistence 17, infrastructure 187, application 1.455, integration 469 và API 394.

Sau khi hợp nhất thay đổi hướng dẫn video từ develop, 97 kiểm thử PostgreSQL cho dự toán và hướng dẫn đều qua. CI lần đầu phát hiện test danh sách bảng thiếu EstimateDeletionReceipt; đã bổ sung bảng này vào hợp đồng model.

Đã qua **455 kiểm thử** trong phạm vi dự toán: 324 ở tầng ứng dụng, 48 qua HTTP và 83 trên PostgreSQL; không có ca bị bỏ qua. Đây là tổng số ca mới và ca hồi quy được chọn theo phạm vi, không phải 455 ca mới. Các project được build bằng `--no-restore`, không có lỗi hoặc cảnh báo trong các lượt kiểm cuối. `dotnet ef migrations has-pending-model-changes --no-build` xác nhận model khớp snapshot. Kiểm cấu trúc, tham chiếu và hash tài liệu cũng đã qua.

Các lệnh chạy từ `bmt-be-my-estimates`:

```sh
dotnet test test/bmt-be.application.tests/bmt-be.application.tests.csproj --no-restore -m:1 --filter 'FullyQualifiedName~estimate'
dotnet test test/bmt-be.api.tests/bmt-be.api.tests.csproj --no-restore -m:1 --filter 'FullyQualifiedName~MyEstimates|FullyQualifiedName~Estimate'
BMT_REQUIRE_DOCKER_TESTS=1 dotnet test test/bmt-be.integration.tests/bmt-be.integration.tests.csproj --no-restore -m:1 --filter 'FullyQualifiedName~Estimate'
```

Trong lần chạy này, test runner cần quyền mở socket ngoài sandbox. PostgreSQL do Testcontainers tạo và dọn; database dev đang chạy không được dùng làm đích kiểm thử.

| Phạm vi đặc tả | Bằng chứng mã test |
|---|---|
| UT-PROJ-072–085: phân trang, Unicode, query, quyền, projection, lỗi đọc, đầu vào và hash | `MyEstimatesTests`, `MyEstimatesApiTests`; `MyEstimatesFlowTests.List_FiltersOwnerDeletedSearchAndStateBeforeStablePaging` |
| UT-PROJ-086–101: lô hỗn hợp, Pending quá hạn, quyền sở hữu, replay, lỗi SQL, cờ mở tính năng | `MyEstimatesTests`; `MyEstimatesFlowTests` cho lô hỗn hợp, key đồng thời, tranh chấp AI/xóa và rollback receipt |
| UT-PROJ-102–112: chặn truy cập/replay, owner/public, email và tệp | `MyEstimatesFlowTests.DeletedDraft_CreateAndSaveReplayCannotReviveIt`, `Delete_DeniesAllOwnerPublicAndReplayPaths_QueuedWorkersStop`; ca chờ khóa trong `EstimateExportCommandTests` |
| UT-PROJ-113–117: worker, terminal history, transaction pipeline | `EstimateSharingWorkerTests.Handle_DeletedEstimate_FailsWithoutExternalIo`; `MyEstimatesFlowTests.Export_CompletionAfterDeletionFailsAndCannotOverwriteLostLease`; `MyEstimatesTransactionTests` |
| Snapshot, khóa thật, ràng buộc và migration | `MyEstimatesStorageTests`: dữ liệu đổi giữa count/page, đọc lại dấu xóa sau chờ khóa, ràng buộc receipt/timestamp và nâng/hạ schema cũ |

Đây là bảng dẫn tới các nhóm hành vi đã kiểm; không đồng nghĩa mọi dòng trong 46 đặc tả UT đã được chạy thành một ca độc lập. Các ca SQL, khóa, rollback và migration dùng PostgreSQL 15 trong container tạm. Test HTTP dùng pipeline thật với MediatR giả; test tích hợp gọi MediatR thật và database thật, dùng dịch vụ AI/tệp/SMTP giả. Không gửi thư hoặc gọi AI thật.

ST-PROJ-078–109 còn phần giao diện chưa được nghiệm thu: chọn trang, hộp xác nhận, trạng thái tải/lỗi, đổi bộ lọc và xử lý phản hồi đến muộn. Workspace không có frontend để triển khai hoặc chạy luồng trình duyệt trong đợt này. Chưa kiểm tải lớn, SLA, kế hoạch truy vấn trên dữ liệu sản xuất hoặc chạy toàn bộ stack qua mạng.

## Mở tính năng

1. Chạy migration bằng quy trình triển khai của môi trường sau khi kiểm quyền cài extension và thời gian khóa bảng.
2. Triển khai đủ API và worker có kiểm DeletedAtUtc. Giữ `ESTIMATE_CUSTOMER_DELETION_ENABLED=false` trong thời gian chuyển đổi.
3. Sau khi mọi instance cũ đã dừng, bật `ESTIMATE_CUSTOMER_DELETION_ENABLED=true` (ánh xạ sang `EstimateOption__CustomerDeletionEnabled`). Danh sách không phụ thuộc cờ tạo hoặc xóa.
4. Nếu cần dừng xóa, tắt cờ nhưng vẫn giữ phiên bản code hiểu dấu xóa. Không quay lại code cũ hoặc bỏ cột/receipt sau lần xóa đầu tiên.

Cấu hình đã có trong hai compose, `.docker/.env.sample` và workflow. Chưa bật cờ hoặc chạy migration lên database đang phục vụ.

## Tài liệu liên quan

- [TDD danh sách](../tdd/TDD-PROJ-004.md), [TDD xóa](../tdd/TDD-PROJ-005.md).
- [Đặc tả Unit Test](my-estimates-unit-test-coverage.md), [độ phủ System Test](my-estimates-system-test-coverage.md).
- [API](../../bmt-be-my-estimates/src/bmt-be.presentation/apis/estimate/EstimateApi.cs), [xử lý xóa](../../bmt-be-my-estimates/src/bmt-be.application/usecases/commands/estimate/DeleteMyEstimatesCommandHandler.cs), [truy vấn và ghi dữ liệu](../../bmt-be-my-estimates/src/bmt-be.persistence/repositories/EstimateStore.MyEstimates.cs).
- [Kiểm thử PostgreSQL](../../bmt-be-my-estimates/test/bmt-be.integration.tests/MyEstimatesFlowTests.cs), [kiểm chứng schema và khóa](../../bmt-be-my-estimates/test/bmt-be.integration.tests/MyEstimatesStorageTests.cs).
