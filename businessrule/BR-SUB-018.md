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


# BR-SUB-018

## Rule Info

- **Name**: Nâng gói thiết kế có hiệu lực ngay và bắt đầu một kỳ mới.
- **Category**: Subscription thiết kế
- **Status**: Deprecated
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chọn nâng gói có hiệu lực ngay khi hoàn tất và bắt đầu kỳ mới. Bỏ lượt dư cũ, cấp đủ hạn mức mới, trả đủ giá kỳ mới và không trừ tiền cho thời gian cũ còn lại. Khách được chọn luôn chu kỳ tháng/năm của gói đích trong lần nâng, ví dụ BASIC tháng lên PRO năm; không phải nâng cùng chu kỳ rồi chờ đổi chu kỳ sau. Nếu đang có yêu cầu hạ gói hoặc chỉ đổi chu kỳ đang chờ, khách phải hủy yêu cầu đó trước rồi mới nâng.

## Statement

**Đã được thay thế bởi [BR-SUB-021](BR-SUB-021.md). Toàn bộ nội dung quy tắc cũ bên dưới chỉ để tra lịch sử, không dùng triển khai hoặc nghiệm thu.**

Khi việc nâng gói thiết kế hoàn tất hợp lệ, tài khoản bắt đầu dùng quyền của gói mới ngay tại thời điểm đó, không chờ ngày hết hạn của gói cũ. Khách được chọn chu kỳ tháng hoặc năm của gói đích ngay trong lần nâng. Kỳ mới bắt đầu tại thời điểm nâng gói hoàn tất và được tính đủ chu kỳ từ mốc đó theo BR-SUB-014. Không giữ ngày hết hạn cũ hoặc cộng phần thời gian còn lại vào kỳ mới. Kỳ mới nhận đủ hạn mức của gói đích, bỏ lượt dư chưa dùng của gói cũ, không cộng dồn. Khách trả đủ giá kỳ gói mới, không trừ tiền cho phần thời gian gói cũ còn lại.

## When

Một yêu cầu nâng gói thiết kế đã hoàn tất hợp lệ và được ghi nhận. Các điều kiện để hoàn tất, bao gồm phần liên quan thanh toán, còn cần thiết kế; việc khách bấm chọn gói chưa tự đủ điều kiện chuyển quyền.

## Then

1. Áp dụng quyền của gói đích ngay khi việc nâng gói hoàn tất; các yêu cầu sử dụng tính năng sau đó kiểm tra quyền gói mới.
2. Gọi T là thời điểm nâng gói hoàn tất: kỳ mới bắt đầu tại T; gói cũ không tiếp tục có hiệu lực sau T. Ngày kết thúc kỳ mới tính từ T theo chu kỳ đã được xác định hợp lệ và BR-SUB-014, giữ giờ theo Việt Nam và lấy cuối tháng đích khi thiếu ngày tương ứng. Không trì hoãn đến ngày hết hạn cũ hoặc cộng thời gian chưa dùng của gói cũ vào kỳ mới.
3. Vẫn bảo đảm tài khoản chỉ có một subscription thiết kế đang hiệu lực theo BR-SUB-006; không cấp hai bộ quyền độc lập từ hai subscription cùng hiệu lực.
4. Tại T, cấp đủ hạn mức của các quyền dạng lượt có trong gói đích cho kỳ mới. Bỏ lượt dư chưa dùng của kỳ cũ; không cộng sang gói mới và không trừ số lượt đã dùng ở kỳ cũ vào hạn mức mới. Ví dụ cũ còn 3, mới cấp 20 thì kỳ mới có 20, không phải 23. Quyền không giới hạn vẫn theo BR-SUB-005; không tự cấp quyền hoặc lượt của quyền không có trong gói đích.
5. Lượt đang giữ cho tác vụ đã bắt đầu hợp lệ vẫn thuộc kỳ cũ theo BR-SUB-003 và BR-SUB-002; không coi đó là lượt dư sẵn dùng để xóa hoặc chuyển sang kỳ mới. Thành công ghi nhận ở kỳ giữ; lỗi giải phóng ở kỳ đó, không cộng vào kỳ mới hoặc mở lại quyền sử dụng kỳ đã kết thúc.
6. Số tiền phải trả khi nâng bằng toàn bộ giá kỳ gói đích đã xác định hợp lệ. Không giảm số tiền này bằng giá trị phần thời gian gói cũ chưa dùng. Ví dụ kỳ mới giá 600.000đ thì khách trả 600.000đ, dù gói cũ vẫn còn thời gian sử dụng.

