# Danh sách và xóa dự toán của tôi

**Cập nhật triển khai 30/09/2026:** backend đã được triển khai theo yêu cầu “bắt đầu chiến”. Xem [báo cáo triển khai](my-estimates-implementation.md). Các đoạn nói “chưa viết/chạy” bên dưới ghi nhận thời điểm thiết kế ban đầu; kết quả kiểm chứng hiện tại nằm trong báo cáo mới.

Người dùng đã xác nhận “chốt” trong hội thoại ngày 30/09/2026 cho hai User Story, hai Business Rule mới và ba phần bổ sung vào quy tắc hiện có. Đã soạn 32 đặc tả ST-PROJ-078–109; xem [bảng độ phủ và hash nguồn](my-estimates-system-test-coverage.md). Reviewer và Approver đều là Tân Trần. Đã hoàn thiện hai TDD để review với API đề xuất, xem [bản tổng hợp kỹ thuật](my-estimates-technical-design.md). Chưa thực thi ST, sửa code hoặc chạy migration; xác nhận nghiệp vụ không phải phê duyệt trên hệ thống quản lý tài liệu.

## Đã xác nhận

| Nội dung | Quyết định |
|---|---|
| Phạm vi dữ liệu | Dự toán thuộc chính khách đang đăng nhập; không phải Công trình của nhóm SITE. |
| Trạng thái danh sách | Đủ nháp, đang xử lý, thành công và thất bại. |
| Thông tin hiển thị | Tên, loại công trình nếu đã chọn, trạng thái, ngày tạo và lần sửa gần nhất; không có ảnh đại diện, không thêm cột địa chỉ. |
| Tìm và xem | Phân trang, tìm một phần tên không phân biệt hoa/thường và dấu, lọc trạng thái, mặc định bản sửa gần nhất đứng trước. |
| Xóa nhiều | Chọn một hoặc nhiều dự toán để xóa cùng lúc. |
| Chọn tất cả | Chỉ chọn các dự toán trên trang đang xem. |
| Kiểm tra từng bản | Xóa bản đủ điều kiện; giữ bản đang xử lý AI và báo riêng kết quả. Không chặn cả lần xóa chỉ vì một bản không đủ điều kiện. |
| Sau khi xóa | Không còn trong danh sách, không mở lại, không có thùng rác hoặc chức năng khôi phục. Đánh dấu đã xóa và giữ dữ liệu nội bộ/lịch sử lượt để đối soát; chưa tự động dọn tệp hoặc đặt thời hạn lưu trong đợt này. |
| Hồ sơ chia sẻ | Ngừng truy cập mới qua link, QR và link trong email; không thu hồi tệp đã được tải về. |
| Lượt sử dụng | Không hoàn lượt đã dùng; xóa không phải một lần tạo AI mới. |
| Gói hết hạn hoặc hết lượt | Vẫn được xóa bản đủ điều kiện thuộc mình; chặn AI đang xử lý vẫn áp dụng. |
| Reviewer và Approver | Cả hai là Tân Trần cho hai US và hai BR mới. |

Các chi tiết bảo vệ quyền sở hữu, đọc dữ liệu đã lưu, không tự gọi AI khi mở lại và quyền xem hồ sơ khi hết hạn gói dùng lại quy tắc hiện hành. Xóa nhiều cần kiểm tra lại trạng thái từng bản lúc xử lý; dữ liệu đã tải trên giao diện không thay thế kiểm tra này.

## Bộ tài liệu đã chốt trong hội thoại

| User Story | Business Rule chính | Phạm vi tiêu chí nghiệm thu |
|---|---|---|
| [STORY-PROJ-006](../userstory/STORY-PROJ-006.md) — Dự toán của tôi | [BR-PROJ-008](../businessrule/BR-PROJ-008.md) | AC-001–010: quyền sở hữu, đủ trạng thái, thông tin hiển thị, phân trang/tìm/lọc/sắp xếp, dữ liệu rỗng, hết hạn gói, mở lại, loại bản đã xóa và lỗi tải. |
| [STORY-PROJ-007](../userstory/STORY-PROJ-007.md) — Xóa nhiều dự toán | [BR-PROJ-009](../businessrule/BR-PROJ-009.md) | AC-001–012: chọn trong trang, xóa bản đủ điều kiện, kết quả từng bản, chặn AI, hết hạn gói, không hoàn lượt, không khôi phục, ngừng chia sẻ, quyền sở hữu và thay đổi trạng thái giữa lúc chọn/xóa. |

Đã bổ sung ngoại lệ cho bản bị xóa vào ba quy tắc hiện có để tránh hiểu rằng hồ sơ còn mở được chỉ vì link còn hạn hoặc khách từng có quyền xem:

