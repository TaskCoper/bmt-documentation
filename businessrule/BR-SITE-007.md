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

# BR-SITE-007

## Rule Info

- **Name**: Tệp công trình tùy chọn, tối đa 9 tệp và 10 MB mỗi tệp; truy cập theo quyền công trình.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng đã xác nhận trong hội thoại mở rộng hồ sơ công trình ngày 01/10/2026; bổ sung cho STORY-SITE-001 và các Story liên quan.

## Statement

Công trình có thể có bản vẽ và ảnh hiện trạng tải lên riêng. Tổng tối đa 9 tệp, mỗi tệp tối đa 10 MB. Tệp được bảo vệ theo quyền xem công trình; có đường dẫn không đồng nghĩa có quyền mở hoặc tải.

## When

Khách thêm, thay, xóa tệp; một người xem hoặc tải tệp; hệ thống kiểm tra tệp trước khi gắn vào công trình.

## Then

1. Có hai nhóm: Bản vẽ và Ảnh hiện trạng. Không bắt buộc có tệp hoặc phải đủ cả hai nhóm.
2. Ảnh hiện trạng nhận JPG/JPEG, PNG, WebP. Bản vẽ nhận PDF, DWG, DXF, JPG/JPEG, PNG, WebP; JPG và JPEG là hai phần mở rộng của cùng định dạng JPEG.
3. Tối đa 9 tệp đang gắn trên một công trình, tính chung hai nhóm. Không đặt thành 9 bản vẽ cộng 9 ảnh. Từ chối thao tác làm tổng vượt 9, kể cả các yêu cầu đồng thời.
4. Mỗi tệp tối đa 10 MB; quy đổi MB thập phân là 10.000.000 byte. Tệp đúng giới hạn được nhận, vượt giới hạn bị từ chối. Kiểm dung lượng thực và định dạng trước khi gắn; không chỉ tin phần mở rộng hoặc số byte client khai báo.
5. Chỉ khách sở hữu được thêm, thay hoặc xóa tệp của công trình và chỉ khi không có gói giữ chỗ theo BR-SITE-002. Nhân viên kể cả Admin chỉ xem/tải trong phạm vi được phép, không quản lý tệp thay khách.
6. Gói đã gán hoặc đã hoàn thành khóa thêm, thay, xóa tệp. Gói đã gỡ hoặc đã hủy không khóa nếu không còn gói giữ chỗ. Kiểm điều kiện tại lúc lưu liên kết tệp, không chỉ lúc bắt đầu upload.
7. Chủ công trình và nhân viên đang có quyền xem công trình theo BR-SITE-003 được xem/tải các tệp của công trình. Người khác, kể cả người giữ đường dẫn, bị từ chối; tệp và bản xem trước không dùng URL công khai để bỏ qua quyền.
8. Khi phân công kết thúc hoặc chuyển giao, yêu cầu xem/tải mới của nhân viên chỉ có quyền theo phân công phải được kiểm lại và từ chối nếu đã mất quyền. Không mở một đường tải công khai chỉ vì nhân viên từng xem được tệp.
9. Không cho gắn tệp của công trình hoặc khách khác bằng cách thay mã tệp/đường dẫn trong API. Quyền upload không tự cấp quyền ghi hồ sơ hoặc quyền đọc công trình khác.
10. Khi thay tệp, chỉ thay liên kết cũ sau khi tệp mới hợp lệ và lưu thành công. Upload hoặc lưu thất bại giữ tệp cũ; không ghi nhận tệp chưa hoàn tất như đã đính kèm.
11. Tệp mới của form tạo công trình chỉ trở thành tệp đính kèm khi hồ sơ và liên kết được lưu thành công. Tạo công trình thất bại không chiếm dự toán nguồn hoặc tạo hồ sơ một phần; cơ chế quản lý tệp tạm thuộc TDD.
12. Bản vẽ kết quả và ảnh đầu vào của dự toán nguồn không tự được sao sang nhóm tệp công trình. Liên kết mở kết quả nguồn theo BR-SITE-004 không chiếm một trong 9 tệp.
13. Sau khi tệp được gỡ hoặc công trình được xóa hợp lệ, không tiếp tục cấp quyền đọc mới qua công trình đó. Việc xóa vật lý trong kho và thời gian lưu là thiết kế vòng đời riêng; không tự mở rộng quy tắc dọn ảnh thành dọn mọi bản vẽ.

## Except

Tệp là tùy chọn, nên công trình không có tệp vẫn tạo được nếu đủ các thông tin bắt buộc khác. Không thể thu hồi bản tệp người có quyền đã tải về; không cam kết ngắt một luồng tải đã bắt đầu trước lúc mất quyền.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Tham chiếu [STORY-SITE-004](../userstory/STORY-SITE-004.md), [STORY-SITE-002](../userstory/STORY-SITE-002.md), [BR-SITE-002](BR-SITE-002.md), [BR-SITE-003](BR-SITE-003.md), [BR-MEDIA-001](BR-MEDIA-001.md) và [BR-MEDIA-002](BR-MEDIA-002.md).
- Quyết định 10 MB của công trình khác giới hạn 5 MiB của ảnh mặc định; không đổi giới hạn các purpose hiện có. TDD phải thống nhất byte, thông báo và kiểm tra trên cả frontend/backend/kho.
- Đây là ngoại lệ về truy cập so với ảnh công khai của MEDIA. Thiết kế phải chứng minh không có URL gốc hoặc bản xem trước công khai đi vòng quyền; cơ chế lưu trữ và truyền tệp chưa được chốt trong BR.
- Nội dung nghiệp vụ đã được xác nhận qua hội thoại; quy đổi 10 MB sang byte ở khoản 4 dùng định nghĩa MB thập phân để làm rõ biên kiểm tra, không dùng ngầm mức 10 MiB của ảnh nhà thầu. Bản US/BR cụ thể đã được người dùng chốt ngày 01/10/2026; đặc tả System Test đã được cập nhật. Chưa triển khai phần mở rộng hoặc chạy kiểm thử cho phần này.
- Reviewer/Approver giữ theo bộ tài liệu SITE hiện có; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa xác định.
