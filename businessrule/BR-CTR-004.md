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

# BR-CTR-004

## Rule Info

- **Name**: Thông tin liên hệ nội bộ và quyền công khai bản scan.
- **Category**: Nhà thầu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong phiên bàn luận tính năng nhà thầu; trang tham khảo https://vnz-bmt-savico-abcxyz.vercel.app/vi/contractors/preview/firm/ctr-cattrang. Quyết định trong hội thoại được ưu tiên khi khác trang mẫu.

## Statement

Người liên hệ, số điện thoại và email của nhà thầu chỉ dành cho admin. Thông tin và bản scan pháp lý, hợp tác BuildX hiển thị theo trạng thái chung của hồ sơ nhà thầu.

## When

Admin quản lý thông tin liên hệ, hồ sơ pháp lý hoặc hợp tác; khách xem chi tiết nhà thầu.

## Then

1. Không trả người liên hệ, số điện thoại hoặc email của nhà thầu trong hồ sơ công khai cho khách.
2. Admin được lưu và xem các thông tin liên hệ nội bộ của nhà thầu.
3. Admin tải bản scan giấy phép hoặc thỏa thuận hợp tác lên hồ sơ; không có lựa chọn ẩn/hiện riêng cho từng tệp.
4. Khi hồ sơ Hiển thị, khách được xem các bản scan đã tải lên mà không cần đăng nhập. Khi hồ sơ Ẩn, các bản scan cũng không được cung cấp công khai.
5. Hồ sơ pháp lý có các trường theo trang mẫu: tên pháp nhân, mã số thuế, ngày thành lập, người đại diện, địa chỉ đăng ký, ngành nghề và quy mô nhân sự. Thông tin giấy phép gồm loại giấy phép, số hiệu/mã số doanh nghiệp, cơ quan cấp, ngày cấp, thời hạn hiệu lực và bản scan. Các phần này có thể bổ sung sau theo BR-CTR-002.
6. Hợp tác BuildX có thông tin thời gian hợp tác, mã hồ sơ, ngày ký, số trang và bản scan. Không dùng quy trình xác minh riêng cho tài liệu.
7. Phần cam kết có thông tin bảo hành, việc dùng hợp đồng mẫu BuildX và bảo hiểm công trình theo trang mẫu. Admin nhập thông tin áp dụng cho nhà thầu; các con số và cam kết minh họa trên trang mẫu không tự trở thành chính sách bắt buộc cho mọi nhà thầu.
8. Mã số thuế đã được admin nhập được hiển thị đầy đủ trên hồ sơ công khai, không che một phần và không yêu cầu khách đăng nhập để xem.

## Except

Thông tin liên hệ tại mục 1 vẫn chỉ dành cho admin, kể cả khi hồ sơ Hiển thị.

## Notes

- Người dùng đã chốt làm phần pháp lý và hợp tác như trang mẫu, nhưng giản lược xác minh thành trạng thái tổng thể của hồ sơ.
- Không dùng việc gửi lời mời báo giá để mở khóa tài liệu hoặc thông tin liên hệ trong phạm vi này. Quyền xem bản scan theo trạng thái chung của hồ sơ.
- Loại/dung lượng tệp, API upload/download và kiểm quyền theo trạng thái hồ sơ đã chốt tại TDD-CTR-001/002. Hợp đồng adapter với nhà cung cấp kho riêng tư vẫn cần đối chiếu khi tích hợp.
- Quyết định mới thay thế lựa chọn Công khai/Chỉ nội bộ của từng bản scan: chỉ dùng trạng thái chung của hồ sơ. Mã số thuế hiển thị đầy đủ khi hồ sơ công khai.
- Người dùng đã chốt bộ US/BR trong hội thoại. Reviewer và Approver: Tân Trần. System Test được soạn theo phần nghiệp vụ đã chốt; chưa chạy kiểm thử hoặc triển khai code. Các điểm còn mở trong tài liệu không tự trở thành quy tắc đã xác nhận. Metadata người phụ trách và ngày hiệu lực còn thiếu; chưa thực hiện phê duyệt trên hệ thống quản lý tài liệu.
