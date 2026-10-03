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

<!-- Thay mã BR-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Effective Date dùng YYYY-MM-DD hoặc để trống. Status: Draft / Active / Deprecated.
Statement, When, Then và Except phải phản ánh đúng chính sách nguồn; không tự đặt công thức, ngưỡng, thời hạn hay ngoại lệ. Chỉ để Except trống khi đã xác định không có ngoại lệ; thiếu thông tin thì hỏi lại.
Version và phê duyệt do hệ thống quản lý. Owner là tên nghiệp vụ; gán tài khoản trên giao diện.
Liên kết Story và các trường quản trị bổ sung trên giao diện sau import.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Name tối đa 300 ký tự (được dùng làm tiêu đề tài liệu); Category tối đa 200; Owner tối đa 200; Source tối đa 500.
- Effective Date: dùng ngày hợp lệ YYYY-MM-DD hoặc để trống khi chưa xác định; ngày không đọc được sẽ bị để trống kèm cảnh báo. Không tự đặt ngày hiệu lực.
ĐỐI CHIẾU FORM BUSINESS RULE (src/features/business-rules/validations.ts; form có thể chặt hơn import):
- Form yêu cầu Name, Category, Statement, When, Then, Source và Owner có nội dung; Except và Notes được phép trống. Liên kết Story đã khai báo không được rỗng.
- Form còn kiểm tra Version và Effective Date không rỗng; với file nhập mới, vẫn để Version trống theo hợp đồng import vì hệ thống quản lý phiên bản. Thiếu ngày hiệu lực thì hỏi lại, không bịa để thoả form.
-->

# BR-CTR-008

## Rule Info

- **Name**: Xác định miền của nhà thầu từ mã tỉnh/thành.
- **Category**: Nhà thầu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Yêu cầu lập bảng ánh xạ mã tỉnh và lọc nhà thầu theo Bắc, Trung, Nam trong hội thoại ngày 2026-10-03; người dùng đồng ý xếp Tây Nguyên vào miền Trung, bổ sung tỉnh cho nhà thầu và giữ nhà thầu chưa có tỉnh trong danh sách chung.

## Statement

Miền của nhà thầu được xác định từ tỉnh/thành của địa chỉ công ty theo bảng bên dưới. Có ba miền Bắc, Trung và Nam; Tây Nguyên thuộc miền Trung. Khi khách chọn một miền, hệ thống chỉ trả nhà thầu đang hiển thị có tỉnh thuộc miền đó và thỏa các bộ lọc khác được chọn.

## When

Admin nhập hoặc sửa tỉnh/thành của nhà thầu; khách mở danh sách hoặc lọc nhà thầu theo miền.

## Then

1. Admin được chọn tỉnh/thành từ danh mục khi tạo hoặc sửa hồ sơ. Tỉnh có thể bổ sung sau, giữ cách tạo nhanh chỉ cần tên theo STORY-CTR-001/AC-006.
2. Hệ thống dùng mã tỉnh để xác định miền; không suy đoán từ chuỗi địa chỉ, tên phường, kinh độ hoặc vĩ độ. Tên tỉnh chỉ dùng để hiển thị.
3. Mỗi mã trong bảng thuộc đúng một miền. Miền Trung gồm các tỉnh Tây Nguyên; miền phản ánh địa chỉ công ty, không phải phạm vi phục vụ tự khai.
4. Khi không chọn miền, nhà thầu đang hiển thị vẫn được xét theo các bộ lọc hiện có, kể cả chưa có tỉnh.
5. Khi chọn miền, loại nhà thầu chưa có tỉnh khỏi kết quả. Không tự gán miền Nam hoặc miền khác cho dữ liệu thiếu.
6. Kết hợp miền với loại công trình, phạm vi thi công và bán kính bằng điều kiện đồng thời. Không thay đổi quy tắc OR trong từng nhóm danh mục theo BR-CTR-005.
7. Admin đổi tỉnh hợp lệ thì lần đọc mới xác định miền theo tỉnh mới. Không cho nhập một miền độc lập mâu thuẫn với tỉnh.
8. Bộ lọc miền riêng không yêu cầu đăng nhập. Điều kiện đăng nhập và sở hữu công trình chỉ áp dụng khi dùng thêm bán kính theo BR-CTR-006.
9. Không có nhà thầu phù hợp thì trả danh sách rỗng; không nới bộ lọc hoặc thay bằng nhà thầu mẫu.
10. Từ chối mã tỉnh ngoài danh mục hỗ trợ hoặc miền không hợp lệ; không âm thầm bỏ qua giá trị sai. Quy cách chuỗi mã và mã lỗi HTTP sẽ được ghi trong TDD trước triển khai.

## Except

