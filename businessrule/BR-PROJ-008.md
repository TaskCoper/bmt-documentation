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

# BR-PROJ-008

## Rule Info

- **Name**: Danh sách dự toán của tôi, tìm theo tên và lọc trạng thái.
- **Category**: Dự toán — danh sách cá nhân
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại: hiển thị đủ trạng thái; tên, loại công trình, trạng thái, ngày tạo, lần sửa gần nhất là đủ, không có ảnh đại diện; đồng ý phân trang, tìm theo tên, lọc trạng thái và bản sửa gần nhất đứng trước. Quyền xem dữ liệu cũ dùng lại BR-SUB-007.

## Statement

Dự toán của tôi chỉ hiển thị các bản chưa bị xóa thuộc tài khoản khách hàng đang đăng nhập. Danh sách gồm bản nháp, đang xử lý, thành công và thất bại; có phân trang, tìm theo tên và lọc theo trạng thái. Khách không cần gói còn hiệu lực hoặc còn lượt chỉ để xem dữ liệu của mình.

## When

Khách mở Dự toán của tôi, đổi trang, tìm theo tên, lọc trạng thái hoặc chọn mở một bản từ danh sách.

## Then

1. Xác định chủ sở hữu từ tài khoản đăng nhập. Không trả dự toán của khách khác, kể cả thông qua tìm kiếm, bộ lọc hoặc thông tin phân trang. Loại tài khoản áp dụng BR-RBAC-005.
2. Khi không lọc trạng thái, tập kết quả gồm đủ nháp, đang xử lý, thành công và thất bại. Không yêu cầu đã chọn loại công trình hoặc có kết quả AI mới được xuất hiện.
3. Mỗi bản hiển thị tên, loại công trình nếu đã chọn, trạng thái, ngày tạo và lần sửa gần nhất. Không hiển thị ảnh đại diện hoặc cột địa chỉ. Bản nháp chưa chọn loại phải thể hiện đúng tình trạng chưa chọn, không tự gán loại khác.
4. Mặc định sắp xếp theo lần sửa gần nhất, mới nhất trước. Áp dụng thứ tự trên tập kết quả đã lọc rồi mới chia trang.
5. Hỗ trợ tìm một phần tên không phân biệt hoa/thường hoặc dấu, kết hợp đồng thời với lọc trạng thái; ví dụ “nha” tìm được “Nhà”. Mọi kết quả vẫn phải thuộc khách, chưa bị xóa và đáp ứng các điều kiện đang áp dụng.
6. Không có bản phù hợp thì trả danh sách rỗng; không bổ sung dữ liệu mẫu hoặc dữ liệu của tài khoản khác. Lỗi tải dữ liệu phải được thể hiện là lỗi, không coi là danh sách rỗng đã tải thành công.
7. Cho phép xem danh sách khi gói hết hạn hoặc hết lượt theo quyền xem dữ liệu cũ tại BR-SUB-007. Xem danh sách không giữ, trừ, hoàn lượt hoặc gọi AI; quyền sửa và gửi AI được kiểm tra riêng.
8. Chọn một bản để mở phải kiểm lại quyền và tình trạng đã xóa. Hiển thị dữ liệu đã lưu, không tự gửi hoặc thử lại AI.
9. Bản đã xóa theo BR-PROJ-009 không xuất hiện trong danh sách, tìm kiếm, bộ lọc hoặc kết quả phân trang; đường dẫn cũ không mở lại được bản đó.

## Except

Người chưa có phiên hợp lệ hoặc dùng tài khoản nhân viên không được dùng danh sách cá nhân này. Quyền xem qua link chia sẻ không cấp quyền liệt kê dự toán của chủ hồ sơ. Quyền xem dữ liệu cũ khi hết hạn gói không áp dụng cho dự toán đã bị xóa.

## Notes

- Áp dụng cho STORY-PROJ-006; thao tác xóa từ danh sách theo STORY-PROJ-007 và BR-PROJ-009.
- “Dự án” trong các câu trả lời về hai tính năng này được hiểu là bản dự toán thuộc nhóm PROJ, không phải thực thể Công trình của nhóm SITE.
- Trạng thái danh sách phải thống nhất với trạng thái bản dự toán đang dùng. Code hiện tại gộp tác vụ thất bại hoặc quá thời gian vào trạng thái dự toán thất bại; không tạo một vòng đời riêng cho danh sách.
- Người dùng xác nhận tìm không phân biệt hoa/thường và dấu ngày 30/09/2026. Kích thước trang, thứ tự phụ khi trùng mốc sửa và cách chuẩn hóa được thiết kế trong TDD-PROJ-004; không đổi tên đã lưu hoặc tên hiển thị.
- Reviewer và Approver đều là Tân Trần theo xác nhận trong hội thoại. Owner và ngày hiệu lực chưa được cung cấp. Người dùng đã chốt quy tắc cùng User Story trong hội thoại ngày 30/09/2026. Đặc tả ST xem [bảng độ phủ](../discovery/my-estimates-system-test-coverage.md); chưa thực thi. Việc chốt trong hội thoại không thay thế phê duyệt trên hệ thống quản lý tài liệu.
