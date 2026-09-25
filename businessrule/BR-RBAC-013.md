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

# BR-RBAC-013

## Rule Info

- **Name**: Phân công ghi theo loại tài nguyên và có thời hạn hiệu lực.
- **Category**: Quản lý người dùng và phân quyền
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định người dùng xác nhận trong hội thoại thiết kế RBAC ngày 20/09/2026: hỗ trợ phân công theo nhiều loại tài nguyên bằng một cơ chế chung. Quyết định người dùng xác nhận trong hội thoại rà soát ngày 25/09/2026: đợt này chỉ phân công theo công trình, mỗi công trình một người phụ trách, người nhận phải có quyền `supervision.complete`, danh sách cần chia lại gồm cả công trình có người phụ trách đang bị khóa. Cũng trong ngày 25/09/2026, khi chuẩn bị tính năng Công trình, người dùng xác nhận đổi đối tượng phân công từ công trình sang từng gói giám sát: chỉ giao gói đang giữ chỗ trên công trình; danh sách cần chia lại chỉ gồm gói đang gán; gói bị hủy vẫn giữ phân công.

## Statement

Phân công được ghi theo loại tài nguyên. Mỗi phân công cho biết nhân viên nào phụ trách loại tài nguyên nào, tài nguyên cụ thể nào, hiệu lực từ lúc nào và tới lúc nào. Trong đợt này chỉ có một loại tài nguyên là gói giám sát, và mỗi gói tại một thời điểm chỉ có một người phụ trách; các loại khác thêm sau dùng cùng cơ chế.

## When

Người có quyền `assignment.manage` tạo, chuyển giao hoặc gỡ một phân công; hoặc hệ thống kiểm tra một người có đang được phân công một tài nguyên hay không.

## Then

1. Ghi phân công gồm nhân viên, loại tài nguyên, định danh tài nguyên và thời điểm bắt đầu hiệu lực. Thời điểm kết thúc để trống nghĩa là còn hiệu lực.
2. Chỉ phân công được cho tài khoản nhân viên đang hoạt động và đang có quyền `supervision.complete`. Từ chối phân công cho tài khoản khách hàng, tài khoản đang bị khóa hoặc nhân viên thiếu quyền này. Điều kiện này áp dụng cả khi chuyển giao.
3. Đợt này không có phân công ở mức khách hàng hay mức công trình. Nhân viên chỉ thao tác được trên gói giám sát được phân công cho chính mình. Gói mới gắn vào một công trình phải được phân công riêng; người phụ trách gói trước đó trên cùng công trình không tự phụ trách gói mới.
4. Mỗi gói tại một thời điểm chỉ có một phân công đang hiệu lực. Yêu cầu giao một gói đang có người phụ trách cho người khác bị từ chối và hướng sang chuyển giao; không tạo phân công thứ hai.
5. Chuyển giao là kết thúc hiệu lực phân công cũ và tạo phân công mới cho người nhận tại cùng thời điểm. Người bàn giao mất quyền thao tác trên tài nguyên đó kể từ lúc đó.
6. Gỡ phân công mà không chuyển giao thì gói chưa có người phụ trách. Danh sách cần chia lại chỉ gồm gói đang ở trạng thái đã gán mà chưa có người phụ trách, hoặc người phụ trách đang bị khóa theo [BR-RBAC-008](BR-RBAC-008.md). Gói chưa gán, đã hoàn thành hoặc đã hủy không vào danh sách này. Gói được khách gán lại sau khi bị gỡ theo [BR-SUB-026](BR-SUB-026.md) chưa có người phụ trách nên vào danh sách này.
7. Mọi thay đổi phân công đều ghi nhật ký theo [BR-RBAC-012](BR-RBAC-012.md).
8. Chỉ giao mới hoặc chuyển giao được gói đang giữ chỗ trên công trình, tức gói đã gán hoặc đã hoàn thành theo [BR-SUB-006](BR-SUB-006.md). Từ chối giao hoặc chuyển giao gói chưa gán công trình hoặc đã hủy. Gỡ phân công được với gói ở mọi trạng thái.
9. Khi gói bị hủy theo [BR-SUB-024](BR-SUB-024.md) hoặc bị gỡ khỏi công trình theo [BR-SUB-026](BR-SUB-026.md), phân công đang hiệu lực của gói kết thúc ngay tại thời điểm đó và được ghi nhật ký theo khoản 7. Hệ thống tự kết thúc phân công; người quản trị không phải gỡ tay. Khi khách gán lại gói đã gỡ, gói chưa có người phụ trách và cần được giao lại.

## Except

Admin thao tác được trên gói giám sát mà không cần phân công, theo [BR-SUB-011](BR-SUB-011.md) và [BR-SUB-012](BR-SUB-012.md).

## Notes

- Khoản 3 và khoản 4 được người dùng xác nhận ngày 25/09/2026. Bản trước cho phân công mức khách hàng có hiệu lực xuống dự án và cho nhiều người cùng phụ trách một tài nguyên; cả hai nội dung này đã bỏ.
- Cùng ngày 25/09/2026, người dùng đổi đối tượng phân công từ công trình sang từng gói giám sát. Khoản 3, 4, 6, 8 và 9 viết theo quyết định này. Ví dụ: gói G1 của công trình P do A phụ trách. G1 bị hủy thì phân công của A trên G1 kết thúc. Khách gắn gói mới G2 vào P thì G2 chưa có người phụ trách, nằm trong danh sách cần chia lại, và người quản trị giao G2 cho A hoặc người khác.
- Người dùng xác nhận ngày 25/09/2026, khi bàn gỡ gói và bỏ khôi phục: khoản 9 đổi từ “giữ phân công khi hủy, người cũ phụ trách tiếp khi khôi phục” sang “kết thúc phân công khi hủy hoặc gỡ”. Không còn thao tác khôi phục theo [BR-SUB-025](BR-SUB-025.md).
- Gói gắn với một công trình theo [BR-SUB-009](BR-SUB-009.md) cho tới khi bị gỡ theo [BR-SUB-026](BR-SUB-026.md), nên người phụ trách gói cũng là người lo công trình đó trong thời gian gói còn ở đó. Công trình được đặc tả tại [STORY-SITE-001](../userstory/STORY-SITE-001.md).
- Cơ chế này được thiết kế để tính năng chia lead sau này dùng lại: lead là một loại tài nguyên mới, không cần thêm bảng phân công riêng. Việc chia lead không thuộc phạm vi đợt này.
- Phân công tham chiếu tới gói giám sát theo định danh. TDD-RBAC-003 cần cập nhật theo quyết định này.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
