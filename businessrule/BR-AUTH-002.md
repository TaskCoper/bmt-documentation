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

# BR-AUTH-002

## Rule Info

- **Name**: Phiên đăng nhập trên app mobile dùng token trả trực tiếp cho app, hạn 30 ngày tính lại mỗi lần làm mới.
- **Category**: Xác thực và phiên đăng nhập
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Hội thoại ngày 26/09/2026: app mobile không dùng cookie; app có đăng ký, xác minh email và quên mật khẩu. Thời hạn phiên, việc tự đăng nhập sau xác minh email và việc đổi mật khẩu cắt mọi phiên do Claude đề xuất; người dùng giao Claude quyết định ("Theo ý bạn").

## Statement

App giữ phiên đăng nhập bằng access token và refresh token do hệ thống trả trực tiếp trong nội dung phản hồi, không dùng cookie. Access token có hạn như web. Refresh token của phiên đăng nhập có hạn 30 ngày và được tính lại mỗi lần làm mới. Phiên kết thúc khi khách hàng đăng xuất, đổi mật khẩu, bị thu hồi phiên theo các cơ chế đang áp cho web, hoặc không làm mới trong 30 ngày.

## When

Hệ thống cấp phiên cho app (đăng nhập, xác minh email, xác minh mã quên mật khẩu), app làm mới phiên, khách hàng đăng xuất trên app, hoặc khách hàng đổi mật khẩu.

## Then

1. Khi cấp phiên, hệ thống trả access token, refresh token và thời điểm hết hạn của refresh token trong nội dung phản hồi. Hệ thống không đặt cookie phiên.
2. Mỗi lần cấp phiên tạo một phiên riêng. Phiên mới không làm mất các phiên khác của cùng tài khoản trên thiết bị khác hoặc trên web.
3. Access token của phiên mobile có hạn như access token của web, hiện là 15 phút theo cấu hình ([BR-RBAC-009](BR-RBAC-009.md)).
4. Refresh token của phiên đăng nhập hết hạn sau 30 ngày. Mỗi lần làm mới thành công, hệ thống trả cặp token mới, refresh token mới hết hạn sau 30 ngày kể từ lúc làm mới, và refresh token cũ không dùng lại được. Không có giới hạn tổng thời gian của một phiên.
5. Xác minh email thành công trên app thì khách hàng được cấp phiên đăng nhập như khoản 1 và khoản 4, không phải đăng nhập lại.
6. Phiên quên mật khẩu cấp cho app giữ thời hạn như phiên quên mật khẩu trên web, không được hạn 30 ngày. Như trên web, phiên này dùng để đặt mật khẩu mới và bị từ chối ở các chức năng thông thường.
7. Khi làm mới phiên, hệ thống chỉ nhận refresh token trong nội dung yêu cầu, không đọc cookie. Refresh token sai, hết hạn, đã dùng rồi hoặc thuộc phiên đã bị thu hồi thì hệ thống trả 401 `InvalidRefreshToken`.
8. Khi khách hàng đăng xuất, app gửi refresh token của phiên và hệ thống thu hồi đúng phiên đó. Thiết bị khác và web không bị đăng xuất.
9. Khi khách hàng đổi mật khẩu thành công, trên app hay trên web, kể cả qua luồng quên mật khẩu, hệ thống cắt mọi phiên của tài khoản, gồm cả phiên trên app đang dùng. Khách hàng phải đăng nhập lại.
10. Mọi cơ chế thu hồi phiên đang áp cho web cũng áp cho phiên mobile.
11. Web giữ nguyên cách dùng cookie: các chức năng cấp phiên của web không trả token trong nội dung phản hồi.

## Except

Sau khi đăng xuất, access token đã cấp cho phiên đó vẫn dùng được tới khi hết hạn, tối đa bằng hạn access token (hiện 15 phút), giống phiên web hiện nay. App phải xóa token khỏi máy ngay khi đăng xuất. Trường hợp đổi mật khẩu không có độ trễ này, vì theo khoản 9 access token cũ bị từ chối ngay.

## Notes

- Ví dụ: khách hàng đăng nhập ngày 01/10, mở app lần cuối và làm mới phiên ngày 20/10. Refresh token mới hết hạn ngày 19/11. Nếu khách hàng không mở app tới sau ngày 19/11, lần làm mới tiếp theo bị từ chối và khách hàng phải đăng nhập lại.
- Lý do chọn 30 ngày: khách hàng mở app ít nhất một lần mỗi tháng thì không phải đăng nhập lại. Rủi ro khi mất điện thoại được giảm nhờ refresh token đổi mới sau mỗi lần dùng, và nhờ khách hàng đổi mật khẩu trên web để cắt mọi phiên.
- Không cho chức năng cấp phiên của web trả token trong nội dung phản hồi (khoản 11), vì nếu có một đoạn script lạ chạy trên trang web, nó có thể đổi cookie HttpOnly thành refresh token đọc được và mang đi dùng nơi khác.
- Thời hạn 30 ngày là giá trị ban đầu và phải đổi được bằng cấu hình. Cách lưu phiên (ví dụ bảng AuthSession đang đề xuất trong TDD-PUSH-001) thuộc TDD.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
