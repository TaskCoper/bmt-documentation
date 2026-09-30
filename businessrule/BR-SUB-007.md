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


# BR-SUB-007

## Rule Info

- **Name**: Điều kiện tạo/lưu dự án và quyền sử dụng hồ sơ cũ khi gói thiết kế hết hạn.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận hết hạn vẫn xem, tải kết quả cũ và xuất PDF; không sửa/lưu thông tin hoặc bắt đầu tác vụ mới cần gói. Tác vụ đã bắt đầu hợp lệ được tiếp tục. Tạo/lưu dự án cần gói còn hiệu lực, quyền tạo thiết kế và lượt sẵn dùng. Dùng hết hoặc toàn bộ lượt còn lại đang giữ đều chặn tạo/lưu. Nếu tác vụ lỗi và trả lượt, khách được tiếp tục khi gói, quyền còn hợp lệ. Ngày 25/09/2026, người dùng xác nhận chỉ tên dự án được đổi bất cứ lúc nào; thông tin đầu vào giữ nguyên điều kiện gói, quyền và lượt.

## Statement

Quy tắc này chỉ áp dụng cho subscription thiết kế. Gói giám sát không có chu kỳ hoặc ngày hết hạn; xem [BR-SUB-011](BR-SUB-011.md).

Khách chỉ được tạo hoặc lưu thông tin dự án khi có gói thiết kế còn hiệu lực, có quyền tạo thiết kế và còn ít nhất một lượt sẵn dùng hoặc được cấp không giới hạn lượt. Khách mới chưa có gói, khách dùng gói chỉ có tra cứu và khách không còn lượt tạo thiết kế sẵn dùng do đã dùng hết hoặc đang giữ hết đều không được tạo/lưu dự án, kể cả lưu nháp. Riêng đổi tên dự án đã có không cần các điều kiện này, theo khoản 11.

Khi subscription hết hạn và chưa gia hạn, tài khoản vẫn xem được dự án và kết quả đã tạo mà tài khoản có quyền truy cập, đồng thời được tải xuống tệp kết quả đã có sẵn trước khi hết hạn và xuất PDF từ kết quả thiết kế cũ, kể cả chưa có PDF trước đó. Khách không được sửa hoặc lưu thông tin đầu vào của dự án khi gói đã hết hạn; riêng tên dự án được đổi bất cứ lúc nào theo khoản 11. Hệ thống không cho bắt đầu thao tác mới cần quyền subscription, kể cả khi kỳ đã hết hạn còn lượt chưa dùng.

## When

Khách yêu cầu tạo/lưu dự án, hoặc truy cập dự án, kết quả cũ hay bắt đầu thao tác cần quyền subscription sau khi hết hạn và chưa có kỳ mới hợp lệ.

## Then

1. Cho phép xem dự án, kết quả đã tạo và tải tệp kết quả đã có sẵn trước khi hết hạn, theo quyền truy cập dữ liệu của tài khoản. Không yêu cầu gia hạn chỉ để tải lại tệp này; tải tệp đã có không phải tạo thiết kế, mở chi tiết mẫu để tra cứu.
2. Khi subscription đã hết hạn và chưa có kỳ mới hợp lệ, từ chối bắt đầu thao tác mới cần quyền subscription; không giữ hoặc tính lượt cho yêu cầu bị từ chối.
3. Cho phép yêu cầu xuất PDF từ kết quả thiết kế cũ có quyền truy cập dù subscription đã hết hạn; không yêu cầu PDF phải được xuất sẵn trước khi hết hạn. Đây là xuất tệp từ kết quả đã có, không phải yêu cầu AI tạo thiết kế.
4. Từ chối yêu cầu của khách sửa/lưu ghi chú hoặc thông tin đầu vào khi subscription thiết kế đã hết hạn và chưa có kỳ mới hợp lệ. Đổi tên dự án theo khoản 11. Giữ nguyên dữ liệu đã lưu; kiểm tra tại nơi xử lý yêu cầu, không chỉ khóa biểu mẫu trên giao diện.
5. Không tự cấp kỳ mới hoặc làm mới hạn mức chỉ vì khách xem, tải hoặc xuất PDF từ kết quả cũ.
6. Với khách mới chưa từng có gói thiết kế, từ chối tạo dự án và lưu thông tin dự án, kể cả lưu nháp hoặc gửi yêu cầu trực tiếp. Báo cần có gói còn hiệu lực; không tạo bản ghi dự án hay lưu thông tin từ yêu cầu bị từ chối, không giữ/trừ lượt hoặc tự cấp gói. Điều kiện quyền tạo thiết kế và lượt sẵn dùng khi bấm Gen AI vẫn giữ nguyên.

7. Từ chối tạo/lưu thông tin dự án nếu gói đang hiệu lực chỉ có tra cứu, không có quyền tạo thiết kế. Áp dụng cho lưu nháp và yêu cầu gửi trực tiếp; báo cần quyền tạo thiết kế, không tạo dự án hoặc ghi thay đổi từ yêu cầu bị từ chối, không giữ/trừ lượt. Kiểm tra quyền theo bản đã cấp, không theo bản nháp hoặc tên gói. Quyền tra cứu hiện có của tài khoản không bị thay đổi.

