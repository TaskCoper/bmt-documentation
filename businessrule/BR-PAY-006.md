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

# BR-PAY-006

## Rule Info

- **Name**: Quản lý kết nối nhận tiền SePay bằng quyền riêng và chọn kết nối đang dùng theo môi trường.
- **Category**: Quản trị thanh toán
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định người dùng xác nhận ngày 26/09/2026 (bốn điểm ban đầu và câu trả lời Q1–Q7), chuyển qua agent điều phối trong đợt làm tiếp thanh toán; nguyên tắc không đổi tài khoản của kết nối đã có đơn lấy từ TDD-PAY-001/Data Model.

## Statement

Chỉ người có quyền `payment.connection.manage` được xem, tạo, sửa kết nối nhận tiền SePay, xem lịch sử và chọn kết nối đang dùng cho từng môi trường Test/Live. Đơn mới dùng kết nối đang dùng của môi trường máy chủ; đơn đã tạo giữ kết nối lúc tạo.

## When

Người dùng xem hoặc thay đổi kết nối nhận tiền, chọn kết nối đang dùng, khách tạo đơn thanh toán mới, hoặc máy chủ khởi động.

## Then

1. Kiểm tra quyền `payment.connection.manage` cho mọi thao tác quản lý kết nối, kể cả yêu cầu gửi trực tiếp. Admin có quyền này mặc định.
2. Secret HMAC của kết nối không đi qua API: API không nhận và không trả secret. Secret đặt ở biến môi trường `SePayOption__Connections__{n}__ConnectionId` và `SePayOption__Connections__{n}__WebhookSecret` theo Id kết nối.
3. Mỗi môi trường Test và Live có tối đa một kết nối đang dùng, do người có quyền chọn qua API. Không còn chọn bằng biến môi trường `SePayOption__ActiveConnectionId`. Chỉ chọn được kết nối đang bật, cùng môi trường và máy chủ đã có secret của nó.
4. Đơn mới dùng kết nối đang dùng tại lúc tạo đơn. Đơn đã tạo giữ kết nối lúc tạo; đổi kết nối đang dùng không đổi kết nối, số tiền hay nội dung chuyển khoản của đơn cũ.
5. Kết nối đã có đơn hoặc đã có giao dịch webhook thì không đổi tài khoản nhận (ngân hàng, số tài khoản, VA, mã ngân hàng QR). Muốn đổi tài khoản nhận thì tạo kết nối mới rồi chọn làm kết nối đang dùng. Kết nối chưa có đơn và chưa có giao dịch webhook thì sửa được các trường này. Bật/tắt sửa được ở mọi kết nối, theo khoản 6. Môi trường của kết nối không đổi sau khi tạo.
6. Không tắt được kết nối đang được chọn cho môi trường; phải chọn kết nối khác trước. Kết nối không được chọn thì tắt được, kể cả khi còn đơn chờ thanh toán; webhook của kết nối đã tắt vẫn được nhận.
7. Không xóa kết nối; ngừng dùng bằng cách tắt.
8. Môi trường của máy chủ chưa có kết nối đang dùng hợp lệ thì tạo đơn trả 503 `PaymentUnavailable`. Không có thao tác bỏ chọn; chỉ đổi sang kết nối khác.
9. Máy chủ biết mình thuộc Test hay Live qua biến `SePayOption__Environment`. Thiếu hoặc sai giá trị thì ứng dụng không khởi động. Biến này chỉ chọn môi trường, không chọn kết nối.
10. Mỗi lần tạo, sửa, chọn kết nối đang dùng thành công được ghi lịch sử: ai làm, lúc nào, làm gì, giá trị trước và sau. Lịch sử không chứa secret và xem được qua API.
11. Hệ thống hỗ trợ cả tài khoản chính và tài khoản ảo (VA). Chưa biết có dùng VA hay không; trước khi mở nhận tiền thật phải thử với tài khoản SePay thật.

## Except

Không có ngoại lệ cho các khoản trên. Yêu cầu bị từ chối không thay đổi dữ liệu và không ghi lịch sử.

## Notes

- Quyền `payment.connection.manage` là quyền mới, độc lập với `commerce.read` của BR-PAY-005.
- Khoản 5–10 lần lượt theo câu trả lời Q1, Q2, Q3, Q4, Q5 và Q7 ngày 26/09/2026. Q6 thuộc cách cảnh báo webhook lệch, ghi ở TDD-PAY-001.
- Bản nháp nghiệp vụ. Code đã có trên nhánh `feature/payment-alerts-connections` của `bmt-be`, chưa merge. Reviewer/Approver theo quy ước đã dùng cho bản nháp PAY, không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- Xem [STORY-PAY-003](../userstory/STORY-PAY-003.md) và [TDD-PAY-001](../tdd/TDD-PAY-001.md).
