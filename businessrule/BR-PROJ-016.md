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

# BR-PROJ-016

## Rule Info

- **Name**: Admin sửa hai tệp luật giá trên giao diện, có lịch sử phiên bản; dải giá được tính lại khi lưu.
- **Category**: Tạo dự toán — luật giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: Admin sửa hai tệp luật giá `khung_gia_tham_khao.md` và `quy_tac_boc_tach.md` trên giao diện; có lịch sử phiên bản; mỗi bản dự toán ghi lại phiên bản luật đã dùng; khi lưu mục 3–5 của khung giá, hệ thống tự tính lại các dải giá. App demo: `resources/rules/*.md`, `savico/tools/derive_price_bands.py`.

## Statement

Chặng Dự toán dùng hai tài liệu luật giá: Khung giá tham khảo và Quy tắc bóc tách. Nội dung ban đầu lấy từ app demo. Admin có quyền tương ứng sửa hai tài liệu này trên giao diện; mỗi lần lưu tạo một phiên bản mới. Khi nội dung Khung giá tham khảo được lưu, hệ thống tự tính lại các dải giá (giá thấp nhất và cao nhất trên mỗi m² sàn quy đổi theo loại công trình và gói) từ đơn giá ở mục 3–5. Bản dự toán dùng phiên bản luật đang áp dụng lúc tiếp nhận lần chạy Dự toán.

## When

Admin xem, sửa, xem trước, lưu hoặc khôi phục phiên bản của tài liệu luật giá; hệ thống tiếp nhận lần chạy chặng Dự toán.

## Then

1. Hệ thống nạp sẵn phiên bản đầu tiên của hai tài liệu từ app demo ở mốc chuyển đổi (commit `059e876`), cùng các dải giá tính từ phiên bản này.
2. Admin xem được nội dung hiện hành, danh sách phiên bản (người lưu, thời điểm, ghi chú thay đổi) và so sánh hai phiên bản.
3. Trước khi lưu Khung giá tham khảo, Admin xem trước được các dải giá sẽ được tính lại từ nội dung đang sửa.
4. Hệ thống từ chối lưu khi không đọc được đơn giá cần thiết ở mục 3–5, hoặc kết quả tính dải giá không hợp lệ (thiếu dải, giá thấp nhất lớn hơn giá cao nhất, giá không dương). Lỗi chỉ rõ mục hoặc dòng gây lỗi; phiên bản hiện hành giữ nguyên.
5. Lưu thành công tạo phiên bản mới gồm nội dung tài liệu và các dải giá đã tính lại; phiên bản này áp dụng cho lần chạy Dự toán tiếp nhận sau đó.
6. Hai người lưu cùng lúc dựa trên cùng phiên bản thì người lưu sau bị từ chối và phải tải lại nội dung mới nhất.
7. Khôi phục một phiên bản cũ tạo phiên bản mới có cùng nội dung; không xóa hay sửa các phiên bản ở giữa.
8. Lần chạy Dự toán chốt phiên bản của cả hai tài liệu (và dải giá đi kèm) lúc tiếp nhận; kết quả ghi lại các phiên bản này theo BR-PROJ-014. Sửa luật sau đó không tính lại dự toán đã có.
9. Chỉ người có quyền quản lý luật giá được xem nội dung và sửa. Khách không xem được tài liệu luật giá hay dải giá nội bộ.

## Except

Quy tắc bóc tách không chứa đơn giá nên lưu tài liệu này không tính lại dải giá; vẫn tạo phiên bản mới như khoản 5.

## Notes

- **Đã xác nhận (11/10/2026):** Admin sửa hai tệp luật giá trên giao diện; lịch sử phiên bản; ghi phiên bản đã dùng vào bản dự toán; tự tính lại dải giá khi lưu mục 3–5.
- **Đã xác nhận (11/10/2026):** so sánh phiên bản và khôi phục phiên bản cũ (khoản 2 và 7); điều kiện từ chối lưu ở khoản 4.
- Ở app demo, dải giá gồm 41 dải với 82 con số (giá thấp và giá cao của mỗi dải), tính bằng `savico/tools/derive_price_bands.py` trên 6 loại công trình tham chiếu: phần thô có một dải cho mỗi loại, không đổi theo gói (5 dải; căn hộ không có phần thô nên không có dải, app demo ghi cặp 0 cho căn hộ); phần hoàn thiện và phần nội thất mỗi phần có một dải cho mỗi loại × gói (6 × 3 = 18 dải mỗi phần). Dải giá quyết định mục tiêu giá, mức tự nâng giá, trần giá và cờ cần kỹ sư rà của chặng Dự toán (BR-PROJ-017). Hệ thống thật phải cho ra đúng các dải này từ phiên bản nạp sẵn.
- **Đã xác nhận (12/10/2026):** phiên bản nạp sẵn của hai tài liệu là bản ở commit `059e876` đã bỏ các chỗ áp quy tắc riêng cho Tây Nguyên, theo BR-PROJ-013 (Tây Nguyên thuộc miền Trung, không hạ giá riêng): cụm "Tây Nguyên /" trong câu ở mục 2 cho phép công trình thấp hơn neo (phần tỉnh lẻ giữ nguyên), phần "Tây Nguyên" trong tiêu đề cột và dòng "Gạch xây" của bảng hãng theo miền (mục 9 Khung giá), dòng Tây Nguyên của bảng mùa khởi công (mục 6 Quy tắc bóc tách). Các chỗ này không thuộc mục 3–5 nên dải giá không đổi. Danh sách chỗ sửa ở TDD-PROJ-009/Architecture (mục O).
- Màn quản trị thuộc STORY-PROJ-011.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
