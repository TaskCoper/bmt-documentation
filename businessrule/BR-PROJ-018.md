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

# BR-PROJ-018

## Rule Info

- **Name**: Khi bật luồng bốn chặng, xóa hẳn các bản dự toán có kết quả giả `mock-v1`, không hoàn lượt.
- **Category**: Tạo dự toán — chuyển luồng
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: luồng mới bật bằng cờ cấu hình, production chỉ bật khi người dùng quyết định; các bản dự toán có kết quả AI giả `mock-v1` bị xóa hẳn khi chuyển luồng; lượt đã trừ cho các bản đó không được trả lại; bản nháp cũ phải bổ sung ảnh lô, số phòng ngủ… mới tạo được 2D. Người dùng chốt ngày 12/10/2026: nếu luồng mới gặp lỗi nghiêm trọng sau khi bật thì tắt luồng, giữ kết quả, không quay về luồng giả.

## Statement

Mỗi môi trường chuyển sang luồng bốn chặng khi cờ cấu hình được bật; production chỉ bật khi người dùng quyết định. Lúc chuyển, mọi bản dự toán có kết quả giả `mock-v1` bị xóa hẳn cùng dữ liệu đi kèm. Lượt đã trừ cho các bản này không được hoàn. Các bản nháp chưa có kết quả được giữ và đi theo luồng mới.

## When

Người vận hành chuyển một môi trường sang luồng bốn chặng.

## Then

1. Trước khi xóa, người vận hành chạy bước xem trước: đếm số bản dự toán, kết quả, tệp, link chia sẻ, yêu cầu xuất tệp và email sẽ bị xóa. Chỉ xóa sau khi đã xem số liệu này.
2. Xóa hẳn mọi bản dự toán có kết quả `mock-v1`, gồm đầu vào, kết quả, danh sách tệp, link chia sẻ, yêu cầu xuất tệp và yêu cầu email của các bản đó. Link và QR đã gửi không mở được nữa.
3. Không hoàn lượt đã dùng cho các bản bị xóa; lịch sử tính lượt giữ nguyên để đối soát.
4. Bản dự toán đang được xử lý bằng luồng cũ lúc chuyển: chờ kết thúc rồi xử lý theo khoản 2 nếu thành công, hoặc giữ lại như bản nháp nếu thất bại.
5. Bản nháp và bản thất bại chưa từng có kết quả được giữ. Khách phải bổ sung các trường mới bắt buộc (ảnh lô đất đạt soi, số phòng ngủ…) trước khi chạy 2D theo luồng mới.
6. Sau khi bật luồng mới, thao tác gửi AI kiểu cũ (một lần trả toàn bộ kết quả) bị từ chối; khách dùng các chặng theo BR-PROJ-010.
7. Khi cờ còn tắt, môi trường giữ nguyên luồng cũ; không xóa dữ liệu theo quy tắc này.
8. **Quay lui khi luồng mới gặp lỗi nghiêm trọng sau khi bật:** người vận hành tắt luồng bốn chặng. Trong lúc tắt:
   - yêu cầu chạy chặng mới bị từ chối với thông báo "Tạo dự toán đang tạm bảo trì"; không giữ lượt, không tính vào bộ đếm lỗi;
   - kết quả các chặng đã thành công vẫn xem, tải và chia sẻ được như bình thường;
   - lần chạy đã được tiếp nhận thì chạy nốt, hoặc lỗi do quá hạn theo BR-PROJ-012; chặng 2D lỗi thì hoàn lượt theo BR-SUB-016;
   - không quay về luồng giả `mock-v1` và không khôi phục các bản đã xóa ở khoản 2.

   Bật lại luồng không cần chuyển dữ liệu; khách chạy tiếp từ chặng đang dừng.

## Except

Quy tắc này chỉ áp dụng một lần cho mỗi môi trường lúc chuyển luồng. Sau đó, xóa bản dự toán theo BR-PROJ-009.

## Notes

- **Đã xác nhận (11/10/2026):** xóa hẳn bản có kết quả `mock-v1`; không hoàn lượt; production bật khi người dùng quyết định; bản nháp cũ phải bổ sung trường mới.
- **Đã xác nhận (11/10/2026):** chờ bản đang chạy luồng cũ kết thúc rồi mới xử lý (khoản 4).
- **Đã xác nhận (12/10/2026):** cách quay lui ở khoản 8; luồng giả `mock-v1` không được dùng lại sau khi đã chuyển.
- Khác với BR-PROJ-009 (đánh dấu đã xóa, giữ dữ liệu nội bộ), quy tắc này xóa vật lý dữ liệu bản dự toán; chỉ giữ lịch sử tính lượt.
- Hiện không có dữ liệu `mock-v1` được nạp sẵn; mọi bản có kết quả `mock-v1` đều do người dùng tạo khi hệ thống chạy adapter giả (production chạy adapter này theo nhánh `fix/production-estimate-mock`).
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