7. Cho phép nâng gói và chọn chu kỳ khác trong cùng một lần. Ngay khi hoàn tất hợp lệ, bắt đầu kỳ mới theo chu kỳ đích, tính đủ giá và cấp đủ hạn mức tương ứng; không chờ hết kỳ cũ dù có đổi tháng/năm. Ví dụ BASIC tháng lên PRO năm thì bắt đầu kỳ PRO năm ngay, trả đủ giá năm và nhận hạn mức dùng cả năm; không cấp hạn mức tháng hoặc làm mới mỗi tháng. Các quy tắc bỏ lượt dư, không khấu trừ thời gian cũ và xử lý lượt đang giữ ở trên vẫn áp dụng.

8. Nếu tài khoản đang có yêu cầu hạ gói chờ áp dụng từ kỳ tiếp theo, khách phải hủy yêu cầu đó thành công trước khi gửi yêu cầu nâng gói mới. Từ chối nâng khi yêu cầu hạ vẫn đang chờ, kể cả yêu cầu gửi trực tiếp; báo khách cần hủy yêu cầu hạ trước. Không tự hủy hoặc thay thế yêu cầu hạ, không đổi gói, kỳ, quyền hoặc số lượt do lần nâng bị từ chối.
9. Sau khi hủy thành công, kiểm tra yêu cầu nâng mới theo các điều kiện hợp lệ thông thường. Hủy yêu cầu hạ không tự hoàn tất nâng gói. Nếu khách chưa nâng hoặc nâng không hoàn tất, yêu cầu hạ cũ vẫn đã hủy; không tự khôi phục. Quy tắc này áp dụng cả khi yêu cầu hạ cũ có kèm đổi chu kỳ.

10. Nếu tài khoản đang có yêu cầu chỉ đổi tháng/năm trong cùng gói đang chờ, cũng phải hủy yêu cầu đó thành công trước khi gửi yêu cầu nâng gói mới. Nếu chưa hủy, từ chối nâng và hướng dẫn hủy trước, kể cả yêu cầu gửi trực tiếp; giữ nguyên yêu cầu đổi chu kỳ, kỳ hiện tại, quyền và lượt. Không tự hủy hoặc thay thế yêu cầu đổi chu kỳ. Sau khi hủy thành công, yêu cầu nâng mới được kiểm tra theo các điều kiện hợp lệ; hủy không tự hoàn tất nâng. Nếu chưa nâng hoặc nâng không hoàn tất, không tự khôi phục yêu cầu đổi chu kỳ đã hủy.

## Except

Không áp dụng mặc định cho hạ gói hoặc chỉ đổi tháng/năm trong cùng gói. Hạ gói có hiệu lực từ kỳ tiếp theo theo [BR-SUB-019](BR-SUB-019.md); đổi tháng/năm trong cùng gói theo cả hai chiều đều áp dụng từ lúc hết kỳ hiện tại theo [BR-SUB-015](BR-SUB-015.md). Kết hợp nâng gói với đổi chu kỳ áp dụng ngay theo quy tắc này; kết hợp hạ gói với đổi chu kỳ được phép và áp dụng từ kỳ tiếp theo theo BR-SUB-019. Admin sửa và Công bố danh mục vẫn theo BR-SUB-004, không tự nâng gói của khách. Các thao tác quản lý gói giám sát không thuộc quy tắc này.

