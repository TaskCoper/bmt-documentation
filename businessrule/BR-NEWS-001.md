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

# BR-NEWS-001

## Rule Info

- **Name**: Quản lý nội dung và vòng đời tin tức
- **Category**: Tin tức
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị nghiệp vụ Tin tức của Cẩm nang BMT.

## Statement

Người có quyền quản lý Tin tức được quản lý bài viết và danh mục, không cần phân công hoặc người duyệt công bố.

## When

Người quản lý tạo, lưu, sửa, công bố, ẩn hoặc xóa bài viết.

## Then

1. Dùng chung quyền quản lý tin tức theo STORY-RBAC-001 cho bài viết và danh mục. Người không có quyền không được thực hiện thao tác quản lý.
2. Bài có các trạng thái Nháp, Công bố và Ẩn. Cho lưu nháp thiếu thông tin; chỉ cho công bố khi có tiêu đề, ảnh đại diện, số phút đọc, nội dung rich text và ít nhất một danh mục hợp lệ.
3. Rich text hỗ trợ định dạng chữ, tiêu đề đoạn, danh sách, liên kết và chèn nhiều ảnh. FE tải ảnh lên cloud trước rồi chèn URL ảnh vào nội dung lưu. Đợt này không có video hoặc tệp đính kèm.
4. Một bài được gắn nhiều danh mục, không có danh mục chính. Được chọn danh mục tại bất kỳ cấp nào; chọn con không bắt buộc gắn thêm cha.
5. Sửa bài đã công bố cập nhật ngay bài đang hiển thị, không tạo phiên bản riêng hoặc lịch sử xem. Nội dung sau sửa vẫn phải đáp ứng điều kiện công bố.
6. Ẩn bài làm bài không xuất hiện trong danh sách công khai và không đọc được qua đường dẫn trực tiếp. Bài đã ẩn có thể công bố lại khi đủ dữ liệu.
7. Cho xóa bài ở mọi trạng thái. Sau xóa, bài không còn trong danh sách và đường dẫn cũ báo không tìm thấy. Không có thùng rác hoặc khôi phục trong phạm vi này.
8. Ghi ngày công bố đầu tiên khi bài được công bố lần đầu. Sửa bài, ẩn bài hoặc công bố lại không thay đổi ngày này.
9. Tiêu đề tối đa 200 ký tự, tính sau khi bỏ khoảng trắng đầu và cuối. Nội dung rich text tối đa 200.000 ký tự, tính trên nội dung đã được hệ thống làm sạch. Giới hạn áp dụng cho mọi lần lưu, kể cả bản nháp. Vượt giới hạn thì từ chối lưu và giữ nguyên bài hiện tại, không tự cắt ngắn.

10. Bỏ trường mô tả ngắn. Số phút đọc do người viết nhập, phải là số nguyên lớn hơn 0 nếu có giá trị; không tự tính từ nội dung. Được để trống khi lưu nháp hoặc sửa bài ẩn; bắt buộc khi công bố và khi lưu sửa bài đang công bố.

## Except

Bản nháp được thiếu các trường bắt buộc khi công bố; không được cung cấp công khai. Bài cũ có trước thay đổi ngày 29/09/2026 giữ số phút đọc trống, không gán mặc định. Bài cũ đang công bố vẫn đọc được; lần lưu sửa tiếp theo phải bổ sung số phút đọc.

## Notes

Không áp dụng cơ chế phiên bản và lượt xem của thư viện mẫu. Xóa bài không đồng nghĩa đã chốt chính sách xóa object trên cloud; phần lưu trữ sẽ được làm rõ khi thiết kế kỹ thuật.

Giới hạn độ dài ở khoản 9 do người dùng xác nhận ngày 26/09/2026. Ký tự được đếm như các module khác: mỗi ký tự Unicode tính là một, kể cả chữ có dấu hoặc ký tự đặc biệt.

Quyết định ngày 29/09/2026: người dùng yêu cầu bỏ mô tả ngắn, thêm số phút đọc nhập tay và xác nhận quy tắc tại khoản 10 cùng cách xử lý bài cũ trong Except.

Owner và ngày hiệu lực chưa xác định. Người dùng đã chốt bộ US/BR Tin tức trong hội thoại. Status Draft vẫn giữ theo quy trình tài liệu; xác nhận này không thay cho phê duyệt trên hệ thống hoặc kết quả kiểm thử.
