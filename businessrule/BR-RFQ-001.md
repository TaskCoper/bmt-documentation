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

# BR-RFQ-001

## Rule Info

- **Name**: Khách gửi lời mời từ hồ sơ của mình đến nhà thầu đang hiển thị.
- **Category**: Mời báo giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị tính năng Mời báo giá ngày 03/10/2026. Người dùng đã chốt bộ US/BR và giao triển khai; chưa phê duyệt hoặc import trên hệ thống quản lý tài liệu.

## Statement

Khách được gửi lời mời khảo sát để nhận báo giá từ hồ sơ của mình đến nhà thầu đang hiển thị. Không yêu cầu hồ sơ phải hoàn thành thiết kế hoặc dự toán. Mỗi lời mời gắn với đúng một hồ sơ, một khách gửi và một nhà thầu.

## When

Khách mở biểu mẫu hoặc gửi lời mời báo giá.

## Then

1. Kiểm tra tài khoản khách và quyền sở hữu hồ sơ tại lúc gửi. Không cho dùng hồ sơ của khách khác bằng cách thay mã hồ sơ.
2. Hồ sơ phải còn tồn tại. Chỉ cần là hồ sơ của khách; không thêm điều kiện hoàn tất thiết kế, dự toán hoặc mua gói để được gửi.
3. Nhà thầu phải tồn tại và đang hiển thị. Thông tin đang nhận dự án không phải điều kiện tiếp nhận; nhà thầu đang hiển thị nhưng ngừng nhận dự án vẫn được nhận lời mời.
4. Khách chọn ngày và khung giờ mong muốn từ danh sách có sẵn theo cách nhập của tư vấn 1:1; số điện thoại liên lạc bắt buộc, ghi chú khảo sát tùy chọn.
5. Điền sẵn số điện thoại từ tài khoản nếu có. Khách được thay số trước khi gửi; lưu số dùng cho lời mời, không tự cập nhật số điện thoại tài khoản.
6. Chỉ tiếp nhận khi đáp ứng giới hạn và điều kiện không trùng nhà thầu theo BR-RFQ-002. Lưu bản hồ sơ tại thời điểm gửi theo BR-RFQ-003, khởi tạo trạng thái Đã gửi và gửi email theo BR-RFQ-005.
7. Khách xem các lời mời của hồ sơ mình, gồm nhà thầu, thông tin đã gửi, trạng thái hiện tại và lịch hẹn mới nhất. Khách không được sửa, hủy hoặc xóa lời mời sau khi gửi.
8. Nhà thầu không có tài khoản đăng nhập trong phạm vi này. Admin liên hệ nhà thầu và sắp xếp lịch; việc tiếp nhận lời mời không tự xác nhận rằng nhà thầu đã đồng ý lịch.

## Except

Nhà thầu ngừng nhận dự án nhưng vẫn đang hiển thị không bị chặn gửi. Không có ngoại lệ cho quyền sở hữu hồ sơ hoặc việc gửi trùng nhà thầu.

## Notes

- Quy tắc chỉ mượn cách nhập của tư vấn 1:1, không tự mang toàn bộ chính sách của tính năng đó sang mời báo giá. Không thêm điều kiện xác minh email hoặc trừ lượt gói khi chưa có yêu cầu.
- Danh sách giờ cụ thể, múi giờ, kiểm tra ngày giờ và giới hạn định dạng sẽ được đối chiếu khi thiết kế; chưa tự đặt thời gian báo trước tối thiểu hoặc lịch rảnh riêng của nhà thầu.
- Phục vụ STORY-RFQ-001; phạm vi quản trị theo BR-RFQ-004.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại. Owner và ngày hiệu lực chưa xác định. Người dùng đã chốt bộ US/BR này và giao triển khai. Đã soạn đặc tả System Test; chưa triển khai mã ứng dụng hoặc chạy kiểm thử. Status Draft không thay thế xác nhận hội thoại.
