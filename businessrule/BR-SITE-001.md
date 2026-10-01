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

# BR-SITE-001

## Rule Info

- **Name**: Hồ sơ công trình đầy đủ thông tin đất, địa chỉ, ngân sách, khởi công và phân loại; tên không trùng trong cùng khách.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định ngày 25/09/2026 về tên, quyền sở hữu và tọa độ; hội thoại ngày 01/10/2026 chốt mở rộng hồ sơ, diện tích đất, địa chỉ ba phần, ngân sách VND, khởi công, danh mục dùng chung và các trường bắt buộc.

## Statement

Công trình phải có đầy đủ thông tin tại Then khi được tạo hoặc lưu sửa. Diện tích là diện tích đất. Tệp đính kèm và dự toán nguồn không bắt buộc; các trường phân loại không áp dụng theo BR-SITE-005 được để trống. Tên công trình không trùng với công trình khác của cùng khách, không phân biệt chữ hoa/thường.

## When

Khách hàng tạo hoặc sửa hồ sơ công trình; hệ thống kiểm tra dữ liệu lấy từ dự toán nguồn trước khi tạo.

## Then

1. Bỏ khoảng trắng đầu/cuối của tên và số nhà–đường trước khi kiểm tra và lưu; giữ khoảng trắng ở giữa.
2. Tên có nội dung, tối đa 200 ký tự. Số nhà–đường có nội dung, giữ giới hạn 500 ký tự của phần địa chỉ nhập tự do. Không tự cắt ngắn dữ liệu vượt giới hạn.
3. Địa chỉ gồm Tỉnh/Thành phố, Phường/Xã và Số nhà–đường; cả ba bắt buộc. Chọn tỉnh/xã từ nguồn địa chỉ dùng chung với dự toán, xã phải thuộc tỉnh đã chọn. Không thêm cấp quận/huyện. Frontend xác định tọa độ của địa chỉ đầy đủ.
4. Tên không trùng tên công trình khác của cùng khách sau khi bỏ khoảng trắng đầu/cuối và không phân biệt hoa/thường. Khi sửa không so với chính công trình đang sửa. Khách khác nhau được đặt trùng tên.
5. Không giới hạn số công trình của khách. Tạo công trình miễn phí, không cần gói thiết kế hay gói giám sát; chọn dự toán nguồn không tạo tác vụ AI hoặc trừ lượt.
6. Yêu cầu bị từ chối không tạo công trình, không chiếm dự toán nguồn và không thay đổi hồ sơ đã lưu.
7. Khi tạo phải gửi đủ vĩ độ và kinh độ hợp lệ cho địa chỉ công trình. Backend từ chối khi thiếu hoặc không hợp lệ, kể cả gọi API trực tiếp. Dự toán nguồn không thay thế yêu cầu xác định tọa độ.
8. Khi sửa địa chỉ được phép theo BR-SITE-002 và BR-SITE-004, phải xác định lại và gửi đủ tọa độ mới. Địa chỉ và tọa độ lưu cùng nhau; thiếu tọa độ thì từ chối toàn bộ thay đổi.
9. AreaM2 là diện tích đất theo m², bắt buộc lớn hơn 0 và tối đa 2 chữ số thập phân. Không coi đây là tổng diện tích sàn; không nhân diện tích với số tầng. Từ chối số có quá 2 chữ số thập phân, không tự làm tròn để lưu.
10. Khi tạo hoặc đổi hiện trạng, bắt buộc chọn đúng một mục đang cho chọn theo BR-SITE-006. Hồ sơ cũ được giữ mục đã ngừng khi sửa trường khác theo ngoại lệ của quy tắc đó. Các mục ban đầu là Đất trống, Có nhà cũ cần phá dỡ và Cải tạo; không dùng hiện trạng làm trạng thái tiến độ thi công.
11. Ngân sách dự kiến là một số tiền nguyên VND lớn hơn 0, bắt buộc nhập. Hiển thị có phân cách hàng nghìn và đơn vị VND, ví dụ 2.000.000.000 ₫. Không nhận khoảng từ–đến hoặc số lẻ dưới một đồng.
12. Dự kiến khởi công bắt buộc chọn một trong bốn giá trị: Càng sớm càng tốt; Trong 1–3 tháng tới; Trong 3–6 tháng tới; Chưa xác định. Chưa xác định là lựa chọn hợp lệ, khác với chưa chọn; không tự quy đổi thành ngày khởi công cụ thể.
13. Loại công trình bắt buộc chọn đúng một loại. Số tầng, Có tum/Không tum, phong cách kiến trúc và phong cách nội thất tuân theo BR-SITE-005. Mỗi nhóm phong cách áp dụng phải chọn đúng một giá trị.
14. Mọi thông tin nêu trên đều bắt buộc, trừ dự toán nguồn, tệp đính kèm và các trường không áp dụng theo BR-SITE-005. Không tạo hồ sơ thiếu thông tin bắt buộc bằng cách coi là bản nháp công trình.
15. Tạo từ dự toán theo BR-SITE-004; tệp theo BR-SITE-007. Ngân sách là thông tin khách khai báo, không tự lấy tổng chi phí AI làm ngân sách; tên công trình, hiện trạng và dự kiến khởi công cũng do khách nhập riêng.

## Except

Trường phân loại bị tắt hoặc không có lựa chọn theo BR-SITE-005 hiển thị Không áp dụng và không bắt nhập. Dự toán nguồn và tệp đính kèm được bỏ trống. Công trình đã xóa theo BR-SITE-002 không tính khi kiểm trùng tên.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Người dùng xác nhận ngày 01/10/2026 không có dữ liệu công trình, kể cả dữ liệu thật; không có yêu cầu chuyển đổi hay tự điền hồ sơ cũ. Đây không phải chỉ thị xóa dữ liệu ở bất kỳ môi trường nào.
- Thay mô tả cũ chỉ có tên và địa chỉ một ô bằng hồ sơ đầy đủ và địa chỉ ba phần. Giới hạn 500 ký tự áp dụng cho Số nhà–đường; cách ghép địa chỉ hiển thị thuộc TDD.
- Ví dụ diện tích đất 100 m², xây 80 m² mỗi tầng và có 3 tầng: AreaM2 của công trình là 100, không phải 80 hoặc 240.
- Giữ dấu và khoảng trắng giữa tên: Nhà phố khác Nha pho; Nhà  phố khác Nhà phố. Quyền quản lý theo [BR-SITE-002](BR-SITE-002.md), quyền xem theo [BR-SITE-003](BR-SITE-003.md).
- Hồ sơ là thực thể riêng với dự toán; quan hệ nguồn có hoặc không theo [BR-SITE-004](BR-SITE-004.md), không đồng nhất hai hồ sơ.
- Địa chỉ nguồn từ dự toán được giữ theo dữ liệu đã hoàn tất; không tự đổi xã/tỉnh hoặc mở khóa địa chỉ nguồn nếu danh mục địa chỉ về sau thay đổi. Cơ chế xác định tọa độ phải được đối chiếu trong TDD-SITE-002.
- Phạm vi này chỉ lưu thông tin để truy vấn sau; chưa thêm tìm kiếm hoặc gợi ý nhà thầu, mẫu thiết kế hay thông tin liên quan.
- Bản US/BR mở rộng đã được người dùng chốt ngày 01/10/2026; System Test đã cập nhật. TDD-SITE-001, TDD-SITE-002 và Unit Test còn cần bổ sung phần mở rộng. Chưa triển khai hoặc chạy kiểm thử cho yêu cầu mới.
- Reviewer và Approver giữ như bản hiện có; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa xác định.
