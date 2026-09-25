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


# BR-SUB-015

## Rule Info

- **Name**: Một gói thiết kế có lựa chọn tháng và năm, giá và hạn mức cấu hình riêng.
- **Category**: Subscription thiết kế
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt giá VND dương, hạn mức riêng tháng/năm, quyền bật/tắt và mức dùng chung. Quyết định mới: đổi tháng/năm cả hai chiều ngay khi hoàn tất theo BR-SUB-021; bỏ lịch đổi cuối kỳ.

## Statement

Một gói thiết kế có hai lựa chọn tháng/năm, giá VND lớn hơn 0 và hạn mức riêng. Quyền bật/tắt và mức tính năng dùng chung trong cùng bản gói. Đổi chu kỳ cùng gói hoặc kèm đổi gói đều có hiệu lực ngay khi hoàn tất theo BR-SUB-021.

## When

Admin cấu hình gói thiết kế hoặc hệ thống xác định chu kỳ và quyền lợi hạn mức cho subscription theo lựa chọn đã được ghi nhận hợp lệ.

## Then

1. Admin cấu hình giá và hạn mức tháng riêng với giá và hạn mức năm trong cùng gói. Giá của từng lựa chọn phải lớn hơn 0. Nếu giá tháng hoặc năm bằng 0 hay âm, hệ thống từ chối lưu/công bố cấu hình đó, kể cả yêu cầu gửi trực tiếp; báo rõ lựa chọn nào có giá không hợp lệ. Không thay đổi bản đang áp dụng khi yêu cầu bị từ chối.
2. Không tự lấy giá hoặc hạn mức năm bằng 12 lần giá hoặc hạn mức tháng; dùng đúng cấu hình của từng lựa chọn.
3. Lựa chọn tháng cấp hạn mức cho kỳ tháng; lựa chọn năm cấp hạn mức cho cả kỳ năm, không tự chia hoặc làm mới mỗi tháng.
4. Tài khoản vẫn chỉ có tối đa một subscription thiết kế đang hiệu lực theo [BR-SUB-006](BR-SUB-006.md).
5. Việc lưu nháp và công bố vẫn tuân theo [BR-SUB-004](BR-SUB-004.md); không sửa hạn mức của kỳ đang sử dụng chỉ vì cấu hình được thay đổi.

6. Trong cùng một bản cấu hình gói, hai lựa chọn dùng cùng quyền bật/tắt và mức tính năng. Không cho lựa chọn năm mở thêm một tính năng hoặc mức cao hơn chỉ vì khác chu kỳ.

7. Giá tháng và năm chỉ dùng đồng Việt Nam (VNĐ, mã VND). Admin nhập và xem giá bằng VNĐ; không cấu hình giá USD hay loại tiền khác trong đợt này. Yêu cầu lưu/công bố bằng loại tiền khác bị từ chối, không tự đổi sang VNĐ hoặc hiểu số tiền đó là VNĐ.

8. Đổi tháng/năm theo cả hai chiều áp dụng BR-SUB-021: kỳ mới bắt đầu ngay khi hoàn tất, bỏ thời gian và lượt dư cũ, cấp đủ hạn mức đích, trả đủ giá kỳ mới. Không tạo lịch chuyển cuối kỳ hoặc thao tác hủy lịch chuyển.

## Except

Không áp dụng chu kỳ cho giám sát. Mua lại cùng gói và cùng chu kỳ áp dụng ngay theo BR-SUB-021; giao dịch thanh toán thiết kế sau theo giới hạn BR-SUB-021.

## Notes

- Admin nhập giá và hạn mức bán sau; không dùng số liệu thử làm giá bán. Quyền lợi hệ thống do hệ thống định nghĩa, Admin không tự tạo định nghĩa.
- BR-SUB-004 giữ quyền kỳ đã cấp khi Admin sửa danh mục; BR-SUB-021 điều chỉnh kỳ khi khách chủ động đổi.
- ST-SUB-039–041 kiểm tra cấu hình tháng/năm; ST-SUB-064–065 kiểm tra giá dương và VND. ST-SUB-101 kiểm tra đổi chu kỳ ngay.
- Các quy tắc cũ về lịch chuyển, hủy/sửa yêu cầu chờ đã bị thay thế; không dùng ST-SUB-085–087, 095, 097–099 làm nghiệm thu hiện tại.
- Tài liệu nháp, metadata chưa đủ; đặc tả chưa thực thi.

