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

# BR-MEDIA-002

## Rule Info

- **Name**: Dọn ảnh mới và ảnh cũ không còn được sử dụng
- **Category**: Quản lý ảnh
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận trong hội thoại ngày 30/09/2026: dọn ảnh trong đợt này; chờ 24 giờ; xử lý cả ảnh cũ; ảnh cũ nằm trong bucket riêng của BMT.

## Statement

Hệ thống tự dọn ảnh không còn được sử dụng trong bucket riêng của BMT, gồm ảnh upload qua API mới và ảnh cũ. Ảnh upload nhưng bỏ form được dọn sau 24 giờ; ảnh đã bị thay hoặc gỡ được dọn sau 24 giờ kể từ khi không còn nơi nào sử dụng. Chỉ có bản ghi đăng ký ảnh cho mẫu thư viện không được coi là ảnh đang được sử dụng. Ảnh còn được bản nháp, nội dung đang ẩn hoặc phiên bản lịch sử sử dụng phải được giữ.

## When

- Hệ thống đối soát ảnh trong kho hoặc xét một ảnh đến hạn dọn.
- Ảnh vừa upload chưa được lưu vào nội dung nào.
- Một thao tác nghiệp vụ đã lưu thành công làm ảnh không còn được sử dụng, hoặc làm ảnh đang chờ dọn được dùng trở lại.

## Then

1. Đối soát cả ảnh mới lẫn ảnh cũ trong bucket riêng của BMT, kể cả ảnh không còn URL trong database.
2. Với ảnh upload mới nhưng không được dùng ở đâu, chỉ xét xóa khi đã qua 24 giờ tính từ khi upload thành công.
3. Với ảnh từng được dùng, chỉ xét xóa khi đã qua 24 giờ liên tục kể từ khi nơi sử dụng cuối cùng được gỡ thành công.
4. Kiểm tra mọi nơi còn sử dụng ảnh trước khi xóa. Một ảnh được dùng ở nhiều nơi chỉ đủ điều kiện dọn khi không còn nơi nào sử dụng; không chỉ kiểm tra màn hình hoặc nội dung vừa sửa.
5. Giữ ảnh còn được bản nháp, nội dung đang ẩn, phiên bản lịch sử hoặc bản chụp dữ liệu còn được lưu sử dụng.
6. Khi ảnh được dùng trở lại trong thời gian chờ, hủy lần chờ đó. Nếu sau này ảnh lại không còn được dùng, tính một lần chờ mới.
7. Chỉ ghi nhận đã xóa sau khi xác định kết quả ở BizFly. Nếu thao tác xóa lỗi hoặc chưa rõ kết quả, theo dõi để xử lý lại và kiểm tra lại nơi sử dụng trước lần thử tiếp theo.
8. Việc dọn diễn ra theo lượt xử lý; 24 giờ là thời gian tối thiểu phải chờ, không phải cam kết file sẽ bị xóa đúng thời điểm đủ 24 giờ.
9. Với ảnh cũ không biết thời điểm ngừng sử dụng, bắt đầu chờ 24 giờ từ lần đầu đối soát đầy đủ và xác định ảnh không còn được dùng. Không lấy ngày tạo file làm thời điểm ngừng sử dụng. Nếu ảnh được dùng trở lại thì hủy lần chờ theo khoản 6.
10. Ảnh đã đăng ký cho mẫu thư viện nhưng chưa gắn hoặc đã được gỡ khỏi mọi phiên bản cũng phải dọn sau 24 giờ nếu không còn nội dung nào khác sử dụng. Bản ghi đăng ký tài nguyên không tự tạo ngoại lệ giữ ảnh. Ảnh chưa từng được dùng áp dụng khoản 2; ảnh đã bị gỡ khỏi nơi sử dụng cuối cùng áp dụng khoản 3; ảnh cũ thiếu lịch sử áp dụng khoản 9. Sau khi ảnh bị dọn, muốn dùng lại phải upload lại.

## Except

- Chưa đối soát đầy đủ, chưa xác định được ảnh thuộc phạm vi BMT hoặc chưa đủ thông tin tính thời gian chờ: giữ ảnh, không tự coi là ảnh được phép xóa.
- File không phải ảnh nằm ngoài phạm vi dọn của tính năng này.
- File có khóa bắt đầu bằng `landing/` thuộc landing page dùng chung bucket: không đối soát, không dọn.
- Không xóa ảnh cũ chỉ vì ảnh không đáp ứng giới hạn định dạng hoặc dung lượng của luồng upload mới.

## Notes

- **Đã xác nhận:** có dọn ảnh trong đợt này; chờ 24 giờ; bao gồm ảnh cũ; bucket riêng của BMT.
- **Đã xác nhận — ảnh cũ thiếu lịch sử:** người dùng đồng ý chờ 24 giờ từ lần đầu xác định ảnh không còn được dùng; áp dụng khoản 9 của Then.
- **Đã xác nhận — tài nguyên thư viện:** người dùng chọn dọn sau 24 giờ đối với ảnh không còn phiên bản hay nội dung nào sử dụng, dù bản ghi đăng ký ảnh cho mẫu thư viện vẫn còn; áp dụng khoản 10 của Then.
- **Đã xác nhận — bucket dùng chung (06/10/2026):** người dùng chốt landing page dùng chung bucket với BMT và yêu cầu hệ thống bỏ qua thư mục `landing/`. Cụm "bucket riêng của BMT" ở trên được hiểu là các khóa ngoài `landing/`. Cách thực hiện nằm tại TDD-MEDIA-001, mục Architecture.
- Tên bucket, endpoint và cách ánh xạ URL cũ về file trong kho cần được kiểm chứng khi thiết kế và cấu hình. Việc xác nhận bucket riêng chưa thay thế thông tin cấu hình thật.
- Lịch chạy là cấu hình kỹ thuật tại TDD-MEDIA-001/Architecture; không làm thay đổi thời gian chờ tối thiểu 24 giờ.
- Reviewer và Approver là Tân Trần theo thông tin người dùng cung cấp. Owner là Tân Trần theo xác nhận bổ sung của người dùng; ngày hiệu lực chưa xác định. Người dùng đã chốt nội dung bộ US/BR trong hội thoại ngày 30/09/2026. Status vẫn là Draft vì chưa thực hiện phê duyệt trên hệ thống tài liệu.
