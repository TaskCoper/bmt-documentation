# Thanh toán và quản lý gói đã mua — quyết định nghiệp vụ

Cập nhật ngày 19/09/2026. Nguồn: các lượt trao đổi và xác nhận trực tiếp của người dùng trong phiên thiết kế thanh toán. Đây là bản lưu nghiệp vụ đã thống nhất, kèm User Story, Business Rule và đặc tả System Test; chưa phải kết quả triển khai hoặc phê duyệt.

## Cập nhật ngày 25/09/2026

Các quyết định người dùng xác nhận ngày 25/09/2026 thay một số điểm của biên bản ngày 19/09 bên dưới. Nội dung cũ được giữ để tra cứu; khi khác nhau, dùng Business Rule hiện hành.

- **Gói giám sát gắn với công trình**, không gắn với bản dự toán. Công trình là thực thể riêng, khách tự tạo miễn phí và không cần gói thiết kế ([BR-SUB-022](../businessrule/BR-SUB-022.md)/Notes). Trong phần “Đã xác nhận” bên dưới, “dự án” của gói giám sát đọc là công trình, “sửa dự án” đọc là đổi công trình theo [BR-SUB-023](../businessrule/BR-SUB-023.md). Story, BR và TDD của Công trình chưa được soạn.
- **Tài khoản nhân viên không mua gói**: không tạo hoặc hủy đơn, kể cả khi có quyền tra cứu ([BR-RBAC-005](../businessrule/BR-RBAC-005.md) khoản 5, STORY-PAY-001/EXC-08 và AC-028, ST-PAY-070).
- **Quyền riêng của nhân viên** là mã quyền trong RBAC, không đòi phân công: `commerce.read` để tra cứu, `supervision.reassign` để đổi công trình, `package.cancel` và `package.restore` để hủy và khôi phục ([BR-RBAC-010](../businessrule/BR-RBAC-010.md) khoản 4). ST-PAY-069 kiểm việc thu hồi `commerce.read`.
- **Gói giám sát** chốt tên, giá và mô tả dịch vụ theo đơn; không dùng danh mục quyền lợi ([BR-SUB-004](../businessrule/BR-SUB-004.md) khoản 5, [BR-SUB-008](../businessrule/BR-SUB-008.md) khoản 7).
- Điểm 4 ở mục “Cần làm rõ” vẫn chưa được xác nhận: nhân viên gỡ gói về chưa gán hoặc đổi công trình của gói đang bị hủy.

## Đã xác nhận

### Mua gói và tạo đơn

- Khách tự mua cả gói thiết kế và giám sát. Một đơn thanh toán mua một gói.
- Dùng VNĐ, số tiền bằng giá niêm yết, không cộng phí tại bước thanh toán. Chưa triển khai giảm giá.
- Khi tạo đơn, lưu giá và quyền lợi khách đã chọn. Admin sửa giá/quyền lợi hoặc ngừng bán không thay điều kiện của đơn đã tạo. Đơn mới vẫn phải đáp ứng điều kiện đang bán.
- Hệ thống tạo QR, nhận thông báo giao dịch qua webhook SePay. Thời hạn chờ là 15 phút từ lúc tạo đơn.
- Mỗi khách chỉ có một đơn thiết kế đang chờ, kể cả đơn nhận một phần tiền. Giám sát được có nhiều đơn chờ và mua nhiều gói cùng loại.
- Giám sát được mua trước khi có dự án, không phải chờ kiểm tra địa điểm, diện tích hoặc phạm vi phục vụ. Thanh toán hợp lệ thì tự cấp gói, không chờ nhân viên tiếp nhận.

### Chuyển tiền và cấp gói

