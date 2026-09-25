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


# BR-SUB-019

## Rule Info

- **Name**: Hạ gói thiết kế từ kỳ tiếp theo.
- **Category**: Subscription thiết kế
- **Status**: Deprecated
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chọn hạ gói từ kỳ tiếp theo, giữ gói hiện tại đến hết kỳ. Khách được tự hủy yêu cầu đang chờ trước thời điểm chuyển; hủy không tự gia hạn. Nếu muốn đổi gói đích của yêu cầu hạ gói đang chờ, khách phải hủy yêu cầu cũ rồi chọn lại, không thay thế trực tiếp. Người dùng cho phép chọn luôn chu kỳ tháng/năm của gói đích trong lần hạ; dùng hết kỳ hiện tại rồi bắt đầu gói và chu kỳ mới.

## Statement

**Đã được thay thế bởi [BR-SUB-021](BR-SUB-021.md). Toàn bộ nội dung quy tắc cũ bên dưới chỉ để tra lịch sử, không dùng triển khai hoặc nghiệm thu.**

Yêu cầu hạ gói thiết kế không làm giảm quyền lợi ngay giữa kỳ. Khách tiếp tục dùng gói hiện tại đến hết kỳ; gói thấp hơn áp dụng từ kỳ tiếp theo khi đủ điều kiện chuyển hợp lệ và yêu cầu chưa bị hủy. Khách được chọn luôn chu kỳ tháng hoặc năm của gói đích trong cùng yêu cầu hạ gói; cả gói và chu kỳ mới cùng áp dụng từ kỳ tiếp theo. Khách được tự hủy yêu cầu đang chờ trước thời điểm chuyển. Muốn đổi sang gói đích khác, khách phải hủy yêu cầu cũ thành công rồi tạo yêu cầu mới.

## When

Tài khoản đang có gói thiết kế còn hiệu lực và có yêu cầu hạ xuống một gói đích hợp lệ.

## Then

1. Ghi nhận gói muốn chuyển cho kỳ tiếp theo; giữ nguyên mốc kết thúc và bộ quyền lợi của kỳ hiện tại. Việc gửi yêu cầu không cắt giảm, làm mới hoặc cấp thêm lượt trong kỳ hiện tại; các thao tác sử dụng vẫn kiểm tra quyền và lượt sẵn dùng như bình thường.
2. Trước mốc kết thúc kỳ hiện tại, yêu cầu sử dụng tính năng vẫn kiểm tra gói hiện tại. Không áp dụng quyền của gói thấp hơn sớm chỉ vì đã có yêu cầu hạ gói.
3. Tại mốc kết thúc kỳ hiện tại, chuyển sang gói đích cho kỳ tiếp theo nếu yêu cầu chưa bị hủy và đã đáp ứng điều kiện chuyển hợp lệ. Từ mốc này, yêu cầu sử dụng mới kiểm tra quyền gói đích; không có hai subscription thiết kế đồng thời hiệu lực.
4. Mốc kết thúc kỳ không bao gồm thời điểm đó theo BR-SUB-014. Ví dụ kỳ PRO kết thúc 30/9 lúc 10:00: trước 10:00 vẫn dùng PRO; từ 10:00 dùng BASIC nếu việc chuyển đủ điều kiện. Đây là mốc đã có của kỳ cũ, không tự chuyển thành cuối ngày 30/9.

5. Khách được tự hủy yêu cầu hạ gói đang chờ của tài khoản mình trước thời điểm chuyển. Sau khi hủy thành công, yêu cầu đó không còn được dùng để chuyển sang gói thấp hơn khi hết kỳ. Giữ nguyên quyền, lượt và ngày hết hạn của kỳ hiện tại; không tự làm mới lượt hoặc gia hạn gói.
6. Hủy yêu cầu đang chờ không phải hoàn tác một lần hạ gói đã có hiệu lực. Việc tiếp tục gói hiện tại ở kỳ sau vẫn phải đáp ứng điều kiện gia hạn được thiết kế cùng thanh toán.

