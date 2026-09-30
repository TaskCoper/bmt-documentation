# Bàn giao backend video hướng dẫn

Backend của GUIDE đã được triển khai theo [TDD-GUIDE-001](../tdd/TDD-GUIDE-001.md), tại [commit ce9d23a](https://github.com/TaskCoper/bmt-be/commit/ce9d23a), trên nhánh `feature/video-guides` từ `develop` tại `ae1bc8b`. Người dùng đã xác nhận chỉ làm backend. Đợt này chưa thay đổi trang `/vi/guide`, chưa làm màn hình admin và chưa triển khai dịch vụ lên môi trường dùng chung.

## Mã nguồn và dữ liệu

- `contract/services/guide`: command/query, DTO, chuẩn hóa NFC, đếm Unicode scalar, kiểm URL và giới hạn 200/2.000 ký tự.
- `application/usecases/commands/guide` và `queries/guide`: quản trị, đọc công khai, kiểm quyền `guide.manage`, trạng thái và phiên bản. Trả lỗi bằng exception để transaction rollback.
- `domain/entities/Guide.cs`, `GuideOrderState.cs` và `IGuideStore`: nội dung hướng dẫn, phiên bản thứ tự chung và cổng dữ liệu.
- `persistence/repositories/GuideStore.cs`: khóa dòng, snapshot đọc danh sách, tìm literal bằng ILike, lọc Published ngay trong SQL.
- `infrastructure/guide`: gọi YouTube bằng video ID; lấy ảnh/thời lượng, kiểm video; cache Redis tối đa 24 giờ. Timeout tổng 5 giây, mỗi lần chờ cache tối đa 250 ms; không tự retry request YouTube.
- `presentation/apis/guide`: 2 endpoint công khai và 9 endpoint quản trị. Ghi dùng cookie vẫn đi qua kiểm Origin hiện có. Response không được lưu cache HTTP.

Đường dẫn trên tính từ `src/` của repository backend. Các handler không lưu ảnh, thời lượng hay payload YouTube vào SQL. Sửa Published hiển thị ngay; nếu thay video không đạt thì giữ nguyên toàn bộ bản cũ. Lỗi phát sau xuất bản không tự làm hướng dẫn bị ẩn.

Migration `20260930004917_AddVideoGuides` tạo Guide, GuideOrderState, các CHECK/FK/index và thêm quyền cùng grant cho vai trò admin hệ thống. Vai trò tùy chỉnh không tự nhận quyền. Migration đã chạy trên PostgreSQL 15 trong container kiểm thử; chưa chạy trên database dùng chung.

## Tích hợp frontend

API dùng envelope Result hiện có. Contract đầy đủ nằm ở [Internal API của TDD](../tdd/TDD-GUIDE-001.md#internal-api).

| Việc cần làm | API |
|---|---|
| Xem danh sách/chi tiết công khai | GET `/api/v1/guides`, GET `/api/v1/guides/{guideId}` |
| Đọc danh sách/chi tiết quản trị | GET `/api/v1/admin/guides`, GET `/api/v1/admin/guides/{guideId}` |
| Tạo nháp | POST `/api/v1/admin/guides` |
| Thay toàn bộ ba trường nội dung | PUT `/api/v1/admin/guides/{guideId}` |
| Xuất bản, ẩn, di chuyển | POST `/api/v1/admin/guides/{guideId}/publish`, `/hide`, `/move` |
| Xóa nháp hoặc đã ẩn | DELETE `/api/v1/admin/guides/{guideId}?expectedVersion=...` |
| Lấy ảnh/thời lượng và kiểm video | POST `/api/v1/admin/guides/video-preview` |

PUT bỏ qua trường nào thì trường đó được lưu NULL. Mọi lệnh trên bản ghi hiện có cần `expectedVersion`; move cần thêm `expectedOrderVersion` lấy từ danh sách quản trị. Sau move phải tải lại danh sách vì phiên bản của nhiều dòng có thể đổi. Xung đột trả 409, không tự gửi lại lệnh với phiên bản mới.

Preview chỉ để hiển thị; publish luôn kiểm trực tiếp YouTube. `metadata=null` là trường hợp hợp lệ khi đọc hoặc khi phản hồi một thao tác không kiểm video. Frontend không dùng thời lượng giả, không lấy ảnh/thời lượng của video trước cho video mới. `videoWarning=YoutubeUnavailable` trên kết quả lưu nháp/ẩn nghĩa là link được lưu nhưng chưa kiểm được video.

## Cấu hình và đưa vào môi trường chạy

1. Bật YouTube Data API v3 cho Google project và cấu hình API key ở backend: `YoutubeOption__ApiKey`. Khi chạy Docker Compose, đặt `YOUTUBE_API_KEY` theo `.docker/.env.sample`; compose đã ánh xạ sang option. Workflow deploy cũng đọc secret `YOUTUBE_API_KEY`; thay đổi này chưa tạo hoặc cập nhật secret trên GitHub. Không đưa key vào frontend, URL hoặc log.
2. Chuẩn bị bản sao lưu, kiểm schema và phối hợp migration với bản code mới. PermissionCatalogGuard yêu cầu danh mục quyền trong code và database khớp nhau, nên code cũ có thể không khởi động sau khi seed quyền mới.
3. Áp dụng migration bằng quy trình triển khai hiện có, sau đó chạy bản backend mới. Không gọi tự động Migrate khi API khởi động.
4. Lấy token mới cho tài khoản được cấp quyền rồi kiểm preview, tạo nháp, xuất bản và đọc công khai bằng một video thật, một Shorts thật.

Nếu thiếu key, preview và publish trả 503 `YoutubeUnavailable`; nháp/ẩn vẫn được lưu URL đúng định dạng kèm cảnh báo. Đọc công khai vẫn trả nội dung SQL khi không lấy được metadata. Hành vi này không chứng minh credentials hoặc quota của môi trường đã được cấu hình đúng.

Nếu quay về code cũ, sao lưu grants rồi phối hợp gỡ các RolePermission/Permission GUIDE, giữ hai bảng nội dung. Không chạy Down một cách máy móc vì Down xóa hai bảng. Chưa thực hiện rollback trên môi trường dùng chung.

Tích hợp được đối chiếu với tài liệu chính thức [videos.list](https://developers.google.com/youtube/v3/docs/videos/list) và [Video resource](https://developers.google.com/youtube/v3/docs/videos). Chưa dùng credentials của BMT gọi YouTube thật.

## Kiểm chứng

Ngày kiểm tra: 30/09/2026. **123 ca GUIDE đạt, không bỏ qua**: 46 application, 24 infrastructure, 39 HTTP API và 14 PostgreSQL. Build API không lỗi/cảnh báo; EF xác nhận model không còn thay đổi chưa có migration. Lệnh EF dùng design-time factory sau khi host báo thiếu cấu hình JWT trong worktree; đây không phải bằng chứng API đã khởi động với cấu hình triển khai.

Kiểm thử hồi quy: application 1.425/1.425, infrastructure 187/187, persistence 17/17, integration 450/450 ở lượt toàn bộ. Sau đó thêm một ca Unicode, chạy lại toàn bộ GUIDE trên PostgreSQL đạt 14/14. Lượt API toàn bộ có 376 đạt và một lỗi thứ tự quyền trong Swagger; đã sửa rồi chạy lại nhóm GUIDE + Swagger đạt 44/44. Không chạy lại 333 ca API còn lại sau chỉnh thứ tự hằng quyền. Không có lỗi kiểm thử còn chưa xử lý trong các lượt kiểm tra này.

Ba lỗi tìm được ở lượt hồi quy đầu là danh sách quyền cố định, danh sách bảng cố định và thứ tự quyền Swagger. Đã bổ sung đúng `guide.manage`, Guide/GuideOrderState vào kỳ vọng, đồng thời giữ thứ tự hằng quyền trùng danh mục. Không bỏ hoặc nới kiểm tra.

File TRX và log của phiên kiểm tra nằm tại `/private/tmp/guide-test-results/`, `/private/tmp/guide-regression-*.log` và `/private/tmp/guide-final-*.log` trên máy thực hiện. Kiểm thử dùng `--no-restore`; không cài package hoặc đổi phiên bản framework.

| Mã kiểm thử | Nội dung được chứng minh | Đặc tả liên quan |
|---|---|---|
| `GuideRulesTests` | Chuẩn hóa, giới hạn, URL, phiên bản, phân trang, escape từ khóa | UT-GUIDE-001–008 |
| `GuideHandlerTests` | Luồng trạng thái, cập nhật nguyên khối, phiên bản, thứ tự, preview, đọc thiếu metadata | UT-GUIDE-020–042 |
| `GuideVideoTests` | Mapping YouTube, video không phù hợp, lỗi HTTP/timeout, cache/batch/TTL | UT-GUIDE-009–019 |
| `GuideApiTests` | Route thật, cookie/policy/CSRF, binding, 422, DTO công khai và no-store; MediatR giả | ST-GUIDE-002/003/043; UT-GUIDE-040 |
| `GuideFlowTests` | Migration, CHECK/FK, SQL tìm kiếm/lọc/phân trang, vòng đời, tranh chấp và rollback qua MediatR thật trên PostgreSQL | Phần backend của ST-GUIDE-035–041 và các luồng CRUD |

Đối chiếu này chỉ ra nhóm hành vi đã kiểm, không tuyên bố toàn bộ bước của 43 System Test đã chạy. PostgreSQL là container tạm; YouTube/cache dùng bản giả trong test. Bộ API chạy middleware và Carter thật nhưng thay MediatR để kiểm riêng biên HTTP. Bộ PostgreSQL chạy MediatR, transaction, repository và migration thật nhưng thay dịch vụ video.

Chưa kiểm trên trình duyệt, chưa kiểm phát video/Shorts thật và chưa kiểm Redis thật. UT-GUIDE-043/044, ST-GUIDE-024/033 cùng các bước giao diện nằm ngoài đợt backend. Chưa xác nhận toàn bộ luồng qua frontend hoặc môi trường triển khai.