## Notes

- Ví dụ BASIC/PRO và ngày 15/9, 30/9 chỉ minh họa. Gói nào cao hơn được xác định theo thứ tự Admin đặt tại [BR-SUB-020](BR-SUB-020.md), không dựa vào tên, giá hoặc chu kỳ. Danh mục thực tế và bản quyền lợi đích dùng cho lần nâng còn cần chốt.
- Đã chốt ngày hết hạn sau nâng tính từ thời điểm nâng hoàn tất. Ví dụ cùng chu kỳ tháng: nâng ngày 15/9 lúc 10:00 thì kỳ mới kết thúc 15/10 lúc 10:00 theo giờ Việt Nam, dù gói cũ dự kiến kết thúc 30/9. Đã chốt cấp đủ hạn mức gói mới, bỏ lượt dư cũ. Tác vụ chạy qua mốc chuyển kỳ vẫn theo BR-SUB-003, không tự hủy hoặc tính lại vào kỳ mới. Đã chốt trả đủ giá kỳ mới, không khấu trừ thời gian gói cũ còn lại. Việc chọn bản giá áp dụng cho lần nâng còn cần chốt. Chưa quyết định xử lý kỳ tương lai đã được đăng ký/gia hạn trước đó nếu có.
- Quy tắc số tiền khi nâng không quyết định chính sách hoàn tiền riêng khi hủy gói, tranh chấp hoặc thanh toán lỗi; các luồng này vẫn để thiết kế sau cùng thanh toán. [ST-SUB-082](../systemtest/ST-SUB-082.md) đặc tả cách tính số tiền khi nâng, chưa thực thi.
- Thiết kế nghiệp vụ nâng gói thuộc đợt hiện tại; tích hợp thanh toán vẫn làm sau. Không tự thêm luồng Admin cấp/nâng gói thủ công để thay thế thanh toán.
- Tham chiếu [STORY-SUB-001](../userstory/STORY-SUB-001.md), [BR-SUB-004](BR-SUB-004.md), [BR-SUB-006](BR-SUB-006.md), [BR-SUB-015](BR-SUB-015.md), [BR-SUB-014](BR-SUB-014.md), [ST-SUB-079](../systemtest/ST-SUB-079.md) và [ST-SUB-080](../systemtest/ST-SUB-080.md).
- [ST-SUB-081](../systemtest/ST-SUB-081.md) kiểm tra cấp đủ hạn mức mới và bỏ lượt dư cũ. Tham chiếu [BR-SUB-002](BR-SUB-002.md), [BR-SUB-003](BR-SUB-003.md), [BR-SUB-005](BR-SUB-005.md) cho làm mới lượt, tác vụ chạy qua kỳ và không giới hạn.
- [ST-SUB-089](../systemtest/ST-SUB-089.md) kiểm tra nâng gói kèm đổi chu kỳ, bắt đầu ngay kỳ đích và dùng đúng giá/hạn mức của lựa chọn đó. Đã chốt phải hủy yêu cầu hạ gói đang chờ trước khi nâng, kể cả yêu cầu hạ kèm đổi chu kỳ. Đã chốt yêu cầu chỉ đổi tháng/năm trong cùng gói đang chờ cũng phải được khách hủy trước khi nâng; xem [ST-SUB-095](../systemtest/ST-SUB-095.md).
- [ST-SUB-094](../systemtest/ST-SUB-094.md) kiểm tra phải hủy yêu cầu hạ đang chờ trước khi nâng và giữ nguyên yêu cầu đó khi lần nâng bị từ chối.
- Bản nháp chưa đủ metadata. Đặc tả test chưa được thực thi; toàn bộ luồng nâng vẫn còn quyết định nghiệp vụ đang mở.
