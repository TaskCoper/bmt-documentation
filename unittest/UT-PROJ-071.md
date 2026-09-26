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

# UT-PROJ-071

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-071 | Tạo dự toán | EstimateSharingStuckRecovery / RecoverStuckEstimateSharingJob / EstimateSharingMaintenanceStore — Job xử lý yêu cầu xuất tệp và yêu cầu email bị kẹt | Branch | REGRESSION | P1 | Đã có mã test trong bmt-be (commit `8de4c21`), tên test ghi ở Rationale. Phần quyết định dùng store giả ghi lại đối số; điều kiện kẹt và việc chạy đồng thời với consumer chạy trên PostgreSQL thật (Testcontainers) với worker thật, SMTP và client tệp giả. Mốc quét T đã làm tròn tới giây; ngưỡng mặc định 15 phút. Lịch Quartz kiểm bằng cấu hình trong bộ nhớ. | (1) Export X1 Pending đổi lần cuối lúc T−20 phút, không lease; X2 Pending T−20 phút, lease còn tới T+1 phút; X3 Pending T−14 phút. Email M1 Queued T−20 phút; M2 Sending T−20 phút, lease hết lúc T−1 phút; M3 Sending T−20 phút, lease tới T+1 phút; M4 Queued T−14 phút; M5 Accepted.<br>(2) Sau đợt quét: message của X1 và M1 tới muộn; khách yêu cầu lại PDF bằng khóa mới; quét lần hai.<br>(3) Job chạy cùng lúc với consumer trên 4 export (5 vòng) và 12 email (3 vòng), sau đó giao lại message email.<br>(4) Store giả: có dòng kẹt nhưng consumer đổi trước (UPDATE không khớp); không có dòng kẹt; 25 export kẹt; cấu hình StuckAfterMinutes = 30, BatchSize = 5.<br>(5) Chu kỳ quét để trống, 0 hoặc 60 giây. | (1) X1 Failed với ExportTimedOut, không có URL, bỏ lease, UpdatedAtUtc = T. M1 và M2 Unknown với EmailTimedOut, AcceptedAtUtc NULL. X2, X3, M3, M4, M5 giữ nguyên. Đúng một cảnh báo Discord loại EstimateSharingStuck, có mã X1 và M1. Có hai index có điều kiện của migration.<br>(2) Consumer tới muộn không nhận việc, không đọc tệp, không gửi thư; khách yêu cầu lại thì có lần thử 2 ở Pending; lần quét hai không đổi lại dòng cũ và không gửi thêm cảnh báo.<br>(3) Mỗi export có đúng một kết cục: Ready có URL, hoặc Failed ExportTimedOut không URL, khớp danh sách job báo. Email job đóng thì Unknown và không bao giờ được gửi; email consumer nhận trước thì Accepted và được gửi đúng một lần; không người nhận nào nhận hai thư.<br>(4) Không đổi được dòng nào thì không cảnh báo; 25 dòng vẫn chỉ một cảnh báo, liệt kê 10 mã và ghi "15 mã khác"; mốc kẹt T−30 phút, lô 5 dòng.<br>(5) Chỉ lên lịch khi chu kỳ là 60: một trigger lặp mỗi 60 giây, không cho chạy chồng. | TDD-PROJ-003/Architecture<br>TDD-PROJ-003/State Diagram<br>STORY-PROJ-003/EXC-03<br>STORY-PROJ-004/EXC-03<br>ST-PROJ-076/System Test<br>ST-PROJ-077/System Test | Người dùng xác nhận ngày 27/09/2026: dòng kẹt quá 15 phút thì export chuyển thất bại để khách bấm lại, email chuyển Unknown, không tự chạy lại hay gửi lại; cảnh báo Discord không spam. Mã test: `EstimateSharingStuckRecoveryTests.Handle_StuckRowsFound_TransitionsEachAndSendsOneAlert`, `Handle_NothingChanged_DoesNotAlert`, `Handle_ConfiguredThreshold_UsesStuckAfterMinutesAndBatchSize`, `Handle_ManyStuckRows_OneAlertListsAtMostTenIdsPerKind`, `Handle_DefaultOption_Uses15MinutesAndDisabledSchedule`; `QuartzScheduleTests.Handle_SharingIntervalNotPositive_DoesNotScheduleJob`, `Handle_SharingIntervalPositive_SchedulesRepeatingJob`; trên PostgreSQL: `EstimateSharingStuckRecoveryFlowTests.Handle_StuckRows_OnlyStaleRowsWithoutLiveLeaseAreClosedOnce`, `Handle_StuckExportRacingConsumer_EndsInExactlyOneOutcome`, `Handle_StuckEmailRacingConsumer_NeverSendsTwiceOrAfterUnknown`. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-003/Architecture
- TDD-PROJ-003/State Diagram
- STORY-PROJ-003/EXC-03
- STORY-PROJ-004/EXC-03
- ST-PROJ-076/System Test
- ST-PROJ-077/System Test