- Nhà thầu cũ chưa có tỉnh vẫn giữ trạng thái hiển thị và thông tin hiện có; admin bổ sung tỉnh khi cập nhật. Không tự chuyển đổi địa chỉ cũ sang mã tỉnh trong đợt này.
- Chưa hỗ trợ tự đổi mã tỉnh thuộc danh mục 63 tỉnh trước sáp nhập sang danh mục 34 tỉnh. Mã ngoài bảng cần được đối chiếu và chọn lại; không tự gán theo khoảng số.

## Notes

Bảng dưới có 34 tỉnh/thành: Bắc 15, Trung 11, Nam 8. Cột mã hai chữ số dùng để đối chiếu danh mục hành chính; cột mã API phản ánh kiểu mã hiện có ở Provinces Open API v2 và backend (ví dụ Hà Nội là `1`). Quy cách nhận `01` hay `1` ở API nhà thầu sẽ được chuẩn hóa trong TDD; hai cách viết không đại diện cho hai tỉnh khác nhau.

Tên trong bảng bỏ tiền tố “Tỉnh”/“Thành phố” để tập trung vào địa danh, không dùng bảng này để xác định loại đơn vị hành chính. Mã tỉnh và cách chia ba miền là hai dữ liệu khác nhau: mã đối chiếu danh mục hành chính; cách gộp ba miền là quy ước nghiệp vụ BMT đã được người dùng xác nhận, không tuyên bố Quyết định 19/2025/QĐ-TTg ban hành bảng ba miền.

| Mã hai chữ số | Mã API | Tỉnh/thành | Miền |
| --- | --- | --- | --- |
| 01 | 1 | Hà Nội | Bắc |
| 04 | 4 | Cao Bằng | Bắc |
| 08 | 8 | Tuyên Quang | Bắc |
| 11 | 11 | Điện Biên | Bắc |
| 12 | 12 | Lai Châu | Bắc |
| 14 | 14 | Sơn La | Bắc |
| 15 | 15 | Lào Cai | Bắc |
| 19 | 19 | Thái Nguyên | Bắc |
| 20 | 20 | Lạng Sơn | Bắc |
| 22 | 22 | Quảng Ninh | Bắc |
| 24 | 24 | Bắc Ninh | Bắc |
| 25 | 25 | Phú Thọ | Bắc |
| 31 | 31 | Hải Phòng | Bắc |
| 33 | 33 | Hưng Yên | Bắc |
| 37 | 37 | Ninh Bình | Bắc |
| 38 | 38 | Thanh Hóa | Trung |
| 40 | 40 | Nghệ An | Trung |
| 42 | 42 | Hà Tĩnh | Trung |
| 44 | 44 | Quảng Trị | Trung |
| 46 | 46 | Huế | Trung |
| 48 | 48 | Đà Nẵng | Trung |
| 51 | 51 | Quảng Ngãi | Trung |
| 52 | 52 | Gia Lai | Trung |
| 56 | 56 | Khánh Hòa | Trung |
| 66 | 66 | Đắk Lắk | Trung |
| 68 | 68 | Lâm Đồng | Trung |
| 75 | 75 | Đồng Nai | Nam |
| 79 | 79 | Hồ Chí Minh | Nam |
| 80 | 80 | Tây Ninh | Nam |
| 82 | 82 | Đồng Tháp | Nam |
| 86 | 86 | Vĩnh Long | Nam |
| 91 | 91 | An Giang | Nam |
| 92 | 92 | Cần Thơ | Nam |
| 96 | 96 | Cà Mau | Nam |

Nguồn đối chiếu:

- [Quyết định 19/2025/QĐ-TTg](https://vanban.chinhphu.vn/?classid=1&docid=214409&orggroupid=3&pageid=27160): ban hành danh mục và mã đơn vị hành chính, có hiệu lực từ 2025-07-01.
- [Provinces Open API v2](https://provinces.open-api.vn/api/v2/): bộ 34 mã được kiểm tra trong phiên ngày 2026-10-03; [tài liệu nhà cung cấp](https://provinces.open-api.vn/) phân biệt v1 trước và v2 sau sáp nhập 07/2025.
- [Cổng thông tin Quảng Ngãi](https://quangngai.gov.vn/tin-tuc/hoi-nghi-nganh-cong-thuong-mien-trung-tay-nguyen-lan-thu-xi-huong-toi-hinh-thanh-chuoi-gia-tri-lien-vung-va-cum-lien-ket.html): đối chiếu nhóm 11 tỉnh/thành miền Trung – Tây Nguyên. Phần gộp thành ba miền phục vụ BMT dựa trên quyết định trong hội thoại.

Phần nghiệp vụ đã được xác nhận qua câu trả lời của người dùng; bản US/BR cập nhật đã được người dùng chốt bằng phản hồi “chốt”. Đã soạn ST-CTR-033 đến ST-CTR-040; chưa chạy test, triển khai code hoặc migration. Owner và ngày hiệu lực trên hệ thống tài liệu chưa xác định; không tự gán từ Reviewer/Approver.
