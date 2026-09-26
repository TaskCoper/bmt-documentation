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

# BR-AUTH-001

## Rule Info

- **Name**: Chỉ tài khoản khách hàng được cấp phiên đăng nhập trên app mobile; nhân viên bị từ chối sau khi đã kiểm mật khẩu hoặc mã.
- **Category**: Xác thực và phiên đăng nhập
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại ngày 26/09/2026: người dùng chốt tài khoản nhân viên không được đăng nhập app. Cách từ chối (kiểm mật khẩu hoặc mã trước, mã lỗi và thông báo) do Claude đề xuất; người dùng giao Claude quyết định ("Theo ý bạn").

## Statement

App mobile BMT chỉ dành cho khách hàng. Mọi chức năng cấp phiên cho app (đăng nhập, xác minh email và xác minh mã quên mật khẩu) chỉ cấp phiên cho tài khoản khách hàng. Tài khoản nhân viên bị từ chối, nhưng chỉ sau khi hệ thống đã kiểm mật khẩu hoặc mã, để người ngoài không dò được email nào thuộc nhân viên.

## When

App gửi yêu cầu đăng nhập, xác minh email hoặc xác minh mã quên mật khẩu qua các chức năng cấp phiên dành cho mobile.

## Then

1. Hệ thống kiểm mật khẩu hoặc mã xác minh trước. Nếu sai, hệ thống trả đúng lỗi như khi tài khoản khách hàng nhập sai; phản hồi không cho biết loại tài khoản.
2. Nếu mật khẩu đúng nhưng tài khoản bị khóa, hệ thống trả lỗi tài khoản bị khóa như chức năng đăng nhập web.
3. Nếu mật khẩu hoặc mã đúng và tài khoản thuộc nhóm nhân viên theo [BR-RBAC-005](BR-RBAC-005.md), hệ thống trả 403 `MobileLoginNotAllowed` với thông báo "Tài khoản nhân viên chỉ dùng BMT trên web". Hệ thống không tạo phiên và không trả token.
4. Nếu mật khẩu hoặc mã đúng và tài khoản thuộc nhóm khách hàng, hệ thống cấp phiên mobile theo [BR-AUTH-002](BR-AUTH-002.md).
5. Các chức năng dùng chung với web, như đăng ký, gửi mã xác minh, gửi mã quên mật khẩu và đổi mật khẩu, không phân biệt loại tài khoản theo quy tắc này.

## Except

Không có ngoại lệ. Nhân viên muốn dùng thử app phải dùng một tài khoản khách hàng riêng, theo [BR-RBAC-005](BR-RBAC-005.md).

## Notes

- Ví dụ: nhân viên `nv@bmt.vn` đăng nhập app với mật khẩu sai thì nhận lỗi sai mật khẩu, giống hệt khách hàng. Nhập đúng mật khẩu thì nhận 403 `MobileLoginNotAllowed`. Nếu hệ thống báo 403 ngay từ khi nhận email, chỉ cần thử email là biết ai là nhân viên.
- Chức năng gửi mã quên mật khẩu dùng chung với web, nên vẫn gửi mã cho email của nhân viên. Từ chối ngay ở bước này sẽ làm lộ loại tài khoản. Nhân viên bị chặn ở bước nhập mã trên app và vẫn đặt lại mật khẩu được trên web.
- Nhóm tài khoản không đổi được sau khi tạo (BR-RBAC-005), nên phiên mobile đã cấp cho khách hàng không cần kiểm lại nhóm tài khoản khi làm mới.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
