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


# BR-CMS-003

## Rule Info

- **Name**: CMS nội dung gói giám sát, độc lập với cấu hình gói và vận hành thực tế.
- **Category**: Quản lý nội dung
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Yêu cầu ngày 09/10/2026 làm tương tự CMS Gói tư vấn cho sáu phần trong ảnh trang /plans/supervision; người dùng chọn quản lý động cả phụ phí, nguyên tắc và hành trình; chọn nhập chữ/số riêng từng ô CMS. STORY-CMS-001 và STORY-CMS-002 là mẫu hành vi hiện có.

## Statement

Người có quyền quản lý nội dung được biên tập sáu phần giới thiệu gói giám sát theo từng ngôn ngữ. Nhóm và hạng mục so sánh, hàng phụ phí, nguyên tắc, bước hành trình và hàng giá trị khách hàng được thêm, sửa, xóa và sắp xếp. Các thay đổi là nội dung trình bày, không cấu hình dịch vụ hoặc gói đã mua.

## When

Người có quyền `news.manage` mở Nội dung → Gói tư vấn → tab Gói giám sát trong admin; hoặc khách đọc các phần này trên `/plans/supervision`.

## Then

1. Phạm vi gồm Bảng so sánh chi tiết; Dịch vụ thêm và phụ phí; Nguyên tắc phạm vi dịch vụ; Hành trình giám sát; Giá trị khách hàng nhận được; ba ghi chú cuối trang. Mỗi phần cho đóng/mở, lưu và khôi phục như tab Gói thiết kế. Hai tab nằm trong một màn Gói tư vấn; chuyển tab giữ bản đang nhập và mỗi tab lưu vào đúng trang CMS của mình.
2. Bảng so sánh có nhóm và hạng mục động; mỗi hạng mục có tên cùng ba ô cho Tự quản lý/An tâm/Toàn diện. Mỗi ô chọn Có (✓), Không (–) hoặc nhập chữ/số riêng. Xóa nhóm đồng thời bỏ các hạng mục thuộc nhóm khỏi CMS.
3. Các hàng số lần kiểm tra, thời gian áp dụng và chi phí được sửa như các ô CMS khác. Sau khi lưu, các ô này không tự đổi theo cấu hình gói. Tên và giá ở đầu cột vẫn theo nguồn gói hiện có; lựa chọn tự quản lý tiếp tục theo cách lấy dữ liệu hiện tại của frontend.
4. Bảng phụ phí quản lý hàng động với tên hạng mục, giá đề xuất và ghi chú bằng chữ. Thêm hoặc sửa giá đề xuất không tạo sản phẩm, phụ phí đơn mua hay yêu cầu thanh toán thật.
5. Nguyên tắc phạm vi quản lý danh sách chữ động; hành trình quản lý danh sách bước động với tiêu đề, mô tả và icon. Số bước theo thứ tự danh sách, không nhập thủ công và không cố định bốn bước.
6. Giá trị khách hàng quản lý hàng động có tên, icon và chữ riêng cho ba cột; quản trị hiển thị dạng bảng. Icon chọn từ thư viện frontend đang dùng, có xem trước; những phần khác đang có icon cũng cho sửa icon.
7. Ba ghi chú cuối trang giữ ba vị trí hiện tại và cho sửa chữ/icon. Tiêu đề phần, tên cột hạng mục, tiêu đề cột phụ phí và chữ nút chọn gói là nội dung CMS. Tên gói ở đầu cột không nhập lại bằng CMS.
8. Cột Toàn diện tiếp tục có màu cam như giao diện hiện tại; các nhóm so sánh tiếp tục theo kiểu phân nhóm đang dùng. Không dùng CMS để thay cờ nổi bật của cấu hình gói.
9. Nút chọn gói giữ điều kiện mua hiện có: dùng đúng gói và giá từ nguồn gói, giữ công trình đang chọn; Tự quản lý không được tạo nút mua khi sửa chữ CMS.
10. Nội dung, thứ tự và icon được lưu riêng theo ngôn ngữ. Chưa lưu không đổi công khai; đóng section không làm mất bản đang nhập. Thiếu bản CMS dùng mặc định đúng ngôn ngữ; không mượn chữ từ ngôn ngữ khác.
11. Cho lưu danh sách động rỗng và không tự khôi phục các mục đã xóa. Giữ tiêu đề phần; bảng so sánh giữ đầu bảng và hàng chọn gói. Nhóm so sánh không có hạng mục chỉ hiện trong quản trị. Khôi phục mặc định là thao tác riêng của người quản lý.
12. Áp dụng kiểm dữ liệu, quyền `news.manage`, phiên bản, trạng thái mặc định/đã biên tập và cập nhật cache theo PageSection hiện có. Mở màn hình không tự ghi; chữ hiển thị như chữ thuần, không chạy HTML hoặc mã do người soạn nhập.
13. Thay CMS không thay giá bán, quyền sử dụng, vòng đời hoặc liên kết công trình của gói; không tạo, giữ, trừ hoặc hoàn lượt kiểm tra, không đặt lịch, phân công kỹ sư, hoàn thành gói hay cấp dịch vụ thật.

## Except

Icon trống, không tồn tại hoặc không tải được dữ liệu vẽ dùng icon dự phòng của frontend, không làm mất chữ đã nhập. Lỗi dữ liệu, quyền hoặc phiên bản giữ bản đã lưu và hiển thị lỗi theo CMS hiện có.

## Notes

**Đã xác nhận qua câu trả lời:** Quản lý động cả nhóm/hạng mục so sánh, giá trị khách hàng, phụ phí, nguyên tắc và hành trình. Các ô số của bảng so sánh nhập riêng bằng CMS như Gói tư vấn; tên/giá đầu cột giữ nguồn gói hiện có.

**Đã chốt bản tổng hợp ngày 09/10/2026:** Lưu theo ngôn ngữ, khôi phục riêng, dùng mặc định khi chưa biên tập, cho danh sách rỗng, giữ bản đang nhập khi đóng/mở, chọn icon có xem trước và dùng icon dự phòng. Ba ghi chú giữ số vị trí hiện tại theo phạm vi ảnh. Cột Toàn diện và nút mua giữ hành vi hiện có.

**Hiện trạng đã kiểm tra:** `supervision-pricing.tsx` dùng các nhóm/hàng và danh sách cố định. `supervision.merge.ts` ghép tên, giá và mã gói từ API cho các gói trả phí; lựa chọn Tự quản lý và một số nội dung trình bày đang từ dữ liệu nền của frontend. Phạm vi CMS này không đổi cơ chế cấu hình hoặc mua gói.

**Metadata còn thiếu:** Owner và ngày hiệu lực chưa được cung cấp. Chưa tự ghi trạng thái Active, nhập tài liệu hoặc phê duyệt trên hệ thống. Chỉ soạn System Test sau khi người dùng chốt cả STORY-CMS-003 và BR-CMS-003.

<!-- Người dùng chốt cả US và BR ngày 09/10/2026; xác nhận Reviewer và Approver đều là Tân Trần. Không tự xác nhận phê duyệt trên hệ thống. -->
