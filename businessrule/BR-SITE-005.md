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

# BR-SITE-005

## Rule Info

- **Name**: Dùng lại danh mục dự toán và giữ cấu hình áp dụng của từng công trình.
- **Category**: Công trình
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Các quyết định người dùng đã xác nhận trong hội thoại mở rộng hồ sơ công trình ngày 01/10/2026; bổ sung cho STORY-SITE-001 và các Story liên quan.

## Statement

Công trình dùng chung danh mục loại công trình, số tầng, tum và hai nhóm phong cách với dự toán. Mỗi công trình giữ cấu hình áp dụng của mình; Admin đổi danh mục không tự đổi hồ sơ cũ.

## When

Khách chọn loại hoặc các đặc điểm công trình, tạo từ dự toán, mở lại hoặc sửa hồ sơ; Admin cập nhật danh mục dùng chung.

## Then

1. Không lập bộ danh mục loại công trình hoặc phong cách riêng cho SITE. Tham chiếu cùng danh tính loại và phong cách của danh mục PROJ để phục vụ truy vấn liên quan về sau; tên hiển thị không thay thế định danh.
2. Công trình độc lập dùng cấu hình danh mục tại lúc tạo. Công trình lấy nguồn dùng đúng cấu hình của dự toán nguồn, kể cả khi khác cấu hình hiện hành.
3. Mỗi công trình chọn một loại; mỗi nhóm phong cách áp dụng chọn đúng một giá trị thuộc đúng nhóm và loại. Số tầng chọn trong danh sách của loại. Có tum là Có hoặc Không khi trường được bật; Không tum khác với Không áp dụng.
4. Trường số tầng hoặc nhóm phong cách bị tắt hoặc không có lựa chọn theo cấu hình của loại thì không cho chọn, hiển thị Không áp dụng và không bắt nhập. Tum bị tắt thì Không áp dụng. Không dùng 0 tầng, một phong cách tự sinh hoặc Không tum để thay giá trị Không áp dụng.
5. Nếu trường được bật và có lựa chọn thì phải chọn hợp lệ trước khi tạo/lưu. Không được bỏ trống trường áp dụng bằng cách tự gửi giá trị Không áp dụng.
6. Danh mục Admin thay đổi không làm mất lựa chọn, đổi nhãn hoặc đổi điều kiện nhập của hồ sơ đã lưu. Khi xem hoặc sửa trường được phép, tiếp tục dùng cấu hình của hồ sơ; không tự chuyển sang bản danh mục mới nhất.
7. Khi đổi loại trên công trình độc lập được phép sửa, chỉ lưu bộ số tầng/tum/phong cách hợp lệ với loại mới trong cấu hình của công trình; phần không áp dụng phải được bỏ, phần bắt buộc phải chọn đủ. Công trình lấy nguồn không được đổi loại hoặc các trường nguồn theo BR-SITE-004.
8. Nếu không có loại nào trong cấu hình áp dụng để tạo công trình độc lập, không cho tạo bằng loại tự nhập. Dữ liệu nguồn đã hoàn tất dùng cấu hình được giữ của nguồn, không bị chuyển sang cấu hình mới để lấp thiếu.

## Except

Không áp dụng là ngoại lệ về tính bắt buộc cho trường phụ thuộc loại, không phải ngoại lệ cho loại công trình hoặc các thông tin bắt buộc khác.

## Notes

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Tham chiếu [BR-PROJ-004](BR-PROJ-004.md) cho cách Admin quản lý danh mục; [BR-SITE-001](BR-SITE-001.md), [BR-SITE-004](BR-SITE-004.md) cho hồ sơ.
- Ví dụ người dùng đã xác nhận: Nhà phố không có phong cách nội thất thì không cho chọn phong cách nội thất và xem là Không áp dụng.
- Không sửa quy tắc lưu cấu hình PROJ: BR-PROJ-004/Then khoản 13 vẫn từ chối lưu nhóm được bật nhưng danh sách rỗng. Quy tắc SITE về Không áp dụng không cho Admin bỏ qua kiểm tra đó.
- Dự toán dùng FloorCount=1 cho Trệt, 3 cho Trệt + 2 lầu; tum tách riêng. Cách hiển thị công trình dùng lại cùng nghĩa, không coi 3 là Trệt + 3 lầu.
- Nội dung nghiệp vụ đã được xác nhận qua hội thoại; bản US/BR cụ thể đã được người dùng chốt ngày 01/10/2026; đặc tả System Test đã được cập nhật. Chưa triển khai phần mở rộng hoặc chạy kiểm thử cho phần này.
- Reviewer/Approver giữ theo bộ tài liệu SITE hiện có; không phải bằng chứng phê duyệt. Owner và ngày hiệu lực chưa xác định.
