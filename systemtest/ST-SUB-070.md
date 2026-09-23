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

<!-- Mỗi file chứa một test, đúng một dòng dữ liệu và 13 cột theo thứ tự bên dưới.
Thay ST-001 ở heading và Test ID, STORY-001 bằng mã Story cần kiểm thử.
Trong ô bảng: xuống dòng bằng <br>, dấu | viết thành \|. Steps gồm các bước đánh số.
Loại: Main / ALT / EXC / NFR / Integration boundary; ghép nhiều loại bằng dấu /.
Suite: SMOKE / REGRESSION / FULL. Priority: P0 / P1 / P2 / P3.
TEST_LINKS là nguồn liên kết khi import; cột Trace to chỉ để đọc. Giữ hai nơi nhất quán.
Owner là tên hiển thị; phê duyệt thực hiện sau import.
Steps và Expected result phải bám Flow, AC và Business Rule của Story có thật. Test data là dữ liệu kiểm thử minh hoạ; không tự thêm hành vi nghiệp vụ hoặc ghi Pass/đã chạy nếu chưa có bằng chứng thực thi.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Bảng phải có header, dòng phân cách và đúng một dòng dữ liệu, đủ 13 cột theo thứ tự mẫu. Reviewer/Approver là hai bullet trước bảng, không thêm thành cột. Test ID trong bảng phải nhất quán với heading; mã tài liệu import lấy từ heading.
- Story tối đa 100 ký tự và được dùng làm tiêu đề tài liệu; Owner tối đa 200. Trạng thái parser nhận: Draft / Approved; đây không phải kết quả chạy test hoặc quyền phê duyệt.
- Giữ Loại: Main / ALT / EXC / NFR / Integration boundary, có thể ghép bằng dấu /; Suite: SMOKE / REGRESSION / FULL; Priority: P0 / P1 / P2 / P3. Không dùng số ngoài dải Priority 0–3.
- Steps là các bước đánh số trong một ô, tách bằng <br>; không xuống dòng vật lý trong ô, dấu | trong nội dung phải viết \|. TEST_LINKS mới là nguồn liên kết import; cột Trace to chỉ hiển thị, hai nơi phải nhất quán.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
-->

# ST-SUB-070

## System Test

**Giới hạn phạm vi:** tiêu chí trả đủ bộ kết quả cùng lúc và tính một lượt vẫn giữ. Phần chọn đầu ra 3D theo quyền gói chưa triển khai; không dùng phần đó để nghiệm thu đợt này theo STORY-SUB-002/AC-025.

- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-SUB-070 | STORY-SUB-001 | Main | REGRESSION | P1 | Tài khoản có kỳ thiết kế còn hiệu lực, có quyền tạo mới và 3D chân thực; còn 2 lượt tạo; dự án chưa có kết quả thành công. Chuẩn bị tác vụ trả đủ bộ kết quả hợp lệ gồm 3D, trong thời gian chờ cho phép. | 1. Bấm Gen AI một lần.<br>2. Khi tác vụ còn xử lý, kiểm tra lượt giữ và kết quả khách được xem.<br>3. Cho tác vụ hoàn tất đủ bộ kết quả, lưu thành công và có thể mở xem.<br>4. Mở kết quả, kiểm tra các phần được trả cùng lúc và số lượt. | Một yêu cầu tạo thiết kế; 2 lượt ban đầu. Phối cảnh 3D nằm trong cùng bộ kết quả, không có yêu cầu tạo 3D riêng. Danh sách các phần đầu ra còn lại theo đặc tả tính năng khi được chốt. | Khi xử lý: giữ 1 lượt, chưa tính đã dùng, không trả một phần như kết quả thành công. Khi đủ kết quả: trả cùng lúc các phần gồm 3D, chuyển đúng 1 lượt giữ thành đã dùng, còn 1 lượt tạo sẵn dùng. Không yêu cầu bấm tạo 3D riêng hoặc tính thêm lượt 3D; lượt tra cứu không đổi. | STORY-SUB-001/AC-040<br>BR-SUB-003/Then<br>BR-SUB-017/Then | Kiểm tra một lần tạo và một bộ kết quả, tránh tách 3D thành tác vụ tính lượt riêng. Đặc tả nháp, chưa chạy; dữ liệu đầu ra đầy đủ phụ thuộc đặc tả tính năng. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-SUB-001/AC-040
- BR-SUB-003/Then
- BR-SUB-017/Then
