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

# BR-HB-002

## Rule Info

- **Name**: Khối Bản tin — bài nổi bật và bài liên quan
- **Category**: Cẩm nang
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế lưu trữ trang Cẩm nang ngày 07/10/2026.

## Statement

Khối Bản tin hiển thị các bài nổi bật theo thứ tự người quản lý sắp xếp, kèm danh sách bài liên quan do hệ thống suy ra.

## When

Người quản lý chọn hoặc sắp xếp bài nổi bật; người đọc mở trang Cẩm nang.

## Then

1. Người quản lý chọn các bài đưa vào khối nổi bật và tự quyết định thứ tự. Thứ tự này không phụ thuộc ngày công bố.
2. Khối hiển thị bốn bài đầu tiên đang công bố, đánh số 01 đến 04 theo thứ tự hiển thị thực tế.
3. Số bài được xếp vào khối không giới hạn. Bài không còn công bố thì bị bỏ qua và các bài sau dồn lên, nên khối vẫn đủ bốn bài nếu còn đủ bài công bố.
4. Một bài chỉ nằm ở một vị trí trong khối và chỉ xuất hiện một lần.
5. Ẩn bài hoặc chuyển bài về nháp không gỡ bài khỏi khối. Bài tạm không hiển thị và quay lại đúng vị trí cũ khi được công bố lại.
6. Xóa bài sẽ gỡ bài khỏi khối nổi bật.
7. Danh sách bài liên quan được suy ra từ các danh mục của bài đang đứng đầu khối, người quản lý không chọn tay. Bài liên quan phải đang công bố và không trùng với các bài đang hiển thị trong khối nổi bật.
8. Danh sách bài liên quan không có liên kết xem tất cả. Người đọc muốn xem thêm thì dùng bộ lọc danh mục ở khối bài viết.
9. Khi không có bài nổi bật nào đang công bố, trang công khai không hiển thị khối Bản tin. Các phần còn lại của trang vẫn hiển thị bình thường, và hệ thống không tự lấy bài khác để lấp vào khối.
10. Khi khối không hiển thị được vì lý do ở khoản 9, màn quản trị phải báo cho người quản lý biết.

## Except

Bài đang nằm trong khối nổi bật vẫn bị ẩn, sửa hoặc xóa bình thường theo [BR-NEWS-001](BR-NEWS-001.md); việc nằm trong khối không khóa bài lại.

## Notes

Ví dụ: người quản lý xếp sáu bài theo thứ tự 1 đến 6. Bài ở vị trí 2 bị ẩn thì khối hiển thị các bài 1, 3, 4, 5 và đánh số 01 đến 04. Công bố lại bài 2 thì nó trở về số 02 và bài 5 lui ra ngoài khối.

Quyết định ngày 07/10/2026: bỏ liên kết "Xem tất cả bài liên quan" khỏi khối, ghi ở khoản 8. Cùng ngày, người dùng chốt ẩn cả khối khi không có bài nổi bật nào đang công bố, ghi ở khoản 9 và 10. Hệ thống không tự lấy bài mới nhất lấp vào, để thứ tự khối luôn là lựa chọn có chủ đích của người quản lý theo khoản 1.

Owner và ngày hiệu lực chưa xác định. Reviewer và Approver lấy theo các tài liệu Tin tức hiện có; cần xác nhận lại nếu người phụ trách đã thay đổi. Quy tắc này thay cho khoản 7 của [BR-NEWS-003](BR-NEWS-003.md) về việc chưa có ghim tin.
