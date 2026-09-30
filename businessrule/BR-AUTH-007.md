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

# BR-AUTH-007

## Rule Info

- **Name**: Không cho đổi email khi tự sửa hồ sơ; các thông tin hồ sơ khác vẫn sửa được.
- **Category**: Xác thực và hồ sơ tài khoản
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại đăng nhập Google ngày 30/09/2026: sau khi được giải thích chức năng sửa hồ sơ đang đổi email mà không xác minh lại, người dùng chọn không cho đổi email và nhắc lại “Không được đổi mail”; các thông tin hồ sơ khác vẫn sửa được.

## Statement

Chức năng tự sửa hồ sơ không cho tài khoản đổi địa chỉ email. Người dùng vẫn được sửa các thông tin hồ sơ khác theo quyền và kiểm tra hiện có. Quy tắc được thực thi ở backend và thể hiện trên giao diện web, app.

## When

Người dùng xem hoặc cập nhật hồ sơ của chính mình trên web, app hoặc gọi trực tiếp chức năng cập nhật hồ sơ của backend.

## Then

1. Email hiện tại không được sửa qua chức năng cập nhật hồ sơ. Giao diện thể hiện email là thông tin không chỉnh sửa trong chức năng này.
2. Backend từ chối yêu cầu thay email, kể cả yêu cầu gọi trực tiếp không qua giao diện. Không đổi email và không ghi một phần các thông tin khác trong cùng yêu cầu bị từ chối.
3. Yêu cầu giữ nguyên email vẫn được cập nhật tên, ảnh, số điện thoại, địa chỉ và các thông tin khác mà chức năng hiện tại cho sửa, nếu đủ quyền và dữ liệu hợp lệ.
4. Không sửa trạng thái xác minh email, mật khẩu hoặc liên kết Google chỉ vì khách cập nhật hồ sơ thông thường.
5. Không coi việc chặn đổi email từ nay là bằng chứng email trong dữ liệu cũ đã được xác minh đúng. Việc tự liên kết Google với tài khoản cũ phải đáp ứng BR-AUTH-004.

## Except

Không có ngoại lệ đổi email qua chức năng tự sửa hồ sơ. Không bổ sung luồng đổi email có xác minh riêng trong tính năng này.

## Notes

- Hiện trạng đã kiểm tra: `PUT /api/v1/users/me` gọi `UpdateUserProfileCommandHandler`; handler kiểm trùng email rồi gán `User.Email`, không xác minh email mới hoặc xóa trạng thái đã xác minh. Đây là lý do cần thay đổi khi thêm tự liên kết Google.
- Thay đổi áp dụng cho chức năng tự sửa hồ sơ đang dùng chung, không chỉ những tài khoản đã liên kết Google. TDD cần giữ tương thích cho client vẫn gửi email không đổi trong yêu cầu cập nhật.
- Quy tắc từ chối toàn bộ yêu cầu thay email là cách cụ thể hóa để người dùng biết thao tác thất bại; mã lỗi và cách so sánh email thuộc TDD.
- Owner và ngày hiệu lực chưa xác định. Tân Trần là Reviewer và Approver theo xác nhận trong hội thoại; việc ghi tên không có nghĩa bộ tài liệu đã được phê duyệt.
- Người dùng đã chốt nội dung quy tắc này trong bộ US/BR đăng nhập Google ngày 30/09/2026. Chưa sửa code hoặc chạy kiểm thử.
