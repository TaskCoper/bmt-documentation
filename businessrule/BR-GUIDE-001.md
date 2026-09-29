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

# BR-GUIDE-001

## Rule Info

- **Name**: Quyền quản lý và vòng đời hướng dẫn
- **Category**: Video hướng dẫn
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị tính năng video hướng dẫn BMT ngày 30/09/2026.

## Statement

Người có quyền Quản lý hướng dẫn được tạo, sửa, xuất bản, ẩn, xóa và sắp xếp hướng dẫn. Hướng dẫn mới là bản Nháp; chỉ bản Xuất bản được cung cấp công khai.

## When

Người dùng truy cập phần quản trị hoặc yêu cầu thay đổi nội dung, trạng thái, thứ tự hướng dẫn.

## Then

1. Kiểm quyền Quản lý hướng dẫn theo cơ chế phân quyền hiện có. Người chưa đăng nhập hoặc không có quyền không được thực hiện thao tác quản trị.
2. Hướng dẫn có ba trạng thái Nháp, Xuất bản và Ẩn. Tạo mới chỉ lưu Nháp, không tự xuất bản.
3. Cho lưu nháp thiếu thông tin. Khi xuất bản bản Nháp hoặc Ẩn, phải có tiêu đề, mô tả ngắn và link video YouTube hợp lệ; kiểm tra video theo BR-GUIDE-002.
4. Sửa hướng dẫn đang xuất bản có hiệu lực ngay khi lưu thành công, không tạo bản sửa chờ xuất bản riêng. Bản sửa vẫn phải đủ tiêu đề, mô tả ngắn và link hợp lệ; nếu thay video thì video mới phải được kiểm tra thành công. Nếu thiếu thông tin hoặc chưa kiểm tra được video mới, từ chối lưu, báo lỗi và giữ nguyên toàn bộ bản đang hiển thị. Nếu cần chuẩn bị, người quản lý ẩn trước rồi sửa.
5. Ẩn hướng dẫn làm hướng dẫn không xuất hiện trong danh sách công khai và không được trả về qua yêu cầu đọc công khai mới. Người quản lý được xuất bản lại sau khi đáp ứng điều kiện xuất bản.
6. Chỉ được xóa hẳn hướng dẫn đang nháp hoặc đã ẩn. Yêu cầu xóa hướng dẫn đang xuất bản bị từ chối, giữ nguyên dữ liệu và yêu cầu ẩn trước.
7. Xóa hẳn là xóa hướng dẫn khỏi BMT, không có thùng rác hoặc khôi phục trong tính năng này và không xóa video trên YouTube.
8. Admin được chủ động sắp xếp thứ tự. Thay đổi thứ tự không làm thay đổi trạng thái xuất bản của hướng dẫn.

## Except

Bản nháp được thiếu thông tin bắt buộc khi xuất bản. Quyền xem công khai không cấp quyền quản trị.

## Notes

Quy tắc này áp dụng cho cả giao diện và yêu cầu gửi trực tiếp đến API. Tên và phạm vi quyền đã được xác nhận; mã quyền kỹ thuật và cách bổ sung vào các vai trò thuộc bước thiết kế.

Người dùng đã xác nhận quy tắc giữ bản đang hiển thị khi lưu sửa thất bại. Reviewer và Approver là Tân Trần. Bản US/BR sau cập nhật chưa được chốt toàn văn; tên người phê duyệt không tự xác nhận tài liệu đã được duyệt. Owner và ngày hiệu lực chưa được cung cấp.
