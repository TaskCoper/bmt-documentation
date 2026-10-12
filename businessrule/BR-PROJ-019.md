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

# BR-PROJ-019

## Rule Info

- **Name**: Quy tắc AI theo loại công trình, phong cách, gói và miền được lưu thành dữ liệu có phiên bản; phần thuật toán vẫn nằm trong mã nguồn.
- **Category**: Tạo dự toán — bộ quy tắc AI
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: lưu riêng đặc điểm từng loại công trình (luật lô đất, lô mặc định), quy tắc mái (loại công trình + phong cách kiến trúc → kiểu mái), mức theo gói cho 2D, Dự toán, 3D, quy tắc theo miền, quy tắc phòng và gara; lưu theo hướng lai; Admin quản lý; bảng có ngay ở đợt dùng, màn Admin làm sau đợt 4.

## Statement

Các quy tắc mà luồng AI dùng theo loại công trình, phong cách, gói và miền được lưu thành dữ liệu, không viết cố định trong mã nguồn. Mỗi quy tắc gồm phạm vi áp dụng và nội dung. Phạm vi lưu dạng quan hệ, trỏ tới mã gốc loại công trình, mã phong cách gốc, gói, miền hoặc chặng có thật. Nội dung là con số khi hệ thống cần tính, và là văn bản khi chỉ cần đưa cho AI đọc. Bộ quy tắc có phiên bản; mỗi lần chạy chặng chốt phiên bản đang áp dụng. Phần thuật toán (cách tính, cách đo, cách quyết định) vẫn nằm trong mã nguồn; Admin chỉ sửa được con số và lời dặn.

## When

Hệ thống tiếp nhận lần chạy chặng và ghép đầu vào cho AI; Admin xem hoặc sửa bộ quy tắc; nhóm dự án nạp sẵn dữ liệu cho một đợt triển khai.

## Then

1. Bộ quy tắc gồm các nhóm sau:
   - **Theo mã gốc loại công trình:** kích thước lô mặc định (mặt tiền, chiều sâu); số tầng tối đa; có tum, có mái, có phần thô hay không; các ngưỡng của luật lô đất (ví dụ bề ngang tối thiểu); mặt tiền tối thiểu để bố trí gara ô tô; diện tích và cạnh tối thiểu của phòng ngủ; kèm lời dặn cho AI.
   - **Ma trận mái:** mỗi mã gốc có một kiểu mái mặc định; mỗi cặp (mã gốc, mã phong cách kiến trúc gốc) có thể có một kiểu mái riêng thay cho mặc định. Kiểu mái thuộc danh sách cố định của hệ thống (ví dụ mái ngói kiểu Thái, mái ngói kiểu Nhật, mái ngói/mansard tân cổ điển, mái bằng bê tông cốt thép, không có mái). Mỗi dòng có hệ số độ dốc, hệ số quy đổi sàn và câu tả mái dùng cho ảnh 3D.
   - **Gói × chặng:** với mỗi gói (Cơ bản, Tiêu chuẩn, VIP) và mỗi chặng 2D, Dự toán, Phối cảnh 3D, có lời dặn riêng; ví dụ 2D là mức đồ đạc trên bản vẽ nội thất, 3D là chất lượng vật liệu. Số tiền và tỷ lệ theo gói vẫn nằm trong luật giá theo BR-PROJ-016, không lặp lại ở đây.
   - **Theo miền:** với mỗi miền (Bắc, Trung, Nam, Tây Nguyên) và mỗi chủ đề, có lời dặn riêng. Phiên bản nạp sẵn có chủ đề thời tiết trên ảnh 3D (ví dụ nắng gắt miền Nam, mùa bão miền Trung). Thêm chủ đề mới là thêm dòng, không đổi cấu trúc dữ liệu. Hãng vật tư theo miền và mùa khởi công vẫn nằm trong hai tài liệu luật giá theo BR-PROJ-016, không lặp lại ở nhóm này.
   - **Lời dặn cho AI chung:** mỗi dòng có phạm vi gồm chặng (bắt buộc) và tùy chọn mã gốc, gói, miền; nội dung văn bản có nội dung, tối đa 2.000 ký tự; trạng thái bật/tắt. Không giới hạn số dòng được ghép vào một prompt. Khi chạy, mọi dòng đang bật và khớp phạm vi được ghép vào prompt của chặng đó. Đây là cách thêm quy tắc chữ mới mà không cần sửa mã nguồn hay cấu trúc dữ liệu.
   - **Prompt:** phần lời dặn của từng prompt theo BR-PROJ-014.