7. Nếu đã có yêu cầu hạ gói đang chờ mà khách muốn chọn gói đích khác, yêu cầu khách hủy yêu cầu cũ trước. Không sửa hoặc thay thế trực tiếp gói đích, kể cả yêu cầu gửi trực tiếp; giữ nguyên yêu cầu đang chờ khi thao tác thay thế bị từ chối.
8. Sau khi hủy thành công, khách có thể chọn lại và tạo yêu cầu hạ gói mới nếu đáp ứng điều kiện hợp lệ tại thời điểm đó. Ví dụ đang dùng PRO và chờ chuyển xuống BASIC: muốn chuyển xuống PLUS thì hủy yêu cầu BASIC, sau đó gửi yêu cầu PLUS. Yêu cầu mới vẫn áp dụng từ kỳ tiếp theo, không đổi ngày hết hạn hiện tại. Hủy xong nhưng chưa tạo được yêu cầu mới thì yêu cầu cũ vẫn đã hủy, không tự khôi phục.

9. Cho phép hạ gói kèm đổi chu kỳ trong cùng một yêu cầu, ví dụ PRO năm xuống BASIC tháng hoặc PRO tháng xuống BASIC năm. Giữ nguyên kỳ đang dùng đến hết hạn; khi yêu cầu chưa bị hủy và đủ điều kiện chuyển hợp lệ, bắt đầu kỳ gói đích theo chu kỳ đã chọn từ mốc đó, tính ngày hết hạn theo BR-SUB-014. Không bắt khách dùng một kỳ của gói thấp hơn theo chu kỳ cũ trước khi đổi chu kỳ.
10. Khi kỳ đích bắt đầu hợp lệ, bỏ lượt dư kỳ cũ và cấp hạn mức tương ứng với lựa chọn tháng/năm của gói đích theo BR-SUB-002 và BR-SUB-015. Hạn mức năm dùng cả kỳ năm, không làm mới mỗi tháng. Tác vụ đã giữ lượt vẫn xử lý ở kỳ cũ theo BR-SUB-003. Nếu khách hủy yêu cầu hạ gói đang chờ này, không thực hiện cả phần đổi gói lẫn đổi chu kỳ thuộc yêu cầu đó.

11. Nếu đang có yêu cầu hạ gói mà khách muốn chỉ đổi tháng/năm trong cùng gói hiện tại, hoặc đang có yêu cầu chỉ đổi chu kỳ mà muốn hạ gói, khách phải hủy yêu cầu cũ thành công trước khi tạo yêu cầu mới. Khi chưa hủy, từ chối yêu cầu mới, kể cả gửi trực tiếp; giữ nguyên yêu cầu cũ, kỳ hiện tại, quyền và lượt. Sau khi hủy, yêu cầu mới phải đáp ứng điều kiện hợp lệ tại thời điểm gửi. Hủy không tự tạo yêu cầu mới hoặc gia hạn; nếu chưa tạo được yêu cầu mới, không tự khôi phục yêu cầu đã hủy.

12. Khi khách giữ nguyên gói đích của yêu cầu hạ gói đang chờ nhưng muốn đổi chu kỳ tháng/năm của gói đích, phải hủy yêu cầu cũ thành công rồi tạo lại. Không sửa chu kỳ trực tiếp, kể cả yêu cầu gửi trực tiếp; khi từ chối, giữ nguyên yêu cầu cũ và kỳ, quyền, lượt hiện tại. Yêu cầu tạo lại phải hợp lệ tại thời điểm gửi và vẫn áp dụng từ cuối kỳ hiện tại. Hủy không tự gia hạn hoặc khôi phục yêu cầu cũ nếu chưa tạo được yêu cầu mới. Nếu gói đích đã ngừng bán, yêu cầu tạo lại bị chặn theo BR-SUB-013, không kế thừa ngoại lệ của yêu cầu đã hủy.

13. Yêu cầu hạ gói hoặc chỉ đổi chu kỳ đang chờ đến cuối kỳ giữ nguyên giá và toàn bộ quyền lợi đã chốt trong yêu cầu hợp lệ. Khi Admin công bố bản khác trước ngày chuyển, không thay giá, hạn mức, quyền bật/tắt hoặc mức tính năng của yêu cầu này bằng bản mới. Khi đến mốc chuyển, nếu yêu cầu chưa hủy và đủ điều kiện, kỳ đích dùng đúng bản đã chốt. Ví dụ đã chốt 200.000đ và 10 lượt thì vẫn dùng các giá trị đó dù bản mới là 250.000đ và 8 lượt. Đây là ví dụ, không phải giá bán chính thức. Quy tắc không tự xác nhận thanh toán hoặc chốt bản cho các kỳ gia hạn về sau.

## Except

