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

# BR-SUB-023

## Rule Info

- **Name**: [ĐÃ BỎ] Nhân viên có quyền riêng được đổi công trình đã gán và phải ghi lý do.
- **Category**: Thanh toán và gói dịch vụ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế thanh toán ngày 19/09/2026; xem discovery/payment-packages.md. Quyết định mới nhất được ưu tiên khi thay thế phương án trước đó.

## Statement

**Đã bỏ ngày 25/09/2026:** Người dùng xác nhận gói đã gắn công trình thì không đổi sang công trình khác, kể cả nhân viên và Admin. Quy tắc này và mã quyền `supervision.reassign` không còn áp dụng; xem [BR-SUB-009](BR-SUB-009.md). Nội dung dưới đây giữ để tra lịch sử, không dùng làm căn cứ nghiệm thu hay triển khai.

Khách không được tự gỡ hoặc đổi công trình của gói giám sát. Nhân viên có quyền riêng được đổi công trình đã gán, bắt buộc nhập lý do.

## When

Có yêu cầu đổi công trình của gói giám sát đã gán.

## Then

1. Từ chối khách tự gỡ hoặc tự đổi; hướng dẫn liên hệ nhân viên. Nhân viên thiếu quyền riêng cũng không được sửa. Người đổi phải là tài khoản nhân viên đang hoạt động có quyền `supervision.reassign` theo [BR-RBAC-010](BR-RBAC-010.md).
2. Nhân viên có quyền phải nhập lý do có nội dung. Công trình đích thuộc cùng khách hàng và chưa có gói giám sát khác đang hiệu lực.
3. Sửa liên kết sang công trình đích; không tạo gói mới, không thu tiền lần nữa, không đổi chủ gói hoặc quyền lợi đã mua.
4. Gói từng gán đúng hạn vẫn được sửa sau một năm từ lúc mua. Không làm mới hạn gán ban đầu hoặc dùng thao tác sửa để cứu gói chưa từng gán đã hết hạn.
5. Không kiểm tra điều kiện đã/chưa khảo sát hoặc giám sát, vì hệ thống chưa quản lý những hoạt động đó.

## Except

Không đổi công trình của gói đã hoàn thành; phải mở lại gói trước theo [BR-SUB-012](BR-SUB-012.md). Không sửa gói khác sang công trình đang có gói đã hoàn thành, vì gói đó vẫn giữ chỗ.

Chưa xác nhận quyền nhân viên gỡ gói về trạng thái chưa gán hoặc sửa liên kết của gói đang bị hủy. Không suy ra các thao tác này từ quyền đổi công trình.

## Notes

Thay giới hạn cũ “gắn cố định, không có ngoại lệ sửa nhầm” trong BR-SUB-009. Không đồng nghĩa khách được dùng đồng thời một gói cho nhiều công trình.

- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Tên Reviewer/Approver lấy theo xác nhận cho các bản nháp mới trong discovery/subscription-entitlements.md; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- [Tổng hợp quyết định và bảng truy vết](../discovery/payment-packages.md).
