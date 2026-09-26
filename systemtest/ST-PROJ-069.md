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

# ST-PROJ-069

## System Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Story | Loại | Suite | Priority | Precondition | Steps | Test data | Expected result | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ST-PROJ-069 | STORY-PROJ-003 | Main / Integration boundary | REGRESSION | P1 | Môi trường thử đã có chức năng tương ứng; C1 là khách đã đăng nhập, sở hữu bản dự toán; gói còn hiệu lực, quyền tạo thiết kế, quota hữu hạn 3, đã dùng 0, đang giữ 0, trừ khi ca nêu khác. Có thể đọc lại dữ liệu đã lưu, trạng thái tác vụ và sổ lượt. Có kết quả và tệp thử theo hợp đồng đầu ra đã thống nhất; cách nhận hoặc xuất PDF/Excel còn chờ tích hợp. Không dùng tệp giả làm bằng chứng tích hợp thật. Dùng dịch vụ email giả lập/hộp thư thử, không gửi tới người thật; có thể kiểm tra nội dung và phản hồi tiếp nhận. C1 sở hữu D1 “Phương án 3” đã thành công với kết quả J1; PDF EX1 đã xuất xong; D1 có link S1 còn hiệu lực. | 1. Ghi lại sổ lượt, số lần gọi AI và mã tệp EX1; người nhận tải PDF qua S1 và giữ bản đã tải.<br>2. C1 đổi tên D1 thành “Phương án chốt”.<br>3. C1 mở hồ sơ D1; người nhận mở S1 bằng trình duyệt không đăng nhập.<br>4. C1 rồi người nhận yêu cầu tải PDF; theo dõi đến khi tệp sẵn sàng và mở tệp.<br>5. Gọi trực tiếp đường tải EX1 bằng phiên C1 và bằng S1.<br>6. C1 gửi email chứa link S1 tới hộp thư thử; đọc nội dung thư.<br>7. Đọc lại J1, sổ lượt, số lần gọi AI và bản PDF người nhận đã tải ở bước 1. | D1 NameVersion=1 trước khi đổi tên; S1 hết hạn sau ngày thử; người nhận trong hộp thư thử. | Hồ sơ của C1 và trang xem qua S1 đều hiện “Phương án chốt”. Lần tải sau khi đổi tên xuất tệp mới từ J1, trong tệp ghi tên “Phương án chốt”; C1 và người nhận dùng chung một tệp mới. EX1 không còn được phục vụ: gọi trực tiếp trả 409 ExportOutdated. Thư mới dùng tên hiện tại. Xuất lại không tính lượt, không gọi AI, J1 giữ nguyên; bản PDF đã tải trước đó vẫn trên máy người nhận, không bị thu hồi. | STORY-PROJ-003/AC-006<br>STORY-PROJ-003/AC-002<br>STORY-PROJ-004/AC-002<br>BR-PROJ-007/Then | Sau khi đổi tên, hồ sơ và tệp luôn theo tên hiện tại mà không tạo thiết kế mới. Từ ngày 26/09/2026, PDF/Excel do AI trả bằng URL và backend không tự dựng tệp, nên bước kiểm “trong tệp ghi tên” phụ thuộc câu hỏi mở ở TDD-PROJ-003/Architecture; nếu người dùng chốt đặt tên mới ở tên tệp tải về thì phải sửa bước kiểm này cùng AC-006. Đặc tả chưa thực thi; dữ liệu trong ca là fixture kiểm thử, không phải mặc định sản phẩm. | [Chưa xác định] | Draft |

## TEST_LINKS

- STORY-PROJ-003/AC-006
- STORY-PROJ-003/AC-002
- STORY-PROJ-004/AC-002
- BR-PROJ-007/Then
