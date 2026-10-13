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

# BR-PROJ-005

## Rule Info

- **Name**: Đầu vào bị khóa từ lúc tiếp nhận chặng 2D; mở lại khi 2D lỗi và khóa vĩnh viễn khi 2D thành công.
- **Category**: Tạo dự toán — xử lý theo chặng
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: biểu mẫu bị khóa từ lúc bấm tạo 2D; 2D lỗi thì mở lại cho khách sửa; 2D thành công thì khóa vĩnh viễn; tên bản dự toán vẫn đổi được như quy tắc hiện có. Thay quy tắc trước “khóa khi AI xử lý, mở khi thất bại” của luồng một lần gửi AI.

## Statement

Đầu vào thiết kế của bản dự toán bị khóa sửa từ lúc hệ thống tiếp nhận lần chạy chặng 2D. Nếu lần chạy 2D lỗi, đầu vào được mở lại để khách sửa hoặc chạy lại. Khi chặng 2D thành công, đầu vào bị khóa vĩnh viễn, kể cả khi các chặng sau lỗi. Tên bản dự toán không thuộc phạm vi khóa.

## When

Khách lưu thay đổi đầu vào (kể cả tự lưu, thay ảnh lô, yêu cầu soi ảnh), hệ thống tiếp nhận hoặc kết thúc lần chạy 2D, hoặc khách đổi tên bản dự toán.

## Then

1. Khóa đầu vào khi tiếp nhận hợp lệ lần chạy 2D. Lần chạy dùng đúng đầu vào đã chốt lúc tiếp nhận.
2. Từ chối mọi thay đổi đầu vào trong lúc lần chạy 2D chưa kết thúc, kể cả yêu cầu gửi trực tiếp và tự lưu đến muộn từ biểu mẫu đã mở trước đó. Không chỉ khóa các ô trên giao diện.
3. Khi lần chạy 2D lỗi, kể cả lỗi do quá hạn theo BR-PROJ-012, mở lại khả năng sửa đầu vào. Quyền tạo/lưu và điều kiện gói/lượt vẫn phải đáp ứng BR-SUB-007.
4. Khách được chạy lại 2D trên cùng bản dự toán, có hoặc không sửa đầu vào, theo BR-PROJ-011. Lần chạy lại được tiếp nhận thì khóa lại như khoản 1.
5. Khi chặng 2D thành công, đầu vào bị khóa vĩnh viễn. Dự toán, Phối cảnh 3D và Hồ sơ lỗi hay chạy lại đều không mở khóa. Muốn đổi đầu vào, khách tạo bản dự toán mới.
6. Kết quả đến muộn của lần chạy 2D đã lỗi do quá hạn không mở lại thành công, không gắn vào lần chạy lại và không được công bố.
7. Thay ảnh lô và yêu cầu soi ảnh là thay đổi đầu vào, nên cũng bị khóa theo các khoản trên.

## Except

Đổi tên bản dự toán không bị khóa theo BR-SUB-007 khoản 11, kể cả khi chặng đang chạy hoặc sau khi 2D thành công. Yêu cầu chạy 2D bị từ chối trước khi tiếp nhận không khóa đầu vào. Việc không được lưu do hết hạn gói, thiếu quyền hoặc hết lượt vẫn áp dụng độc lập theo BR-SUB-007.

## Notes

- **Đã xác nhận (11/10/2026):** khóa từ lúc tạo 2D; 2D lỗi thì mở; 2D thành công thì khóa vĩnh viễn; tên vẫn đổi được.
- **Đã xác nhận (11/10/2026):** thay ảnh lô và soi ảnh thuộc phạm vi khóa (khoản 7), vì kết quả soi là đầu vào của chặng 2D.
- Ví dụ: 2D thành công, Dự toán lỗi 3 lần. Khách không sửa được số phòng ngủ để thử lại; khách liên hệ hỗ trợ để Admin đặt lại bộ đếm theo BR-PROJ-011, hoặc tạo bản dự toán mới.
- Không thêm quyền khách hủy lần chạy.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
