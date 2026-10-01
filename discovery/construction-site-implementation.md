# Triển khai backend hồ sơ công trình mở rộng

Ngày kiểm tra: 01/10/2026. Backend được triển khai theo TDD và nghiệp vụ đã chốt, tại `bmt-be-construction-sites`, nhánh `feature/construction-site-profile`, từ commit `b0d3793`. Worktree riêng giúp giữ nguyên các thay đổi tọa độ dự toán chưa commit trong `bmt-be`.

Đã thêm PdfPig **0.1.16** và ACadSharp **3.8.0**, rồi chạy restore theo xác nhận “Cho phép tải dependency” của người dùng. Code đã được commit và push lên `develop` ngày 01/10/2026; xem phần đồng bộ bên dưới. Chưa trực tiếp triển khai dịch vụ hoặc áp dụng migration lên database dùng chung trong tác vụ này.

## Phạm vi đã triển khai

| Phần | Hành vi |
|---|---|
| Hồ sơ công trình | Lưu diện tích đất, địa chỉ tỉnh/xã/số nhà–đường, ngân sách VND nguyên, thời điểm khởi công, hiện trạng và các lựa chọn từ catalog dùng chung. Diện tích tối đa hai chữ số thập phân; số tiền và diện tích trong API dùng chuỗi số. |
| Catalog | Ghim revision khi tạo. Chỉ bắt buộc lựa chọn thuộc nhóm có áp dụng; nhóm tắt hoặc không có lựa chọn dùng null. Phân biệt tum=false và null. |
| Hiện trạng | Seed ba mục đã chốt. Chỉ Admin có quyền tương ứng được tạo, đổi tên/thứ tự và ngừng cho chọn. Hồ sơ giữ tên đã lưu; mục ngừng chọn vẫn được giữ trên hồ sơ cũ. |
| Dự toán nguồn | Chỉ dự toán của khách, hoàn tất và chưa xóa/chưa được dùng. FK bảo vệ owner, revision và snapshot đầu vào. Hỗ trợ snapshot v1/v2; tọa độ công trình vẫn do luồng tạo cung cấp. Dữ liệu kế thừa bị khóa. |
| Xóa dự toán | Trả ConstructionSiteInUse khi đang làm nguồn công trình. Receipt mới dùng v2, vẫn đọc v1. Tạo công trình và xóa nguồn phối hợp cùng khóa hàng Estimate. |
| Tệp | Hai nhóm bản vẽ/ảnh hiện trạng; tối đa 9 tệp, mỗi tệp 10.000.000 byte. Kiểm ảnh bằng bộ giải mã, PDF/DWG/DXF bằng parser. Ticket gắn owner và đích; ticket đã dùng không thể dùng lại sau khi gỡ tệp. |
| Thay và xóa tệp | Thay tệp trong cùng transaction, giữ slot khi công trình đủ 9 tệp. Gỡ attachment/reference nhưng giữ object final riêng tư theo phạm vi retention đã chốt. |
| Đọc tệp | GET/HEAD và Range qua API có xác thực. Kiểm quyền công trình trước gọi storage; không trả URL đọc công khai. Chặn prefix riêng tư trong các luồng nhận URL ảnh công khai, kể cả mã hóa nhiều lớp và đường dẫn có `..`. |
| Khóa theo gói | Gói Assigned/Completed khóa hồ sơ, xóa công trình và thay đổi tệp. Giữ các quyền Customer/staff, phạm vi phân công và cơ chế version hiện có. |

Không bổ sung chức năng gợi ý hoặc truy vấn liên quan mới; dữ liệu được lưu để phục vụ các chức năng đó sau này.

## Mã nguồn và tài liệu đối chiếu

