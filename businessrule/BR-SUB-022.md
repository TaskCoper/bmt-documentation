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

# BR-SUB-022

## Rule Info

- **Name**: Gói giám sát được mua trước và phải gán lần đầu trong một năm.
- **Category**: Thanh toán và gói dịch vụ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế thanh toán ngày 19/09/2026; xem discovery/payment-packages.md. Quyết định mới nhất được ưu tiên khi thay thế phương án trước đó.

## Statement

Khách được mua nhiều gói giám sát chưa gán công trình, kể cả nhiều gói cùng loại. Mỗi gói phải được khách gán cho một công trình trong một năm từ lúc cấp; quá hạn chưa gán thì mất quyền sử dụng.

## When

Gói giám sát đã được cấp hoặc khách yêu cầu gán lần đầu cho công trình.

## Then

1. Không yêu cầu công trình khi mua hoặc cấp gói. Gói chưa gán nằm trong các gói đã mua của khách, không phát sinh thanh toán lần nữa khi gán.
2. Khách tự gán gói của mình cho một công trình thuộc mình, trong hạn một năm từ lúc cấp. Mỗi gói chỉ gán cho một công trình.
3. Nếu công trình đã có gói giám sát đang hiệu lực, chặn gán thêm. Gói bị chặn vẫn chưa gán và giữ hạn ban đầu để dùng cho công trình khác.
4. Quá một năm mà chưa từng gán thì không cho sử dụng hoặc gán lần đầu. Không tự hoàn tiền hay tạo gói thay thế.
5. Gán công trình được coi là đã sử dụng gói. Sau khi gán đúng hạn, gói tiếp tục phục vụ công trình sau mốc một năm, không tự hết hạn vì mốc này.
6. Hiện chỉ quản lý gói gán cho công trình nào; không quản lý hoạt động khảo sát/giám sát để quyết định quyền gán hoặc sửa.

## Except

Gói bị nhân viên hủy không được sử dụng khi chưa khôi phục hợp lệ. Hạn gán lần đầu không phải chu kỳ giám sát. Đổi công trình sau lần gán đúng hạn theo BR-SUB-023.

## Notes

Thay quy tắc cũ bắt buộc gắn công trình ngay khi cấp và không có hạn cho mọi gói giám sát. Hạn gán là cùng ngày và giờ năm sau theo Asia/Ho_Chi_Minh; ngày 29/02 thành 28/02 nếu năm sau không nhuận. Chỉ gán trước hạn; đúng mốc hạn bị từ chối. Không coi một năm là 365 ngày.

- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Tên Reviewer/Approver lấy theo xác nhận cho các bản nháp mới trong discovery/subscription-entitlements.md; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- Công trình là thực thể riêng, khác với bản dự toán; khách tự tạo công trình miễn phí, không cần gói thiết kế. Người dùng xác nhận ngày 25/09/2026; Story và BR tạo, quản lý công trình chưa được soạn và sẽ chuẩn bị riêng.
- [Tổng hợp quyết định và bảng truy vết](../discovery/payment-packages.md).