- Cộng dồn các khoản chuyển hợp lệ của cùng đơn. Khi chưa đủ, hiển thị “Đã nhận tiền nhưng chưa đủ, vui lòng chuyển thêm”, số đã nhận và số còn thiếu.
- QR cập nhật số tiền còn thiếu nhưng giữ nội dung chuyển khoản của đơn. Nhận tiền thiếu không gia hạn 15 phút ban đầu.
- Tổng tiền bằng hoặc lớn hơn giá đơn trong hạn thì đủ điều kiện cấp gói. Phần dư do nhân viên xử lý bên ngoài nếu khách khiếu nại.
- Xét thời điểm giao dịch SePay cung cấp, không xét thời điểm nhận webhook. Giao dịch phát sinh trong hạn vẫn hợp lệ dù webhook đến sau khi màn hình báo hết hạn.
- Khoản bổ sung phát sinh sau hạn không giúp đơn đủ điều kiện. Nhân viên xử lý tiền bên ngoài; hệ thống không thực hiện hoàn tiền, không ghi nhận hoặc đánh dấu đã hoàn tiền.
- Mỗi đơn chỉ cấp một gói. Webhook lặp không cộng tiền hai lần; chuyển đủ thêm lần nữa vào đơn đã hoàn tất không cấp thêm gói. Nhân viên xử lý khoản chuyển thêm bên ngoài.
- Không có chức năng nhân viên xác nhận thanh toán thủ công để cấp gói.

### Hủy đơn và webhook đến muộn

- Dùng chung cho hai loại gói: đơn chưa ghi nhận tiền được khách hủy; đã ghi nhận một phần thì không được tự hủy, tiếp tục trả phần thiếu hoặc chờ hết hạn.
- Sau khi hủy hoặc hết hạn, khách có thể tạo đơn mới; giá và quyền lợi được chốt lại tại lúc tạo mới.
- Nếu webhook đến sau khi hủy nhưng tiền đã đủ trước lúc hủy và trong hạn 15 phút, vẫn công nhận lần mua.
- Nếu tiền chỉ đủ sau lúc hủy, không cấp gói; nhân viên xử lý bên ngoài.

### Thứ tự mua gói thiết kế

- Giữ quy tắc đổi gói, đổi chu kỳ hoặc mua lại cùng gói: trả đủ giá kỳ mới, bắt đầu kỳ mới khi hệ thống thực sự cấp gói, cấp đủ hạn mức, bỏ thời gian/lượt dư cũ. Lượt đang giữ tiếp tục thuộc kỳ cũ theo quy tắc subscription hiện có.
- Không tính lùi kỳ sử dụng về thời điểm chuyển tiền do webhook đến chậm.
- Khi có hai đơn hợp lệ, ghi nhận cả hai lần mua. Thứ tự dựa trên thời điểm mỗi đơn nhận đủ tiền; lần mua sau quyết định gói đang hiệu lực. Webhook của lần mua trước đến muộn không thay gói sau.
- Ví dụ A đủ tiền 10:14, B đủ tiền 10:17 nhưng B được thông báo trước: ghi nhận cả A/B, B vẫn hiệu lực. Không dùng đề xuất cũ “đơn cũ đến muộn thì không công nhận và hoàn tiền”.
- Giám sát không có cơ chế thay thế này: mỗi đơn hợp lệ cấp một gói riêng.

### Gán gói giám sát cho dự án

- Khách có thể sở hữu nhiều gói chưa gán, kể cả cùng loại. Mỗi gói gán cho một dự án của khách; gán không thu tiền lần nữa.
- Phải gán lần đầu trong một năm từ lúc cấp gói. Quá hạn chưa từng gán thì mất quyền sử dụng. Gán dự án được coi là đã sử dụng.
- Gán đúng hạn thì tiếp tục phục vụ dự án sau mốc một năm; hạn gán không phải thời hạn phục vụ dự án.
- Mỗi dự án chỉ có một gói giám sát đang hiệu lực. Chặn gán thêm; gói chưa gán giữ nguyên hạn để dùng cho dự án khác.
- Khách không tự gỡ/đổi dự án. Nhân viên có quyền riêng được sửa, phải ghi lý do; dự án đích cùng khách và chưa có gói giám sát hiệu lực.
- Cho phép nhân viên sửa liên kết sau một năm nếu gói từng gán đúng hạn. Không tạo gói mới hoặc làm mới hạn.
- Hiện chỉ quản lý liên kết gói–dự án; không kiểm tra đã/chưa khảo sát hoặc giám sát để cho phép sửa.

### Nhân viên hủy và khôi phục gói

