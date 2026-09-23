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

# BR-RBAC-006

## Rule Info

- **Name**: Người quản trị tạo tài khoản nhân viên kèm mật khẩu sinh tự động, nhân viên phải đổi ở lần đăng nhập đầu.
- **Category**: Quản lý người dùng và phân quyền
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định người dùng xác nhận trong hội thoại thiết kế RBAC ngày 20/09/2026: bỏ luồng mời qua email; Admin tạo tài khoản và chuyển mật khẩu cho nhân viên bằng kênh riêng; hệ thống sinh mật khẩu và hiển thị một lần; nhân viên bắt buộc đổi mật khẩu ở lần đăng nhập đầu.

## Statement

Tài khoản nhân viên chỉ được tạo bởi người có quyền `user.manage`. Hệ thống sinh mật khẩu ngẫu nhiên đủ mạnh, hiển thị đúng một lần cho người tạo và chỉ lưu bản băm. Người quản trị chuyển mật khẩu cho nhân viên bằng kênh riêng ngoài hệ thống. Ở lần đăng nhập đầu, nhân viên đăng nhập được nhưng phải đổi mật khẩu trước khi dùng bất kỳ chức năng nào khác.

## When

Người có quyền `user.manage` tạo một tài khoản nhân viên, hoặc một tài khoản nhân viên chưa từng đổi mật khẩu gửi yêu cầu tới hệ thống.

## Then

1. Kiểm email chưa thuộc tài khoản nào theo [BR-RBAC-005](BR-RBAC-005.md), và các vai trò được chọn đều là vai trò nhân viên.
2. Kiểm rào chắn cấp quyền theo [BR-RBAC-004](BR-RBAC-004.md) trước khi tạo.
3. Sinh mật khẩu ngẫu nhiên đủ mạnh, lưu bản băm, đánh dấu tài khoản phải đổi mật khẩu và đặt tài khoản ở trạng thái đang hoạt động.
4. Trả mật khẩu đúng một lần trong phản hồi tạo tài khoản. Không lưu bản rõ, không gửi email và không có chức năng xem lại mật khẩu đã sinh.
5. Nhân viên đăng nhập được bằng mật khẩu đó. Trong lúc tài khoản còn bị đánh dấu phải đổi mật khẩu, hệ thống từ chối mọi yêu cầu khác ngoài yêu cầu đổi mật khẩu, kể cả khi tài khoản đã được gán vai trò có quyền.
6. Khi nhân viên đổi mật khẩu thành công, bỏ đánh dấu phải đổi mật khẩu và hủy toàn bộ phiên đang mở của tài khoản đó. Nhân viên đăng nhập lại bằng mật khẩu mới.
7. Ghi nhật ký việc tạo tài khoản và thời điểm nhân viên đổi mật khẩu lần đầu theo [BR-RBAC-012](BR-RBAC-012.md). Không ghi mật khẩu hoặc bản băm vào nhật ký.

## Except

Không có đường nào xem lại mật khẩu đã sinh, kể cả với người tạo ra tài khoản. Nhân viên mất mật khẩu trước khi kịp đổi thì dùng luồng quên mật khẩu sẵn có của hệ thống; luồng đó gửi mã qua email nên chỉ dùng được khi nhân viên truy cập được hộp thư đã đăng ký.

## Notes

- Không có đường đăng ký công khai nào tạo ra tài khoản nhân viên, và không có cách nâng tài khoản khách hàng đã có thành tài khoản nhân viên.
- Việc chuyển mật khẩu cho nhân viên diễn ra ngoài hệ thống, nên hệ thống không bảo đảm được kênh đó an toàn. Đây là lý do bắt buộc đổi mật khẩu ở lần đăng nhập đầu: sau bước đó, người quản trị không còn biết mật khẩu của nhân viên, nên một thao tác ghi trong nhật ký quy được trách nhiệm cho chính nhân viên đó.
- Việc hủy phiên ở điểm 6 là cần thiết vì người quản trị từng biết mật khẩu ban đầu. Không hủy thì một phiên mở bằng mật khẩu cũ vẫn sống tiếp sau khi nhân viên đã đổi.
- Email vẫn là định danh đăng nhập và phải là duy nhất. Vì tài khoản được bàn giao trực tiếp chứ không qua email, hệ thống đánh dấu email đã xác thực ngay khi tạo; nhân viên không phải đi thêm một vòng xác thực email.
- **Điểm còn mở**: nếu người quản trị nhập một địa chỉ email mà nhân viên không truy cập được, nhân viên mất mật khẩu sẽ không tự khôi phục được và cũng chưa có chức năng đặt lại mật khẩu cho người khác. Cần chốt có bổ sung chức năng đó không.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
