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

# ST-PROJ-072

## System Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-PROJ-072 | STORY-PROJ-002 | NFR / Integration boundary | REGRESSION | P0 | Môi trường thử đã có chức năng tương ứng; C1 là khách đã đăng nhập, sở hữu bản dự toán; gói còn hiệu lực, quyền tạo thiết kế, quota hữu hạn 3, đã dùng 0, đang giữ 0, trừ khi ca nêu khác. Có thể đọc lại dữ liệu đã lưu, trạng thái tác vụ và sổ lượt. Có ba bản cài đặt API cùng một database thử: Staging với `EstimateAiOption__Mode=Mock` và năm URL tệp mẫu đã upload lên kho presign (tên máy chủ có trong `UploadedFileOption__AllowedHosts`); Staging không đặt Mode; và một lần khởi động với `ASPNETCORE_ENVIRONMENT=Production` cùng `EstimateAiOption__Mode=Mock`. Job quá hạn quét mỗi 60 giây. | 1. Khởi động bản Production có Mode=Mock; đọc log khởi động.<br>2. Trên bản không đặt Mode, C1 bấm Nhận dự toán với D1 đủ đầu vào; đọc tác vụ và sổ lượt.<br>3. Trên bản Mock, C1 bấm Nhận dự toán với D1; đọc trạng thái ngay sau khi nhận rồi sau vài lượt quét của worker.<br>4. Đọc sổ lượt, danh sách tệp kết quả đã lưu và trạng thái bản dự toán; thử lưu đầu vào D1.<br>5. Đổi một URL tệp mẫu thành URL chưa có tệp, gửi D2 và chờ quá thời hạn. | D1, D2 đủ đầu vào, InputVersion=7. Bộ kết quả mẫu mock-v1; năm tệp mẫu (ảnh bìa, mặt bằng 2D, phối cảnh, PDF, Excel) do người vận hành upload. Không dùng AI thật. | Bước 1: API Production khởi động với Mock; lặp bước 3–4 trên bản Production bằng database thử và bộ tệp mẫu như Staging, nhận cùng bộ kết quả mẫu và quyết toán một lượt. Bước 2: 503 DependencyUnavailable, không có tác vụ, sẵn 3/giữ 0/dùng 0. Bước 3: 202 với state Pending; sau khi worker gửi và nhận kết quả, GET trả Succeeded kèm resultUrl là route của backend, không có URL tệp gốc. Bước 4: sẵn 2/giữ 0/dùng 1; có năm tệp và bảng dự toán ghi rõ dữ liệu mẫu; lưu đầu vào bị từ chối EstimateAlreadyGenerated. Bước 5: D2 không thành công, hết hạn với failureCode ResultFileUnavailable, lượt được trả. | STORY-PROJ-002/AC-001<br>STORY-PROJ-002/AC-003<br>STORY-PROJ-002/EXC-01<br>STORY-PROJ-002/Main Flow<br>BR-SUB-003/Then<br>BR-PROJ-007/Then | Yêu cầu ngày 06/10/2026: adapter AI giả được bật ở Development/Staging/Production; không cấu hình thì 503 không giữ lượt, adapter luôn thành công với bộ kết quả mẫu. Kết quả chạy với adapter giả không phải bằng chứng tích hợp AI thật. Đặc tả chưa thực thi; dữ liệu trong ca là fixture kiểm thử. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-PROJ-002/AC-001
- STORY-PROJ-002/AC-003
- STORY-PROJ-002/EXC-01
- STORY-PROJ-002/Main Flow
- BR-SUB-003/Then
- BR-PROJ-007/Then