- Chỉ nhân viên được phân quyền riêng mới được thao tác. Hủy và khôi phục đều phải có lý do.
- Hủy áp dụng cho thiết kế và giám sát, kể cả giám sát chưa gán. Hủy hiệu lực ngay, giữ lịch sử; dự án có gói giám sát bị hủy được nhận gói khác.
- Khảo sát thấy công trình không phù hợp: nhân viên hoàn tiền/bù trừ bên ngoài và hủy hiệu lực gói trong hệ thống. Không xây chức năng chuyển tiền hoặc xác nhận đã hoàn tiền.
- Khôi phục giữ nguyên thời hạn, quyền lợi và số lượt trước hủy; không cộng thời gian hoặc cấp lại lượt.
- Chặn khôi phục nếu tài khoản thiết kế hoặc dự án giám sát đã có gói khác hiệu lực.
- Chặn khôi phục thiết kế đã hết kỳ và giám sát chưa từng gán đã quá một năm. Giám sát từng gán đúng hạn vẫn được khôi phục sau một năm nếu không xung đột.
- Chỉ khôi phục gói do nhân viên hủy. Gói thiết kế bị lần mua mới thay thế không được khôi phục; hủy gói mới không tự quay lại gói cũ.

### Tra cứu quản trị gói, đơn và giao dịch

- **Đã xác nhận:** Admin và nhân viên có quyền tra cứu riêng đều được xem ai mua gói nào và các giao dịch trong hệ thống.
- Giao dịch chưa khớp đơn vẫn hiển thị với nhãn “Chưa xác định đơn”; chưa có thao tác gán thủ công. Không tự suy đoán người mua/gói.
- Quyền tra cứu không cấp thêm quyền sửa dự án, hủy hoặc khôi phục; các thao tác đó giữ quyền riêng đã chốt.
- Đã bổ sung [STORY-PAY-002](../userstory/STORY-PAY-002.md), [BR-PAY-005](../businessrule/BR-PAY-005.md) và ST-PAY-047–054. Phạm vi gồm truy vết gói–đơn–giao dịch và lịch sử, không phải chức năng hoàn tiền.

## Đề xuất chưa chốt

Đã lưu thiết kế kỹ thuật và đặc tả Unit Test tại [bàn giao TDD](payment-technical-design.md). Các tên schema/API là phương án kỹ thuật, không phải code đã triển khai.

Danh sách trường, bộ lọc, phân trang, schema, API và cách xử lý đồng thời đã được mô tả trong TDD. Đây là phương án kỹ thuật, chưa phải chức năng đã triển khai.


## Cần làm rõ

**Ngoại lệ đã chốt thêm:** nếu bổ sung giao dịch muộn làm đảo lại thứ tự giữa các gói đã cấp, giữ gói đang hiệu lực; nhân viên xử lý ngoài hệ thống. Không tự khôi phục gói đã bị thay thế.

Không cần hỏi lại các quyết định ở trên. Những điểm chưa có câu trả lời được giữ riêng:

1. Đã giải quyết khi thiết kế TDD: chỉ chấp nhận giao dịch trước hạn/hủy; đúng mốc không hợp lệ. Cùng thời điểm đủ tiền thì đơn tạo sau được coi là mua sau.
2. Đã giải quyết khi thiết kế TDD: hạn gán là cùng ngày/giờ năm sau theo giờ Việt Nam; 29/02 thành 28/02 ở năm không nhuận. Phải gán trước hạn, không gồm đúng mốc.
3. Đã giải quyết khi thiết kế TDD: tác vụ AI bắt đầu hợp lệ trước hủy được hoàn tất theo quyền và lượt đã giữ; chỉ chặn tác vụ mới.
4. Quyền nhân viên gỡ về chưa gán hoặc sửa dự án của gói đang bị hủy chưa được xác nhận; phạm vi đã chốt là sửa dự án đã gán hợp lệ.
5. Còn thiếu cấu hình tài khoản ngân hàng, connection và secret SePay của môi trường triển khai. Hợp đồng provider, cách lưu giao dịch và phục hồi khi cấp gói lỗi đã được thiết kế trong TDD-PAY-001.
6. Sprint, Priority, Creator/Owner và người triển khai/kiểm thử chưa được phân công; ngày hiệu lực/phê duyệt chưa được xác nhận. Reviewer/Approver dùng Tân Trần theo xác nhận cho các bản nháp mới trong ghi chú subscription hiện có; tên metadata không đồng nghĩa đã duyệt.

