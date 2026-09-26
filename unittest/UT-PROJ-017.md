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

# UT-PROJ-017

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-017 | Tạo dự toán | SaveEstimateInputHandler / validator — Thay ảnh không hợp lệ giữ ảnh cũ | Error | FULL | P1 | Clock cố định và fake dependency có thể đọc lại đối số/trạng thái; không gọi dịch vụ thật. Theo quyết định ngày 26/09/2026, ảnh đầu vào là URL do frontend upload; backend kiểm URL tuyệt đối https, tối đa 2048 ký tự, không kiểm định dạng hay dung lượng tệp. Bổ sung lần 2 cùng ngày: URL mới phải thuộc tên miền kho presign; fixture cấu hình `UploadedFileOption.AllowedHosts=[cdn.example.test]`. Bản đang có InputImageUrl=F1 (URL https trên cdn.example.test). | PUT thay inputImageUrl=F2 với F2 lần lượt là: URL http, URL tương đối, URL có khoảng trắng, URL dài 2049 ký tự; biến thể F2 là URL https hợp lệ nhưng cùng yêu cầu có lựa chọn sai (số tầng ngoài danh sách); biến thể đối chứng F2 hợp lệ, không có lỗi khác. Biến thể tên miền: F2 ở evil.example.org, img.cdn.example.test (tên miền con), cdn.example.test.evil.org, cổng 8443, có `user@`; F2 ở CDN.Example.Test (khác hoa thường). Biến thể chưa cấu hình tên miền (danh sách rỗng): gửi F2 mới; lưu mô tả khi F1 nằm ngoài danh sách; xóa ảnh (null). | URL sai bị validator từ chối với InvalidEstimateInput (422), không tới handler. Biến thể có lựa chọn sai bị handler từ chối cả lần lưu; F1 và InputVersion giữ nguyên, không có ghi chờ lưu, không yêu cầu xóa F1. Biến thể đối chứng lưu F2, InputVersion tăng 1, chỉ còn F2 là ảnh đầu vào. Tên miền ngoài danh sách, tên miền con, cổng khác 443 hoặc có thông tin đăng nhập bị handler từ chối 422 InvalidEstimateInput ở trường `input.inputImageUrl`, F1 giữ nguyên; khác hoa thường được nhận. Danh sách rỗng: F2 mới bị từ chối 503 DependencyUnavailable, không ghi; lưu mô tả khi không đổi ảnh và xóa ảnh vẫn thành công. | TDD-PROJ-001/Architecture<br>TDD-PROJ-001/Internal API<br>BR-PROJ-002/Then<br>ST-PROJ-011/System Test | Thay ảnh không hợp lệ hoặc lưu lỗi giữ ảnh cũ (BR-PROJ-002 khoản 8, 9). Mã test: `EstimateValidatorTests.Handle_InputImageUrl_RequiresAbsoluteHttps`, `EstimateValidatorTests.Handle_InputImageUrlLongerThan2048_IsRejected`, `EstimateCommandHandlerTests.Handle_ReplaceImageRejected_KeepsOldImageUrl`, `Handle_ReplaceImageWithUrlOutsideUploadStore_IsRejectedAndKeepsOldImage`, `Handle_ReplaceImageWithUppercaseUploadHost_IsSaved`, `Handle_UploadHostsNotConfigured_RejectsOnlyNewImageUrl`, `UploadedFileUrlPolicyTests` (khớp tên máy chủ, punycode, danh sách rỗng, định dạng phần tử cấu hình). Kiểm hành vi ở biên unit; không dùng mock để kết luận transaction, SQL hoặc tích hợp thật đúng. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-001/Architecture
- TDD-PROJ-001/Internal API
- BR-PROJ-002/Then
- ST-PROJ-011/System Test
