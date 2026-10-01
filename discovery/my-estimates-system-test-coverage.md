# System Test cho danh sách và xóa dự toán của tôi

> Cập nhật 01/10/2026: điều kiện xóa dự toán có bổ sung trường hợp đang là nguồn của công trình. Xem [độ phủ hồ sơ công trình](construction-site-system-test-coverage.md) cho ST-PROJ-110–114 và điều chỉnh dữ liệu nền của ST-PROJ-093–109. Các hash và nhận định tại mốc soạn ban đầu bên dưới được giữ làm lịch sử.


Người dùng xác nhận **“chốt”** trong hội thoại ngày 30/09/2026 cho STORY-PROJ-006, STORY-PROJ-007, BR-PROJ-008, BR-PROJ-009 và phần bổ sung về bản đã xóa trong BR-PROJ-006, BR-PROJ-007, BR-SUB-007. Đây là xác nhận nghiệp vụ trong hội thoại, không có approvalId hoặc bằng chứng import/publish/phê duyệt trên Document First.

Đã soạn **32 đặc tả ST-PROJ-078–109**: 15 ca cho danh sách và 17 ca cho xóa nhiều. Mỗi file có một ca, đúng 13 cột, Trace to khớp TEST_LINKS. Các ca có liên kết tới **22 AC, 2 Main Flow, 6 Alternative Flow và 7 Exception Flow** của hai Story. Hai nhóm Non-Functional được đối chiếu ở phần dưới. Đây là độ phủ đặc tả và liên kết, không phải độ phủ mã hoặc kết quả Pass.

Nguồn chính là bộ tài liệu trong workspace BMT theo AGENTS.md, không lấy một project Document First khác làm căn cứ. Chưa viết mã test, chưa chạy các ca, chưa sửa ứng dụng hoặc triển khai. Hợp đồng API mới và implementation chưa có nên các ca **chưa sẵn sàng thực thi**; không dùng dữ liệu mock làm bằng chứng backend hiện đã đáp ứng tính năng.

## Phạm vi và điều kiện thực thi

- Điểm vào là giao diện Dự toán của tôi và yêu cầu HTTP thực qua backend. Đọc database thật để đối chiếu trạng thái truy cập, dữ liệu còn lại, tác vụ và sổ lượt; không gọi riêng handler thay cho một System Test.
- Fixture độc lập cho từng ca và từng biến thể, gồm khách C1/C2, nhân viên, Admin, trạng thái gói/lượt và dự toán cần dùng. Các Id D1/D2 trong đặc tả là ký hiệu dữ liệu thử; sinh Id mới ở mỗi lần chạy. Không phụ thuộc thứ tự thực thi các ca.
- AI được giả lập để giữ tác vụ đang chạy hoặc cung cấp bộ kết quả thử hợp lệ theo hợp đồng khi có. Tệp và SMTP dùng dịch vụ thử/hộp thư thử; không gọi AI trả phí hoặc gửi thư tới người thật. Đếm lời gọi để kiểm thao tác đọc/xóa không tạo thêm tác vụ.
- Ca lỗi dùng proxy/harness điều khiển mất kết nối hoặc phản hồi lỗi; ca thứ tự AI/xóa chờ bằng chứng commit, không dùng sleep để đoán. Nếu thiếu điểm điều khiển thì ghi Blocked cho ca đó, không coi bước chưa làm là Pass.
- Kích thước trang P lấy từ hợp đồng/cấu hình được thiết kế. Fixture tên dùng chuỗi trùng hoàn toàn để không tự quyết định tìm gần đúng, hoa/thường hoặc dấu. Mốc sửa khác nhau để không tự quyết định thứ tự phụ.
- Sau từng ca, dừng stub, gỡ lỗi mô phỏng và dọn dữ liệu bằng công cụ quản lý môi trường thử. Không dùng tính năng khôi phục dự toán vì nghiệp vụ không có chức năng này.
- Reviewer/Approver là Tân Trần theo bộ tài liệu tính năng; Owner người thực thi chưa được chỉ định. Giữ trạng thái Markdown Draft; không đổi thành Approved để mô phỏng việc phê duyệt hoặc kết quả chạy.

