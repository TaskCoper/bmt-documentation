# Thiết kế kỹ thuật Tạo dự toán

Đã soạn ba bản nháp TDD cho toàn bộ luồng Tạo dự toán, từ nhập liệu đến hồ sơ thi công. Nghiệp vụ US/BR đã được người dùng chốt; người dùng đã xác nhận “Ok chốt đi” cho cả ba TDD trong hội thoại sau khi làm rõ việc lưu và xem lại bước 2–3. Reviewer và Approver: Tân Trần. Chưa triển khai mã ứng dụng, chạy migration, gọi AI thật hoặc gửi email.

| Tài liệu | Phạm vi | Story |
|---|---|---|
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md) | Danh mục có phiên bản, một diện tích chung, ảnh/mô tả, tạo bằng tên, tự lưu, quyền sở hữu và điều kiện gói/lượt | STORY-PROJ-001, STORY-PROJ-005 |
| [TDD-PROJ-002](../tdd/TDD-PROJ-002.md) | Đóng băng đầu vào, giữ lượt, gửi và theo dõi AI, nhận kết quả, lỗi/quá hạn, thử lại và quyết toán kỳ gốc | STORY-PROJ-002; phần khóa đầu vào và nguồn kết quả của Story 001/003 |
| [TDD-PROJ-003](../tdd/TDD-PROJ-003.md) | Đọc hồ sơ, PDF/Excel, một link đang hiệu lực, QR, hết hạn/thu hồi và email một người nhận | STORY-PROJ-003, STORY-PROJ-004 |

Mỗi TDD có sơ đồ kiến trúc, sequence, activity, state, quan hệ dữ liệu; bảng/cột/ràng buộc, ý nghĩa từng bảng và mẫu lưu trữ; API nội bộ, lỗi và phần chờ tích hợp. Mã PROJ giữ nguyên để bảo toàn liên kết. Tên thực thể và API đề xuất là `Estimate` và `/estimates`, dành từ “dự án” cho tính năng khác.

## Cập nhật ngày 25/09/2026

Ba TDD được sửa theo US/BR người dùng chốt ngày 25/09/2026; người dùng xác nhận bản sửa trước khi cập nhật test. Thay đổi chính:

- **Đổi tên bản dự toán bất cứ lúc nào** ([BR-SUB-007](../businessrule/BR-SUB-007.md) khoản 11): kể cả khi gói hết hạn, hết lượt, toàn bộ lượt còn lại đang bị giữ hoặc AI đang xử lý; chỉ cần quyền sở hữu và tên hợp lệ. API riêng `PATCH /api/v1/estimates/{estimateId}/name` nhận `{name, nameVersion}`; lỗi 409 `EstimateNameVersionConflict` hoặc 422 `InvalidEstimateInput`. Tên không còn nằm trong snapshot gửi AI (TDD-PROJ-001, TDD-PROJ-002).
- **Hồ sơ và tệp dùng tên hiện tại** ([BR-PROJ-007](../businessrule/BR-PROJ-007.md) khoản 7): tệp PDF/Excel xuất theo tên cũ không được phục vụ nữa; lần tải sau xuất lại từ kết quả đã lưu, không tính lượt và không gọi AI. `EstimateExport.NameVersion` ghi tên dùng khi xuất; tệp cũ trả 409 `ExportOutdated` (TDD-PROJ-003).
- **Dùng lại link còn hiệu lực** trả 200 kèm `requestedExpiryApplied`: `true` khi ngày khách chọn trùng ngày đang có, `false` khi khác; không đổi hạn ngầm (TDD-PROJ-003).
- **Quyền quản trị danh mục** theo STORY-RBAC-001 có mã `estimate.catalog.manage` trong TDD-RBAC-001; kiểm theo mã quyền, không theo tên vai trò.
- Đối tượng gắn gói giám sát, trước đây gọi là “dự án”, nay là **công trình** (`ConstructionSite`): thực thể riêng do khách tự tạo. Bản dự toán và công trình không liên kết trong đợt này ([BR-SUB-007](../businessrule/BR-SUB-007.md)/Notes).

Test bổ sung: ST-PROJ-061 đến ST-PROJ-070 và UT-PROJ-049 đến UT-PROJ-061. Các mục bên dưới giữ nội dung bàn giao ban đầu, trừ số liệu và hiện trạng code đã cập nhật.

## Đã xác nhận

