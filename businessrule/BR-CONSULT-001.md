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

# BR-CONSULT-001

## Rule Info

- **Name**: Quản lý và ẩn hiện hồ sơ KTS
- **Category**: Tư vấn kiến trúc sư
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại nghiên cứu tính năng tư vấn KTS ngày 2026-09-23. Người dùng đã chốt bộ US/BR trong hội thoại.

## Statement

Admin quản lý hồ sơ KTS; chỉ hồ sơ đang hiển thị được nhận yêu cầu mới.

## When

Admin thêm, sửa, ẩn/hiện hồ sơ hoặc khách gửi yêu cầu cho KTS.

## Then

1. Hồ sơ gồm ảnh đại diện, họ tên, chức danh, chuyên môn, số năm kinh nghiệm, số công trình và giới thiệu.
2. Người có quyền quản lý tư vấn KTS theo STORY-RBAC-001 được thêm, sửa, ẩn và hiện hồ sơ; không có bước phê duyệt hồ sơ.
3. KTS bị ẩn không được nhận yêu cầu mới. Hệ thống kiểm tra khi tiếp nhận, kể cả khi khách đã mở trang trước lúc hồ sơ bị ẩn.
4. Ẩn hồ sơ không xóa yêu cầu đã gửi; admin vẫn xem và xử lý các yêu cầu này.

5. Chuyên môn của KTS lấy từ danh mục category. Một KTS có thể thuộc nhiều category.
6. Người có quyền quản lý tư vấn KTS được tạo, sửa và xóa category chuyên môn. Chỉ được xóa khi category không còn được gán cho bất kỳ KTS nào, kể cả KTS đang bị ẩn. Nếu còn được sử dụng, phải bỏ gán khỏi tất cả hồ sơ trước khi xóa.

7. Hồ sơ KTS bắt buộc có đủ bảy nhóm thông tin: ảnh đại diện, họ tên, chức danh, chuyên môn, số năm kinh nghiệm, số công trình và giới thiệu. Chuyên môn phải chọn ít nhất một category. Thiếu thông tin bắt buộc thì không được lưu hồ sơ.

8. Người tạo chủ động chọn Ẩn/Hiện khi tạo hồ sơ KTS, không có bước phê duyệt.

9. Ảnh đại diện phải là đường dẫn tới tệp trên kho ảnh của hệ thống, cùng kho với ảnh của dự toán. Đường dẫn ngoài kho bị từ chối khi tạo hồ sơ hoặc khi đổi sang ảnh mới; sửa hồ sơ mà giữ nguyên ảnh đang lưu thì không bị kiểm lại.

## Except

Các yêu cầu đã gửi trước khi ẩn vẫn được giữ để xử lý.

## Notes

Người dùng xác nhận khi bắt đầu thiết kế kỹ thuật: người quản trị nhập đường dẫn ảnh có sẵn cho ảnh đại diện; backend không nhận tệp ảnh. Ngày 26/09/2026 người dùng chốt thêm khoản 9: đường dẫn đó phải thuộc kho ảnh của hệ thống (kho presign dùng chung với ảnh dự toán), không nhận ảnh ở tên miền bất kỳ.

STORY-CONSULT-001; STORY-CONSULT-002. Giới hạn dữ liệu cụ thể sẽ xác định khi thiết kế; người tạo chủ động chọn Ẩn/Hiện khi tạo hồ sơ; không tự thêm chức năng xóa hồ sơ.

Nội dung nghiệp vụ của bộ US/BR đã được người dùng chốt trong hội thoại. Metadata người phụ trách tài liệu chưa xác định; không suy ra phê duyệt trên hệ thống quản lý tài liệu.
