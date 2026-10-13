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

# BR-PROJ-010

## Rule Info

- **Name**: Bốn chặng tạo kết quả: 2D trước, Dự toán và Phối cảnh 3D chạy cùng lúc, Hồ sơ sau cùng; chặng đã thành công không chạy lại.
- **Category**: Tạo dự toán — xử lý theo chặng
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt trong hội thoại ngày 10–11/10/2026 (chuyển luồng app demo `design-estimate`; chặng đã có kết quả không chạy lại) và ngày 13/10/2026 theo yêu cầu khách hàng giảm thời gian chờ (Dự toán và 3D chạy cùng lúc sau một lần bấm; Hồ sơ tự tạo khi vào bước cuối; thanh tiến trình 4 bước). Tóm tắt ở `discovery/estimate-pipeline-planning.md`, mục “Luồng và giao diện”.

## Statement

Một bản dự toán tạo kết quả qua bốn chặng: Bản vẽ 2D, Dự toán, Phối cảnh 3D và Hồ sơ. Khách bấm chạy 2D. Khi 2D thành công, khách bấm Đi tiếp một lần: hệ thống tiếp nhận cùng lúc Dự toán và Phối cảnh 3D, hoặc từ chối cả hai. Hồ sơ chỉ mở khi cả Dự toán và Phối cảnh 3D đã thành công, và được tạo tự động khi khách vào bước Hồ sơ. Chặng đã thành công giữ nguyên kết quả và không chạy lại được. Mỗi chặng có tối đa một lần chạy đang chờ hoặc đang chạy; chỉ Dự toán và Phối cảnh 3D được chạy cùng lúc với nhau.

## When

Chủ bản dự toán yêu cầu chạy 2D, bấm Đi tiếp hoặc Thử lại, vào bước Hồ sơ, hoặc mở bản dự toán để xem trạng thái các chặng; hệ thống tiếp nhận hoặc kết thúc một lần chạy.

## Then

1. Điều kiện mở từng chặng:
   - Bản vẽ 2D: đầu vào đủ điều kiện theo BR-PROJ-001, BR-PROJ-002, BR-PROJ-004 và ảnh lô đất đã soi đạt theo BR-PROJ-015; khách đủ quyền, gói và lượt theo BR-SUB-007.
   - Dự toán: chặng 2D đã thành công.
   - Phối cảnh 3D: chặng 2D đã thành công. Không cần chờ chặng Dự toán.
   - Hồ sơ: cả chặng Dự toán và chặng Phối cảnh 3D đã thành công.

   Yêu cầu chạy một chặng chưa đủ điều kiện bị từ chối kèm lý do; không giữ hay trừ lượt và không ghi nhận lần chạy.
2. Chạy Dự toán và Phối cảnh 3D bằng một lần bấm:
   - Khi 2D thành công, khách có một thao tác Đi tiếp. Thao tác này yêu cầu chạy mọi chặng chưa thành công trong hai chặng Dự toán và Phối cảnh 3D. Không có thao tác chỉ chạy riêng một trong hai chặng.
   - Hệ thống kiểm tra từng chặng được yêu cầu: điều kiện ở khoản 1, giới hạn số lần lỗi theo BR-PROJ-011, hàng chờ và tạm ngắt dịch vụ AI theo BR-PROJ-012. Chỉ cần một chặng không được tiếp nhận thì từ chối cả lần bấm, kèm lý do; không tạo lần chạy nào và không tính vào bộ đếm lỗi.
   - Khi được tiếp nhận, hai chặng chạy độc lập. Chặng nào xong trước thì khách xem được kết quả chặng đó trước.