## Mốc nguồn

Hash SHA-256 tính trên toàn bộ byte UTF-8 của file, kể cả comment mẫu. Cột trước ghi nhận xác nhận là nội dung người dùng vừa chốt hoặc nguồn quy tắc được dùng lại. Cột hiện tại là nguồn của các ST sau khi cập nhật câu mô tả trạng thái chốt trong Context/Notes; không sửa AC, Flow, Then, Except hoặc quyết định nghiệp vụ ở bước này.

Mỗi liên kết DOC-KEY/section trong ST dùng cặp **DOC-KEY#contentHash** tương ứng ở cột hiện tại dưới đây. Không thêm hash vào TEST_LINKS vì importer dùng mã tài liệu và section riêng.

| Nguồn | SHA-256 trước ghi nhận xác nhận | documentKey#contentHash hiện tại |
|---|---|---|
| [STORY-PROJ-006](../userstory/STORY-PROJ-006.md) | `d41a430ea3704169db48fbacea6f4de99a9a615761b8eaccf5f42306fcabe5f1` | `STORY-PROJ-006#25528fcb4486569030446c3c0638e94aabdea82735257adbe10827ceddc0fcf4` |
| [STORY-PROJ-007](../userstory/STORY-PROJ-007.md) | `0dc02d810c936544e13a51a059ca89dc4023b917bdc789532e3668360d30800c` | `STORY-PROJ-007#cb68fc28ef1e7916acdec428c78ee480d53f5402df809143d950507c32a6dad3` |
| [BR-PROJ-008](../businessrule/BR-PROJ-008.md) | `fa08697ef5822ffd80dff6dceeb6ccf59a1e9da27d04d2f08cb486a83d1e2d94` | `BR-PROJ-008#d49cfc74575b7c6b4a1b6c8d11eacb22e79bdc59049bea976298a30d095234a5` |
| [BR-PROJ-009](../businessrule/BR-PROJ-009.md) | `5a8e5fd9c6380285cbc935fb424bdaa9f82a6c292e18518e7ead1b003be6423e` | `BR-PROJ-009#9534aa28ae7b92a8257b5d28d7f28256ce965d371490265c0bc19d2359388b89` |
| [BR-PROJ-006](../businessrule/BR-PROJ-006.md) | `f4c4091932e61de9be35f006ad433ecd575c257c325e46e58ed6c7d35b4aa22d` | `BR-PROJ-006#801781983b7919f82c6d1f89c9cf06219299e7ca962afa89a837dee228984b03` |
| [BR-PROJ-007](../businessrule/BR-PROJ-007.md) | `1775260d346143694b2fcf5d286e3deafca679406abe15a129e3b7740fcc9171` | `BR-PROJ-007#9c95725288ea602430b2135f9496410e6f7f5243fc3909b1598d3bebfc3d71d4` |
| [BR-SUB-007](../businessrule/BR-SUB-007.md) | `aeb979aee6c6a1fd561d92bcd2a655f1b4216e39fab15ed941b1fbc97bba451b` | `BR-SUB-007#425376c29cbd8cff086a3ea11c3f82837064be4bef9281a5720a8aef226102bf` |
| [BR-PROJ-005](../businessrule/BR-PROJ-005.md) | `21eaa06d91d8a46300fcf80f549d4531a32e4c74fa139436bb652dbeef52c774` | `BR-PROJ-005#21eaa06d91d8a46300fcf80f549d4531a32e4c74fa139436bb652dbeef52c774` |
| [BR-SUB-003](../businessrule/BR-SUB-003.md) | `fb6c087cf36159b0d8faeff2b3e900ba279ed590d815855dcb71a5691fa32b99` | `BR-SUB-003#fb6c087cf36159b0d8faeff2b3e900ba279ed590d815855dcb71a5691fa32b99` |
| [BR-RBAC-005](../businessrule/BR-RBAC-005.md) | `8dfa2f529a208b0754e9a8c568834a80ab612693fdd4e51e009dbba308705872` | `BR-RBAC-005#8dfa2f529a208b0754e9a8c568834a80ab612693fdd4e51e009dbba308705872` |