## Bộ tài liệu đã lưu

| Mục tiêu | User Story | Business Rule | System Test |
| --- | --- | --- | --- |
| Tạo đơn, thanh toán QR và cấp gói | [STORY-PAY-001](../userstory/STORY-PAY-001.md) | [BR-PAY-001](../businessrule/BR-PAY-001.md), [BR-PAY-002](../businessrule/BR-PAY-002.md), [BR-PAY-003](../businessrule/BR-PAY-003.md), [BR-PAY-004](../businessrule/BR-PAY-004.md) | ST-PAY-001–023, ST-PAY-070 |
| Gán và đổi công trình của gói giám sát | [STORY-SUB-004](../userstory/STORY-SUB-004.md) | [BR-SUB-022](../businessrule/BR-SUB-022.md), [BR-SUB-023](../businessrule/BR-SUB-023.md) | ST-PAY-024–033 |
| Hủy và khôi phục gói | [STORY-SUB-005](../userstory/STORY-SUB-005.md) | [BR-SUB-024](../businessrule/BR-SUB-024.md), [BR-SUB-025](../businessrule/BR-SUB-025.md) | ST-PAY-034–046 |
| Tra cứu quản trị người mua, gói và giao dịch | [STORY-PAY-002](../userstory/STORY-PAY-002.md) | [BR-PAY-005](../businessrule/BR-PAY-005.md) | ST-PAY-047–054, ST-PAY-069 |

61 ca ban đầu và hai ca ST-PAY-069, ST-PAY-070 thêm ngày 25/09/2026 là đặc tả, chưa phải mã kiểm thử hoặc kết quả chạy. Mỗi ca có một file, đúng bảng 13 cột và liên kết Story/AC/BR. Các ví dụ giá/ngày/lượt là dữ liệu thử, không phải cấu hình bán thật.

## Ảnh hưởng tới tài liệu cũ

- [BR-SUB-004](../businessrule/BR-SUB-004.md): bản giá/quyền lợi được chốt khi tạo đơn cho cả hai loại gói, không lấy bản mới nhất lúc cấp giám sát.
- [BR-SUB-006](../businessrule/BR-SUB-006.md): giữ giới hạn hiệu lực; bổ sung nhiều gói giám sát chưa gán.
- [BR-SUB-009](../businessrule/BR-SUB-009.md): bỏ bắt buộc dự án ngay khi cấp; bổ sung hạn gán và quyền nhân viên sửa liên kết.
- [BR-SUB-013](../businessrule/BR-SUB-013.md): ngừng bán không hủy điều kiện của đơn đã tạo.
- [BR-SUB-014](../businessrule/BR-SUB-014.md), [BR-SUB-021](../businessrule/BR-SUB-021.md): phân biệt lúc đủ tiền để xếp thứ tự mua với lúc cấp gói để bắt đầu kỳ sử dụng.
- [STORY-SUB-003](../userstory/STORY-SUB-003.md), [BR-SUB-011](../businessrule/BR-SUB-011.md), [BR-SUB-012](../businessrule/BR-SUB-012.md): luồng lịch sử hoàn thành/mở lại không phải hủy/khôi phục. Đợt thanh toán không tự thêm quản lý hoạt động khảo sát/giám sát; phần gán cố định và không thời hạn tuyệt đối phải đọc theo quy tắc mới.
- [TDD-SUB-003](../tdd/TDD-SUB-003.md): mô hình bắt buộc ProjectId, không có hạn gán, cấm sửa ProjectId và quyền theo phân công chưa bao phủ nghiệp vụ mới. Cần sửa ở bước thiết kế kỹ thuật; không dùng nguyên bản để triển khai thanh toán.
- [TDD-SUB-001](../tdd/TDD-SUB-001.md) và [TDD-SUB-002](../tdd/TDD-SUB-002.md): cần nối phiên bản gói với đơn và bổ sung cấp gói theo thứ tự giao dịch, hủy/khôi phục. Thiết kế mới đã lưu tại [bàn giao TDD](payment-technical-design.md).
- [ST-SUB-033](../systemtest/ST-SUB-033.md): điều kiện nhận bản mới của giám sát phải dựa trên lúc tạo đơn mới, không chỉ lúc cấp. Các đặc tả kỹ thuật/UT cũ cần rà soát ở bước thiết kế kỹ thuật, chưa tuyên bố đã đồng bộ toàn bộ chuỗi phụ thuộc.

