# Đặc tả Unit Test cho danh sách và xóa nhiều dự toán

**Cập nhật triển khai 30/09/2026:** backend đã được triển khai theo yêu cầu “bắt đầu chiến”. Xem [báo cáo triển khai](my-estimates-implementation.md). Các đoạn nói “chưa viết/chạy” bên dưới ghi nhận thời điểm thiết kế ban đầu; kết quả kiểm chứng hiện tại nằm trong báo cáo mới.

Người dùng xác nhận “ok chốt” bộ đặc tả và bảng độ phủ trong hội thoại ngày 30/09/2026: gồm UT-PROJ-072–117 mới, phần cập nhật UT-PROJ-015/021/044/051 và phạm vi kiểm chứng tích hợp bên dưới. Xác nhận này kết thúc bước thiết kế tài liệu, chưa phải yêu cầu triển khai mã ứng dụng hoặc mã test.

Người dùng đã chốt TDD-PROJ-004 và TDD-PROJ-005 trong hội thoại ngày 30/09/2026. Đã soạn 46 đặc tả mới UT-PROJ-072–117 và bổ sung điều kiện bản chưa xóa cho bốn ca replay hiện có: UT-PROJ-015/021/044/051. Chưa viết mã test, chạy ứng dụng, chạy test, sinh migration hoặc import tài liệu. Status Draft và các trường Version/Updated At/Change Log của TDD vẫn theo mẫu; xác nhận trong hội thoại không tự phê duyệt trên hệ thống.

Reviewer và Approver tiếp tục là Tân Trần theo bộ tài liệu đã thống nhất. Owner/người thực thi chưa được cung cấp, giữ [Chưa xác định]. Không thay quyết định nghiệp vụ hoặc hợp đồng API của hai TDD đã chốt.

## Mốc TDD được chốt

Hash dưới đây là nội dung người dùng đã chốt trước khi bổ sung ghi nhận xác nhận và liên kết tới bộ UT. Thay đổi sau mốc này chỉ ghi nhận trạng thái trong hội thoại và truy vết, không đổi API, schema, thuật toán hoặc quy tắc xử lý.

| TDD | SHA-256 tại lúc chốt |
|---|---|
| [TDD-PROJ-004](../tdd/TDD-PROJ-004.md) | `28a22a6b9e76f0684b395aaa7693489ba2ec509a5245acb1930d230fd4c9570e` |
| [TDD-PROJ-005](../tdd/TDD-PROJ-005.md) | `2a041b71ed68e13be3b4b930664ed7b512184406a593c687a438f261531dad57` |

## Danh mục đặc tả mới

Tên unit mới hoặc phần sửa là mục tiêu triển khai theo TDD, chưa được mô tả như code đã tồn tại. Với ca gọi nhiều handler, mỗi biến thể phải gọi handler tương ứng trong fixture riêng; kiểm helper chung không thay thế kiểm caller. Các giá trị expected độc lập với helper đang được test; ví dụ hash ở UT-PROJ-085 là hằng của fixture, không tính lại bằng mã production.