- Giữ toàn bộ phạm vi trong năm US và bảy BR-PROJ: cả năm loại ban đầu và loại Admin thêm, tách phong cách kiến trúc/nội thất, cấu hình tầng/tum riêng, giữ danh mục tại lúc tạo bản dự toán.
- Tạo/tự lưu phải kiểm quyền lợi và lượt sẵn dùng, nhưng không giữ hoặc trừ lượt. Gửi AI mới giữ một lượt; chỉ khi đủ kết quả được lưu và mở được mới tính đã dùng. Lỗi/quá hạn giải phóng tại kỳ đã giữ.
- Chỉ có một link đang hiệu lực trên mỗi bản dự toán, dùng chung sao chép, QR và email. Dùng lại khi còn hiệu lực; chỉ tạo link khác sau hết hạn hoặc thu hồi.
- Link dùng được hết ngày đã chọn theo Việt Nam; hết hiệu lực từ 00:00 ngày kế tiếp. Cho chọn ngày hiện tại, không chọn ngày đã qua.
- Tên loại công trình và tên phong cách bắt buộc có nội dung sau trim, tối đa 200 ký tự; cho phép trùng tên.
- Ba quyết định cuối đã bổ sung vào BR-PROJ-004/006, STORY-PROJ-004/005 và ST-PROJ-058–060. Giữ nguyên mốc hash cũ của bộ 57 ST; ghi mốc mới riêng trong [bảng truy vết](estimate-system-test-coverage.md).

## Phương án kỹ thuật đã chốt trong hội thoại

| Phương án | Lý do và giới hạn |
|---|---|
| Mỗi lần Admin lưu tạo một phiên bản danh mục bất biến; bản dự toán giữ mã phiên bản | Bản cũ tiếp tục dùng đúng tên, ảnh và lựa chọn cũ. Sao chép cấu hình mỗi phiên bản tốn thêm dữ liệu; chưa có số liệu để cần cơ chế lưu phần chênh lệch. |
| Tự lưu dùng phiên bản đầu vào và mã chống gửi lặp | Mất phản hồi không tạo bản/ghi lặp; tab cũ không ghi đè tab mới. Xung đột trả 409 để khách đối chiếu dữ liệu, không tự chọn bên thắng. |
| Dùng chung UsageOperation và sổ quota của Subscription | Tránh hai nguồn trạng thái AI/quota. Đầu vào, giữ lượt và tác vụ cùng commit; công bố kết quả và tính lượt cũng cùng commit. |
| Khóa AccountCommerceState trước dữ liệu tài khoản | Thống nhất thứ tự với thanh toán/hủy/khôi phục gói. Cần điều chỉnh toàn bộ đường quota trong TDD-SUB-002 còn mô tả khóa User. |
| Worker nhận việc từ PostgreSQL, có mã nhận việc và hạn giữ việc | Không thêm hàng đợi bên ngoài khi chưa cần. Hết thời gian giữ việc không có nghĩa được gửi lại AI/email nếu chưa biết lần trước đã được tiếp nhận hay chưa. |
| Kho tệp riêng tư; backend kiểm quyền trước mỗi lần xem/tải | Thu hồi/hết hạn chặn yêu cầu mới, kể cả tải một phần tệp. Không cung cấp đường tải công khai độc lập với link. Tệp đã tải và luồng đã được cấp quyền trước thu hồi không thể thu lại. |
| Một con trỏ share hiện hành, khóa bản dự toán khi tạo/thu hồi | Hai yêu cầu đồng thời không cấp hai link còn hiệu lực. Link cũ giữ lịch sử; tạo link mới không phục hồi QR/email cũ. |
| Token ngẫu nhiên; lưu hash kiểm quyền và bản mã để chủ sở hữu lấy lại | Dùng cùng link cho sao chép/QR/email. Cần kho khóa Data Protection bền vững, dùng chung giữa các instance. |
| Xuất tệp và gửi email có trạng thái riêng | Lỗi xuất/gửi không gọi lại AI hoặc tính thêm lượt. SMTP xác nhận tiếp nhận chỉ là Accepted, không phải đã phát thư; mất bằng chứng sau gửi là Unknown, không tự gửi lại. |

Quy ước API, byte/MB, cách đếm ký tự, kiểu số diện tích, khóa và mã lỗi thuộc phương án TDD đã được chốt; các mục ghi rõ chờ tích hợp vẫn chưa có giá trị để triển khai. Các ví dụ mã, thời gian và dữ liệu là fixture minh họa, không phải cấu hình sản phẩm đã được phê duyệt.

## Hiện trạng đã kiểm tra và phần bị ảnh hưởng

Lúc khảo sát ngày 21/09/2026, backend có tài khoản/xác thực và nền Carter, MediatR, EF Core; DbContext chỉ khai báo User. Kiểm lại ngày 25/09/2026: code đã có thêm RBAC, danh mục gói, kỳ thiết kế, `PeriodQuota` và `UsageOperation`. Vẫn chưa có các bảng Estimate, danh mục loại công trình/phong cách, lưu tệp hoặc điều phối AI theo thiết kế này. Các TDD SUB/PAY/RBAC được viện dẫn là nguồn thiết kế, không phải bằng chứng đã triển khai.

