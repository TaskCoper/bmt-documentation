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

# BR-SUB-021

## Rule Info

- **Name**: Đổi gói, đổi chu kỳ hoặc mua lại thiết kế bắt đầu kỳ mới ngay.
- **Category**: Subscription thiết kế
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chọn đổi gói ngay, không so bậc hoặc giá. Sau ví dụ PLUS năm đã dùng 2 tháng còn 10 tháng, người dùng tiếp tục chọn 1: đổi tháng/năm cùng gói cũng áp dụng ngay, bỏ phần còn lại và trả đủ giá kỳ mới.

## Statement

**Bổ sung thanh toán 19/09/2026:** Giá/quyền lợi chốt lúc tạo đơn theo BR-PAY-001. Đủ tiền hợp lệ theo BR-PAY-002/BR-PAY-003 thì cấp gói; kỳ bắt đầu lúc thực sự cấp theo BR-PAY-004. Nếu nhiều đơn hợp lệ được thông báo ngược thứ tự, lần đủ tiền sau quyết định gói hiệu lực; webhook cũ không kích hoạt đè gói sau. Các ghi chú bên dưới để mốc chốt/điều kiện hoàn tất chờ thiết kế đã được giải quyết trong bốn BR thanh toán. Gói bị thay thế không được khôi phục theo BR-SUB-025.

**Phạm vi mới nhất:** chỉ quyền tạo thiết kế mới và tra cứu mẫu được triển khai logic sử dụng/tính lượt trong đợt này. Quyền 3D vẫn cấu hình bật/tắt và hiển thị, chưa điều khiển AI hoặc kiểm tra quyền tạo 3D. Các mô tả xử lý 3D bên dưới là thiết kế cho giai đoạn sau, không phải tiêu chí nghiệm thu hiện tại. Không suy ra AI hiện phải luôn trả hoặc luôn bỏ 3D.

Đổi sang gói thiết kế khác, đổi tháng/năm trong cùng gói hoặc mua lại cùng gói/cùng chu kỳ đều có hiệu lực ngay khi hoàn tất hợp lệ. Không phân loại nâng/hạ theo bậc, giá hoặc quyền lợi để quyết định thời điểm chuyển. Kỳ cũ kết thúc tại thời điểm chuyển; kỳ mới bắt đầu từ đó, cấp đủ hạn mức, bỏ lượt dư và không cộng thời gian cũ. Khách trả đủ giá kỳ đích, không khấu trừ thời gian chưa dùng.

## When

Tài khoản có kỳ thiết kế còn hiệu lực; việc đổi sang gói khác, đổi chu kỳ cùng gói đổi cả gói lẫn chu kỳ hoặc mua lại cùng gói/cùng chu kỳ đã hoàn tất hợp lệ. Chỉ chọn gói hoặc bắt đầu giao dịch chưa đủ điều kiện; điều kiện hoàn tất sẽ thiết kế cùng thanh toán.

## Then

