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

# BR-PAY-004

## Rule Info

- **Name**: Cấp gói một lần và xác định thứ tự mua thiết kế theo thời điểm đủ tiền.
- **Category**: Thanh toán và gói dịch vụ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong hội thoại thiết kế thanh toán ngày 19/09/2026; xem discovery/payment-packages.md. Quyết định mới nhất được ưu tiên khi thay thế phương án trước đó.

## Statement

Mỗi đơn thanh toán hợp lệ chỉ cấp một gói. Với thiết kế, lần mua có thời điểm đủ tiền sau quyết định gói đang hiệu lực; webhook cũ đến muộn không được thay gói mua sau.

## When

Đơn đã đủ tiền theo BR-PAY-002 và BR-PAY-003, cần ghi nhận lần mua và cấp quyền.

## Then

1. Tự cấp gói thiết kế hoặc giám sát theo giá và quyền lợi đã lưu trong đơn, không chờ nhân viên tiếp nhận. Gói giám sát được cấp khi chưa gán công trình.
2. Thiết kế bắt đầu kỳ mới tại lúc hệ thống thực sự cấp gói; không tính lùi thời gian sử dụng về lúc giao dịch phát sinh chỉ vì webhook đến chậm.
3. Đổi gói, đổi tháng/năm hoặc mua lại cùng gói/cùng chu kỳ theo BR-SUB-021: cấp đủ hạn mức mới, bỏ thời gian và lượt dư cũ, không khấu trừ tiền. Lượt đã giữ vẫn thuộc kỳ cũ.
4. Nếu hai đơn thiết kế đều hợp lệ, ghi nhận cả hai lần mua. So sánh thời điểm mỗi đơn nhận đủ tiền, không so thời điểm tạo đơn hoặc webhook đến. Đơn mua trước được nhận diện là đã bị lần mua sau thay thế, không kích hoạt đè lên gói sau.
5. Webhook lặp hoặc khách chuyển đủ thêm lần nữa cho đơn đã hoàn tất không tạo gói thứ hai và không làm mới kỳ/lượt. Khoản chuyển thêm thực tế do nhân viên xử lý hoàn tiền bên ngoài.
6. Giám sát cho phép mỗi đơn hợp lệ cấp một gói riêng, kể cả các đơn cùng loại hoặc webhook đến ngược thứ tự; không dùng cơ chế thay thế gói thiết kế cho giám sát.

## Except

Nếu khoản cũ đến muộn làm thay đổi thời điểm đủ tiền và đảo thứ tự giữa các gói đã cấp (A đã thay B, sau đó mới biết A đủ tiền trước B), giữ gói A đang hiệu lực, không tự chuyển lại B. Nhân viên xử lý bên ngoài. Đây là ngoại lệ đã xác nhận khi thiết kế TDD, khác với nhận lần đầu một đơn mua trước sau khi đơn mua sau đã cấp.

Nếu hai đơn có cùng thời điểm đủ tiền theo độ chính xác SePay, đơn tạo sau được coi là lần mua sau. Nhân viên hủy gói theo BR-SUB-024; không có thao tác khôi phục vì BR-SUB-025 đã bỏ ngày 25/09/2026.

## Notes

Ví dụ A đủ tiền 10:14, B đủ tiền 10:17, webhook B đến trước A: ghi nhận cả A/B, B vẫn là gói hiệu lực. Không suy ra lịch sử kỳ sử dụng giả cho A; cách biểu diễn kỹ thuật cần được thiết kế.

Làm rõ phạm vi của ngoại lệ đảo thứ tự ở Except (ghi chú ngày 26/09/2026, không đổi quy tắc hay STORY-PAY-001/AC-027). Mỗi khách chỉ có một đơn thiết kế đang chờ, gồm cả đơn đã nhận một phần tiền (BR-PAY-001 khoản 3), và khoản hợp lệ phải phát sinh từ lúc tạo đơn tới trước lúc hết hạn hoặc lúc hủy (BR-PAY-002, BR-PAY-003). Vì vậy đơn tạo sau chỉ được tạo khi đơn trước đã rời trạng thái chờ. Đối chiếu với code trên `develop` của `bmt-be` tại `9c7b147` (`PurchaseOrderingPolicy`, `PaymentEligibilityPolicy`, `CreatePaymentOrderCommandHandler`, index `UX_PaymentOrder_OnePendingDesign`):

- Đơn trước đã hết hạn hoặc bị hủy khi tạo đơn sau: mọi khoản hợp lệ của đơn trước phát sinh trước lúc tạo đơn sau, còn khoản hợp lệ của đơn sau phát sinh từ lúc tạo đơn sau. Đơn trước vì vậy luôn đủ tiền trước (cùng giây thì đơn tạo sau là lần mua sau). Webhook đến muộn của đơn trước chỉ rơi vào khoản 4 của Then: ghi nhận, không kích hoạt đè gói sau. Không có đảo thứ tự.
- Đơn trước đã được ghi nhận đủ tiền và cấp gói khi tạo đơn sau: đơn trước (B) được cấp trước, đơn sau (A) được cấp sau và thay B như bình thường. Muốn A lại "đủ tiền trước B" thì khoản đến muộn của A phải phát sinh sau lúc tạo A nhưng trước thời điểm đủ tiền của B. Nghĩa là thời điểm giao dịch mà SePay báo cho khoản làm B đủ tiền phải muộn hơn lúc tạo A, dù hệ thống đã nhận và xử lý khoản đó trước lúc tạo A.

Như vậy, tình huống ở Except và AC-027 chỉ xảy ra khi thời điểm giao dịch SePay báo đi trước đồng hồ máy chủ BMT (lệch giờ, hoặc đọc sai múi giờ của `transactionDate`), đủ để khoản làm B đủ tiền mang thời điểm muộn hơn lúc tạo A. Nếu hai đồng hồ khớp nhau thì với quy tắc một đơn thiết kế đang chờ, tình huống này không xảy ra. Ví dụ theo AC-027: A được tạo lúc 10:13 theo giờ máy chủ, sau khi hệ thống đã cấp B; SePay lại báo khoản làm B đủ tiền lúc 10:16, tức đi trước đồng hồ máy chủ ít nhất ba phút. Phân tích này dựa trên code, chưa kiểm trên môi trường thật. Cần làm rõ: độ lệch thực tế giữa thời điểm giao dịch SePay và đồng hồ máy chủ; quy tắc và AC vẫn giữ nguyên để phòng trường hợp này.

- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Tên Reviewer/Approver lấy theo xác nhận cho các bản nháp mới trong discovery/subscription-entitlements.md; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa được phân công/xác nhận.
- [Tổng hợp quyết định và bảng truy vết](../discovery/payment-packages.md).