| Vị trí | Ảnh hưởng khi triển khai |
|---|---|
| Domain và Application | Thêm thực thể/cấu hình danh mục, Estimate, snapshot/kết quả, export/share/email; validator, policy và handler theo ba TDD. |
| Persistence | Thêm mapping và FK ghép bảo vệ quyền sở hữu; UoW dùng chung theo scope; khóa hàng và chỉ mục chống tác vụ sống trùng. Chỉ lập kế hoạch migration sau khi kiểm tra schema/dữ liệu đích. |
| TransactionPipelineBehavior | Hiện commit cả phản hồi Result.Failure bình thường. Lỗi cần rollback phải có đường xử lý rõ; Failed/TimedOut của AI là trạng thái cần commit khoản trả lượt. |
| Presentation và xác thực | Thêm Carter endpoint, kiểm owner/AccountKind, quyền estimate.catalog.manage, ánh xạ lỗi 409/413/503 và DTO theo TDD. Chống CSRF cho cookie dùng lớp chung ở [TDD-AUTH-001](../tdd/TDD-AUTH-001.md), không làm riêng. |
| MailService | Task hiện tại không phân biệt SMTP đã nhận với lỗi đóng kết nối. Adapter gửi hồ sơ cần giữ đúng bằng chứng tiếp nhận, dùng cùng cấu hình mà không tự thay hợp đồng gửi thư xác thực. |
| Subscription/PAY/RBAC | Dùng quyền và quota đã thiết kế; thống nhất khóa thương mại, bổ sung FK Estimate vào UsageOperation và permission quản trị. Không cấp quota từ gói hoàn thiện Cơ bản/Tiêu chuẩn/VIP. |

Đường dẫn file đã khảo sát được ghi trong References/Others của từng TDD. Chưa sửa các module trên.

## Cần làm rõ hoặc chờ tích hợp

| Phụ thuộc | Phần còn thiếu | Ảnh hưởng |
|---|---|---|
| AI service do nhóm khác phụ trách | API/xác thực, ánh xạ đầu vào và ảnh, submit/status/callback, đối chiếu yêu cầu, schema kết quả, bộ đầu ra bắt buộc và dữ liệu mẫu | Chưa thể hoàn tất adapter hoặc xác nhận kết quả đủ để chốt lượt. Không tự tạo hợp đồng giả. |
| PDF/Excel | AI trả tệp hay backend xuất từ nguồn; phần nào bắt buộc ngay khi AI thành công | Chưa chọn thư viện xuất hoặc tự đặt nội dung bản vẽ/dự toán. |
| Địa chỉ | Nguồn tỉnh/xã, phiên bản dữ liệu; xử lý địa giới ngừng dùng ở bản nháp cũ | Có thể thiết kế port và lưu địa chỉ đã xác minh; phần hành vi với địa giới cũ còn mở, không suy từ quy tắc danh mục phong cách. |
| Kho tệp và kho khóa | Nhà cung cấp, cấu hình truy cập, keyring dùng chung, backup/restore | Cần có trước bật upload/chia sẻ; không có URL công khai tạm để bỏ qua yêu cầu thu hồi. |
| Vận hành | Timeout AI, lease/retry, rate limit, lưu giữ dữ liệu/tệp mồ côi, ngưỡng cảnh báo, môi trường thử | Không tự đặt số thành yêu cầu nghiệp vụ; cấu hình bắt buộc phải được kiểm tra trước nhận việc. |
| Nền quyền/gói/lượt | Các module liên quan chưa có trong code, cần thống nhất thứ tự khóa giữa các TDD | Chưa thể triển khai dự toán độc lập rồi bỏ qua kiểm quota hoặc quyền lợi. |

## Truy vết và kiểm chứng

| Story / nhóm AC | Rule chính | TDD | Đặc tả System Test |
|---|---|---|---|
| STORY-PROJ-001 / AC-001–020 | BR-PROJ-001–005, BR-SUB-007, BR-RBAC-005 | TDD-PROJ-001; khóa AI ở TDD-PROJ-002 | ST-PROJ-001–020, 061–063, 065–068; các ca quyền/lượt/AI liên quan trong bảng truy vết |
| STORY-PROJ-002 / AC-001–007 | BR-PROJ-005/007, BR-SUB-003/016/017 | TDD-PROJ-002 | ST-PROJ-021–032, 057, 064 |
| STORY-PROJ-003 / AC-001–006 | BR-PROJ-007, BR-SUB-007 | TDD-PROJ-002/003 | ST-PROJ-033–038, 057, 069 |
| STORY-PROJ-004 / AC-001–010 | BR-PROJ-006, BR-SUB-007 | TDD-PROJ-003 | ST-PROJ-039–047, 057, 059–060, 070 |
| STORY-PROJ-005 / AC-001–010 | BR-PROJ-004 | TDD-PROJ-001 | ST-PROJ-048–056, 057–058 |