2. Các quy tắc sau là thuật toán và nằm trong mã nguồn; Admin không sửa cách làm, chỉ sửa con số liên quan ở khoản 1:
   - Thứ tự ưu tiên kích thước lô theo BR-PROJ-001 khoản 6.
   - Đo hình học, đường bao lô và kiểm tra bản vẽ.
   - Quyết định lô có đủ chỗ cho số phòng ngủ hoặc gara mong muốn hay không; khi không đủ, phương án giảm số phòng hoặc bỏ gara và ghi lý do vào kết quả 2D.
   - Lưới an toàn của chặng Dự toán (BR-PROJ-017) và cách đọc mô tả tiếng Việt.
3. Khi lưu, hệ thống từ chối: phạm vi trỏ tới mã gốc, phong cách, gói, miền hoặc chặng không tồn tại; con số ngoài khoảng hợp lệ của từng trường; hai dòng cùng phạm vi trong nhóm chỉ cho phép một dòng (ví dụ hai kiểu mái cho cùng một cặp). Lỗi chỉ rõ dòng và trường; bộ quy tắc hiện hành giữ nguyên.
4. Mỗi lần lưu tạo phiên bản mới của bộ quy tắc; phiên bản cũ được giữ để tra cứu, so sánh và khôi phục như BR-PROJ-016 khoản 2, 7. Hai người lưu cùng lúc dựa trên cùng phiên bản thì người lưu sau bị từ chối.
5. Lần chạy chặng chốt phiên bản bộ quy tắc đang áp dụng lúc tiếp nhận và dùng nó tới khi kết thúc, kể cả lúc tự làm lại. Kết quả ghi lại phiên bản đã dùng theo BR-PROJ-014. Đổi quy tắc không làm đổi kết quả đã có.
6. Phiên bản đầu tiên được nạp sẵn từ app demo `design-estimate` ở mốc chuyển đổi (commit `059e876`): lô mặc định, ngưỡng, kiểu mái, mức đồ đạc, câu tả thời tiết theo miền và các lời dặn hiện viết trong mã của app demo. Với phiên bản nạp sẵn, kết quả trên các bộ dữ liệu đối chiếu phải giống app demo.
7. Bảng và dữ liệu nạp sẵn của nhóm quy tắc nào có ngay ở đợt triển khai dùng nhóm đó; mã nguồn đọc quy tắc từ dữ liệu. Trước khi có màn quản trị (STORY-PROJ-013, sau đợt 4), quy tắc chỉ đổi qua bản phát hành có migration dữ liệu.
8. Chỉ người có quyền quản lý quy tắc AI được xem và sửa. Khách không xem được quy tắc hay lời dặn.

## Except

Miền giá theo tỉnh/thành theo BR-PROJ-013, luật giá và dải giá theo BR-PROJ-016, model và hạn chờ theo STORY-PROJ-012 được quản lý riêng; quy tắc này không lặp lại các dữ liệu đó.

## Notes

- **Đã xác nhận (11/10/2026):** các nhóm quy tắc ở khoản 1; lưu theo hướng lai (phạm vi dạng quan hệ, số có kiểu, lời dặn văn bản, bảng lời dặn chung); quy tắc thuật toán vẫn là mã nguồn; bảng có ngay ở đợt dùng, màn Admin làm sau đợt 4.
- **Đã xác nhận (11/10/2026):** mỗi thông tin chỉ có một nơi lưu. Hãng vật tư theo miền (mục 9 Khung giá tham khảo) và mùa khởi công (Quy tắc bóc tách) giữ trong luật giá như app demo; thời tiết trên ảnh 3D (đang viết trong mã của app demo) chuyển sang nhóm theo miền. Lời dặn chung tối đa 2.000 ký tự mỗi dòng, không giới hạn số dòng; Admin tự cân nhắc độ dài prompt.
- Ví dụ ma trận mái theo app demo: Nhà phố mọi phong cách → mái bằng; Nhà mái + mái Nhật → ngói kiểu Nhật, còn lại → ngói kiểu Thái; Villa + tân cổ điển → ngói/mansard, villa hiện đại → mái bằng; Nhà vườn + sân vườn → ngói kiểu Thái, cấp 4 hiện đại → mái bằng; Căn hộ → không có mái. Kiểu mái dùng chung cho chặng Dự toán (tính tiền mái) và chặng 3D (vẽ mái), nên sai một dòng là sai cả giá lẫn ảnh.
- Ví dụ lô mặc định theo app demo: Nhà phố 5×20 m, Villa 12×15 m, Nhà mái 10×14 m, Nhà vườn/cấp 4 10×16 m, Căn hộ 9×10 m.
- Thiết kế bảng cụ thể thuộc TDD của từng đợt.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