## Hiện trạng mã nguồn đã kiểm tra

- [ApplicationDbContext](../../bmt-be/src/bmt-be.persistence/ApplicationDbContext.cs) chỉ khai báo DbSet User; chưa có bảng đơn, thanh toán hoặc gói trong file này.
- `RoleNames.cs` lúc khảo sát có User/Admin và quyền xác thực; chưa có các quyền riêng cho sửa dự án, hủy và khôi phục gói. File này nay không còn trong code.
- [ICurrentUserService](../../bmt-be/src/bmt-be.application/abstractions/ICurrentUserService.cs) hiện cung cấp UserId và Role. Không suy ra đã có nguồn phân quyền nhân viên/dự án.
- **Kiểm lại ngày 25/09/2026:** các ghi nhận trên là hiện trạng lúc khảo sát ngày 19/09. Code hiện đã có RBAC với danh mục mã quyền ở `PermissionNames`, danh mục gói, kỳ thiết kế, vòng đời gói và gói giám sát (còn dùng cột `ProjectId`). Vẫn chưa có bảng đơn, giao dịch hoặc tích hợp SePay.
- Tìm mã C# trong src với sepay/payment/subscription/entitlement chưa thấy module tương ứng. Không chạy restore, build, test, dịch vụ hoặc migration trong bước lưu tài liệu.

## Thứ tự chuẩn bị tiếp theo

1. Giải quyết các điểm biên nghiệp vụ còn mở và lấy thông tin tích hợp SePay; không mở lại phần đã chốt.
2. Thiết kế kỹ thuật cho đơn/giao dịch/cấp gói, quyền riêng và liên kết dự án. Xác minh tài liệu chính thức SePay trước khi ghi hợp đồng provider; đã đối chiếu tài liệu chính thức và lưu hợp đồng đề xuất tại TDD-PAY-001; chưa kiểm thử tài khoản thật.
3. Đồng bộ TDD và đặc tả Unit Test cũ bị ảnh hưởng; bổ sung kiểm thử giao dịch lặp/đồng thời, kiểm tra quyền sở hữu và lỗi cấp gói tại ranh giới tích hợp.
4. Triển khai sau khi được giao triển khai; chạy các ca System Test trên môi trường có persistence và tích hợp phù hợp. Hiện chưa có bằng chứng test chạy đạt.

## Kiểm tra tài liệu khi lưu

- Đã kiểm tra 74 tài liệu nghiệp vụ: 4 User Story, 9 Business Rule và 61 System Test.
- 61/61 AC mới có ca System Test truy vết; từng tham chiếu Story/AC và BR/Then tồn tại.
- Mỗi file System Test có đúng một dòng dữ liệu và 13 cột; Trace to khớp TEST_LINKS. Giữ nguyên comment hướng dẫn của ba template.
- Không phát hiện mã tài liệu trùng hoặc liên kết file hỏng trong bộ mới và bản tổng hợp này.
- Đây là kiểm tra cấu trúc/liên kết tài liệu cục bộ, không phải chạy test ứng dụng hoặc kiểm tra nhập tài liệu trên Document First. Chưa rà soát đầy đủ toàn bộ chuỗi tham chiếu lịch sử; các TDD/Unit Test cũ bị ảnh hưởng vẫn cần cập nhật ở bước thiết kế kỹ thuật.

## Bảng truy vết nghiệm thu

Mọi AC mới có ít nhất một ca System Test bên dưới. Các ca từ chối vẫn phải kiểm dữ liệu không đổi; các ca thanh toán cần phân biệt giao dịch thực tế mới với webhook gửi lại.

