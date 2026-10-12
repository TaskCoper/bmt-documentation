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

# BR-PROJ-014

## Rule Info

- **Name**: Mỗi kết quả chặng ghi lại model AI, phiên bản prompt, phiên bản bộ quy tắc, luật giá và cấu hình đã dùng.
- **Category**: Tạo dự toán — cấu hình AI và truy vết
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: Admin đổi model AI và tham số chất lượng trên giao diện; mỗi kết quả ghi lại model và phiên bản prompt; luật giá có lịch sử phiên bản; chuyển đúng hành vi app demo trước, sửa chất lượng sau. Cùng ngày người dùng đổi quyết định “prompt nằm trong mã nguồn”: Admin sửa phần lời dặn của prompt trên giao diện, có chạy thử.

## Statement

Admin cấu hình model AI và tham số chất lượng cho từng việc của luồng tạo dự toán. Mỗi prompt gồm phần cấu trúc dữ liệu AI phải trả (nằm trong mã nguồn) và phần lời dặn (Admin sửa trên giao diện, có phiên bản). Mỗi lần chạy chặng chốt cấu hình, phiên bản prompt và bộ quy tắc AI (BR-PROJ-019) đang áp dụng lúc tiếp nhận; kết quả ghi lại các phiên bản này cùng phiên bản luật giá với chặng Dự toán. Thay đổi về sau không làm đổi kết quả đã có.

## When

Admin lưu cấu hình AI hoặc luật giá; hệ thống tiếp nhận một lần chạy chặng; người có quyền xem chi tiết kết quả của một bản dự toán.

## Then

1. Admin có quyền tương ứng được chọn model và tham số chất lượng cho từng việc (soi ảnh lô, lập phương án 2D, vẽ bản vẽ, lập dự toán, dựng 3D) trong danh sách model hệ thống hỗ trợ. Không nhận model nằm ngoài danh sách.
2. Mỗi lần lưu cấu hình tạo một phiên bản mới; phiên bản cũ được giữ để tra cứu. Hai người lưu cùng lúc dựa trên cùng phiên bản thì người lưu sau bị từ chối và phải tải lại.
3. Lần chạy chặng dùng phiên bản cấu hình đang áp dụng lúc tiếp nhận cho tới khi kết thúc, kể cả lúc tự làm lại theo BR-PROJ-011. Cấu hình mới áp dụng cho lần chạy tiếp nhận sau đó.
4. Mỗi prompt có một mã (ví dụ lập phương án 2D, lập dự toán, dựng ảnh phòng). Admin có quyền tương ứng sửa **phần lời dặn** của prompt; phần cấu trúc dữ liệu AI phải trả (các trường bắt buộc, công cụ `submit_estimate`, json_schema) chỉ đổi qua mã nguồn. Trước khi áp dụng, Admin xem trước prompt đã ghép đủ và chạy thử trên một bản dự toán mẫu; bản lưu tạo phiên bản mới, có so sánh và khôi phục phiên bản cũ như BR-PROJ-016 khoản 2, 7. Prompt nạp sẵn là bản của app demo ở mốc chuyển đổi.
5. Kết quả mỗi chặng lưu: model đã gọi cho từng việc, phiên bản của từng prompt đã dùng, phiên bản bộ quy tắc AI, phiên bản cấu hình; chặng Dự toán lưu thêm phiên bản của hai tệp luật giá theo BR-PROJ-016 và miền giá đã chốt theo BR-PROJ-013.
6. Đổi cấu hình, prompt, bộ quy tắc AI hoặc luật giá không làm đổi, tính lại hay xóa kết quả đã lưu của bất kỳ chặng nào.
7. Khách không thấy tên model, phiên bản prompt hay chi phí AI. Chỉ người có quyền xem chi tiết luồng AI mới thấy các thông tin này.

## Except

Chế độ chạy thử (Fake) dùng phản hồi ghi sẵn thay cho lời gọi AI thật; kết quả vẫn ghi phiên bản prompt và cấu hình, còn tên model ghi là bản giả. Chế độ này không bật ở production.

## Notes

- **Đổi quyết định ngày 11/10/2026:** trước đó chốt “prompt nằm trong mã nguồn, không sửa trên giao diện”; nay Admin sửa phần lời dặn, có xem trước và chạy thử (khoản 4). Màn quản trị prompt làm ở đợt sau đợt 4 (STORY-PROJ-013); trước đó prompt nạp sẵn từ app demo và chỉ đổi qua bản phát hành.
- **Đã xác nhận (11/10/2026):** Admin đổi model và tham số trên giao diện; prompt có phiên bản; ghi model và phiên bản prompt vào kết quả; luật giá có lịch sử và mỗi bản ghi phiên bản luật đã dùng.
- Mốc chuyển đổi ban đầu là app demo ở commit `059e876`, prompt `2d-13` cho 2D và `dt-14` cho dự toán. Các lỗi chất lượng đã thấy (tự thêm giếng trời, `wc_count = 0`, dòng mái bị loại vì 0 đồng) được sửa sau bằng phiên bản prompt hoặc luật mới, không sửa ngầm trong lần chuyển mã.
- Danh sách tham số chất lượng, model được hỗ trợ và cờ khả năng của từng model thiết kế ở TDD-PROJ-008/Architecture.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
