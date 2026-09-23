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


# BR-SUB-020

## Rule Info

- **Name**: Xác định nâng và hạ gói theo thứ tự do Admin đặt.
- **Category**: Subscription thiết kế
- **Status**: Deprecated
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chọn Admin đặt thứ tự các gói, ví dụ BASIC < PLUS < PRO. Chuyển lên là nâng gói, chuyển xuống là hạ gói; không phụ thuộc giá bán hoặc lựa chọn tháng/năm. Người dùng xác nhận mỗi gói có một bậc riêng, không cho hai gói khác nhau ngang bậc. Khi gói đã có khách sử dụng, Admin không được đổi bậc; muốn thay đổi cách phân cấp thì tạo gói mới.

## Statement

**Đã được thay thế bởi [BR-SUB-021](BR-SUB-021.md). Toàn bộ nội dung quy tắc cũ bên dưới chỉ để tra lịch sử, không dùng triển khai hoặc nghiệm thu.**

Admin đặt thứ tự các gói thiết kế để hệ thống xác định nâng hay hạ gói. Mỗi gói có một bậc riêng; hai gói khác nhau không được cùng bậc. Gói đích cao hơn gói hiện tại trong thứ tự này thì là nâng; thấp hơn thì là hạ. Không phân loại bằng tên gói, giá bán, số lượt hoặc độ dài chu kỳ. Bậc của gói đã có khách sử dụng không được thay đổi; muốn phân cấp khác phải tạo gói mới.

## When

Admin cấu hình thứ tự gói thiết kế, hoặc hệ thống xác định loại thay đổi khi khách chọn gói đích.

## Then

1. Cho Admin đặt thứ tự các gói thiết kế. Lựa chọn tháng và năm của cùng một gói dùng chung vị trí trong thứ tự này.
2. Với hai gói khác nhau đã có thứ tự rõ ràng, nếu gói đích cao hơn thì áp dụng quy tắc nâng gói theo BR-SUB-018, kể cả khi khách chọn chu kỳ đích ngắn hơn hoặc giá kỳ đích thấp hơn số tiền của kỳ cũ.
3. Nếu gói đích thấp hơn thì áp dụng quy tắc hạ gói theo BR-SUB-019, kể cả khi chu kỳ đích dài hơn hoặc giá kỳ đích cao hơn số tiền của kỳ cũ.
4. Nếu vẫn là cùng gói và chỉ đổi tháng/năm thì áp dụng BR-SUB-015; không coi sang năm là nâng gói hoặc sang tháng là hạ gói chỉ vì khác chu kỳ.
5. Thứ tự gói chỉ dùng để phân loại chuyển gói, không tự cấp quyền của gói thấp hơn cho gói cao hơn. Quyền sử dụng vẫn kiểm tra theo bản quyền lợi đã cấp; không tự suy ra mọi gói cao đều chứa toàn bộ quyền của gói thấp. Thứ tự gói khác với các mức của một quyền lợi theo BR-SUB-005.

6. Không chấp nhận cấu hình thứ tự có hai gói thiết kế khác nhau cùng bậc. Nếu Admin gửi cấu hình trùng bậc, từ chối ghi nhận cấu hình đó và báo rõ cần đặt bậc khác nhau, kể cả yêu cầu gửi trực tiếp. Giữ nguyên cấu hình hợp lệ trước đó; không tự đổi bậc, dùng giá để phá hòa hoặc phân loại hai gói này là chỉ đổi chu kỳ.
7. Tháng và năm của cùng một gói vẫn dùng chung một bậc; đây không phải hai gói khác nhau bị trùng bậc. Thay đổi giá hoặc hạn mức không tự làm đổi thứ tự gói.

8. Khi gói đã từng có khách sử dụng, từ chối mọi thao tác đổi bậc của chính gói đó, kể cả yêu cầu gửi trực tiếp hoặc công bố bản cấu hình khác làm đổi bậc. Giữ nguyên bậc đã có, không đổi cách phân loại nâng/hạ do thao tác bị từ chối. Việc các kỳ sử dụng cũ đã hết hạn không tự mở lại quyền sửa bậc.
9. Nếu muốn có gói ở bậc khác, Admin tạo gói mới với bậc riêng chưa trùng gói khác và tuân theo các điều kiện cấu hình hợp lệ. Không tự chuyển khách, quyền lợi đã cấp hoặc yêu cầu đang chờ từ gói cũ sang gói mới. Quy tắc khóa bậc không cấm sửa giá/quyền lợi theo quy trình đã chốt tại BR-SUB-004.

## Except

Không áp dụng cho gói giám sát theo dự án. Điều kiện lưu nháp gói chưa đặt bậc còn cần chốt; không tự gán bậc mặc định hoặc dùng giá để phân loại thay.

## Notes

- BASIC < PLUS < PRO chỉ là ví dụ. Thứ tự và danh mục bán thực tế do Admin cấu hình, chưa cố định từ tên gói trên trang tham khảo.
- Đã chốt khóa bậc khi gói đã có khách sử dụng. Điều kiện sửa bậc của gói chưa có khách sử dụng nhưng đã có yêu cầu chuyển đang chờ, và thời điểm chốt bậc cho yêu cầu đó, còn cần làm rõ. Không tự sửa yêu cầu đang chờ khi Admin tạo gói mới.
- Tham chiếu [STORY-SUB-001](../userstory/STORY-SUB-001.md), [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-018](BR-SUB-018.md), [BR-SUB-019](BR-SUB-019.md), [BR-SUB-015](BR-SUB-015.md), [BR-SUB-005](BR-SUB-005.md) và [ST-SUB-091](../systemtest/ST-SUB-091.md).
- [ST-SUB-092](../systemtest/ST-SUB-092.md) kiểm tra từ chối cấu hình trùng bậc giữa hai gói khác nhau và giữ bậc chung cho tháng/năm của cùng gói. Cách xử lý số bậc của gói ngừng bán hoặc đã lưu trữ còn cần làm rõ nếu cho tái sử dụng.
- [ST-SUB-093](../systemtest/ST-SUB-093.md) kiểm tra không đổi bậc gói đã có khách sử dụng. [BR-SUB-004](BR-SUB-004.md) vẫn áp dụng cho thay đổi giá/quyền lợi; không coi khóa bậc là khóa toàn bộ cấu hình gói.
- Bản nháp còn thiếu metadata; đặc tả test chưa thực thi. Điều kiện cấu hình thứ tự chưa đầy đủ.
