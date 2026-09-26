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

# ST-PROJ-071

## System Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-PROJ-071 | STORY-PROJ-001 | EXC / ALT / Integration boundary | REGRESSION | P1 | C1 là khách đã đăng nhập, sở hữu D1 và đủ điều kiện lưu. Môi trường thử điều khiển được đường ra tới provinces.open-api.vn (proxy hoặc máy chủ giả trả đúng cấu trúc `GET /api/v2/?depth=2`) và đọc/xóa được khóa dữ liệu địa chỉ trong Redis. Đặt `ProvincesOpenApiOption__CacheTtlMinutes` nhỏ để thử bản lưu quá hạn. | 1. Xóa bản lưu, chặn nguồn; gọi GET danh sách tỉnh; PUT D1 chọn tỉnh; PUT D1 chỉ sửa mô tả.<br>2. Mở nguồn; gọi GET danh sách tỉnh, danh sách xã của một tỉnh; PUT D1 chọn tỉnh P và xã W của tỉnh đó với phiên bản vừa nhận; thử thêm xã thuộc tỉnh khác.<br>3. Chặn nguồn, chờ bản lưu quá hạn; gọi lại GET danh sách tỉnh và PUT D1 chọn xã khác của P.<br>4. Cho máy chủ giả đổi dữ liệu (gộp W vào xã khác); mở nguồn, chờ bản lưu quá hạn rồi gọi GET danh sách tỉnh; PUT D1 chọn xã với phiên bản cũ; mở lại D1; PUT D1 chỉ sửa mô tả. | Dữ liệu thật hoặc giả cùng cấu trúc API v2 sau sáp nhập 07/2025 (34 tỉnh/thành, hai cấp tỉnh–xã). Mã tỉnh/xã là số của nguồn, backend trả dạng chuỗi. Phiên bản dữ liệu dạng `pov2-` cộng 16 ký tự hex. | Bước 1: GET tỉnh và PUT chọn tỉnh trả 503 DependencyUnavailable, không ghi; PUT chỉ sửa mô tả thành công. Bước 2: trả 34 tỉnh cùng một datasetVersion; lưu được P/W với tên lấy từ nguồn; xã thuộc tỉnh khác bị từ chối 422. Bước 3: nguồn lỗi nhưng GET vẫn trả danh sách và phiên bản đã lưu; PUT chọn xã khác vẫn lưu được, không 503. Bước 4: datasetVersion mới khác cũ; PUT với phiên bản cũ trả 409 LocationDatasetChanged, không ghi; D1 vẫn hiển thị W và phiên bản cũ đã lưu; PUT chỉ sửa mô tả vẫn lưu được. Kiểm chặn gửi AI khi xã không còn trong dữ liệu mới thuộc TDD-PROJ-002, nằm ở ST-PROJ-073. | STORY-PROJ-001/AC-005<br>STORY-PROJ-001/AC-015<br>STORY-PROJ-001/ALT-05<br>BR-PROJ-004/Then<br>BR-PROJ-004/Notes | Nguồn địa chỉ provinces.open-api.vn theo quyết định ngày 26/09/2026: nguồn lỗi thì dùng bản đã lưu, chỉ 503 khi chưa có; bản nháp giữ xã cũ đã lưu. Đặc tả chưa thực thi. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-PROJ-001/AC-005
- STORY-PROJ-001/AC-015
- STORY-PROJ-001/ALT-05
- BR-PROJ-004/Then
- BR-PROJ-004/Notes
