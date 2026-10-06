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

# BR-SITE-003

## Rule Info

- **Name**: Khách hàng xem công trình của mình; nhân viên xem công trình theo quyền phân công hoặc theo gói đang phụ trách.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại chuẩn bị nghiệp vụ Công trình ngày 25/09/2026: nhân viên xem theo quyền đang có, không thêm mã quyền; khách thấy gói kèm trạng thái, chưa thấy nhân viên phụ trách; nhân viên phụ trách theo từng gói.

## Statement

Khách hàng chỉ xem công trình của chính mình. Admin và nhân viên có quyền `assignment.manage` xem mọi công trình. Nhân viên có quyền `supervision.complete` chỉ xem công trình của những gói giám sát đang được phân công cho mình. Các tài khoản khác không xem được công trình.

## When

Có yêu cầu xem danh sách, chi tiết công trình hoặc xem/tải tệp đính kèm công trình.

## Then

1. Khách hàng chỉ thấy công trình thuộc tài khoản của mình. Mỗi công trình hiện tên, địa chỉ và các gói giám sát đã gắn, kèm trạng thái gói: đã gán, đã hoàn thành hoặc đã hủy. Gói đã bị gỡ khỏi công trình không còn hiện ở công trình đó. Khách không thấy tên nhân viên phụ trách gói.
2. Admin và nhân viên có quyền `assignment.manage` xem mọi công trình của mọi khách hàng. Mỗi công trình hiện tên, địa chỉ, khách hàng sở hữu và các gói giám sát đã gắn, kèm trạng thái gói.
3. Nhân viên có quyền `supervision.complete` mà không có `assignment.manage` chỉ xem công trình của những gói giám sát đang được phân công cho mình tại thời điểm xem, với cùng thông tin như khoản 2. Khi phân công kết thúc hoặc được chuyển giao, nhân viên mất quyền xem công trình đó ngay. Gỡ gói hoặc hủy gói đều kết thúc phân công theo [BR-RBAC-013](BR-RBAC-013.md).
4. Nhân viên có nhiều quyền thì xem theo phạm vi rộng nhất trong các quyền đó.
5. Tài khoản nhân viên không có quyền `assignment.manage` hay `supervision.complete` thì không xem được danh sách hoặc chi tiết công trình.
6. Các quyền trong quy tắc này chỉ cho xem. Không quyền nào cho nhân viên tạo, sửa hoặc xóa công trình; xem [BR-SITE-002](BR-SITE-002.md).
7. Yêu cầu ngoài phạm vi bị từ chối và không tiết lộ tên, địa chỉ hay gói của công trình, theo [BR-RBAC-011](BR-RBAC-011.md).

8. Trong chi tiết, trả đầy đủ thông tin hồ sơ theo BR-SITE-001, lựa chọn/Không áp dụng theo BR-SITE-005, thông tin có hoặc không có dự toán nguồn và danh sách tệp. Không tự cấp thêm quyền đọc kết quả dự toán cho nhân viên chỉ vì công trình có nguồn; mở dự toán vẫn theo quyền hiện có của module dự toán.
9. Quyền xem/tải tệp đi theo đúng phạm vi xem công trình tại thời điểm yêu cầu theo BR-SITE-007. Người có URL nhưng không có quyền vẫn bị từ chối. Quyền xem không cấp quyền thêm, thay hoặc xóa tệp.
10. Mất phạm vi xem do kết thúc/chuyển giao phân công cũng làm mất quyền gửi yêu cầu xem/tải tệp mới. Không chỉ ẩn danh sách tệp trong giao diện mà để đường tải tiếp tục công khai.

11. Lựa chọn công trình để tìm nhà thầu được lưu trong database theo tài khoản khách đang đăng nhập. Chỉ chọn công trình thuộc mình và đã được lưu hợp lệ; bản nhập liệu trên thiết bị chưa thành công trình không được lưu thành lựa chọn. Không lưu lựa chọn trong localStorage.
12. Mỗi khách có tối đa một lựa chọn. Chọn lại cùng công trình không tạo bản ghi trùng. Khi nhiều thiết bị cùng đổi lựa chọn, dữ liệu từ giao dịch ghi thành công sau cùng là lựa chọn được đọc ở lần tiếp theo. Lỗi lưu giữ lựa chọn đã lưu trước đó; frontend báo lỗi và chưa chuyển trang từ popup.
13. Xóa hợp lệ công trình đang chọn đồng thời xóa lựa chọn đó. Khi chưa có lựa chọn, API trả không có lựa chọn; mở danh sách để đọc không tự ghi lựa chọn mặc định.

## Except

Nhân viên có quyền `commerce.read` thấy tên công trình gắn với gói khi tra cứu gói đã mua theo [BR-PAY-005](BR-PAY-005.md), nhưng không xem được danh sách hoặc chi tiết công trình của khách bằng quyền này.

## Notes

- Bổ sung ngày 06/10/2026 theo yêu cầu người dùng trong hội thoại sửa popup chọn dự án: lưu trạng thái dự án đang chọn trong database, không dùng localStorage. Các khoản 11–13 mô tả yêu cầu này và cách thực thi quyền sở hữu hiện có, không đổi vòng đời công trình.

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Không thêm mã quyền riêng để xem công trình. Danh mục quyền có 14 mã: ngày 25/09/2026 bỏ `supervision.reassign` và `package.restore`, thêm `supervision.unassign`; ngày 26/09/2026 thêm `payment.connection.manage` theo BR-PAY-006. Trên `develop` của `bmt-be` tại `79faf34`, code đã có 13 trong 14 mã (đã có `news.manage`); `library.manage` có ở nhánh `feature/library-admin` (commit `66e4671`), chưa merge.
- Nhân viên được phân công theo từng gói giám sát, không theo công trình; xem [BR-RBAC-013](BR-RBAC-013.md). Người phụ trách gói xem được công trình đang gắn với gói đó; khi gói bị gỡ hoặc bị hủy, phân công kết thúc và quyền xem cũng hết.
- Khoản 3 là phạm vi xem đi theo phân công. [BR-RBAC-010](BR-RBAC-010.md) khoản 1 cho quyền xem không đòi phân công; `supervision.complete` là quyền thao tác, không phải quyền xem, nên phạm vi đọc công trình của người giữ quyền này giới hạn theo gói được phân công.
- Ví dụ: N phụ trách G1 gắn công trình A; G3 gắn B do N2 phụ trách. N thấy A, không thấy B. Nếu G1 được chuyển giao cho N2 lúc 10:00 thì từ 10:00 N không xem được A.
- Bản nháp nghiệp vụ, chưa triển khai hoặc chạy kiểm thử. Reviewer và Approver lấy theo xác nhận đang dùng cho các bản nháp mới, không phải bằng chứng đã phê duyệt. Owner và ngày hiệu lực chưa xác định.

- Bổ sung ngày 01/10/2026: hồ sơ mở rộng và tệp riêng tư theo [BR-SITE-007](BR-SITE-007.md); chưa triển khai phần mở rộng hoặc chạy kiểm thử.