## Danh mục ca kiểm thử

Tất cả các ca có thể tự động hóa sau khi có API/giao diện và harness phù hợp. Có thể kiểm thủ công phần giao diện, nhưng vẫn phải lưu bằng chứng request/response và dữ liệu sau thao tác. Chưa có ca nào được thực thi hoặc được xác nhận đã tự động hóa.

| Ca | Story | Mục tiêu | Loại | Ưu tiên |
|---|---|---|---|---|
| [ST-PROJ-078](../systemtest/ST-PROJ-078.md) | STORY-PROJ-006 | Đi hết danh sách và mở dự toán ở các trạng thái | Main / Integration boundary | P0 |
| [ST-PROJ-079](../systemtest/ST-PROJ-079.md) | STORY-PROJ-006 | Thông tin mỗi dòng và bản nháp chưa chọn loại công trình | Main / ALT | P1 |
| [ST-PROJ-080](../systemtest/ST-PROJ-080.md) | STORY-PROJ-006 | Sắp xếp toàn tập trước khi phân trang | Main / Integration boundary | P1 |
| [ST-PROJ-081](../systemtest/ST-PROJ-081.md) | STORY-PROJ-006 | Tìm đúng tên và loại bản đã xóa hoặc của người khác | Main / NFR | P0 |
| [ST-PROJ-082](../systemtest/ST-PROJ-082.md) | STORY-PROJ-006 | Lọc từng trạng thái và thống nhất với màn hình chi tiết | Main | P1 |
| [ST-PROJ-083](../systemtest/ST-PROJ-083.md) | STORY-PROJ-006 | Kết hợp tìm tên và trạng thái trước khi phân trang | Main / NFR | P0 |
| [ST-PROJ-084](../systemtest/ST-PROJ-084.md) | STORY-PROJ-006 | Tài khoản chưa có dự toán nhận danh sách rỗng | ALT | P1 |
| [ST-PROJ-085](../systemtest/ST-PROJ-085.md) | STORY-PROJ-006 | Không có kết quả phù hợp bộ lọc | ALT | P1 |
| [ST-PROJ-086](../systemtest/ST-PROJ-086.md) | STORY-PROJ-006 | Hết hạn gói hoặc hết lượt vẫn xem danh sách và mở dữ liệu cũ | ALT / Integration boundary | P0 |
| [ST-PROJ-087](../systemtest/ST-PROJ-087.md) | STORY-PROJ-006 | Mở bản đang xử lý hoặc thất bại không tự gửi lại AI | Main / Integration boundary | P0 |
| [ST-PROJ-088](../systemtest/ST-PROJ-088.md) | STORY-PROJ-006 | Từ chối danh sách với phiên thiếu hoặc hết hiệu lực | EXC / NFR | P0 |
| [ST-PROJ-089](../systemtest/ST-PROJ-089.md) | STORY-PROJ-006 | Tài khoản nhân viên kể cả Admin không dùng danh sách khách hàng | EXC / NFR | P0 |
| [ST-PROJ-090](../systemtest/ST-PROJ-090.md) | STORY-PROJ-006 | Cách ly tài khoản khi tìm kiếm và mở Id của khách khác | EXC / NFR | P0 |
| [ST-PROJ-091](../systemtest/ST-PROJ-091.md) | STORY-PROJ-006 | Danh sách cũ không mở lại bản vừa bị xóa ở phiên khác | EXC / Integration boundary | P0 |
| [ST-PROJ-092](../systemtest/ST-PROJ-092.md) | STORY-PROJ-006 | Lỗi tải danh sách không bị hiển thị như dữ liệu rỗng | EXC | P1 |
| [ST-PROJ-093](../systemtest/ST-PROJ-093.md) | STORY-PROJ-007 | Xóa nhiều bản đủ điều kiện từ danh sách | Main / Integration boundary | P0 |
| [ST-PROJ-094](../systemtest/ST-PROJ-094.md) | STORY-PROJ-007 | Chọn tất cả giới hạn ở trang đang xem và tôn trọng bỏ chọn | Main / NFR | P0 |
| [ST-PROJ-095](../systemtest/ST-PROJ-095.md) | STORY-PROJ-007 | Xóa 8 bản đủ điều kiện và giữ 2 bản đang xử lý | ALT / Integration boundary | P0 |
| [ST-PROJ-096](../systemtest/ST-PROJ-096.md) | STORY-PROJ-007 | Không bản nào đủ điều kiện khi toàn bộ đang xử lý AI | ALT / Integration boundary | P0 |
| [ST-PROJ-097](../systemtest/ST-PROJ-097.md) | STORY-PROJ-007 | Gói hết hạn vẫn xóa được bản đủ điều kiện | ALT / Integration boundary | P0 |
| [ST-PROJ-098](../systemtest/ST-PROJ-098.md) | STORY-PROJ-007 | Hết lượt không ngăn xóa, nhưng AI đang chạy vẫn bị chặn | ALT / Integration boundary | P0 |
| [ST-PROJ-099](../systemtest/ST-PROJ-099.md) | STORY-PROJ-007 | Chọn một bản dùng cùng quy tắc xóa | ALT | P1 |
| [ST-PROJ-100](../systemtest/ST-PROJ-100.md) | STORY-PROJ-007 | Xóa kết quả đã thành công không hoàn lượt hoặc sửa lịch sử tính lượt | Integration boundary | P0 |
| [ST-PROJ-101](../systemtest/ST-PROJ-101.md) | STORY-PROJ-007 | Không mở lại hoặc thao tác trên bản đã xóa qua các đường cũ của chủ sở hữu | Main / NFR / Integration boundary | P0 |
| [ST-PROJ-102](../systemtest/ST-PROJ-102.md) | STORY-PROJ-007 | Xóa ngừng xem và tải mới qua link, QR, email | Main / NFR / Integration boundary | P0 |
| [ST-PROJ-103](../systemtest/ST-PROJ-103.md) | STORY-PROJ-007 | Yêu cầu xóa trộn bản của người khác, mã không tồn tại và bản đã xóa | EXC / NFR | P0 |
| [ST-PROJ-104](../systemtest/ST-PROJ-104.md) | STORY-PROJ-007 | Phiên thiếu hoặc hết hiệu lực không được xóa | EXC / NFR | P0 |
| [ST-PROJ-105](../systemtest/ST-PROJ-105.md) | STORY-PROJ-007 | Nhân viên kể cả Admin không được xóa dự toán khách hàng | EXC / NFR | P0 |
| [ST-PROJ-106](../systemtest/ST-PROJ-106.md) | STORY-PROJ-007 | AI được tiếp nhận sau lúc chọn nhưng trước khi xử lý xóa | EXC / Integration boundary | P0 |
| [ST-PROJ-107](../systemtest/ST-PROJ-107.md) | STORY-PROJ-007 | Xóa thành công trước thì yêu cầu AI từ phiên cũ không được tiếp nhận | Integration boundary / NFR | P0 |
| [ST-PROJ-108](../systemtest/ST-PROJ-108.md) | STORY-PROJ-007 | Mất phản hồi hoặc lỗi máy chủ không làm báo sai kết quả xóa nhiều | EXC / Integration boundary | P0 |
| [ST-PROJ-109](../systemtest/ST-PROJ-109.md) | STORY-PROJ-007 | Gửi lại yêu cầu xóa không làm bản xuất hiện lại hoặc tác động bản ngoài tập | EXC / Integration boundary | P0 |