- [BR-PROJ-006/Except](../businessrule/BR-PROJ-006.md#except): ngừng xem/tải mới qua chia sẻ sau khi xóa; thu hồi link đơn thuần vẫn khác xóa bản dự toán.
- [BR-PROJ-007/Except](../businessrule/BR-PROJ-007.md#except): không cung cấp lại kết quả hoặc hồ sơ của bản đã xóa.
- [BR-SUB-007/Except](../businessrule/BR-SUB-007.md#except): quyền với hồ sơ cũ áp dụng cho bản chưa bị xóa; hết hạn gói hoặc hết lượt vẫn được xem danh sách còn lại và xóa bản đủ điều kiện.

Ba phần bổ sung này được người dùng chốt cùng bộ US/BR. Chỉ cập nhật Context/Notes để ghi nhận xác nhận khi viết ST; không thay đổi các khoản hiện có về sửa đầu vào, tiếp nhận AI, thời hạn link hoặc tính lượt. Mốc hash trước và sau cập nhật ghi nhận nằm trong bảng độ phủ.

## Hiện trạng đã kiểm tra

| Thành phần | Bằng chứng và ảnh hưởng |
|---|---|
| API dự toán | [EstimateApi.cs](../../bmt-be/src/bmt-be.presentation/apis/estimate/EstimateApi.cs) hiện có tạo, đọc từng bản, lưu đầu vào, đổi tên, gửi và theo dõi AI; chưa có API danh sách hoặc xóa. |
| Đọc bản và trạng thái | [GetEstimateQueryHandler.cs](../../bmt-be/src/bmt-be.application/usecases/queries/estimate/GetEstimateQueryHandler.cs) trả trạng thái nháp, đang xử lý, thành công hoặc thất bại; cần dùng cùng cách xác định trạng thái cho danh sách. |
| Quyền sở hữu | [EstimateAccess.cs](../../bmt-be/src/bmt-be.application/usecases/commands/estimate/EstimateAccess.cs) kiểm tài khoản khách. [EstimateStore.cs](../../bmt-be/src/bmt-be.persistence/repositories/EstimateStore.cs) đọc theo Id và OwnerId; chưa có thao tác danh sách hoặc xóa. |
| Dữ liệu dự toán | [Estimate.cs](../../bmt-be/src/bmt-be.domain/entities/Estimate.cs) có chủ sở hữu, tên, mốc tạo/sửa và đầu vào; chưa có trạng thái xóa. [EstimateConfigurations.cs](../../bmt-be/src/bmt-be.persistence/configurations/EstimateConfigurations.cs) đã có index theo chủ sở hữu và lần sửa gần nhất. Đây là hiện trạng, chưa phải quyết định schema cho tính năng mới. |
| Lịch sử AI | [EstimateGenerationConstraintTests.cs](../../bmt-be/test/bmt-be.integration.tests/EstimateGenerationConstraintTests.cs) có mã test kiểm tra không xóa vật lý bản còn lịch sử tham chiếu. Chỉ đọc mã test, chưa chạy lại. Không thể coi thêm một lệnh xóa dòng là đã đáp ứng nghiệp vụ. |
| Đường chủ sở hữu và chia sẻ | [EstimateSharingSupport.cs](../../bmt-be/src/bmt-be.application/usecases/commands/estimateSharing/EstimateSharingSupport.cs) có hai đường kiểm quyền riêng; [EstimateSharingStore.cs](../../bmt-be/src/bmt-be.persistence/repositories/EstimateSharingStore.cs) đọc quyền chia sẻ từ link và bản dự toán. Cả hai đường phải xét bản đã xóa. |
| Công việc nền | [EstimateSharingWorkers.cs](../../bmt-be/src/bmt-be.application/services/EstimateSharingWorkers.cs) chuẩn bị tệp và gửi email; email kiểm lại link trước gửi. Cần xem xét các công việc đã được tiếp nhận trước khi khách xóa, tránh cấp lại quyền truy cập bản đã xóa. |

Nguồn nghiệp vụ trực tiếp đã đối chiếu gồm [BR-RBAC-005](../businessrule/BR-RBAC-005.md), [BR-SUB-003](../businessrule/BR-SUB-003.md), [BR-SUB-007](../businessrule/BR-SUB-007.md), [BR-PROJ-005](../businessrule/BR-PROJ-005.md), [BR-PROJ-006](../businessrule/BR-PROJ-006.md) và [BR-PROJ-007](../businessrule/BR-PROJ-007.md). Các luồng mở đầu vào, AI, hồ sơ và chia sẻ nằm ở STORY-PROJ-001–004.

## Đề xuất chưa chốt

- Giao diện có bước xác nhận trước xóa, nêu số bản, tên các bản đã chọn, việc không thể khôi phục và ngừng chia sẻ. Quy tắc thông báo hậu quả đã được ghi trong bản nháp; cách bố trí hộp xác nhận chưa được người dùng quyết định riêng.
- Khi đổi trang, từ khóa hoặc bộ lọc, bỏ lựa chọn cũ để tránh xóa nhầm bản không còn nhìn thấy. Người dùng đã chốt phạm vi Chọn tất cả là trang đang xem; hành vi giữ lựa chọn thủ công qua nhiều trang chưa được xác nhận, nên chưa đưa thành AC.

## Cần làm rõ ở bước tiếp theo

- Metadata còn thiếu: Creator, Sprint, Assignee của US; Owner và Effective Date của BR. Không tự lấy Reviewer/Approver làm người thay thế. Các trường chưa biết được giữ rõ trong bản nháp.
- Không còn quyết định nghiệp vụ cản TDD backend. Các chi tiết phân trang, phản hồi từng bản, mã lặp, retry, đồng thời với AI/worker và migration đã được thiết kế trong TDD-PROJ-004/005; đã được người dùng chốt trong hội thoại.
- Hạn lưu lâu dài, dọn tệp, RPO/RTO và số liệu tải chưa được cung cấp; đợt này không tự đặt các chính sách đó.
- Giới hạn audit chuỗi tham chiếu ghi trong [bản tổng hợp kỹ thuật](my-estimates-technical-design.md); không coi việc kiểm mã tài liệu tồn tại là đã review toàn hệ thống.

## Phạm vi thay đổi và cách kiểm chứng dự kiến

Sau xác nhận của người dùng, đã thiết kế 32 System Test theo 22 AC, hai Main Flow và 13 nhánh ALT/EXC của hai Story, gồm cả ảnh hưởng của bản đã xóa lên hồ sơ/chia sẻ/gói. [Bảng độ phủ](my-estimates-system-test-coverage.md) đối chiếu từng AC, nhánh và từng khoản của hai BR mới; ST-PROJ-097/098/101/102 bổ sung phạm vi của ba quy tắc liên quan. Các ca chưa được thực thi và còn cần hợp đồng API để hoàn thiện kỳ vọng HTTP/mã lỗi.

Các tài liệu liên quan đã được ghi chú tác động thiết kế gồm [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) cho danh sách và truy cập dự toán, [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) cho trạng thái và việc xóa đồng thời với tiếp nhận AI, [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) cho hồ sơ, tệp, link và email. Thiết kế mới nằm ở TDD-PROJ-004/005, liên kết từ ba TDD nguồn.

Phần code dự kiến cần xem xét gồm contract/validator, query danh sách, command xóa nhiều, endpoint, nơi kiểm quyền truy cập dự toán và các repository liên quan. Chỉ thay đổi mô hình dữ liệu sau khi đã đánh giá kiến trúc dữ liệu, schema, quan hệ, chuẩn hóa và khả năng tương thích dữ liệu hiện có bằng các skill của dự án.

Chiến lược kiểm chứng gồm hành vi ở handler, truy vấn và thay đổi dữ liệu trên PostgreSQL thực, cùng luồng giao diện/API. Các đặc tả ST đã viết tập trung vào cách ly tài khoản, đủ trạng thái, tìm/lọc/phân trang, kết quả xóa từng bản, thứ tự tiếp nhận AI và xóa, không hoàn lượt và không còn truy cập qua đường dẫn cũ. Chưa viết mã kiểm thử hoặc có kết quả chạy.

Trong lần chuẩn bị này kiểm tra cấu trúc Markdown, mã tài liệu, tham chiếu trực tiếp, độ dài theo mẫu, 13 cột của từng ST, sự nhất quán Trace to/TEST_LINKS và độ phủ các AC/nhánh/quy tắc trong bộ mới. Không có kết quả kiểm thử ứng dụng hoặc xác nhận toàn bộ chuỗi tài liệu đã đầy đủ.

## Tiến độ thiết kế kỹ thuật

Người dùng đã chốt bộ đặc tả Unit Test và bảng độ phủ trong hội thoại ngày 30/09/2026. Phần thiết kế hai tính năng đã được bàn giao và xác nhận; chưa chuyển sang triển khai code.

Theo yêu cầu “lên TDD”, đã soạn [TDD-PROJ-004](../tdd/TDD-PROJ-004.md) và [TDD-PROJ-005](../tdd/TDD-PROJ-005.md), đồng thời thêm liên kết vào hai Story. Người dùng đã trả lời cả cách giữ dữ liệu và tìm tên không dấu; phần thiết kế phụ thuộc đã được bổ sung. Đã chốt TDD và soạn 46 đặc tả UT-PROJ-072–117; xem [bảng độ phủ UT](my-estimates-unit-test-coverage.md). Chưa viết/chạy mã test. Xem [bản tổng hợp kỹ thuật](my-estimates-technical-design.md) để biết đề xuất, truy vết và giới hạn khảo sát.
