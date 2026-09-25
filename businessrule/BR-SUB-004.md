<!-- HƯỚNG DẪN CHO AI KHI ĐIỀN HOẶC CHỈNH SỬA MẪU
- Đọc toàn bộ mẫu, các comment hướng dẫn và tài liệu nguồn được cung cấp trước khi viết. Tuân thủ các quy tắc nghiệp vụ, điều kiện, ngoại lệ và giới hạn đã được xác nhận.
- Không bịa, tự chế hoặc suy đoán thành sự thật: yêu cầu, quy tắc, số liệu, API, schema, mã lỗi, nguồn tham khảo, người phụ trách và kết quả kiểm thử phải có căn cứ. Nội dung ví dụ trong mẫu không phải dữ kiện của dự án.
- Khi thiếu thông tin hoặc các nguồn mâu thuẫn, hỏi người dùng để làm rõ; giữ nguyên placeholder ở phần chưa xác định. Không tự chọn đáp án, lấp chỗ trống hay tuyên bố tài liệu đã hoàn tất.
- Chỉ thay nội dung cần điền hoặc được yêu cầu sửa. Không tự mở rộng phạm vi, sửa nghĩa quy tắc, bỏ điều kiện/ngoại lệ, xoá hay ghi đè nội dung hợp lệ đã có.
- Giữ nguyên tên, cấp và thứ tự heading, nhãn in đậm, tiền tố bullet, cấu trúc danh sách, tên/thứ tự/số cột bảng và cú pháp code fence. Không dịch, đổi tên, gộp hoặc thêm mục ngoài cấu trúc mẫu.
- Chỉ lặp khối luồng, AC hoặc API theo đúng khuôn khi có căn cứ; chỉ bỏ phần tuỳ chọn khi hướng dẫn riêng của mẫu cho phép và đã xác định không áp dụng.
- Mã tham chiếu và section phải trỏ tới tài liệu có thật đã được cung cấp hoặc kiểm chứng. Không giữ mã ví dụ như tham chiếu thật và không tự tạo tài liệu liên quan để hợp thức hoá liên kết.
- Giữ các comment hướng dẫn trong file. Xuất Markdown UTF-8, mỗi file một tài liệu; không bọc toàn bộ file trong code fence, không chèn lời dẫn hay giải thích ngoài mẫu.
- Trước khi bàn giao, đối chiếu nội dung với nguồn, kiểm tra cấu trúc, giá trị cho phép, giới hạn độ dài và tính nhất quán của mã/tham chiếu. Báo rõ phần chưa đủ thông tin; không tuyên bố đã chạy test hoặc import khi chưa thực hiện.
-->

<!-- Thay mã BR-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Effective Date dùng YYYY-MM-DD hoặc để trống. Status: Draft / Active / Deprecated.
Statement, When, Then và Except phải phản ánh đúng chính sách nguồn; không tự đặt công thức, ngưỡng, thời hạn hay ngoại lệ. Chỉ để Except trống khi đã xác định không có ngoại lệ; thiếu thông tin thì hỏi lại.
Version và phê duyệt do hệ thống quản lý. Owner là tên nghiệp vụ; gán tài khoản trên giao diện.
Liên kết Story và các trường quản trị bổ sung trên giao diện sau import.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Name tối đa 300 ký tự (được dùng làm tiêu đề tài liệu); Category tối đa 200; Owner tối đa 200; Source tối đa 500.
- Effective Date: dùng ngày hợp lệ YYYY-MM-DD hoặc để trống khi chưa xác định; ngày không đọc được sẽ bị để trống kèm cảnh báo. Không tự đặt ngày hiệu lực.
ĐỐI CHIẾU FORM BUSINESS RULE (src/features/business-rules/validations.ts; form có thể chặt hơn import):
- Form yêu cầu Name, Category, Statement, When, Then, Source và Owner có nội dung; Except và Notes được phép trống. Liên kết Story đã khai báo không được rỗng.
- Form còn kiểm tra Version và Effective Date không rỗng; với file nhập mới, vẫn để Version trống theo hợp đồng import vì hệ thống quản lý phiên bản. Thiếu ngày hiệu lực thì hỏi lại, không bịa để thoả form.
-->


# BR-SUB-004

## Rule Info

