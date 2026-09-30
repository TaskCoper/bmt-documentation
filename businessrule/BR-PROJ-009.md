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

# BR-PROJ-009

## Rule Info

- **Name**: Xóa nhiều dự toán của tôi theo điều kiện từng bản, không khôi phục hoặc hoàn lượt.
- **Category**: Dự toán — xóa dự toán cá nhân
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại: xóa nhiều; xóa bản đủ điều kiện, giữ bản đang xử lý AI; Chọn tất cả chỉ ở trang đang xem; xóa xong không mở lại hoặc khôi phục, ngừng xem qua link chia sẻ, không hoàn lượt; hết hạn gói hoặc hết lượt vẫn được xóa.

## Statement

Khách được chọn một hoặc nhiều dự toán của mình để xóa. Hệ thống kiểm tra riêng từng bản và xóa các bản đủ điều kiện; bản đang được AI xử lý bị chặn. Bản đã xóa không còn trong danh sách, không mở lại hoặc khôi phục được và không còn truy cập hồ sơ qua link đã chia sẻ. Xóa không hoàn lượt đã dùng, không yêu cầu gói còn hiệu lực hoặc còn lượt.

## When

Khách yêu cầu xóa các dự toán đã chọn từ Dự toán của tôi, hoặc có yêu cầu thao tác, mở hồ sơ hay truy cập link của một bản đã xóa.

## Then

1. Chỉ tài khoản khách hàng có phiên hợp lệ được xóa các bản thuộc chính mình. Backend kiểm tra quyền sở hữu của từng bản, không tin phạm vi lựa chọn do giao diện gửi lên.
2. Khách có thể chọn một hoặc nhiều bản. Chọn tất cả chỉ chọn các bản trên trang đang xem, không chọn toàn bộ kết quả ở mọi trang. Không tự mở rộng tập xóa ngoài các bản khách đã chọn.
3. Bản đủ điều kiện xóa phải thuộc người gọi, chưa bị xóa và không đang được AI xử lý tại thời điểm xử lý xóa bản đó. Bản nháp, thành công hoặc thất bại đều được xóa khi đáp ứng các điều kiện này.
4. Kiểm lại trạng thái khi xử lý; trạng thái được tải hoặc được chọn trước đó không thay thế việc kiểm tra hiện tại. Nếu đã tiếp nhận AI trước khi xóa thì chặn xóa. Nếu đã xóa thành công thì không cho tiếp nhận AI mới trên bản đó.
5. Trong một lần xóa nhiều, xóa các bản đủ điều kiện và giữ nguyên các bản không đủ điều kiện. Trả kết quả từng bản cùng lý do không xóa được khi có thể công bố; không từ chối cả lần chỉ vì một bản bị chặn và không hoàn tác các bản đã xóa thành công vì lý do đó.
6. Bản không thuộc khách hoặc không tồn tại không được trả thông tin riêng để giải thích kết quả. Bản đã xóa không được tạo lại, khôi phục hoặc tính thêm lượt khi yêu cầu cũ được gửi lại.
7. Bản xóa thành công phải biến mất khỏi danh sách, tìm kiếm, bộ lọc và kết quả phân trang. Chủ sở hữu không thể mở lại đầu vào, kết quả hoặc hồ sơ bằng đường dẫn cũ; không có thùng rác hoặc chức năng khôi phục.
8. Sau khi xóa thành công, ngừng cấp quyền xem và tải mới qua mọi link chia sẻ của bản, gồm link trực tiếp, QR và link trong email đã gửi. Link còn hạn không làm dự toán đã xóa tiếp tục truy cập được.
9. Hết hạn gói hoặc hết lượt không ngăn chủ sở hữu xóa bản đủ điều kiện. Quy tắc này không mở quyền sửa đầu vào hoặc gửi AI.
10. Không hoàn lượt đã dùng do xóa dự toán. Xóa không giữ hoặc trừ lượt mới; không sửa kết quả tính lượt của tác vụ đã kết thúc theo BR-SUB-003.
11. Không hủy AI hoặc giải phóng lượt đang giữ bằng thao tác xóa. Khi bản đang xử lý bị chặn, tác vụ tiếp tục theo BR-SUB-003 và BR-PROJ-005.
12. Giao diện phải thể hiện việc không thể khôi phục và mất quyền truy cập hồ sơ đã chia sẻ khi khách yêu cầu xóa, đồng thời phản ánh đúng kết quả từng bản sau xử lý.
13. Nếu lỗi mạng hoặc máy chủ khiến chưa xác định đủ kết quả, không báo toàn bộ đã xóa hoặc toàn bộ còn nguyên. Cho khách tải lại danh sách để kiểm tra; không dùng lỗi phản hồi làm lý do khôi phục bản đã xóa hoặc hoàn lượt.

## Except

Chặn xóa khi AI đang xử lý, kể cả khi bản đã được chọn trước lúc AI bắt đầu. Việc ngừng truy cập chỉ áp dụng cho yêu cầu mới; không thu hồi được bản tệp mà người nhận đã tải về trước đó. Không tự cam kết dừng một luồng tải đã được cấp quyền trước lúc xóa; chi tiết xử lý đồng thời cần được làm rõ trong thiết kế.

## Notes

- Áp dụng cho STORY-PROJ-007; danh sách các bản còn lại theo STORY-PROJ-006 và BR-PROJ-008.
- Ví dụ đã được người dùng chọn: 10 bản được yêu cầu xóa, 8 bản đủ điều kiện và 2 bản đang xử lý AI thì xóa 8, giữ 2 và báo kết quả riêng; không chuyển thành thao tác tất cả thành công hoặc không xóa gì.
- Quyền xem hồ sơ cũ tại BR-SUB-007 và quyền truy cập link tại BR-PROJ-006 chỉ tiếp tục với bản chưa bị xóa. Thu hồi link đơn thuần vẫn giữ quyền xem của chủ sở hữu theo BR-PROJ-006; xóa bản theo quy tắc này chấm dứt cả quyền mở lại của chủ sở hữu.
- Ngày 30/09/2026, người dùng đồng ý đánh dấu đã xóa, giữ dữ liệu nội bộ và lịch sử lượt để đối soát; khách không thể khôi phục. Đợt này chưa tự động dọn tệp hoặc đặt thời hạn lưu; không xóa vật lý lịch sử tính lượt. Chi tiết schema ở TDD-PROJ-005.
- Hợp đồng phản hồi từng bản, giới hạn số phần tử, xử lý mã lặp, yêu cầu gửi lại và lỗi giữa chừng được thiết kế trong TDD-PROJ-005. Ràng buộc nghiệp vụ là không làm mất kết quả xóa đã thành công, không làm lộ bản của người khác và không báo thành công khi chưa có căn cứ.
- Reviewer và Approver đều là Tân Trần theo xác nhận trong hội thoại. Owner và ngày hiệu lực chưa được cung cấp. Người dùng đã chốt quy tắc cùng User Story trong hội thoại ngày 30/09/2026. Đặc tả ST xem [bảng độ phủ](../discovery/my-estimates-system-test-coverage.md); chưa thực thi. Việc chốt trong hội thoại không thay thế phê duyệt trên hệ thống quản lý tài liệu.
