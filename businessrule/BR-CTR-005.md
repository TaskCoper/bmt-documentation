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

# BR-CTR-005

## Rule Info

- **Name**: Lọc danh sách theo các mảng GUID của năng lực nhà thầu.
- **Category**: Nhà thầu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong phiên bàn luận tính năng nhà thầu; trang tham khảo https://vnz-bmt-savico-abcxyz.vercel.app/vi/contractors/preview/firm/ctr-cattrang. Quyết định trong hội thoại được ưu tiên khi khác trang mẫu.

## Statement

Không có query lọc thì lấy toàn bộ nhà thầu đang hiển thị. Query loại công trình và query phạm vi thi công đều nhận mảng GUID; khớp ít nhất một giá trị trong mỗi mảng và đáp ứng cả hai nhóm khi truyền cả hai.

## When

Khách lấy danh sách nhà thầu có hoặc không có query loại công trình và phạm vi thi công.

## Then

1. Không có query lọc thì lấy toàn bộ nhà thầu đang hiển thị; không yêu cầu đăng nhập.
2. Loại công trình và phạm vi thi công là hai query độc lập, mỗi query nhận mảng GUID của danh mục tương ứng.
3. Với mỗi mảng được truyền, nhà thầu phải nhận ít nhất một giá trị trong mảng đó.
4. Nếu truyền cả hai mảng, nhà thầu phải khớp ít nhất một loại công trình và ít nhất một phạm vi thi công được yêu cầu.
5. Áp dụng bộ lọc trên các danh mục admin chọn trực tiếp trong hồ sơ nhà thầu; không lọc theo các dự án đã thực hiện.
6. Nếu có query bán kính, kết hợp điều kiện bán kính theo BR-CTR-006 với các query danh mục được truyền.

## Except

[Chưa xác định: mảng rỗng, GUID không tồn tại hoặc không hợp lệ; cách phân trang và thứ tự trả danh sách.]

## Notes

- Ví dụ: loại [Nhà phố, Biệt thự] và phạm vi [Phần thô, Trọn gói]. Nhà thầu có Nhà phố và Phần thô trong năng lực được khai báo sẽ khớp, kể cả chưa có dự án đã thực hiện phù hợp.
- Get all trả toàn bộ tập nhà thầu công khai trong một phản hồi không phân trang theo TDD-CTR-002 đã chốt; có query thì áp các bộ lọc tương ứng.
- Không tự thêm điều kiện điểm tối thiểu, nhận dự án, bảo hiểm hoặc giấy phép để loại nhà thầu khỏi danh sách; chưa có yêu cầu cho các điều kiện này.
- Người dùng đã chốt bộ US/BR trong hội thoại. Reviewer và Approver: Tân Trần. System Test được soạn theo phần nghiệp vụ đã chốt; chưa chạy kiểm thử hoặc triển khai code. Các điểm còn mở trong tài liệu không tự trở thành quy tắc đã xác nhận. Metadata người phụ trách và ngày hiệu lực còn thiếu; chưa thực hiện phê duyệt trên hệ thống quản lý tài liệu.
