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


# BR-SUB-016

## Rule Info

- **Name**: Tự kết thúc tác vụ tạo thiết kế quá thời gian chờ và giải phóng lượt.
- **Category**: Subscription thiết kế
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chọn tự đánh dấu tác vụ thất bại khi quá thời gian chờ, giải phóng lượt đang giữ và không tự trừ lại nếu kết quả đến muộn. Người dùng chọn không đưa kết quả muộn cho khách, giữ tác vụ thất bại. Thời gian chờ cấu hình riêng; lúc đầu 15 phút chỉ là ví dụ, ngày 26/09/2026 người dùng chốt dùng 15 phút.

## Statement

Tác vụ tạo thiết kế chưa có kết quả cuối cùng khi hết thời gian chờ được cấu hình sẽ tự chuyển sang thất bại do quá thời gian. Hệ thống giải phóng lượt đang giữ của tác vụ. Kết quả đến muộn sau khi đã xử lý quá thời gian không được đưa vào kết quả của khách, không chuyển tác vụ thành thành công và không được tự tính lượt lại.

## When

Một tác vụ tạo thiết kế còn đang chờ kết quả đã vượt thời gian chờ được cấu hình; hoặc có kết quả đến sau khi tác vụ đã được xử lý thất bại do quá thời gian.

## Then

1. Tự ghi nhận tác vụ thất bại do quá thời gian và giải phóng lượt giữ, không chờ nhân viên xử lý thủ công.
2. Nếu kỳ giữ lượt còn hiệu lực, lượt được dùng lại. Nếu kỳ đã hết hạn, chỉ giải phóng lượt giữ của kỳ cũ; không làm lượt hết hạn dùng lại được, không cộng sang kỳ mới.
3. Kết quả đến muộn không tự ghi nhận lượt đã dùng, giữ lại lượt hoặc trừ lượt ở kỳ cũ hay kỳ mới.
4. Khi nhận kết quả muộn của tác vụ đã thất bại do quá thời gian, giữ nguyên trạng thái thất bại và không công bố kết quả đó cho khách qua giao diện hoặc API. Không gắn kết quả muộn vào yêu cầu mới nếu khách đã gửi lại.
5. Tác vụ đã hoàn thành thành công và tính lượt trước khi xử lý quá thời gian không bị đổi thành thất bại hoặc trả lượt bởi xử lý quá thời gian đến sau.

## Except

Không tự gửi lại tác vụ hoặc tạo tác vụ mới từ xử lý quá thời gian. Việc subscription hết hạn không phải nguyên nhân tự hủy tác vụ; thời gian chờ xử lý là điều kiện riêng.

## Notes

- Thời gian chờ là cấu hình riêng. Ngày 26/09/2026 người dùng chốt: thời gian chờ một lần tạo thiết kế là 15 phút, tính từ lúc hệ thống tiếp nhận tác vụ; hệ thống rà tác vụ quá hạn mỗi 60 giây. Đây là hai giá trị cấu hình (`EstimateAiOption__GenerationTimeoutMinutes`, `UsageMaintenanceOption__ScanIntervalSeconds`, xem TDD-SUB-002/Architecture), không làm đổi quy tắc: quy tắc áp dụng với thời gian chờ đang được cấu hình.
- Mốc bắt đầu đo thời gian, cơ chế phát hiện, xử lý kết quả đến sát hạn và chống giải phóng/trừ lặp sẽ được làm rõ trong TDD; không cho một tác vụ vừa hoàn lượt do quá thời gian vừa tính lượt thành công đến muộn.
- Việc không đưa kết quả muộn cho khách không đồng nghĩa với yêu cầu xóa ngay dữ liệu nội bộ. Cách lưu phục vụ đối soát, thời hạn lưu và dọn dữ liệu thuộc thiết kế kỹ thuật, chưa chốt ở đây.
- Với không giới hạn lượt, vẫn xử lý kết quả quá thời gian; không có số dư hữu hạn để hoàn.
- Tham chiếu [BR-SUB-003](BR-SUB-003.md), [BR-SUB-002](BR-SUB-002.md), [STORY-SUB-001](../userstory/STORY-SUB-001.md), [ST-SUB-042](../systemtest/ST-SUB-042.md), [ST-SUB-043](../systemtest/ST-SUB-043.md) và [ST-SUB-044](../systemtest/ST-SUB-044.md).
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
