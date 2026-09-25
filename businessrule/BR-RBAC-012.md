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

# BR-RBAC-012

## Rule Info

- **Name**: Ghi nhật ký mọi thay đổi vai trò, quyền và phân công.
- **Category**: Quản lý người dùng và phân quyền
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định người dùng xác nhận trong hội thoại thiết kế RBAC ngày 20/09/2026: ghi nhật ký đầy đủ ai làm gì cho ai.

## Statement

Hệ thống ghi nhật ký mọi thay đổi về vai trò, quyền và phân công. Mỗi bản ghi cho biết ai thao tác, thao tác gì, trên đối tượng nào, vào lúc nào và nội dung thay đổi là gì. Nhật ký chỉ đọc, không sửa và không xóa.

## When

Hệ thống xử lý xong một trong các thao tác: tạo, sửa hoặc xóa vai trò; tạo tài khoản nhân viên, nhân viên đổi mật khẩu lần đầu, khóa, mở khóa hoặc buộc đăng xuất tài khoản; gán hoặc thu hồi vai trò; tạo, chuyển giao hoặc gỡ phân công.

## Then

1. Ghi một bản ghi gồm người thao tác, loại thao tác, đối tượng bị tác động, thời điểm và nội dung thay đổi trước và sau.
2. Ghi cả các yêu cầu bị từ chối vì rào chắn quyền, để tra được ai đã thử làm gì.
3. Giữ nguyên bản ghi khi vai trò, phân công hoặc tài khoản liên quan bị xóa sau này; nhật ký lưu tên và định danh tại thời điểm thao tác.
4. Không cho sửa hoặc xóa bản ghi nhật ký qua giao diện hay API.
5. Người có quyền `audit.read` tra cứu được nhật ký, lọc theo người thao tác, đối tượng hoặc khoảng thời gian. Quyền này không kéo theo quyền thay đổi vai trò hay phân công.

## Except

Quy tắc này không quy định nhật ký cho các nghiệp vụ khác. Lý do bắt buộc khi hủy hoặc gỡ gói vẫn theo [BR-SUB-024](BR-SUB-024.md) và [BR-SUB-026](BR-SUB-026.md); hai loại bản ghi này tồn tại song song, không thay thế nhau. Khoản khôi phục theo BR-SUB-025 đã bỏ ngày 25/09/2026.

## Notes

- Thao tác thu hồi vai trò và gỡ phân công không bắt nhập lý do trong đợt này; nếu sau này cần thì bổ sung, quy tắc ghi nhật ký vẫn giữ nguyên.
- Người dùng xác nhận ngày 25/09/2026: trong đợt này hệ thống giữ toàn bộ nhật ký, không tự xóa bản ghi nào. Thời hạn lưu sẽ được xem lại khi làm phần lưu trữ nhật ký dài hạn, hiện nằm ngoài phạm vi của STORY-RBAC-004.
- Người dùng xác nhận ngày 25/09/2026: yêu cầu phân công bị từ chối vì gói đã có người phụ trách, gói chưa gán công trình hoặc đã hủy, hoặc người nhận thiếu quyền `supervision.complete` là lỗi nghiệp vụ, không phải rào chắn quyền, nên không ghi nhật ký theo khoản 2.
- Người dùng xác nhận ngày 25/09/2026: các yêu cầu bị từ chối vì lỗi nghiệp vụ dưới đây cũng không ghi nhật ký theo khoản 2:
  - tạo hoặc đổi tên vai trò trùng tên một vai trò đang có;
  - xóa vai trò còn người giữ;
  - tạo tài khoản nhân viên bằng email đã thuộc tài khoản khác;
  - gán vai trò Khách hàng cho nhân viên;
  - thu hồi vai trò, hoặc bỏ quyền khỏi vai trò, khiến người đang phụ trách gói giám sát mất quyền `supervision.complete`.
- Vẫn ghi nhật ký yêu cầu bị từ chối trong ba trường hợp: bị chặn bởi một trong ba rào chắn của [BR-RBAC-004](BR-RBAC-004.md), sửa hoặc xóa vai trò hệ thống theo [BR-RBAC-002](BR-RBAC-002.md), và tự khóa tài khoản của chính mình theo [BR-RBAC-008](BR-RBAC-008.md) khoản 7.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
