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

# BR-SITE-006

## Rule Info

- **Name**: Admin quản lý danh mục hiện trạng; hồ sơ cũ giữ lựa chọn đã lưu.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng đã xác nhận trong hội thoại mở rộng hồ sơ công trình ngày 01/10/2026; bổ sung cho STORY-SITE-001 và các Story liên quan.

## Statement

Admin quản lý danh mục hiện trạng bằng thêm, đổi tên, sắp xếp và ngừng cho chọn. Công trình phải chọn một hiện trạng; thay đổi danh mục không tự đổi hồ sơ đã lưu.

## When

Admin quản lý danh mục hiện trạng hoặc khách chọn, giữ hoặc đổi hiện trạng khi tạo/sửa công trình.

## Then

1. Các mục ban đầu gồm đúng Đất trống, Có nhà cũ cần phá dỡ, Cải tạo. Đây là tình trạng ban đầu của đất/nhà, không phải trạng thái tiến độ công trình.
2. Chỉ Admin quản lý danh mục hiện trạng. Quyền quản lý danh mục không cấp quyền tạo, sửa, xóa công trình thay khách.
3. Cho phép thêm mục có tên, đổi tên, thay đổi thứ tự hiển thị và ngừng cho chọn một mục. Thay đổi danh mục không tự ghi lại các công trình đã lưu.
4. Khi tạo công trình hoặc đổi hiện trạng, khách chỉ chọn một mục còn cho chọn. Mục đã ngừng không hiện như lựa chọn mới hợp lệ; backend kiểm lại lúc lưu.
5. Công trình đã chọn mục sau đó bị ngừng vẫn giữ được mục đó và thông tin đã lưu. Khi sửa trường khác, không ép khách đổi hiện trạng chỉ vì mục cũ đã bị ngừng.
6. Không được xóa mục đã được công trình sử dụng. Ngừng cho chọn không gỡ liên kết hoặc làm mất dữ liệu hiện trạng trên hồ sơ.
7. Thứ tự do Admin sắp xếp quyết định thứ tự các mục được hiển thị để chọn, không quyết định giá trị đang chọn trên công trình.

## Except

Mục đã ngừng vẫn được giữ trên công trình đã dùng trước đó; ngoại lệ này không cho công trình khác chọn mới mục đó.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Tham chiếu [STORY-SITE-003](../userstory/STORY-SITE-003.md), [BR-SITE-001](BR-SITE-001.md) và [BR-SITE-002](BR-SITE-002.md).
- Phạm vi đã chốt không thêm thao tác xóa danh mục chưa dùng hoặc tự khôi phục mục đã ngừng. Tên/mã quyền kỹ thuật và giới hạn trường tên được xác định ở TDD, không tự coi quyền xem công trình là quyền quản lý danh mục.
- Nội dung nghiệp vụ đã được xác nhận qua hội thoại; bản US/BR cụ thể đã được người dùng chốt ngày 01/10/2026; đặc tả System Test đã được cập nhật. Chưa triển khai phần mở rộng hoặc chạy kiểm thử cho phần này.
- Reviewer/Approver giữ theo bộ tài liệu SITE hiện có; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa xác định.