| Đặc tả | Nhóm | Unit và hành vi chính |
|---|---|---|
| [UT-PROJ-072](../unittest/UT-PROJ-072.md) | Danh sách | GetMyEstimatesQueryValidator — biên phân trang |
| [UT-PROJ-073](../unittest/UT-PROJ-073.md) | Danh sách | GetMyEstimatesQueryValidator — offset không tràn số |
| [UT-PROJ-074](../unittest/UT-PROJ-074.md) | Danh sách | GetMyEstimatesQueryValidator / xử lý search — trim và Unicode scalar |
| [UT-PROJ-075](../unittest/UT-PROJ-075.md) | Danh sách | GetMyEstimatesQueryValidator — trạng thái hợp lệ |
| [UT-PROJ-076](../unittest/UT-PROJ-076.md) | Danh sách | EstimateApi — phần đọc query mới |
| [UT-PROJ-077](../unittest/UT-PROJ-077.md) | Danh sách | GetMyEstimatesQueryHandler — dùng tài khoản từ phiên và chuyển bộ lọc |
| [UT-PROJ-078](../unittest/UT-PROJ-078.md) | Danh sách | GetMyEstimatesQueryHandler — không có Customer hợp lệ |
| [UT-PROJ-079](../unittest/UT-PROJ-079.md) | Danh sách | GetMyEstimatesQueryHandler / Response — giữ dòng nháp chưa chọn loại |
| [UT-PROJ-080](../unittest/UT-PROJ-080.md) | Danh sách | GetMyEstimatesQueryHandler / PagedResult — rỗng và trang vượt cuối |
| [UT-PROJ-081](../unittest/UT-PROJ-081.md) | Danh sách | GetMyEstimatesQueryHandler — lỗi đọc không thành trang rỗng |
| [UT-PROJ-082](../unittest/UT-PROJ-082.md) | Xóa nhiều | DeleteMyEstimatesCommandValidator — số lượng trước loại trùng |
| [UT-PROJ-083](../unittest/UT-PROJ-083.md) | Xóa nhiều | DeleteMyEstimatesCommandValidator — GUID rỗng không bị bỏ qua |
| [UT-PROJ-084](../unittest/UT-PROJ-084.md) | Xóa nhiều | DeleteMyEstimatesCommandValidator / đọc key — biên Idempotency-Key |
| [UT-PROJ-085](../unittest/UT-PROJ-085.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — hash tập Id chuẩn hóa |
| [UT-PROJ-086](../unittest/UT-PROJ-086.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — xử lý lô hỗn hợp |
| [UT-PROJ-087](../unittest/UT-PROJ-087.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — tất cả Pending, kể cả quá deadline |
| [UT-PROJ-088](../unittest/UT-PROJ-088.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — đã xóa không đổi dấu thời gian |
| [UT-PROJ-089](../unittest/UT-PROJ-089.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — ẩn sự khác biệt bản lạ và không tồn tại |
| [UT-PROJ-090](../unittest/UT-PROJ-090.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — không mở rộng tập được chọn |
| [UT-PROJ-091](../unittest/UT-PROJ-091.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — gói không chặn, lịch sử giữ nguyên |
| [UT-PROJ-092](../unittest/UT-PROJ-092.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — đọc lại sau khóa trước quyết định |
| [UT-PROJ-093](../unittest/UT-PROJ-093.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — replay giữ kết quả lúc yêu cầu đầu |
| [UT-PROJ-094](../unittest/UT-PROJ-094.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — key trùng, tập khác |
| [UT-PROJ-095](../unittest/UT-PROJ-095.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — cùng key ở hai tài khoản |
| [UT-PROJ-096](../unittest/UT-PROJ-096.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — key mới đánh giá lại sau AI |
| [UT-PROJ-097](../unittest/UT-PROJ-097.md) | Xóa nhiều | EstimateStore — phần kiểm cấu trúc receipt trước lưu (dự kiến) |
| [UT-PROJ-098](../unittest/UT-PROJ-098.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — UPDATE bất ngờ không tác động hàng |
| [UT-PROJ-099](../unittest/UT-PROJ-099.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — lỗi store giữa lô được truyền ra |
| [UT-PROJ-100](../unittest/UT-PROJ-100.md) | Xóa nhiều | EstimateApi / cổng xóa — tính năng chưa bật |
| [UT-PROJ-101](../unittest/UT-PROJ-101.md) | Xóa nhiều | DeleteMyEstimatesCommandHandler — chặn người gọi không phải Customer |
| [UT-PROJ-102](../unittest/UT-PROJ-102.md) | Chặn truy cập | SaveEstimateInputCommandHandler — bản đã xóa chặn trước replay |
| [UT-PROJ-103](../unittest/UT-PROJ-103.md) | Chặn truy cập | RequestEstimateGenerationCommandHandler — xóa chặn yêu cầu mới và replay |
| [UT-PROJ-104](../unittest/UT-PROJ-104.md) | Chặn truy cập | CreateEstimateCommandHandler — receipt trỏ bản đã xóa |
| [UT-PROJ-105](../unittest/UT-PROJ-105.md) | Chặn truy cập | RenameEstimateCommandHandler — nhánh UPDATE=0 sau xóa |
| [UT-PROJ-106](../unittest/UT-PROJ-106.md) | Chặn truy cập | GetEstimateQueryHandler / GetEstimateGenerationQueryHandler / query catalog — bản đã xóa |
| [UT-PROJ-107](../unittest/UT-PROJ-107.md) | Chặn truy cập | EstimateSharingSupport.RequireOwnerAsync — bản đã xóa giống không thuộc chủ |
| [UT-PROJ-108](../unittest/UT-PROJ-108.md) | Chặn truy cập | EstimateSharingSupport.RequireGrantAsync — share của bản xóa không có grant |
| [UT-PROJ-109](../unittest/UT-PROJ-109.md) | Chặn truy cập | Các handler owner share/export/email — guard trước replay |
| [UT-PROJ-110](../unittest/UT-PROJ-110.md) | Chặn truy cập | RequestSharedEstimateExportCommandHandler — kiểm grant lại sau khóa |
| [UT-PROJ-111](../unittest/UT-PROJ-111.md) | Chặn truy cập | Các query tải tệp owner/public — không mở URL khi bản xóa |
| [UT-PROJ-112](../unittest/UT-PROJ-112.md) | Worker | EstimateShareEmailDispatcher — queued email của bản đã xóa |
| [UT-PROJ-113](../unittest/UT-PROJ-113.md) | Worker | EstimateExportPreparer — bản xóa trước I/O |
| [UT-PROJ-114](../unittest/UT-PROJ-114.md) | Worker | EstimateExportPreparer — kết quả hoàn tất sau I/O |
| [UT-PROJ-115](../unittest/UT-PROJ-115.md) | Worker | FinalizeEstimateGenerationCommandHandler — kết quả đến muộn của bản đã xóa |
| [UT-PROJ-116](../unittest/UT-PROJ-116.md) | Xóa nhiều | TransactionPipelineBehavior với DeleteMyEstimatesCommand — chỉ trả sau commit |
| [UT-PROJ-117](../unittest/UT-PROJ-117.md) | Danh sách | TransactionPipelineBehavior với GetMyEstimatesQuery — không bọc transaction ghi |

## Truy vết tiêu chí nghiệm thu

| Story / AC | Quy tắc / thiết kế | Unit Test tương ứng | Cần kiểm ở cấp cao hơn |
|---|---|---|---|
| STORY-PROJ-006 AC-001 | BR-PROJ-008 khoản 1–2; 004 Architecture | 077, 078, 079 | SQL owner/chưa xóa/đủ bốn trạng thái: ST-PROJ-078/079/090/091. |
| STORY-PROJ-006 AC-002 | BR-PROJ-008 khoản 3; 004 Data Model | 079 | LEFT JOIN/revision và hiển thị: ST-PROJ-078/090. |
| STORY-PROJ-006 AC-003 | BR-PROJ-008 khoản 4; 004 Internal API | 072, 073, 076, 080 | Sắp cả tập trước phân trang, tie Id và snapshot: ST-PROJ-080, bổ sung kiểm PostgreSQL dưới đây. |
| STORY-PROJ-006 AC-004 | BR-PROJ-008 khoản 5; 004 Data Model | 074, 075, 077 | Bỏ dấu/đ, Unicode, ký tự literal, lọc kết hợp: ST-PROJ-081/082/083 và kiểm SQL dưới đây. |
| STORY-PROJ-006 AC-005 | BR-PROJ-008 khoản 6 | 080 | Tập rỗng và UI: ST-PROJ-084/085. |
| STORY-PROJ-006 AC-006 | BR-PROJ-008 khoản 7, BR-SUB-007 | 077, 117 | Không đổi dữ liệu/gói/lượt: ST-PROJ-086. |
| STORY-PROJ-006 AC-007 | BR-PROJ-008 khoản 8 | 106, 107 | Mở bản và quyền sửa hiện hành: ST-PROJ-087. |
| STORY-PROJ-006 AC-008 | BR-PROJ-008 khoản 9, BR-PROJ-009 | 106–111 | Không còn trong SQL, mọi route cũ bị chặn: ST-PROJ-091/101/109. |
| STORY-PROJ-006 AC-009 | BR-PROJ-008 Except, BR-RBAC-005 | 078 | HTTP session/role: ST-PROJ-088/089. |
| STORY-PROJ-006 AC-010 | BR-PROJ-008 khoản 6 | 081 | UI lỗi/retry/search và phản hồi cũ đến muộn: ST-PROJ-092, kiểm UI khi triển khai. |
| STORY-PROJ-007 AC-001 | BR-PROJ-009 khoản 2, 12 | 090 | Chọn tất cả chỉ trang đang xem và thông báo hậu quả: ST-PROJ-094. |
| STORY-PROJ-007 AC-002 | BR-PROJ-009 khoản 3 | 086, 091 | Dấu xóa và lịch sử giữ nguyên trong DB: ST-PROJ-093. |
| STORY-PROJ-007 AC-003 | BR-PROJ-009 khoản 5 | 086, 116 | Commit lô hỗn hợp: ST-PROJ-095, PostgreSQL riêng. |
| STORY-PROJ-007 AC-004 | BR-PROJ-009 khoản 4, 11 | 087 | Pending và quota thật: ST-PROJ-096. |
| STORY-PROJ-007 AC-005 | BR-PROJ-009 khoản 9 | 091 | Hết hạn/hết lượt: ST-PROJ-097/098. |
| STORY-PROJ-007 AC-006 | BR-PROJ-009 khoản 10, BR-SUB-003 | 091, 115 | So Used/Reserved/lịch sử trước–sau: ST-PROJ-100. |
| STORY-PROJ-007 AC-007 | BR-PROJ-009 khoản 6–7 | 088, 102–107, 109, 111 | Bộ lọc và mọi route/replay: ST-PROJ-101/109. |
| STORY-PROJ-007 AC-008 | BR-PROJ-009 khoản 8, Except; BR-PROJ-006/007 Except | 108, 110–114 | Link/QR/email/file/Range và stream đang chạy: ST-PROJ-102, kiểm worker đồng thời. |
| STORY-PROJ-007 AC-009 | BR-PROJ-009 khoản 1, 5–6 | 088, 089, 095 | Truy vấn owner và không lộ dữ liệu: ST-PROJ-103. |
| STORY-PROJ-007 AC-010 | BR-PROJ-009 khoản 4, 11 | 092, 103 | Hai thứ tự AI/xóa dưới khóa thật: ST-PROJ-106/107. |
| STORY-PROJ-007 AC-011 | BR-PROJ-009 khoản 1 | 101 | Auth, role, CSRF: ST-PROJ-104/105 và HTTP contract. |
| STORY-PROJ-007 AC-012 | BR-PROJ-009 khoản 13 | 093, 098, 099, 116 | Lỗi SQL/commit/mất response: ST-PROJ-108 và kiểm tích hợp. |

Các số UT rút gọn trong bảng đều thuộc UT-PROJ. Những ca kỹ thuật bổ sung 082–085, 094, 096–100 kiểm hợp đồng đầu vào, hash, receipt, lỗi kỹ thuật và cờ triển khai của TDD-PROJ-005; không tự tạo thêm AC. Các ALT/EXC tiếp tục theo [bảng độ phủ ST](my-estimates-system-test-coverage.md).

## Những phần không thể chứng minh bằng Unit Test

Các ST-PROJ-078–109 hiện có vẫn là đặc tả chưa chạy. Khi triển khai, cần bổ sung fixture và kỳ vọng API cụ thể theo TDD đã chốt; không coi các mục dưới đây đã có mã test chỉ vì được liệt kê.

| Phạm vi tích hợp / hệ thống | Bằng chứng cần có |
|---|---|
| SQL danh sách | Hai owner, bản đã xóa, nháp chưa chọn loại, catalog ghim khác hiện hành, lịch sử Failed/TimedOut rồi Pending/Succeeded; count và items cùng phạm vi, sort toàn tập và Id ASC khi bằng mốc sửa. |
| Tìm tên PostgreSQL | Với unaccent thật: nha/NHÀ/nhà dạng tổ hợp tìm Nhà; dong tìm ĐÔNG; I/i; dấu %, _, gạch chéo ngược không là wildcard; từ khóa chỉ còn rỗng sau chuẩn hóa trả rỗng; input dạng SQL injection không mở rộng phạm vi. Tên gốc không đổi. |
| Snapshot đọc | Chèn/xóa/đổi trạng thái giữa COUNT và lấy trang bằng hai connection có barrier; một response phải dùng cùng snapshot Repeatable Read. Không yêu cầu snapshot xuyên các lần chuyển trang. |
| HTTP | Binding số/GUID/JSON sai kiểu trả 400; query lặp/lạ, body lạ, key sai và biên validator đúng 422; state/search/default đúng; auth 401/403; CSRF; private,no-store; DTO và các mã lỗi khớp. |
| Xóa và quota | Lô hỗn hợp commit dấu xóa/receipt cùng nhau; Name/InputVersion/NameVersion/ModifiedAtUtc/CurrentShareId và lịch sử giữ nguyên; Used/Reserved không đổi. Khác owner không được cập nhật. |
| Khóa AI/xóa | Hai transaction thật, kiểm cả thứ tự AI commit trước và xóa commit trước, gồm replay generation key cũ. Không dùng delay ngẫu nhiên thay barrier. |
| Key và commit | Hai yêu cầu cùng owner/key đồng thời; cùng key khác hash; response mất sau commit; SQL lỗi trước commit. Đọc DB để chứng minh không có receipt thiếu hàng xóa hoặc báo Deleted sai. |
| Ràng buộc receipt | PK, FK User RESTRICT, unique ActorId/RequestKey, hash, ResponseVersion, JSON array/độ dài; kết quả chứa Id lạ không có FK Estimate nhưng không tiết lộ dữ liệu. |
| Guard sản phẩm | Gọi mọi route đọc/ghi/replay owner/public sau xóa, gồm create replay, catalog, status AI, share, QR, email, export, result file; không dựa vào mỗi helper được test. GET/HEAD/Range mới không mở kết nối tệp. |
| Worker export | Xóa trước I/O, trong I/O và cạnh tranh với transaction hoàn tất: khóa Estimate trước export, đúng lease, Ready trước xóa giữ lịch sử nhưng không tải được; Deleted sau I/O ghi Failed/EstimateDeleted, không URL. |
| Email/stream đang chạy | Queued kiểm sau xóa thì Skipped; SMTP đã được cấp quyền trước xóa có thể Accepted, link vẫn không mở được. Stream đã cấp quyền có thể kết thúc; yêu cầu mới bị chặn. Không báo test thất bại chỉ vì bytes đã tải không bị thu hồi. |
| Migration/triển khai | Từ schema cũ, toàn bộ DeletedAtUtc mới NULL, receipt rỗng; extension/locale đúng; cờ xóa đóng đến khi mọi reader/worker đã có guard. Sau lần xóa đầu không quay về code làm lộ dữ liệu. Diễn tập backup theo giới hạn trong TDD. |
| Giao diện | Loading/rỗng/lỗi, giữ filter khi retry, reset trang khi đổi filter, bỏ response cũ, chọn trang hiện tại, kết quả từng Id, thông báo mất quyền chia sẻ, về trang hợp lệ sau xóa. Không tự quyết việc giữ lựa chọn thủ công qua trang. |

## Kiểm tra tài liệu và giới hạn

Bộ mới chỉ có đặc tả, không có kết quả Pass/Fail. Kiểm tra cục bộ mã không trùng, comment mẫu, một dòng/13 cột, enum, Reviewer/Approver, liên kết trực tiếp và Trace to khớp TEST_LINKS. Mỗi expected nêu dữ liệu/lỗi cần khẳng định và tác động không được phát sinh. Không chạy dotnet test cho thay đổi tài liệu.

Đã dùng khảo sát code/TDD từ bước thiết kế và đọc thêm pipeline, quyền truy cập, các ca replay chịu ảnh hưởng. Phạm vi audit đệ quy toàn hệ thống vẫn có giới hạn được ghi ở [bản tổng hợp kỹ thuật](my-estimates-technical-design.md); bộ này không tuyên bố đã đọc/xác minh mọi tài liệu trong đồ thị 726 tài liệu. Chưa kiểm parser import trên ứng dụng quản lý tài liệu.