8. Với quyền tạo thiết kế có hạn mức hữu hạn, khi đã dùng hết lượt trong kỳ thì từ chối tạo dự án hoặc lưu thông tin dự án, dù gói vẫn còn hiệu lực. Áp dụng cả lưu nháp, yêu cầu gửi trực tiếp và biểu mẫu mở trước khi hết lượt. Báo đã hết lượt tạo thiết kế; không tạo bản ghi, không ghi thay đổi từ yêu cầu bị từ chối, giữ nguyên thông tin đã lưu. Không giữ/trừ thêm lượt hoặc tự cấp lượt. Điều kiện lượt ở khoản này không áp dụng cho đổi tên theo khoản 11.

9. Khi toàn bộ lượt tạo thiết kế còn lại đang được giữ cho tác vụ chạy, tạm chặn tạo/lưu dự án vì không còn lượt sẵn dùng. Lượt đang giữ chưa phải lượt đã dùng. Nếu tác vụ thất bại/quá thời gian và giải phóng lượt, khách được gửi lại yêu cầu tạo/lưu khi có ít nhất một lượt sẵn dùng, gói còn hiệu lực và còn quyền tạo thiết kế; không tự lưu lại yêu cầu từng bị từ chối. Nếu tác vụ thành công dùng hết lượt, tiếp tục chặn theo khoản 8. Khoản này không chặn đổi tên theo khoản 11.

10. Khi gói thiết kế hết hạn, chủ sở hữu bản dự toán vẫn được đổi tên bản dự toán theo khoản 11, xuất PDF/Excel, tạo link chia sẻ và QR, gửi email chứa link, chọn ngày hết hạn và thu hồi link của hồ sơ đã có. Không yêu cầu gia hạn hoặc còn lượt tạo thiết kế cho các thao tác này; không giữ/trừ lượt hay gọi AI tạo thiết kế mới. Link, QR và email tuân theo BR-PROJ-006. Người nhận dùng link còn hiệu lực được xem/tải không cần đăng nhập hoặc có subscription.

11. Chủ sở hữu được đổi tên dự án bất cứ lúc nào, kể cả khi gói đã hết hạn, đã dùng hết lượt, toàn bộ lượt còn lại đang được giữ hoặc AI đang xử lý dự án đó. Tên vẫn phải hợp lệ theo BR-PROJ-003 và chỉ chủ sở hữu được đổi. Tên không phải đầu vào gửi AI; đổi tên không giữ/trừ lượt, không gọi AI và không mở quyền sửa thông tin đầu vào. Cách hiển thị tên mới trên hồ sơ chia sẻ và tệp đã xuất theo BR-PROJ-007.

## Except

Quyền xem, đổi tên và sử dụng hồ sơ cũ chỉ áp dụng cho bản dự toán chưa bị xóa. Theo [BR-PROJ-008](BR-PROJ-008.md), khách vẫn được xem danh sách dự toán còn lại khi hết hạn gói hoặc hết lượt. Theo [BR-PROJ-009](BR-PROJ-009.md), khách vẫn được xóa bản thuộc mình đủ điều kiện, nhưng chặn bản đang được AI xử lý; xóa không hoàn lượt, không thể khôi phục và không mở lại hồ sơ đã xóa. Đây không phải quyền sửa đầu vào hoặc bắt đầu tác vụ AI mới.

Tác vụ đã bắt đầu hợp lệ trước khi hết hạn được tiếp tục đến khi hoàn thành dù chưa gia hạn. Với tạo thiết kế, thành công tính lượt vào kỳ đã giữ; lỗi giải phóng lượt giữ nhưng không cho dùng lại lượt đã hết hạn, theo [BR-SUB-003](BR-SUB-003.md). Xuất PDF từ kết quả cũ là thao tác được phép sau hết hạn. Quyết định này không mở quyền Gen AI tạo mới, không thay đổi kết quả nguồn. Sửa/lưu thông tin đầu vào của dự án cần subscription còn hiệu lực, có quyền tạo thiết kế và còn lượt sẵn dùng hoặc được cấp không giới hạn; đổi tên theo khoản 11 không cần các điều kiện này. Xuất Excel dự toán và quản lý chia sẻ hồ sơ cũ áp dụng khoản 10 theo xác nhận bổ sung của người dùng.

## Notes

