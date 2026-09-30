# Thiết kế danh sách và xóa nhiều dự toán của tôi

**Cập nhật triển khai 30/09/2026:** backend đã được triển khai theo yêu cầu “bắt đầu chiến”. Xem [báo cáo triển khai](my-estimates-implementation.md). Các đoạn nói “chưa viết/chạy” bên dưới ghi nhận thời điểm thiết kế ban đầu; kết quả kiểm chứng hiện tại nằm trong báo cáo mới.

Đã hoàn thiện nội dung hai TDD để review theo yêu cầu “lên TDD”: [TDD-PROJ-004](../tdd/TDD-PROJ-004.md) cho danh sách và [TDD-PROJ-005](../tdd/TDD-PROJ-005.md) cho xóa nhiều. Người dùng đã chốt cả hai trong hội thoại ngày 30/09/2026; metadata vẫn Draft theo mẫu nhập. Đã soạn 46 đặc tả Unit Test, xem [bảng độ phủ UT](my-estimates-unit-test-coverage.md). Chưa sửa mã ứng dụng, tạo migration hoặc viết/chạy mã test.

## Đã xác nhận

Nghiệp vụ trong STORY-PROJ-006/007 và BR-PROJ-008/009 đã được người dùng chốt. Danh sách có đủ bốn trạng thái và chỉ bản của khách chưa xóa; xóa nhiều chỉ nhắm tập Id đã chọn, xóa bản đủ điều kiện, chặn Pending, không hoàn lượt và không khôi phục. Owner và mọi link/QR/email đều không cấp quyền mở mới hồ sơ đã xóa. Reviewer và Approver tiếp tục là Tân Trần theo bộ tài liệu dự toán; không suy ra trạng thái Approved.

Ngày 30/09/2026 người dùng xác nhận hai điểm cuối: đánh dấu đã xóa và giữ dữ liệu nội bộ/lịch sử lượt, không cho khôi phục, chưa tự động dọn tệp hoặc đặt thời hạn lưu; tìm tên không phân biệt hoa/thường và dấu, “nha” tìm được “Nhà”. Đã ghi vào US/BR; ST-PROJ-081 bổ sung các biến thể tìm theo quyết định này.

## Thiết kế đã chốt trong hội thoại

- GET `/api/v1/estimates`, mặc định 10 bản, tối đa 100; ModifiedAtUtc DESC, Id ASC; query chỉ lấy các trường danh sách và trạng thái từ UsageOperation.
- POST `/api/v1/estimates/bulk-delete`, tối đa 100 phần tử, Idempotency-Key và kết quả từng Id. Một transaction lưu những bản đủ điều kiện cùng biên nhận; bản bị chặn là kết quả riêng, không làm hỏng lô.
- Biên nhận giữ nguyên kết quả lần yêu cầu: retry không tự xóa một bản từng bị chặn nhưng vừa chạy AI xong. Lần người dùng chủ động xóa tiếp dùng key mới.
- Mọi đường đọc, mutation và replay thao tác cũ phải xét bản đã xóa. Đồng thời với AI được xếp theo khóa AccountCommerceState rồi Estimate.

## Cần làm rõ

Không còn câu hỏi nghiệp vụ cản thiết kế API/schema của hai TDD. Cách bố trí hộp xác nhận và việc giữ lựa chọn thủ công qua trang vẫn là đề xuất giao diện, chưa đưa thành AC mới. Creator/Sprint/Assignee của US, Owner/Effective Date của BR chưa được cung cấp; không tự điền.

TDD-PROJ-004 dùng unaccent + chuẩn hóa Unicode trong truy vấn PostgreSQL, không đổi tên lưu. TDD-PROJ-005 thêm DeletedAtUtc và EstimateDeletionReceipt; chỉ mở endpoint xóa sau khi mọi reader/worker có guard. Môi trường triển khai cần kiểm extension/locale, thời gian khóa migration và tải thực tế. Chưa đặt SLA, RPO/RTO hoặc chính sách giữ dữ liệu dài hạn.

Sau xác nhận chốt TDD, đã hoàn thành đặc tả UT-PROJ-072–117 và cập nhật điều kiện chưa xóa cho UT-PROJ-015/021/044/051; không thay hợp đồng đã chốt.

## Căn cứ khảo sát và khác biệt cần giữ rõ

