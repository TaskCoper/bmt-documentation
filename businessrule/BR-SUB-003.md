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


# BR-SUB-003

## Rule Info

- **Name**: Chỉ tính lượt tạo thiết kế khi thành công.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt giữ trước 1 lượt tạo mới, thành công tính đã dùng, lỗi giải phóng; tác vụ qua kỳ tính vào kỳ giữ và được tiếp tục sau hết hạn. Người dùng chọn thành công là AI tạo đủ kết quả, đã lưu và khách có thể mở xem, không chờ khách duyệt hoặc hài lòng. Người dùng xác nhận toàn bộ kết quả AI trả cùng một lúc; phối cảnh 3D là một phần của bộ kết quả.

## Statement

**Phạm vi mới nhất:** chỉ quyền tạo thiết kế mới và tra cứu mẫu được triển khai logic sử dụng/tính lượt trong đợt này. Quyền 3D vẫn cấu hình bật/tắt và hiển thị, chưa điều khiển AI hoặc kiểm tra quyền tạo 3D. Các mô tả xử lý 3D bên dưới là thiết kế cho giai đoạn sau, không phải tiêu chí nghiệm thu hiện tại. Không suy ra AI hiện phải luôn trả hoặc luôn bỏ 3D.

Một yêu cầu tạo thiết kế chỉ được tính là đã dùng một lượt khi tạo thành công. Hệ thống giữ trước một lượt trong lúc xử lý để lượt đó không được dùng cho yêu cầu khác; nếu tạo lỗi thì giải phóng lượt đã giữ. Tác vụ chạy qua hai kỳ vẫn tính vào kỳ đã giữ lượt, không ảnh hưởng hạn mức kỳ mới. Tác vụ đã bắt đầu hợp lệ không bị hủy chỉ vì subscription hết hạn và chưa gia hạn; vẫn chịu giới hạn thời gian xử lý theo BR-SUB-016.

## When

Tài khoản có quyền tạo thiết kế với hạn mức hữu hạn và còn ít nhất một lượt có thể sử dụng; yêu cầu được tiếp nhận để xử lý.

## Then

1. Giữ một lượt từ hạn mức chung của tài khoản. Lượt đang giữ chưa được tính là đã dùng nhưng không còn sẵn cho yêu cầu khác.
2. Một lần Gen AI trả toàn bộ bộ kết quả cùng lúc; phối cảnh 3D là một phần của bộ kết quả khi tài khoản có quyền tương ứng, không phải thao tác tạo riêng. Thành công nghĩa là AI tạo đủ kết quả, hệ thống đã lưu và khách có thể mở xem. Không trả trước một phần kết quả như một lần thành công rồi bổ sung 3D sau; không tính lượt riêng cho 3D. Không cần khách thực sự mở, bấm duyệt hoặc hài lòng; việc không thích kết quả không tự hoàn lượt. Khi thành công, chuyển lượt đang giữ thành lượt đã dùng; không trừ thêm một lượt nữa.
3. Khi tạo lỗi, giải phóng lượt đang giữ và không ghi nhận lượt đã dùng. Nếu vẫn trong cùng kỳ còn hiệu lực, lượt này có thể dùng lại.

4. Nếu tác vụ hoàn thành sau khi sang kỳ mới, ghi nhận lượt đã dùng vào kỳ đã giữ lượt. Nếu tác vụ lỗi, giải phóng lượt giữ ở kỳ cũ; không cộng lượt sang kỳ mới.

5. Nếu subscription hết hạn mà chưa có kỳ mới, không hủy tác vụ đã bắt đầu chỉ vì hết hạn. Thành công thì tính lượt vào kỳ đã giữ; lỗi thì giải phóng lượt giữ ở kỳ đó nhưng không cho sử dụng lại lượt đã hết hạn.

6. Tác vụ AI đã được tiếp nhận hợp lệ hoàn tất theo bộ quyền của gói tại thời điểm tiếp nhận, kể cả quyền 3D, dù khách đổi gói hoặc chu kỳ trong lúc xử lý. Không bổ sung hoặc bỏ đầu ra của tác vụ đó theo gói mới. Yêu cầu mới sau khi chuyển kiểm tra quyền gói mới. Quy tắc này không cho phép trả kết quả muộn sau timeout; BR-SUB-016 vẫn áp dụng. Lượt vẫn được ghi nhận vào kỳ đã giữ theo các khoản trên.

