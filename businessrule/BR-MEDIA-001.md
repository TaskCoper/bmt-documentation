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

# BR-MEDIA-001

## Rule Info

- **Name**: Cấp presigned URL và sử dụng ảnh upload
- **Category**: Quản lý ảnh
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định của người dùng trong hội thoại xây dựng presigned URL cho BMT ngày 30/09/2026; tham khảo code Taskcoper, không lấy hành vi Taskcoper làm quy tắc mặc định.

## Statement

Backend BMT cấp presigned URL qua BizFly cho mọi tài khoản đã đăng nhập, gồm quản trị viên và khách hàng. Luồng ảnh mặc định nhận JPG, PNG hoặc WebP, tối đa 5 MiB mỗi ảnh. Riêng admin tải ảnh nhà thầu tối đa 10 MiB và bản scan PDF/JPG/PNG tối đa 20 MiB theo BR-CTR-004. URL xem ảnh tạm thời là URL công khai, cố định; quyền upload độc lập với quyền sửa nội dung sử dụng ảnh.

## When

- Người dùng xin URL upload hoặc gửi ảnh qua luồng upload mới.
- Người dùng gửi URL ảnh đã upload vào một API nghiệp vụ của BMT.
- Một người mở URL công khai của ảnh còn tồn tại trong kho.

## Then

1. Chỉ cấp URL upload khi phiên đăng nhập hợp lệ. Luồng ảnh mặc định không yêu cầu vai trò quản trị; purpose ContractorImage và ContractorScan chỉ dành cho admin.
2. Purpose Image mặc định nhận JPG/PNG/WebP ≤5 MiB; ContractorImage nhận JPG/PNG/WebP ≤10 MiB; ContractorScan nhận PDF/JPG/PNG ≤20 MiB. Không nhận WebP làm bản scan.
3. Frontend upload trực tiếp lên BizFly bằng presigned URL do backend BMT cấp.
4. URL upload có thời hạn. URL xem ảnh là URL https cố định, không hết hạn theo thời hạn URL upload; người có URL xem ảnh không cần đăng nhập BMT.
5. API nghiệp vụ vẫn kiểm tra quyền và các điều kiện lưu hiện có. Có quyền upload không cấp thêm quyền tạo hoặc sửa bài viết, dự toán hay nội dung khác.
6. Khi thay ảnh, chỉ thay URL đã lưu sau khi API nghiệp vụ lưu thành công. Thao tác lưu thất bại không làm mất liên kết tới ảnh cũ.
7. Ảnh upload không được dùng hoặc ảnh đã bị gỡ/thay được xét dọn theo BR-MEDIA-002.

## Except

- Quy tắc định dạng và dung lượng ở đây áp dụng cho luồng upload ảnh mới; không tự biến thành điều kiện xóa các ảnh cũ đã tồn tại.
- URL xem ảnh không còn bảo đảm đọc được sau khi ảnh đủ điều kiện dọn và đã bị xóa theo BR-MEDIA-002.

## Notes

- Đã xác nhận: dùng BizFly, cấp presign trong backend BMT, phục vụ cả quản trị viên và khách hàng, yêu cầu đăng nhập, JPG/PNG/WebP tối đa 5 MiB, URL xem công khai và cố định.
- Thời hạn cụ thể của URL upload và cách kiểm tra file thực tế thuộc thiết kế tiếp theo; không xem giới hạn frontend hoặc tham số chưa được thực thi là bằng chứng file đã đạt điều kiện.
- Đây là thay đổi so với cách một số tài liệu cũ mô tả dịch vụ presign nằm ngoài backend. Cần đối chiếu và cập nhật các tài liệu bị ảnh hưởng trước khi triển khai; bộ US/BR này đã được người dùng chốt.
- Reviewer và Approver là Tân Trần theo thông tin người dùng cung cấp. Owner là Tân Trần theo xác nhận bổ sung của người dùng; ngày hiệu lực chưa xác định. Người dùng đã chốt nội dung bộ US/BR trong hội thoại ngày 30/09/2026. Status vẫn là Draft vì chưa thực hiện phê duyệt trên hệ thống tài liệu.
