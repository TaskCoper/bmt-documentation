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

# BR-PROJ-017

## Rule Info

- **Name**: Ghi chú nội bộ và cờ cần rà soát chỉ lưu nội bộ, không xuất hiện trong nội dung gửi khách.
- **Category**: Tạo dự toán — kết quả và hồ sơ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: cờ “cần kỹ sư rà” và “cần nhân viên rà” chỉ lưu nội bộ, không có bước duyệt; ghi chú nội bộ được lọc khỏi nội dung gửi khách như app demo đang làm. App demo: `savico/modules/estimate/fill.py` (lọc nội dung nội bộ), `staff.py` (chẩn đoán cho nhân viên), `dutoan.py` (cờ `can_ky_su_duyet`).

## Statement

Khi lập dự toán, hệ thống có thể gắn cờ “cần kỹ sư rà” hoặc “cần nhân viên rà” và tạo ghi chú chẩn đoán nội bộ, chẳng hạn khối lượng lệch so với hình học, đơn giá dưới sàn phải tự nâng, hoặc thiếu dòng. Các cờ và ghi chú này chỉ được lưu nội bộ. Nội dung khách thấy trên màn hình, trong Excel, trong PDF và qua link chia sẻ đã được lọc bỏ mọi thông tin nội bộ. Cờ không chặn chặng Dự toán thành công và không tạo bước duyệt.

## When

Chặng Dự toán tạo kết quả; hệ thống dựng nội dung hiển thị, Excel hoặc PDF cho khách; người có quyền xem chi tiết luồng AI của một bản dự toán.

## Then

1. Kết quả Dự toán được lưu thành hai phần: phần gửi khách và phần nội bộ. Phần nội bộ gồm cờ cần rà, lý do gắn cờ, ghi chú chẩn đoán, tên model và chi phí AI.
2. Trước khi lưu phần gửi khách, hệ thống áp dụng bộ lọc nội dung nội bộ của app demo: bỏ các câu hoặc vế câu nhắc tới khung giá, bảng giá, chỉ tiêu nội bộ, số dòng tối thiểu, khối lượng tham chiếu và các dấu hiệu nội bộ khác. Nội dung hợp lệ còn lại giữ nguyên.
3. Màn hình kết quả của khách, Excel, PDF và trang xem qua link chỉ dùng phần gửi khách.
4. Cờ cần rà không chặn chặng Dự toán thành công, không chặn các chặng sau, không tạo việc cần duyệt và không gửi thông báo cho khách.
5. Người có quyền xem chi tiết luồng AI xem được phần nội bộ của một bản dự toán. Khách không xem được dù gửi yêu cầu trực tiếp.

## Except

Lưới an toàn có thể tự đổi số liệu trước khi lưu (ví dụ tự nâng đơn giá lên sàn khung giá). Số liệu đã đổi là kết quả gửi khách; lý do đổi chỉ nằm trong phần nội bộ.

## Notes

- **Đã xác nhận (11/10/2026):** cờ chỉ lưu nội bộ, không có bước duyệt; lọc ghi chú nội bộ như app demo.
- Ví dụ từ lần chạy thử ngày 11/10/2026: đơn giá AI trả 4,42 triệu/m² dưới sàn 6,96 triệu/m², hệ thống tự nâng đơn giá lên 6,97 triệu/m². Khách thấy bảng giá sau khi nâng; dòng “đã tự nâng đơn giá ×1,575 để đạt neo” chỉ nằm trong phần nội bộ.
- Màn xem chi tiết cho người có quyền thuộc STORY-PROJ-012.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