3. Lỗi và thử lại ở hai chặng Dự toán và Phối cảnh 3D:
   - Một chặng lỗi không làm dừng hay làm lỗi chặng kia. Kết quả của chặng đã thành công xem được ngay.
   - Khách có một thao tác Thử lại. Thao tác này chạy lại mọi chặng đang lỗi trong hai chặng; mỗi chặng tính vào bộ đếm lỗi riêng theo BR-PROJ-011.
   - Chặng đã đạt giới hạn số lần lỗi không được chạy lại và không chặn chặng lỗi còn lại. Khi mọi chặng đang lỗi đều đã đạt giới hạn, thao tác Thử lại bị từ chối và khách được báo liên hệ hỗ trợ theo BR-PROJ-011 khoản 2.
   - Các chặng được chạy lại được tiếp nhận cùng nhau hoặc cùng bị từ chối theo cách kiểm tra ở khoản 2.

   Ví dụ: Dự toán thành công, Phối cảnh 3D lỗi. Khách xem dự toán ngay; bấm Thử lại thì chỉ Phối cảnh 3D chạy lại. Nếu cả hai lỗi và Dự toán đã hết số lần chạy lại, bấm Thử lại thì chỉ Phối cảnh 3D chạy lại.
4. Hồ sơ được tạo tự động khi khách vào bước Hồ sơ: khách bấm chuyển sang bước Hồ sơ, hoặc mở thẳng bước Hồ sơ từ danh sách hay thanh tiến trình. Hệ thống tiếp nhận lần chạy Hồ sơ khi chặng Hồ sơ đủ điều kiện ở khoản 1, chưa thành công, chưa có lần chạy Hồ sơ nào và không có lần chạy Hồ sơ đang chờ hoặc đang chạy. Đã có hồ sơ thành công thì chỉ hiển thị, không tạo lại. Sau khi lần chạy Hồ sơ lỗi, khách chạy lại theo BR-PROJ-011.
5. Ngoài việc tự tạo Hồ sơ ở khoản 4, hệ thống không tự chạy chặng nào. Khi 2D thành công, hệ thống không tự chạy Dự toán hay Phối cảnh 3D. Khách có thể dừng ở bất kỳ bước nào; kết quả của các chặng đã thành công vẫn được giữ và xem được.
6. Mỗi chặng của một bản dự toán có tối đa một lần chạy đang chờ hoặc đang chạy. Dự toán và Phối cảnh 3D được cùng chờ hoặc cùng chạy. Yêu cầu chạy một chặng đang có lần chạy chờ hoặc đang chạy bị từ chối, kể cả yêu cầu gửi lặp hay gửi từ thiết bị khác; yêu cầu bị từ chối không tạo thêm lần chạy.
7. Chặng đã thành công không được chạy lại; yêu cầu chạy lại bị từ chối và kết quả cũ giữ nguyên. Muốn phương án khác, khách tạo bản dự toán mới.
8. Chỉ chủ bản dự toán (tài khoản khách hàng theo BR-RBAC-005) được yêu cầu chạy chặng. Gói và lượt chỉ được kiểm tra ở chặng 2D theo BR-SUB-003; các chặng sau không cần gói còn hiệu lực hay còn lượt.
9. Mọi chặng dùng đầu vào đã chốt lúc tiếp nhận lần chạy 2D thành công cùng kết quả đã lưu của chặng 2D. Không đọc lại đầu vào, danh mục hay vùng giá mới hơn cho các chặng sau. Phối cảnh 3D không dùng kết quả của chặng Dự toán: kiểu mái trên ảnh 3D được xác định bằng đúng quy tắc mái mà chặng Dự toán dùng để tính tiền (ma trận mái của bộ quy tắc AI đã chốt theo BR-PROJ-019 và phương án 2D), nên khớp với kiểu mái đã tính tiền.
10. Khách xem được trạng thái của từng chặng: Chưa mở, Sẵn sàng, Đang chạy, Thành công hoặc Lỗi. Chặng Lỗi kèm lý do và số lần chạy lại còn lại theo BR-PROJ-011; chặng Chưa mở kèm điều kiện còn thiếu. Thanh tiến trình có 4 bước: Nhập liệu → Bản vẽ 2D → Dự toán & 3D → Hồ sơ. Bước Dự toán & 3D hiển thị trạng thái riêng của từng chặng.
11. Chặng chỉ thành công khi toàn bộ kết quả bắt buộc của chặng đã được tạo, kiểm tra và lưu, và khách mở xem được. Không công bố một phần kết quả dở dang là kết quả thành công.

