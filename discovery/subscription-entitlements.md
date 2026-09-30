# Subscription và Entitlement BMT — ghi chú nghiệp vụ

## Cập nhật đồng bộ — 25/09/2026

Người dùng chốt thêm một loạt quyết định ngày 25/09/2026; US, BR, TDD và test đã được sửa theo. Các ghi chú lịch sử bên dưới giữ nguyên để tra cứu; khi khác nhau, dùng Business Rule hiện hành.

- **Công trình thay cho “dự án” của gói giám sát.** Gói giám sát và phân công nhân viên gắn với công trình (`ConstructionSite`), một thực thể riêng khác bản dự toán, do khách tự tạo miễn phí. Story, BR và TDD của Công trình chưa được soạn. Chỗ nào bên dưới nói “dự án” của gói giám sát thì đọc là công trình.
- **Hoàn thành và mở lại gói giám sát:** Admin hoặc nhân viên có `supervision.complete` đang phụ trách chính công trình đó. Không còn phân công mức khách hàng; mỗi công trình có tối đa một người phụ trách ([BR-SUB-011](../businessrule/BR-SUB-011.md), [BR-SUB-012](../businessrule/BR-SUB-012.md), [BR-RBAC-013](../businessrule/BR-RBAC-013.md)).
- **Công bố gói giám sát** cần tên, giá và mô tả dịch vụ; gói giám sát không dùng danh mục quyền lợi ([BR-SUB-008](../businessrule/BR-SUB-008.md) khoản 7, STORY-SUB-002/AC-027 và EXC-06, ST-SUB-124). Quyền cấu hình gói có mã `plan.manage`.
- **Phạm vi nghiệm thu quyền:** theo BR-SUB-008 khoản 12, các AC về quyền dạng mức hoặc kiểm tra quyền bật/tắt không nghiệm thu đợt này: STORY-SUB-001/AC-009, AC-010 và STORY-SUB-002/AC-001, AC-015, AC-016. ST-SUB-009, ST-SUB-010, ST-SUB-011, ST-SUB-067 và ST-SUB-068 đã rút khỏi nghiệm thu. Phối cảnh 3D vẫn là quyền bật/tắt để hiển thị, chưa kiểm khi Gen AI (khoản 9).
- **Mốc chốt giá và quyền lợi** của mọi lần mua, kể cả đổi gói và mua lại, là lúc tạo đơn theo BR-PAY-001. Các ghi chú “thiết kế cùng thanh toán” bên dưới đã có câu trả lời này ([BR-SUB-004](../businessrule/BR-SUB-004.md), [BR-SUB-021](../businessrule/BR-SUB-021.md)).
- **Đổi tên** dự án/bản dự toán được phép bất cứ lúc nào, không cần gói hay lượt ([BR-SUB-007](../businessrule/BR-SUB-007.md) khoản 11, ST-SUB-125).
- **Tư vấn KTS miễn phí** là kênh riêng, không thay cam kết tư vấn offline của gói (BR-SUB-008/Notes, BR-CONSULT-002).
- **Thiết kế kỹ thuật:** TDD-SUB-003 đã bị thay bởi [TDD-SUB-004](../tdd/TDD-SUB-004.md) (gán; đổi công trình đã bỏ ngày 25/09/2026), [TDD-SUB-005](../tdd/TDD-SUB-005.md) (hủy, khôi phục) và [TDD-SUB-006](../tdd/TDD-SUB-006.md) (hoàn thành, mở lại). UT-SUB-050 đã rút; UT-SUB-051 đến UT-SUB-074 theo TDD-SUB-006; UT-SUB-075 đến UT-SUB-078 thêm cho TDD-SUB-001 và UT-SUB-079 cho TDD-SUB-002. Sau khi chuyển sang phân công theo gói và bỏ đổi công trình (25/09/2026, lần 2): UT-SUB-070 đã rút; UT-SUB-052, 053, 057, 061, 062, 063, 066, 069, 074 đã sửa; thêm UT-SUB-080 và UT-SUB-081 cho TDD-SUB-006.

## Cập nhật thanh toán — 19/09/2026

Đã lưu các quyết định mới tại [Thanh toán và quản lý gói đã mua](payment-packages.md), kèm 4 User Story, 9 Business Rule và 68 đặc tả System Test (đã bổ sung tra cứu quản trị). Các ghi chú lịch sử dưới đây còn nói “thanh toán thiết kế sau”, “giám sát bắt buộc gắn dự án”, “không có hạn gán” hoặc “không cho sửa dự án” phải đọc theo quyết định mới. TDD và Unit Test cũ chưa bao phủ thanh toán, hạn gán hoặc hủy/khôi phục; không coi đã sẵn sàng triển khai toàn bộ tích hợp. Các điểm biên thanh toán đã chốt và được lưu cùng 4 TDD, 70 đặc tả Unit Test tại [bàn giao thiết kế thanh toán](payment-technical-design.md). Chưa viết code hoặc chạy test ứng dụng.

## Bàn giao thiết kế kỹ thuật — bản nháp

**Đã xác nhận:** Tân Trần là Reviewer và Approver của các bản nháp mới. Với tra cứu, sau khi server ghi nhận thành công thì giữ 1 lượt dù mất phản hồi; gửi lại cùng lần mở trả kết quả đã lưu, không trừ thêm. Quyết định nằm ở BR-SUB-017/Then, STORY-SUB-001/AC-071 và ST-SUB-109.

**Thiết kế đề xuất:** lưu revision quyền lợi bất biến, kỳ theo tài khoản, quota riêng cho hai quyền, operation có khóa chống xử lý lặp; transaction PostgreSQL bảo vệ bộ đếm. Gọi dịch vụ nội dung/AI ngoài transaction. Các tên lớp, bảng và API trong TDD là thiết kế mới, chưa có mã triển khai.

| Phạm vi / nguồn | Thiết kế | Đặc tả unit test | Kiểm chứng tích hợp bổ sung |
| --- | --- | --- | --- |
| STORY-SUB-002; BR-SUB-004/005/008/013/015 | [TDD-SUB-001](../tdd/TDD-SUB-001.md) | UT-SUB-001–013, UT-SUB-075–078 | ST-SUB-113 |
| STORY-SUB-001; BR-SUB-001/002/003/006/007/014/016/017/021 | [TDD-SUB-002](../tdd/TDD-SUB-002.md) | UT-SUB-014–049, UT-SUB-079 | ST-SUB-109–112, ST-SUB-114 |
| STORY-SUB-003; BR-SUB-004/006/009/011/012 | [TDD-SUB-006](../tdd/TDD-SUB-006.md); [TDD-SUB-003](../tdd/TDD-SUB-003.md) đã bị thay | UT-SUB-051–069, UT-SUB-071–074, UT-SUB-080–081; UT-SUB-050 và UT-SUB-070 đã rút | ST-SUB-115–116 |

Lần bàn giao đầu đã soạn 58 file trong `unittest/` và 8 ca tích hợp bổ sung trong `systemtest/`, mỗi file một test. Đến ngày 25/09/2026, bộ UT-SUB có 81 file, trong đó UT-SUB-050 và UT-SUB-070 đã rút. Đây là đặc tả, chưa viết hoặc chạy mã test. Các ca tích hợp phải dùng PostgreSQL thật; mock chỉ kiểm tra nhánh xử lý. Người thực thi test chưa được phân công.

**Phạm vi lịch sử không đưa vào nghiệm thu hiện tại:** BR-SUB-018–020; xếp bậc/nâng/hạ và đổi gói chờ cuối kỳ; logic 3D và các mức tư vấn; BR-SUB-010 và bộ đếm giám sát offline. ST-SUB-010/011/068/069/102/104 cùng các AC cũ mô tả mức quyền, giới hạn đúng ba quyền hoặc chặn tính năng 3D phải đọc theo quyết định mới nhất ở đầu tài liệu. ST-SUB-070 chỉ giữ phần bộ kết quả đầy đủ, chưa nghiệm thu điều khiển 3D. Không coi phần lịch sử là yêu cầu triển khai bổ sung.

**Cần làm rõ trước khi tích hợp đầy đủ:** contract thanh toán/cấp kỳ hợp lệ; module Công trình (trước gọi là dự án) và phân công nhân viên; nguồn nội dung mẫu; adapter AI/lưu đầu ra; giá trị timeout vận hành; danh sách tên quyền hiển thị bổ sung và cách hiển thị khi tắt. Các điểm này được giữ mở, không ngăn soạn thiết kế lõi. Chưa chạy migration, import hoặc sửa mã ứng dụng.


## Phạm vi hiện tại đã chốt: hai quyền có logic, các quyền khác bật/tắt

Người dùng xác nhận: chỉ Số phương án thiết kế mới và Tra cứu thư viện mẫu cần xử lý logic sử dụng/tính lượt. Mọi quyền lợi còn lại cấu hình bật/tắt theo gói để lưu/hiển thị, chưa triển khai logic tính năng hoặc bộ đếm tương ứng. Không cần hỏi lại dạng cấu hình của các quyền này.

Tư vấn vẫn offline, giữ mô tả tự do và cam kết của kỳ đã mua; không trở lại danh sách mức Online/Ưu tiên/1:1. Việc bật/tắt quyền hiển thị không tự cấp lượt, điều khiển AI hoặc cung cấp dịch vụ. Quy trình cấu hình/lưu nháp/Công bố và các quy tắc subscription đã chốt vẫn giữ. Không tự đưa mọi dòng quảng cáo thành tính năng cần code.

Đã cập nhật BR-SUB-005/008, STORY-SUB-002/AC-026 và ST-SUB-108. Các mô tả cần chốt dạng quyền khác trong lịch sử bên dưới đã được quyết định này thay thế; danh sách tên quyền cụ thể và cách hiển thị khi tắt vẫn chưa được chốt đầy đủ. Đặc tả chưa thực thi.


## Quyết định mới nhất: cấu hình quyền và phạm vi logic sử dụng

- **Đã xác nhận:** các quyền chưa triển khai logic vẫn có thể nằm trong danh mục quyền hệ thống để cấu hình/hiển thị. Không còn giới hạn tổng danh mục đúng ba mục. Hiện chỉ Số phương án thiết kế mới và Tra cứu thư viện mẫu có logic sử dụng/tính lượt.
- **Đã xác nhận riêng cho 3D:** vẫn cấu hình bật/tắt và hiển thị theo gói, chưa dùng để điều khiển đầu ra AI hoặc kiểm tra quyền tạo 3D. Không tự triển khai điều kiện bật 3D phải có quyền tạo thiết kế; câu hỏi này trước đó chưa được người dùng chọn.
- Quy tắc giữ quyền tác vụ đang chạy được lưu cho bước triển khai tính năng tương ứng; phần 3D chưa nghiệm thu hiện tại. Kế toán lượt, timeout, không trả kết quả muộn và giới hạn một kỳ hiệu lực vẫn giữ nguyên.
- Tư vấn vẫn là mô tả tự do, vận hành offline và giữ cam kết theo kỳ đã mua. Chưa có quyết định chuyển tư vấn sang trường mức riêng.
- **Cần làm rõ:** danh sách và dạng cấu hình các quyền bổ sung cần hiển thị; không tự đưa mọi nội dung website vào danh mục hoặc khôi phục chức năng chỉnh sửa đã bỏ. Cách trình bày quyền chưa triển khai chưa được chốt.
- Đã cập nhật BR-SUB-003/005/008/021, hai Story và các đặc tả bị ảnh hưởng; AC-025 của STORY-SUB-002 và ST-SUB-107 là phạm vi mới. Nội dung ba quyền/AI 3D ở các phần lịch sử bên dưới không được dùng để hỏi lại hoặc triển khai trái quyết định này. Chưa viết code hoặc chạy test ứng dụng.


Cập nhật ngày 18/09/2026. Tài liệu này gộp ghi chú trao đổi, hướng dẫn tiếp tục và báo cáo rà soát thành một nơi theo dõi. Các quyết định nghiệp vụ giữ nguyên; User Story, Business Rule và đặc tả test vẫn nằm trong các thư mục riêng.

## Điểm tiếp tục mới nhất: mô tả tư vấn tự do

- **Đã xác nhận:** người dùng chọn 2: Admin nhập nội dung tư vấn tự do trong mô tả gói, không có trường chọn mức tư vấn riêng, danh sách mức cố định hoặc entitlement tư vấn. Tư vấn vẫn offline. Đã cập nhật BR-SUB-008/Notes, STORY-SUB-002/AC-023 và ST-SUB-105.
- **Đã xác nhận thêm:** người dùng chọn 1: giữ nội dung tư vấn của lần mua trong suốt kỳ đã mua; mô tả mới áp dụng lần mua mới. BR-SUB-004/Then, STORY-SUB-002/AC-024 và ST-SUB-106 ghi nhận quyết định.
- **Cần làm rõ tiếp:** dữ liệu tối thiểu để được Công bố gói thiết kế; không hỏi lại giá dương, hạn mức tối thiểu hoặc danh mục ba quyền đã chốt. Cách lưu mô tả đã chốt thuộc thiết kế kỹ thuật; không yêu cầu người dùng chọn kiểu dữ liệu.

## Xác nhận bổ sung: tư vấn vận hành offline

Người dùng xác nhận ba mức tư vấn trên trang gói — BASIC: Online, PLUS: Ưu tiên, PRO: Chuyên gia 1:1 — đang được thực hiện offline; website chưa có tính năng tư vấn tương ứng.

- **Đã xác nhận:** đây là mô tả dịch vụ đi kèm gói, chưa phải quyền tính năng trên nền tảng. Danh mục thiết kế tạm thời vẫn chỉ có tạo thiết kế, tra cứu mẫu và phối cảnh 3D.
- Không dùng ba giá trị tư vấn làm quyền dạng mức để triển khai trong đợt này; không tự thêm lượt tư vấn, lịch hẹn hoặc chức năng tư vấn trực tuyến. Khả năng hỗ trợ quyền dạng mức của hệ thống vẫn giữ cho nhu cầu được xác nhận sau.
- Cách vận hành, cam kết và giới hạn tư vấn offline chưa được mô tả chi tiết; không tự đặt số buổi, thời lượng hoặc thời gian phản hồi. Phần này không đồng nghĩa với trợ lý AI/chatbot đang hiển thị trên trang.
- Đã cập nhật BR-SUB-008/Notes, STORY-SUB-002/Out of Scope và ST-SUB-104. Đặc tả chưa thực thi.

## Quét lại hai trang gói dịch vụ — 18/09/2026

Đọc trực tiếp nội dung trang và dấu có/không trong bảng so sánh bằng trình duyệt. Đây là thống kê giao diện, không phải cấu hình bán hoặc phạm vi triển khai đã được duyệt. Người dùng đồng ý danh mục thiết kế tạm thời ba quyền: tạo thiết kế, tra cứu mẫu, phối cảnh 3D. Đã cập nhật BR-SUB-008, STORY-SUB-002/AC-022, ST-SUB-104.

### Thiết kế

