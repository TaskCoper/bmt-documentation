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

# UT-PROJ-062

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-PROJ-062 | Tạo dự toán | ProvincesOpenApiLocationCatalog — Dùng bản lưu khi nguồn địa chỉ lỗi, 503 khi chưa có bản nào | Error | FULL | P1 | Đã có mã test trong bmt-be, tên test ghi ở Rationale. Không gọi dịch vụ thật: HttpMessageHandler giả trả dữ liệu cùng cấu trúc `GET /api/v2/?depth=2` (hai tỉnh mẫu: mã 1 có xã 4 và 8, mã 4 có xã 1273; xã nằm trong mảng `wards` của tỉnh, không có `province_code`); Redis thay bằng bộ nhớ có tuần tự hóa JSON; đồng hồ giả. Option: CacheTtlMinutes=60, RefreshRetrySeconds=30. | (a) Lần đọc đầu; (b) đọc lại sau 59 phút; (c) sau 61 phút nguồn trả 500, JSON hỏng hoặc mảng rỗng; (d) chưa có bản lưu, nguồn trả 502, quá thời gian chờ, xã ghi `province_code` của tỉnh khác hoặc tỉnh không có xã; (e) nguồn lỗi rồi đọc lại sau 29 và 30 giây; (f) Redis lỗi; (g) `PrepareAsync` khi nguồn và Redis cùng lỗi; (h) `province_code` của xã thiếu, NULL hoặc khớp tỉnh bao ngoài; (i) bản lưu đã cũ, nguồn mới trả xã có `province_code` bằng 0, số âm hoặc mã tỉnh khác. | (a) Gọi đúng `https://provinces.open-api.vn/api/v2/?depth=2`, trả hai tỉnh với mã dạng chuỗi và phiên bản bắt đầu `pov2-`, lưu Redis không đặt hạn. (b) Không gọi nguồn. (c) Trả lại bản cũ cùng phiên bản. (d) 503 DependencyUnavailable, không ghi Redis. (e) Lần 29 giây không gọi nguồn và vẫn 503; lần 30 giây gọi lại và nhận dữ liệu. (f) Lấy thẳng từ nguồn. (g) Không ném lỗi. (h) Trả đúng danh sách xã và xác minh theo tỉnh bao ngoài; xã của tỉnh khác vẫn bị từ chối, đọc lại dùng cache. (i) Dùng bản cũ và không ghi đè nội dung cache. | TDD-PROJ-001/External API<br>TDD-PROJ-001/Architecture<br>BR-PROJ-004/Notes<br>ST-PROJ-071/System Test | Quyết định ngày 26/09/2026: nguồn lỗi thì dùng bản đã lưu, chỉ 503 khi chưa có. Mã test: `ProvincesOpenApiLocationCatalogTests.Handle_FirstLoad_FetchesV2DatasetAndCachesWithoutExpiry`, `Handle_FreshCache_DoesNotCallSource`, `Handle_StaleCacheAndSourceFails_ReturnsCachedData`, `Handle_NoCacheAndSourceFails_ReturnsDependencyUnavailable`, `Handle_AfterFailure_WaitsRetryIntervalBeforeCallingAgain`, `Handle_RedisFailure_FallsBackToSource_PrepareSwallowsUnavailable`, `Handle_OptionalWardProvinceCode_VerifiesUsingContainingProvince`, `Handle_ExplicitWrongWardProvinceCode_DoesNotReplaceCachedDataset`. Không chứng minh dịch vụ thật trả đúng cấu trúc; phần đó kiểm ở ST-PROJ-071. | [Chưa xác định] | Draft |

## TEST_LINKS

- TDD-PROJ-001/External API
- TDD-PROJ-001/Architecture
- BR-PROJ-004/Notes
- ST-PROJ-071/System Test