## Except

- Admin không chạy chặng thay khách. Admin chỉ được đặt lại bộ đếm lỗi theo BR-PROJ-011.
- Bản dự toán đã xóa theo BR-PROJ-009 không được chạy chặng nào và không tự tạo Hồ sơ.
- Luồng bốn chặng chỉ hoạt động khi cờ cấu hình của hệ thống đang bật. Khi cờ tắt, yêu cầu chạy chặng bị từ chối với lý do tính năng chưa sẵn sàng; không giữ hay trừ lượt.

## Notes

- **Đã xác nhận (13/10/2026), theo yêu cầu khách hàng giảm thời gian chờ:** sau 2D chỉ có một nút Đi tiếp chạy cả Dự toán và Phối cảnh 3D; nhận cả hai hoặc không nhận; một chặng lỗi thì khách vẫn xem chặng đã xong, một nút Thử lại chạy lại các chặng đang lỗi, chặng hết số lần chạy lại không chặn chặng còn lại; Hồ sơ chỉ mở khi cả hai xong và tự tạo khi khách vào bước Hồ sơ, kể cả mở thẳng bước này; thanh tiến trình 4 bước; kết quả Dự toán và 3D hiển thị trên một màn có hai tab (STORY-PROJ-003). Lượt, giới hạn lỗi và hạn chờ từng chặng không đổi.
- **Đã xác nhận (14/10/2026):** khi một chặng lỗi trong lúc chặng kia của cặp còn đang chạy, bấm Thử lại thì chặng lỗi chạy lại ngay; chặng đang chạy không bị ảnh hưởng (khoản 3). Ca kiểm thử ST-PROJ-226.
- **Thay đổi so với bản 11/10/2026:** bỏ quy định khách tự bấm từng chặng theo thứ tự cố định, bỏ điều kiện 3D cần Dự toán thành công và bỏ giới hạn tối đa một chặng chạy tại một thời điểm. Các quyết định khác của ngày 11/10/2026 giữ nguyên: chặng đã có kết quả không chạy lại, chặng lỗi chạy lại theo BR-PROJ-011, khách được dừng giữa chừng, gói hết hạn sau khi xong 2D vẫn chạy tiếp được.
- **Đã xác nhận (13/10/2026, lượt sau):** người dùng chốt US/BR và đồng ý: Hồ sơ lỗi thì khi vào lại bước Hồ sơ hệ thống không tự chạy lại, khách bấm Chạy lại (khoản 4); bản đã xong Dự toán nhưng chưa chạy 3D thì Đi tiếp chỉ chạy 3D (khoản 2). Đặc tả System Test: ST-PROJ-216 đến ST-PROJ-225 và các ca cũ đã sửa, xem `discovery/estimate-system-test-coverage.md`; chưa thực thi.
- Khoản 2 yêu cầu “mọi chặng chưa thành công” để bản dự toán đã xong Dự toán theo luồng cũ nhưng chưa chạy 3D vẫn đi tiếp được: bấm Đi tiếp chỉ chạy Phối cảnh 3D.
- Khoản 9: trước 13/10/2026, chặng 3D đọc kiểu mái từ kết quả Dự toán đã lưu. Hai cách cho cùng kiểu mái khi dùng cùng bộ quy tắc và cùng phương án 2D; cách tính cụ thể thuộc TDD-PROJ-009 và TDD-PROJ-010.
- Quy tắc này thay mô hình “một lần gửi AI trả toàn bộ bộ kết quả” trước đây của STORY-PROJ-002 và BR-SUB-003.
- Ví dụ: khách xong 2D rồi dừng. Hai tuần sau gói đã hết hạn, khách bấm Đi tiếp: yêu cầu được nhận vì gói chỉ được kiểm tra ở chặng 2D.
- Tên trạng thái và tên nút hiển thị trên giao diện có thể khác, miễn giữ đúng ý nghĩa của khoản 2, 3 và 10.
- Reviewer và Approver là Tân Trần theo các bản PROJ trước; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
