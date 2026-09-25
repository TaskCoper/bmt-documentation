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

# BR-SUB-024

## Rule Info

- **Name**: Nhân viên có quyền riêng được hủy hiệu lực cả hai loại gói; hủy không hoàn tác được.
- **Category**: Thanh toán và gói dịch vụ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế thanh toán ngày 19/09/2026; xem discovery/payment-packages.md. Quyết định mới nhất được ưu tiên khi thay thế phương án trước đó.

## Statement

**Cập nhật 25/09/2026:** Người dùng bỏ thao tác khôi phục cho cả gói thiết kế và gói giám sát; [BR-SUB-025](BR-SUB-025.md) không còn áp dụng. Hủy là thao tác cuối cùng. Hủy gói giám sát kết thúc phân công của gói, và gói đã hủy không còn khóa việc sửa hoặc xóa công trình.

Nhân viên có quyền riêng được hủy hiệu lực gói thiết kế hoặc giám sát, bắt buộc nhập lý do. Hủy có hiệu lực ngay, không hoàn tác được và giữ lịch sử.

## When

Nhân viên yêu cầu hủy một gói đã cấp.

## Then

1. Kiểm tra quyền riêng và lý do; không mặc định mọi nhân viên được hủy. Thiếu quyền hoặc lý do thì từ chối, giữ nguyên gói.
2. Cho phép hủy gói giám sát chưa gán, đã gán hoặc đã hoàn thành, cũng như gói thiết kế. Gói đã hủy không còn cấp quyền sử dụng mới.
3. Hủy gói giám sát đã gán hoặc đã hoàn thành thì gói nhả chỗ và công trình được nhận gói giám sát khác. Gói đã hủy không khóa việc sửa hoặc xóa công trình theo [BR-SITE-002](BR-SITE-002.md). Giữ lịch sử gói, trạng thái trước khi hủy, thao tác hủy, cùng tên và địa chỉ công trình tại lúc hủy.
4. Hệ thống không chuyển tiền, không đánh dấu đã hoàn tiền và không tự cấp lại gói từ quyết định hoàn tiền bên ngoài.
5. Nếu khảo sát sau mua thấy công trình không phù hợp, nhân viên hoàn tiền hoặc bù trừ với khách bên ngoài hệ thống, chẳng hạn qua điện thoại, và hủy hiệu lực gói trong hệ thống.
6. Hủy gói thiết kế mới không tự khôi phục gói cũ đã bị lần mua mới thay thế.
7. Hủy gói giám sát kết thúc ngay phân công đang hiệu lực của gói và ghi nhật ký phân công theo [BR-RBAC-013](BR-RBAC-013.md) khoản 9. Gói mới gắn vào cùng công trình sau đó cần phân công riêng.
8. Không có thao tác khôi phục gói đã hủy, cho cả gói thiết kế và gói giám sát. Nếu hủy nhầm, nhân viên xử lý tiền với khách bên ngoài hệ thống; khách còn nhu cầu thì mua gói mới theo [STORY-PAY-001](../userstory/STORY-PAY-001.md).
9. Khách vẫn thấy gói đã hủy trong danh sách gói của mình với trạng thái đã hủy. Nếu công trình của gói đã bị xóa, hiện tên công trình tại lúc hủy.
10. Trước khi gửi yêu cầu hủy, giao diện hiện hộp xác nhận nói rõ việc hủy không hoàn tác được; nhân viên nhập lý do trong hộp này. Hệ thống vẫn kiểm tra quyền và lý do trên yêu cầu xử lý theo khoản 1, không chỉ dựa vào hộp xác nhận.

## Except

Tác vụ AI đã bắt đầu hợp lệ trước khi hủy được tiếp tục theo quyền và lượt đã giữ. Chỉ chặn tác vụ mới. Thành công/lỗi/timeout vẫn quyết toán vào kỳ cũ; không cấp hoặc cộng lượt vào kỳ khác.

## Notes

Hủy hiệu lực gói khác với hủy đơn chưa nhận tiền. Việc hoàn tiền không cần API hoặc màn hình xác nhận hoàn tiền trong phạm vi này.

- Người dùng xác nhận ngày 25/09/2026: bỏ khôi phục cho cả hai loại gói; hủy nhầm thì xử lý tiền bên ngoài và khách mua lại nếu cần; có hộp xác nhận trước khi hủy; hủy gói giám sát kết thúc phân công; gói đã hủy không khóa sửa hoặc xóa công trình; trạng thái hiển thị là "đã hủy". Khoản 7 đổi từ “giữ phân công khi hủy” sang “kết thúc phân công khi hủy”; khôi phục theo BR-SUB-025 không còn áp dụng. Khoản 8 đến 10 thêm cùng ngày.
- Hủy khác gỡ gói: gỡ theo [BR-SUB-026](BR-SUB-026.md) giữ gói dùng được để khách gán lại, còn hủy chấm dứt gói.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Tên Reviewer/Approver lấy theo xác nhận cho các bản nháp mới trong discovery/subscription-entitlements.md; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- [Tổng hợp quyết định và bảng truy vết](../discovery/payment-packages.md).
