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

# BR-PROJ-001

## Rule Info

- **Name**: Các loại công trình dùng một trường diện tích chung.
- **Category**: Tạo dự toán — thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận: hỗ trợ năm loại công trình, dùng một trường diện tích bắt buộc chung thay hai trường diện tích đất/xây dựng. Diện tích lớn hơn 0, tối đa hai chữ số thập phân, chưa đặt giới hạn tối đa theo nghiệp vụ.

## Statement

Danh mục ban đầu gồm Nhà phố, Villa/Biệt thự, Nhà mái, Nhà vườn/Nhà cấp 4 và Căn hộ; Admin được thêm loại theo BR-PROJ-004. Các loại công trình dùng một trường bắt buộc có nhãn Diện tích, đơn vị m²; giá trị lớn hơn 0 và có tối đa hai chữ số thập phân. Chưa đặt giới hạn tối đa theo nghiệp vụ. Không yêu cầu nhập riêng diện tích đất, diện tích xây dựng hoặc chiều ngang/dài.

## When

Hệ thống kiểm tra tính đầy đủ của thông tin bản dự toán dùng làm đầu vào cho AI service.

## Then

1. Cho phép chọn loại công trình từ danh mục do Admin quản lý theo BR-PROJ-004; giữ hỗ trợ năm loại ban đầu.
2. Yêu cầu có giá trị cho trường Diện tích cho các loại công trình. Thiếu diện tích thì thông tin chưa đủ để gửi AI.
3. Không thêm trường diện tích đất hoặc diện tích xây dựng bắt buộc cho một nhóm loại công trình.
4. Không coi diện tích là dữ liệu chỉ dành cho Căn hộ hoặc cho bốn loại còn lại.
5. Truyền giá trị diện tích người dùng cung cấp theo hợp đồng tích hợp sau này. Không tự thay trường này bằng một giá trị nhân với số tầng; tính toán kết quả thuộc AI service theo quyết định của người dùng.
6. Giá trị diện tích đã nhập phải là số lớn hơn 0, tối đa hai chữ số thập phân. Số nguyên dương vẫn hợp lệ; giá trị 0, âm hoặc vượt độ chính xác này không hợp lệ. Không tự làm tròn dữ liệu vượt độ chính xác để coi là hợp lệ.
7. Không tự đặt trần diện tích theo loại công trình hoặc một con số tối đa nghiệp vụ khi chưa có quyết định mới.

## Except

Được tạo dự toán bằng tên và tự lưu tiến độ khi chưa nhập diện tích theo BR-PROJ-003. Điều kiện bắt buộc có diện tích áp dụng trước khi gửi AI, không chặn lưu thông tin chưa đầy đủ.

## Notes

- Người dùng đã chốt bộ US/BR của Tạo dự toán trong hội thoại. Status Draft và metadata chưa đủ vẫn được giữ; chưa import hoặc phê duyệt trên hệ thống quản lý tài liệu.
- Phương án hai trường diện tích đất/diện tích xây dựng và việc xác định trường diện tích xây dựng là diện tích chiếm đất đã được trao đổi trước quyết định một trường chung. Không dùng cấu trúc hai trường cũ để triển khai.
- Đã chốt giá trị lớn hơn 0 và tối đa hai chữ số thập phân. Ví dụ 70, 70.5 và 70.25 đáp ứng giới hạn số; 0, -1 và 70.251 không đáp ứng. Các số này chỉ minh họa quy tắc, không phải giá trị mặc định.
- Kiểu dữ liệu lưu trữ và giới hạn biểu diễn kỹ thuật sẽ xác định khi thiết kế; không coi “chưa đặt trần nghiệp vụ” là khả năng lưu số lớn tùy ý. Cách diễn giải diện tích trong hợp đồng với AI service còn chờ tích hợp; backend không tự đặt công thức tính tổng diện tích sàn.
- AI service do bên khác phụ trách và đang chờ tích hợp. Chưa có tên trường hoặc kiểu dữ liệu API đã thống nhất.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