1. Gọi T là thời điểm hoàn tất hợp lệ. Trước T, kỳ cũ giữ nguyên; từ T, kỳ mới có hiệu lực và kỳ cũ không còn cấp quyền cho thao tác mới. Luôn có tối đa một subscription thiết kế hiệu lực.
2. Áp dụng chung cho gói đích giá cao hơn, thấp hơn hoặc bằng giá, nhiều hoặc ít quyền hơn; không cần bậc gói. Đây không phải thứ tự các mức của một quyền lợi; quy tắc mức tính năng trong BR-SUB-005 vẫn giữ nguyên.
3. Khách được chọn chu kỳ tháng hoặc năm của gói đích. Tháng sang năm và năm sang tháng trong cùng gói cũng chuyển ngay tại T; không có yêu cầu hẹn đổi cuối kỳ trong phạm vi này.
4. Tính ngày hết hạn mới từ T theo BR-SUB-014, cùng giờ Việt Nam, thiếu ngày lấy cuối tháng đích. Không giữ ngày hết hạn cũ hoặc cộng phần thời gian còn lại. Hạn mức năm dùng cả năm, không làm mới mỗi tháng.
5. Cấp đủ hạn mức của các quyền dạng lượt trong bản quyền lợi đích đã xác định hợp lệ. Bỏ lượt dư kỳ cũ, không cộng sang kỳ mới và không trừ lượt đã dùng kỳ cũ vào hạn mức mới. Không tự cấp quyền không có trong gói đích; không giới hạn vẫn theo BR-SUB-005.
6. Lượt đang giữ thuộc kỳ cũ, không bị xóa như lượt dư. Tác vụ thành công ghi đã dùng ở kỳ cũ; lỗi giải phóng ở kỳ cũ, không cộng vào kỳ mới hoặc mở lại kỳ đã kết thúc. Timeout và kết quả muộn vẫn theo BR-SUB-016. Tác vụ AI đã được tiếp nhận hợp lệ hoàn tất theo bộ quyền của gói tại thời điểm tiếp nhận, kể cả quyền 3D, dù khách đổi gói hoặc chu kỳ trong lúc xử lý. Không bổ sung hoặc bỏ đầu ra của tác vụ đó theo gói mới. Yêu cầu mới sau khi chuyển kiểm tra quyền gói mới. Quy tắc này không cho phép trả kết quả muộn sau timeout; BR-SUB-016 vẫn áp dụng.
7. Trả đủ giá kỳ đích đã xác định hợp lệ, không khấu trừ thời gian cũ chưa dùng. Ví dụ PLUS năm còn 10 tháng đổi sang PLUS tháng: bỏ phần thời gian còn lại, kỳ tháng bắt đầu ngay tại T và trả đủ giá tháng. Không tự quyết định hoàn tiền/tranh chấp hoặc khuyến mãi từ quy tắc này.
8. Giao dịch chưa hoàn tất không tự kết thúc kỳ cũ, cấp kỳ mới hay làm mới lượt. Việc bỏ lịch đổi cuối kỳ không loại bỏ nhu cầu xử lý giao dịch đang chờ, lỗi hoặc gửi lặp; chi tiết thuộc thiết kế thanh toán và TDD.
9. Không tự xóa dữ liệu và kết quả đã có do đổi gói. Quyền truy cập dữ liệu cũ vẫn theo BR-SUB-007; thao tác tạo mới hoặc tra cứu mới kiểm tra gói hiện tại theo BR-SUB-017. Admin sửa danh mục không tự chuyển gói của khách.

10. Cho phép khách chủ động mua lại đúng cùng gói và cùng chu kỳ khi kỳ hiện tại còn hiệu lực, kể cả đã hết lượt hoặc còn lượt dư. Khi hoàn tất hợp lệ, trả đủ giá kỳ mới, kết thúc kỳ cũ và bắt đầu kỳ mới ngay tại T; cấp đủ hạn mức, bỏ thời gian và lượt dư cũ. Không cộng dồn, không chỉ nạp thêm lượt hoặc tự gia hạn. Lượt giữ và quyền của tác vụ đã tiếp nhận vẫn thuộc kỳ cũ theo khoản 6. Bản giá/quyền lợi và mốc chốt bản của giao dịch mua lại vẫn thiết kế cùng thanh toán; không tự suy ra được giữ bản cũ hoặc phải dùng bản mới nhất.

## Except

Không áp dụng cho giám sát. Mốc chốt giá/quyền cho giao dịch, gói ngừng bán trong lúc giao dịch và kỳ tương lai đã trả trước thiết kế cùng thanh toán; không lấy chính sách lịch chuyển đã bỏ làm quy tắc giao dịch mới.

## Notes

- Người dùng chọn 1: mua lại cùng gói/cùng chu kỳ trước hạn áp dụng ngay; xem STORY-SUB-001/AC-070 và [ST-SUB-103](../systemtest/ST-SUB-103.md).

- Người dùng đồng ý giữ bộ quyền lúc tiếp nhận cho tác vụ đang chạy; xem STORY-SUB-001/AC-069 và [ST-SUB-102](../systemtest/ST-SUB-102.md).

- Thay thế BR-SUB-018, BR-SUB-019 và BR-SUB-020 về đổi gói, bậc và lịch hạ. Thay phần đổi chu kỳ chờ cuối kỳ trong BR-SUB-015. Không thay cấu hình giá, hạn mức, quyền chung tháng/năm hoặc quy tắc giữ quyền đã cấp khi Admin sửa danh mục.
- Chính sách cũ hủy/sửa yêu cầu hẹn cuối kỳ và các ca kiểm tra tương ứng được rút khỏi nghiệm thu; giữ mã để tra lịch sử, không tái sử dụng.
- STORY-SUB-001/AC-067 và AC-068, ST-SUB-100 và ST-SUB-101 đặc tả hai nhóm đổi gói/đổi chu kỳ. Dữ liệu thử không phải giá bán. Chưa chạy kiểm thử, chưa triển khai hoặc phê duyệt tài liệu; metadata còn thiếu.
