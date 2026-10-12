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
Thay UT-001 ở heading và Test ID. Trong ô bảng: xuống dòng bằng <br>, dấu | viết thành \|.
Loại: Happy / Branch / Boundary / Error / Quirk / Determinism. Suite: SMOKE / REGRESSION / FULL. Priority: P0 / P1 / P2 / P3.
TEST_LINKS là nguồn liên kết khi import; cột Trace to chỉ để đọc. Giữ hai nơi nhất quán.
Mỗi liên kết dùng DOC-KEY/section: ghi chú. Owner là tên hiển thị; phê duyệt thực hiện sau import.
Expected output và assertion phải suy ra từ Business Rule/contract đã xác nhận. Dữ liệu mock chỉ là dữ liệu kiểm thử minh hoạ, không phải dữ liệu production; không ghi Pass/đã chạy nếu chưa có bằng chứng thực thi.
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
- Module tối đa 200 ký tự; Unit under test tối đa 500 (được dùng làm tiêu đề tài liệu); Owner tối đa 200. Trạng thái parser nhận: Draft / Approved; đây không phải kết quả chạy test hoặc quyền phê duyệt.
- Giữ Loại: Happy / Branch / Boundary / Error / Quirk / Determinism; Suite: SMOKE / REGRESSION / FULL; Priority: P0 / P1 / P2 / P3. Không dùng số ngoài dải Priority 0–3.
- Không xuống dòng vật lý trong ô: dùng <br>; dấu phân cột trong nội dung phải viết \|. TEST_LINKS mới là nguồn liên kết import; cột Trace to chỉ hiển thị, hai nơi phải nhất quán.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
-->

# UT-PROJ-363

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-363 | Tạo dự toán — chặng Dự toán | PriceAnchors — sàn giá, chỉ tiêu từng phần và trần của 18 ô loại neo giá × gói | Determinism | REGRESSION | P0 | Unit dự kiến của TDD-PROJ-009, chưa có mã. Thư viện thuần bmt-be.estimatepipeline/Estimate; ca chuyển sang test/bmt-be.estimatepipeline.tests/Ported/Estimate/. Giá neo, hệ số gói và dải giá đọc từ phiên bản Khung giá PV1 nạp sẵn. | 18 ô: nhà phố (nha-pho, hien-dai), nhà mái (nha-mai, mai-thai-hien-dai), biệt thự hiện đại (villa-biet-thu, hien-dai), biệt thự tân cổ điển (villa-biet-thu, tan-co-dien), cấp 4 trọn gói (nha-vuon-cap4, cap4-hien-dai), căn hộ (can-ho, toi-gian); mỗi loại ba gói Cơ bản, Tiêu chuẩn, VIP; địa chỉ bất kỳ (không có điều chỉnh theo vùng). | (sàn; chỉ tiêu; trần) tr/m² đúng từng số: nhà phố (5,8; thô 3,77 + HT 2,03; 8,21), (6,96; 4,51 + 2,45; 9,4), (8,12; 5,3 + 2,82; 10,96); nhà mái (6,96; 4,66 + 2,3; 9,4), (8,35; 5,54 + 2,81; 11,28), (9,74; 6,51 + 3,23; 13,15); biệt thự hiện đại (10,44; 7,28 + 3,16; 14,09), (12,53; 8,65 + 3,88; 16,91), (14,62; 10,13 + 4,49; 19,73); biệt thự tân cổ điển (8,12; 5,63 + 2,49; 10,96), (9,74; 6,7 + 3,04; 13,15), (11,37; 7,83 + 3,54; 15,35); cấp 4 trọn gói (5; 2,38 + 1,27 + NT 1,35; 9,13), (6; 2,78 + 1,5 + 1,72; 9,25), (7; 3,29 + 1,72 + 1,99; 9,45); căn hộ (5; HT 1,96 + NT 3,04; 9,03), (6; 2,3 + 3,7; 9,27), (7; 2,67 + 4,33; 9,45). neo = AbsoluteTrM2 hoặc neo nhà phố × TownhouseFactor, neo gói = neo × AnchorFactor tính bằng double; sàn = neo gói làm tròn 2 số lẻ; trần = max(neo gói chưa làm tròn, tổng đầu cao dải) × 1,35 làm tròn 2 số lẻ. Không hạ 5% cho vùng nào.<br>Tên test: `The_enforced_minimum_of_every_cell_is_exactly_what_it_was`. | TDD-PROJ-009/Architecture<br>TDD-PROJ-009/Data Model<br>BR-PROJ-016/Then<br>BR-PROJ-013/Then<br>BR-PROJ-017/Then | Mục F và quyết định 4 của mục R (giá neo là dữ liệu có kiểu của phiên bản); neo_san_tr_m2 (dutoan.py:1254–1303). Chuyển từ tests/estimate/test_estimate_price_invariants.py::test_the_enforced_minimum_of_every_cell_is_exactly_what_it_was (bảng MUC_TOI_THIEU_TR_M2). Bỏ nhánh hạ 5% của app demo theo quyết định người dùng 12/10/2026. Đặc tả unit test dự kiến, chưa có mã và chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-009/Architecture
- TDD-PROJ-009/Data Model
- BR-PROJ-016/Then
- BR-PROJ-013/Then
- BR-PROJ-017/Then