Nguồn: [Trang gói thiết kế](https://vnz-bmt-savico-abcxyz.vercel.app/vi/plans).

| HẠNG MỤC | BASIC | PLUS | PRO |
| --- | --- | --- | --- |
| Số phương án thiết kế mới | 3 phương án | 10 phương án | 20 phương án |
| Lượt chỉnh sửa phương án | 3 lượt | 10 lượt | 20 lượt |
| Tra cứu thư viện mẫu | 20 lượt | 50 lượt | 100 lượt |
| Upload ảnh khu đất / hiện trạng | Có | Có | Có |
| Nhập diện tích, vị trí, quy mô | Có | Có | Có |
| Chọn loại công trình | Có | Có | Có |
| Chọn phong cách / màu sắc | Có | Có | Có |
| Bố trí công năng | Cơ bản | 2D & 3D | 2D & 3D nâng cao |
| Phương án hình ảnh thiết kế | Có | Có | Có |
| Dự toán phần thô | Có | Có | Có |
| Dự toán hoàn thiện | Có | Có | Có |
| Dự toán nội thất | Sơ bộ | Chi tiết | Chi tiết & tối ưu |
| Danh mục vật liệu đề xuất | Có | Có | Có |
| Bảng khối lượng & dự toán | Có | Có | Có |
| Xuất hồ sơ thi công | Có | Có | Có |
| Bộ dữ liệu tổng hợp gửi nhà thầu | Có | Có | Có |
| So sánh các phương án thiết kế | Không | Có | Có |
| So sánh chi phí giữa các phương án | Không | Có | Có |
| So sánh phương án vật liệu | Không | Có | Có |
| Tối ưu theo ngân sách mục tiêu | Không | Không | Có |
| Phân tích chênh lệch chi phí khi thay đổi | Không | Không | Có |
| Đề xuất vật liệu thay thế theo ngân sách | Không | Không | Có |
| Hỗ trợ tư vấn | Tư vấn online | Tư vấn ưu tiên | Tư vấn 1:1 cùng chuyên gia |
| Phối cảnh 3D chân thực | Không | Không | Có |
| Quà tặng đặc biệt | Không | Không | Bộ thiết bị vệ sinh châu Âu trị giá 100 triệu đồng |
| Điều kiện áp dụng | Không | Không | Áp dụng khi khách hàng chọn gói triển khai trọn gói cùng SVC |

### Giám sát

Nguồn: [Trang gói giám sát](https://vnz-bmt-savico-abcxyz.vercel.app/vi/plans/supervision).

| HẠNG MỤC | TỰ QUẢN LÝ | SVC CHECK | SVC CONTROL |
| --- | --- | --- | --- |
| Số lần kỹ sư kiểm tra thực tế | — | 6 | 12 |
| Thời gian áp dụng | Không giới hạn | 6 tháng | 6 tháng |
| Chi phí | 0 ₫ | 8.900.000 ₫ | 18.900.000 ₫ |
| Lưu hồ sơ dự án trên SAVICO | Có | Có | Có |
| Bảng điều khiển 6 giai đoạn | Không | Có | Có |
| Timeline thi công | Không | Có | Có |
| Theo dõi % tiến độ | Không | Có | Có |
| Hình ảnh hiện trường theo giai đoạn | Không | Có | Có |
| Theo dõi vật liệu đã cam kết | Không | Có | Có |
| Ghi nhận sai khác | Không | Có | Có |
| Ghi nhận phát sinh | Không | Có | Có |
| Theo dõi vấn đề cần xử lý | Không | Có | Có |
| Kiểm tra phần thô | Không | Theo các mốc chính | Tất cả các mốc |
| Kiểm tra đấm mốc ẩm | Không | 1 mốc | Có |
| Kiểm tra móng thấm | Không | 1 mốc | Có |
| Kiểm tra hoàn thiện | Không | 1 mốc | Có |
| Đối chiếu vật liệu thực tế | Không | Theo lần kiểm tra | Mọi lần kiểm tra |
| Báo cáo sau mỗi lần kiểm tra | Không | Có | Có |
| Nghiệm thu theo giai đoạn | Không | Theo các mốc chính | Đầy đủ |
| Theo dõi xử lý lỗi | Không | Không | Có |
| Kiểm tra tổng thể trước bàn giao | Không | Có | Có |
| Biên bản bàn giao | Không | Có | Có |
| Báo cáo tổng kết công trình | Không | Có | Có |

| HẠNG MỤC | GIÁ ĐỀ XUẤT | GHI CHÚ |
| --- | --- | --- |
| Thêm 01 lượt kiểm tra | 990.000đ – 1.290.000đ | Khi cần thêm mốc ngoài gói |
| Kiểm tra khẩn / ngoài kế hoạch | 1.490.000đ | Tùy khối lượng bố trí sự việc |
| Ngoài khu vực phục vụ chuẩn | Theo thực tế | Tính thêm phí di chuyển nếu có |
| Gia hạn dự án trên 6 tháng | Theo tháng / phạm vi | Báo giá theo tiến độ còn lại |

### Đối chiếu với quyết định nghiệp vụ

- Thiết kế: giá trang BASIC 399.000đ, PLUS 1.490.000đ, PRO 3.990.000đ là giá giao diện mua một lần, chưa phải giá tháng/năm đã chốt. Không nhập làm cấu hình bán.
- Lượt chỉnh sửa đã bỏ; so sánh và tối ưu ngân sách đã hoãn. Trang vẫn quảng bá các mục này.
- Phân mức bố trí công năng và dự toán nội thất trên trang đã bị thay thế bằng mức chi tiết chung giữa các gói đủ quyền tạo. Quyền 3D vẫn độc lập, Admin cấu hình, không mặc định chỉ PRO.
- Mua một lần/lượt không hết hạn trên trang không khớp subscription tháng/năm và bỏ lượt dư mỗi kỳ. Cần sửa nội dung bán khi triển khai.
- Giám sát: giới hạn 6 tháng và phí gia hạn trên trang không khớp gói theo dự án không thời hạn đã chốt. Lịch và quản lý lượt giám sát vẫn offline; các mục theo dõi số hóa chưa tự trở thành quyền cần code.
- Trang Tự quản lý ghi tự cập nhật tiến độ 6 giai đoạn, trong khi bảng so sánh đánh dấu không có bảng điều khiển 6 giai đoạn. Thẻ CONTROL liệt kê báo cáo tổng kết là điểm bổ sung, nhưng bảng ghi CHECK cũng có. Giữ nguyên hai quan sát, chưa tự chọn cách hiểu.
- Tên “kiểm tra đấm mốc ẩm” và “kiểm tra móng thấm” được chép đúng nhãn trang; có dấu hiệu lỗi nội dung, chưa tự sửa thành tên nghiệp vụ khác.
- Quà tặng PRO trị giá 100 triệu chỉ áp dụng khi chọn triển khai trọn gói cùng SVC; không tự cấp quà khi mua PRO. Tư vấn, quà tặng và các dịch vụ thực tế chưa được thêm vào danh mục ba quyền thiết kế.
- Trang giám sát ghi phạm vi nhà ở đến 3 tầng, 300 m² sàn và bán kính phục vụ 30 km; ngoài phạm vi tính thêm phí. Đây là điều kiện dịch vụ trên trang, chưa được chốt thành validator backend. Không bao gồm kỹ sư thường trực.
- Cả hai trang có trợ lý AI hiển thị 10/10 tin nhắn hôm nay; chưa có căn cứ coi đây là hạn mức entitlement theo gói. Không tự thêm quyền chatbot.
- Các đầu ra trên trang như dự toán phần thô/hoàn thiện, vật liệu, bảng khối lượng, hồ sơ và dữ liệu gửi nhà thầu là đầu vào để chốt đặc tả bộ kết quả; thống kê không tự xác nhận tất cả đã được thiết kế đầy đủ.

## Điểm tiếp tục hiện tại — đã chốt mua lại cùng gói trước hạn

Người dùng chọn **1** sau ví dụ PLUS năm đã dùng 2 tháng, còn 10 tháng: đổi sang PLUS tháng ngay khi hoàn tất, bỏ phần thời gian/lượt dư cũ và trả đủ giá tháng mới. Chiều tháng sang năm cũng theo cùng chính sách. Đây là quyết định mới nhất, thay thế lịch chuyển cuối kỳ trong các bảng và báo cáo lịch sử bên dưới.

- **Đã xác nhận:** mọi lần đổi sang gói khác hoặc đổi tháng/năm cùng gói đều áp dụng ngay khi hoàn tất hợp lệ; không cần bậc hoặc so giá. Bắt đầu kỳ mới, cấp đủ hạn mức, bỏ lượt dư và thời gian cũ, trả đủ giá kỳ mới. Không còn luồng hẹn/hủy/sửa lịch đổi cuối kỳ.
- **Đã đồng bộ:** BR-SUB-021 là quy tắc chung; BR-SUB-018–020 được đánh dấu đã thay thế. BR-SUB-002/004/006/013/014/015 cập nhật dẫn chiếu/phạm vi. STORY-SUB-001/ALT-19, AC-067/068 và ST-SUB-100/101 mô tả mô hình mới. AC-049–066 của Story 001, AC-018–021 của Story 002 và ST-SUB-079–099 giữ làm lịch sử, không nghiệm thu hoặc tái sử dụng mã.
- **Đã xác nhận thêm:** tác vụ AI đã tiếp nhận trước khi đổi gói tiếp tục hoàn tất theo bộ quyền lúc tiếp nhận, kể cả 3D; yêu cầu mới dùng quyền gói mới. Lượt thuộc kỳ cũ; timeout/kết quả muộn không đổi. Đã cập nhật BR-SUB-003/008/021, STORY-SUB-001/AC-069 và ST-SUB-102; không hỏi lại.
- **Đã xác nhận mua lại:** người dùng chọn 1; cùng gói/cùng chu kỳ được mua lại trước hạn, trả đủ giá, bắt đầu kỳ mới ngay, bỏ thời gian/lượt dư cũ. AC-070 và ST-SUB-103 kiểm tra; đã cập nhật BR-SUB-021/015.
- **Đã xác nhận danh mục tạm thời:** chỉ gồm tạo thiết kế (lượt), tra cứu mẫu (lượt) và phối cảnh 3D (bật/tắt); người dùng đã đồng ý. Cơ chế mức tính năng đã chốt hỗ trợ vẫn giữ, chưa tự thêm một quyền phân mức cụ thể.
- **Cần làm rõ tiếp:** phạm vi các nhánh bổ sung và nội dung đầu ra AI. Mốc chốt giao dịch/thanh toán vẫn để sau; chưa tuyên bố toàn bộ nghiệp vụ sẵn sàng triển khai.

Các phần rà soát trước quyết định này bên dưới là lịch sử; không dùng các câu hỏi bậc hoặc lịch chuyển đã bỏ để hỏi lại. Chưa chạy ứng dụng, chưa thực thi test hoặc sửa code.

## Rà soát trước khi chốt đổi chu kỳ: đổi gói ngay, không phân loại nâng/hạ

Mục này được ghi sau khi hợp nhất tài liệu. Người dùng chọn phương án 1: đổi sang gói thiết kế khác ngay khi hoàn tất hợp lệ, bắt đầu kỳ mới, trả đủ giá gói đích, cấp đủ lượt mới và bỏ lượt dư cũ. Người dùng yêu cầu rà toàn bộ nghiệp vụ vì lo xung đột. Quyết định này thay thế cách phân loại nâng/hạ để chọn thời điểm chuyển; không dùng bậc hoặc so giá cho mục đích đó. Các bảng và báo cáo bên dưới còn chứa quy tắc trước thay đổi, cần đọc theo kết quả đối chiếu ở mục này. Tại thời điểm rà soát này chưa đồng bộ. Trạng thái mới nhất và các mã thay thế nằm ở mục đầu tài liệu.

### Đã xác nhận

- Đổi sang gói khác áp dụng ngay khi hoàn tất hợp lệ, không phân biệt giá hoặc quyền lợi tăng/giảm. Chọn gói chưa phải hoàn tất.
- Bắt đầu kỳ mới, trả đủ giá kỳ đích, cấp đủ hạn mức mới, bỏ lượt dư cũ. Không cộng thời gian cũ hoặc khấu trừ thời gian chưa dùng theo cách xử lý ngay đã chọn.
- Chưa có câu trả lời mới về chỉ đổi tháng/năm trong cùng gói. Không tự suy ra quyết định đổi gói đã bao gồm trường hợp này.

### Kết quả rà soát tác động

Đã quét nội dung 121 file nghiệp vụ: 3 Story, 20 BR, 94 đặc tả System Test, 1 discovery và 3 tài liệu phần hoãn; đối chiếu các nhánh đổi gói với quy tắc về quyền, lượt, vòng đời và quản trị. Đây là rà soát tài liệu, không phải kiểm thử ứng dụng. Các số lượng lịch sử ở cuối tài liệu không phải số lượng hiện tại.

| Nhóm | Tác động của quyết định mới | Tài liệu cần đồng bộ |
|---|---|---|
| Bậc gói | Bỏ phân loại nâng/hạ và các ràng buộc bậc phục vụ phân loại. Câu hỏi khóa bậc R04 và phần bậc của R05 không còn cần trả lời. Không bỏ thứ tự mức của một quyền lợi; đó là khái niệm khác. | BR-SUB-020; STORY-SUB-002/AC-018–AC-020; ST-SUB-091–093. |
| Thời điểm đổi gói | Quy tắc hạ chờ hết kỳ mâu thuẫn trực tiếp với lựa chọn mới. Dùng chung luồng đổi ngay cho mọi gói đích hợp lệ, kể cả gói ít quyền hơn. | BR-SUB-018, BR-SUB-019; STORY-SUB-001/AC-049–AC-054, AC-058–AC-061; ST-SUB-079–084, 088–090, 094. |
| Chỉ đổi chu kỳ | BR-SUB-015 vẫn ghi cả hai chiều chờ hết kỳ. Nếu giữ, vẫn có yêu cầu chờ và thao tác hủy; nếu đổi ngay thì phải thay cả nhánh này. Chưa tự chọn thay người dùng. | BR-SUB-015; STORY-SUB-001/AC-055–AC-057, AC-062, AC-064; ST-SUB-085–087, 095, 097. |
| Yêu cầu hạ đang chờ | Mô hình mới không tạo lịch hạ cuối kỳ cho lần đổi sang gói khác. Các ca hủy/sửa yêu cầu hạ không còn dùng nguyên trạng. Không đồng nhất việc bỏ lịch chuyển với bỏ trạng thái giao dịch đang xử lý. Chưa có bằng chứng dữ liệu vận hành cần chuyển đổi. | BR-SUB-018/019; STORY-SUB-001/AC-054, AC-058, AC-061, AC-064–AC-065; ST-SUB-084, 088, 094, 097–098. |
| Giá/quyền đã chốt và ngừng bán | Giữ quyền đã cấp và chặn yêu cầu mới vào gói ngừng bán vẫn đúng. Ngoại lệ cho yêu cầu hẹn cuối kỳ phải thu hẹp theo nhánh còn tồn tại. Không tự bỏ nguyên tắc giữ bản quyền lợi của kỳ hoặc dùng bản mới nhất; mốc chốt giao dịch vẫn cần thiết kế. | BR-SUB-004, BR-SUB-013, BR-SUB-015/019; STORY-SUB-001/AC-063, AC-066; STORY-SUB-002/AC-021; ST-SUB-096, 099. |
| Kỳ và hạn mức | Giữ tối đa một subscription hiệu lực, kỳ mới bắt đầu tại T, đủ hạn mức, không chuyển lượt dư. Mở rộng cách xử lý vốn dành cho nâng sang mọi lần đổi gói; sửa các dẫn chiếu chỉ nói nâng/hạ. | BR-SUB-002, BR-SUB-006, BR-SUB-014, BR-SUB-018; ST-SUB-080–082. |
| Tác vụ đang chạy | Lượt đã giữ vẫn thuộc kỳ cũ; thành công ghi ở kỳ cũ, lỗi giải phóng ở kỳ cũ, không cộng sang kỳ mới. Khi đổi sang gói thiếu 3D, cần chốt rõ bộ quyền dùng để hoàn tất tác vụ đã tiếp nhận; quy tắc kế toán lượt chưa tự đủ để xác định bộ đầu ra. | BR-SUB-003, BR-SUB-004, BR-SUB-008, BR-SUB-016; ST-SUB-005–006, 015–016, 069–070. |
| Dữ liệu cũ và giám sát | Không có căn cứ xóa kết quả cũ vì đổi gói. Giữ quyền truy cập dữ liệu đã có; thao tác mới kiểm tra gói mới. Gói giám sát theo dự án, hoàn thành/mở lại, lịch offline không bị đổi bởi chính sách thiết kế. | BR-SUB-007, BR-SUB-009–012, BR-SUB-017; STORY-SUB-003. |

### Đề xuất chưa chốt

- Cho cả đổi tháng/năm cùng gói áp dụng ngay để dùng chung một luồng. Trước khi xác nhận, phải nêu rõ khách đang dùng năm sẽ mất thời gian còn lại nếu chủ động đổi ngay sang tháng.
- Tác vụ AI đã tiếp nhận hợp lệ hoàn tất theo bộ quyền đã ghi nhận lúc tiếp nhận, kể cả quyền 3D; thao tác mới dùng quyền gói mới. Đây là đề xuất làm rõ đầu ra khi giảm quyền, không phải quyết định mới đã xác nhận.
- Trước bước hoàn tất đổi gói, hiển thị giá phải trả, ngày hết hạn mới, lượt và thời gian cũ sẽ mất. Đây là đề xuất giao diện, chưa thêm thành điều kiện nghiệp vụ đã chốt.

### Cần làm rõ trước khi đồng bộ mô hình mới

1. Chỉ đổi chu kỳ trong cùng gói: đổi ngay như đổi gói hay vẫn đợi hết kỳ? Đây là câu tiếp theo cần trả lời.
2. Xác nhận cách hoàn tất tác vụ đang chạy khi gói mới thiếu quyền mà tác vụ cũ đã dùng; không hỏi lại cách tính lượt ở kỳ cũ.
3. Cùng gói và cùng chu kỳ mà muốn mua lại ngay để có lượt mới: có cho phép không? Quyết định đổi sang gói khác chưa bao phủ việc này; không tự biến thành gia hạn, mua thêm lượt hoặc làm mới miễn phí.

Giao dịch chưa hoàn tất không được làm mất gói cũ hoặc cấp lượt mới chỉ vì khách bấm chọn. Chống xử lý lặp, hai lần hoàn tất đồng thời và thao tác chuyển thất bại thuộc thiết kế giao dịch; phải giữ giới hạn một gói hiệu lực và không cấp lặp. Các chính sách hoàn tiền/tranh chấp, kỳ tương lai đã trả trước và thời điểm chốt giá vẫn thuộc phần thanh toán đã để sau; quyết định đổi ngay không tự giải quyết các phần này.

Đọc phần đầu để tiếp tục trao đổi. Phần lịch sử ở cuối giữ nguồn và bằng chứng của các lần rà soát trước; không dùng câu hỏi hoặc nhận xét cũ làm danh sách cần hỏi lại.

- [Tiếp tục trao đổi](#tiep-tuc)
- [Đã xác nhận, cần làm rõ và đề xuất chưa chốt](#quyet-dinh)
- [Phần đã để sau và trạng thái tài liệu](#pham-vi)
- [Chi tiết các quyết định và căn cứ](#chi-tiet)
- [Nguồn và khảo sát giao diện](#nguon)
- [Tài liệu nháp liên quan](#tai-lieu)
- [Lịch sử rà soát và bộ câu hỏi](#lich-su)


<a id="tiep-tuc"></a>

## Bắt đầu từ đây khi quay lại

Người dùng có thể gửi:

> Tiếp tục chốt nghiệp vụ Subscription/Entitlement BMT. Đọc bmt-documentation/discovery/subscription-entitlements.md và các tài liệu được dẫn trước khi hỏi tiếp. Đừng hỏi lại các quyết định đã chốt.

Agent cần đọc AGENTS.md áp dụng và dùng skill prepare-feature cùng vietnamese-clear-writing trong bmt-be/.claude/skills/. Đọc tài liệu liên quan và các tham chiếu của chúng theo hướng dẫn dự án. Chỉ đang thiết kế nghiệp vụ và tài liệu; chưa được chuyển sang triển khai code, tích hợp thanh toán hoặc tự cấp gói thủ công.

## Điểm dừng chính xác

**Đã xác nhận R03 cho yêu cầu đang chờ:** Người dùng chọn 1: hạ gói hoặc chỉ đổi chu kỳ giữ giá và toàn bộ quyền lợi đã chốt, dù Admin công bố bản khác trước ngày chuyển. Xem BR-SUB-004, BR-SUB-015, BR-SUB-019, STORY-SUB-001/AC-066 và [ST-SUB-099](../systemtest/ST-SUB-099.md). Thời điểm giao dịch/thu tiền chưa được chốt. Phần chọn bản khi nâng gói không được tự coi là đã xác nhận từ câu trả lời này.

**Cập nhật lượt tiếp tục:** Người dùng chọn **1** cho R01 ở cả hai chiều: đang chờ hạ gói mà muốn chỉ đổi chu kỳ, hoặc đang chờ chỉ đổi chu kỳ mà muốn hạ gói, đều phải hủy yêu cầu cũ thành công trước khi tạo yêu cầu mới. Khi chưa hủy, từ chối yêu cầu mới và giữ nguyên yêu cầu cũ. Đã cập nhật BR-SUB-015, BR-SUB-019, STORY-SUB-001/AC-064 và [ST-SUB-097](../systemtest/ST-SUB-097.md). R02 đã được chốt tiếp: chỉ đổi tháng/năm của gói đích trong yêu cầu hạ đang chờ cũng phải hủy rồi tạo lại. Yêu cầu mới phải hợp lệ tại thời điểm gửi; gói đích đã ngừng bán thì không tạo lại được. Xem BR-SUB-019, STORY-SUB-001/AC-065 và [ST-SUB-098](../systemtest/ST-SUB-098.md).

Câu về ngừng bán đã chốt trước đó vẫn giữ nguyên: yêu cầu chuyển đã ghi nhận hợp lệ trước lúc gói đích ngừng bán vẫn được thực hiện nếu chưa hủy và đủ điều kiện khác. Chặn yêu cầu mới; hủy rồi tạo lại sau lúc ngừng bán không được hưởng ngoại lệ.

Không còn câu R03 về yêu cầu đang chờ cần trả lời. Người dùng yêu cầu rà lại các quyết định để tránh hỏi lặp; đã đồng bộ các ghi chú lỗi thời. Sau lượt rà soát, người dùng yêu cầu gộp ba file discovery thành tài liệu này. Chưa có quyết định nghiệp vụ mới sau R03.

<a id="quyet-dinh"></a>

## Bảng quyết định và phần còn mở

Đã đối chiếu ghi chú trao đổi, ghi chú tiếp tục, 3 Story, 20 BR và các đặc tả System Test hiện có. Lượt rà soát trước khi hợp nhất đã sửa ghi chú lỗi thời và thêm ST-SUB-099 cho quyết định giữ bản giá/quyền lợi; không đánh giá code hoặc chạy ứng dụng. Các nhận xét “còn thiếu” và số lượng trong báo cáo ngày 17/09 là lịch sử, không phải danh sách câu hỏi hiện tại.

### Đã xác nhận — không hỏi lại

| Nhóm | Quyết định dùng để đối chiếu câu hỏi | Nguồn |
|---|---|---|
| Phạm vi gói | Thiết kế thuộc tài khoản, dùng chung giữa dự án; tối đa một gói thiết kế hiệu lực. Giám sát gắn cố định từng dự án, tối đa một gói đang thực hiện/dự án; được dùng cùng thiết kế. | BR-SUB-001, BR-SUB-006, BR-SUB-009 |
| Kỳ và lượt | Tháng/năm trong cùng gói; giá và hạn mức riêng, quyền bật/tắt và mức dùng chung. Kỳ tính theo lịch và giờ Việt Nam; thiếu ngày lấy cuối tháng, đúng giờ kết thúc là hết hạn. Lượt dư không chuyển kỳ; hạn mức năm dùng cả năm. | BR-SUB-002, BR-SUB-014, BR-SUB-015 |
| Tạo thiết kế | Giữ một lượt khi tiếp nhận; đủ kết quả, đã lưu và mở được mới tính dùng; lỗi giải phóng. Không chờ khách hài lòng. Qua kỳ xử lý ở kỳ giữ; quá thời gian thất bại, không trả kết quả muộn hoặc trừ lại. Thất bại được thử lại cùng dự án; thành công muốn phương án khác phải tạo dự án mới. | BR-SUB-003, BR-SUB-016, BR-SUB-017 |
| Tra cứu và tạo/lưu dự án | Danh sách/tìm kiếm công khai, không tính lượt; chi tiết cần đăng nhập, gói/quyền/lượt, mỗi lần trả được nội dung tính một lượt. Tạo/lưu cần gói, quyền tạo và lượt sẵn dùng; dùng hết hoặc giữ hết đều chặn, tạo/lưu không tự trừ lượt. | BR-SUB-007, BR-SUB-017 |
| Sau hết hạn | Xem dữ liệu cũ có quyền truy cập, tải tệp đã có và xuất PDF từ kết quả cũ; không sửa/lưu thông tin dự án hoặc bắt đầu Gen AI mới. | BR-SUB-007 |
| Danh mục và quyền | Hệ thống định nghĩa quyền; Admin cấu hình. Chỉ hai loại lượt tạo mới/tra cứu; không có tính năng chỉnh sửa sau thành công. 3D là bật/tắt, thuộc cùng bộ kết quả, không có lượt riêng. Dự toán nội thất và bố trí công năng cùng mức chi tiết. Thiếu/tắt quyền không được dùng; thiếu mức không tự cấp mức thấp nhất. | BR-SUB-005, BR-SUB-008, BR-SUB-017 |
| Công bố | Lưu nháp rồi Công bố; giữ quyền kỳ đang dùng và gói giám sát đã cấp. Giá VND lớn hơn 0; hạn mức ít nhất 1 hoặc không giới hạn. Được chọn tạo mới, tra cứu hoặc cả hai. Không có quyền chỉ được lưu nháp, không được Công bố. Giá/hạn mức bán do Admin nhập sau. | BR-SUB-004, BR-SUB-005, BR-SUB-008, BR-SUB-015 |
| Chuyển gói | Nâng có hiệu lực ngay khi hoàn tất, bắt đầu kỳ mới, cấp đủ lượt, bỏ lượt dư và trả đủ giá kỳ mới, không khấu trừ thời gian cũ. Hạ và chỉ đổi tháng/năm chờ hết kỳ. Nâng/hạ đều được chọn luôn chu kỳ đích. | BR-SUB-018, BR-SUB-019, BR-SUB-015 |
| Đổi yêu cầu đang chờ | Các tình huống đã hỏi đều hủy trước rồi tạo lại: đổi gói đích, đổi chu kỳ đích khi hạ, chuyển giữa hạ và chỉ đổi chu kỳ, muốn nâng khi đang chờ. Không tự thay thế, khôi phục yêu cầu đã hủy hoặc gia hạn. Không tách thêm ví dụ tháng/năm để hỏi lại cùng chính sách. | ST-SUB-088, ST-SUB-094, ST-SUB-095, ST-SUB-097, ST-SUB-098 |
| Bản giá/quyền lợi | Đăng ký/gia hạn dùng bản quyền lợi đã chốt. Hạ gói hoặc chỉ đổi chu kỳ đang chờ giữ cả giá và toàn bộ quyền lợi đã chốt dù có bản công bố mới; không hỏi lại theo từng quyền hoặc theo chiều đổi tháng/năm. | BR-SUB-004, BR-SUB-015, BR-SUB-019, ST-SUB-099 |
| Bậc và ngừng bán | Admin đặt bậc riêng, không phân loại theo giá/chu kỳ, không tự kế thừa quyền theo bậc. Gói đã từng có khách dùng không đổi bậc. Ngừng bán chặn yêu cầu mới nhưng giữ gói đã cấp và yêu cầu chuyển hợp lệ đã ghi nhận; hủy rồi tạo lại không giữ ngoại lệ. | BR-SUB-020, BR-SUB-013 |
| Giám sát | Không có thời hạn/chu kỳ; Admin hoặc nhân viên hiện phụ trách dự án hoàn thành/mở lại. Mở lại cần lý do và không chồng gói; không yêu cầu dữ liệu lịch/lượt offline. | BR-SUB-011, BR-SUB-012 |

### Các ghi chú lỗi thời đã sửa

- BR-SUB-001 còn ghi thời điểm tính lượt khác và hết hạn chưa rõ: thay bằng tham chiếu quy tắc tra cứu, hiệu lực và quyền sau hết hạn đã chốt.
- BR-SUB-004 còn ghi hạ gói/đổi chu kỳ chưa chốt: ghi đúng thời điểm chuyển và chính sách giữ bản mới được xác nhận.
- BR-SUB-005 còn ghi chung mọi kết hợp quyền đều chưa chốt: phân biệt các thao tác tạo, 3D và tra cứu đã rõ với quyền mới chưa có trong danh mục.
- BR-SUB-009 và STORY-SUB-003 còn đặt đổi công trình thành câu hỏi mở: sửa theo phạm vi gắn cố định; không tự thêm ngoại lệ sửa gán nhầm.
- BR-SUB-015, BR-SUB-019, STORY-SUB-001 và discovery còn ghi sửa yêu cầu đang chờ chưa chốt: đối chiếu lại R01/R02 và các quy tắc hủy trước đã có.
- ST-SUB-001 còn chờ chốt thao tác/thời điểm trừ: cụ thể hóa bằng Gen AI thành công theo BR-SUB-003, không hỏi lại người dùng.

### Cần làm rõ — chỉ phần chưa có quyết định

| Nhóm | Phần còn mở thực sự | Giới hạn khi hỏi |
|---|---|---|
| R04–R05 | Khóa bậc gói chưa từng được dùng nhưng đã có yêu cầu chuyển vào; lưu nháp chưa đặt bậc; tái sử dụng bậc gói ngừng bán; điều kiện công bố còn lại. | Gộp thành nhóm cấu hình; không hỏi lại khóa bậc gói đã được dùng, bậc riêng, giá dương hoặc hạn mức tối thiểu. |
| Q01–Q03 | Quyền còn lại thực sự cần kiểm soát và danh sách đầu ra bắt buộc để đánh giá đủ kết quả. | Không hỏi lại 3D, hai loại lượt, mức chi tiết chung hoặc tiêu chí thành công. Giá trị quyền từng gói do Admin cấu hình; không ép chốt bảng giá bán. |
| Q08, Q10–Q13 | Có đưa dùng thử, mua thêm/tặng/bù lượt, quyền riêng, hủy/tạm dừng/thu hồi gói, màn hình lịch sử/thông báo và thao tác quản trị bổ sung vào đợt này không. | Hỏi phạm vi theo nhóm trước; chưa xác nhận không có nghĩa đã hoãn hoặc phải triển khai. |
| Phần còn lại của R03 | Bản giá/quyền lợi của lần nâng chưa được câu trả lời về yêu cầu chờ cuối kỳ xác nhận; mốc giao dịch cụ thể chưa thiết kế. | Không tự mở rộng quyết định mới sang nâng. Khi bàn giao dịch nâng, chỉ hỏi khác biệt có tác động thật; không lặp câu giữ bản cho hạ/đổi chu kỳ. |
| Thiết kế kỹ thuật và vận hành | Cơ chế chống lặp/đồng thời, đo thời gian chờ, hiệu năng và lưu trữ. | Cơ chế kỹ thuật giao TDD; chính sách lưu dữ liệu/cam kết vận hành chỉ hỏi khi cần, không tự đặt con số. |

Q04, Q05 và Q07 không còn là nhóm câu hỏi chung. Định dạng xuất khác chỉ xem xét nếu có yêu cầu thực tế. Thanh toán, kích hoạt/gia hạn, hoàn tiền/tranh chấp, mốc ngày qua nhiều kỳ, lịch/lượt giám sát, so sánh thiết kế và tối ưu ngân sách vẫn thuộc phần đã để sau. Chỉnh sửa sau Gen AI thành công là phần đã bỏ, không phải nợ để mở lại.

### Đề xuất chưa chốt

Tiếp tục bằng nhóm cấu hình bậc R04–R05, sau đó chốt phạm vi các quyền/tính năng còn lại. Trước mỗi câu hỏi, đối chiếu bảng “Đã xác nhận” ở trên và BR liên quan; chỉ hỏi khi không tìm thấy quyết định bao phủ tình huống. Không đánh dấu toàn bộ nghiệp vụ hoàn tất khi còn các mục cần làm rõ.

<a id="pham-vi"></a>

## Đã để làm sau — không mở lại tự động

- Tích hợp thanh toán, nhận/kích hoạt và gia hạn gói, thanh toán lỗi/hoàn tiền/tranh chấp, lịch gia hạn qua các tháng bị thiếu ngày. Không tự thêm luồng Admin cấp gói thủ công.
- [Lịch và lượt giám sát offline](../debt/supervision-offline.md).
- [AI gợi ý tối ưu ngân sách](../debt/ai-budget-optimization.md).
- [So sánh các thiết kế](../debt/design-comparison.md).
- Chỉnh sửa thiết kế sau Gen AI thành công là tính năng đã bỏ, không phải nợ để tự làm lại.

## Trạng thái tài liệu và cách tiếp tục

- Tài liệu là bản nháp, chưa phê duyệt. Metadata người phụ trách/reviewer/approver chưa đủ; không tự đặt tên. Chưa có TDD hoặc Unit Test cho subscription.
- Các System Test là đặc tả Markdown, chưa được chạy. Không có kiểm thử ứng dụng hay triển khai code trong các lượt xác nhận này. Chỉ kiểm tra cấu trúc tài liệu, liên kết và tham chiếu.
- Mã cao nhất tại điểm dừng: BR-SUB-020; ST-SUB-099; STORY-SUB-001/AC-066; STORY-SUB-002/AC-021. Trước khi tạo mới vẫn phải kiểm tra file hiện có để tránh trùng mã.
- Không tái sử dụng AC-026–AC-029 của STORY-SUB-001 và ST-SUB-046–049, ST-SUB-054 đã rút do bỏ lượt chỉnh sửa. BR-SUB-010 và ST-SUB-024/025 thuộc giám sát đã hoãn.
- Mỗi lượt hỏi ngắn, một tình huống rõ, thường hai lựa chọn; giải thích ví dụ nếu người dùng nói “rõ hơn”. Sau câu trả lời, cập nhật Story, BR, đặc tả test và tài liệu discovery này. Không hỏi lại phần đã chốt trừ khi có mâu thuẫn thực tế mới.
- Khi đã xử lý các nhóm còn mở hoặc xác nhận để làm sau, tóm tắt phạm vi đã chốt. Không tự thêm câu hỏi vô hạn về tình huống ngoài phạm vi, không tự tuyên bố hoàn tất toàn bộ nghiệp vụ lúc vẫn còn quyết định quan trọng chưa rõ.
- Đây là file bàn giao trong máy, không phải bản sao trên dịch vụ bên ngoài. Lần này không commit, push, import hoặc công bố tài liệu. Nếu mở phiên mới, cần mở đúng workspace BMT có các file này.

<a id="chi-tiet"></a>

## Chi tiết các quyết định và căn cứ

- Thiết kế nghiệp vụ đổi gói (nâng/hạ gói) và đổi chu kỳ tháng/năm ngay trong đợt hiện tại theo lựa chọn của người dùng. Đã chốt nâng gói có hiệu lực ngay sau khi hoàn tất; không chờ gói cũ hết hạn. Đã chốt kỳ mới bắt đầu tại thời điểm nâng hoàn tất; ví dụ nâng tháng ngày 15/9 thì kết thúc 15/10, không giữ ngày hết hạn cũ. Đã chốt bỏ lượt dư chưa dùng của gói cũ, cấp đủ hạn mức gói mới; ví dụ cũ còn 3 và mới cấp 20 thì có 20. Tác vụ đã giữ lượt tiếp tục được xử lý ở kỳ cũ theo BR-SUB-003, không cộng/trừ vào kỳ mới. Đã chốt khi nâng trả đủ giá kỳ mới, không trừ tiền cho thời gian gói cũ còn lại; xem [ST-SUB-082](../systemtest/ST-SUB-082.md). Hạ gói đã chốt giữ gói hiện tại đến hết kỳ rồi chuyển từ kỳ tiếp theo theo [BR-SUB-019](../businessrule/BR-SUB-019.md), đặc tả [ST-SUB-083](../systemtest/ST-SUB-083.md). Khách được tự hủy yêu cầu hạ gói đang chờ trước thời điểm chuyển, không tự gia hạn gói hiện tại; xem [ST-SUB-084](../systemtest/ST-SUB-084.md). Đã chốt tháng sang năm trong cùng gói từ lúc hết kỳ tháng; bỏ lượt tháng dư, cấp hạn mức năm dùng cả kỳ, xem [ST-SUB-085](../systemtest/ST-SUB-085.md). Đã chốt năm sang tháng cũng chờ hết kỳ năm, bỏ lượt năm dư và cấp hạn mức tháng; xem [ST-SUB-086](../systemtest/ST-SUB-086.md). Đã chốt nút “Hủy yêu cầu đổi chu kỳ” cho cả hai chiều trước thời điểm chuyển: xử lý hủy thành công thì bỏ yêu cầu ngay, giữ nguyên kỳ đang dùng và không tự gia hạn; xem [ST-SUB-087](../systemtest/ST-SUB-087.md). Cách đổi lựa chọn đang chờ đã chốt tại ST-SUB-097 và ST-SUB-098; điều kiện giao dịch còn cần thiết kế; xem [ST-SUB-081](../systemtest/ST-SUB-081.md). Xem [BR-SUB-018](../businessrule/BR-SUB-018.md), [ST-SUB-079](../systemtest/ST-SUB-079.md). Chỉ ghi nhận phạm vi thiết kế; tích hợp thanh toán, luồng nhận gói và gia hạn vẫn thuộc giai đoạn sau. Xem [BR-SUB-006](../businessrule/BR-SUB-006.md), [BR-SUB-015](../businessrule/BR-SUB-015.md).

- Kỳ thiết kế mới dùng bản quyền lợi đã chốt khi đăng ký/gia hạn, không tự lấy bản công bố mới nhất lúc kỳ bắt đầu. Ví dụ chốt 20 lượt rồi Admin công bố 30 lượt thì kỳ đã chốt vẫn nhận 20. Áp dụng cho cả hạn mức, quyền bật/tắt và mức tính năng, không chỉ số lượt. Thời điểm chốt cụ thể sẽ thiết kế cùng thanh toán. Xem [BR-SUB-004](../businessrule/BR-SUB-004.md), [ST-SUB-007](../systemtest/ST-SUB-007.md).

- Khách chưa đăng nhập hoặc chưa có gói vẫn được tìm kiếm/xem danh sách mẫu, không cần tài khoản hoặc dự án và không mất lượt. Mở chi tiết phải đăng nhập, có gói còn hiệu lực, quyền tra cứu và lượt sẵn dùng hoặc không giới hạn. Không tự cấp tài khoản, gói hoặc lượt miễn phí. Xem thêm [ST-SUB-078](../systemtest/ST-SUB-078.md). Xem [BR-SUB-017](../businessrule/BR-SUB-017.md), [ST-SUB-077](../systemtest/ST-SUB-077.md).

- Khách mới chưa từng có gói thiết kế không được tạo dự án hoặc lưu thông tin dự án, kể cả lưu nháp; phải có gói còn hiệu lực trước. Đã chốt phải có quyền tạo thiết kế; gói chỉ có tra cứu không được tạo/lưu dự án theo [ST-SUB-074](../systemtest/ST-SUB-074.md). Đã chốt nếu đã dùng hết lượt tạo thiết kế thì cũng chặn tạo/lưu thông tin, kể cả lưu nháp hoặc biểu mẫu mở trước đó; xem [ST-SUB-075](../systemtest/ST-SUB-075.md). Đã chốt toàn bộ lượt còn lại đang giữ cũng tạm chặn tạo/lưu. Nếu tác vụ lỗi trả lượt trong kỳ còn hiệu lực, khách được gửi lại yêu cầu khi đủ quyền và lượt; tạo/lưu thông tin không giữ/trừ lượt. Xem [ST-SUB-076](../systemtest/ST-SUB-076.md). Gen AI vẫn cần gói, quyền và lượt hợp lệ. Xem [BR-SUB-007](../businessrule/BR-SUB-007.md), [ST-SUB-073](../systemtest/ST-SUB-073.md).

- Hoãn tính năng đặt các kết quả thiết kế cạnh nhau để so sánh; không đưa quyền so sánh vào danh mục đợt này. Khách vẫn xem từng kết quả riêng theo quyền truy cập đã chốt. Xem [nợ nghiệp vụ](../debt/design-comparison.md).

- Hoãn AI gợi ý giảm chi phí/tối ưu ngân sách trong đợt hiện tại; không đưa quyền này vào danh mục đang triển khai. Dự toán nội thất vẫn làm như đã chốt; không bắt buộc có gợi ý giảm chi phí để tính đủ bộ kết quả. Xem [nợ nghiệp vụ](../debt/ai-budget-optimization.md).

- Bố trí công năng có cùng mức chi tiết giữa các gói có quyền tạo thiết kế; không chia cơ bản/nâng cao theo tên hoặc giá gói. Phương án vẫn phụ thuộc dữ liệu dự án. Tiêu chí mức chi tiết chung chưa chốt. Quyền phối cảnh 3D chân thực vẫn riêng theo gói, không bị thay đổi bởi quyết định này. Xem [BR-SUB-008](../businessrule/BR-SUB-008.md), [ST-SUB-072](../systemtest/ST-SUB-072.md).

- Phần dự toán nội thất trong kết quả AI có cùng mức chi tiết giữa các gói có quyền tạo thiết kế; không chia sơ bộ/chi tiết theo gói. Chưa xác định mức chi tiết chung hoặc danh sách hạng mục cụ thể. Không suy ra các dự án có cùng số tiền dự toán, không cấp quyền tạo thiết kế cho gói chỉ có tra cứu. Xem [BR-SUB-008](../businessrule/BR-SUB-008.md), [ST-SUB-071](../systemtest/ST-SUB-071.md).

- Khách bấm Gen AI một lần; toàn bộ kết quả AI trả cùng lúc, phối cảnh 3D là một phần của bộ kết quả theo quyền gói. Không tách thao tác tạo 3D hoặc trả phần thiết kế trước rồi bổ sung 3D sau một lần đã báo thành công. Một bộ kết quả thành công chỉ tính 1 lượt tạo mới theo quy tắc đã chốt. Xem [BR-SUB-003](../businessrule/BR-SUB-003.md), [ST-SUB-070](../systemtest/ST-SUB-070.md).

- Phối cảnh 3D chân thực là quyền dạng bật/tắt riêng theo gói; Admin chọn gói nào được tạo ảnh này. Thiếu quyền hoặc đã tắt thì không được tạo. Chưa chốt gói cụ thể nào được bật, không mặc định riêng PRO. Không thêm lượt 3D riêng hoặc mở lại Gen AI trên dự án đã thành công. Xem [BR-SUB-008](../businessrule/BR-SUB-008.md), [ST-SUB-069](../systemtest/ST-SUB-069.md).

- Các gói thiết kế khác cả số lượt lẫn tính năng được dùng. Admin cấu hình quyền bật/tắt hoặc mức tính năng riêng giữa các gói; tháng/năm trong cùng bản gói vẫn dùng chung các quyền này. Đã chốt quyền tạo phối cảnh 3D chân thực; phần còn lại của danh mục và cách chia cho từng gói chưa chốt. Các mức xem/xoay 3D từng nêu chỉ là ví dụ; không tự lấy toàn bộ bảng trên trang làm cấu hình chính thức. Xem [BR-SUB-005](../businessrule/BR-SUB-005.md), [STORY-SUB-002](../userstory/STORY-SUB-002.md), [ST-SUB-009](../systemtest/ST-SUB-009.md).

- Quyền dạng mức tính năng chưa được cấu hình trong gói thì không cho dùng tính năng đó, không tự cấp mức thấp nhất. Admin phải thêm quyền và chọn mức cụ thể; vẫn giữ quyền của kỳ đã cấp. Ví dụ xem/xoay mô hình 3D chỉ để giải thích, không phải danh mục đã chốt. Xem [BR-SUB-005](../businessrule/BR-SUB-005.md), [ST-SUB-068](../systemtest/ST-SUB-068.md).

- Quyền bật/tắt chưa được thêm vào gói hoặc đã tắt thì khách không được dùng tính năng tương ứng. Admin phải thêm và bật quyền; vẫn kiểm tra bản quyền lợi đã cấp và các điều kiện sử dụng khác. Xem [BR-SUB-005](../businessrule/BR-SUB-005.md), [ST-SUB-067](../systemtest/ST-SUB-067.md).

- Gói chưa có quyền lợi được lưu nháp nếu các dữ liệu khác hợp lệ, nhưng không được Công bố. Phải thêm ít nhất một quyền lợi hợp lệ; không bắt buộc có cả hai quyền tạo mới/tra cứu. Xem [BR-SUB-008](../businessrule/BR-SUB-008.md), [ST-SUB-066](../systemtest/ST-SUB-066.md).

- Giá tháng và giá năm của gói thiết kế đều phải lớn hơn 0; không cho lưu/công bố giá 0 hoặc âm, không cấu hình gói thiết kế miễn phí. Admin vẫn nhập giá bán cụ thể sau. Đợt này chỉ dùng VNĐ cho cả giá tháng và năm, không cấu hình loại tiền khác. Chính sách khuyến mãi/dùng thử chưa chốt. Xem [BR-SUB-015](../businessrule/BR-SUB-015.md), [ST-SUB-064](../systemtest/ST-SUB-064.md).

- Admin được chọn gói chỉ có tạo thiết kế, chỉ có tra cứu mẫu hoặc cả hai. Quyền dạng lượt không được chọn thì không được sử dụng, không tự có lượt mặc định. Việc bỏ quyền khỏi cấu hình không sửa kỳ đã cấp. Xem [BR-SUB-008](../businessrule/BR-SUB-008.md), [ST-SUB-063](../systemtest/ST-SUB-063.md).
- Quyền dạng lượt đưa vào gói phải có hạn mức ít nhất 1 lượt hoặc không giới hạn; không cho nhập 0. Đây là hạn mức cấu hình, không phải số dư còn lại của khách sau sử dụng. Xem [BR-SUB-005](../businessrule/BR-SUB-005.md), [ST-SUB-062](../systemtest/ST-SUB-062.md).

- Gen AI thất bại chưa có kết quả: khách được chủ động thử lại trên cùng dự án, giữ thông tin đã nhập. Mỗi lần vẫn kiểm tra gói còn hiệu lực, quyền và lượt sẵn dùng; đủ điều kiện thì giữ 1 lượt tạo mới, thành công tính đã dùng, lỗi giải phóng. Không tự thử lại hoặc bỏ qua điều kiện vì lần trước từng hợp lệ. Xem [BR-SUB-017](../businessrule/BR-SUB-017.md), [ST-SUB-060](../systemtest/ST-SUB-060.md), [ST-SUB-061](../systemtest/ST-SUB-061.md).

- Sau khi dự án đã Gen AI thành công và có kết quả, muốn phương án khác khách phải tạo dự án mới, nhập thông tin rồi Gen AI; không Gen AI thêm phương án trên dự án cũ. Dự án mới vẫn dùng lượt chung của tài khoản. Xem [BR-SUB-017](../businessrule/BR-SUB-017.md), [ST-SUB-059](../systemtest/ST-SUB-059.md).

- Khi subscription thiết kế hết hạn, khách không được sửa/lưu thông tin dự án như tên, ghi chú hoặc thông tin đầu vào. Vẫn được xem, tải tệp đã có và xuất PDF từ kết quả cũ theo quyền truy cập. Xem [BR-SUB-007](../businessrule/BR-SUB-007.md), [ST-SUB-057](../systemtest/ST-SUB-057.md).

- Sau khi subscription hết hạn, khách vẫn được tải tệp kết quả đã tạo/lưu sẵn trước đó mà tài khoản có quyền truy cập. Không cần gia hạn chỉ để tải lại. Người dùng xác nhận thêm: vẫn được xuất PDF từ kết quả thiết kế cũ dù chưa có PDF trước khi hết hạn; không mở quyền Gen AI tạo/chỉnh sửa thiết kế. Xem [BR-SUB-007](../businessrule/BR-SUB-007.md), [ST-SUB-055](../systemtest/ST-SUB-055.md).

- Với tạo thiết kế mới bằng AI, thành công để tính lượt là: AI tạo đủ kết quả, hệ thống đã lưu và khách có thể mở xem. Không chờ khách mở/duyệt hoặc hài lòng; khách không thích kết quả không tự được hoàn lượt. Danh sách đầu ra bắt buộc vẫn cần định nghĩa theo luồng tính năng. Xem [BR-SUB-003](../businessrule/BR-SUB-003.md), [BR-SUB-017](../businessrule/BR-SUB-017.md), [ST-SUB-053](../systemtest/ST-SUB-053.md).

- Tra cứu mẫu: mỗi lần mở chi tiết một mẫu tính 1 lượt. Tìm kiếm và xem danh sách không dùng lượt. Chỉ tính lượt khi hệ thống trả được nội dung chi tiết; lỗi không tải được nội dung thì không mất lượt. Cơ chế giữ lượt và xác nhận trả nội dung sẽ thiết kế trong TDD. Xem [BR-SUB-017](../businessrule/BR-SUB-017.md), [ST-SUB-050](../systemtest/ST-SUB-050.md), [ST-SUB-051](../systemtest/ST-SUB-051.md).


- Đã chốt lại chỉ có hai loại lượt: tạo thiết kế mới và tra cứu mẫu, mỗi loại có hạn mức riêng. Không có tính năng chỉnh sửa sau khi Gen AI ra kết quả, nên loại bỏ quyền, số dư và toàn bộ cơ chế giữ/trừ/hoàn lượt chỉnh sửa. Quyết định này thay thế các trao đổi trước, không phải hoãn triển khai. Xem [BR-SUB-017](../businessrule/BR-SUB-017.md) và [ST-SUB-045](../systemtest/ST-SUB-045.md).

- Bắt đầu thiết kế tính năng quản lý gói subscription, quyền lợi và entitlement.
- Dùng trang gói dịch vụ SAVICO làm nguồn tham khảo.
- Phạm vi bao gồm cả gói thiết kế và gói giám sát. Danh mục quyền lợi cần xác định phù hợp phần giám sát tối giản; lịch và quản lý lượt trên nền tảng đã hoãn theo quyết định sau đó. Việc đưa giám sát vào phạm vi không tự xác nhận toàn bộ chính sách đang hiển thị trên trang.
- Thiết kế dùng subscription định kỳ. Giám sát là ngoại lệ đã chốt lại: gói theo dự án cố định, không có chu kỳ hoặc ngày hết hạn, kết thúc bằng nút hoàn thành khi dự án xong.
- Một gói thiết kế có cả hai lựa chọn tháng/năm, mỗi lựa chọn có giá và hạn mức riêng. Không tạo thành hai gói riêng và không tự lấy cấu hình năm bằng 12 lần cấu hình tháng. Hạn mức làm mới theo kỳ subscription, không có kỳ làm mới riêng.
- Hạn mức lượt thiết kế làm mới mỗi kỳ subscription; lượt dư kỳ trước không cộng dồn. Ví dụ hạn mức 10 lượt, cuối kỳ còn 3 thì kỳ mới có 10 lượt, không phải 13. Quy tắc áp dụng khi tài khoản có kỳ subscription mới hợp lệ, không mặc định tự gia hạn.
- Với tạo thiết kế: giữ trước một lượt khi tiếp nhận xử lý; thành công thì tính lượt đã dùng, lỗi thì giải phóng lượt giữ. Tác vụ chạy qua hai kỳ vẫn tính vào kỳ đã giữ lượt; lỗi thì giải phóng lượt giữ ở kỳ cũ, không cộng sang kỳ mới. Tác vụ đã bắt đầu hợp lệ được tiếp tục sau khi hết hạn dù chưa gia hạn; thành công tính vào kỳ đã giữ, lỗi giải phóng lượt nhưng không dùng lại lượt đã hết hạn. Đợt này chưa hỗ trợ khách hàng hủy tác vụ tạo thiết kế. Tác vụ vượt thời gian chờ cấu hình tự chuyển thất bại và giải phóng lượt; kết quả muộn không tự tính lượt lại. Kết quả muộn không đưa cho khách; tác vụ giữ trạng thái thất bại. Chưa chốt con số thời gian chờ; chưa áp dụng mặc định cho quyền lợi khác.
- Quyền lợi thiết kế được cấp theo tài khoản và dùng chung cho các dự án của tài khoản. Giám sát là ngoại lệ đã xác nhận: subscription gắn với một công trình, không chia quyền lợi hoặc lượt sang công trình khác. Với thiết kế, các dự án cùng sử dụng hạn mức của tài khoản, không được cấp lại một hạn mức riêng cho mỗi dự án.
- Admin sửa gói theo luồng lưu nháp → kiểm tra → Công bố. Chỉ bản đã công bố mới được chọn để cấp quyền. Thiết kế giữ kỳ hiện tại, kỳ mới dùng bản đã chốt khi đăng ký/gia hạn; không tự thay bản đã chốt khi có lần Công bố mới. Giám sát đã cấp giữ quyền cũ, thay đổi chỉ áp dụng cho gói cấp mới.
- Với thiết kế, khi Admin sửa quyền lợi của gói, giữ nguyên quyền lợi và hạn mức đã cấp cho kỳ hiện tại; kỳ tiếp theo hợp lệ dùng bản đã chốt khi đăng ký/gia hạn. Yêu cầu hạ gói hoặc đổi chu kỳ đang chờ giữ cả giá và quyền lợi đã chốt theo R03; mốc giao dịch cụ thể để thiết kế cùng thanh toán.
- Hỗ trợ ba dạng quyền lợi: bật/tắt tính năng, hạn mức lượt và mức tính năng (ví dụ cơ bản/nâng cao). Mức cao bao gồm các mức thấp hơn của cùng quyền lợi; mức thấp không mở mức cao. Chưa chốt danh mục cụ thể, tên và thứ tự các mức hoặc cách kết hợp nhiều quyền lợi cho một thao tác.
- Mỗi tài khoản có tối đa một subscription thiết kế đang hiệu lực; mỗi công trình có tối đa một gói giám sát đang hiệu lực. Tài khoản có thể đồng thời dùng thiết kế và nhiều gói giám sát cho các công trình khác nhau. Quyết định này thay thế giới hạn giám sát theo tài khoản trước đó. Quy tắc không hạn chế lưu lịch sử. Nghiệp vụ đổi gói được thiết kế trong đợt này; các quyết định chuyển đã chốt và phần còn mở được tổng hợp ở đầu tài liệu.
- Khi subscription thiết kế hết hạn và chưa gia hạn, khách vẫn xem được dự án và kết quả đã tạo thuộc quyền truy cập của mình. Không được bắt đầu thao tác mới cần quyền subscription, kể cả còn lượt dư. Tác vụ đã bắt đầu hợp lệ trước khi hết hạn được tiếp tục đến khi hoàn thành dù chưa gia hạn.
- Với mỗi quyền lợi dạng lượt, Admin có thể cấu hình số lượt cụ thể hoặc không giới hạn. Không giới hạn lượt vẫn yêu cầu subscription còn hiệu lực để bắt đầu thao tác mới và không tự mở các quyền khác.
- Danh mục quyền lợi gắn với tính năng do hệ thống định nghĩa sẵn. Admin chọn quyền lợi và cấu hình giá trị cho từng gói, không tự tạo định nghĩa quyền lợi mới. Danh mục cụ thể còn cần chốt.
- “Lượt kiểm tra thực tế” tương ứng với quyền lợi “Số lần kỹ sư kiểm tra thực tế”: một buổi kỹ sư đến kiểm tra tại công trình tính là một lượt. Đơn vị tính và phương án giữ/trừ đã trao đổi được lưu tham khảo; quản lý lịch và lượt trên nền tảng hiện HOÃN, tiếp tục vận hành offline theo [nợ nghiệp vụ](../debt/supervision-offline.md).
- Hoàn thành gói giám sát: Admin hoặc nhân viên phụ trách dự án được thực hiện. Khách hàng và nhân viên không phụ trách không có quyền.
- Mở lại gói giám sát đã hoàn thành: Admin hoặc nhân viên phụ trách được thao tác, bắt buộc ghi lý do; không được tạo hai gói đang thực hiện trên cùng dự án.
- Khi sửa quyền lợi gói giám sát, gói đã cấp giữ nguyên quyền lợi; thay đổi đã công bố chỉ áp dụng cho gói cấp mới. Mở lại cùng gói đã hoàn thành không phải cấp gói mới. Lịch và lượt vẫn vận hành offline.
- Khi Admin ngừng bán: ẩn gói khỏi danh sách đăng ký và chặn cấp mới; các gói đã cấp tiếp tục theo quyền lợi/vòng đời hiện có. Gia hạn thiết kế đã ngừng bán sẽ chốt cùng thanh toán sau.
- Kỳ thiết kế tính từ ngày bắt đầu có hiệu lực: tháng kết thúc cùng ngày tháng sau, năm kết thúc cùng ngày/tháng năm sau. Nếu thiếu ngày tương ứng thì lấy cuối tháng đích. Không theo lịch tháng/năm chung và không tự gia hạn. Giờ kết thúc giữ nguyên giờ bắt đầu theo giờ Việt Nam; đúng mốc kết thúc thì kỳ cũ hết hiệu lực. Mốc ngày khi gia hạn qua tháng ngắn sẽ chốt cùng luồng gia hạn.
- Nhân viên phụ trách được xác định theo phân công hiện tại của dự án. Nhân viên đó được hoàn thành/mở lại gói giám sát của dự án; không phân công riêng theo từng gói.
- Ngoài phạm vi đợt này: khách hàng hủy tác vụ tạo thiết kế đang chạy. Không thêm nút/API hủy; vẫn giữ quy tắc thành công tính lượt, lỗi giải phóng lượt.
- Trong cùng bản gói thiết kế, tháng/năm dùng chung quyền bật/tắt và mức tính năng; cấu hình ở cấp gói, không có giá trị riêng theo chu kỳ. Quy tắc giữ nguyên quyền lợi kỳ đã cấp vẫn áp dụng.
- Giá và hạn mức cụ thể sẽ do Admin nhập sau trên trang quản trị, lưu nháp rồi Công bố. Không cần chốt các con số trong đợt thiết kế này; không tự dùng trang tham khảo hoặc ví dụ kiểm thử làm dữ liệu bán chính thức. Danh mục định nghĩa quyền lợi hệ thống là phần riêng, không chuyển thành quyền Admin tự tạo quyền lợi.
- Quá thời gian chờ tạo thiết kế: tự đánh dấu thất bại và giải phóng lượt. Kỳ còn hiệu lực thì dùng lại lượt được; kỳ hết hạn thì không dùng lại hoặc cộng sang kỳ mới. Không tự trừ lại lượt khi kết quả đến muộn; không đưa kết quả đó cho khách hoặc gắn vào yêu cầu mới, tác vụ vẫn thất bại. Ví dụ 15 phút không phải mặc định đã chốt.
- Phạm vi thiết kế gồm quản trị gói/quyền lợi, cấp, kiểm tra và trừ lượt. Với giám sát, chỉ quản lý gói gắn với khách hàng/dự án và nút Hoàn thành gói giám sát; không triển khai lịch hoặc số dư/giữ/trừ/hoàn lượt. Thanh toán làm sau.
- Cách tài khoản nhận gói và gia hạn sẽ được xử lý khi tích hợp luồng thanh toán. Không bổ sung luồng Admin cấp hoặc gia hạn thủ công trong đợt này. Phần cấp quyền vẫn thuộc phạm vi đã chọn; sự kiện nào kích hoạt việc cấp quyền sẽ được làm rõ cùng luồng thanh toán.
- Nội dung trang về thanh toán một lần và lượt không hết hạn cần sửa theo mô hình định kỳ. Đây là yêu cầu thay đổi đã xác nhận; trang chưa được chỉnh sửa trong bước khảo sát này.
- Trao đổi và xác nhận nghiệp vụ trước; sau đó tạo Story, BR, System Test và chuyển sang thiết kế TDD, Unit Test theo quy trình dự án.

<a id="nguon"></a>

## Nguồn đã đọc

- [Gói thiết kế](https://vnz-bmt-savico-abcxyz.vercel.app/vi/plans).
- [Gói giám sát](https://vnz-bmt-savico-abcxyz.vercel.app/vi/plans/supervision), được liên kết từ trang gói thiết kế.
- [Điều khoản sử dụng](https://vnz-bmt-savico-abcxyz.vercel.app/vi/terms). Trang này ghi rõ nội dung còn là mẫu, cần có bản chính thức trước khi vận hành.
- Các template User Story, Business Rule và System Test trong `../templates/`.
- Mã nguồn `User`, `RoleNames` và `ApplicationDbContext` trong backend. DbContext hiện khai báo Users; tìm trong mã nguồn và test chưa thấy phần subscription/entitlement. Thư mục tài liệu chưa có Story/BR/TDD/test nghiệp vụ để kế thừa tại thời điểm khảo sát.

## Thông tin trên trang tại thời điểm khảo sát

Các số liệu dưới đây là quan sát lịch sử từ giao diện, chưa phải quyết định cho backend. Cột lượt chỉnh sửa không còn áp dụng: người dùng đã loại bỏ tính năng và lượt chỉnh sửa; cần gỡ quyền lợi này khỏi nội dung bán gói khi cập nhật trang.

| Gói thiết kế | Giá hiển thị | Phương án mới | Lượt chỉnh sửa | Lượt tra cứu mẫu |
|---|---|---|---|---|
| BASIC | 399.000đ | 3 | 3 | 20 |
| PLUS | 1.490.000đ | 10 | 10 | 50 |
| PRO | 3.990.000đ | 20 | 20 | 100 |

Tại thời điểm khảo sát, trang mô tả các gói thiết kế là mua một lần, các lượt không hết hạn và dữ liệu dự án lưu theo tài khoản. Mô tả mua một lần không còn phù hợp với quyết định dùng subscription định kỳ; cần thay nội dung về thời hạn và lượt sau khi chốt chính sách tương ứng. Việc dùng chung quyền lợi giữa các dự án đã được người dùng xác nhận riêng, không suy ra từ mô tả lưu dữ liệu trên trang. Các mức giá trong bảng chưa được xác nhận là giá theo tháng hay năm.

Tại thời điểm khảo sát, trang phân mức dự toán nội thất sơ bộ/chi tiết/chi tiết và tối ưu; bố trí công năng cơ bản/2D và 3D/nâng cao. Phân mức dự toán nội thất và bố trí công năng trên trang đã bị thay thế bởi quyết định dùng cùng mức chi tiết giữa các gói; cần sửa nội dung này khi cập nhật trang. Bảng so sánh từng đánh dấu tính năng so sánh dành cho PLUS và PRO; tối ưu ngân sách và phối cảnh 3D chân thực dành cho PRO. Người dùng đã hoãn cả so sánh và tối ưu ngân sách; không lấy các nội dung này làm quyền phải triển khai hoặc quyền đang bán trong đợt hiện tại. Các dấu có/không đã được kiểm tra trong nội dung bảng, không suy ra chỉ từ tên gói.

Quà tặng PRO đi kèm điều kiện chọn dịch vụ triển khai trọn gói; chưa thể coi đó là quyền tự động nhận ngay sau khi mua PRO. Đường dẫn chọn PLUS sử dụng tham số `plan=advanced`; cần xác định mã gói chính thức trước khi thiết kế contract.

Tab giám sát có lựa chọn tự quản lý miễn phí, SVC CHECK 8.900.000đ với 6 lượt kiểm tra và SVC CONTROL 18.900.000đ với 12 lượt. Hai gói trả phí ghi áp dụng theo dự án, tối đa 6 tháng. Trang còn có lượt kiểm tra mua thêm, kiểm tra ngoài kế hoạch, phí di chuyển và gia hạn theo báo giá. Người dùng đã xác nhận đưa gói giám sát vào phạm vi. Mô hình giám sát gắn với một công trình đã được xác nhận riêng. Giá và số lượt trên trang chưa được xác nhận là cấu hình chính thức. Mô tả tối đa 6 tháng không áp dụng cho thiết kế hiện tại: gói giám sát theo dự án không thời hạn, hoàn thành thủ công. Lịch và lượt trên nền tảng đã hoãn.

## Cách phân biệt khái niệm đang đề xuất

- **Gói dịch vụ:** thứ được đưa ra bán, gồm giá và các quyền lợi đi kèm.
- **Quyền lợi trong gói:** mô tả khách được dùng tính năng gì, ở mức nào và bao nhiêu lượt.
- **Subscription thiết kế của tài khoản:** ghi nhận tài khoản đang đăng ký gói nào, với chu kỳ tháng hoặc năm được cấu hình cho gói. Điều kiện hiệu lực còn cần làm rõ; luồng nhận gói và gia hạn để thiết kế cùng thanh toán sau. Quyền lợi thiết kế dùng chung giữa các dự án.
- **Gói giám sát của khách hàng:** gắn cố định với một dự án, không chu kỳ/ngày hết hạn; có nút hoàn thành khi dự án xong.
- **Entitlement — quyền sử dụng thực tế:** quyền khách đang có tại thời điểm kiểm tra, gồm quyền mở tính năng hoặc hạn mức còn được dùng theo chính sách đã chốt.

Đây là cách gọi để trao đổi, chưa phải quyết định về bảng dữ liệu. Chưa mặc định subscription phải tự gia hạn hoặc mọi quyền lợi đều có bộ đếm. Quyền tư vấn và quà tặng có thể cần quy trình thực hiện riêng, cần làm rõ trước khi thiết kế.

<a id="tai-lieu"></a>

## Tài liệu nháp từ phần đã chốt

- [BR-SUB-016](../businessrule/BR-SUB-016.md) và ST-SUB-042–ST-SUB-044: xử lý quá thời gian và kết quả đến muộn, không trừ/hoàn lượt lặp.

- [ST-SUB-041](../systemtest/ST-SUB-041.md): quyền bật/tắt và mức tính năng dùng chung cho tháng/năm.

- [BR-SUB-015](../businessrule/BR-SUB-015.md), [ST-SUB-039](../systemtest/ST-SUB-039.md) và [ST-SUB-040](../systemtest/ST-SUB-040.md): hai lựa chọn tháng/năm trong cùng gói thiết kế.

- [ST-SUB-038](../systemtest/ST-SUB-038.md): hết hạn đúng giờ bắt đầu theo giờ Việt Nam, kiểm tra tại ranh giới thời gian.

- [BR-SUB-014](../businessrule/BR-SUB-014.md), [ST-SUB-036](../systemtest/ST-SUB-036.md) và [ST-SUB-037](../systemtest/ST-SUB-037.md): tính ngày kết thúc kỳ từ ngày bắt đầu, bao gồm cuối tháng và năm nhuận.

- [BR-SUB-013](../businessrule/BR-SUB-013.md), [ST-SUB-034](../systemtest/ST-SUB-034.md) và [ST-SUB-035](../systemtest/ST-SUB-035.md): ngừng bán chặn cấp mới, giữ nguyên gói khách đã nhận.

- [ST-SUB-033](../systemtest/ST-SUB-033.md): quyền lợi giám sát đã cấp giữ nguyên, bản mới chỉ áp dụng khi cấp gói mới.

- [BR-SUB-012](../businessrule/BR-SUB-012.md) và ST-SUB-029–ST-SUB-032: mở lại gói với quyền phù hợp, lý do bắt buộc và không chồng gói đang thực hiện.

- [ST-SUB-026](../systemtest/ST-SUB-026.md) và [ST-SUB-028](../systemtest/ST-SUB-028.md): cho phép Admin/nhân viên phụ trách và từ chối người không có quyền hoàn thành gói.

- [Nợ nghiệp vụ giám sát](../debt/supervision-offline.md): hoãn quản lý lịch và lượt. BR-SUB-010, ST-SUB-024, ST-SUB-025 và AC-004/AC-005 của STORY-SUB-003 không thuộc nghiệm thu hiện tại.
- [BR-SUB-011](../businessrule/BR-SUB-011.md), [ST-SUB-026](../systemtest/ST-SUB-026.md), [ST-SUB-027](../systemtest/ST-SUB-027.md): gói không chu kỳ, hoàn thành thủ công theo dự án.

- [BR-SUB-010](../businessrule/BR-SUB-010.md) và [ST-SUB-025](../systemtest/ST-SUB-025.md): HOÃN giữ/trừ lượt trên nền tảng, lưu tham khảo cho giai đoạn sau.

- [ST-SUB-024](../systemtest/ST-SUB-024.md): HOÃN kiểm thử số lượt trên nền tảng.

- [ST-SUB-022](../systemtest/ST-SUB-022.md) và [ST-SUB-023](../systemtest/ST-SUB-023.md): cho phép nhiều công trình dùng gói riêng, chặn hai gói giám sát cùng hiệu lực trên một công trình.

- [STORY-SUB-003](../userstory/STORY-SUB-003.md), [BR-SUB-009](../businessrule/BR-SUB-009.md) và [ST-SUB-021](../systemtest/ST-SUB-021.md): giám sát chỉ dùng cho công trình được gắn.

- [ST-SUB-020](../systemtest/ST-SUB-020.md): một subscription thiết kế và một giám sát cùng có hiệu lực.

- [BR-SUB-008](../businessrule/BR-SUB-008.md) và [ST-SUB-019](../systemtest/ST-SUB-019.md): chọn quyền lợi từ danh mục hệ thống.

- [ST-SUB-017](../systemtest/ST-SUB-017.md) và [ST-SUB-018](../systemtest/ST-SUB-018.md): cấu hình không giới hạn lượt và kiểm tra hiệu lực subscription.

- [ST-SUB-015](../systemtest/ST-SUB-015.md) và [ST-SUB-016](../systemtest/ST-SUB-016.md): tác vụ hoàn thành sau khi hết hạn, chưa gia hạn.

- [BR-SUB-007](../businessrule/BR-SUB-007.md), [ST-SUB-013](../systemtest/ST-SUB-013.md) và [ST-SUB-014](../systemtest/ST-SUB-014.md): xem dữ liệu cũ và chặn thao tác mới khi hết hạn.

- [BR-SUB-006](../businessrule/BR-SUB-006.md) và [ST-SUB-012](../systemtest/ST-SUB-012.md): giới hạn thiết kế theo tài khoản và giám sát theo công trình.

- [ST-SUB-010](../systemtest/ST-SUB-010.md) và [ST-SUB-011](../systemtest/ST-SUB-011.md): kiểm tra mức cao bao gồm mức thấp, không áp dụng chiều ngược lại.

- [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-005](../businessrule/BR-SUB-005.md) và [ST-SUB-009](../systemtest/ST-SUB-009.md): cấu hình ba dạng quyền lợi.

- [ST-SUB-008](../systemtest/ST-SUB-008.md): bản nháp không ảnh hưởng quyền lợi đang dùng hoặc cấp cho kỳ tiếp theo.

- [BR-SUB-004](../businessrule/BR-SUB-004.md) và [ST-SUB-007](../systemtest/ST-SUB-007.md): giữ nguyên quyền lợi kỳ hiện tại, kỳ mới dùng bản đã chốt khi đăng ký/gia hạn.

- [BR-SUB-003](../businessrule/BR-SUB-003.md): giữ lượt tạo thiết kế, chỉ tính lượt khi thành công.
- [ST-SUB-003](../systemtest/ST-SUB-003.md) và [ST-SUB-004](../systemtest/ST-SUB-004.md): kiểm tra tạo thành công và tạo lỗi.

- [ST-SUB-005](../systemtest/ST-SUB-005.md) và [ST-SUB-006](../systemtest/ST-SUB-006.md): kiểm tra tác vụ chạy qua hai kỳ khi thành công hoặc lỗi.
- [STORY-SUB-001](../userstory/STORY-SUB-001.md): dùng chung hạn mức subscription giữa các dự án của tài khoản.
- [BR-SUB-001](../businessrule/BR-SUB-001.md): phạm vi cấp quyền và dùng chung hạn mức theo tài khoản.
- [ST-SUB-001](../systemtest/ST-SUB-001.md): kiểm tra một dự án sử dụng lượt làm giảm số lượt chung của tài khoản.
- [BR-SUB-002](../businessrule/BR-SUB-002.md): làm mới hạn mức theo kỳ, không cộng dồn.
- [ST-SUB-002](../systemtest/ST-SUB-002.md): kiểm tra lượt dư không chuyển sang kỳ mới.

Các tài liệu này ghi phần dùng chung hạn mức và làm mới hạn mức đã xác nhận. Chưa hoàn thiện toàn bộ luồng subscription; thời điểm tính lượt cho tạo thiết kế đã chốt, các quyền lợi khác và điều kiện hiệu lực còn cần làm rõ. Luồng nhận gói và gia hạn sẽ thiết kế cùng thanh toán sau. Test chưa được thực thi.

Chưa tạo quy tắc hay kết quả test từ các lựa chọn chưa có câu trả lời. Thông tin người soạn, reviewer và approver sẽ được bổ sung khi hoàn thiện bộ tài liệu chính thức; không tự điền tên hoặc đánh dấu đã duyệt.

<a id="lich-su"></a>

## Lịch sử rà soát và bộ câu hỏi

Phần này lưu báo cáo ngày 17/09 và các cập nhật trước khi hợp nhất. Các số lượng, khoảng trống test và đề xuất thứ tự hỏi là ghi nhận tại thời điểm đó. Trạng thái hiện tại nằm trong [bảng quyết định](#quyet-dinh).

Ngày rà soát: 17/09/2026. Đây là báo cáo đối chiếu và danh sách câu hỏi, chưa phải quy tắc mới được người dùng xác nhận.

**Cập nhật sau rà soát:** Người dùng chốt không có tính năng chỉnh sửa sau khi Gen AI trả kết quả. Hệ thống chỉ có lượt tạo mới và tra cứu; toàn bộ quyết định về lượt chỉnh sửa trước đây đã bị thay thế. Đã rút AC-026–AC-029 của STORY-SUB-001 và các bản nháp ST-SUB-046–049, ST-SUB-054; không tái sử dụng các mã này. Các số lượng và kết quả kiểm tra ở báo cáo gốc bên dưới là lịch sử, không phải số lượng hiện tại. Tạo mới tính thành công khi đủ kết quả, đã lưu và mở xem được. Tra cứu tính mỗi lần trả được chi tiết mẫu; tìm kiếm/xem danh sách và lần mở lỗi không mất lượt.

**Kết luận:** BMT đã chốt phần lớn nguyên tắc về phạm vi cấp quyền, chu kỳ, hạn mức và vòng đời tác vụ tạo thiết kế. Vẫn còn những quyết định ảnh hưởng trực tiếp đến việc kiểm tra quyền và tính lượt. Cần làm rõ các điểm này hoặc xác nhận đưa chúng ra ngoài đợt hiện tại trước khi coi nghiệp vụ đã hoàn tất.

Đã đọc 3 User Story, 16 Business Rule, 44 đặc tả System Test, [ghi chú trao đổi](subscription-entitlements.md) và [nợ nghiệp vụ giám sát](../debt/supervision-offline.md). Trong đó BR-SUB-010 và ST-SUB-024/ST-SUB-025 thuộc phần đã hoãn. Không đánh giá mã ứng dụng hay chạy test trong lần rà soát này.

**Nguồn dùng để xây dựng bộ câu hỏi**

Các câu hỏi dưới đây do agent tổng hợp từ những tình huống trong tài liệu chính thức, rồi đối chiếu với BMT. Đây không phải một bộ câu hỏi chuẩn duy nhất hay yêu cầu BMT phải dùng các sản phẩm này.

| Nguồn | Nội dung dùng để rà soát |
|---|---|
| [Stripe — Entitlements](https://docs.stripe.com/billing/entitlements?dashboard-or-api=api) | Gắn tính năng với gói, xác định quyền đang hiệu lực và thời điểm áp dụng thay đổi quyền. |
| [OpenMeter — Entitlement](https://openmeter.io/docs/billing/entitlements/entitlement) | Phân biệt quyền bật/tắt, cấu hình tính năng và hạn mức sử dụng; kiểm tra quyền, làm mới hạn mức và tra cứu sử dụng. |
| [OpenMeter — Grant](https://openmeter.io/docs/billing/entitlements/grant) | Hạn dùng của lượt được cấp, lượt bổ sung, thứ tự sử dụng khi có nhiều nguồn và chuyển lượt dư. |
| [Chargebee — Subscription Entitlements](https://www.chargebee.com/docs/billing/2.0/entitlements/subscription-entitlements) | Quyền riêng của một subscription, ngoại lệ so với gói và thời hạn của ngoại lệ. |
| [Chargebee — Feature Management](https://www.chargebee.com/docs/billing/2.0/entitlements/feature-management) | Vòng đời định nghĩa quyền lợi, tác động của thay đổi và lịch sử người thao tác. |
| [Stripe — Subscription lifecycle](https://docs.stripe.com/billing/subscriptions/overview) | Trạng thái subscription, thanh toán chưa hoàn tất hoặc thất bại và quan hệ với quyền truy cập. |
| [Stripe — Modify subscriptions](https://docs.stripe.com/billing/subscriptions/change), [Cancel subscriptions](https://docs.stripe.com/billing/subscriptions/cancel) | Đổi gói, đổi chu kỳ, dùng thử, hủy ngay hoặc cuối kỳ, điều chỉnh tiền khi thay đổi. |

Các nhận xét về phần còn thiếu bên dưới là kết quả phân tích tài liệu BMT. Không lấy chính sách mặc định của nhà cung cấp làm quyết định của dự án.

**Bộ câu hỏi Q01–Q07 trong lần rà soát trước**

| Mã | Câu hỏi cần trả lời | Khoảng trống trong tài liệu và tác động |
|---|---|---|
| Q01 | **Đợt này hệ thống thực sự kiểm soát những quyền nào?** Tạo mới, tra cứu mẫu, xuất hồ sơ, 3D… gồm những mục nào? Mỗi quyền thuộc dạng nào, có những mức nào? Giám sát tối giản còn quyền nào cần kiểm tra trên nền tảng? | Đã xác nhận các gói thiết kế khác cả số lượt lẫn tính năng; không hỏi lại việc có cần phân biệt tính năng giữa các gói. [BR-SUB-008/Notes](../businessrule/BR-SUB-008.md) đã xác nhận quyền tạo phối cảnh 3D chân thực dạng bật/tắt, Admin chọn gói được dùng; dự toán nội thất và bố trí công năng đều có cùng mức chi tiết giữa các gói, không phân mức theo gói; AI gợi ý giảm chi phí/tối ưu ngân sách đã hoãn theo [nợ nghiệp vụ](../debt/ai-budget-optimization.md); so sánh kết quả cũng đã hoãn theo [nợ nghiệp vụ so sánh](../debt/design-comparison.md); các quyền khác và cách chia từng gói còn mở; [STORY-SUB-002/Context](../userstory/STORY-SUB-002.md) ghi quyền giám sát còn mở. Chưa thể xác định đầy đủ thao tác phải kiểm tra quyền. Chốt tên và ý nghĩa nghiệp vụ; mã kỹ thuật thiết kế sau. |
| Q02 | **Một thao tác cần đồng thời những quyền nào?** Với phần tạo phối cảnh 3D chân thực, đã chốt cần quyền 3D; toàn bộ kết quả AI trả cùng lúc, phối cảnh 3D là một phần theo quyền gói, không tạo hoặc trả riêng. Không thêm loại lượt 3D: vẫn chỉ có lượt tạo mới và tra cứu. | [BR-SUB-005/Except](../businessrule/BR-SUB-005.md) ghi chưa chốt cách kết hợp. Với quyền dạng mức, đã chốt chưa cấu hình thì không được dùng; không tự cấp mức thấp nhất. Với quyền bật/tắt, đã chốt chưa thêm hoặc đã tắt đều không cho dùng; Admin phải thêm và bật quyền, vẫn áp dụng theo bản quyền lợi đã cấp. Với quyền dạng lượt, đã chốt Admin được chọn tạo thiết kế, tra cứu hoặc cả hai; quyền không được chọn không được sử dụng và không tự có lượt mặc định theo [BR-SUB-008](../businessrule/BR-SUB-008.md). Không tự coi có lượt là được dùng mọi tính năng. Nếu Q01 phát hiện quyền trùng giữa thiết kế và giám sát, cần chốt nguồn quyền nào áp dụng, có cộng hay lấy mức cao hơn không; không đặt vấn đề này nếu không có quyền trùng. |
| Q03 | **Khi nào một yêu cầu được coi là tạo thiết kế thành công?** Cần lưu đủ kết quả để khách mở được hay chỉ cần dịch vụ tạo trả về? Nếu kết quả thiếu một phần hoặc khách không hài lòng thì có tính lượt không? | Đã chốt trong [BR-SUB-003](../businessrule/BR-SUB-003.md) và [BR-SUB-017](../businessrule/BR-SUB-017.md): đủ kết quả, đã lưu, có thể mở xem; không chờ khách duyệt hoặc hài lòng. Đã chốt toàn bộ kết quả AI trả cùng lúc, phối cảnh 3D là một phần theo quyền gói; không báo thành công cho bộ kết quả dở dang. Còn cần danh sách chi tiết các phần đầu ra theo quyền gói để kiểm tra “đủ kết quả”. |
| Q04 | **Một lượt tạo mới hoặc tra cứu tương ứng với việc gì?** Khi đã có kết quả mà muốn phương án khác thì bắt đầu từ đâu? | Đã chốt tạo mới giữ 1 lượt, thành công mới tính đã dùng; tra cứu mỗi lần trả được chi tiết mẫu tính 1 lượt, tìm kiếm/xem danh sách và lần mở lỗi không dùng lượt. Không có tính năng hoặc lượt chỉnh sửa. Đã chốt sau khi thành công muốn phương án khác phải tạo dự án mới, nhập thông tin rồi Gen AI. Đã chốt thất bại được chủ động thử lại trên cùng dự án, giữ thông tin đã nhập và kiểm tra lại hiệu lực gói/quyền/lượt. Không tự thử lại. Xem [BR-SUB-017](../businessrule/BR-SUB-017.md). |
| Q05 | **Sau khi gói hết hạn, “xem dữ liệu cũ” cụ thể cho phép những gì?** Mở xem đã chốt; tải tệp đã có, xuất lại hồ sơ và sửa dữ liệu dự án cần quyền còn hiệu lực hay được dùng tiếp? | Đã cập nhật [BR-SUB-007](../businessrule/BR-SUB-007.md): được xem và tải tệp kết quả đã có sẵn trước khi hết hạn, theo quyền truy cập. Đã xác nhận thêm: được xuất PDF từ kết quả cũ dù chưa có PDF trước khi hết hạn. Đã chốt không cho khách sửa/lưu thông tin dự án khi gói hết hạn. Chỉ còn định dạng xuất khác nếu danh mục có yêu cầu; không hỏi lại các thao tác đã chốt. |
| Q06 | **Một gói cần đủ dữ liệu gì mới được Công bố?** Khi thiếu quyền bắt buộc, sai kiểu giá trị hoặc thiếu cấu hình tháng/năm thì báo lỗi gì cho Admin? | [STORY-SUB-002/EXC-01](../userstory/STORY-SUB-002.md) đã chốt hạn mức 0 không hợp lệ: quyền dạng lượt đưa vào gói phải có ít nhất 1 lượt hoặc không giới hạn. Số dư còn lại về 0 do sử dụng không bị cấm. Admin được chọn một hoặc cả hai quyền tạo mới/tra cứu, không bắt buộc có cả hai. Đã chốt giá tháng/năm phải lớn hơn 0 theo [BR-SUB-015](../businessrule/BR-SUB-015.md); giá 0 hoặc âm bị từ chối khi lưu/công bố. Đã chốt chỉ dùng VNĐ cho giá tháng/năm; không hỗ trợ cấu hình giá bằng loại tiền khác trong đợt này. Đã chốt gói không có quyền lợi được lưu nháp nhưng không được Công bố; phải thêm ít nhất một quyền lợi hợp lệ theo [BR-SUB-008](../businessrule/BR-SUB-008.md). Các dữ liệu bắt buộc khác vẫn cần làm rõ. Không hỏi lại số tiền/số lượt bán cụ thể. Việc hỗ trợ số 0, mức hợp lệ và dữ liệu bắt buộc ảnh hưởng trực tiếp đến cấu hình sản phẩm; kiểu dữ liệu và mã lỗi thuộc TDD. |
| Q07 | **Nếu có nhiều lần Công bố trước kỳ mới, dùng bản nào?** | Đã chốt dùng bản quyền lợi ghi nhận khi đăng ký/gia hạn theo [BR-SUB-004](../businessrule/BR-SUB-004.md). Nếu đã chốt B có 20 lượt rồi công bố C có 30 lượt, kỳ mới vẫn dùng B, gồm cả quyền bật/tắt và mức tính năng. Thời điểm chốt cụ thể và tình huống giao dịch sẽ thiết kế cùng thanh toán. [ST-SUB-007](../systemtest/ST-SUB-007.md) kiểm tra không thay bằng bản mới nhất; không hỏi lại chính sách này. |

Thứ tự Q01 → Q07 là kế hoạch lịch sử. Dùng bảng quyết định và phần còn mở ở đầu tài liệu để chọn câu hỏi; không hỏi lại Q04, Q05 hoặc Q07 đã chốt. Mỗi lượt chỉ hỏi một nhóm, thường 1–3 câu. Không yêu cầu người dùng trả lời toàn bộ bảng một lần.

**Cần xác định có làm trong đợt này không — chưa coi là chức năng bắt buộc**

Đây là các nhánh phổ biến có thể phát sinh. Chưa thấy quyết định rõ trong bộ tài liệu đã đọc. Đề xuất trước tiên chọn làm ngay hoặc để sau; chỉ thiết kế chi tiết nhánh được chọn.

| Mã | Câu hỏi về phạm vi | Căn cứ và giới hạn |
|---|---|---|
| Q08 | Khách chưa từng có gói được dùng những gì? Có dùng thử không? | Đã chốt không cấu hình gói thiết kế miễn phí và khách mới chưa có gói không được tạo/lưu dự án, kể cả lưu nháp; cần gói còn hiệu lực trước theo [BR-SUB-007](../businessrule/BR-SUB-007.md). Đã chốt tạo/lưu dự án cần thêm quyền tạo thiết kế; gói chỉ có tra cứu không đủ điều kiện. Đã chốt khi dùng hết lượt tạo thiết kế cũng chặn tạo/lưu thông tin, kể cả lưu nháp; không tự tính lượt cho thao tác nhập thông tin. Đã chốt toàn bộ lượt còn lại đang giữ cũng tạm chặn tạo/lưu; lỗi trả lượt trong kỳ còn hiệu lực thì khách được gửi lại yêu cầu nếu đủ quyền và có lượt sẵn dùng. Không tự lưu yêu cầu bị từ chối, không giữ/trừ lượt chỉ vì lưu thông tin. Đã chốt khách đã đăng nhập chưa có gói được tìm kiếm/xem danh sách mẫu không mất lượt; mở chi tiết vẫn cần gói còn hiệu lực, quyền tra cứu và lượt sẵn dùng hoặc không giới hạn theo [BR-SUB-017](../businessrule/BR-SUB-017.md). Đã chốt khách chưa đăng nhập cũng được tìm kiếm/xem danh sách; mở chi tiết phải đăng nhập rồi kiểm tra gói/quyền/lượt. Dùng thử còn cần xác định phạm vi; không hỏi lại quyền tạo/lưu khi chưa có gói. |
| Q09 | Khi nâng/hạ gói hoặc đổi tháng/năm, chuyển lúc nào và xử lý quyền, lượt, thời gian còn lại thế nào? | Người dùng đã chọn thiết kế nghiệp vụ đổi gói/chu kỳ ngay trong đợt này; đề xuất để toàn bộ phần này cùng thanh toán không được chọn. [BR-SUB-006](../businessrule/BR-SUB-006.md), [BR-SUB-015](../businessrule/BR-SUB-015.md) đã ghi các thời điểm chuyển được xác nhận; chỉ còn những điều kiện cụ thể được nêu là chưa rõ. Đã chốt nâng gói có hiệu lực ngay khi hoàn tất theo [BR-SUB-018](../businessrule/BR-SUB-018.md). Đã chốt bắt đầu kỳ mới tại lúc nâng hoàn tất, tính đủ chu kỳ từ mốc đó, không giữ ngày hết hạn cũ. Đã chốt bỏ lượt dư chưa dùng của gói cũ và cấp đủ hạn mức gói mới; không khấu trừ lượt cũ đã dùng. Tác vụ qua kỳ theo BR-SUB-003. Đã chốt nâng gói trả đủ giá kỳ mới, không trừ tiền cho thời gian cũ chưa dùng; đặc tả [ST-SUB-082](../systemtest/ST-SUB-082.md). Hạ gói đã chốt từ kỳ tiếp theo, giữ quyền gói hiện tại đến hết kỳ theo [BR-SUB-019](../businessrule/BR-SUB-019.md), đặc tả [ST-SUB-083](../systemtest/ST-SUB-083.md). Đã chốt khách tự hủy yêu cầu hạ gói đang chờ trước thời điểm chuyển; hủy không tự gia hạn, xem [ST-SUB-084](../systemtest/ST-SUB-084.md). Đã chốt tháng sang năm trong cùng gói từ lúc hết kỳ tháng; bỏ lượt tháng dư, cấp hạn mức năm dùng cả kỳ theo BR-SUB-015 và [ST-SUB-085](../systemtest/ST-SUB-085.md). Đã chốt năm sang tháng cũng chờ hết kỳ năm, bỏ lượt năm dư và cấp hạn mức tháng; xem [ST-SUB-086](../systemtest/ST-SUB-086.md). Đã chốt khách tự hủy yêu cầu đổi chu kỳ đang chờ trước thời điểm chuyển ở cả hai chiều; giữ kỳ hiện tại, không tự gia hạn, xem [ST-SUB-087](../systemtest/ST-SUB-087.md). Đã chốt đổi gói đích của yêu cầu hạ gói đang chờ phải hủy yêu cầu cũ rồi chọn lại, không thay thế trực tiếp; xem [ST-SUB-088](../systemtest/ST-SUB-088.md). Đã chốt nâng gói được chọn luôn chu kỳ đích, bắt đầu ngay kỳ mới theo giá/hạn mức tương ứng; xem [ST-SUB-089](../systemtest/ST-SUB-089.md). Đã chốt hạ gói được chọn luôn chu kỳ đích, áp dụng cùng lúc từ kỳ tiếp theo; xem [ST-SUB-090](../systemtest/ST-SUB-090.md). Đã chốt phân loại nâng/hạ theo thứ tự Admin đặt, độc lập với giá và tháng/năm theo [BR-SUB-020](../businessrule/BR-SUB-020.md), [ST-SUB-091](../systemtest/ST-SUB-091.md). Đã chốt mỗi gói có bậc riêng, không cho hai gói ngang bậc; xem [ST-SUB-092](../systemtest/ST-SUB-092.md). Đã chốt khóa bậc gói đã có khách sử dụng; muốn phân cấp khác phải tạo gói mới, xem [ST-SUB-093](../systemtest/ST-SUB-093.md). Đã chốt phải hủy yêu cầu hạ gói đang chờ trước khi nâng, xem [ST-SUB-094](../systemtest/ST-SUB-094.md). Đã chốt phải hủy yêu cầu chỉ đổi chu kỳ đang chờ trước khi nâng, không tự hủy thay khách; xem [ST-SUB-095](../systemtest/ST-SUB-095.md). Bản giá áp dụng khi nâng, điều kiện giao dịch và các điều kiện còn lại về cấu hình bậc gói chưa được dùng còn mở; không suy ra cùng chính sách cho các trường hợp này. Tích hợp thanh toán vẫn làm sau. |
| Q10 | Có mua thêm lượt, tặng lượt, bù lượt hoặc mở quyền riêng cho một khách không? Nếu có, hết hạn lúc nào và dùng lượt nguồn nào trước? | [BR-SUB-004/Except](../businessrule/BR-SUB-004.md) chưa chốt ngoại lệ trong kỳ. Không đồng nghĩa với việc tự thêm luồng Admin cấp gói; luồng đó đã được để cùng thanh toán. Chỉ hỏi chi tiết nhiều nguồn lượt nếu BMT muốn hỗ trợ. |
| Q11 | Có hủy subscription, tạm dừng hoặc thu hồi quyền trước hạn không? Ai được thực hiện, tác vụ đang chạy xử lý thế nào? | Chưa có quy tắc trong các Story. **Khác với hủy tác vụ tạo thiết kế**, vốn đã xác nhận chưa hỗ trợ. Khóa tài khoản do quyền truy cập/an toàn tài khoản cần đối chiếu với module tài khoản nếu mở phạm vi. |
| Q12 | Khách và nhân viên cần xem số lượt sẵn dùng, đang giữ, đã dùng và lịch sử đến mức nào? Có thông báo gần hết lượt/sắp hết hạn không? | Các test kiểm tra số dư nhưng chưa có Story riêng cho màn hình tra cứu, phân quyền xem lịch sử hay thông báo. Không tự biến kiểm tra nội bộ thành yêu cầu xây màn hình đầy đủ. |
| Q13 | Có mở bán lại, xóa bản nháp hoặc khôi phục cấu hình gói cũ không? Ai cần xem lịch sử thay đổi? | [BR-SUB-013/Except](../businessrule/BR-SUB-013.md) chưa chốt mở bán lại. Ngừng bán không phải xóa gói đã cấp. Admin sửa/công bố đã chốt; không cần hỏi lại quyền này, chỉ hỏi nếu muốn thêm vai trò hoặc thao tác. |

**Đã chủ động để làm sau — không dùng để kết luận đợt này thiếu nghiệp vụ**

| Nhóm | Câu hỏi lưu cho giai đoạn phù hợp | Căn cứ |
|---|---|---|
| Cấp/kích hoạt và gia hạn | Sự kiện nào cấp gói? Thanh toán thành công nhưng cấp quyền thất bại thì xử lý ra sao? Có tự gia hạn không? Gia hạn muộn bắt đầu từ ngày nào? | [Ghi chú phạm vi](subscription-entitlements.md), [STORY-SUB-001/Out of Scope](../userstory/STORY-SUB-001.md). Không tự thêm Admin cấp/gia hạn thủ công. |
| Thanh toán lỗi, hủy và hoàn tiền | Có thời gian cho dùng tiếp khi chưa thu được tiền không? Khi hoàn tiền hoặc có tranh chấp thì quyền thay đổi thế nào? | Thanh toán ngoài phạm vi. Các tình huống này cần thiết kế khi mở luồng đó; chưa được tự cho thời gian dùng thêm. |
| Giá khi gia hạn | Khách cũ giữ giá cũ hay theo giá mới? Có tiếp tục gia hạn gói đã ngừng bán không? | [BR-SUB-004](../businessrule/BR-SUB-004.md) chỉ chốt thay đổi quyền, không chốt giá; [BR-SUB-013](../businessrule/BR-SUB-013.md) để gia hạn gói ngừng bán cùng thanh toán. |
| Mốc ngày qua nhiều kỳ | Bắt đầu 31/1 → 28/2 thì kỳ sau kết thúc 28/3 hay trở lại 31/3? | [BR-SUB-014/Notes](../businessrule/BR-SUB-014.md) đã chủ động để cùng gia hạn. Quy tắc một kỳ hiện tại vẫn rõ. |
| Lịch và lượt giám sát | Đặt/hủy/đổi lịch, kỹ sư vắng mặt, buổi dở dang, giữ/trừ/hoàn lượt và nhập dữ liệu offline thế nào? | [Nợ nghiệp vụ giám sát](../debt/supervision-offline.md). Không mở lại các câu hỏi này trong đợt hiện tại. |
| AI gợi ý giảm chi phí/tối ưu ngân sách | Có cần đề xuất đổi vật liệu, giảm hạng mục hoặc phương án khác để phù hợp ngân sách không? | Người dùng chọn chưa làm trong đợt này; dự toán nội thất vẫn giữ nguyên. Xem [nợ nghiệp vụ](../debt/ai-budget-optimization.md). Không mở lại trong đợt hiện tại. |
| So sánh kết quả thiết kế | Có đặt các kết quả cạnh nhau để đối chiếu không, gói nào được dùng? | Người dùng chọn hoãn; khách vẫn mở xem từng kết quả riêng. Xem [nợ nghiệp vụ](../debt/design-comparison.md). Không mở lại trong đợt hiện tại. |
| Giá và hạn mức bán cụ thể | BASIC/PLUS/PRO có giá và bao nhiêu lượt? | [BR-SUB-015](../businessrule/BR-SUB-015.md): Admin nhập sau. Không cần câu trả lời để thiết kế khả năng cấu hình. |

**Phần giao cho thiết kế kỹ thuật, không cần hỏi người dùng về cơ chế cài đặt**

- Chống trừ/hoàn lặp và chống dùng quá lượt khi nhiều yêu cầu đến cùng lúc. Kết quả nghiệp vụ phải tuân theo BR-SUB-003; chọn khóa, transaction hoặc cách tổ chức dữ liệu trong TDD.
- Phân biệt gửi lại cùng yêu cầu do mạng lỗi với khách chủ động tạo yêu cầu mới. Ý nghĩa thao tác lấy từ Q03/Q04; cách định danh yêu cầu và xử lý sự kiện lặp thuộc TDD.
- Chọn cơ chế xử lý khi thành công và hết thời gian chờ xảy ra sát nhau, bảo đảm chỉ một kết quả cuối cùng và không tính/hoàn hai lần theo BR-SUB-016.
- Lưu bản quyền đã cấp, kiểm tra quyền ở backend, đồng bộ dữ liệu kiểm tra quyền và xử lý lỗi phụ thuộc. Nếu muốn cho dùng tạm khi không xác minh được quyền thì đó là ngoại lệ nghiệp vụ mới, không được tự thêm.
- Ánh xạ Admin/nhân viên phụ trách vào phân quyền backend; dùng phân công hiện tại của dự án như đã chốt. Không cần hỏi lại ai được hoàn thành/mở lại.
- Xác định và thử nghiệm thời gian chờ phù hợp với luồng tạo thiết kế. Đã chốt ngày 26/09/2026: 15 phút tính từ lúc tiếp nhận, job rà tác vụ quá hạn mỗi 60 giây, cả hai là cấu hình (TDD-SUB-002/Architecture). Nếu ngưỡng này là cam kết với khách thì cần thống nhất trước vận hành.
- Chính sách lưu dữ liệu cũ, dữ liệu nội bộ đến muộn và lịch sử sử dụng cần đầu vào từ chính sách sản phẩm/vận hành. Quyền xem sau hết hạn không có nghĩa lưu vĩnh viễn; không tự quyết định xóa dữ liệu.

**Khoảng trống về tài liệu và đặc tả test**

Kiểm tra liên kết file nội bộ và các tham chiếu mã/section trong 63 tài liệu không phát hiện đích thiếu. Cả 42 AC đều có ít nhất một tham chiếu từ System Test; trong đó 2 AC giám sát đã hoãn, còn 40 AC thuộc phạm vi hiện tại. Có 44 đặc tả test, gồm 42 hiện hành và 2 đã hoãn. Đây chỉ là kết quả kiểm tra tài liệu, chưa chứng minh tất cả nhánh nghiệp vụ đã đủ hoặc hệ thống chạy đúng.

Các tình huống cần bổ sung đặc tả từ quy tắc đã có, không cần hỏi lại chính sách:

| Tình huống | Quy tắc đã có | Phần chưa thấy ca kiểm thử riêng |
|---|---|---|
| Chỉ còn một lượt nhưng hai dự án cùng gửi yêu cầu | [BR-SUB-003](../businessrule/BR-SUB-003.md) | Chỉ một yêu cầu giữ được lượt, không âm số dư. ST-SUB-003 hiện thử một yêu cầu. |
| Còn 0 lượt hoặc toàn bộ lượt đang được giữ | [BR-SUB-003](../businessrule/BR-SUB-003.md) | Chặn bắt đầu tác vụ mới, không gọi xử lý tạo rồi mới phát hiện thiếu lượt. |
| Kết quả thành công/lỗi bình thường được gửi lặp | [BR-SUB-003](../businessrule/BR-SUB-003.md) | Không trừ/hoàn lặp ngoài trường hợp timeout. ST-SUB-042/043 đã có lặp cho timeout/kết quả muộn. |
| Tác vụ đã thành công rồi mới nhận xử lý timeout | [BR-SUB-016/Then](../businessrule/BR-SUB-016.md) | Không chuyển sang thất bại hoặc trả lại lượt đã dùng. |
| Công bố thay đổi quyền bật/tắt hoặc mức tính năng | [BR-SUB-004](../businessrule/BR-SUB-004.md) | Quyền kỳ hiện tại giữ nguyên, kỳ mới nhận bản mới; ST-SUB-007 chủ yếu thử hạn mức. |
| Nhân viên đã được gỡ phân công vẫn gửi yêu cầu hoàn thành/mở lại | [BR-SUB-011](../businessrule/BR-SUB-011.md), [BR-SUB-012](../businessrule/BR-SUB-012.md) | Kiểm tra bằng phân công hiện tại. ST-SUB-028/031 kiểm tra người không phụ trách nhưng chưa có bước thay đổi phân công. |
| Mở lại gói giám sát sau khi danh mục đã đổi quyền | [BR-SUB-004](../businessrule/BR-SUB-004.md), [BR-SUB-012](../businessrule/BR-SUB-012.md) | Giữ đúng quyền đã cấp, không lấy quyền của bản vừa công bố. ST-SUB-029 chưa kiểm tra kết hợp này. |

Những ca phụ thuộc câu trả lời Q01–Q07 chỉ được viết kết quả mong đợi sau khi quyết định tương ứng rõ. Báo cáo này ghi danh sách cần bổ sung, chưa tạo hoặc chạy các ca mới.

Các chỗ dễ khiến agent hỏi lặp hoặc hiểu sai:

- Mục “Các quyết định đang chờ trả lời” trong ghi chú cũ liệt kê cả quyền hoàn thành/mở lại giám sát và cấu hình tháng/năm đã chốt. Đã thay bằng danh sách câu hỏi thật sự còn mở và liên kết tới báo cáo này.
- STORY-SUB-002 đặt cả “chưa chốt danh mục/điều kiện công bố” trong Out of Scope. Đây chưa phải bằng chứng người dùng đồng ý hoãn; Q01/Q06 vẫn cần quyết định hoặc giới hạn phạm vi rõ.
- STORY-SUB-003/ALT-01 và BR-SUB-009/Except còn nói “chưa chốt đổi công trình”, trong khi quyết định hiện tại là gắn cố định một dự án. Không coi đó là yêu cầu xây chức năng chuyển dự án; nếu sau này cần sửa gán nhầm, phải xác định đó là ngoại lệ mới.
- Một số câu vẫn gọi gói giám sát là “subscription”, nhắc “lượt” hoặc “cùng kỳ”. Các phần hoãn đã có nhãn nhưng phần đang dùng nên thống nhất tên “gói giám sát”, tránh kéo lại mô hình định kỳ/bộ đếm.
- BR-SUB-001/Notes còn nói kiểm tra hết hạn cần làm rõ, trong khi BR-SUB-007 và BR-SUB-014 đã chốt. Các thao tác xem, tải tệp, xuất PDF và sửa/lưu thông tin ở Q05 hiện đã chốt; định dạng xuất khác chỉ xét nếu có trong danh mục.
- Metadata người soạn/reviewer/approver chưa đầy đủ; chưa có TDD và Unit Test cho tính năng. Đây là tình trạng hồ sơ, không phải lý do hỏi lại các quyết định nghiệp vụ đã xác nhận.

**Đề xuất tại thời điểm rà soát — đã được cập nhật ở đầu tài liệu**

Bắt đầu bằng Q01: lập danh mục quyền thật sự cần ở đợt đầu. Sau đó gắn từng thao tác với quyền và cách tính lượt. Các nhánh Q08–Q13 chỉ cần quyết định phạm vi trước; không phải hệ thống subscription nào cũng cần xây đủ ngay từ đầu. Giữ nguyên các phần đã hoãn và chỉ cập nhật Story, BR, System Test khi có quyết định mới.

### Tham chiếu bổ sung từ các bản ghi đã hợp nhất

- [ST-SUB-099](../systemtest/ST-SUB-099.md)