- Phần danh sách và xóa dự toán được bổ sung theo các quyết định mới trong hội thoại; người dùng đã chốt bộ STORY-PROJ-006/007 và BR-PROJ-008/009 cùng phần bổ sung này trong hội thoại ngày 30/09/2026. Đặc tả ST bổ sung ở [bảng độ phủ](../discovery/my-estimates-system-test-coverage.md); chưa triển khai hoặc thực thi; các khoản quyền/gói/lượt hiện có giữ nguyên với bản chưa bị xóa.
- Nhóm STORY-PROJ-*** đã được người dùng đổi tên thành “Tạo dự toán”. Khi áp dụng quy tắc này cho nhóm đó, các thao tác tạo/lưu và truy cập kết quả trước đây gọi là “dự án” được hiểu là thao tác trên bản dự toán. Đây là đối chiếu thuật ngữ, không thay đổi quyền/gói/lượt và không quy định tính năng quản lý dự án trong tương lai. Gói giám sát không gắn với bản dự toán mà gắn với công trình, một thực thể riêng do khách tự tạo; hai thực thể này không liên kết trong đợt này (người dùng xác nhận ngày 25/09/2026).

- Quyền xem/tải hồ sơ cũ không tự mở quyền xem dữ liệu của tài khoản khác. Chủ dự án có thể cấp quyền xem/tải hồ sơ qua link còn hiệu lực theo BR-PROJ-006; quyền chia sẻ này không cấp quyền sửa hoặc tạo thiết kế.
- Đã chốt tạo/lưu dự án cần gói còn hiệu lực và quyền tạo thiết kế; gói chỉ có tra cứu không đủ điều kiện. Đã chốt dùng hết lượt tạo thiết kế thì chặn tạo/lưu thông tin dự án. Quyền không giới hạn không bị coi là hết lượt vì số lần đã dùng. Đã chốt toàn bộ lượt còn lại đang giữ cũng tạm chặn tạo/lưu dự án. Khi giải phóng lượt về kỳ còn hiệu lực, kiểm tra lại gói, quyền và lượt ở yêu cầu tiếp theo. Giải phóng lượt thuộc kỳ đã hết hạn không mở quyền tạo/lưu trở lại. Quy tắc này không thay đổi thao tác của Admin/nhân viên đối với gói giám sát.
- [ST-SUB-074](../systemtest/ST-SUB-074.md) kiểm tra gói chỉ có tra cứu không được tạo/lưu dự án.
- [ST-SUB-073](../systemtest/ST-SUB-073.md) kiểm tra khách mới chưa có gói bị từ chối tạo/lưu dự án.
- Chính sách lưu trữ dữ liệu dài hạn chưa được chốt; không suy ra lưu vĩnh viễn từ quyền xem dữ liệu cũ.
- Luồng mua và nhận gói theo STORY-PAY-001; mua lại trước hạn theo BR-SUB-021.
- Tham chiếu [STORY-SUB-001](../userstory/STORY-SUB-001.md), [BR-SUB-003](BR-SUB-003.md), [ST-SUB-013](../systemtest/ST-SUB-013.md) và [ST-SUB-014](../systemtest/ST-SUB-014.md).
- [ST-SUB-055](../systemtest/ST-SUB-055.md) kiểm tra tải tệp kết quả đã có sau khi hết hạn.
- [ST-SUB-056](../systemtest/ST-SUB-056.md) kiểm tra xuất PDF từ kết quả cũ chưa từng có PDF sau khi hết hạn.
- Quy tắc khóa sửa/lưu ở đây áp dụng cho khách hàng khi gói thiết kế hết hạn; không thay đổi quyền Admin/nhân viên hoàn thành hoặc mở lại gói giám sát đã chốt.
- [ST-SUB-057](../systemtest/ST-SUB-057.md) kiểm tra từ chối sửa/lưu thông tin sau hết hạn, kể cả gửi yêu cầu trực tiếp.
- [ST-SUB-075](../systemtest/ST-SUB-075.md) kiểm tra hết lượt thì chặn tạo/lưu dự án, kể cả biểu mẫu đã mở trước khi lượt cuối được dùng. Quy tắc tính lượt vẫn giữ nguyên: tạo/lưu thông tin không tự giữ hoặc trừ lượt; chỉ tiếp nhận Gen AI hợp lệ mới giữ lượt theo [BR-SUB-003](BR-SUB-003.md).
- [ST-SUB-076](../systemtest/ST-SUB-076.md) kiểm tra chặn khi lượt cuối đang giữ và cho tạo/lưu lại sau khi tác vụ lỗi trả lượt trong kỳ còn hiệu lực. Tạo/lưu thông tin chỉ kiểm tra điều kiện, không tự giữ/trừ lượt. Giới hạn này áp dụng cho thao tác của khách; không ngăn hệ thống lưu kết quả của tác vụ AI đã được tiếp nhận hợp lệ.
- Xác nhận bổ sung trong hội thoại chuẩn bị tạo dự toán: cho phép đầy đủ các thao tác với hồ sơ đã có sau khi gói hết hạn. Khoản 10 làm rõ phần Excel và chia sẻ trước đây chưa quyết định. Người dùng đã chốt bộ US/BR của Tạo dự toán. ST-PROJ-035 kiểm tra PDF/Excel cũ sau hết hạn; ST-PROJ-046 kiểm tra chia sẻ và email. Các ca mới bổ sung phạm vi này, chưa được thực thi; không thay đổi các đặc tả ST-SUB đã có.
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