- Đã đọc nhóm [SITE-001](../tdd/TDD-SITE-001.md), [SITE-002](../tdd/TDD-SITE-002.md), [SITE-003](../tdd/TDD-SITE-003.md), [SITE-004](../tdd/TDD-SITE-004.md), [SITE-005](../tdd/TDD-SITE-005.md), cùng US/BR/UT/ST và phần PROJ/MEDIA/SUB được dẫn tới trước khi viết mã. [Bảng thiết kế](construction-site-technical-design.md) ghi phạm vi dùng chung và thứ tự triển khai.
- [ConstructionSiteProfileWriter](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.application/services/ConstructionSiteProfileWriter.cs): ghi hồ sơ độc lập hoặc từ nguồn, khóa trường nguồn và giữ revision.
- [ConstructionSiteEstimateSourceReader](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.application/services/ConstructionSiteEstimateSourceReader.cs): kiểm nguồn và đọc snapshot hoàn tất.
- [ConstructionSiteAttachmentWriter](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.application/services/ConstructionSiteAttachmentWriter.cs): tiêu thụ ticket, quản lý slot và reference trong transaction.
- [ConstructionSiteFilesApi](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.presentation/apis/constructionSite/ConstructionSiteFilesApi.cs): upload, attachment và nội dung riêng tư.
- [SiteDrawingInspector](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.infrastructure/files/SiteDrawingInspector.cs): kiểm cấu trúc PDF/DWG/DXF.

API hồ sơ v1 đã đổi contract như TDD-SITE-003: client cũ chỉ gửi tên/địa chỉ không thể tạo hồ sơ thiếu trường. Request có trường ngoài contract bị từ chối. Các route hỗ trợ form gồm create-options, catalog theo công trình, estimate-sources và hiện trạng đang cho chọn. Đây là phần backend phục vụ form; giao diện frontend chưa được sửa trong worktree này.

## Migration

