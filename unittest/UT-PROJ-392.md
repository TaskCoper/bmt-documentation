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

# UT-PROJ-392

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-392 | Tạo dự toán — luật giá | SavePriceRuleDocumentCommandHandler — nội dung không hợp lệ bị từ chối, chỉ rõ mục và dòng | Error | REGRESSION | P0 | Unit dự kiến của TDD-PROJ-009, chưa có mã. Store giả; PriceBandDeriver thật; con trỏ PriceFramework ở PV1. | (a) content rỗng. (b) content 200.001 ký tự. (c) Khung giá xóa dòng "Ốp gạch tường WC / bếp" ở mục 4. (c2) Đổi tên dòng đó thành "Ốp gạch tường WC/bếp". (d) Dòng "Tủ quần áo MDF" ở mục 5 ghi "5.000.000 – 2.800.000". (e) Mất tiêu đề "## 3." (đổi thành "## Kết cấu"). (f) Quy tắc bóc tách rỗng. (g) Ô NC của dòng "Bê tông M250 (móng/cột/dầm/sàn)" ở mục 3 ghi "liên hệ". (h) Nội dung gốc, VT của dòng "Đào đất móng (máy + thủ công)" vẫn là "0". | Ca (a)–(g): 422 do ValidationException, title "Validation Failure", code "ValidationFailure", messageCode InvalidPriceRuleDocument, errors là mảng; không thêm phiên bản, con trỏ và dải giá giữ nguyên. (a), (b), (f): lỗi của trường content. (c) và (c2): PropertyName "content.section4[Ốp gạch tường WC / bếp]", ErrorMessage "Không tìm thấy dòng 'Ốp gạch tường WC / bếp' ở mục 4; giữ đúng tên dòng như phiên bản hiện hành.". (d) PropertyName "content.section5[Tủ quần áo MDF]", ErrorMessage "Dải giá 'Tủ quần áo MDF' có giá thấp lớn hơn giá cao.". (e) Mỗi dòng cần thiết của mục 3 có một lỗi "Không tìm thấy dòng '{tên dòng}' ở mục 3; giữ đúng tên dòng như phiên bản hiện hành.", ví dụ dòng "Bê tông M250 (móng/cột/dầm/sàn)". (g) PropertyName "content.section3[Bê tông M250 (móng/cột/dầm/sàn)]", ErrorMessage "Không đọc được khoảng giá của dòng 'Bê tông M250 (móng/cột/dầm/sàn)' ở mục 3.". (h) Không có lỗi cho dòng đào đất; lưu thành công.<br>Tên test: `Handle_InvalidContent_ThrowsInvalidPriceRuleDocumentWithSectionAndRow`. | TDD-PROJ-009/Architecture<br>TDD-PROJ-009/Error Codes<br>TDD-PROJ-009/Internal API<br>BR-PROJ-016/Then<br>STORY-PROJ-011/AC-004<br>STORY-PROJ-011/EXC-02<br>ST-PROJ-184/System Test | Mục H "Lưu" và "Thông báo lỗi 422" (câu của ca g theo quyết định 8 của mục R), BR-PROJ-016 khoản 4; giới hạn 200.000 ký tự ở mục L và Data Model. Tên dòng là tên bộ tính dùng (derive_price_bands.py:260, 262, 322, 360); bảng mục 3–5 được tìm theo tiêu đề "## {N}." như doc_khung_don_gia (derive_price_bands.py:115–117); ô "0" đọc được như _khoang_tien (derive_price_bands.py:93–102). Hình dạng lỗi (c) theo ví dụ của preview ở Internal API; app demo yêu cầu mọi dòng có giá thấp nhỏ hơn giá cao (tests/estimate/test_price_band_derivation.py::test_every_priced_row_has_a_real_low_end_and_an_apartment_has_no_rough_band). Đặc tả unit test dự kiến, chưa có mã và chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-009/Architecture
- TDD-PROJ-009/Error Codes
- TDD-PROJ-009/Internal API
- BR-PROJ-016/Then
- STORY-PROJ-011/AC-004
- STORY-PROJ-011/EXC-02
- ST-PROJ-184/System Test