- **Name**: Thiết kế dùng bản quyền lợi đã chốt cho từng kỳ; giám sát giữ quyền gói đã cấp.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chốt lưu nháp → kiểm tra → Công bố; giữ quyền kỳ thiết kế hiện tại và quyền gói giám sát đã cấp. Với kỳ thiết kế mới, dùng bản quyền lợi đã chốt khi đăng ký/gia hạn, không tự lấy bản công bố mới nhất. Thời điểm chốt cụ thể sẽ thiết kế cùng thanh toán. Giám sát chỉ áp dụng thay đổi cho gói cấp mới.

## Statement

**Cập nhật thanh toán 19/09/2026:** Mốc chốt giá, toàn bộ quyền lợi và nội dung cam kết của lần mua là lúc tạo đơn theo [BR-PAY-001](BR-PAY-001.md). Các ghi chú bên dưới từng để mốc này chờ thiết kế thanh toán đã được giải quyết; không tự lấy bản mới tại lúc cấp gói. Không thay phạm vi tư vấn/quà tặng đã chốt.

Khi Admin sửa quyền lợi của gói, subscription thiết kế giữ nguyên quyền lợi của kỳ hiện tại; gói giám sát đã cấp giữ nguyên quyền lợi đã nhận. Admin lưu thay đổi thành bản nháp để kiểm tra trước khi bấm Công bố. Với thiết kế, kỳ mới sử dụng bản quyền lợi đã công bố và đã chốt cho lần đăng ký/gia hạn đó, không tự lấy bản công bố mới nhất khi kỳ bắt đầu. Với giám sát, thay đổi chỉ áp dụng cho gói cấp mới. Bản nháp không ảnh hưởng quyền lợi khách hàng.

## When

Admin sửa, lưu nháp hoặc công bố quyền lợi của gói, hoặc hệ thống áp dụng bản quyền lợi đã chốt cho kỳ thiết kế mới hợp lệ.

## Then

1. Với thiết kế, giữ nguyên quyền lợi và hạn mức đã cấp cho kỳ hiện tại. Việc sửa gói không làm mới số lượt, thay đổi lượt đã dùng hoặc lượt đang giữ của kỳ này.
2. Với thiết kế, khi bắt đầu một kỳ hợp lệ, dùng đúng bản quyền lợi đã chốt khi đăng ký/gia hạn kỳ đó. Nếu đã chốt bản B có 20 lượt rồi Admin công bố C có 30 lượt trước lúc kỳ bắt đầu, kỳ đó vẫn nhận B với 20 lượt. Bản chỉ lưu nháp không được dùng để chốt hoặc cấp quyền.
3. Lưu nháp không thay thế bản đang công bố. Admin kiểm tra rồi Công bố để bản mới có thể được chọn cho các lần đăng ký/gia hạn phù hợp; Công bố không thay thế bản đã chốt cho một kỳ hoặc sửa quyền kỳ hiện tại.
4. Hạn mức thiết kế kỳ mới tuân theo quy tắc không cộng dồn; tác vụ đã giữ lượt ở kỳ cũ vẫn được tính vào kỳ cũ.

5. Với giám sát, giữ nguyên mô tả dịch vụ của từng gói đã cấp; gói giám sát không dùng danh mục quyền lợi theo BR-SUB-008 khoản 7. Chốt mô tả khi tạo đơn theo BR-PAY-001, không lấy bản mới nhất tại lúc cấp gói. Đơn tạo trước khi công bố thay đổi vẫn cấp theo bản đã lưu; đơn mới chọn bản công bố phù hợp tại lúc tạo. Lưu nháp không được dùng để cấp quyền.

6. Bản đã chốt gồm toàn bộ quyền lợi của kỳ: hạn mức, quyền bật/tắt và mức tính năng. Không ghép hạn mức của bản đã chốt với quyền bật/tắt hoặc mức của bản công bố sau đó. Bản đã chốt được ghi nhận lúc tạo đơn theo BR-PAY-001; không tự thêm luồng nhận gói hoặc gia hạn thủ công.

7. Đổi gói hoặc chu kỳ ngay theo BR-SUB-021. Chính sách cũ giữ giá/quyền cho lịch chuyển cuối kỳ không còn nhánh áp dụng; giá và quyền của giao dịch đổi ngay chốt lúc tạo đơn theo BR-PAY-001. Không thay nguyên tắc giữ bản quyền lợi đã chốt cho từng kỳ.