## Except

Đợt này không hỗ trợ khách hàng hủy tác vụ tạo thiết kế. Tác vụ tiếp tục theo kết quả xử lý: thành công tính lượt, lỗi giải phóng lượt đã giữ. Tác vụ vượt thời gian chờ được cấu hình tự chuyển thất bại và giải phóng lượt theo [BR-SUB-016](BR-SUB-016.md). Kết quả đến muộn sau xử lý quá thời gian không tự tính lượt lại. Chưa chốt giá trị thời gian chờ.

## Notes

- Giữ quyền tác vụ khi đổi gói theo BR-SUB-021 và [ST-SUB-102](../systemtest/ST-SUB-102.md); người dùng đã xác nhận.

- Ví dụ cùng một kỳ: có 10 lượt; đang tạo thì còn 9 lượt sẵn dùng và 1 lượt đang giữ. Thành công thì còn 9 lượt sẵn dùng, 1 lượt đã dùng; lỗi thì trở lại 10 lượt sẵn dùng, không có lượt đã dùng từ yêu cầu này.
- Cơ chế giữ/trừ từ số lượt còn lại áp dụng cho hạn mức hữu hạn. Quyền không giới hạn lượt theo [BR-SUB-005](BR-SUB-005.md) không có số dư hữu hạn để trừ; cách ghi nhận sử dụng thuộc thiết kế kỹ thuật.
- Quy tắc này mô tả tạo mới. Hệ thống không có tính năng hoặc lượt chỉnh sửa thiết kế; tra cứu có quy tắc riêng tại [BR-SUB-017](BR-SUB-017.md).
- Đã xác nhận một bộ kết quả AI được trả cùng lúc, trong đó có phần phối cảnh 3D theo quyền được cấp. Khi bộ kết quả còn thiếu phần cần trả thì chưa đạt điều kiện thành công; lỗi hoặc quá thời gian xử lý theo quy tắc đã chốt, không tính thành công cho phần kết quả dở dang. Danh sách chi tiết các phần kết quả theo quyền gói vẫn phải được định nghĩa trong luồng tạo thiết kế; chưa tự đặt số tệp, số ảnh hoặc tiêu chuẩn chuyên môn. Không tính thành công chỉ vì dịch vụ AI báo xong trong khi chưa lưu hoặc chưa thể mở xem. Kết quả đến sau khi tác vụ đã thất bại do quá thời gian vẫn theo BR-SUB-016.
- Tham chiếu [STORY-SUB-001](../userstory/STORY-SUB-001.md), [BR-SUB-001](BR-SUB-001.md), [BR-SUB-002](BR-SUB-002.md), [ST-SUB-003](../systemtest/ST-SUB-003.md) và [ST-SUB-004](../systemtest/ST-SUB-004.md).
- [ST-SUB-005](../systemtest/ST-SUB-005.md) và [ST-SUB-006](../systemtest/ST-SUB-006.md) kiểm tra tác vụ chạy qua hai kỳ.
- [ST-SUB-015](../systemtest/ST-SUB-015.md) và [ST-SUB-016](../systemtest/ST-SUB-016.md) kiểm tra tác vụ kết thúc sau khi hết hạn, chưa gia hạn.
- [ST-SUB-053](../systemtest/ST-SUB-053.md) kiểm tra tính lượt khi kết quả sẵn sàng, không chờ khách duyệt.
- [ST-SUB-070](../systemtest/ST-SUB-070.md) kiểm tra trả cùng lúc bộ kết quả có 3D và chỉ tính một lượt tạo thiết kế.
- AI gợi ý giảm chi phí đã hoãn theo [nợ nghiệp vụ](../debt/ai-budget-optimization.md). Thiếu phần gợi ý này không khiến bộ kết quả hiện tại bị coi là thiếu hoặc thất bại; dự toán nội thất vẫn thuộc phạm vi đã chốt.
- Bản nháp còn thiếu metadata; đặc tả test chưa được thực thi.
