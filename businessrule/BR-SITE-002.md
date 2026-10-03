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

# BR-SITE-002

## Rule Info

- **Name**: Chỉ khách hàng sở hữu được tạo, sửa và xóa công trình; chỉ sửa hoặc xóa khi không có gói giữ chỗ; xóa còn cần chưa có lời mời báo giá.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại chuẩn bị nghiệp vụ Công trình ngày 25/09/2026: khách tự tạo công trình; sửa tên, địa chỉ bất cứ lúc nào; chỉ xóa khi chưa từng có gói giám sát gắn vào; nhân viên không tạo, sửa, xóa hộ khách. Cập nhật cùng ngày: khóa sửa và xóa khi công trình có gói giữ chỗ; gói đã gỡ hoặc đã hủy không khóa.

## Statement

**Cập nhật 25/09/2026:** Người dùng xác nhận khách không sửa hoặc xóa được công trình khi công trình có gói giám sát giữ chỗ, tức gói đã gán hoặc đã hoàn thành. Thay cho quy định cũ “sửa bất cứ lúc nào, chỉ xóa công trình chưa từng có gói”.

Công trình thuộc tài khoản khách hàng đã tạo ra nó. Chỉ khách hàng đó được sửa hoặc xóa công trình, và chỉ khi công trình không có gói giám sát giữ chỗ. Nếu đã có lời mời báo giá ở bất kỳ trạng thái nào, công trình không được xóa theo [BR-RFQ-003](BR-RFQ-003.md); điều kiện này không khóa sửa hồ sơ.

## When

Có yêu cầu tạo, sửa hoặc xóa công trình.

## Then

1. Chỉ tài khoản khách hàng được tạo công trình. Công trình mới thuộc chính tài khoản gửi yêu cầu; không tạo công trình cho tài khoản khác.
2. Chỉ khách hàng sở hữu được sửa hoặc xóa công trình. Từ chối yêu cầu của khách hàng khác và của mọi tài khoản nhân viên, kể cả Admin.
3. Khách sửa được các trường hồ sơ không bị khóa bởi dự toán nguồn theo BR-SITE-004 khi công trình không có gói giám sát giữ chỗ theo [BR-SUB-006](BR-SUB-006.md). Công trình đang có gói đã gán hoặc đã hoàn thành thì mọi yêu cầu sửa bị từ chối. Gói đã bị gỡ theo [BR-SUB-026](BR-SUB-026.md) hoặc đã hủy theo [BR-SUB-024](BR-SUB-024.md) không khóa việc sửa.
4. Khách xóa được công trình khi chưa có lời mời báo giá và không có gói giám sát giữ chỗ, kể cả công trình từng có gói nay đã bị gỡ hoặc đã hủy. Công trình đang có gói đã gán hoặc đã hoàn thành thì không xóa được. Xóa công trình không xóa lịch sử của các gói đã gỡ hoặc đã hủy; lịch sử vẫn giữ tên và địa chỉ công trình tại lúc gỡ hoặc hủy.
5. Công trình đã xóa không còn trong danh sách của khách và không nhận gói giám sát.
6. Yêu cầu bị từ chối không thay đổi công trình hay gói.

7. Các trường lấy từ dự toán nguồn vẫn bị khóa sau khi gói đã gỡ hoặc đã hủy; không dùng việc nhả chỗ của gói để đổi dữ liệu nguồn. Không có dự toán nguồn thì không được bổ sung sau khi tạo.
8. Quy tắc khóa do gói áp dụng cả thêm, thay, xóa tệp theo BR-SITE-007. Nhân viên, kể cả Admin, không được quản lý tệp thay khách.
9. Xóa công trình hợp lệ giải phóng dự toán nguồn cho lần tạo khác hoặc cho phép xóa dự toán theo BR-PROJ-009; không xóa dự toán cùng công trình. Không cấp quyền đọc tệp mới qua công trình đã xóa.

## Except

Không có ngoại lệ cho việc nhân viên tạo, sửa hoặc xóa công trình thay khách.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Không cần điều kiện riêng về nhân viên phụ trách khi sửa hoặc xóa: phân công theo từng gói giám sát và kết thúc khi gói bị gỡ hoặc bị hủy theo [BR-RBAC-013](BR-RBAC-013.md), nên công trình không có gói giữ chỗ thì cũng không còn người phụ trách.
- Khóa sửa khi có gói giữ chỗ để khách không đổi địa chỉ công trình nhằm dùng gói cho một nơi khác. Khách gõ sai địa chỉ thì liên hệ tổng đài để nhân viên gỡ gói theo [BR-SUB-026](BR-SUB-026.md), sau đó khách sửa các trường được phép rồi gán lại. Các trường lấy từ dự toán vẫn bị khóa theo BR-SITE-004.
- Gói đã gắn thì không đổi thẳng sang công trình khác, kể cả nhân viên; chỉ được gỡ theo BR-SUB-026. Gắn gói theo [BR-SUB-022](BR-SUB-022.md); giới hạn gói giữ chỗ trên một công trình theo [BR-SUB-006](BR-SUB-006.md). Khách không dùng thao tác xóa công trình để gỡ gói.
- Tài khoản nhân viên bị từ chối theo cách tách nhóm tài khoản ở [BR-RBAC-005](BR-RBAC-005.md). Yêu cầu của khách khác bị từ chối mà không tiết lộ thông tin công trình, theo [BR-RBAC-011](BR-RBAC-011.md) khoản 5.
- Dữ liệu hồ sơ theo [BR-SITE-001](BR-SITE-001.md); quyền xem theo [BR-SITE-003](BR-SITE-003.md).
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.
- Bổ sung ngày 01/10/2026: hồ sơ đầy đủ, khóa nguồn theo [BR-SITE-004](BR-SITE-004.md), tệp theo [BR-SITE-007](BR-SITE-007.md). Phần mở rộng chưa triển khai; US/BR đã được chốt và System Test đã cập nhật; TDD và Unit Test còn cần cập nhật.
