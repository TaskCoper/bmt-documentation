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

# BR-AUTH-004

## Rule Info

- **Name**: Tự liên kết Google với tài khoản cùng email và bảo vệ tài khoản chưa xác minh.
- **Category**: Xác thực và đăng nhập Google
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại ngày 30/09/2026: người dùng chọn tự liên kết theo email; đồng ý vô hiệu mật khẩu cũ, hủy phiên cũ và gửi mật khẩu tạm mới nếu tài khoản BMT chưa xác minh email.

## Statement

Khi quyền sở hữu email đã được xác minh theo BR-AUTH-003, hệ thống tự liên kết Google với tài khoản khách hàng cùng email. Tài khoản đã xác minh đúng email hiện tại giữ mật khẩu; tài khoản chưa xác minh phải vô hiệu mật khẩu và phiên cũ trước khi dùng Google.

## When

Khách xác thực Google thành công và hệ thống cần xác định tài khoản BMT tương ứng.

## Then

1. Nếu tài khoản Google đã có liên kết, đăng nhập vào đúng tài khoản BMT được liên kết, sau khi kiểm trạng thái và loại tài khoản theo BR-AUTH-003. Không tự chuyển liên kết sang người khác chỉ vì email hoặc tên thay đổi.
2. Nếu chưa có liên kết và email đã xác minh thuộc một tài khoản khách hàng BMT, dùng chính tài khoản đó. Giữ mã tài khoản, gói, quyền lợi và dữ liệu đang thuộc khách; không tạo bản sao hoặc tài khoản thứ hai.
3. Nếu tài khoản BMT đã xác minh đúng email hiện tại, giữ mật khẩu đang có. Khách có thể tiếp tục đăng nhập bằng mật khẩu hoặc Google; lần liên kết này không sinh mật khẩu tạm mới.
4. Nếu tài khoản BMT chưa xác minh email, chỉ sau khi xác minh chủ email theo BR-AUTH-003 mới liên kết. Vô hiệu mật khẩu cũ, hủy toàn bộ phiên cũ, đánh dấu email đã xác minh và sinh mật khẩu tạm mới gửi đến email đã xác minh theo BR-AUTH-005. Sau đó cấp phiên Google mới.
5. Các mã xác minh hoặc khôi phục có trước việc tiếp nhận tài khoản ở khoản 4 không được dùng để khôi phục quyền truy cập cũ. Cách vô hiệu hóa thuộc thiết kế kỹ thuật.
6. Nếu liên kết hiện có mâu thuẫn với tài khoản đích, không tự gộp tài khoản hoặc ghi đè liên kết. Không thay đổi dữ liệu và không cấp phiên cho tài khoản khác.
7. Đăng nhập lại bằng cùng Google không sinh lại mật khẩu tạm và không gửi lại thư cấp mật khẩu chỉ vì khách đăng nhập lại.
8. Không tự ghi đè hồ sơ khách hàng khi liên kết hoặc đăng nhập lại, theo BR-AUTH-006.

## Except

Tài khoản nhân viên, tài khoản bị khóa hoặc đã xóa bị từ chối theo BR-AUTH-003, kể cả khi email Google trùng khớp. Ngoại lệ thay mật khẩu ở khoản 4 chỉ áp dụng cho tài khoản khách hàng chưa xác minh; không dùng để mở khóa hoặc khôi phục tài khoản đã xóa.

## Notes

- Ví dụ minh họa: một người đăng ký bằng email của khách nhưng chưa xác minh. Khi chủ email thật đăng nhập Google, giữ mật khẩu do người đăng ký trước đặt sẽ để lại một đường truy cập. Vì vậy người dùng đã đồng ý thay mật khẩu và hủy các phiên cũ trong nhánh này.
- Phát hiện khi khảo sát: `UpdateUserProfileCommandHandler` đang đổi `User.Email` nhưng không xóa hoặc xác minh lại `IsEmailVerified`. Vì vậy cờ này có thể không chứng minh quyền sở hữu email hiện tại. Người dùng đã chốt không cho đổi email khi tự sửa hồ sơ, theo BR-AUTH-007; các thông tin khác vẫn sửa được.
- Người dùng xác nhận hiện chỉ có tài khoản thử nghiệm, chưa có tài khoản khách hàng thật. Vì vậy chưa cần quy trình chuyển đổi tài khoản khách hàng thật từng đổi email. Dữ liệu thử vẫn phải được phân loại theo bằng chứng xác minh khi kiểm chứng; câu trả lời này không cho phép tự xóa hoặc đặt lại dữ liệu đang có.
- Đối chiếu thêm mã nguồn: nhánh đổi mật khẩu bằng mật khẩu hiện tại trong `ChangePasswordCommandHandler` đang đặt `IsEmailVerified=true` dù không kiểm mã email. Do đó chỉ chặn đổi email chưa đủ để bảo đảm khoản 3. TDD phải bảo đảm việc xác minh gắn với đúng địa chỉ email và có bằng chứng thực tế; biết mật khẩu hoặc đổi mật khẩu thông thường không tự chứng minh quyền sở hữu email. Đây là điều kiện kỹ thuật để thực hiện quy tắc đã nêu, không phải một luồng đổi email mới.
- Nguồn kỹ thuật: [Google — Verify the Google ID token](https://developers.google.com/identity/gsi/web/guides/verify-google-id-token) yêu cầu dùng mã tài khoản Google ổn định (`sub`) để nhận diện, không dùng email làm mã liên kết lâu dài.
- Người dùng đã chốt cả bộ US/BR, gồm khoản 5 và khoản 6. Phạm vi chỉ gồm đăng nhập và liên kết Google phục vụ luồng này; chức năng quản lý nhiều tài khoản Google, đổi hoặc gỡ liên kết nằm ngoài STORY-AUTH-002. TDD cần làm rõ ràng buộc dữ liệu và cách phát hiện liên kết mâu thuẫn.
- Người dùng đã chốt nội dung quy tắc này trong bộ US/BR đăng nhập Google ngày 30/09/2026. Owner và ngày hiệu lực còn chờ xác nhận. Tân Trần là Reviewer và Approver; chưa có thao tác phê duyệt hoặc nhập tài liệu lên hệ thống.
