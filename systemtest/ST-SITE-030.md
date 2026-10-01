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

# ST-SITE-030

## System Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-SITE-030 | STORY-SITE-001 | ALT | REGRESSION | P1 | Khách U1 có công trình A từng có gói G1, nay G1 đã bị nhân viên hủy; công trình B từng có gói G2, nay G2 đã được nhân viên gỡ khỏi B và đang chưa gán. A và B không có gói giữ chỗ. U1 đang đăng nhập. Hồ sơ thử được chuẩn bị đủ trường mới theo dữ liệu nền; giữ các quyền/gói của riêng ca. | 1. Ghi bản chụp lịch sử hủy G1 và gỡ G2.<br>2. Đổi số nhà–đường của A từ 12 Đường A sang 30 Đường B, giữ tỉnh/xã và gửi tọa độ mới (11,106).<br>3. Đổi tên B từ Nhà vườn sang Nhà vườn mới bằng body đủ trường hiện có.<br>4. Đọc hồ sơ và đối chiếu bản chụp lịch sử. | Dữ liệu nền (nguồn, tệp, gói và trường thay đổi lấy theo Precondition/Steps của từng ca): tên riêng không trùng; diện tích đất 100.25 m²; hiện trạng Đất trống đang cho chọn; ngân sách 2000000000 VND; khởi công Trong 1–3 tháng tới; Nhà phố, 3 tầng, Không tum, một phong cách kiến trúc và một nội thất hợp lệ theo cấu hình R1; địa chỉ P1/X1/12 Đường A; tọa độ (10,106); không tệp. P1/X1 là bí danh cho mã tỉnh/xã hợp lệ, X1 thuộc P1; chuẩn bị danh mục và nguồn địa chỉ thử, không gửi bí danh làm mã API. Các trường không được nêu là thiếu/sai luôn đầy đủ; riêng ca thiếu tọa độ bỏ đúng trường đang kiểm.<br>A/B được tạo độc lập, không có gói giữ chỗ. Lịch sử G1/G2 được tạo bằng thao tác hủy/gỡ hợp lệ trong bước chuẩn bị, chụp tên/địa chỉ đầy đủ trước thao tác sửa. | Cả hai lần sửa thành công. A lưu số nhà–đường và tọa độ mới, B lưu tên mới; các trường khác giữ nguyên. Tên và địa chỉ trong sự kiện hủy/gỡ giữ đúng bản chụp trước sửa. | STORY-SITE-001/AC-017<br>STORY-SITE-001/ALT-02<br>BR-SITE-002/Then<br>BR-SUB-024/Then<br>BR-SUB-026/Then | Gói đã hủy hoặc đã gỡ không khóa việc sửa công trình; lịch sử gói giữ thông tin công trình tại lúc hủy hoặc gỡ (quyết định ngày 25/09/2026). Đặc tả chưa chạy. Đã cập nhật fixture hồ sơ mở rộng sau khi người dùng chốt US/BR ngày 01/10/2026; chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-SITE-001/AC-017
- STORY-SITE-001/ALT-02
- BR-SITE-002/Then
- BR-SUB-024/Then
- BR-SUB-026/Then
