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

# BR-PROJ-005

## Rule Info

- **Name**: Khóa sửa đầu vào khi AI xử lý và cho sửa hoặc thử lại sau thất bại.
- **Category**: Tạo dự toán — xử lý AI
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại: khóa sửa khi AI đang chạy; thất bại thì được sửa hoặc thử lại. Bản dự toán đã thành công muốn phương án khác phải tạo dự toán mới theo BR-SUB-017.

## Statement

Sau khi yêu cầu tạo thiết kế được tiếp nhận hợp lệ, đầu vào của bản dự toán bị khóa sửa trong thời gian AI xử lý. Nếu tác vụ thất bại và bản dự toán chưa có kết quả thành công, khách được sửa thông tin hoặc chủ động thử lại khi còn đáp ứng điều kiện sử dụng.

## When

Khách gửi yêu cầu tạo thiết kế, lưu thay đổi đầu vào khi AI đang xử lý, hoặc sửa/thử lại một bản dự toán đã xử lý thất bại.

## Then

1. Khóa sửa đầu vào khi tiếp nhận hợp lệ yêu cầu tạo thiết kế. Tác vụ dùng đúng thông tin đã tiếp nhận.
2. Từ chối cập nhật đầu vào trong lúc AI đang xử lý, kể cả yêu cầu gửi trực tiếp và yêu cầu tự lưu đến muộn từ biểu mẫu đã mở trước đó. Không chỉ khóa các trường trên giao diện.
3. Khi tác vụ thất bại, gồm thất bại do quá thời gian theo BR-SUB-016, mở lại khả năng sửa đầu vào nếu bản dự toán chưa có kết quả thành công. Quyền tạo/lưu và điều kiện gói/lượt vẫn phải đáp ứng BR-SUB-007.
4. Cho phép khách chủ động thử lại cùng bản dự toán, giữ dữ liệu đã có; khách có thể sửa trước khi thử lại. Mỗi yêu cầu thử lại phải kiểm tra lại dữ liệu, quyền và lượt theo BR-SUB-017.
5. Sau khi yêu cầu thử lại được tiếp nhận hợp lệ, khóa sửa lại cho đến kết quả xử lý cuối cùng.
6. Khi bản dự toán đã thành công, muốn phương án khác phải tạo dự toán mới. Không dùng thao tác sửa hoặc thử lại để thay thế kết quả thành công theo BR-SUB-017.
7. Kết quả đến muộn của lần xử lý đã quá thời gian không mở lại thành công, không gắn vào lần thử lại và không được công bố theo BR-SUB-016.

## Except

Yêu cầu gửi tạo thiết kế bị từ chối trước khi được tiếp nhận hợp lệ không làm bản dự toán chuyển sang đang xử lý hoặc khóa đầu vào theo quy tắc này. Việc không được lưu do hết hạn gói, thiếu quyền hoặc hết lượt vẫn áp dụng độc lập theo BR-SUB-007.

## Notes

- Chỉ sửa dữ liệu sau thất bại không tự gửi lại AI. Khách phải chủ động gửi yêu cầu mới; không tự thử lại tác vụ theo các quy tắc hiện có.
- Quy tắc khóa ở đây áp dụng cho đầu vào thiết kế, không áp dụng cho tên bản dự toán; đổi tên theo BR-SUB-007 khoản 11 (người dùng xác nhận ngày 25/09/2026). Không tự mở rộng thành chính sách khóa hoặc cho phép đổi các thông tin quản trị khác.
- Không thêm quyền khách hàng hủy tác vụ; phạm vi hiện tại theo BR-SUB-003.
- Mốc tiếp nhận, chống yêu cầu trùng và bảo vệ khỏi cập nhật đồng thời sẽ xác định trong thiết kế; không chọn công nghệ giao tiếp hoặc trạng thái API của AI khi chưa có hợp đồng.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