[Bảng truy vết ST](estimate-system-test-coverage.md) ghi từng AC/nhánh và ca cụ thể. Có 70 đặc tả ST, phủ liên kết của 53 AC, 5 Main Flow và 31 nhánh ALT/EXC; đây không phải kết quả chạy. Người dùng đã chốt TDD, đáp ứng điều kiện của [skill design-feature-technical](../../bmt-be/.codex/skills/design-feature-technical/SKILL.md). Đã bổ sung [61 đặc tả Unit Test](estimate-unit-test-coverage.md), chưa viết mã test hoặc thực thi.

Kiểm tra tĩnh đã thực hiện trên 77 file thuộc bộ TDD/US/BR/ST PROJ và hai bảng bàn giao: không phát hiện lỗi cấu trúc heading TDD, số cột ST, mã test, Trace to/TEST_LINKS hoặc đường dẫn Markdown trong phạm vi kiểm. Ba TDD có 15 khối Mermaid; chưa chạy trình render sơ đồ hoặc importer. Ví dụ API khớp endpoint khai báo; metadata lịch sử của TDD mới để trống. Đây là kiểm tra tài liệu, không phải kiểm thử ứng dụng.

Kiểm chứng sau triển khai cần PostgreSQL thật cho cạnh tranh lượt, khóa danh mục, phiên bản đầu vào, callback/timeout đồng thời và một link; kho tệp riêng tư để kiểm thu hồi; AI/SMTP sandbox để tạo lỗi có kiểm soát. Kiểm tra trình duyệt riêng cho giữ nội dung chưa lưu, QR, fragment và tải tệp. Mock hoặc kiểm cấu trúc Markdown không chứng minh các hành vi này hoạt động.

Phạm vi nguồn đã đọc sâu gồm bộ PROJ và các nguồn trực tiếp về quota, vòng đời gói, khóa thanh toán và RBAC. Việc quét tham chiếu mở rộng tìm được 307 tài liệu trước khi thêm ba TDD và ba ST mới, không thiếu mã trong tập quét đó; chưa hoàn tất đọc và đối chiếu nội dung toàn bộ chuỗi. Không coi kết quả tồn tại mã là đã kiểm toàn bộ section, thiết kế hoặc phụ thuộc ngoài PROJ. Các mâu thuẫn đã thấy về khóa User/AccountCommerceState và tên Project/Estimate được nêu rõ trong TDD-PROJ-002.

## Thứ tự triển khai sau khi chốt thiết kế

1. Hoàn tất hợp đồng tích hợp và rà soát phụ thuộc còn mở; thống nhất nền RBAC/subscription/khóa thương mại và transaction. CSRF đã thống nhất ngày 26/09/2026 ở [TDD-AUTH-001](../tdd/TDD-AUTH-001.md).
2. Đặc tả Unit Test theo TDD đã được chốt; cập nhật phần kiểm chứng tích hợp tương ứng. Đã soạn đặc tả Unit Test cho phần có hợp đồng rõ; các adapter còn mở giữ riêng trong bảng UT.
3. Triển khai danh mục, tạo/tự lưu và kho ảnh; kiểm chứng quyền lợi, phiên bản và dữ liệu cũ.
4. Triển khai tiếp nhận AI, snapshot, worker, chốt lượt và các trường hợp timeout/kết quả muộn.
5. Triển khai đọc hồ sơ, export, link/QR và email; kiểm quyền trên mọi đường tải.
6. Chạy test trên môi trường thử, phân biệt kết quả với giả lập và tích hợp thật; chỉ mở tính năng khi các phụ thuộc bắt buộc đã sẵn sàng.

## Ghi nhận chốt TDD

Xác nhận “Ok chốt đi” áp dụng cho TDD-PROJ-001–003 bản ngày 21/09/2026, gồm lưu bền vững kết quả AI và hồ sơ để xem lại, không gọi AI hoặc tính lượt khi xem lại. Bản sửa ngày 25/09/2026 được người dùng xác nhận trước khi cập nhật UT/ST. [Bảng UT](estimate-unit-test-coverage.md) lưu SHA-256 của ba file làm căn cứ, gồm cả [mốc ngày 25/09/2026](estimate-unit-test-coverage.md#cập-nhật-ngày-25092026). Không thay Status/Version/Updated At/Change Log để mô phỏng phê duyệt trên hệ thống quản lý tài liệu.