| System Test | Story / AC | Business Rule | Mục tiêu |
| --- | --- | --- | --- |
| [ST-PAY-001](../systemtest/ST-PAY-001.md) | STORY-PAY-001/AC-001 | BR-PAY-001 | Tạo đơn một gói bằng VNĐ |
| [ST-PAY-002](../systemtest/ST-PAY-002.md) | STORY-PAY-001/AC-002 | BR-PAY-001 | Giữ giá và quyền lợi của đơn |
| [ST-PAY-003](../systemtest/ST-PAY-003.md) | STORY-PAY-001/AC-003 | BR-PAY-001 | Ngừng bán sau khi tạo đơn |
| [ST-PAY-004](../systemtest/ST-PAY-004.md) | STORY-PAY-001/AC-004 | BR-PAY-001 | Giới hạn đơn thiết kế chờ |
| [ST-PAY-005](../systemtest/ST-PAY-005.md) | STORY-PAY-001/AC-005 | BR-PAY-001 | Mua nhiều gói giám sát chưa có công trình |
| [ST-PAY-006](../systemtest/ST-PAY-006.md) | STORY-PAY-001/AC-006 | BR-PAY-002 | Thiếu tiền và QR phần còn thiếu |
| [ST-PAY-007](../systemtest/ST-PAY-007.md) | STORY-PAY-001/AC-007 | BR-PAY-002 | Cộng nhiều khoản đủ tiền |
| [ST-PAY-008](../systemtest/ST-PAY-008.md) | STORY-PAY-001/AC-008 | BR-PAY-002 | Chuyển dư ngay lần đầu |
| [ST-PAY-009](../systemtest/ST-PAY-009.md) | STORY-PAY-001/AC-009 | BR-PAY-002 | Chuyển bổ sung vượt giá |
| [ST-PAY-010](../systemtest/ST-PAY-010.md) | STORY-PAY-001/AC-010 | BR-PAY-004 | Webhook đến muộn |
| [ST-PAY-011](../systemtest/ST-PAY-011.md) | STORY-PAY-001/AC-011 | BR-PAY-002 | Tiền thực tế về muộn |
| [ST-PAY-012](../systemtest/ST-PAY-012.md) | STORY-PAY-001/AC-012 | BR-PAY-002 | Bổ sung sau hạn |
| [ST-PAY-013](../systemtest/ST-PAY-013.md) | STORY-PAY-001/AC-013 | BR-PAY-002 | Webhook trùng không cộng tiền hai lần |
| [ST-PAY-014](../systemtest/ST-PAY-014.md) | STORY-PAY-001/AC-014 | BR-PAY-002 | Webhook đảo thứ tự các khoản bổ sung |
| [ST-PAY-015](../systemtest/ST-PAY-015.md) | STORY-PAY-001/AC-015 | BR-PAY-003 | Khách hủy đơn chưa nhận tiền |
| [ST-PAY-016](../systemtest/ST-PAY-016.md) | STORY-PAY-001/AC-016 | BR-PAY-003 | Chặn hủy đơn nhận một phần |
| [ST-PAY-017](../systemtest/ST-PAY-017.md) | STORY-PAY-001/AC-017 | BR-PAY-003 | Đủ tiền trước hủy nhưng webhook muộn |
| [ST-PAY-018](../systemtest/ST-PAY-018.md) | STORY-PAY-001/AC-018 | BR-PAY-003 | Đủ tiền sau hủy |
| [ST-PAY-019](../systemtest/ST-PAY-019.md) | STORY-PAY-001/AC-019 | BR-PAY-004 | Hai lần chuyển đủ cho cùng đơn |
| [ST-PAY-020](../systemtest/ST-PAY-020.md) | STORY-PAY-001/AC-020 | BR-PAY-004 | Hai đơn thiết kế và webhook đến ngược |
| [ST-PAY-021](../systemtest/ST-PAY-021.md) | STORY-PAY-001/AC-021 | BR-PAY-004 | Đổi hoặc mua lại thiết kế |
| [ST-PAY-022](../systemtest/ST-PAY-022.md) | STORY-PAY-001/AC-022 | BR-PAY-003 | Hủy đơn giám sát dùng chung quy tắc |
| [ST-PAY-023](../systemtest/ST-PAY-023.md) | STORY-PAY-001/AC-023 | BR-PAY-004 | Gói cũ giữ nguyên khi tiền chưa đủ |
| [ST-PAY-024](../systemtest/ST-PAY-024.md) | STORY-SUB-004/AC-001 | BR-SUB-022 | Gán lần đầu không thu tiền lại |
| [ST-PAY-025](../systemtest/ST-PAY-025.md) | STORY-SUB-004/AC-002 | BR-SUB-022 | Quá hạn chưa từng gán |
| [ST-PAY-026](../systemtest/ST-PAY-026.md) | STORY-SUB-004/AC-003 | BR-SUB-022 | Gán đúng hạn vẫn phục vụ sau một năm |
| [ST-PAY-027](../systemtest/ST-PAY-027.md) | STORY-SUB-004/AC-004 | BR-SUB-022 | Chặn công trình đã có gói |
| [ST-PAY-028](../systemtest/ST-PAY-028.md) | STORY-SUB-004/AC-005 | BR-SUB-009 | Khách không tự gỡ hoặc đổi |
| [ST-PAY-029](../systemtest/ST-PAY-029.md) | STORY-SUB-004/AC-006 | BR-SUB-023 | Đã rút ngày 25/09/2026: bỏ đổi công trình |
| [ST-PAY-030](../systemtest/ST-PAY-030.md) | STORY-SUB-004/AC-007 | BR-SUB-023 | Đã rút ngày 25/09/2026: bỏ đổi công trình |
| [ST-PAY-031](../systemtest/ST-PAY-031.md) | STORY-SUB-004/AC-008 | BR-SUB-023 | Đã rút ngày 25/09/2026: bỏ đổi công trình |
| [ST-PAY-032](../systemtest/ST-PAY-032.md) | STORY-SUB-004/AC-009 | BR-SUB-023 | Đã rút ngày 25/09/2026: bỏ đổi công trình |
| [ST-PAY-071](../systemtest/ST-PAY-071.md) | STORY-SUB-004/AC-013 | BR-SUB-009 | Không ai đổi được công trình của gói đã gán |
| [ST-PAY-033](../systemtest/ST-PAY-033.md) | STORY-SUB-004/AC-010 | BR-SUB-022 | Không gán lần đầu sang công trình người khác |
| [ST-PAY-034](../systemtest/ST-PAY-034.md) | STORY-SUB-005/AC-001 | BR-SUB-024 | Hủy gói thiết kế |
| [ST-PAY-035](../systemtest/ST-PAY-035.md) | STORY-SUB-005/AC-002 | BR-SUB-024 | Hủy giám sát chưa gán |
| [ST-PAY-036](../systemtest/ST-PAY-036.md) | STORY-SUB-005/AC-003 | BR-SUB-024 | Hủy giám sát đã gán giải phóng công trình |
| [ST-PAY-037](../systemtest/ST-PAY-037.md) | STORY-SUB-005/AC-004 | BR-SUB-024 | Chặn hủy thiếu quyền hoặc lý do |
| [ST-PAY-038](../systemtest/ST-PAY-038.md) | STORY-SUB-005/AC-005 | BR-SUB-025 | Khôi phục thiết kế giữ nguyên thời hạn và lượt |
| [ST-PAY-039](../systemtest/ST-PAY-039.md) | STORY-SUB-005/AC-006 | BR-SUB-025 | Khôi phục giám sát chưa gán còn hạn |
| [ST-PAY-040](../systemtest/ST-PAY-040.md) | STORY-SUB-005/AC-007 | BR-SUB-025 | Khôi phục giám sát đã gán sau một năm |
| [ST-PAY-041](../systemtest/ST-PAY-041.md) | STORY-SUB-005/AC-008 | BR-SUB-025 | Chặn khôi phục xung đột thiết kế |
| [ST-PAY-042](../systemtest/ST-PAY-042.md) | STORY-SUB-005/AC-009 | BR-SUB-025 | Chặn khôi phục xung đột giám sát |
| [ST-PAY-043](../systemtest/ST-PAY-043.md) | STORY-SUB-005/AC-010 | BR-SUB-025 | Chặn khôi phục thiết kế hết kỳ |
| [ST-PAY-044](../systemtest/ST-PAY-044.md) | STORY-SUB-005/AC-011 | BR-SUB-025 | Chặn khôi phục giám sát quá hạn chưa gán |
| [ST-PAY-045](../systemtest/ST-PAY-045.md) | STORY-SUB-005/AC-012 | BR-SUB-025 | Không khôi phục gói thiết kế đã bị thay thế |
| [ST-PAY-046](../systemtest/ST-PAY-046.md) | STORY-SUB-005/AC-013 | BR-SUB-025 | Chặn khôi phục thiếu quyền hoặc lý do |
| [ST-PAY-047](../systemtest/ST-PAY-047.md) | STORY-PAY-002/AC-001 | BR-PAY-005 | Admin tra cứu người mua và gói |
| [ST-PAY-048](../systemtest/ST-PAY-048.md) | STORY-PAY-002/AC-002 | BR-PAY-005 | Nhân viên có quyền tra cứu |
| [ST-PAY-049](../systemtest/ST-PAY-049.md) | STORY-PAY-002/AC-003 | BR-PAY-005 | Từ chối người không có quyền |
| [ST-PAY-050](../systemtest/ST-PAY-050.md) | STORY-PAY-002/AC-004 | BR-PAY-005 | Giao dịch chưa khớp đơn |
| [ST-PAY-051](../systemtest/ST-PAY-051.md) | STORY-PAY-002/AC-005 | BR-PAY-005 | Xem các khoản chuyển bổ sung |
| [ST-PAY-052](../systemtest/ST-PAY-052.md) | STORY-PAY-002/AC-006 | BR-PAY-005 | Tra cứu đơn không thành công và gói lịch sử |
| [ST-PAY-053](../systemtest/ST-PAY-053.md) | STORY-PAY-002/AC-007 | BR-PAY-005 | Quyền xem không cấp quyền sửa |
| [ST-PAY-054](../systemtest/ST-PAY-054.md) | STORY-PAY-002/AC-008 | BR-PAY-005 | Đối chiếu gói với lần mua và tiền |
| [ST-PAY-055](../systemtest/ST-PAY-055.md) | STORY-PAY-001/AC-024 | BR-PAY-002 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-056](../systemtest/ST-PAY-056.md) | STORY-PAY-001/AC-025 | BR-PAY-003 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-057](../systemtest/ST-PAY-057.md) | STORY-PAY-001/AC-026 | BR-PAY-004 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-058](../systemtest/ST-PAY-058.md) | STORY-PAY-001/AC-027 | BR-PAY-004 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-059](../systemtest/ST-PAY-059.md) | STORY-SUB-004/AC-011 | BR-SUB-022 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-060](../systemtest/ST-PAY-060.md) | STORY-SUB-004/AC-012 | BR-SUB-022 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-061](../systemtest/ST-PAY-061.md) | STORY-SUB-005/AC-014 | BR-SUB-024 | Biên/ngoại lệ bổ sung khi thiết kế kỹ thuật |
| [ST-PAY-069](../systemtest/ST-PAY-069.md) | STORY-PAY-002/AC-003 | BR-RBAC-009 | Thu hồi quyền tra cứu có hiệu lực theo hạn token hoặc buộc đăng xuất |
| [ST-PAY-070](../systemtest/ST-PAY-070.md) | STORY-PAY-001/AC-028 | BR-RBAC-005 | Tài khoản nhân viên không tạo hoặc hủy đơn mua gói |

## Bổ sung thiết kế kỹ thuật

Xem [bàn giao TDD](payment-technical-design.md): 4 TDD, 74 Unit Test và 7 System Test tích hợp bổ sung ST-PAY-062–068. Số liệu 61 ca ở phần kiểm tra nghiệp vụ phía trên không bao gồm 7 ca kỹ thuật này và hai ca thêm ngày 25/09/2026.
