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

# BR-SUB-026

## Rule Info

- **Name**: Nhân viên có quyền riêng được gỡ gói giám sát đã gán khỏi công trình để khách gán lại.
- **Category**: Thanh toán và gói dịch vụ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận ngày 25/09/2026 khi bàn luận gói giám sát: khách gán nhầm công trình thì gọi tổng đài; nhân viên gỡ gói về chưa gán để khách tự gán lại; gói giữ hạn gán ban đầu. Xem discovery/payment-packages.md.

## Statement

Khi khách gán nhầm công trình, nhân viên có quyền `supervision.unassign` được gỡ gói giám sát đang ở trạng thái đã gán khỏi công trình, bắt buộc nhập lý do. Gói quay về chưa gán, giữ hạn gán ban đầu, và khách tự gán lại. Khách không tự gỡ được gói.

## When

Có yêu cầu gỡ một gói giám sát khỏi công trình đang gắn với gói.

## Then

1. Người gỡ phải là tài khoản nhân viên đang hoạt động có quyền `supervision.unassign` theo [BR-RBAC-010](BR-RBAC-010.md); không cần được phân công gói. Từ chối khách hàng, kể cả chủ gói, và nhân viên thiếu quyền, kể cả khi gửi thẳng tới API.
2. Người gỡ phải nhập lý do có nội dung; lý do rỗng hoặc chỉ có khoảng trắng bị coi là thiếu. Ngoài lý do, không đặt thêm điều kiện: không giới hạn số lần gỡ, không giới hạn thời gian kể từ lúc gán và không đối chiếu dữ liệu khảo sát/giám sát đang vận hành offline.
3. Chỉ gỡ được gói đang ở trạng thái đã gán. Từ chối gói chưa gán, đã hoàn thành hoặc đã hủy.
4. Từ chối gỡ khi đã đến hoặc đã qua hạn gán ban đầu theo [BR-SUB-022](BR-SUB-022.md), và báo cho nhân viên biết lý do. Gói vẫn gắn công trình cũ. Muốn chấm dứt gói thì nhân viên dùng thao tác hủy theo [BR-SUB-024](BR-SUB-024.md).
5. Khi gỡ hợp lệ, gói về chưa gán và không còn giữ chỗ trên công trình cũ. Giữ nguyên chủ gói, mô tả dịch vụ, quyền lợi đã cấp và hạn gán ban đầu. Không thu thêm tiền, không hoàn tiền và không tạo gói mới.
6. Phân công đang hiệu lực của gói kết thúc ngay khi gỡ và được ghi nhật ký phân công theo [BR-RBAC-013](BR-RBAC-013.md).
7. Mỗi lần gỡ, hệ thống lưu người gỡ, thời điểm, lý do và công trình cũ, gồm tên và địa chỉ công trình tại lúc gỡ. Chỉ nhân viên có quyền `commerce.read` xem được lịch sử gỡ, cùng chỗ tra cứu gói đã mua theo [BR-PAY-005](BR-PAY-005.md); quyền `supervision.unassign` không kèm quyền xem lịch sử này. Khách chỉ thấy gói đang chưa gán và hạn gán còn lại; không thấy công trình cũ, thời điểm gỡ hay lý do gỡ. Nếu khách chưa gán lại tới hạn, gói hiện trạng thái quá hạn gán theo [BR-SUB-022](BR-SUB-022.md).
8. Sau khi gỡ, khách tự gán lại theo đúng các điều kiện ở [BR-SUB-022](BR-SUB-022.md): công trình thuộc chính khách, công trình chưa có gói giữ chỗ và việc gán diễn ra trước hạn gán ban đầu. Khách được gán lại vào chính công trình cũ. Sau khi gán lại, gói chưa có người phụ trách và vào danh sách cần chia lại theo [BR-RBAC-013](BR-RBAC-013.md) khoản 6.

## Except

Không có thao tác đổi thẳng gói sang công trình khác, kể cả với Admin; nhân viên chỉ gỡ, khách tự gán lại theo [BR-SUB-009](BR-SUB-009.md). Không có ngoại lệ cho khách tự gỡ gói.

## Notes

- Quy tắc này khác [BR-SUB-023](BR-SUB-023.md) đã bỏ: BR-SUB-023 cho nhân viên đổi thẳng công trình, còn quy tắc này chỉ cho gỡ để khách tự chọn lại công trình đúng.
- Gói đã hoàn thành được mở lại theo [BR-SUB-012](BR-SUB-012.md) thì trở về đã gán, nên gỡ được theo quy tắc này.
- Sau khi gỡ, nếu công trình cũ không còn gói nào giữ chỗ thì khách sửa hoặc xóa được công trình theo [BR-SITE-002](BR-SITE-002.md).
- Ví dụ: gói cấp 01/10/2026 lúc 10:00 nên hạn gán là 01/10/2027 lúc 10:00. Khách gán nhầm công trình A ngày 20/09/2027. Nếu nhân viên gỡ ngày 25/09/2027, gói về chưa gán và khách phải gán lại trước 01/10/2027 lúc 10:00. Nếu nhân viên gỡ ngày 05/10/2027, yêu cầu bị từ chối vì đã quá hạn gán; gói vẫn gắn A.
- Mã quyền `supervision.unassign` là tên đề xuất; tên cuối cùng chốt cùng danh mục quyền ở TDD-RBAC-001.
- Người dùng xác nhận ngày 25/09/2026: chỉ người có `commerce.read` xem lịch sử gỡ; gói đã gỡ mà quá hạn gán hiển thị giống gói chưa từng gán đã quá hạn.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Tên Reviewer/Approver lấy theo xác nhận cho các bản nháp mới trong discovery/subscription-entitlements.md; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- [Tổng hợp quyết định và bảng truy vết](../discovery/payment-packages.md).
