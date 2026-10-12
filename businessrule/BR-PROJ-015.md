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

# BR-PROJ-015

## Rule Info

- **Name**: Ảnh lô đất được AI soi ngay khi tải lên; chỉ ảnh đạt mới được dùng để tạo bản vẽ 2D.
- **Category**: Tạo dự toán — thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt ngày 11/10/2026: ảnh lô đất bắt buộc, JPG/PNG/WebP, tối đa 10 MB, tối thiểu 640×480; AI soi ảnh ngay khi tải lên và ảnh không đạt chặn tạo 2D; frontend giữ cách tải ảnh qua URL presign, backend tự đọc ảnh từ kho để kiểm tra; kết quả soi được nhớ theo tài khoản, ảnh, phiên bản prompt và model, chỉ dùng lại trong cùng tài khoản; mỗi tài khoản soi tối đa 30 ảnh mới mỗi ngày, Admin cấu hình được. Người dùng chốt ngày 12/10/2026: thêm trần số lần soi mới của cả hệ thống mỗi ngày kèm cảnh báo để chống lạm dụng; soi ảnh bị từ chối khi dịch vụ AI đang tạm ngắt. App demo: `docs/API.md` mục “Quy tắc cần nhớ”.

## Statement

Ảnh lô đất là ảnh bản vẽ hoặc sơ đồ thể hiện hình dạng lô đất, mặt bằng hoặc các phòng. Sau khi khách tải ảnh lên, hệ thống đọc ảnh từ kho, kiểm tra định dạng, dung lượng và kích thước, rồi gửi AI soi. Kết quả soi là Đạt hoặc Không đạt kèm lý do. Chỉ ảnh lô đang gắn với bản dự toán và có kết quả Đạt mới được dùng để chạy chặng 2D. Mỗi tài khoản được soi tối đa 30 ảnh mới mỗi ngày; cả hệ thống cũng có trần số lần soi mới mỗi ngày. Admin cấu hình được cả hai con số.

## When

Khách tải ảnh lô đất cho bản dự toán, yêu cầu soi lại ảnh, hoặc yêu cầu chạy chặng 2D.

## Then

1. Ảnh lô hợp lệ về tệp khi: định dạng thật đọc từ nội dung là JPG, PNG hoặc WebP; dung lượng không vượt 10 MiB (10.485.760 byte); kích thước tối thiểu 640×480 điểm ảnh; cạnh dài không quá 3 lần cạnh ngắn. Ảnh không đạt một trong các điều kiện này bị từ chối, không gửi AI soi và không tính vào hạn mức soi.
2. Backend tự đọc nội dung ảnh từ kho để kiểm tra và tính dấu vân tay (hash) của ảnh; không tin định dạng, dung lượng hay kích thước do giao diện gửi lên.
3. AI soi ảnh theo tiêu chí của app demo:
   - Đạt: bản vẽ tay, bản vẽ kỹ thuật, mặt bằng hoặc trích lục/bản đồ địa chính thể hiện được hình dạng lô đất hoặc các phòng; kể cả ảnh chụp, bản scan hay ảnh chụp màn hình của các bản vẽ đó.
   - Không đạt: ảnh không phải bản vẽ (ảnh chụp nhà, phòng, đất hay người thực tế, ảnh phối cảnh 3D, văn bản…); bản vẽ không thể hiện phòng lẫn hình dạng lô; một ảnh có từ hai mặt bằng tầng trở lên.

   Thiếu kích thước hoặc chi tiết không làm ảnh Không đạt; chặng 2D tự thiết kế phần còn thiếu.