| Nguồn đã đối chiếu | Kết luận ảnh hưởng thiết kế |
|---|---|
| TDD-PROJ-001/002/003 | Đã đối chiếu các phần nghiệp vụ/kỹ thuật liên quan của ba TDD; schema danh mục, đầu vào, AI, kết quả, chia sẻ, email dùng lại. Một số đoạn hiện trạng còn mô tả trước triển khai, phải đối chiếu code. |
| STORY-PROJ-006/007, BR-PROJ-008/009, ST-PROJ-078–109 | Hai Story có 22 AC, 2 Main Flow, 6 ALT, 7 EXC; ST là đặc tả chưa chạy. |
| BR-PROJ-005/006/007, BR-SUB-003/007, BR-RBAC-005 | Trạng thái AI, quyền cũ khi hết gói, ngoại lệ bản xóa, không hoàn lượt và tách Customer/Staff. |
| TDD-SUB-002/Data Model, TDD-PAY-001/Data Model | Đối chiếu phần UsageOperation, PeriodQuota và AccountCommerceState liên quan. Không đổi luồng mua/gán/hủy gói. |
| TDD-AUTH-001/Architecture và code kiểm Origin | Dùng policy phiên và CSRF sẵn có; route xóa không được miễn trừ. |
| EstimateStore / EstimateConfigurations | Có index owner/mốc sửa/Id; chưa có trường xóa. ReadGenerationAsync ưu tiên tác vụ sống, sau đó thất bại gần nhất. |
| TransactionPipelineBehavior / EfUnitOfWork | Command tự có transaction; không cần tạo command con tự commit. Kết quả phần tử không đủ điều kiện phải là dữ liệu trả về, không ném lỗi cả lô. |
| Create/Save/RequestGeneration handlers | Các receipt hoặc operation cũ có thể được trả lại; phải thêm kiểm quyền bản còn dùng trước replay, gồm cả replay tạo bản. |
| EstimateSharingStore / EstimateSharingWorkers | Public grant JOIN Estimate; email kiểm link trước SMTP. Export hiện chưa có kiểm bản xóa, thiết kế bổ sung ở TDD-PROJ-005, chưa sửa code. |
| BR-PROJ-006/007 và TDD-PROJ-001 | Chặn hồ sơ qua BMT không thu hồi được ảnh đầu vào ở URL công khai hoặc tệp đã tải. Không tự thêm lời hứa xóa ở kho ngoài. |

Phạm vi tham chiếu: đã quét đệ quy các mã tài liệu từ nhóm TDD/US/BR/ST gốc, tìm được 726 tài liệu liên thông (33 Story, 73 BR, 24 TDD, 323 ST, 273 UT), không có mã tài liệu thiếu trong tập quét. Đây là kiểm tồn tại theo mã, **không phải đã đọc và xác minh nội dung toàn bộ 726 tài liệu**, hoặc đã kiểm mọi section/URL của chúng. Nhóm liên thông lan sang gói, thanh toán, thư viện, RBAC và các test của chúng. Audit toàn bộ đồ thị vẫn chưa hoàn tất; phạm vi bàn giao này là thiết kế hai tính năng và kiểm các tham chiếu trực tiếp của tài liệu đã sửa, không xác nhận mọi phụ thuộc toàn hệ thống đã đúng.

Trong nhóm PROJ hiện có những ghi chú cũ như “chưa có module”, “chưa triển khai” hoặc “chưa thiết kế danh sách”; chúng không phải bằng chứng code hiện tại. Hai TDD mới tách hiện trạng đọc được trong workspace khỏi đề xuất. Đã thêm ghi chú dẫn sang thiết kế mới ở Architecture của TDD-PROJ-001/002/003; không sửa hàng loạt những đoạn lịch sử không thuộc phạm vi.

## Đối chiếu nghiệp vụ với thiết kế