[20261001101812_ExpandConstructionSiteProfiles](https://github.com/TaskCoper/bmt-be/blob/689e36e/src/bmt-be.persistence/Migrations/20261001101812_ExpandConstructionSiteProfiles.cs) tạo cấu trúc mới, seed hiện trạng, quyền Admin, FK/index/trigger, nới địa chỉ lịch sử và cho phép receipt v1/v2.

Up dừng nếu bảng ConstructionSite có dữ liệu. Không xóa dữ liệu, không điền giá trị giả để vượt qua kiểm tra. Down cũng dừng nếu đã có công trình hoặc ticket mới. Các kiểm tra này đã chạy trên PostgreSQL tạm; chưa chứng minh database triển khai thực tế đang rỗng.

FK ghép attachment → MediaObject được tạo trực tiếp bằng SQL để giữ SourceUploadId nullable cho object MEDIA cũ. Lý do và cách ánh xạ đã bổ sung trong TDD-SITE-005. Khi tạo migration sau này cần giữ FK này; EF model không tự quản lý FK ghép đó.

## Kết quả kiểm thử

| Bộ test | Kết quả thực thi |
|---|---|
| Application | 1.696 qua, 0 lỗi. |
| Infrastructure | 319 qua, 0 lỗi; gồm PDF hợp lệ, DWG/DXF do writer tạo và tệp chỉ giả header. |
| Persistence | 17 qua, 0 lỗi. |
| Domain | 1 qua, 0 lỗi. |
| API | 460 qua, 0 lỗi, 1 bỏ qua theo cấu hình test Google có sẵn. Chạy Carter, JWT, middleware và binding thật; nhiều handler/tích hợp là bản giả của test host. |
| PostgreSQL integration | Bộ đầy đủ 547 qua, 0 lỗi trên PostgreSQL 15 tạm; hai test tranh chấp nguồn bổ sung sau đó cũng qua 2/2. |

Build API cuối bằng `dotnet build --no-restore` thành công, 0 lỗi và 0 cảnh báo trong lượt build đó. `git diff --check` không báo lỗi; 55 liên kết file trong các tài liệu vừa cập nhật đều có đích.

Các lượt trên có **3.042 test qua và 1 test bỏ qua**, cộng theo bộ và hai test mới riêng biệt; không cộng lại các lần chạy lại cùng ca. Log thực thi ở `/private/tmp/site-app-green.log`, `/private/tmp/site-infra-green.log`, `/private/tmp/site-solution-rerun.log`, `/private/tmp/site-pg-green.log` và `/private/tmp/site-source-races.log`. Log solution trước đó còn ghi các lỗi fixture đã được sửa; kết quả Application/Persistence/Infrastructure/Integration cuối lấy từ các lượt chạy lại tương ứng.

Các bằng chứng trực tiếp cho tính năng nằm trong:

- [ConstructionSiteProfileTests](https://github.com/TaskCoper/bmt-be/blob/689e36e/test/bmt-be.application.tests/usecases/constructionSite/ConstructionSiteProfileTests.cs) và [ConstructionSitePayloadTests](https://github.com/TaskCoper/bmt-be/blob/689e36e/test/bmt-be.application.tests/usecases/constructionSite/ConstructionSitePayloadTests.cs): số, tính áp dụng, snapshot, đầu vào giả mạo, URL riêng tư và quá dung lượng thực.
- [ConstructionSiteProfileFlowTests](https://github.com/TaskCoper/bmt-be/blob/689e36e/test/bmt-be.integration.tests/ConstructionSiteProfileFlowTests.cs): hồ sơ lưu thật, revision cũ, hiện trạng, nguồn/xóa/receipt, 9 slot/thay tệp/rollback, owner/đích upload, quyền đọc trước storage và yêu cầu đồng thời.
- [ConstructionSiteMigrationTests](https://github.com/TaskCoper/bmt-be/blob/689e36e/test/bmt-be.integration.tests/ConstructionSiteMigrationTests.cs): chặn migration khi có công trình cũ, giữ nguyên dữ liệu và schema cũ.
- [ConstructionSiteApiPipelineTests](https://github.com/TaskCoper/bmt-be/blob/689e36e/test/bmt-be.api.tests/security/ConstructionSiteApiPipelineTests.cs): JSON sai/ngoài contract trả 422 trước dispatch; GET/HEAD tệp từ người chưa đăng nhập trả 401.

Các test cũ được cập nhật cho hồ sơ bắt buộc, địa chỉ ghép, bảng mới, quyền mới và receipt v2. Test migration cũ chèn/đọc đúng cột của phiên bản mà chúng kiểm tra. Lượt hồi quy còn phát hiện lỗi đăng ký pipeline địa chỉ quá rộng; đã sửa để chỉ chạy với lệnh tạo/sửa công trình.

## Giới hạn còn lại trước phát hành

- Chưa sửa hoặc chạy trình duyệt trên form frontend. Không đánh dấu 105 đặc tả System Test đã chốt là toàn bộ đã qua; báo cáo này chỉ ghi kết quả test mã nguồn backend đã thực thi, không thay bảng đối chiếu từng UT/ST.
- Chưa kiểm kho BizFly/CDN thật, chính sách bucket chặn đọc ẩn danh, truyền bytes GET/HEAD/Range qua môi trường triển khai hoặc lỗi mạng giữa chừng. Test quyền đọc PostgreSQL dùng storage giả; parser test dùng fixture cục bộ. Cần kiểm các điểm tích hợp thật trước phát hành tính năng tệp riêng tư.
- Trước áp dụng migration cần kiểm dữ liệu thực tế, dừng writer cũ và phối hợp phát hành client theo contract mới. Chưa chạy migration trên database dùng chung.
- Không đổi trạng thái phê duyệt tài liệu, không import Document First và không tuyên bố đã kiểm đủ mọi đặc tả chỉ từ số lượng test qua.

## Đồng bộ lên develop và main

Theo yêu cầu người dùng ngày 01/10/2026, code công trình đã được tích hợp với `origin/develop` tại mốc `815e7ee`, gồm phần tọa độ dự toán đã có trên nhánh. Commit tính năng là `e5bd345`; commit tích hợp và đã push lên `develop` là [689e36e](https://github.com/TaskCoper/bmt-be/commit/689e36e).

Sau tích hợp, đã đồng bộ target model của migration SITE với hai cột tọa độ Estimate. Kiểm tra lại đạt 1.735/1.735 test Application, 136/136 test PostgreSQL cho công trình/dự toán/gỡ gói và 31/31 test API công trình/tọa độ/Swagger. Các lượt này kiểm trên code sau merge; không cộng với số ca trước merge để báo tổng mới. Log ở `/private/tmp/site-push-app.log`, `/private/tmp/site-push-pg.log` và `/private/tmp/site-push-api.log`.

Bộ tài liệu đưa lên `main` gồm 261 file liên quan công trình và các điểm tích hợp PROJ/MEDIA/SUB. Đã kiểm 1.229 liên kết file, không có đích thiếu; `git diff --cached --check` đạt. Phần CDN và tạo nhanh dự toán đang được chỉnh riêng trong workspace không nằm trong lần đồng bộ này. Không đổi trạng thái phê duyệt hay import tài liệu.
