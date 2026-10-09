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

# BR-CMS-002

## Rule Info

- **Name**: Hạng mục và icon CMS động của bảng giá trị khách hàng
- **Category**: CMS Bảng giá
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng ngày 09/10/2026 yêu cầu cho phép thêm hạng mục và chọn icon; sau đó chọn thêm, sửa, xóa và sắp xếp hạng mục. Cơ chế CMS đối chiếu TDD-HB-003; quyền thuê bao đối chiếu BR-SUB-008.

## Statement

Bảng Giá trị khách hàng nhận được là nội dung CMS động, mỗi hạng mục có tên, icon và chữ riêng cho ba cột BASIC/PLUS/PRO. Người quản lý được thêm, sửa, xóa và sắp xếp các hạng mục.

## When

Người có quyền `news.manage` sửa phần Giá trị khách hàng nhận được trong Nội dung → Gói tư vấn → tab Gói thiết kế; hoặc khách đọc bảng này trên `/plans`.

## Then

1. Người quản lý thêm, sửa, xóa và sắp xếp hạng mục ngay trong bảng quản trị; tên và ba ô là chữ CMS, không phải cấu hình entitlement.
2. Mỗi hạng mục cho chọn icon của thư viện frontend đang dùng và xem trước. Icon cùng nội dung đã lưu được hiển thị ở trang công khai.
3. Tiêu đề, hạng mục, icon và thứ tự lưu riêng theo ngôn ngữ qua cơ chế CMS hiện có. Đóng section không làm mất bản đang sửa; chưa lưu không đổi nội dung công khai.
4. Validation chữ, quyền quản lý, kiểm phiên bản, khôi phục và làm mới cache theo CMS PageSection hiện có.
5. Bảng chưa có danh sách động dùng bốn hàng, chữ đã sửa và icon cũ làm dữ liệu ban đầu; mở bảng không tự ghi.
6. Cho phép lưu danh sách rỗng, giữ tiêu đề công khai và không tự phục hồi hàng mặc định; khôi phục là thao tác riêng.
7. Thay đổi CMS không sửa giá, quyền sử dụng, hạn mức và cấu hình gói đã công bố hoặc thuê bao đã mua.

## Except

Icon trống, không tồn tại hoặc không tải được dữ liệu vẽ dùng icon dự phòng của frontend; không làm mất chữ đã nhập.

## Notes

**Đã xác nhận:** Thêm hạng mục, chọn icon; người dùng đã chọn thêm, sửa, xóa và sắp xếp hạng mục. Ba cột tiếp tục là chữ theo bảng value đang dùng, không chuyển sang kiểu Có/Không của bảng comparison.

**Đã chốt ngày 09/10/2026:** Bốn hàng cũ giữ toàn bộ chữ đã nhập, thứ tự và icon làm dữ liệu ban đầu. Xóa hết hàng cho lưu danh sách rỗng, giữ tiêu đề công khai và không tự thêm lại mặc định. Icon chưa chọn hoặc không tra được dùng icon dự phòng của frontend, không chặn lưu chữ theo chính sách icon CMS hiện có.

**Metadata còn thiếu:** Metadata quản trị chưa có tên Reviewer, Approver hoặc Owner; không tự điền người phê duyệt hay trạng thái Active.