## AC và các luồng

| Nguồn | Ca có liên kết và kỳ vọng tương ứng |
|---|---|
| STORY-PROJ-006/Main Flow | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-079](../systemtest/ST-PROJ-079.md), [ST-PROJ-080](../systemtest/ST-PROJ-080.md), [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-087](../systemtest/ST-PROJ-087.md) |
| STORY-PROJ-006/ALT-01 | [ST-PROJ-084](../systemtest/ST-PROJ-084.md), [ST-PROJ-085](../systemtest/ST-PROJ-085.md) |
| STORY-PROJ-006/ALT-02 | [ST-PROJ-086](../systemtest/ST-PROJ-086.md) |
| STORY-PROJ-006/ALT-03 | [ST-PROJ-079](../systemtest/ST-PROJ-079.md) |
| STORY-PROJ-006/EXC-01 | [ST-PROJ-088](../systemtest/ST-PROJ-088.md), [ST-PROJ-089](../systemtest/ST-PROJ-089.md) |
| STORY-PROJ-006/EXC-02 | [ST-PROJ-090](../systemtest/ST-PROJ-090.md), [ST-PROJ-091](../systemtest/ST-PROJ-091.md) |
| STORY-PROJ-006/EXC-03 | [ST-PROJ-092](../systemtest/ST-PROJ-092.md) |
| STORY-PROJ-006/AC-001 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-090](../systemtest/ST-PROJ-090.md) |
| STORY-PROJ-006/AC-002 | [ST-PROJ-079](../systemtest/ST-PROJ-079.md) |
| STORY-PROJ-006/AC-003 | [ST-PROJ-080](../systemtest/ST-PROJ-080.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md) |
| STORY-PROJ-006/AC-004 | [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-085](../systemtest/ST-PROJ-085.md) |
| STORY-PROJ-006/AC-005 | [ST-PROJ-084](../systemtest/ST-PROJ-084.md), [ST-PROJ-085](../systemtest/ST-PROJ-085.md) |
| STORY-PROJ-006/AC-006 | [ST-PROJ-086](../systemtest/ST-PROJ-086.md) |
| STORY-PROJ-006/AC-007 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-086](../systemtest/ST-PROJ-086.md), [ST-PROJ-087](../systemtest/ST-PROJ-087.md), [ST-PROJ-090](../systemtest/ST-PROJ-090.md) |
| STORY-PROJ-006/AC-008 | [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-091](../systemtest/ST-PROJ-091.md) |
| STORY-PROJ-006/AC-009 | [ST-PROJ-088](../systemtest/ST-PROJ-088.md), [ST-PROJ-089](../systemtest/ST-PROJ-089.md) |
| STORY-PROJ-006/AC-010 | [ST-PROJ-092](../systemtest/ST-PROJ-092.md) |
| STORY-PROJ-006/Non-Functional | [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-088](../systemtest/ST-PROJ-088.md), [ST-PROJ-089](../systemtest/ST-PROJ-089.md), [ST-PROJ-090](../systemtest/ST-PROJ-090.md), [ST-PROJ-091](../systemtest/ST-PROJ-091.md) |
| STORY-PROJ-007/Main Flow | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md) |
| STORY-PROJ-007/ALT-01 | [ST-PROJ-095](../systemtest/ST-PROJ-095.md) |
| STORY-PROJ-007/ALT-02 | [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| STORY-PROJ-007/ALT-03 | [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md) |
| STORY-PROJ-007/EXC-01 | [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md) |
| STORY-PROJ-007/EXC-02 | [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| STORY-PROJ-007/EXC-03 | [ST-PROJ-106](../systemtest/ST-PROJ-106.md) |
| STORY-PROJ-007/EXC-04 | [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |
| STORY-PROJ-007/AC-001 | [ST-PROJ-094](../systemtest/ST-PROJ-094.md) |
| STORY-PROJ-007/AC-002 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md) |
| STORY-PROJ-007/AC-003 | [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| STORY-PROJ-007/AC-004 | [ST-PROJ-096](../systemtest/ST-PROJ-096.md) |
| STORY-PROJ-007/AC-005 | [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| STORY-PROJ-007/AC-006 | [ST-PROJ-100](../systemtest/ST-PROJ-100.md) |
| STORY-PROJ-007/AC-007 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| STORY-PROJ-007/AC-008 | [ST-PROJ-102](../systemtest/ST-PROJ-102.md) |
| STORY-PROJ-007/AC-009 | [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| STORY-PROJ-007/AC-010 | [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md) |
| STORY-PROJ-007/AC-011 | [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md) |
| STORY-PROJ-007/AC-012 | [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |
| STORY-PROJ-007/Non-Functional | [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-100](../systemtest/ST-PROJ-100.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-108](../systemtest/ST-PROJ-108.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |

## Điều kiện trong Business Rule

Số khoản dưới đây là khoản trong Then, không phải một section mới của tài liệu. TEST_LINKS trỏ tới Then; Rationale mỗi ca nêu những khoản được kiểm tra.

| Quy tắc/khoản | Ca kiểm tra |
|---|---|
| BR-PROJ-008/Then khoản 1 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-080](../systemtest/ST-PROJ-080.md), [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-084](../systemtest/ST-PROJ-084.md), [ST-PROJ-088](../systemtest/ST-PROJ-088.md), [ST-PROJ-089](../systemtest/ST-PROJ-089.md), [ST-PROJ-090](../systemtest/ST-PROJ-090.md) |
| BR-PROJ-008/Then khoản 2 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-079](../systemtest/ST-PROJ-079.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md) |
| BR-PROJ-008/Then khoản 3 | [ST-PROJ-079](../systemtest/ST-PROJ-079.md) |
| BR-PROJ-008/Then khoản 4 | [ST-PROJ-080](../systemtest/ST-PROJ-080.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md) |
| BR-PROJ-008/Then khoản 5 | [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-085](../systemtest/ST-PROJ-085.md) |
| BR-PROJ-008/Then khoản 6 | [ST-PROJ-084](../systemtest/ST-PROJ-084.md), [ST-PROJ-085](../systemtest/ST-PROJ-085.md), [ST-PROJ-092](../systemtest/ST-PROJ-092.md) |
| BR-PROJ-008/Then khoản 7 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-084](../systemtest/ST-PROJ-084.md), [ST-PROJ-086](../systemtest/ST-PROJ-086.md) |
| BR-PROJ-008/Then khoản 8 | [ST-PROJ-078](../systemtest/ST-PROJ-078.md), [ST-PROJ-082](../systemtest/ST-PROJ-082.md), [ST-PROJ-086](../systemtest/ST-PROJ-086.md), [ST-PROJ-087](../systemtest/ST-PROJ-087.md), [ST-PROJ-090](../systemtest/ST-PROJ-090.md) |
| BR-PROJ-008/Then khoản 9 | [ST-PROJ-081](../systemtest/ST-PROJ-081.md), [ST-PROJ-083](../systemtest/ST-PROJ-083.md), [ST-PROJ-091](../systemtest/ST-PROJ-091.md), [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md) |
| BR-PROJ-009/Then khoản 1 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md) |
| BR-PROJ-009/Then khoản 2 | [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md) |
| BR-PROJ-009/Then khoản 3 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md) |
| BR-PROJ-009/Then khoản 4 | [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md) |
| BR-PROJ-009/Then khoản 5 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-108](../systemtest/ST-PROJ-108.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| BR-PROJ-009/Then khoản 6 | [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| BR-PROJ-009/Then khoản 7 | [ST-PROJ-091](../systemtest/ST-PROJ-091.md), [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| BR-PROJ-009/Then khoản 8 | [ST-PROJ-102](../systemtest/ST-PROJ-102.md) |
| BR-PROJ-009/Then khoản 9 | [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| BR-PROJ-009/Then khoản 10 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-100](../systemtest/ST-PROJ-100.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| BR-PROJ-009/Then khoản 11 | [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-100](../systemtest/ST-PROJ-100.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |
| BR-PROJ-009/Then khoản 12 | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md) |
| BR-PROJ-009/Then khoản 13 | [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |

Các ngoại lệ trong BR-PROJ-008 được kiểm ở ST-PROJ-088–091. Chặn xóa AI trong BR-PROJ-009/Except được kiểm ở ST-PROJ-095, 096, 106; việc không thu hồi tệp đã tải được kiểm ở ST-PROJ-102. Thời điểm ngừng một luồng tải đã cấp quyền trước xóa chưa có quyết định kỹ thuật nên không tự đặt kỳ vọng.

| Phần bổ sung đã chốt | Ca bổ sung | Điều cần chứng minh |
|---|---|---|
| BR-PROJ-006/Except | [ST-PROJ-102](../systemtest/ST-PROJ-102.md) | Link còn hạn, QR và email không cấp quyền xem/tải mới sau xóa; hồ sơ khác vẫn truy cập được. |
| BR-PROJ-007/Except | [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md) | Chủ sở hữu và người có link đều không lấy lại hồ sơ qua đường cũ, không tạo quyền chia sẻ mới. |
| BR-SUB-007/Except | [ST-PROJ-086](../systemtest/ST-PROJ-086.md), [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md) | Hết hạn/hết lượt vẫn đọc bản còn lại và xóa bản đủ điều kiện; quyền cũ không mở lại bản đã xóa. |

## Ràng buộc ngoài chức năng

- Quyền sở hữu và loại tài khoản: ST-PROJ-081, 083, 088–090, 103–105 kiểm bằng request thực, tên/Id đối chứng và nội dung lỗi; không chỉ kiểm có nút bị ẩn.
- Không đưa bản đã xóa vào dữ liệu hoặc phân trang: ST-PROJ-081, 083, 091, 101 kiểm lại tập Id và thông tin phân trang.
- Kết quả từng bản và tính đúng dữ liệu đã ghi: ST-PROJ-093–096, 103, 108, 109 kiểm cả phản hồi lẫn dữ liệu sau yêu cầu; trường hợp mất phản hồi phải phản ánh sự chưa chắc chắn.
- Kiểm lại trạng thái AI: ST-PROJ-106, 107 kiểm hai thứ tự tiếp nhận có thể quan sát. Chưa tuyên bố phủ mọi tranh chấp transaction bên trong; thiết kế kỹ thuật cần bổ sung các điểm đồng bộ phù hợp.
- Mọi đường truy cập cũ của chủ sở hữu và người nhận link: ST-PROJ-101, 102. Các đường tải đang diễn ra, job nền đã nhận việc và chính sách dọn dữ liệu cần chốt thêm trước khi thêm kỳ vọng cụ thể.
- US chưa đặt ngưỡng hiệu năng, SLA hoặc chuẩn trình duyệt cụ thể; không tạo ca có ngưỡng tùy ý để báo đã phủ.

## Các ca và mã test hiện có có thể dùng làm đối chứng

- [ST-PROJ-018](../systemtest/ST-PROJ-018.md) và [ST-PROJ-036](../systemtest/ST-PROJ-036.md): cách ly chủ sở hữu khi đọc, sửa và tải. ST mới bổ sung danh sách và xóa, không coi các ca cũ là đã kiểm phần mới.
- [ST-PROJ-035](../systemtest/ST-PROJ-035.md) và [ST-PROJ-046](../systemtest/ST-PROJ-046.md): quyền hồ sơ còn tồn tại khi gói hết hạn; giữ làm đối chứng để tính năng xóa không chặn hồ sơ chưa xóa.
- [ST-PROJ-040](../systemtest/ST-PROJ-040.md): thu hồi link chặn người nhận nhưng chủ sở hữu vẫn xem được. ST-PROJ-101/102 kiểm khác biệt sau khi xóa bản: cả hai đường truy cập đều bị chặn.
- [EstimateQueryTests.cs](../../bmt-be/test/bmt-be.application.tests/usecases/estimate/EstimateQueryTests.cs) có ca đọc đầu vào, trạng thái và từ chối nhân viên/khách khác. Đây là test application hiện có, không phải System Test cho API mới.
- [EstimateSharingFlowTests.cs](../../bmt-be/test/bmt-be.integration.tests/EstimateSharingFlowTests.cs) có ca `Handle_PublicDownloadAfterRevoke_IsBlockedWithoutOpeningFile`, có thể tham khảo fixture tệp và cách đối chiếu sau thu hồi cho phần chặn sau xóa.
- [EstimateConcurrencyTests.cs](../../bmt-be/test/bmt-be.integration.tests/EstimateConcurrencyTests.cs) có các ca đồng thời khi tạo/lưu/đổi tên. Có thể tham khảo hạ tầng đồng bộ; chưa có bằng chứng kiểm tranh chấp xóa/AI.

## Phần chưa thể kết luận hoặc thực thi

1. Chưa có TDD/API mới: chưa chốt route, phương thức, HTTP status, mã lỗi từng bản, schema request/response, giới hạn batch và xử lý request trùng. Các đặc tả chỉ chốt kỳ vọng nghiệp vụ; phải bổ sung hợp đồng trước chạy, không đánh dấu Pass nếu chưa kiểm các trường này.
2. Chưa có giao diện/API xóa và danh sách trong code đã khảo sát. Ca UI cần giao diện; ca API/database cần implementation và môi trường thử phù hợp. Không có báo cáo thực thi cho 32 ca.
3. Thuật toán tìm tên (chuỗi con, hoa/thường, dấu), thứ tự khi thời điểm sửa bằng nhau, kích thước trang và lỗi tham số chưa được chốt. Fixture hiện tại tránh tự quyết định các trường hợp này; chúng chưa được phủ ở mức hợp đồng chi tiết.
4. Không thiết kế test đòi xóa vật lý, dọn tệp bên AI, khôi phục/thùng rác, hủy tác vụ hoặc hoàn lượt; các việc đó ngoài phạm vi hoặc chưa có chính sách. Khi thiết kế cách lưu dữ liệu cần bổ sung kiểm chứng DB phù hợp mà giữ nguyên kỳ vọng không truy cập lại.
5. Không áp đặt cấu trúc hộp xác nhận, hành vi giữ lựa chọn qua nhiều trang hoặc ngắt luồng tải đang diễn ra; bộ đã chốt chỉ yêu cầu thông báo hậu quả và Chọn tất cả trong trang đang xem.
6. Owner/người thực thi chưa có; các trường phân công và ngày hiệu lực còn thiếu trong nguồn được giữ nguyên. Không coi xác nhận nghiệp vụ là xác nhận metadata đã đầy đủ.
7. Chưa hoàn tất audit toàn bộ đồ thị tham chiếu ngoài bộ nguồn trực tiếp của hai Story. Những giới hạn đã nêu trong [bản nghiên cứu](my-estimates-planning.md) vẫn còn; không dùng bảng này làm bằng chứng đã đọc toàn bộ TDD-PROJ hoặc toàn bộ hệ thống.

## Kiểm tra tài liệu

Kiểm tra cục bộ: mã ST không trùng, mỗi file một dòng dữ liệu đúng 13 cột, comment mẫu được giữ nguyên, Trace to khớp TEST_LINKS, mọi mã/section đích trực tiếp tồn tại, mỗi AC/nhánh có ít nhất một liên kết, từng khoản Then của hai BR mới có ca tương ứng và hash nguồn khớp file tại mốc soạn ban đầu. Các kiểm tra này xác minh cấu trúc/truy vết, không thay thế review chất lượng hành vi hoặc chạy ứng dụng.


## Bổ sung ở bước TDD

Các hash nguồn bên trên được giữ làm mốc lúc soạn ST. Sau yêu cầu “lên TDD”, hai Story đã được thêm References/TDDs, không đổi AC/Flow. Hash mới và bản nháp kỹ thuật ở [bảng thiết kế](my-estimates-technical-design.md). TDD chưa được chốt; các ST vẫn chưa thực thi và chưa được cập nhật thành hợp đồng HTTP cuối cùng.

Ngày 30/09/2026, người dùng xác nhận giữ dữ liệu nội bộ sau xóa và tìm tên không phân biệt hoa/thường/dấu. Đã cập nhật nguồn US/BR và ST-PROJ-081 để kiểm “nha”, “NHÀ” và dấu tổ hợp đều tìm được Nhà An, vẫn loại bản đã xóa hoặc của khách khác. Giữ nguyên mã test, Trace to và TEST_LINKS. Các hash cũ bên trên là mốc lịch sử; hash hiện tại ở [bảng thiết kế](my-estimates-technical-design.md). Tổng vẫn là 32 ST, chưa chạy; chưa tạo đặc tả Unit Test.

Người dùng đã chốt hai TDD trong hội thoại ngày 30/09/2026. Đặc tả UT và phần phải kiểm bằng PostgreSQL/HTTP nằm ở [bảng độ phủ UT](my-estimates-unit-test-coverage.md); ST vẫn chưa chạy. Các ghi chú “chưa có TDD” trong mốc soạn ST ban đầu là điều kiện lịch sử, hợp đồng hiện tại lấy từ TDD-PROJ-004/005 đã chốt.
