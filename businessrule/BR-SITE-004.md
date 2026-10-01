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

# BR-SITE-004

## Rule Info

- **Name**: Tạo công trình từ một dự toán hoàn tất, giữ nguồn và khóa dữ liệu lấy từ dự toán.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng đã xác nhận trong hội thoại mở rộng hồ sơ công trình ngày 01/10/2026; bổ sung cho STORY-SITE-001 và các Story liên quan.

## Statement

Khách được tạo công trình độc lập hoặc lấy thông tin từ đúng một dự toán của mình đã hoàn tất và chưa xóa. Một dự toán chỉ liên kết với tối đa một công trình còn tồn tại. Nguồn và thông tin lấy từ nguồn không được sửa sau khi tạo.

## When

Khách mở form tạo công trình, chọn/bỏ/đổi dự toán nguồn, lưu công trình, sửa hồ sơ hoặc yêu cầu xóa dự toán đã được dùng.

## Then

1. Danh sách nguồn chỉ gồm dự toán của chính khách, hoàn tất (Succeeded), chưa xóa và chưa được công trình còn tồn tại nào sử dụng. Kiểm lại tất cả điều kiện khi lưu; trạng thái trên form không thay thế kiểm tra backend.
2. Khi chọn dự toán, lấy diện tích đất, tỉnh/thành phố, phường/xã, số nhà–đường, loại công trình, số tầng, tum, phong cách kiến trúc và nội thất từ thông tin đầu vào đã hoàn tất; dùng cấu hình của dự toán theo BR-SITE-005. Không lấy diện tích sàn do AI tính thay diện tích đất.
3. Các trường lấy từ nguồn hiển thị nhưng không cho sửa. Backend cũng phải ngăn giả mạo các giá trị này qua API. Trường không áp dụng giữ Không áp dụng, không yêu cầu khách điền để vượt khóa.
4. Tên công trình, hiện trạng, ngân sách dự kiến và dự kiến khởi công do khách nhập riêng và bắt buộc. Tệp đính kèm là tùy chọn. Không lấy tên dự toán làm tên công trình hoặc tổng chi phí kết quả làm ngân sách.
5. Trước khi tạo, khách được chọn nguồn khác hoặc bỏ nguồn. Khi đổi nguồn, các trường lấy từ nguồn phải phản ánh nguồn mới; khi bỏ nguồn, các trường đó trở lại nhập trực tiếp và được kiểm theo cấu hình cho công trình độc lập. Hồ sơ chỉ được lưu sau khi đáp ứng đầy đủ BR-SITE-001.
6. Lưu tham chiếu bằng khóa ngoại tới dự toán nguồn; công trình độc lập không có tham chiếu. Bảo đảm cùng chủ sở hữu và mỗi dự toán chỉ được một công trình còn tồn tại tham chiếu, kể cả tạo đồng thời.
7. Sau khi tạo, không đổi hoặc gỡ dự toán nguồn; công trình độc lập cũng không được bổ sung nguồn. Các trường lấy từ dự toán luôn bị khóa, kể cả chưa có gói giám sát hoặc gói đã gỡ/hủy. Các trường nhập riêng chỉ sửa theo BR-SITE-002.
8. Không đồng bộ tự động thay đổi tên dự toán hoặc danh mục về công trình. Hồ sơ giữ thông tin và cấu hình tại lúc tạo; việc đổi tên dự toán không đổi tên công trình.
9. Chặn xóa dự toán đang được công trình còn tồn tại tham chiếu theo BR-PROJ-009, kể cả công trình chưa có gói hoặc gói đã gỡ/hủy. Không tự xóa công trình hoặc gỡ nguồn để cho phép xóa dự toán.
10. Sau khi công trình được xóa hợp lệ theo BR-SITE-002, dự toán còn tồn tại được phép dùng tạo công trình mới hoặc được xóa nếu đáp ứng BR-PROJ-009. Công trình đang có gói giữ chỗ không được xóa để giải phóng nguồn.
11. Giữ liên kết mở kết quả dự toán nguồn theo quyền truy cập dự toán hiện có. Không tự sao bản vẽ kết quả, ảnh đầu vào hoặc tệp khác của dự toán vào nhóm tệp công trình; chúng không chiếm hạn mức 9 tệp của BR-SITE-007.
12. Nếu điều kiện nguồn thay đổi trước lúc lưu, từ chối tạo và giữ dữ liệu khách đang nhập để chọn lại. Không cho đồng thời tạo hai công trình dùng cùng nguồn hoặc tạo thành công bằng nguồn vừa bị xóa.

## Except

Không bắt buộc có dự toán nguồn. Việc không có gói thiết kế hoặc hết lượt không tự cấm tạo công trình từ một dự toán đủ điều kiện; tạo công trình không dùng lượt AI.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Tham chiếu [STORY-SITE-001](../userstory/STORY-SITE-001.md), [BR-SITE-001](BR-SITE-001.md), [BR-SITE-002](BR-SITE-002.md), [BR-SITE-005](BR-SITE-005.md), [BR-PROJ-009](BR-PROJ-009.md) và [STORY-PROJ-007](../userstory/STORY-PROJ-007.md).
- Quyền xem công trình không tự mở quyền xem dự toán riêng của khách cho nhân viên; giữ luồng mở kết quả và kiểm quyền hiện có. TDD sẽ xác định cách bảo đảm khóa ngoại, tính duy nhất và tranh chấp với xóa dự toán.
- Nội dung nghiệp vụ đã được xác nhận qua hội thoại; bản US/BR cụ thể đã được người dùng chốt ngày 01/10/2026; đặc tả System Test đã được cập nhật. Chưa triển khai phần mở rộng hoặc chạy kiểm thử cho phần này.
- Reviewer/Approver giữ theo bộ tài liệu SITE hiện có; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa xác định.
