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

# BR-PROJ-002

## Rule Info

- **Name**: Đầu vào thiết kế có ảnh hoặc mô tả và tuân thủ giới hạn dữ liệu.
- **Category**: Tạo dự toán — thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại: có ít nhất ảnh hoặc mô tả, không bắt buộc đồng thời cả hai; giữ một ảnh JPG/PNG/HEIC tối đa 10 MB và mô tả tối đa 500 ký tự như trang mẫu.

## Statement

Thông tin dùng để tạo thiết kế phải có ít nhất ảnh hoặc mô tả. Khách được cung cấp chỉ ảnh, chỉ mô tả hoặc cả hai. Mỗi bản dự toán nhận tối đa một ảnh thuộc định dạng JPG, PNG hoặc HEIC, dung lượng tối đa 10 MB; trường Mô tả chi tiết tối đa 500 ký tự, dùng làm đầu vào gửi AI. Không có trường ghi chú riêng trong phạm vi Tạo dự toán.

## When

Hệ thống kiểm tra tính đầy đủ của thông tin bản dự toán dùng làm đầu vào cho AI service.

## Then

1. Có ảnh mà không có mô tả thì đáp ứng điều kiện về sự hiện diện của ảnh/mô tả.
2. Có mô tả mà không có ảnh thì đáp ứng điều kiện này.
3. Có cả ảnh và mô tả thì đáp ứng điều kiện này; không bắt khách bỏ một trong hai.
4. Không có cả ảnh lẫn mô tả thì thông tin chưa đủ để gửi AI; yêu cầu khách bổ sung ít nhất một trong hai.
5. Đáp ứng điều kiện ảnh/mô tả không thay thế yêu cầu về diện tích hoặc các trường đầu vào khác được chốt riêng.
6. Khi có ảnh, chỉ nhận tối đa một ảnh JPG, PNG hoặc HEIC, không vượt quá 10 MB. Ảnh ngoài định dạng hoặc vượt giới hạn không được coi là ảnh đầu vào hợp lệ.
7. Khi có Mô tả chi tiết, nội dung không vượt quá 500 ký tự. Nội dung vượt giới hạn không được coi là mô tả đầu vào hợp lệ.

8. Khi thay ảnh đầu vào, giữ ảnh cũ đang gắn với bản dự toán cho đến khi ảnh mới hợp lệ, tải lên và lưu thay thế thành công. Nếu tải hoặc lưu ảnh mới thất bại, ảnh cũ vẫn là ảnh đầu vào đã lưu; không báo đã thay ảnh thành công.
9. Sau khi thay thành công, bản dự toán chỉ có một ảnh đầu vào là ảnh mới. Việc xử lý tệp tải tạm không có nghĩa cho phép nhiều ảnh đầu vào.

## Except

Được tạo dự toán bằng tên và tự lưu tiến độ khi chưa có ảnh hoặc mô tả theo BR-PROJ-003. Điều kiện có ít nhất một trong hai áp dụng trước khi gửi AI, không chặn lưu thông tin chưa đầy đủ.

## Notes

- Người dùng đã chốt bộ US/BR trong hội thoại. Trạng thái Draft của file không phải bằng chứng phê duyệt hoặc import trên hệ thống quản lý tài liệu.
- Đã chốt số lượng ảnh, định dạng, dung lượng và giới hạn mô tả. Đã chốt giữ ảnh cũ đến khi ảnh mới tải và lưu thay thế thành công; trường hợp chưa có ảnh cũ mà tải ảnh mới lỗi thì bản dự toán vẫn chưa có ảnh hợp lệ. Dấu bắt buộc trên giao diện mẫu không thay thế quyết định cho phép chỉ có mô tả.
- Mô tả chi tiết là trường mô tả thiết kế duy nhất, được gửi AI khi khách cung cấp. Người dùng đã chọn không thêm ghi chú riêng; đề xuất ghi chú riêng tùy chọn 500 ký tự trước đó không áp dụng. Giới hạn tên bản dự toán được quy định riêng tại BR-PROJ-003.
- Quy tắc này chỉ xác định sự hiện diện của dữ liệu, không khẳng định mọi tệp hoặc nội dung mô tả đều hợp lệ.
- Tầng thực thi (người dùng xác nhận ngày 26/09/2026): frontend kiểm định dạng JPG/PNG/HEIC và dung lượng tối đa 10 MB ở khoản 6 trước khi upload tệp qua dịch vụ presigned URL; backend chỉ lưu URL của tệp và kiểm URL là URL tuyệt đối dùng https, không giới hạn tên miền. Backend không kiểm định dạng hay dung lượng, nên yêu cầu gửi thẳng API với URL https của tệp sai định dạng không bị backend chặn. "Lưu thay thế thành công" ở khoản 8 là lưu URL mới thành công; upload hoặc lưu lỗi thì URL cũ vẫn là ảnh đầu vào. Ảnh đầu vào nằm ở URL công khai theo xác nhận của người dùng. Nghĩa của các khoản không đổi. Thiết kế ở TDD-PROJ-001.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
