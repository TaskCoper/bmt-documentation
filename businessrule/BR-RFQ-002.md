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

# BR-RFQ-002

## Rule Info

- **Name**: Giới hạn nhà thầu theo từng hồ sơ và áp dụng cấu hình hiện tại.
- **Category**: Mời báo giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị tính năng Mời báo giá ngày 03/10/2026. Người dùng đã chốt bộ US/BR và giao triển khai; chưa phê duyệt hoặc import trên hệ thống quản lý tài liệu.

## Statement

Mỗi hồ sơ mặc định được gửi lời mời đến tối đa 3 nhà thầu khác nhau. Mỗi cặp hồ sơ–nhà thầu chỉ có một lời mời. Giới hạn được lưu bằng cấu hình và áp dụng chung cho mọi hồ sơ ngay khi thay đổi.

## When

Khách gửi lời mời hoặc người có quyền thay đổi giới hạn.

## Then

1. Đếm số nhà thầu đã được mời theo từng hồ sơ, không cộng chung các hồ sơ của một khách. Giới hạn ban đầu là 3.
2. Một hồ sơ đã gửi cho nhà thầu A thì không được tạo lời mời thứ hai cho A, kể cả yêu cầu trước đã Hoàn tất hoặc nhà thầu đã bị ẩn rồi hiện lại.
3. Chỉ nhận lời mời tới nhà thầu chưa từng được mời khi số nhà thầu đã mời của hồ sơ còn nhỏ hơn giới hạn hiện hành.
4. Mọi lời mời đã tiếp nhận đều tính vào giới hạn, không phụ thuộc trạng thái. Đổi trạng thái, đổi lịch hoặc ẩn nhà thầu không hoàn lại lượt.
5. Giới hạn là cấu hình dùng chung có thể thay đổi, không phải giá trị cố định chỉ được sửa trong mã ứng dụng và không cố định theo ngày tạo hồ sơ.
6. Tăng từ 3 lên 5 cho phép hồ sơ đã mời 3 nhà thầu mời thêm tối đa 2 nhà thầu khác. Vẫn không được gửi lại cho nhà thầu cũ.
7. Giảm từ 3 xuống 2 giữ nguyên các lời mời đã có. Hồ sơ đã mời 2 hoặc nhiều hơn bị chặn gửi thêm; không tự xóa lời mời để đưa số đã gửi về giới hạn mới.
8. Quy tắc không trùng và không vượt giới hạn phải đúng cả khi có nhiều yêu cầu gửi đồng thời. Yêu cầu bị từ chối không tạo lời mời hoặc tiêu tốn thêm lượt.
9. Giới hạn phải là số nguyên từ 1 trở lên. Từ chối 0, số âm và số không nguyên; không tự làm tròn hoặc dùng 0 để tạm ngừng gửi hay biểu thị không giới hạn. Thay đổi bị từ chối giữ nguyên cấu hình đang dùng.
10. Admin và người được cấp quyền cấu hình riêng được sửa giới hạn. Quyền quản lý lời mời không tự bao gồm quyền cấu hình; người không có quyền cấu hình bị từ chối thay đổi.

## Except

Số lời mời cũ được phép lớn hơn giới hạn mới sau khi giảm cấu hình; đây không phải quyền gửi thêm.

## Notes

- Phục vụ STORY-RFQ-001 và STORY-RFQ-003.
- Người dùng đã xác nhận giới hạn là số nguyên từ 1 trở lên và quyền sửa cấu hình ở khoản 9–10. Giới hạn biểu diễn kỹ thuật và mã quyền cụ thể sẽ được xác định trong TDD; không tự đặt trần nghiệp vụ. Giá trị mặc định 3 và cách áp dụng khi tăng/giảm cũng đã được xác nhận.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại. Owner và ngày hiệu lực chưa xác định. Người dùng đã chốt bộ US/BR này và giao triển khai. Đã soạn đặc tả System Test; chưa triển khai mã ứng dụng hoặc chạy kiểm thử. Status Draft không thay thế xác nhận hội thoại.
