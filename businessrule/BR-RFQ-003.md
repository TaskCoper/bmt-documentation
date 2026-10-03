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

# BR-RFQ-003

## Rule Info

- **Name**: Giữ nguyên hồ sơ và tệp tại thời điểm gửi; chặn xóa hồ sơ đã có lời mời.
- **Category**: Mời báo giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại chuẩn bị tính năng Mời báo giá ngày 03/10/2026. Người dùng đã chốt bộ US/BR và giao triển khai; chưa phê duyệt hoặc import trên hệ thống quản lý tài liệu.

## Statement

Mỗi lời mời giữ một bản hồ sơ tại thời điểm gửi, gồm thông tin và các tệp đã đính kèm. Admin xem bản này khi xử lý. Hồ sơ đã có ít nhất một lời mời thì không được xóa.

## When

Hệ thống tiếp nhận lời mời, người có quyền đọc bản đã gửi, hoặc khách sửa, gỡ tệp hay xóa hồ sơ gốc.

## Then

1. Lưu thông tin hồ sơ và các bản vẽ, ảnh hiện trạng đã đính kèm tại thời điểm tiếp nhận thành công. Không yêu cầu bổ sung tệp nếu hồ sơ chưa có tệp.
2. Bản lưu phải phản ánh cùng hồ sơ của khách đã dùng để gửi lời mời, không thay bằng hồ sơ của khách khác hoặc dữ liệu mới nhất khi admin mở yêu cầu.
3. Khách sửa thông tin hoặc thêm, thay, gỡ tệp trên hồ sơ gốc thì bản đã gửi vẫn giữ nguyên. Admin vẫn xem được nội dung tệp đã gửi sau khi tệp bị gỡ khỏi hồ sơ gốc.
4. Mỗi lần mời một nhà thầu khác lưu hồ sơ tại thời điểm gửi lần đó. Nếu khách sửa hồ sơ giữa hai lần gửi thì hai lời mời có thể giữ hai bản khác nhau.
5. Quyền sửa hồ sơ gốc vẫn theo các quy tắc hiện có của hồ sơ; lời mời không tự khóa sửa hoặc mở khóa những trường vốn bị khóa.
6. Từ khi có một lời mời được tiếp nhận, chặn xóa hồ sơ gốc, kể cả khi tất cả lời mời đã Hoàn tất hoặc nhà thầu đã bị ẩn. Giữ hồ sơ để khách tiếp tục theo dõi lời mời.
7. Sửa lịch, trạng thái hoặc ghi chú nội bộ của lời mời không được sửa bản hồ sơ và các tệp đã lưu.
8. Quyền quản lý lời mời cho phép xem bản hồ sơ và tệp đã gửi phục vụ xử lý yêu cầu; không tự cấp quyền sửa hồ sơ gốc. Tệp không trở thành công khai chỉ vì được lưu cùng lời mời.

## Except

Hồ sơ chưa có lời mời vẫn áp dụng điều kiện xóa hiện có. Không có tệp đính kèm thì bản lưu gồm thông tin hồ sơ, không tạo tệp giả.

## Notes

- Phục vụ STORY-RFQ-001 và STORY-RFQ-002.
- Người dùng đã xác nhận giữ cả thông tin và tệp, chặn xóa hồ sơ đã có lời mời. Cần cập nhật điều kiện xóa hồ sơ và vòng đời tệp hiện có khi tích hợp.
- Cơ chế lưu bản bất biến, bảo vệ tệp khỏi bị dọn khi gỡ liên kết gốc, và phạm vi dữ liệu cụ thể thuộc thiết kế kỹ thuật. Không tự mở quyền đọc toàn bộ dự toán nguồn hoặc các gói giám sát từ quyền quản lý lời mời.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại. Owner và ngày hiệu lực chưa xác định. Người dùng đã chốt bộ US/BR này và giao triển khai. Đã soạn đặc tả System Test; chưa triển khai mã ứng dụng hoặc chạy kiểm thử. Status Draft không thay thế xác nhận hội thoại.
