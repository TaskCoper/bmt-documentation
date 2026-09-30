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

# BR-AUTH-003

## Rule Info

- **Name**: Khách hàng đăng nhập Google trên web và app; tự tạo tài khoản khi chưa có.
- **Category**: Xác thực và đăng nhập Google
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại chuẩn bị đăng nhập Google ngày 30/09/2026: người dùng chọn cả web và app, chỉ khách hàng, tự tạo tài khoản mới; chọn tự liên kết khi Google đủ căn cứ xác nhận chủ email và xác minh bổ sung ở trường hợp còn lại.

## Statement

BMT cho khách hàng đăng nhập bằng Google trên web và app mobile. Sau khi xác thực Google và xác minh quyền sở hữu email, hệ thống tự tạo tài khoản khách hàng nếu chưa có, hoặc dùng tài khoản đã liên kết theo BR-AUTH-004. Luồng này không cấp quyền nhân viên.

## When

Khách chọn đăng nhập Google trên web hoặc app BMT.

## Then

1. Hệ thống phải xác thực kết quả do Google cấp trước khi tin danh tính hoặc email. Dữ liệu email, tên và ảnh do client tự khai không thay thế kết quả đã xác thực.
2. Khi chưa có liên kết Google, hệ thống phải xác minh quyền sở hữu email trước khi tạo tài khoản hoặc liên kết với tài khoản BMT. Với Gmail hoặc Google Workspace đủ điều kiện theo Notes, không yêu cầu khách nhập thêm mã email của BMT. Các trường hợp khác phải hoàn thành bước xác minh bổ sung; chưa hoàn thành thì chưa tạo tài khoản, chưa liên kết và chưa cấp phiên sử dụng thông thường.
3. Nếu email chưa thuộc tài khoản BMT nào, hệ thống tự tạo tài khoản đang hoạt động, thuộc nhóm khách hàng, nhận vai trò Khách hàng theo BR-RBAC-005/Then. Email được ghi nhận đã xác minh sau khi đáp ứng khoản 2.
4. Tài khoản mới nhận mật khẩu tạm theo BR-AUTH-005 và thông tin hồ sơ theo BR-AUTH-006. Khách không phải đặt mật khẩu hoặc nhập số điện thoại, địa chỉ để hoàn tất đăng nhập Google.
5. Nếu email đã có tài khoản, xử lý theo BR-AUTH-004; không tạo tài khoản thứ hai để tránh bước liên kết.
6. Nếu tài khoản đích thuộc nhóm nhân viên, bị khóa hoặc đã xóa, không cấp phiên Google, không chuyển thành khách hàng và không tự mở khóa hay khôi phục tài khoản.
7. Đăng nhập Google thành công cấp phiên BMT theo đúng nền tảng đang dùng: web giữ phiên qua cookie; app nhận token trực tiếp theo BR-AUTH-002. Đăng nhập Google không tự cấp thêm quyền hoặc quyền lợi ngoài vai trò và dữ liệu của tài khoản.
8. Khách hủy bước Google hoặc kết quả xác thực không hợp lệ thì không tạo tài khoản, không liên kết và không cấp phiên.

## Except

Tài khoản đã liên kết Google được nhận diện theo liên kết đó ở các lần đăng nhập sau, theo BR-AUTH-004; không tạo lại tài khoản chỉ vì thông tin Google thay đổi.

## Notes

- Nguồn kỹ thuật: [Google — Verify the Google ID token](https://developers.google.com/identity/gsi/web/guides/verify-google-id-token). Google xác nhận quyền sở hữu email đối với Gmail; với Google Workspace cần email đã xác minh và thông tin miền do Google cấp (`hd`). Tài khoản Google dùng email của nhà cung cấp khác không đủ căn cứ chỉ nhờ `email_verified=true`; cần xác minh bổ sung. TDD phải quy định cách kiểm các điều kiện này và cách xác minh bổ sung.
- Phạm vi nền tảng và loại tài khoản đã được người dùng xác nhận. Các bước từ chối, bảo vệ phiên và cách áp điều kiện Google là phần cụ thể hóa cần được review trong bộ US/BR.
- Người dùng đã chốt nội dung quy tắc này cùng STORY-AUTH-002 và BR-AUTH-004 đến BR-AUTH-007 trong hội thoại ngày 30/09/2026. System Test được soạn từ bộ nghiệp vụ đã chốt; TDD-AUTH-003 cũng đã được chốt trong hội thoại ngày 30/09/2026. Status vẫn là Draft; chưa triển khai. Owner và ngày hiệu lực còn chờ xác nhận. Tân Trần là Reviewer và Approver; chưa có thao tác phê duyệt hoặc nhập tài liệu lên hệ thống.
