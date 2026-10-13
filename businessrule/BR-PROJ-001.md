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

- **Name**: Kích thước lô đất (mặt tiền × chiều sâu) thay cho trường Diện tích; không bắt buộc.
- **Category**: Tạo dự toán — thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: bỏ ô Diện tích, thay bằng mặt tiền × chiều sâu, không bắt buộc; ngày 11/10/2026 (lượt sau) bỏ điền sẵn kích thước từ bước soi ảnh, giống app demo. Thay quyết định trước đó dùng một trường Diện tích bắt buộc chung cho mọi loại công trình. Danh mục năm loại ban đầu giữ nguyên.

## Statement

Danh mục ban đầu gồm Nhà phố, Villa/Biệt thự, Nhà mái, Nhà vườn/Nhà cấp 4 và Căn hộ; Admin được thêm loại theo BR-PROJ-004. Đầu vào không còn trường Diện tích. Khách có thể nhập kích thước lô đất gồm mặt tiền và chiều sâu, đơn vị mét; hai ô không bắt buộc. Hai ô không được điền sẵn; khi khách để trống, chặng 2D tự đọc kích thước trên ảnh lô như app demo.

## When

Khách nhập hoặc sửa kích thước lô đất, hoặc hệ thống kiểm tra đầu vào trước khi chạy chặng 2D.

## Then

1. Cho phép chọn loại công trình từ danh mục do Admin quản lý theo BR-PROJ-004; giữ hỗ trợ năm loại ban đầu.
2. Không có trường Diện tích trên biểu mẫu và không dùng Diện tích làm điều kiện chạy chặng 2D.
3. Mặt tiền và chiều sâu, khi có, phải là số lớn hơn 0, tối đa hai chữ số thập phân. Giá trị 0, âm hoặc vượt độ chính xác không hợp lệ; không tự làm tròn để chấp nhận.
4. Hai ô đi cùng nhau: hoặc để trống cả hai, hoặc có cả hai. Chỉ có một ô có giá trị thì đầu vào chưa đủ điều kiện chạy 2D; bản nháp vẫn được lưu.
5. Hệ thống không tự điền hay ghi đè hai ô này. Bước soi ảnh (BR-PROJ-015) không đọc kích thước lô; kích thước trên ảnh chỉ được AI đọc trong chặng 2D.
6. Khi chạy 2D, kích thước lô của phương án lấy theo thứ tự ưu tiên:
   1. số khách nhập hoặc chấp nhận ở hai ô;
   2. kích thước AI đọc được từ ảnh lô trong bước lập phương án của chặng 2D;
   3. kích thước đọc được từ mô tả;
   4. kích thước lô mặc định theo mã gốc của loại công trình, lấy từ bộ quy tắc AI theo BR-PROJ-019.

   Thứ tự này là thuật toán trong mã nguồn, Admin không đổi được; Admin chỉ đổi được kích thước mặc định ở bước 4.
7. Không tự đổi hai số khách nhập thành một giá trị diện tích hay nhân với số tầng. Diện tích sàn và diện tích xây dựng là kết quả của chặng 2D, lưu riêng, không ghi đè đầu vào.
8. Kích thước khách nhập phải nằm trong giới hạn lô của mã gốc loại công trình (mặc định theo app demo: mỗi cạnh 2,5–120 m, diện tích 12–5.000 m²; lấy từ bộ quy tắc AI theo BR-PROJ-019). Ngoài giới hạn thì yêu cầu tạo bản vẽ 2D bị từ chối ngay ở ô kích thước, trước khi giữ lượt; bản nháp vẫn lưu được.

## Except

Bản nháp được tạo và tự lưu khi chưa có kích thước lô theo BR-PROJ-003. Bản dự toán tạo trước khi đổi quy tắc mà đã có giá trị Diện tích vẫn giữ giá trị đó để tra cứu; giá trị này không còn là đầu vào của luồng bốn chặng.

## Notes

- **Đã xác nhận (11/10/2026):** bỏ Diện tích; thêm mặt tiền × chiều sâu không bắt buộc. Lượt sau cùng ngày: bỏ điền sẵn từ bước soi ảnh, vì ở chế độ AI thật app demo không đọc kích thước khi soi.
- **Đã xác nhận (11/10/2026):** giới hạn số ở khoản 3 (giữ như trường Diện tích cũ); hai ô phải có cùng nhau ở khoản 4; không tự điền hay ghi đè ở khoản 5.
- **Đã xác nhận (11/10/2026):** thứ tự ưu tiên kích thước lô ở khoản 6; kích thước mặc định và các ngưỡng luật lô đất theo loại công trình lưu trong bộ quy tắc AI (BR-PROJ-019).
- **Đã xác nhận (11/10/2026, khi duyệt TDD):** lô khách nhập ngoài giới hạn bị chặn trước khi giữ lượt (khoản 8), giống app demo từ chối trước khi gọi AI. Các luật lô đất khác của app demo (`savico/modules/plan2d/lot_rules.py`) vẫn có thể khiến lần chạy 2D lỗi; đó là kết quả của chặng 2D theo BR-PROJ-011.
- Quyết định trước (một trường Diện tích bắt buộc, lớn hơn 0, tối đa hai chữ số thập phân) không còn áp dụng cho luồng bốn chặng.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