| Story / AC | Rule | Phần thiết kế | System Test |
|---|---|---|---|
| STORY-PROJ-006 AC-001, AC-002 | BR-PROJ-008 khoản 1–3 | 004 Architecture/Data Model: owner, bốn trạng thái, LEFT JOIN loại | ST-PROJ-078, 079, 082, 090 |
| STORY-PROJ-006 AC-003, AC-004 | BR-PROJ-008 khoản 4–5 | 004 Architecture/Internal API: lọc, sort, trang; tìm không phân biệt hoa/thường/dấu | ST-PROJ-080, 081, 082, 083 |
| STORY-PROJ-006 AC-005 | BR-PROJ-008 khoản 6 | 004 Internal API: tập rỗng khác lỗi | ST-PROJ-084, 085 |
| STORY-PROJ-006 AC-006, AC-007 | BR-PROJ-008 khoản 7–8, BR-SUB-007 | 004 Architecture: không kiểm quyền dùng mới, mở dữ liệu đã lưu | ST-PROJ-086, 087 |
| STORY-PROJ-006 AC-008 | BR-PROJ-008 khoản 9, BR-PROJ-009 | 004 và 005 Architecture: loại bản xóa, kiểm lại khi mở | ST-PROJ-091, 101 |
| STORY-PROJ-006 AC-009, AC-010 | BR-PROJ-008/Except và khoản 6 | 004 Internal API: auth/role/lỗi tải | ST-PROJ-088, 089, 092 |
| STORY-PROJ-007 AC-001 | BR-PROJ-009 khoản 2, 12 | 005 Internal API/Architecture: Id được chọn trên trang, thông báo hậu quả | ST-PROJ-094 |
| STORY-PROJ-007 AC-002, AC-003, AC-004 | BR-PROJ-009 khoản 3–5, 11 | 005 transaction và kết quả từng Id | ST-PROJ-093, 095, 096, 099 |
| STORY-PROJ-007 AC-005, AC-006 | BR-PROJ-009 khoản 9–11, BR-SUB-003/007 | 005 khóa điều phối không kiểm/cấp gói, không sửa lượt | ST-PROJ-097, 098, 100 |
| STORY-PROJ-007 AC-007 | BR-PROJ-009 khoản 6–7 | 005 bảng tác động reader/mutation/replay | ST-PROJ-101, 109 |
| STORY-PROJ-007 AC-008 | BR-PROJ-009 khoản 8 và Except | 005 owner/public/stream/email/worker | ST-PROJ-102 |
| STORY-PROJ-007 AC-009 | BR-PROJ-009 khoản 1, 5–6 | 005 results không lộ tài nguyên khách khác | ST-PROJ-103 |
| STORY-PROJ-007 AC-010 | BR-PROJ-009 khoản 4, 11 | 005 thứ tự khóa, chặn Pending và AI đến sau | ST-PROJ-106, 107 |
| STORY-PROJ-007 AC-011, AC-012 | BR-PROJ-009 khoản 1, 13 | 005 Internal API và retry cùng key | ST-PROJ-104, 105, 108 |

Các nhánh ALT/EXC giữ truy vết tới ST trong bảng độ phủ nghiệp vụ. Phần kiểm chứng kỹ thuật đã phân chia trong bảng UT và kế hoạch tích hợp: phân trang sai/biên, ký tự tìm kiếm, cùng mốc sửa; key lặp/khác nội dung; commit mất phản hồi; replay tạo/lưu/gen/share của bản đã xóa; worker và stream tranh xóa; triển khai code cũ/mới không làm dữ liệu hiện lại. Đã soạn ca Unit Test chi tiết ở bảng độ phủ UT; chưa viết/chạy mã test hoặc đánh dấu Pass.

## Mốc nguồn

Các hash trong bảng ST cũ là mốc nội dung lúc soạn ST, trước khi thêm liên kết TDD. Lần hoàn thiện này bổ sung quyết định tìm không dấu vào Flow/AC-004 của STORY-PROJ-006, BR-PROJ-008 và ST-PROJ-081; ghi cách giữ dữ liệu ở STORY-PROJ-007 và BR-PROJ-009. Hash mới ở bảng này theo dõi nội dung hiện tại; không đại diện phê duyệt.

| Tài liệu | SHA-256 |
|---|---|
| [STORY-PROJ-006](../userstory/STORY-PROJ-006.md) | `aaf2bf802d1c6f4d0d31a1d9d4b692e5ccf6f51d934e65ff93d4875869845da6` |
| [STORY-PROJ-007](../userstory/STORY-PROJ-007.md) | `2cfc83786c0561ce90de2d2dad530c990d598a7ae134cade1bc459887cfe1579` |
| [BR-PROJ-008](../businessrule/BR-PROJ-008.md) | `115d079d9690793b00119103ee878c921710500188baa7fafa744e1f1c9f19bd` |
| [BR-PROJ-009](../businessrule/BR-PROJ-009.md) | `f89e5c9b5a2b5073828b2ac414939fcc44608c73c320d9b507fdcacfeb53663f` |
| [TDD-PROJ-004](../tdd/TDD-PROJ-004.md) | `604c32d4851f33cc187a7cefd07cedc6550e9d04eb78ae7e26af2d409de3ac42` |
| [TDD-PROJ-005](../tdd/TDD-PROJ-005.md) | `755c28955080d6f0aece878cf1a537a159ab48e86b5d34ec593ebe22f75c4e95` |

ST-PROJ-081 hiện tại: `ebb9396aee5e84fb87ee5129ffccc76ab922a2bf8c741effa0c22c68e251e052`. Chỉ là hash nội dung; ca chưa chạy.