8. Khách đã mua giữ quyền lợi tư vấn offline theo nội dung mô tả đã chốt cho lần mua đó trong suốt kỳ đã mua. Admin sửa hoặc công bố mô tả mới không thay cam kết tư vấn của kỳ đang dùng; nội dung mới áp dụng cho lần mua mới. Tư vấn vẫn là mô tả tự do, không phải entitlement dạng mức hoặc hạn mức lượt. Nội dung tư vấn của lần mua chốt lúc tạo đơn theo BR-PAY-001.

## Except

Khách chủ động đổi gói hoặc chu kỳ theo BR-SUB-021 sẽ kết thúc kỳ cũ và bắt đầu kỳ mới. Đây không phải Admin sửa danh mục rồi tự thay quyền kỳ đang dùng. Ngừng bán theo BR-SUB-013.

## Notes

- Người dùng chọn 1: giữ cam kết tư vấn offline của kỳ đã mua. Xem STORY-SUB-002/AC-024 và [ST-SUB-106](../systemtest/ST-SUB-106.md). Quyết định này chỉ chốt nội dung tư vấn, không tự áp dụng cho mọi chương trình quà tặng hoặc dịch vụ offline khác.

- ST-SUB-099 là đặc tả lịch sử của lịch chuyển cuối kỳ đã bỏ; không phải nghiệm thu giao dịch đổi ngay.

- Phần lưu nháp/Công bố áp dụng cho danh mục gói. Với thiết kế, thay đổi chỉ có thể áp dụng ở kỳ mới qua bản được chốt cho kỳ đó; không mặc định kỳ mới luôn nhận mọi thay đổi vừa công bố. Giám sát không có kỳ tiếp theo; gói đã cấp giữ nguyên quyền lợi, thay đổi chỉ áp dụng cho gói cấp mới. Hoàn thành rồi mở lại cùng gói không phải cấp gói mới, nên không tự đổi quyền lợi.
- Ví dụ minh họa: kỳ hiện tại có 10 lượt, đã dùng 2. Khách chốt bản B với 20 lượt cho kỳ mới; sau đó Admin công bố C với 30 lượt. Kỳ hiện tại vẫn còn 8 lượt; kỳ mới nhận đúng 20 lượt của B, không phải 30 và không cộng thêm 8 lượt dư. Không tự coi khách được giữ B cho mọi kỳ sau; mỗi lần đăng ký/gia hạn có bản đã chốt tương ứng.
- Quy tắc áp dụng cho quyền lợi nói chung, không chỉ hạn mức lượt. Mốc chốt giá của giao dịch đổi ngay là lúc tạo đơn theo BR-PAY-001.
- Điều kiện tối thiểu về quyền lợi theo [BR-SUB-008](BR-SUB-008.md): gói chưa có quyền lợi được lưu nháp, nhưng không được Công bố; phải bổ sung ít nhất một quyền lợi hợp lệ.
- Luồng lưu nháp → kiểm tra → Công bố đã chốt; chưa đặt thêm người duyệt riêng. Cách chọn bản cho kỳ thiết kế đã chốt: dùng bản ghi nhận khi đăng ký/gia hạn. Thời điểm chốt và các tình huống giao dịch theo BR-PAY-001 đến BR-PAY-004; các điều kiện công bố còn thiếu tiếp tục được làm rõ.
- Không tự tạo hoặc gia hạn subscription chỉ vì gói được sửa. Luồng mua và nhận gói theo STORY-PAY-001.
- Tham chiếu [STORY-SUB-001](../userstory/STORY-SUB-001.md), [BR-SUB-002](BR-SUB-002.md), [BR-SUB-003](BR-SUB-003.md) và [ST-SUB-007](../systemtest/ST-SUB-007.md).
- [ST-SUB-008](../systemtest/ST-SUB-008.md) kiểm tra bản nháp không được áp dụng cho kỳ tiếp theo.
- [ST-SUB-033](../systemtest/ST-SUB-033.md) kiểm tra giữ quyền lợi gói giám sát đã cấp và áp dụng bản mới cho gói cấp mới. Quản lý lịch và lượt giám sát vẫn hoãn.
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
