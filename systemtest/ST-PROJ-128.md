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

# ST-PROJ-128

## System Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-PROJ-128 | STORY-PROJ-002 | Main | SMOKE | P0 | C1 là khách đã đăng nhập, sở hữu bản dự toán D1 mã `BUILDX-20261011-Q7K2M9`: nhà phố 2 tầng, lô 5×20 m, 3 phòng ngủ, gói Tiêu chuẩn, ảnh lô đã soi Đạt, đủ địa chỉ, tọa độ và phong cách. Cấu hình: hạn chờ 2D 20 / Dự toán 20 / 3D 40 / Hồ sơ 5 phút, giới hạn lỗi 3 lần mỗi chặng. Gói C1 còn hiệu lực, lượt sẵn 3/giữ 0/dùng 0. Chưa có lần chạy chặng nào. Chạy với chế độ AI giả phát lại phản hồi ghi sẵn (dự kiến theo TDD đợt 1). Ca chưa chạy. | 1. C1 mở D1, xem thanh tiến trình 4 bước (Nhập liệu → Bản vẽ 2D → Dự toán & 3D → Hồ sơ).<br>2. Bấm Tạo bản vẽ 2D; đọc sổ lượt và trạng thái chặng ngay sau khi tiếp nhận.<br>3. Chờ lần chạy kết thúc; đọc lại sổ lượt.<br>4. Mở màn Bản vẽ 2D và xem trạng thái bước Dự toán & 3D. | Lượt trước: sẵn 3/giữ 0/dùng 0. Phản hồi AI ghi sẵn của phương án 2 tầng (mỗi tầng 1 bản vẽ kỹ thuật, 1 bản vẽ nội thất). | Bước 2: chặng 2D Đang chạy; lượt sẵn 2/giữ 1/dùng 0; đầu vào bị khóa. Bước 3: chặng 2D Thành công, màn hình tự cập nhật trạng thái mà không cần tải lại trang; lượt sẵn 2/giữ 0/dùng 1. Bước 4: thấy phương án từng tầng (tên phòng, diện tích) và 4 bản vẽ (2 kỹ thuật, 2 nội thất). Chặng Dự toán và 3D Sẵn sàng, Hồ sơ Chưa mở; hệ thống không tự chạy Dự toán hay 3D. | STORY-PROJ-002/AC-001<br>STORY-PROJ-002/AC-003<br>STORY-PROJ-002/AC-012<br>STORY-PROJ-002/Main Flow<br>BR-PROJ-010/Then<br>BR-SUB-003/Then<br>STORY-PROJ-003/Main Flow | Luồng chính của chặng 2D với mô hình một lượt cho cả bộ: giữ lượt khi tiếp nhận, tính đã dùng khi 2D thành công, không tự chạy chặng sau. Đặc tả kiểm thử, chưa thực thi; dữ liệu trong ca là dữ liệu thử, không phải mặc định sản phẩm. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-PROJ-002/AC-001
- STORY-PROJ-002/AC-003
- STORY-PROJ-002/AC-012
- STORY-PROJ-002/Main Flow
- BR-PROJ-010/Then
- BR-SUB-003/Then
- STORY-PROJ-003/Main Flow
