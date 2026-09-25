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


# BR-SUB-013

## Rule Info

- **Name**: Ngừng bán chặn cấp mới, giữ nguyên gói đã cấp.
- **Category**: Quản trị gói
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chốt ngừng bán ẩn gói, chặn yêu cầu mới, giữ gói đã cấp. Quyết định mới bỏ lịch chuyển cuối kỳ; xử lý giao dịch đang chờ thanh toán khi ngừng bán còn cần thiết kế.

## Statement

Ngừng bán ẩn gói và chặn yêu cầu đăng ký hoặc chuyển mới sang gói đó. Gói đã cấp tiếp tục theo quyền và vòng đời hiện có. Không tự kết thúc hoặc chuyển gói của khách.

## When

Admin ngừng bán một gói thiết kế hoặc giám sát; hoặc có yêu cầu cấp mới từ gói đã ngừng bán.

## Then

1. Gói ngừng bán không xuất hiện trong danh sách gói có thể đăng ký.
2. Từ chối tạo đơn mua mới hoặc yêu cầu chuyển mới sang gói đã ngừng bán, kể cả khi gửi trực tiếp với mã gói đã biết. Đơn đã tạo trước khi ngừng bán vẫn giữ giá/quyền lợi và được cấp gói nếu thanh toán hợp lệ theo BR-PAY-001–004; không chặn cấp chỉ vì lúc nhận webhook gói đã ngừng bán.
3. Subscription thiết kế đã cấp tiếp tục đến hết kỳ hiện tại với quyền lợi, lượt đã dùng và lượt đang giữ không thay đổi do ngừng bán.
4. Gói giám sát đã cấp tiếp tục theo công trình, không tự hoàn thành hoặc hết hạn do ngừng bán; giữ nguyên quyền lợi đã cấp.
5. Không xóa gói đã cấp, không tự đổi sang gói khác và không làm mới hạn mức do ngừng bán.

## Except

Đơn đã tạo trước khi ngừng bán được tiếp tục theo điều kiện đã lưu và hạn thanh toán. Mở bán lại và cơ chế gia hạn riêng vẫn chưa chốt; không tự cho tạo đơn mới mua gói ngừng bán. Ngoại lệ lịch chuyển cuối kỳ của mô hình cũ không áp dụng.

## Notes

- Phần giao dịch đang chờ đã được chốt tại [BR-PAY-001](BR-PAY-001.md), [BR-PAY-002](BR-PAY-002.md), [BR-PAY-003](BR-PAY-003.md), [BR-PAY-004](BR-PAY-004.md), thay các ghi chú chờ thiết kế thanh toán trước đây.

- Ngừng bán là trạng thái của gói trong danh mục, không phải hoàn thành gói giám sát của một khách hàng.
- Mở lại cùng gói giám sát đã cấp là sửa trạng thái theo [BR-SUB-012](BR-SUB-012.md), không phải cấp gói mới.
- Luồng mua và nhận gói theo STORY-PAY-001; kiểm thử ràng buộc cấp mới tại điểm tích hợp sẽ được xác định trong TDD.
- Tham chiếu [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-004](BR-SUB-004.md), [BR-SUB-011](BR-SUB-011.md), [ST-SUB-034](../systemtest/ST-SUB-034.md) và [ST-SUB-035](../systemtest/ST-SUB-035.md).
- ST-SUB-096 và STORY-SUB-002/AC-021 được giữ như lịch sử của lịch chuyển đã bỏ, không dùng nghiệm thu hiện tại.
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