4. Kết quả soi kèm lý do bằng ngôn ngữ khách hiểu được. Bước soi không đọc kích thước lô; kích thước trên ảnh do chặng 2D đọc theo BR-PROJ-001 khoản 6.
5. Ảnh có kết quả Không đạt chặn chạy chặng 2D. Khách cần thay ảnh khác. Ảnh chưa có kết quả soi (đang soi, hoặc soi lỗi do dịch vụ AI) cũng chặn chạy 2D; khách được yêu cầu soi lại.
6. Kết quả soi được nhớ lâu dài theo bộ (tài khoản, hash ảnh, phiên bản prompt soi, model soi). Cùng tài khoản tải lại đúng ảnh đó khi prompt và model không đổi thì dùng lại kết quả cũ, không gọi AI và không tính vào hạn mức soi. Kết quả soi của tài khoản này không được dùng cho tài khoản khác.
7. Mỗi tài khoản được tối đa 30 lần soi mới mỗi ngày theo giờ Việt Nam (Asia/Ho_Chi_Minh), Admin cấu hình được. Lần soi dùng lại kết quả cũ, ảnh bị từ chối ở khoản 1 và lần soi lỗi do dịch vụ AI không tính vào hạn mức. Hết hạn mức thì từ chối soi ảnh mới đến hết ngày và báo khách thử lại vào ngày hôm sau.
8. Soi ảnh không giữ, trừ hay hoàn lượt tạo dự toán theo BR-SUB-003, và không cần gói còn lượt; khách vẫn cần quyền sửa bản dự toán theo BR-PROJ-005.
9. Khi tiếp nhận lần chạy 2D, hệ thống kiểm tra ảnh đang gắn với bản dự toán có kết quả Đạt đã lưu. Nếu Admin đổi model hoặc prompt soi sau lần soi đó, kết quả Đạt cũ vẫn được dùng; không bắt khách soi lại. Model và prompt mới chỉ áp dụng cho ảnh được soi sau khi đổi.
10. **Trần chung của cả hệ thống:** tổng số lần soi mới của mọi tài khoản trong một ngày (giờ Việt Nam) không vượt trần Admin cấu hình. Khi tổng đạt 80% trần, hệ thống gửi một cảnh báo cho Admin trong ngày đó. Khi đã đạt trần, yêu cầu soi ảnh mới bị từ chối với thông báo "Hệ thống đang bận, vui lòng thử lại sau" và không tính vào hạn mức của tài khoản. Lần soi dùng lại kết quả cũ theo khoản 6 vẫn được trả, vì không gọi AI.
11. Khi dịch vụ AI dùng để soi ảnh đang tạm ngắt theo BR-PROJ-012 khoản 9, yêu cầu soi ảnh mới bị từ chối ngay, không tính vào hạn mức.

## Except

- Thay ảnh khi đầu vào đang khóa theo BR-PROJ-005 bị từ chối như mọi thay đổi đầu vào khác; không soi ảnh mới cho bản đang khóa.
- Ảnh danh mục phong cách của Admin và các ảnh khác không thuộc quy tắc này; giới hạn chung của BR-MEDIA-001 vẫn áp dụng cho chúng.

## Notes

- **Đã xác nhận (12/10/2026):** thêm trần chung mỗi ngày và cảnh báo (khoản 10); giữ 30 lần mỗi tài khoản. **Đề xuất chưa chốt:** trần chung mặc định 1.000 lần soi mới mỗi ngày, Admin chỉnh được.
- **Đã xác nhận (11/10/2026, khi duyệt TDD):** từ chối ảnh có cạnh dài quá 3 lần cạnh ngắn, như app demo (khoản 1).
- **Đã xác nhận (11/10/2026):** ảnh lô bắt buộc; JPG/PNG/WebP; tối thiểu 640×480; AI soi khi tải lên; ảnh không đạt chặn 2D; backend đọc ảnh từ kho; nhớ kết quả theo tài khoản + hash + prompt + model; 30 lần soi mới mỗi ngày, Admin cấu hình được.
- **Đã xác nhận (11/10/2026, lượt sau):** giới hạn 10 MiB dành riêng cho ảnh lô đất; các ảnh khác giữ 5 MiB theo BR-MEDIA-001, quy tắc đó được bổ sung ngoại lệ tương ứng.
- **Đã xác nhận (11/10/2026):** hạn mức soi đặt lại lúc 00:00 giờ Việt Nam; lần soi lỗi do dịch vụ AI, ảnh sai tệp và ảnh đã soi không tính vào hạn mức (khoản 7); đổi model hoặc prompt soi thì vẫn dùng kết quả Đạt cũ (khoản 9).
- App demo nhận nhiều ảnh, mỗi ảnh một tầng. Hệ thống thật giữ một ảnh đầu vào cho mỗi bản dự toán theo BR-PROJ-002; ảnh có từ hai mặt bằng tầng trở lên Không đạt như app demo.
- Ví dụ: khách tải ảnh chụp căn nhà đang xây. AI trả Không đạt, lý do “ảnh chụp công trình thực tế, không phải bản vẽ”. Nút tạo 2D bị khóa cho tới khi khách thay bằng ảnh bản vẽ lô đất và ảnh này Đạt.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
