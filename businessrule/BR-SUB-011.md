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


# BR-SUB-011

## Rule Info

- **Name**: Gói giám sát theo dự án, hoàn thành bằng thao tác thủ công.
- **Category**: Gói giám sát
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng yêu cầu hoãn quản lý lịch và lượt đang vận hành offline; gói giám sát không có chu kỳ, theo dự án cố định và có nút bấm hoàn thành cho khách hàng cụ thể khi dự án xong. Người dùng chọn Admin và nhân viên phụ trách được bấm hoàn thành; nhân viên phụ trách được xác định theo phân công dự án, không theo từng gói.

## Statement

**Cập nhật 24/09/2026:** Hoàn thành vẫn thuộc phạm vi và áp dụng trên vòng đời mới: chỉ gói đã gán dự án mới được hoàn thành. Người bấm phải có quyền `supervision.complete`; Admin có quyền này thì không cần phân công, nhân viên có quyền này phải được phân công trực tiếp dự án. Gói đã hoàn thành vẫn giữ chỗ trên dự án và không được sửa dự án.

**Giới hạn áp dụng từ 19/09/2026:** Nội dung hoàn thành thủ công dưới đây là luồng đã thiết kế trước, không phải thao tác hủy gói. Phạm vi thanh toán hiện chỉ quản lý gói gán vào dự án nào, không thêm điều kiện hoạt động khảo sát/giám sát. Quy định “không ngày hết hạn” chỉ còn áp dụng sau khi gán đúng hạn; gói chưa gán có hạn một năm theo BR-SUB-022. Nhân viên được sửa liên kết theo BR-SUB-023; không dùng “gắn cố định” để cấm ngoại lệ mới. Hủy/khôi phục theo BR-SUB-024/BR-SUB-025.

Gói giám sát của khách hàng gắn cố định với một dự án, không có chu kỳ tháng/năm hoặc ngày hết hạn. Khi dự án xong, Admin hoặc nhân viên phụ trách dự án đó được bấm Hoàn thành gói giám sát để ghi nhận gói đã hoàn thành.

## When

Admin hoặc nhân viên phụ trách chọn đúng khách hàng, dự án và gói đã gán dự án đó, rồi bấm Hoàn thành gói giám sát khi dự án đã xong.

## Then

1. Chuyển gói được chọn từ đã gán sang đã hoàn thành. Không bắt nhập lý do; lưu người thao tác và thời điểm. Từ chối nếu gói chưa gán hoặc đang bị hủy.
2. Giữ đúng liên kết khách hàng và dự án của gói; không hoàn thành các gói khác của khách hàng, không thay đổi subscription thiết kế.
3. Không yêu cầu nhập số lượt đã dùng, trừ hết lượt hoặc đóng các lịch hẹn trên nền tảng để hoàn thành gói, vì các phần này đang vận hành offline.
4. Gói không tự hết hạn hoặc hoàn thành chỉ do thời gian trôi qua; không tự gia hạn, tạo kỳ mới hoặc làm mới hạn mức giám sát.

5. Khách hàng và nhân viên không phụ trách không được hoàn thành gói. Hệ thống kiểm tra quyền trên yêu cầu xử lý, không chỉ ẩn nút trên giao diện.
6. Người thao tác phải có quyền `supervision.complete`. Admin có quyền này thì không cần phân công. Nhân viên có quyền này phải đang được phân công trực tiếp dự án; phân công ở mức khách hàng sở hữu dự án không tính.
7. Gói đã hoàn thành vẫn gắn với dự án và vẫn giữ chỗ theo [BR-SUB-006](BR-SUB-006.md). Không được sửa dự án của gói đã hoàn thành; phải mở lại trước theo [BR-SUB-012](BR-SUB-012.md). Khách vẫn xem được trạng thái đã hoàn thành của gói mình.

## Except

Nếu bấm hoàn thành nhầm, Admin hoặc nhân viên phụ trách được mở lại và phải ghi lý do theo [BR-SUB-012](BR-SUB-012.md). Chưa yêu cầu tự chuyển trạng thái dự án hoặc tự hoàn thành gói theo sự kiện khác.

## Notes

- Đây là ngoại lệ của [BR-RBAC-013](BR-RBAC-013.md) điểm 3: phân công mức khách hàng không có hiệu lực xuống dự án cho thao tác hoàn thành và mở lại gói giám sát.
- Quyền nhân viên phụ trách chỉ áp dụng cho gói giám sát của dự án được phân công. Dùng phân công hiện tại của dự án để kiểm tra quyền hoàn thành hoặc mở lại, không tạo phân công riêng cho từng gói. Cách lưu dữ liệu, quản lý phân công và ánh xạ vai trò backend sẽ làm rõ trong thiết kế; không mặc định mọi nhân viên có quyền trên mọi khách hàng.
- [ST-SUB-028](../systemtest/ST-SUB-028.md) kiểm tra từ chối người không có quyền.
- Đang thực hiện/đã hoàn thành là trạng thái nghiệp vụ đề xuất cách gọi; mã trạng thái và API sẽ thiết kế sau.
- Không thêm thao tác gửi thông báo hoặc thanh toán vào nút hoàn thành.
- [BR-SUB-006](BR-SUB-006.md) quy định tối đa một gói giám sát đang thực hiện trên mỗi dự án; [BR-SUB-009](BR-SUB-009.md) quy định phạm vi dự án.
- Tham chiếu [STORY-SUB-003](../userstory/STORY-SUB-003.md), [ST-SUB-026](../systemtest/ST-SUB-026.md), [ST-SUB-027](../systemtest/ST-SUB-027.md) và [nợ nghiệp vụ](../debt/supervision-offline.md).
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
