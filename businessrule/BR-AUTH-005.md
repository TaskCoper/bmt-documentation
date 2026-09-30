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

# BR-AUTH-005

## Rule Info

- **Name**: Mật khẩu tạm gửi qua email không hết hạn theo thời gian; dùng mật khẩu tạm phải đổi trước khi tiếp tục.
- **Category**: Xác thực và đăng nhập Google
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại ngày 30/09/2026: người dùng chọn gửi mật khẩu ngẫu nhiên qua email, dùng mật khẩu tạm không hết hạn theo thời gian; chỉ hết hiệu lực khi đổi hoặc đặt lại thành công; đăng nhập Google bình thường; không nhận được thư hoặc mất mật khẩu thì dùng Quên mật khẩu.

## Statement

Tài khoản khách hàng mới qua Google và tài khoản chưa xác minh được tiếp nhận theo BR-AUTH-004 nhận một mật khẩu tạm ngẫu nhiên qua email. Mật khẩu này không hết hạn theo thời gian, nhưng chỉ cho phép khách vào bước đổi mật khẩu. Đăng nhập bằng Google không bị hạn chế vì mật khẩu tạm chưa được đổi.

## When

Hệ thống cấp mật khẩu tạm cho khách, khách đăng nhập bằng mật khẩu đó, đăng nhập Google, đổi mật khẩu hoặc dùng Quên mật khẩu.

## Then

1. Sinh mật khẩu tạm ngẫu nhiên khi tạo tài khoản khách hàng qua Google hoặc khi tiếp nhận tài khoản chưa xác minh theo BR-AUTH-004/Then khoản 4. Gửi mật khẩu đến email đã xác minh của khách. Không gửi thư mật khẩu cho mỗi lần đăng nhập Google.
2. Mật khẩu tạm không hết hạn chỉ vì thời gian trôi qua. Khi khách đổi hoặc đặt lại mật khẩu thành công, mật khẩu tạm mất hiệu lực; nếu thao tác thất bại thì không tự vô hiệu mật khẩu đang có.
3. Đăng nhập bằng mật khẩu tạm đúng chỉ cấp quyền thực hiện bước đổi mật khẩu trước khi tiếp tục. Khách chưa được dùng các chức năng nghiệp vụ, kể cả tài khoản đã xác minh email và có vai trò Khách hàng. Làm mới phiên không được bỏ giới hạn này.
4. Phiên được tạo từ đăng nhập Google không bị bắt đổi mật khẩu chỉ vì tài khoản còn mật khẩu tạm. Quy tắc này không bỏ qua trạng thái khóa, xóa, loại tài khoản hoặc các điều kiện phân quyền khác.
5. Khách phải chọn mật khẩu mới đáp ứng chính sách mật khẩu hiện có của BMT. Sau khi đổi thành công, mật khẩu mới dùng như mật khẩu thông thường.
6. Giữ quy tắc thu hồi phiên hiện có ở BR-AUTH-002/Then khoản 9: đổi hoặc đặt lại mật khẩu thành công hủy các phiên BMT của tài khoản trên web và app, gồm cả phiên cấp qua Google. Khách đăng nhập lại bằng Google hoặc mật khẩu mới. Liên kết Google vẫn còn.
7. Nếu gửi thư thất bại, thư chưa đến hoặc khách mất mật khẩu tạm, dùng Quên mật khẩu để xác minh email và tự đặt mật khẩu mới. Không cần chức năng xem lại hoặc gửi lại mật khẩu tạm từ hồ sơ.
8. Không trả mật khẩu tạm trong kết quả đăng nhập, không ghi mật khẩu, nội dung thư có mật khẩu, token hoặc mã xác minh vào log. Dữ liệu dùng để kiểm mật khẩu phải là bản băm; việc bảo vệ nội dung thư chờ gửi thuộc TDD.

## Except

Tài khoản đã xác minh email trước khi liên kết Google giữ nguyên mật khẩu theo BR-AUTH-004/Then khoản 3. Quy tắc này không thay đổi quy trình cấp mật khẩu cho nhân viên ở BR-RBAC-006.

## Notes

- Người dùng đã chốt rõ việc không đặt hạn theo thời gian sau khi được giải thích rủi ro của mật khẩu còn nằm trong email. Không tự đổi thành hạn 24 giờ hoặc chuyển sang gửi liên kết đặt mật khẩu.
- Khái niệm mật khẩu tạm ở đây là giới hạn quyền khi đăng nhập bằng mật khẩu đó, không phải mật khẩu tự hết hạn hoặc mất hiệu lực ngay khi nhập đúng lần đầu. Giới hạn kết thúc khi đổi hoặc đặt lại mật khẩu thành công.
- Điểm thiết kế đã phát hiện: `AccessClaimsBuilder` hiện đưa `MustChangePassword` từ tài khoản vào mọi phiên; `AuthSessionIssuer` dựng lại thông tin đó khi làm mới. Cần phân biệt cách đã xác thực phiên để đáp ứng khoản 3 và khoản 4 mà vẫn giữ quy tắc nhân viên hiện có.
- BMT hiện gửi email qua hàng đợi. Nếu dùng nguyên nội dung `SendEmailEvent.Body` cho mật khẩu tạm, bản rõ có thể xuất hiện trong dữ liệu outbox, hàng đợi hoặc hàng đợi lỗi. TDD phải xử lý bảo vệ và vòng đời dữ liệu này; không được mô tả rằng toàn hệ thống chỉ giữ bản băm khi nội dung thư vẫn chứa mật khẩu.
- Người dùng đã chọn Quên mật khẩu làm cách xử lý khi không nhận được mật khẩu tạm. Không bổ sung chức năng gửi lại mật khẩu tạm cho khách. Cách vận hành hàng đợi và xử lý lần gửi đang chờ thuộc TDD; đây không phải yêu cầu bỏ cơ chế thử lại kỹ thuật đang có.
- Người dùng đã chốt nội dung quy tắc này trong bộ US/BR đăng nhập Google ngày 30/09/2026. Owner và ngày hiệu lực còn chờ xác nhận. Tân Trần là Reviewer và Approver; chưa có thao tác phê duyệt hoặc nhập tài liệu lên hệ thống.
