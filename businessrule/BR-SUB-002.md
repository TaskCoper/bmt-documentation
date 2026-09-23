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


# BR-SUB-002

## Rule Info

- **Name**: Làm mới hạn mức theo kỳ subscription, không cộng dồn lượt dư.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng xác nhận: làm mới mỗi kỳ subscription; bỏ 3 lượt dư, kỳ mới có 10 lượt.

## Statement

Quy tắc này chỉ áp dụng cho subscription thiết kế. Gói giám sát không có chu kỳ hoặc ngày hết hạn; xem [BR-SUB-011](BR-SUB-011.md).

Hạn mức lượt được làm mới mỗi kỳ subscription, theo lựa chọn tháng hoặc năm của subscription trong gói. Lựa chọn năm cấp hạn mức cho cả kỳ năm, không làm mới mỗi tháng. Lượt dư của kỳ cũ không được cộng vào kỳ mới.

## When

Tài khoản bắt đầu một kỳ subscription mới hợp lệ.

## Then

Số lượt thiết kế bằng hạn mức được cấp cho kỳ mới, dùng chung theo tài khoản. Số lượt dư của kỳ cũ không còn được sử dụng và không làm tăng hạn mức kỳ mới.

## Except

Kỳ mới do đổi gói hoặc chu kỳ giữa kỳ cũng cấp đủ hạn mức gói đích và bỏ lượt dư chưa dùng của kỳ cũ theo [BR-SUB-021](BR-SUB-021.md). Lượt đang giữ vẫn xử lý theo kỳ đã giữ, không xóa lịch sử hoặc chuyển lượt giữ sang kỳ mới.


Không cộng dồn lượt dư trong phạm vi quy tắc đã xác nhận. Lượt đang giữ cho tác vụ tạo thiết kế vẫn thuộc kỳ cũ. Khi tác vụ kết thúc, xử lý trên kỳ cũ theo [BR-SUB-003](BR-SUB-003.md), không thay đổi hạn mức kỳ mới.

## Notes

- Ví dụ: kỳ cũ còn 3 lượt, kỳ mới có hạn mức 10 thì tài khoản có 10 lượt, không phải 13. Đây là dữ liệu minh họa.
- Quy tắc này không tự gia hạn subscription. Luồng nhận gói và gia hạn sẽ thiết kế cùng thanh toán sau.
- Ngày kết thúc tính từ ngày bắt đầu theo [BR-SUB-014](BR-SUB-014.md); không theo lịch tháng/năm chung. Kỳ kết thúc đúng giờ bắt đầu theo giờ Việt Nam, không kéo dài đến hết ngày.
- [STORY-SUB-001](../userstory/STORY-SUB-001.md), [BR-SUB-001](BR-SUB-001.md) và [ST-SUB-002](../systemtest/ST-SUB-002.md) mô tả hành vi dùng chung và kiểm thử làm mới hạn mức.
- Giá và hạn mức riêng cho từng lựa chọn theo [BR-SUB-015](BR-SUB-015.md). Hạn mức kỳ mới lấy từ bản quyền lợi đã chốt cho kỳ đó theo [BR-SUB-004](BR-SUB-004.md), không tự lấy bản công bố mới nhất.
- Bản nháp còn thiếu metadata; chưa phê duyệt.
