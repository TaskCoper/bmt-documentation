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

# BR-CMS-001

## Rule Info

- **Name**: Bảng so sánh gói có nhóm và hạng mục CMS động, độc lập với quyền thuê bao
- **Category**: CMS bảng giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Yêu cầu và ba câu trả lời của người dùng trong hội thoại ngày 09/10/2026: thêm/xóa/sắp xếp nhóm và hạng mục; mỗi ô chọn Có, Không hoặc nhập chữ; hai hàng số lượt nhập riêng trong CMS và không đổi hạn mức thật. Cơ chế CMS hiện có đối chiếu TDD-HB-003; phạm vi quyền thuê bao đối chiếu BR-SUB-008 khoản 12.

## Statement

Người quản lý được thêm, xóa và sắp xếp nhóm và hạng mục của bảng so sánh BASIC/PLUS/PRO. Mỗi ô là nội dung trình bày của CMS, độc lập với cấu hình quyền sử dụng và hạn mức thuê bao.

## When

Người có quyền quản lý nội dung sửa phần Bảng so sánh chi tiết trong Nội dung → Gói tư vấn → tab Gói thiết kế; hoặc khách mở bảng so sánh trên `/plans`.

## Then

1. Nhóm và hạng mục có thể được thêm, sửa tên, xóa và đổi thứ tự từ quản trị. Không giới hạn cấu trúc vào các nhóm hoặc hàng viết cố định trong mã hiện tại.
2. Mỗi hạng mục thuộc một nhóm và có tên hàng cùng ba ô tương ứng BASIC, PLUS, PRO. Thứ tự nhóm và thứ tự hạng mục là thứ tự đã lưu trong CMS.
3. Mỗi ô có một trong ba cách nhập: Có hiển thị ✓; Không hiển thị –; Nhập chữ hiển thị chữ/số người quản lý nhập. Ba cột được nhập độc lập.
4. Hai hàng Số phương án thiết kế mới và Tra cứu thư viện mẫu được sửa từng ô như các hạng mục CMS khác. Nội dung này không ghi vào hạn mức thật của gói; sau khi đã lưu ô CMS, thay hạn mức thật không tự ghi đè ô đó.
5. Thêm hoặc đánh dấu Có cho một hạng mục CMS không tạo định nghĩa quyền lợi, cấp quyền, giữ/trừ/giải phóng lượt hoặc khởi động dịch vụ. Xóa hoặc đánh dấu Không cũng không thu hồi quyền thật của khách.
6. Xóa nhóm đồng thời bỏ các hạng mục thuộc nhóm khỏi bảng CMS; xóa hạng mục chỉ bỏ hạng mục đó. Các thao tác không sửa hoặc xóa gói thuê bao và định nghĩa quyền lợi.
7. Thay đổi chỉ áp dụng trên trang công khai sau khi lưu CMS thành công. Nội dung theo từng ngôn ngữ, quyền `news.manage`, kiểm phiên bản và khôi phục mặc định tiếp tục dùng cơ chế CMS hiện có.
8. Tên nhóm, tên hàng và chữ trong ô hiển thị như chữ thuần. Dùng giới hạn chữ hiện có của CMS: tối đa 1.000 ký tự sau khi bỏ khoảng trắng đầu/cuối; vượt giới hạn thì từ chối, không tự cắt.
9. Phần tiêu đề bảng, tên cột hạng mục và chữ trên các nút chọn gói tiếp tục sửa được như hiện tại. Thao tác chọn gói vẫn dùng gói thật; màu cột nổi bật tiếp tục theo cờ Nổi bật của gói đã công bố.

10. Khi chưa có bản bảng động, vẫn đọc được nội dung CMS cũ. Quản trị lấy bảng đang hiển thị làm điểm bắt đầu; lần lưu bảng động tạo nội dung CMS độc lập và giữ các chỉnh sửa chữ hiện có. Không tự ghi vào production khi triển khai.
11. Cho phép lưu không có nhóm; bảng công khai giữ tiêu đề và hàng nút chọn gói, không tự thêm lại các hàng mặc định. Nhóm không có hạng mục chỉ hiện trong quản trị. Khôi phục mặc định là thao tác riêng.

## Except

Phạm vi chỉ là nội dung bảng so sánh. Cấu hình giá, quyền và hạn mức thật vẫn được quản lý ở màn gói và áp dụng theo các quy tắc thuê bao hiện có. BR-SUB-008 khoản 12 hiện chỉ xác định hai quyền có logic kiểm tra và tính lượt: tạo thiết kế/dự toán và tra cứu chi tiết mẫu. Các cấu hình khác để lưu và hiển thị không phải căn cứ tự bổ sung logic quyền trong thay đổi CMS này.

## Notes

**Đã xác nhận:** Ba lựa chọn của người dùng ở Source là căn cứ của khoản 1–5. Bảng quản trị cần giữ dạng bảng để sửa dễ; cấu trúc mới áp dụng cả nhóm và hạng mục hiện có.

**Hiện trạng đã kiểm tra:** `plans/comparison` hiện lưu các trường chữ phẳng. Tên nhóm/hàng có thể sửa nhưng không thêm/xóa/sắp xếp; hai hàng số lượt cùng nhiều ô khác đang lấy giá trị từ cấu hình gói. TDD-HB-003 mô tả hiện trạng này. Khoản 4 của bản nháp thay cách lấy giá trị của bảng; không thay BR-SUB-008.

**Đã chốt — giữ nội dung cũ:** Khi chưa có bản bảng động, hệ thống vẫn đọc được cấu trúc CMS cũ. Khi biên tập lần đầu, quản trị lấy nội dung đang hiển thị làm điểm bắt đầu; lưu thành công tạo bản CMS độc lập cho các ô. Không tự xóa các câu chữ người quản lý đã sửa trước đó và không tự ghi vào dữ liệu production trong lúc triển khai.

**Đã chốt — bảng rỗng:** Cho phép lưu danh sách không có nhóm để bỏ các hàng so sánh; không tự phục hồi những hàng đã xóa bằng bản mặc định. Giữ tiêu đề và hàng nút chọn gói. Nhóm chưa có hạng mục không xuất hiện trên trang công khai, nhưng vẫn có trong quản trị để thêm dòng. Khôi phục mặc định là thao tác riêng của người quản lý theo cơ chế CMS hiện có.

**Cần làm rõ về metadata:** Creator, Assignee, Owner, Reviewer, Approver, Sprint và ngày hiệu lực chưa được cung cấp cho tài liệu mới. Chưa ghi tên hoặc ngày thay người dùng; bản nháp chưa đủ metadata để coi là tài liệu đã hoàn tất hoặc đã phê duyệt.

Người dùng trả lời “chốt” ngày 09/10/2026 sau lượt hỏi chốt cả US/BR và hai đề xuất. Đã xác nhận nội dung nghiệp vụ; metadata chưa đủ không phải bằng chứng tài liệu đã được phê duyệt trên hệ thống. System Test được soạn tiếp theo bộ nội dung đã chốt.
