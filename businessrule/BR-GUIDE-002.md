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

# BR-GUIDE-002

## Rule Info

- **Name**: Nội dung hướng dẫn và thông tin video YouTube
- **Category**: Video hướng dẫn
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị tính năng video hướng dẫn BMT ngày 30/09/2026.

## Statement

Mỗi hướng dẫn có một bản nội dung tiếng Việt gồm tiêu đề, mô tả ngắn và link tới một video lưu trên YouTube. Hệ thống tự lấy ảnh đại diện, thời lượng và kiểm tra video trước khi xuất bản.

## When

Người quản lý gán video, yêu cầu xuất bản hoặc người dùng phát video của hướng dẫn đang hiển thị.

## Then

1. Không chia hướng dẫn theo danh mục và không có nội dung bài viết kèm theo trong đợt này.
2. Sau khi người quản lý dán link YouTube, hệ thống lấy ảnh đại diện và thời lượng của video. Hai thông tin này không yêu cầu người quản lý nhập tay.
3. Tiêu đề và mô tả ngắn là nội dung người quản lý nhập cho hướng dẫn. Video được lưu và phát từ YouTube; BMT không nhận tệp video tải lên trong tính năng này.
4. Khi yêu cầu xuất bản, nếu không kiểm tra được video do YouTube lỗi, video không tồn tại hoặc không cho phát trên website, hệ thống báo lỗi và giữ nguyên trạng thái hướng dẫn.
5. Khi hướng dẫn đã xuất bản nhưng video không phát được, hệ thống báo cho người xem. Không tự ẩn hướng dẫn chỉ vì lỗi phát; người quản lý chủ động sửa hoặc ẩn.
6. Không coi ảnh đại diện hoặc thời lượng của video cũ là thông tin của link video mới khi người quản lý thay link.
7. Khi YouTube tạm thời lỗi, vẫn cho lưu link đúng định dạng vào bản Nháp và thông báo chưa lấy hoặc kiểm tra được thông tin video. Người quản lý được thử lấy lại sau; việc xuất bản vẫn bị chặn cho đến khi kiểm tra thành công.
8. Khi sửa hướng dẫn đang xuất bản mà thay video, chỉ lưu thay đổi nếu kiểm tra video mới thành công. Nếu thất bại, giữ toàn bộ bản đang hiển thị, bao gồm link, ảnh đại diện và thời lượng cũ, theo BR-GUIDE-001.

## Except

Lưu nháp có thể thiếu thông tin bắt buộc khi xuất bản theo BR-GUIDE-001. Kiểm tra trước xuất bản không bảo đảm video phát được mãi mãi hoặc trên mọi thiết bị, khu vực và cấu hình trình duyệt.

## Notes

Theo [tài liệu YouTube về video](https://developers.google.com/youtube/v3/docs/videos), API có thông tin ảnh đại diện, thời lượng và cờ cho phép nhúng. Cờ cho phép nhúng không bảo đảm mọi lần phát đều thành công; thiết kế cần xử lý lỗi thực tế của trình phát theo [IFrame Player API](https://developers.google.com/youtube/iframe_api_reference).

Người dùng đã xác nhận cách lưu nháp khi YouTube tạm thời lỗi và cách giữ bản đang hiển thị khi thay video chưa kiểm tra được. Giới hạn độ dài nội dung, dạng link hỗ trợ và các trường hợp video đặc biệt sẽ được làm rõ trong bước thiết kế; không tự suy ra giới hạn thời lượng từ dòng giới thiệu trên giao diện.

Reviewer và Approver là Tân Trần. Bản US/BR sau cập nhật chưa được chốt toàn văn; tên người phê duyệt không tự xác nhận tài liệu đã được duyệt. Owner và ngày hiệu lực chưa được cung cấp.
