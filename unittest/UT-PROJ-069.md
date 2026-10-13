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

# UT-PROJ-069

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-069 | Tạo dự toán | EstimateShareRateLimits / ConfigureRateLimiter — Giới hạn tần suất riêng của route công khai theo link: theo IP và theo link | Branch | REGRESSION | P1 | Đã có mã test trong bmt-be (commit `8de4c21`), tên test ghi ở Rationale. Host TestServer dùng đúng `ConfigureRateLimiter` và thứ tự middleware của Program.cs. MediatR giả: hồ sơ của link A trả 200, mọi link khác trả 404 ShareUnavailable. IP của kết nối đặt qua header chỉ có trong test, như `RemoteIpAddress` sau forwarded headers. Mỗi ca dựng host riêng với giới hạn nhỏ. Các ca kiểm số lượt trong cùng một cửa sổ dùng SlidingWindowRateLimiter thật qua factory, tắt cấp lại lượt tự động và bọc bằng RateLimiter để bộ quản lý partition không gọi TryReplenish. Giữ nguyên cách chia ngăn IP/link, middleware, kiểm cấu hình và phản hồi 429 của ứng dụng. | (1) Giới hạn IP = 3, link = 1000: cùng một IP gọi xem hồ sơ, trạng thái tệp và tải tệp của ba link khác nhau, rồi gọi lần thứ tư với IP đó viết dạng IPv6 ánh xạ (`::ffff:`); IP khác gọi link A; IP đầu gọi route của chủ sở hữu.<br>(2) Giới hạn IP = 1000, link = 3: ba IP khác nhau gọi link A (xem hồ sơ, trạng thái tệp, tải tệp); IP thứ tư gọi link A, IP thứ năm gọi link A với mã viết hoa; IP thứ tư gọi link B.<br>(3) Giới hạn link = 1: link A có thật và link B không có, mỗi link gọi hai lần từ hai IP.<br>(4) Không cấu hình gì; cấu hình PerIpPermitLimit = 0. | (1) Ba request đầu qua; request thứ tư nhận 429, `Retry-After: 60` và nội dung như bộ giới hạn hiện có, handler không chạy; IP khác vẫn qua; route chủ sở hữu không bị tính (401 vì không có phiên).<br>(2) Request của IP thứ tư và bản mã viết hoa nhận 429; link B vẫn qua.<br>(3) Lần đầu: 200 và 404; lần hai: hai phản hồi 429 giống hệt nhau về mã, nội dung, kiểu nội dung và Retry-After, nên không biết được link nào có thật.<br>(4) Mặc định 60 và 300; giá trị 0 làm API từ chối khởi động. | TDD-PROJ-003/Architecture<br>TDD-PROJ-003/Internal API<br>ST-PROJ-074/System Test | Người dùng xác nhận ngày 27/09/2026: 60 lượt/phút mỗi IP và 300 lượt/phút mỗi link, tải tệp tính chung, vượt thì 429 như cách hiện có và không lộ link có tồn tại hay không. Đối chiếu ngày 30/09/2026: chèn tạm khoảng chờ 65 giây trước request thứ tư tái hiện bản cũ trả 200 và bản sửa trả đúng 429; khoảng chờ này được bỏ trước commit. Test không phụ thuộc thời gian thực của runner; không dùng các ca này để kết luận cơ chế cấp lại lượt sau một phút đã được kiểm. Kiểm thử hết cửa sổ thuộc ST-PROJ-074. Test đi qua pipeline HTTP thật nhưng không có proxy thật; phần forwarded headers đã có test riêng theo TDD-AUTH-001. Mã test: `EstimateSharingRateLimitApiTests.Handle_PublicRoutesOverPerIpLimit_Returns429ForThatIpOnly`, `Handle_PublicLinkOverPerLinkLimit_Returns429FromAnyIp`, `Handle_ExistingAndMissingLinkOverLimit_ReturnIdentical429`, `Handle_DefaultsAndInvalidConfiguration_Use60And300AndRejectZero`. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-003/Architecture
- TDD-PROJ-003/Internal API
- ST-PROJ-074/System Test
