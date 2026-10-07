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

# BR-NEWS-004

## Rule Info

- **Name**: Đường dẫn và ngôn ngữ của bài tin tức
- **Category**: Tin tức
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế lưu trữ trang Cẩm nang ngày 07/10/2026.

## Statement

Mỗi bài tin tức có một đường dẫn hiện tại và giữ lại các đường dẫn cũ; đường dẫn cũ chuyển hướng về đường dẫn hiện tại. Mỗi bài thuộc đúng một ngôn ngữ.

## When

Người quản lý lưu bài, đổi tiêu đề hoặc đổi đường dẫn; người đọc mở một đường dẫn bài.

## Then

1. Bài có một đường dẫn hiện tại dùng cho trang công khai. Hệ thống sinh đường dẫn từ tiêu đề; người quản lý sửa lại được.
2. Đường dẫn là duy nhất trên toàn hệ thống, tính cả các đường dẫn cũ của mọi bài.
3. Người quản lý tự nhập đường dẫn mà trùng thì từ chối lưu và giữ nguyên bài hiện tại. Đường dẫn do hệ thống sinh từ tiêu đề mà trùng thì hệ thống tự nối thêm số thứ tự cho tới khi được, vì hai bài được phép trùng tiêu đề.
4. Sửa tiêu đề không tự đổi đường dẫn. Chỉ đổi khi người quản lý chủ động sửa.
5. Đổi đường dẫn thì đường dẫn cũ được giữ lại và vẫn trỏ về đúng bài đó.
6. Mở bằng đường dẫn hiện tại thì trả nội dung bài. Mở bằng đường dẫn cũ thì chuyển hướng vĩnh viễn sang đường dẫn hiện tại. Đường dẫn không tồn tại thì báo không tìm thấy.
7. Xóa bài thì xóa toàn bộ đường dẫn của bài, gồm cả các đường dẫn cũ. Các đường dẫn đó được dùng lại cho bài khác.
8. Bài công bố bắt buộc có đường dẫn. Bài nháp được để trống.
9. Mỗi bài thuộc đúng một ngôn ngữ. Bản dịch của một bài là một bài riêng, có trạng thái, ngày công bố và đường dẫn riêng.
10. Bản tiếng Anh tồn tại độc lập, không bắt buộc phải có bản tiếng Việt.

## Except

Các bài đã có trước khi áp dụng quy tắc này được coi là tiếng Việt và chưa có đường dẫn. Bài đang công bố vẫn đọc được qua định danh cũ; người quản lý bổ sung đường dẫn ở lần lưu sửa tiếp theo. Hệ thống không tự sinh đường dẫn cho bài cũ.

## Notes

Chuyển hướng vĩnh viễn thay vì trả cùng nội dung ở hai đường dẫn, vì hai đường dẫn cùng nội dung bị công cụ tìm kiếm coi là trùng lặp và chia nhỏ thứ hạng của chính bài đó.

Ví dụ cho khoản 3: hai bài cùng tên "Chọn sơn" cho đường dẫn `chon-son` và `chon-son-2`. Nếu người quản lý tự nhập `chon-son` cho bài thứ hai thì bị từ chối, vì đó là lựa chọn có chủ đích của họ.

Đường dẫn duy nhất trên toàn hệ thống nghĩa là bản tiếng Việt và bản tiếng Anh của cùng một bài không dùng chung một đường dẫn. Thực tế không vướng vì tiêu đề hai ngôn ngữ khác nhau.

Owner và ngày hiệu lực chưa xác định. Reviewer và Approver lấy theo các tài liệu Tin tức hiện có; cần xác nhận lại nếu người phụ trách đã thay đổi. Quy tắc này bổ sung cho [BR-NEWS-001](BR-NEWS-001.md), không thay thế các điều kiện công bố đã có ở đó.
