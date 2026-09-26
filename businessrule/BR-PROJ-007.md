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

# BR-PROJ-007

## Rule Info

- **Name**: Dự toán và hồ sơ dùng kết quả AI đã lưu; backend không tự tính nội dung chuyên môn.
- **Category**: Tạo dự toán — kết quả và hồ sơ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận phạm vi cả ba bước như trang mẫu; AI service do bên khác phụ trách và trả kết quả, BMT thu thập đầu vào; giữ PDF, Excel và các thao tác chia sẻ. Hợp đồng AI trước mắt ghi chờ tích hợp. Ngày 26/09/2026 người dùng xác nhận khoản 4 (AI trả PDF/Excel, backend không tự dựng) và khoản 7 (tên mới chỉ áp vào tên tệp tải về).

## Statement

Backend BMT dùng kết quả AI đã lưu để cung cấp bảng chi phí và hồ sơ thiết kế, thi công gắn với từng bản dự toán. AI service chịu trách nhiệm nội dung thiết kế, khối lượng, đơn giá và số tiền. Backend không xây bảng đơn giá, tự bóc khối lượng hoặc tính lại dự toán. Khách được xem kết quả, tải PDF hồ sơ và Excel dự toán trong phạm vi tính năng.

## When

Hệ thống nhận kết quả AI hoặc khách yêu cầu xem dự toán, xem hồ sơ và tải tệp của bản dự toán.

## Then

1. Giữ cấu trúc dự toán theo trang mẫu gồm phần thô, hoàn thiện và nội thất, cùng tổng dự toán và nội dung tư vấn do AI trả. Áp dụng phạm vi này cho mọi loại công trình trong danh mục theo BR-PROJ-004; không tự loại nhóm phần thô riêng với Căn hộ.
2. Dữ liệu phục vụ màn hình, PDF và Excel phải thuộc đúng bản dự toán và cùng kết quả nguồn đã lưu. Không dùng số tiền, diện tích hoặc hình minh họa mẫu của website làm kết quả thật.
3. Kiểm tra phản hồi theo hợp đồng AI trước khi công bố. Hợp đồng cần xác định trường và đầu ra bắt buộc; không tự điền kết quả chuyên môn còn thiếu để báo thành công.
4. Hỗ trợ xem hồ sơ, chuẩn bị/tải PDF và tải Excel dự toán. AI trả tệp PDF và Excel như mọi tệp kết quả; backend không tự dựng tệp. Thời điểm AI trả từng tệp xác định khi có hợp đồng.
5. Xem, xuất hoặc tải từ kết quả cũ không tạo thiết kế AI mới, không thay thế kết quả nguồn và không giữ/trừ thêm lượt tạo thiết kế.
6. Khi gói hết hạn, vẫn được dùng các thao tác hồ sơ cũ theo BR-SUB-007. Chia sẻ qua link, QR và email áp dụng BR-PROJ-006.
7. Sau khi chủ sở hữu đổi tên bản dự toán theo BR-SUB-007 khoản 11, màn hình của chủ sở hữu và trang xem qua link luôn hiện tên hiện tại. Tệp PDF/Excel tải về sau khi đổi tên mang tên hiện tại ở tên tệp; nội dung tệp giữ nguyên như lúc AI tạo, kể cả tên in trong tệp nếu có. Việc tải lại không tính lượt, không gọi AI, không tự dựng lại tệp và không thay kết quả nguồn. Tệp người nhận đã tải về trước đó không bị thu hồi.

## Except

Nếu chưa có bộ kết quả hợp lệ hoặc chưa chuẩn bị được tệp, không báo đã có tệp để tải. Lỗi xuất hoặc tải tệp từ một kết quả đã thành công không biến thành một lần tạo thiết kế mới hoặc tự trừ lượt; giữ kết quả nguồn để xử lý lại thao tác bị lỗi.

## Notes

- Khoản 7 do người dùng xác nhận ngày 25/09/2026. Ngày 26/09/2026 người dùng xác nhận sửa phần về tệp: bản cũ yêu cầu tệp xuất trước khi đổi tên không được dùng nữa và lần tải sau xuất lại với tên mới. Vì AI tạo PDF/Excel và backend không tự dựng tệp, tên mới nay chỉ áp vào tên tệp tải về (backend đặt khi chuyển tiếp tệp, TDD-PROJ-003); nội dung tệp giữ tên tại lúc AI tạo.
- Khoản 4 cập nhật ngày 26/09/2026: AI trả URL cho mọi tệp kết quả, kể cả PDF và Excel; backend chỉ lưu URL, không tự dựng tệp. Người dùng xác nhận giữ bản sửa khoản 4 này ngày 26/09/2026.

- Trang mẫu mô tả dự toán là tham khảo. Không suy ra giá trị mẫu hoặc hình mẫu là định mức áp dụng cho công trình thực tế.
- BR-PROJ-004 tách lựa chọn phong cách kiến trúc và nội thất theo loại công trình. Người dùng đã chốt cấu hình chỉ điều khiển lựa chọn đầu vào; AI vẫn trả đủ kết quả. Không loại phần thiết kế, dự toán hoặc hồ sơ chỉ vì một nhóm lựa chọn phong cách bị tắt.
- Trang hồ sơ có bìa, mặt bằng 2D, phối cảnh và dự toán. Danh sách đầu ra thực sự bắt buộc, định dạng, độ chi tiết và điều kiện hoàn tất cần đối chiếu hợp đồng AI; không cam kết số trang hoặc bộ môn bản vẽ chưa được xác nhận.
- Đầu vào Diện tích theo BR-PROJ-001 phải giữ đúng giá trị khách nhập. Nếu AI trả thêm diện tích sàn ước tính, đó là dữ liệu kết quả riêng, không âm thầm ghi đè trường đầu vào.
- BR-SUB-003 quyết định thời điểm thành công và tính lượt. Cần chốt rõ tệp nào thuộc bộ kết quả bắt buộc lúc hoàn tất AI, tệp nào được xuất sau từ kết quả nguồn; không lấy nút Render hồ sơ trên trang mẫu để tự quyết định mốc này.
- Chưa thêm bước nhân viên duyệt nội dung hoặc quy trình kiểm định chuyên môn vào luồng. Hợp đồng AI và các giới hạn của kết quả sẽ được làm rõ trước tích hợp.
- Owner và ngày hiệu lực chưa xác định; bản nháp chưa được phê duyệt và chưa có kiểm thử thực thi.