Không áp dụng thời điểm này cho nâng gói; nâng gói theo BR-SUB-018. Chỉ đổi tháng/năm trong cùng gói theo cả hai chiều đều theo [BR-SUB-015](BR-SUB-015.md). Hạ gói kèm đổi chu kỳ áp dụng theo quy tắc này từ kỳ tiếp theo. Không áp dụng cho gói giám sát theo dự án.

## Notes

- Người dùng chọn giữ bản giá và quyền lợi đã chốt cho yêu cầu hạ gói/đổi chu kỳ đang chờ. [ST-SUB-099](../systemtest/ST-SUB-099.md) kiểm tra giá cùng toàn bộ quyền; thời điểm giao dịch cụ thể và thu tiền vẫn thiết kế cùng thanh toán.

- Người dùng chọn phương án 1 cho cả hai chiều chuyển giữa hạ gói và chỉ đổi chu kỳ: phải hủy yêu cầu cũ trước. [ST-SUB-097](../systemtest/ST-SUB-097.md) kiểm tra quyết định này. Người dùng tiếp tục chọn hủy rồi tạo lại cả khi chỉ sửa chu kỳ của gói đích; xem [ST-SUB-098](../systemtest/ST-SUB-098.md).

- Quyết định này chốt thời điểm hạ gói, không tự xác nhận thanh toán hoặc tự gia hạn. Điều kiện tiếp nhận và hoàn tất chuyển, thanh toán và cách xử lý khi không đủ điều kiện sẽ thiết kế sau; không tự kéo dài gói cũ hoặc cấp gói mới miễn phí.
- Đã chốt khách tự hủy yêu cầu hạ gói đang chờ trước thời điểm chuyển. Đổi gói đích phải hủy yêu cầu cũ rồi chọn lại. Thứ tự gói do Admin đặt theo [BR-SUB-020](BR-SUB-020.md), không dựa vào giá hoặc chu kỳ. Yêu cầu đang chờ giữ bản giá và quyền lợi đã chốt theo khoản 13. BASIC/PLUS/PRO chỉ là ví dụ, không phải danh mục đã duyệt.
- Làm mới hạn mức khi một kỳ mới hợp lệ bắt đầu và tác vụ đã giữ lượt tiếp tục theo [BR-SUB-002](BR-SUB-002.md), [BR-SUB-003](BR-SUB-003.md). Không coi việc đang chờ hạ gói là bắt đầu kỳ mới.
- Tham chiếu [BR-SUB-006](BR-SUB-006.md), [BR-SUB-014](BR-SUB-014.md), [BR-SUB-018](BR-SUB-018.md), [STORY-SUB-001](../userstory/STORY-SUB-001.md) và [ST-SUB-083](../systemtest/ST-SUB-083.md).
- [ST-SUB-084](../systemtest/ST-SUB-084.md) kiểm tra tự hủy yêu cầu đang chờ, không chuyển theo yêu cầu đã hủy và không tự gia hạn.
- [ST-SUB-088](../systemtest/ST-SUB-088.md) kiểm tra phải hủy yêu cầu hạ gói cũ trước khi chọn gói đích khác. Đã chốt muốn nâng khi đang có yêu cầu hạ chờ thì cũng phải hủy yêu cầu hạ trước, theo [BR-SUB-018](BR-SUB-018.md) và [ST-SUB-094](../systemtest/ST-SUB-094.md). Khi đang chờ hạ gói mà muốn chỉ đổi chu kỳ trong cùng gói, phải hủy yêu cầu hạ trước theo khoản 11; chiều ngược lại cũng áp dụng.
- [ST-SUB-090](../systemtest/ST-SUB-090.md) kiểm tra hạ gói kèm chọn chu kỳ khác, bắt đầu kỳ đích sau khi hết kỳ hiện tại và hủy toàn bộ yêu cầu đang chờ. Thời điểm thu tiền và các điều kiện thanh toán vẫn để thiết kế sau.
- Nếu gói đích ngừng bán sau khi yêu cầu chuyển đã được ghi nhận hợp lệ, giữ yêu cầu và vẫn thực hiện khi đến kỳ nếu đủ điều kiện theo [BR-SUB-013](BR-SUB-013.md); không tự hủy vì ngừng bán.
- Bản nháp còn thiếu metadata. Đặc tả test chưa thực thi; luồng đổi gói vẫn còn quyết định nghiệp vụ đang mở.
