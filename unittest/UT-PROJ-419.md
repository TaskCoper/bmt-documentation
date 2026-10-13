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

# UT-PROJ-419

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-419 | Tạo dự toán — chặng Dự toán | EstimateResultProjection — dựng hợp đồng estimate-v1 từ CustomerJson | Happy | SMOKE | P0 | Unit dự kiến của TDD-PROJ-009, chưa có mã. Application thuần. | R2 Succeeded lúc 2026-10-11T05:03:21Z, estimateDateVn 2026-10-11, tong_san_so 270. Dòng thô trong nhóm "I. PHẦN MÓNG & NỀN": "Bê tông móng M250", m³, khối lượng 18,5, VT 1.500.000, NC 380.000, vật liệu ghép "Bê tông thương phẩm M250 — Lê Phan (hoặc tương đương)". Dòng nội thất nhóm "TẦNG 1": "Tủ bếp dưới + trên", kích thước "3200×600×850 mm", md, 3,2, đơn giá 5.200.000. Tổng: thô 1.300.000.000, hoàn thiện 581.900.000, nội thất 486.000.000. thoi_gian_tho "~95 ngày", thoi_gian_hoan_thien "~55 ngày", thoi_gian_noi_that "~40 ngày sản xuất song song, 10 ngày lắp", thoi_gian_tong "~5,5 tháng"; van_tu_van "Kính gửi Anh/Chị, …". Thêm hai ca chỉ đổi tổng và tổng sàn để kiểm làm tròn pricePerM2Vnd: (b) tổng 2.367.900.135, tong_san_so 270; (c) tổng 2.367.900.000, tong_san_so 270,5. | Ca chính: khóa cấp 1 đúng: runId, contractVersion="estimate-v1", completedAtUtc, estimateDateVn="2026-10-11", convertedFloorAreaM2=270, totals {structure 1.300.000.000, finishing 581.900.000, interior 486.000.000, grandTotal 2.367.900.000}, pricePerM2Vnd=8.770.000 (tổng ÷ tong_san_so), parts, schedule {structure, finishing, interior, total} lấy từ bốn ô thời gian, advice = van_tu_van. parts theo thứ tự Structure "Phần thô", Finishing "Hoàn thiện", Interior "Nội thất", mỗi phần có total và groups; tên nhóm "PHẦN MÓNG & NỀN" (bỏ số thứ tự). Dòng thô: size=null, unitPrice=1.880.000 (VT + NC đã làm tròn), amount=34.780.000; dòng nội thất: size="3200×600×850 mm", unitPrice=5.200.000, amount=16.640.000. Không có ghi chú dòng, khối phân tích, nhận xét, cờ hay cảnh báo. (b) pricePerM2Vnd = 8.770.001 (8.770.000,5 làm tròn nửa xa số 0; làm tròn nửa về số chẵn sẽ sai thành 8.770.000). (c) pricePerM2Vnd = 8.753.789 (từ 8.753.789,28…).<br>Tên test: `Project_CustomerJsonToEstimateV1Contract`. | TDD-PROJ-009/Internal API<br>TDD-PROJ-009/Architecture<br>STORY-PROJ-008/Main Flow<br>STORY-PROJ-008/AC-003<br>BR-PROJ-007/Then<br>ST-PROJ-140/System Test | Mục P và ví dụ Response 200 của GET …/result; đơn giá và thành tiền theo gia_dong (service.py:1855–1879), tên phần theo PART_TITLES (service.py:77); đơn giá m² sàn quy đổi = tổng ÷ tong_san_so, làm tròn nửa xa số 0 tới đồng như ô D22 =IFERROR(D20/D21,"—") định dạng #,##0 của resources/excel/template.xlsx (mục G). Mỗi mốc thời gian lấy đúng ô của phần đó như tests/estimate/test_estimate.py::test_the_api_names_each_phase_with_its_own_schedule. Đặc tả unit test dự kiến, chưa có mã và chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-009/Internal API
- TDD-PROJ-009/Architecture
- STORY-PROJ-008/Main Flow
- STORY-PROJ-008/AC-003
- BR-PROJ-007/Then
- ST-PROJ-140/System Test
