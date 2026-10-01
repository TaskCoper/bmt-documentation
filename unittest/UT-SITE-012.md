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

# UT-SITE-012

## Unit Test

- **Reviewer**: Tân Trần
- **Approver**: Tân Trần

| Test ID | Module | Unit under test | Loại | Suite | Priority | Precondition / Mock setup | Input | Expected output | Trace to (requirement / BR) | Rationale | Owner | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| UT-SITE-012 | Công trình | UpdateConstructionSiteCommandHandler — sửa tên và địa chỉ khi công trình không có gói giữ chỗ | Happy | REGRESSION | P1 | EF Core InMemory qua InMemoryDbFixture, dùng IUnitOfWork thật; phiên giả bằng CurrentUserServiceStub; IConstructionSiteRowLocker giả (NSubstitute) trả true cho (CS1, U1); sau Handle gọi UnitOfWork.CompleteAsync để lưu. CS1 của U1: Name="Nhà phố Quận 7", Version=1, CreatedAtUtc=2026-10-01T02:00:00Z. Biến thể 1: CS1 chưa có gói nào. Biến thể 2: CS1 chỉ còn gói G0 ở trạng thái CanceledByStaff (vẫn trỏ CS1, không giữ chỗ). Clock=2026-10-05T03:00:00Z. Bổ sung theo TDD-SITE-002 đã chốt: công trình trong fixture có Latitude=10, Longitude=106; request tạo/sửa trong ca này gửi đủ cặp (10,106) để tiếp tục kiểm mục tiêu gốc. Đây là cập nhật đặc tả; chưa sửa hay chạy lại mã test. Bổ sung hồ sơ theo TDD-SITE-003 đã chốt: mỗi site là độc lập (SourceEstimateId=NULL), trừ biến thể nói khác; fixture tạo/sửa có AreaM2="100.25", H1=Đất trống active, BudgetVnd="2000000000", PlannedStart=Within1To3Months; revision V1/B1=Nhà phố, tầng {1,3}, tum bật, styles Architecture K1/Interior N1; chọn 3,false,K1,N1. Địa chỉ tách ProvinceCode=1, WardCode=4, dataset=pov2-fixture; adapter giả trả Thành phố Hà Nội/Phường Ba Đình, AddressDetail theo ca. POST độc lập có expectedCatalogRevisionId=V1; PUT dùng revision đã lưu; không tệp. Chỉ trường được kiểm trong ca mới được thiếu/sai. Các nhãn Address=A/B trong fixture cũ là địa chỉ gộp đọc ra; request mới gửi profile.addressDetail tương ứng, không nhận field address cũ. Đây là cập nhật đặc tả, không phải mã test cũ đã hỗ trợ schema mới. | Ở từng biến thể, U1 gửi name="Nhà phố mới", profile.addressDetail="12 Nguyễn Thị Thập, Quận 7, TP.HCM", expectedVersion=1. | Cả hai biến thể thành công: CS1 có Name="Nhà phố mới", NormalizedName="NHÀ PHỐ MỚI", địa chỉ mới, Version=2, UpdatedAtUtc=2026-10-05T03:00:00Z; trả 200 với version=2. LockOwnedForUpdateAsync được gọi đúng một lần với (CS1, U1). Không có lệnh ghi nào vào SupervisionGrant hay Assignment; ở biến thể 2, G0 vẫn ở CanceledByStaff và vẫn trỏ CS1. | BR-SITE-002/Then<br>STORY-SITE-001/ALT-02<br>STORY-SITE-001/AC-017<br>TDD-SITE-001/Architecture<br>TDD-SITE-001/Data Model<br>TDD-SITE-002/Architecture<br>TDD-SITE-003/Internal API | Cập nhật ngày 25/09/2026 theo thiết kế lần 2 của TDD-SITE-001 (chưa có trong code, bmt-be develop commit 1ffdfbf). Khách chỉ sửa được công trình không có gói giữ chỗ; gói đã hủy không khóa việc sửa (BR-SITE-002 khoản 3). Ca sửa công trình có gói Assigned hoặc Completed chuyển sang UT-SITE-028; ca gói đã gỡ ở UT-SITE-029. Mã test hiện có bmt-be.application.tests/usecases/constructionSite/ConstructionSiteTests.cs:Update_RenameRules đang sửa công trình có gói Assigned và kỳ vọng thành công, trái với quy tắc mới, nên phải viết lại theo đặc tả này. Test hiện chưa kiểm UpdatedAtUtc (dùng đồng hồ hệ thống), Name, NormalizedName, Address đã lưu và lời gọi cổng khóa. Phần mô tả mã test/kết quả kiểm trước đây là bằng chứng cho contract cũ; phải cập nhật fixture và chạy lại trước khi công nhận contract mở rộng. Ở unit validator/handler, mã HTTP là ánh xạ contract, không tự chứng minh endpoint hoặc rollback PostgreSQL. | [Chưa xác định] | Draft |

## TEST_LINKS

- BR-SITE-002/Then
- STORY-SITE-001/ALT-02
- STORY-SITE-001/AC-017
- TDD-SITE-001/Architecture
- TDD-SITE-001/Data Model
- TDD-SITE-002/Architecture
- TDD-SITE-003/Internal API
