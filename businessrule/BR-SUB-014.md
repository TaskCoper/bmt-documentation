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


# BR-SUB-014

## Rule Info

- **Name**: Kỳ thiết kế tính từ ngày bắt đầu, không theo lịch tháng/năm chung.
- **Category**: Subscription thiết kế
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chọn tính kỳ từ ngày bắt đầu sử dụng: tháng kết thúc cùng ngày tháng sau, năm kết thúc cùng ngày năm sau; nếu không có ngày tương ứng thì lấy ngày cuối tháng. Người dùng chọn hết hạn đúng giờ bắt đầu theo giờ Việt Nam.

## Statement

Kỳ subscription thiết kế tính từ ngày bắt đầu có hiệu lực của gói. Kỳ tháng có ngày kết thúc là cùng ngày trong tháng kế tiếp; kỳ năm có ngày kết thúc là cùng ngày, cùng tháng của năm kế tiếp. Nếu tháng/năm đích không có ngày đó, dùng ngày cuối tháng đích. Giờ kết thúc trùng giờ bắt đầu theo giờ Việt Nam; không kéo dài đến hết ngày kết thúc.

## When

Hệ thống xác định ngày kết thúc của một kỳ subscription thiết kế hợp lệ từ ngày bắt đầu và lựa chọn chu kỳ tháng hoặc năm đã ghi nhận cho subscription theo [BR-SUB-015](BR-SUB-015.md).

## Then

1. Với kỳ tháng, cộng một tháng theo lịch và giữ ngày trong tháng nếu ngày đó tồn tại; nếu không, lấy ngày cuối tháng đích.
2. Với kỳ năm, cộng một năm theo lịch và giữ ngày/tháng nếu tồn tại; ngày 29/02 chuyển thành 28/02 khi năm đích không nhuận.
3. Không rút ngắn kỳ đầu về cuối tháng hoặc cuối năm chung. Không thay một tháng bằng số ngày cố định hoặc một năm bằng 365 ngày cố định.
4. Việc xác định ngày kết thúc không tự tạo kỳ tiếp theo hoặc tự gia hạn.

5. Giữ nguyên giờ, phút và giây của thời điểm bắt đầu khi xác định thời điểm kết thúc theo giờ Việt Nam (Asia/Ho_Chi_Minh).
6. Kỳ có hiệu lực từ thời điểm bắt đầu đến trước thời điểm kết thúc. Đúng thời điểm kết thúc, kỳ cũ đã hết hạn; yêu cầu mới cần quyền subscription không được dùng kỳ cũ. Tác vụ đã bắt đầu hợp lệ tuân theo quy tắc đã chốt, không bị hủy chỉ vì hết hạn.

## Except

Chỉ áp dụng cho kỳ thiết kế. Gói giám sát có hạn gán lần đầu một năm theo [BR-SUB-022](BR-SUB-022.md); quy ước biên của hạn gán đã chốt riêng trong BR-SUB-022, không lấy từ quy tắc này.

## Notes

- Khi đổi gói hoặc chu kỳ hoàn tất, kỳ mới tính từ thời điểm nâng theo [BR-SUB-021](BR-SUB-021.md), không giữ mốc hết hạn cũ. Quy tắc lịch ở đây không quyết định tiền chênh lệch hoặc cách xử lý lượt khi nâng.

- Ví dụ kỳ tháng: 15/09/2026 → 15/10/2026; 31/01/2027 → 28/02/2027; 31/01/2028 → 29/02/2028.
- Ví dụ kỳ năm: 15/09/2026 → 15/09/2027; 29/02/2028 → 28/02/2029.
- Ngày bắt đầu là ngày gói có hiệu lực, không mặc định là lần đầu khách thực hiện thao tác. Kỳ bắt đầu lúc hệ thống thực sự cấp gói theo BR-PAY-004; thời điểm đủ tiền dùng để xét hạn thanh toán và thứ tự mua, không tự tính lùi kỳ sử dụng.
- Ví dụ giờ Việt Nam: bắt đầu 14:30 ngày 15/09/2026 thì kỳ tháng hết hạn lúc 14:30 ngày 15/10/2026. Nếu bắt đầu 14:30 ngày 31/01/2027, hết hạn lúc 14:30 ngày 28/02/2027. Không cộng thêm thời gian đến hết ngày.
- Khi gia hạn qua tháng ngắn, có giữ mốc ngày đăng ký ban đầu cho các kỳ sau hay lấy ngày đã điều chỉnh của kỳ trước sẽ chốt trong luồng gia hạn; chưa suy ra từ ví dụ một kỳ.
- Tham chiếu [BR-SUB-002](BR-SUB-002.md), [STORY-SUB-001](../userstory/STORY-SUB-001.md), [ST-SUB-036](../systemtest/ST-SUB-036.md) và [ST-SUB-037](../systemtest/ST-SUB-037.md).
- [ST-SUB-038](../systemtest/ST-SUB-038.md) kiểm tra trước và đúng thời điểm hết hạn.
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
