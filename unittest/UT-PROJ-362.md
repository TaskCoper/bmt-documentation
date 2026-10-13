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

# UT-PROJ-362

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-362 | Tạo dự toán — chặng Dự toán | EstimateStageExecutor — mã lỗi cuối khi kiểm trường bắt buộc thất bại | Branch | REGRESSION | P0 | Unit dự kiến của TDD-PROJ-009, chưa có mã. Executor với IEstimateTurns giả; lượt chính trả tool_use nhưng kết quả cuối không qua validate_du_lieu. | (a) Lượt hỏi lại phân tích bị cắt ở 4.000 token (AiOutputTruncated) và vẫn thiếu trường (như hồ sơ 82UWR4). (b) Lượt hỏi lại lỗi dịch vụ đã hết lần thử. (c) Lượt hỏi lại hết tiền. (d) Lượt hỏi lại sai khóa. (e) Lượt hỏi lại trả đủ nhưng AI vẫn thiếu trường. (f) Lượt hỏi lại không trả tool_use. (g) bang_ke_phong 20 ký tự nên không hỏi lại bóc tách, thiếu noi_that. (h) Thiếu noi_that nên lưới phân tích không chạy, thiếu trường phân tích. (i) tong_san_so có nhưng bằng 0. (j) Lượt hỏi lại trả HTTP 400 (AiRequestInvalid). (k) Lượt hỏi lại trả stop_reason=refusal. (l) Lượt hỏi lại phân tích bị cắt và vẫn thiếu trường, sau đó giá trên trần nên có lượt rà giá, lượt này lỗi dịch vụ đã hết lần thử; chạy lần đầu rồi nhận lại từ điểm lưu. (m) Lượt hỏi lại rà giá lỗi dịch vụ nhưng kết quả cuối đủ trường bắt buộc. | (a) AiOutputTruncated, CountsTowardLimit=false. (b) AiProviderUnavailable, false. (c) AiOutOfCredit, false. (d) AiProviderUnavailable, false. (e)–(i) AiOutputIncomplete, CountsTowardLimit=true. (j) AiProviderUnavailable, false. (k) AiOutputIncomplete, true. (l) AiOutputTruncated (lượt lỗi hệ thống đầu tiên theo thứ tự chạy: lưới phân tích chạy trước lưới sàn giá), false, ở cả lần đầu và lần nhận lại. Với (a)–(l): không công bố dự toán dở dang; không ghi EstimateCostResult. (m) Lần chạy Succeeded, không có FailureCode; lượt lỗi được ghi bước failed và ghi chú nội bộ.<br>Tên test: `ExecuteAsync_ValidationFailure_UsesReaskSystemCodeOrCountsIncomplete`. | TDD-PROJ-009/Architecture<br>BR-PROJ-011/Then<br>STORY-PROJ-008/EXC-02<br>STORY-PROJ-008/AC-006<br>ST-PROJ-144/System Test<br>ST-PROJ-201/System Test | Bảng mã lỗi cuối của mục E và quyết định 9 của mục R (người dùng chốt 12/10/2026, chọn "Không tính, lỗi hệ thống (Đề xuất)"): lượt hỏi lại lỗi không tự đặt mã lỗi; khi kiểm trường bắt buộc thất bại mà có lượt hỏi lại lỗi vì hệ thống thì lấy mã hệ thống của lượt lỗi đầu tiên theo thứ tự chạy, ánh xạ như lượt chính (MapAiCode), không tính (BR-PROJ-011 khoản 1) — ca (a)–(d), (j), (l); refusal và không có tool_use ở lượt hỏi lại thì AiOutputIncomplete có tính — ca (f), (k); các trường hợp còn lại tính theo quyết định 7. Ví dụ 82UWR4 ở mục E. Ca (b) tương ứng tests/estimate/test_estimate_pipeline_nets.py::test_failed_reask_ends_in_validation_failure_with_the_raw_log_filled (app demo dừng ở estimate_validation_failed; hệ thống thật thêm mã lỗi cuối theo bảng của mục E). Đặc tả unit test dự kiến, chưa có mã và chưa chạy. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-009/Architecture
- BR-PROJ-011/Then
- STORY-PROJ-008/EXC-02
- STORY-PROJ-008/AC-006
- ST-PROJ-144/System Test
- ST-PROJ-201/System Test
