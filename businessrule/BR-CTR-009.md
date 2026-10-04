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

# BR-CTR-009

## Rule Info

- **Name**: Lưu địa chỉ nhà thầu theo tỉnh/thành, phường/xã và số nhà–đường.
- **Category**: Nhà thầu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng yêu cầu ngày 2026-10-04 bổ sung địa chỉ nhà thầu tương tự Dự toán/Công trình; đối chiếu BR-PROJ-004 và BR-SITE-001. Chi tiết tương thích và điều kiện lưu đã được người dùng chốt cùng TDD-CTR-003.

## Statement

Địa chỉ công ty của nhà thầu được lưu riêng theo tỉnh/thành, phường/xã và số nhà–đường. Tỉnh và phường dùng nguồn danh mục chung với dự toán/công trình; phường phải thuộc tỉnh được chọn. Miền tiếp tục xác định theo tỉnh công ty.

## When

Admin tạo, sửa hoặc đọc địa chỉ nhà thầu; khách đọc hồ sơ nhà thầu công khai.

## Then

1. Admin chọn tỉnh/thành, chọn phường/xã thuộc tỉnh và nhập số nhà–đường. Không thêm cấp quận/huyện. Địa chỉ đăng ký pháp nhân và vị trí dự án đã thực hiện là trường khác, không thuộc thay đổi này.
2. Backend lưu mã và tên tỉnh/phường đã xác minh cùng phiên bản danh mục; không dùng tên do client tự khai. Số nhà–đường là trường riêng; địa chỉ đầy đủ được ghép từ ba phần để hiển thị.
3. Đổi tỉnh thì frontend bỏ lựa chọn phường cũ và yêu cầu chọn phường thuộc tỉnh mới; giữ số nhà–đường để admin sửa. Từ chối phường sai tỉnh ngay cả khi gọi API trực tiếp.
4. Miền lấy từ mã tỉnh theo BR-CTR-008. Danh mục địa chỉ và bảng phân miền có vai trò khác nhau; không coi phiên bản của hai danh mục là một.
5. Giữ tạo nhanh chỉ cần tên. Khi bắt đầu nhập địa chỉ mới trong form admin, nhập đủ ba phần và xác định tọa độ trước khi lưu; địa chỉ và cặp tọa độ được ghi cùng một lần sửa.
6. Giữ nguyên địa chỉ, tỉnh và tọa độ cũ khi hồ sơ chưa được bổ sung cấu trúc mới. Không đoán phường hoặc số nhà–đường từ chuỗi địa chỉ. Sửa trường khác không buộc chuyển địa chỉ cũ ngay.
7. Chỉ xóa bộ địa chỉ đã tách khi admin chủ động xóa toàn bộ và nhà thầu đang Ẩn; nhà thầu Hiển thị tiếp tục phải đủ địa chỉ/tọa độ theo BR-CTR-002. Không tự làm hồ sơ cũ mất trạng thái Hiển thị.
8. Giữ mã/tên/phiên bản đã lưu khi địa chỉ không đổi, kể cả nguồn danh mục lỗi hoặc thay tên. Chọn địa chỉ mới phải dùng danh mục hiện hành; nếu nguồn lỗi và có bản lưu thì dùng bản lưu, nếu chưa có bản dùng được thì báo lỗi, không nhận lựa chọn giả.

## Except

- Dữ liệu cũ chỉ có Address hoặc thêm ProvinceCode vẫn được đọc theo dữ liệu đã lưu, cho đến khi admin bổ sung địa chỉ tách riêng.
- Địa chỉ trong lời mời báo giá đã gửi không được viết lại theo địa chỉ công ty mới; snapshot lịch sử giữ nguyên theo BR-RFQ-003.

## Notes

Yêu cầu tách ba phần đã được người dùng giao triển khai. Người dùng đã chốt phần tương thích và điều kiện lưu trong TDD-CTR-003 bằng phản hồi “chốt”. Quyền, version, Ẩn/Hiển thị và bộ lọc miền giữ quy tắc hiện hành. Nguồn dùng lại: [BR-PROJ-004](BR-PROJ-004.md), [BR-SITE-001](BR-SITE-001.md), [BR-CTR-008](BR-CTR-008.md). Thiết kế tại [TDD-CTR-003](../tdd/TDD-CTR-003.md); đã triển khai phần bổ sung này trong workspace; chưa phát hành lên môi trường chung.
