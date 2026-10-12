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

- **Name**: Đầu vào thiết kế gồm ảnh lô đất bắt buộc, mô tả không bắt buộc và các ô AI cần.
- **Category**: Tạo dự toán — thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: giữ biểu mẫu hiện có, xử lý theo cách “Tải ảnh lô đất + mô tả” của app demo `design-estimate`; ảnh lô đất bắt buộc, JPG/PNG/WebP, tối đa 10 MB riêng cho ảnh lô, tối thiểu 640×480; mô tả tối đa 4.000 ký tự, không bắt buộc khi đã có ảnh lô; thêm số phòng ngủ 1–10 bắt buộc, có gara ô tô không bắt buộc. Người dùng chốt ngày 12/10/2026: cạnh có cửa vào làm như khối “Xác nhận trước khi vẽ” của app demo — hiện sau khi ảnh lô soi Đạt, mặc định Trái, chỉ ghi nhận khi khách bấm chọn. Thay quy tắc trước “có ít nhất ảnh hoặc mô tả; ảnh tối đa 5 MiB; mô tả tối đa 500 ký tự”.

## Statement

Trước khi chạy chặng 2D, bản dự toán phải có đúng một ảnh lô đất hợp lệ và đã soi Đạt theo BR-PROJ-015, và số phòng ngủ mong muốn từ 1 đến 10. Mô tả chi tiết không bắt buộc, tối đa 4.000 ký tự. Ô có gara ô tô không bắt buộc. Không có trường ghi chú riêng.

## When

Khách nhập hoặc sửa đầu vào, hệ thống tự lưu, hoặc kiểm tra đầu vào trước khi chạy chặng 2D.

## Then

1. Ảnh lô đất bắt buộc. Mỗi bản dự toán có tối đa một ảnh lô: JPG, PNG hoặc WebP, không vượt 10 MiB (10.485.760 byte), tối thiểu 640×480 điểm ảnh. Ảnh được tải qua luồng tải ảnh lô đất riêng theo BR-MEDIA-001 và được soi theo BR-PROJ-015. Thiếu ảnh, ảnh không hợp lệ hoặc chưa soi Đạt thì chưa đủ điều kiện chạy 2D.
2. Mô tả chi tiết không bắt buộc. Khi có, nội dung không vượt 4.000 ký tự và được dùng làm đầu vào của chặng 2D. Mô tả vượt giới hạn bị từ chối khi tạo và khi tự lưu; không tự cắt ngắn.
3. Số phòng ngủ mong muốn là số nguyên từ 1 đến 10, bắt buộc trước khi chạy 2D. Đây là mong muốn của khách; số phòng thực tế trên phương án có thể ít hơn khi lô không đủ chỗ, và lý do được ghi trong kết quả 2D. Diện tích và cạnh tối thiểu của phòng ngủ lấy từ bộ quy tắc AI theo BR-PROJ-019.
4. Có gara ô tô là lựa chọn có/không, mặc định không. Chọn có thì chặng 2D bố trí gara khi lô cho phép; mặt tiền tối thiểu để bố trí gara lấy từ bộ quy tắc AI theo BR-PROJ-019 (app demo dùng 4,5 m). Lô không đủ thì phương án không có gara và ghi lý do trong kết quả 2D.
5. Cạnh có cửa vào (Trên, Trái, Phải, Dưới) là cạnh của bản vẽ lô có cửa chính hoặc cổng. Khi ảnh lô đã soi Đạt, biểu mẫu hiện khối xác nhận cạnh này bên cạnh ảnh thu nhỏ, như khối “Xác nhận trước khi vẽ” của app demo: mặc định hiển thị Trái kèm lời nhắc chạm cạnh khác nếu bản vẽ có cửa ở cạnh khác. Ô không bắt buộc. Khách chưa bấm thì hệ thống dùng cạnh trái, ghi nhận đó là mặc định (không phải khách chọn) và gắn cờ nội bộ cho nhân viên rà theo BR-PROJ-017; khách đã bấm thì dùng đúng cạnh đó.
6. Khi thay ảnh lô, giữ ảnh cũ cho tới khi ảnh mới hợp lệ, tải lên và lưu thay thế thành công. Nếu tải hoặc lưu ảnh mới thất bại, ảnh cũ vẫn là ảnh đầu vào; không báo đã thay thành công. Kết quả soi đi theo ảnh: thay ảnh thì phải có kết quả soi của ảnh mới.
7. Sau khi thay thành công, bản dự toán chỉ có một ảnh lô là ảnh mới. Việc xử lý tệp tải tạm không có nghĩa cho phép nhiều ảnh đầu vào.
8. Các điều kiện ở khoản 1–4 cùng với BR-PROJ-001 và BR-PROJ-004 là điều kiện đầu vào của chặng 2D; đáp ứng điều kiện này không thay thế việc kiểm tra quyền, gói và lượt.

## Except

Bản dự toán được tạo nhanh chỉ với tên và tự lưu khi chưa có ảnh, mô tả, số phòng ngủ hay các ô khác theo BR-PROJ-003. Các điều kiện bắt buộc chỉ áp dụng khi chạy chặng 2D, không chặn lưu bản nháp.

## Notes

- **Đã xác nhận (11/10/2026):** ảnh lô bắt buộc; JPG/PNG/WebP; tối thiểu 640×480; 10 MiB riêng cho ảnh lô (các ảnh khác giữ 5 MiB); mô tả tối đa 4.000 ký tự và không bắt buộc khi đã có ảnh lô; số phòng ngủ 1–10 bắt buộc; gara không bắt buộc.
- **Đã xác nhận (11/10/2026):** gara mặc định không (khoản 4).
- **Đã xác nhận (12/10/2026):** cạnh có cửa vào theo khối “Xác nhận trước khi vẽ” của luồng tải ảnh lô trong app demo (`web/app.js`, `confirmBlockHtml`): hiện sau khi soi Đạt, mặc định Trái, chưa bấm thì ghi nhận là mặc định. Thay quyết định bỏ ô cùng ngày, vốn dựa trên đối chiếu thiếu: AI không trả cạnh mặt tiền dưới dạng dữ liệu, nên không có ô này thì bản vẽ luôn đặt mặt tiền ở cạnh trái dù ảnh hay mô tả nêu cạnh khác.
- App demo nhận nhiều ảnh, mỗi ảnh một tầng, và cho chạy chỉ với mô tả khi không có ảnh. Hệ thống thật giữ một ảnh lô cho mỗi bản dự toán và bắt buộc có ảnh.
- Tầng thực thi: frontend tải ảnh lên kho qua URL presign; backend tự đọc nội dung ảnh từ kho để kiểm tra định dạng, dung lượng, kích thước và tính hash (BR-PROJ-015). Ảnh cũ chỉ được gỡ sau khi lưu ảnh mới thành công; dọn ảnh không còn nơi dùng theo BR-MEDIA-002.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
