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

# UT-PROJ-373

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-373 | Tạo dự toán — chặng Dự toán | PriceFloorNet.RaisePrices — tự nâng đơn giá tới 100% neo, giữ nguyên khối lượng | Happy | SMOKE | P0 | Unit dự kiến của TDD-PROJ-009, chưa có mã. Thư viện thuần bmt-be.estimatepipeline/Estimate; ca chuyển sang test/bmt-be.estimatepipeline.tests/Ported/Estimate/. Giá neo, hệ số gói và dải giá đọc từ phiên bản Khung giá PV1 nạp sẵn. Dữ liệu dự toán mẫu demo_du_lieu của app demo được chép thành fixture của test (nhánh demo không được chuyển sang hệ thống thật). | (a) Nhà phố hiện đại Tiêu chuẩn, đơn giá dữ liệu mẫu × 0,8, tổng sàn 300. (b) Ví dụ ST-PROJ-142: nhà phố Tiêu chuẩn, đơn giá m² ban đầu 4,42. (c) nhan_don_gia_phan với hệ số 1,2 trên hai dòng: VT 300 / NC 200 và VT 300.000 / NC 200.000, khối lượng 10. | (a) don_gia_cuoi = sàn = 6,96, trang_thai_cuoi=dat, không cảnh báo đỏ; tu_nang_kieu="don_gia", tu_nang_ngoai_khung=true, ngoai_tam_khung=true (6,96 > 6,19), can_ky_su_duyet=true; hệ số cần < 6, không chạm trần ×6; lưu ý có "CAO HƠN mức tối đa khung đơn giá mục 3–5" và "GIỮ NGUYÊN khối lượng theo bản vẽ". Mọi khối lượng giữ nguyên; dòng được nâng có giá cao hơn và ghi chú "đơn giá theo mặt bằng giá thị trường hiện nay"; dòng vật liệu cơ bản (tên bỏ dấu chứa "be tong", "thep xay dung", "cop pha", "dao dat", "lap dat"…) giữ giá và không có ghi chú; số dòng trong tu_nang_dong = tu_nang_so_dong > 0; ngoai_khung_tr = round((don_gia_cuoi − 6,19) × 300) và 0 < ngoai_khung_tr < tu_nang_tien_them_tr; không ô gửi khách nào chứa "hệ NÂNG", "Hệ thống đã nâng", "neo mục 2", "giá neo". (b) tu_nang_he_so_can = 1,575, đạt sàn sau khi nâng, khối lượng giữ nguyên, tu_nang_do_gia_thap=true. (c) so_dong=2; dòng lẻ giữ VT ≥ 300, NC ≥ 200; dòng thường thành 360.000 / 240.000; cả hai có ghi chú trên.<br>Tên test: `The_auto_raise_reaches_the_anchor_through_prices_and_never_through_quantity`. | TDD-PROJ-009/Architecture<br>STORY-PROJ-008/AC-004<br>BR-PROJ-017/Except<br>ST-PROJ-142/System Test | Mục F "Dưới sàn → tự nâng đơn giá" và quyết định 3 của mục R (giữ câu ghi chú gửi khách). nhan_don_gia_phan, HANG_HOA_KHONG_NANG (dutoan.py:1585–1734, 1995–2110). Chuyển từ tests/estimate/test_estimate_quantity.py::test_the_auto_raise_reaches_the_anchor_through_prices_and_never_through_quantity và ::test_the_price_raise_only_ever_raises_even_on_a_price_below_one_thousand_dong; ca (b) theo ví dụ ở mục F. Đặc tả unit test dự kiến, chưa có mã và chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-009/Architecture
- STORY-PROJ-008/AC-004
- BR-PROJ-017/Except
- ST-PROJ-142/System Test
