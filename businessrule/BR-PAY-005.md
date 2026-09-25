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

# BR-PAY-005

## Rule Info

- **Name**: Admin và nhân viên có quyền tra cứu được xem người mua, gói, đơn và giao dịch.
- **Category**: Quản trị thanh toán
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng bổ sung quản lý ai mua gói nào và các giao dịch; xác nhận hiển thị giao dịch chưa khớp đơn và cho cả Admin, nhân viên có quyền tra cứu riêng được xem trong hội thoại thiết kế thanh toán.

## Statement

Admin và nhân viên được cấp quyền tra cứu riêng được xem người mua, gói đã mua, đơn thanh toán và từng giao dịch nhận tiền trong hệ thống. Giao dịch chưa khớp đơn vẫn được hiển thị với nhãn “Chưa xác định đơn”. Quyền tra cứu không tự cấp quyền đổi công trình, hủy hoặc khôi phục gói.

## When

Người dùng truy cập danh sách hoặc chi tiết quản trị gói đã mua, đơn thanh toán hoặc giao dịch.

## Then

1. Kiểm tra người gọi là Admin hoặc nhân viên có quyền tra cứu riêng. Áp dụng cho cả danh sách và chi tiết, kể cả yêu cầu trực tiếp; không chỉ ẩn nút trên giao diện.
2. Cho biết khách nào đã mua gói nào và liên kết đơn mua tương ứng. Gói giám sát chưa gán vẫn thuộc danh sách gói đã mua.
3. Cho tra cứu đơn và các giao dịch thực tế liên quan, gồm các khoản chuyển bổ sung; phân biệt tiền từng giao dịch với tổng tiền nhận của đơn. Không coi webhook gửi lại là giao dịch tiền mới.
4. Giữ khả năng tra cứu đơn đang chờ, nhận thiếu, hết hạn, đã hủy và đã thanh toán; gói không còn hiệu lực vẫn là lịch sử mua, không được trình bày như gói đang dùng.
5. Giao dịch chưa khớp đơn vẫn có thông tin giao dịch đã nhận và nhãn “Chưa xác định đơn”. Không tự gán khách hàng/gói từ suy đoán và chưa hỗ trợ gán thủ công.
6. Quyền xem không thay thế các quyền riêng ở BR-SUB-023, BR-SUB-024 và BR-SUB-025. Không ghi nhận hoàn tiền, chuyển tiền hoặc xác nhận cấp gói thủ công từ màn hình tra cứu.

## Except

Người không có quyền bị từ chối, không nhận dữ liệu quản trị. Việc chưa khớp đơn không phải lý do để ẩn giao dịch hợp lệ đã nhận. Quy tắc không cho phép tự sửa dữ liệu tiền, gán giao dịch thủ công hoặc cấp gói từ giao dịch chưa khớp.

## Notes

- Truy vết giao dịch của đơn và bản giá/quyền lợi đã lưu dùng lại BR-PAY-001, BR-PAY-002 và BR-PAY-004; không tạo luồng tính tiền khác cho quản trị.
- Danh sách trường, tìm kiếm/bộ lọc, phân trang và bố trí màn hình sẽ cụ thể hóa ở bước thiết kế. Không tự bổ sung báo cáo doanh thu, xuất file hoặc quyền chỉnh sửa từ yêu cầu xem.
- Hợp đồng SePay và cách lưu giao dịch chưa khớp còn cần thiết kế kỹ thuật; không khẳng định tên trường provider trong tài liệu nghiệp vụ.
- Xem [STORY-PAY-002](../userstory/STORY-PAY-002.md) và [tổng hợp](../discovery/payment-packages.md). Bản nháp, chưa triển khai hoặc chạy test; Owner/ngày hiệu lực chưa xác định. Reviewer/Approver theo xác nhận đã có cho bản nháp mới, không phải bằng chứng đã duyệt.
