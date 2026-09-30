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

# BR-CTR-006

## Rule Info

- **Name**: Tọa độ công ty, tọa độ công trình và tìm nhà thầu theo bán kính.
- **Category**: Nhà thầu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng xác nhận trong phiên bàn luận tính năng nhà thầu; trang tham khảo https://vnz-bmt-savico-abcxyz.vercel.app/vi/contractors/preview/firm/ctr-cattrang. Quyết định trong hội thoại được ưu tiên khi khác trang mẫu.

## Statement

Công ty nhà thầu lưu kinh độ và vĩ độ; công trình của khách phải có kinh độ và vĩ độ ngay khi tạo. Query bán kính tìm nhà thầu theo khoảng cách đường thẳng từ tọa độ công trình khách chọn tới tọa độ công ty, với đơn vị km.

## When

Admin quản lý vị trí công ty; khách tạo công trình, đổi địa chỉ công trình hoặc lấy danh sách nhà thầu có query bán kính.

## Then

1. Lưu địa chỉ, kinh độ và vĩ độ công ty trong hồ sơ nhà thầu. Tọa độ dùng để hiển thị trên bản đồ và tính khoảng cách tới công trình.
2. Khi khách nhập địa chỉ để tạo công trình, frontend phải xác định kinh độ và vĩ độ, rồi gửi cả hai giá trị xuống backend cùng yêu cầu tạo. Backend từ chối tạo công trình nếu thiếu một trong hai trường; không chờ tới lúc tìm nhà thầu mới bổ sung tọa độ.
3. Khi có query bán kính, khách phải đăng nhập và chọn công trình thuộc tài khoản của mình làm tâm tìm kiếm.
4. Đơn vị bán kính và khoảng cách là km. Khoảng cách là đường thẳng giữa hai tọa độ, không phải quãng đường di chuyển.
5. Chỉ trả các nhà thầu đang hiển thị nằm trong bán kính yêu cầu và đáp ứng các query danh mục nếu có.
6. Khi khách đổi địa chỉ công trình, frontend xác định lại kinh độ và vĩ độ rồi gửi cùng địa chỉ mới. Backend kiểm tra quyền sửa hiện có và chỉ lưu địa chỉ cùng đủ hai tọa độ; nếu thiếu một trong hai thì từ chối yêu cầu, giữ nguyên dữ liệu cũ, theo BR-SITE-001 và BR-SITE-002.

## Except

Bán kính không hợp lệ bị từ chối theo TDD-CTR-002 đã được người dùng chốt.

## Notes

- Người dùng xác nhận chưa có công trình thật cần giữ, hoặc chỉ có dữ liệu thử. Kế hoạch kiểm preflight, chuẩn bị fixture và đặt tọa độ NOT NULL nằm trong TDD-SITE-002; chưa chạy migration.

- Đối chiếu code hiện có: bmt-be/src/bmt-be.domain/entities/ConstructionSite.cs và contract/services/constructionSite/Command.cs mới có tên, địa chỉ, chưa có tọa độ. Đây là yêu cầu thay đổi hiện trạng, chưa phải chức năng đã triển khai.
- STORY-SITE-001 và BR-SITE-001 đã bổ sung yêu cầu frontend gửi tọa độ và backend kiểm tra bắt buộc khi tạo công trình hoặc đổi địa chỉ. Thiết kế, API và kiểm thử hiện có vẫn cần rà soát và cập nhật; chưa triển khai thay đổi này.
- Phạm vi đã chốt dùng tọa độ công ty. Hỗ trợ nhiều chi nhánh chưa được yêu cầu; không coi việc thiết kế chi nhánh là điều kiện để hoàn tất nghiệp vụ hiện tại.
- Frontend chịu trách nhiệm xác định tọa độ từ địa chỉ khách nhập; chưa chọn dịch vụ xác định tọa độ cụ thể. TDD-CTR-002 đã chốt bán kính hữu hạn lớn hơn 0, không đặt trần mới, bao gồm điểm đúng đường biên và giữ thứ tự CreatedAtUtc DESC, Id DESC khi có bán kính.
- Người dùng đã chốt bộ US/BR trong hội thoại. Reviewer và Approver: Tân Trần. System Test được soạn theo phần nghiệp vụ đã chốt; chưa chạy kiểm thử hoặc triển khai code. Các điểm còn mở trong tài liệu không tự trở thành quy tắc đã xác nhận. Metadata người phụ trách và ngày hiệu lực còn thiếu; chưa thực hiện phê duyệt trên hệ thống quản lý tài liệu.
