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

# BR-PROJ-012

## Rule Info

- **Name**: Mỗi chặng có hạn chờ riêng; quá hạn thì lần chạy lỗi và kết quả muộn bị bỏ.
- **Category**: Tạo dự toán — xử lý theo chặng
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: hạn chờ theo từng chặng, mặc định 2D 20 phút, Dự toán 20 phút, Phối cảnh 3D 40 phút, Hồ sơ 5 phút; Admin cấu hình được. Thời gian đo thực tế khi chạy app demo ngày 11/10/2026: khoảng 15 phút cho cả bộ nhà phố 2 tầng. Người dùng chốt ngày 12/10/2026: quá hạn không tính vào bộ đếm lỗi; từ chối ngay khi hàng chờ đầy; tạm ngắt nhận lần chạy mới khi dịch vụ AI lỗi liên tục; lần chạy tốn chi phí AI quá ngưỡng chỉ gửi cảnh báo; hàng chờ theo thứ tự tiếp nhận.

## Statement

Mỗi lần chạy chặng phải xong trong hạn chờ của chặng đó, tính từ lúc hệ thống tiếp nhận lần chạy. Mặc định: Bản vẽ 2D 20 phút, Dự toán 20 phút, Phối cảnh 3D 40 phút, Hồ sơ 5 phút; Admin cấu hình được. Quá hạn mà chưa có kết quả cuối cùng thì lần chạy lỗi do quá hạn. Kết quả đến sau thời điểm đó không được công bố. Để khách không phải chờ một lần chạy khó thành công, hệ thống từ chối ngay lúc bấm khi hàng chờ đã đầy hoặc dịch vụ AI đang tạm ngắt.

## When

Một lần chạy chặng chưa kết thúc khi hết hạn chờ, có kết quả đến sau khi lần chạy đã lỗi do quá hạn, Admin đổi hạn chờ, khách yêu cầu chạy một chặng khi hệ thống đông hoặc dịch vụ AI đang lỗi liên tục, hoặc một lần chạy kết thúc.

## Then

1. Hệ thống tự ghi nhận lần chạy lỗi do quá hạn, không chờ nhân viên xử lý. Lỗi do quá hạn là lỗi hệ thống nên không tính vào bộ đếm theo BR-PROJ-011 khoản 1.
2. Chặng 2D quá hạn: hoàn lượt đang giữ theo BR-SUB-016 và mở lại khả năng sửa đầu vào theo BR-PROJ-005.
3. Chặng Dự toán, Phối cảnh 3D hoặc Hồ sơ quá hạn: chặng chuyển sang Lỗi; không ảnh hưởng lượt và không ảnh hưởng kết quả của các chặng đã thành công.
4. Kết quả đến sau khi lần chạy đã lỗi do quá hạn không được công bố cho khách, không đổi lần chạy thành thành công và không gắn vào lần chạy lại.
5. Phần kết quả đã tạo và lưu xong trước hạn, chẳng hạn ảnh 3D đã dựng, được giữ để lần chạy lại dùng tiếp theo BR-PROJ-011 khoản 8.
6. Hạn chờ mới do Admin đổi chỉ áp dụng cho lần chạy được tiếp nhận sau khi lưu thay đổi. Lần chạy đang diễn ra giữ hạn đã áp dụng lúc tiếp nhận.
7. Mỗi lời gọi AI trong một lần chạy chỉ được dùng phần thời gian còn lại của hạn chờ; không có bước nào được kéo dài lần chạy quá hạn.
8. **Hàng chờ đầy:** khi số lần chạy đang chờ được xử lý của toàn hệ thống đã đạt ngưỡng Admin cấu hình, yêu cầu chạy chặng mới bị từ chối ngay với thông báo "Hệ thống đang bận, vui lòng thử lại sau vài phút". Không tạo lần chạy, không giữ lượt và không tính vào bộ đếm lỗi.
9. **Tạm ngắt khi dịch vụ AI lỗi liên tục:** khi một dịch vụ AI lỗi liên tục (tài khoản AI hết tiền, khóa truy cập bị từ chối, hoặc số lời gọi lỗi liên tiếp đạt ngưỡng Admin cấu hình), hệ thống tạm ngừng nhận lần chạy mới của các chặng dùng dịch vụ đó trong khoảng thời gian Admin cấu hình. Trong thời gian này, khách bấm chạy được báo ngay "Dịch vụ AI đang tạm gián đoạn, vui lòng thử lại sau"; không tạo lần chạy, không giữ lượt, không tính vào bộ đếm. Yêu cầu soi ảnh lô cũng bị từ chối như vậy. Hệ thống gửi cảnh báo cho Admin lúc bắt đầu và lúc kết thúc tạm ngắt. Hết thời gian tạm ngắt thì nhận lại bình thường; nếu dịch vụ vẫn lỗi thì tạm ngắt tiếp.
10. **Cảnh báo chi phí:** khi một lần chạy kết thúc mà tổng chi phí AI ước tính của nó vượt ngưỡng Admin cấu hình (mặc định 3 USD), hệ thống gửi cảnh báo cho Admin. Lần chạy không bị dừng hay đổi kết quả vì lý do này.
11. Các lần chạy được nhận xử lý theo thứ tự tiếp nhận, không ưu tiên chặng nào.

## Except

Lần chạy đã thành công trước khi hệ thống phát hiện quá hạn không bị đổi thành lỗi. Gói hết hạn không làm lần chạy đang diễn ra lỗi; hạn chờ là điều kiện riêng. Hàng chờ đầy và tạm ngắt chỉ chặn lần chạy mới; lần chạy đã được tiếp nhận vẫn tiếp tục trong hạn chờ của nó.

## Notes

- **Đã xác nhận (11/10/2026):** bốn giá trị hạn chờ mặc định và việc Admin cấu hình được.
- **Đã xác nhận (12/10/2026):** quá hạn không tính vào bộ đếm lỗi; từ chối ngay khi hàng chờ đầy, không giữ lượt; tạm ngắt nhận lần chạy mới khi dịch vụ AI lỗi liên tục, kèm cảnh báo; lần chạy tốn quá ngưỡng chi phí chỉ cảnh báo, không dừng; xử lý theo thứ tự tiếp nhận, không ưu tiên chặng 2D. Người dùng không chọn cách tính hạn chờ từ lúc bắt đầu xử lý, nên hạn chờ vẫn tính từ lúc tiếp nhận.
- **Đề xuất chưa chốt (giá trị mặc định, Admin chỉnh được):** ngưỡng hàng chờ 40 lần chạy đang chờ; tạm ngắt sau 10 lời gọi lỗi liên tiếp hoặc ngay khi tài khoản AI hết tiền, mỗi lần ngắt 5 phút; ngưỡng cảnh báo chi phí 3 USD mỗi lần chạy (app demo tốn khoảng 1,35 USD cho cả bộ).
- Hệ thống rà lần chạy quá hạn định kỳ, mặc định mỗi 30 giây (TDD-PROJ-006). Vì vậy thời điểm ghi nhận lỗi có thể trễ tối đa một chu kỳ rà so với mốc hạn.
- Ví dụ: chặng 3D tiếp nhận lúc 10:00 với hạn 40 phút. Đến 10:40 mới dựng xong 5/8 ảnh thì lần chạy lỗi do quá hạn; 5 ảnh đã lưu được giữ cho lần chạy lại.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
