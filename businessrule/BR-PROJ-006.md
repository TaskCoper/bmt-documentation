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

# BR-PROJ-006

## Rule Info

- **Name**: Chia sẻ hồ sơ bằng link, QR và email có thời hạn, có thể thu hồi.
- **Category**: Tạo dự toán — chia sẻ hồ sơ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại: giữ đủ PDF, Excel, link, QR và email; ai có link/QR đều được xem và tải không cần đăng nhập; chủ sở hữu bản dự toán chọn ngày hết hạn, được thu hồi sớm; email gửi link dùng cùng thời hạn và quyền thu hồi.

## Statement

Chủ sở hữu bản dự toán chia sẻ hồ sơ bằng link và QR, hoặc gửi link qua email. Bất kỳ ai có link còn hiệu lực đều được xem và tải hồ sơ mà không cần đăng nhập. Chủ sở hữu bản dự toán chọn ngày hết hạn và có thể thu hồi quyền truy cập qua link trước ngày đó.

## When

Chủ sở hữu bản dự toán chia sẻ hồ sơ, chọn ngày hết hạn hoặc thu hồi link; người nhận mở link, quét QR hoặc truy cập từ email.

## Then

1. Khi tạo link chia sẻ, chủ sở hữu bản dự toán chọn ngày hết hạn. Không tự thay bằng thời hạn cố định hoặc mặc định không hết hạn.
2. QR và link gửi trong email dẫn tới cùng quyền truy cập hồ sơ được chia sẻ. Chủ sở hữu bản dự toán nhập một địa chỉ email nhận mỗi lần gửi. Email gửi link, không đính kèm PDF hoặc Excel.
3. Khi link còn hiệu lực và chưa bị thu hồi, cho người nhận xem và tải hồ sơ mà không yêu cầu đăng nhập.
4. Chủ sở hữu bản dự toán có thể thu hồi sớm. Sau khi thu hồi hoặc đến hạn, từ chối yêu cầu xem và tải mới qua link đó, kể cả mở từ QR hoặc email đã gửi trước đây.
5. Quyền qua link chỉ cho xem và tải hồ sơ được chia sẻ; không cấp quyền sửa đầu vào, gửi AI, quản lý chia sẻ hoặc xem bản dự toán khác.
6. Việc hết hạn hoặc thu hồi link không xóa kết quả nguồn của bản dự toán và không tạo một lần xử lý AI mới.
7. Chủ sở hữu bản dự toán vẫn được xuất PDF/Excel, tạo link/QR, gửi email và thu hồi link của hồ sơ đã có khi gói hết hạn theo BR-SUB-007. Gói hết hạn không tự làm link hết hiệu lực; kiểm tra ngày hết hạn và việc thu hồi của chính link đó.

8. Link dùng được hết ngày chủ sở hữu chọn theo giờ Việt Nam (Asia/Ho_Chi_Minh); từ 00:00 ngày kế tiếp thì hết hiệu lực. Cho phép chọn ngày hiện tại, không cho chọn ngày đã qua theo múi giờ này. Ví dụ chọn 25/09/2026 thì hết hiệu lực lúc 00:00 ngày 26/09/2026 giờ Việt Nam.
9. Mỗi bản dự toán chỉ có một link đang hiệu lực, dùng chung cho sao chép, QR và email. Khi link còn hiệu lực thì dùng lại; chỉ tạo link khác sau khi link cũ hết hạn hoặc bị thu hồi. Các yêu cầu đồng thời không được tạo nhiều link đang hiệu lực cho cùng bản dự toán.

## Except

Thu hồi hoặc hết hạn chỉ ngăn truy cập tiếp qua link; không thu hồi được bản tệp mà người nhận đã tải về trước đó. Quyền của chủ sở hữu bản dự toán khi đăng nhập được kiểm tra riêng, không phụ thuộc vào việc link chia sẻ còn hiệu lực hay không.

## Notes

- Phạm vi đã xác nhận gồm tải PDF hồ sơ và Excel dự toán. Cách nhận hoặc xuất tệp từ kết quả AI còn chờ hợp đồng tích hợp, không mặc định AI đã trả sẵn các tệp này.
- Quyền thao tác trên hồ sơ cũ sau khi gói hết hạn đã được người dùng xác nhận riêng; BR-SUB-007 được bổ sung tương ứng. Đây không phải quyền tạo thiết kế AI mới.
- Múi giờ và mốc hết hiệu lực đã được xác nhận tại khoản 8. Thiết kế phải kiểm tra hiệu lực cho từng yêu cầu xem/tải, không dùng đường tải tệp bỏ qua việc thu hồi hoặc hết hạn.
- Đã chốt một link đang hiệu lực tại khoản 9. Thay đổi ngày hết hạn hoặc gia hạn link cũ chưa thuộc phạm vi; không tự bổ sung các thao tác này.
- Người dùng đã xác nhận nhập một địa chỉ email nhận mỗi lần gửi, không giới hạn về email tài khoản của chủ sở hữu bản dự toán. Địa chỉ phải có định dạng email hợp lệ; chưa mở gửi đồng thời nhiều người. Chưa gửi email hoặc liên hệ bên thứ ba trong quá trình khảo sát.
- Reviewer và Approver là Tân Trần. Owner và ngày hiệu lực chưa xác định; bản nháp chưa được phê duyệt, chưa có kết quả kiểm thử.
