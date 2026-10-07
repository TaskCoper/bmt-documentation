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

# BR-PROJ-003

## Rule Info

- **Name**: Tạo nhanh dự toán bằng tên và tự lưu thông tin đang nhập.
- **Category**: Tạo dự toán — lưu tiến độ
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng yêu cầu ngày 01/10/2026: tạo nhanh không bắt buộc kinh độ, vĩ độ. Thay yêu cầu trước đó bắt buộc tọa độ ngay khi tạo. Quy tắc gửi đủ cặp khi đổi địa chỉ và trước khi gửi AI giữ nguyên. Ngày 07/10/2026 người dùng chốt mã dự toán `BUILDX-YYYYMMDD-XXXXXX` (khoản 12).

## Statement

Khách nhập tên để tạo nhanh dự toán khi đáp ứng các điều kiện tạo hiện hành. Không bắt buộc kinh độ, vĩ độ ở bước này; có thể bổ sung sau khi nhập địa chỉ. Hệ thống tự lưu thông tin đang nhập để khách mở lại và làm tiếp.

## When

Khách tạo nhanh dự toán bằng tên, hoặc nhập, sửa thông tin đầu vào của bản dự toán đang chuẩn bị gửi AI.

## Then

1. Tạo dự toán sau thao tác tạo với tên hợp lệ, không đợi có tọa độ hoặc đến khi khách bấm Nhận dự toán. Có thể nhập mô tả ngay khi tạo nhanh; mô tả không bắt buộc, tối đa 500 ký tự và dùng chung Mô tả chi tiết của biểu mẫu đầu vào theo BR-PROJ-002. Không thêm trường ghi chú riêng.
2. Cho phép tạo dự toán khi chưa có diện tích, ảnh hoặc mô tả thiết kế. Đây là thông tin chưa hoàn tất, chưa đủ điều kiện gửi AI.
3. Tự lưu các thay đổi đầu vào trong bản dự toán đang chuẩn bị; không yêu cầu khách bấm Lưu nháp cho từng lần lưu.
4. Khi mở lại bản dự toán, trả thông tin đã lưu thành công để khách nhập tiếp.
5. Cho phép lưu tiến độ chưa đầy đủ. Điều kiện có diện tích và có ít nhất ảnh hoặc mô tả được kiểm tra khi chuẩn bị gửi AI theo BR-PROJ-001 và BR-PROJ-002.
6. Thao tác tạo dự toán hoặc tự lưu không tự khởi chạy AI. Cách tiếp nhận yêu cầu AI thuộc luồng gửi tạo thiết kế.

7. Khi tự lưu thất bại do mất mạng hoặc lỗi máy chủ, giữ nội dung đang nhập khi trang còn mở, thông báo chưa lưu và cung cấp thao tác Thử lại. Tự thử lưu lại khi kết nối phục hồi; mỗi yêu cầu thử lại vẫn phải kiểm tra quyền, điều kiện gói/lượt, dữ liệu và trạng thái khóa đầu vào.
8. Chỉ báo đã lưu sau khi backend xác nhận lưu thành công. Tự thử lưu lại hoặc bấm Thử lại ở đây không gửi lại tác vụ AI.

9. Tên bản dự toán bắt buộc có nội dung, tối đa 200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Lưu tên sau khi bỏ khoảng trắng đầu/cuối; tên rỗng, chỉ có khoảng trắng hoặc vượt 200 ký tự sau bước này không hợp lệ. Từ chối tạo hoặc lưu tên không hợp lệ; không tự cắt ngắn tên để chấp nhận, giữ dữ liệu đã lưu khi yêu cầu sửa bị từ chối. Đổi tên bản dự toán đã có chỉ cần quyền sở hữu và tên hợp lệ; không phụ thuộc gói, lượt hay khóa đầu vào, theo BR-SUB-007 khoản 11.

10. Tạo nhanh được bỏ cả kinh độ và vĩ độ hoặc gửi cả hai null; backend lưu NULL, không tự điền (0,0). Nếu gửi tọa độ thì phải có đủ cặp hợp lệ. Chỉ gửi một tọa độ hoặc gửi giá trị sai bị từ chối. Bản chưa có tọa độ vẫn tự lưu các trường khác khi không đổi địa chỉ. Trước khi gửi AI phải có đủ tọa độ hợp lệ.

11. Khi khách đổi địa chỉ, frontend lấy lại kinh độ và vĩ độ từ dịch vụ bản đồ, gửi đủ cả hai cùng yêu cầu lưu địa chỉ theo BR-PROJ-004 khoản 18. Backend lưu địa chỉ và tọa độ cùng nhau; thiếu một hoặc cả hai tọa độ thì từ chối yêu cầu, giữ nguyên dữ liệu đã lưu. Việc sửa vẫn phải đáp ứng điều kiện quyền/gói/lượt và khóa đầu vào hiện hành.

12. Khi tạo, hệ thống cấp cho bản dự toán một mã dạng `BUILDX-YYYYMMDD-XXXXXX`, ví dụ `BUILDX-20261005-Q7K2M9`. `BUILDX` là thương hiệu; `YYYYMMDD` là ngày tạo theo giờ Việt Nam (Asia/Ho_Chi_Minh); `XXXXXX` là 6 ký tự ngẫu nhiên lấy từ chữ hoa và số, bỏ 0, O, 1, I, L vì dễ đọc nhầm với nhau (còn 31 ký tự). Mã không trùng với mã của bản dự toán khác, kể cả bản đã xóa, và không được cấp lại cho bản khác. Mã đã cấp không bao giờ đổi, kể cả khi đổi tên hoặc sửa đầu vào; khách không tự nhập hay sửa mã. Mã hiển thị ở danh sách, chi tiết và kết quả của chủ sở hữu, và ở trang xem qua link chia sẻ theo BR-PROJ-006 khoản 10.

## Except

Không hỗ trợ khôi phục phần chưa lưu sau khi đóng hoặc tải lại trang; khi mở lại chỉ trả dữ liệu backend đã lưu thành công. Không tự thử lại yêu cầu bị từ chối do không đủ quyền/gói/lượt theo BR-SUB-007, hoặc dùng thử lại để vượt qua khóa đầu vào theo BR-PROJ-005. Lỗi dữ liệu cần được sửa trước khi yêu cầu lưu lại.

Bản dự toán tạo trước khi áp dụng khoản 12, kể cả bản đã xóa, được cấp mã một lần theo ngày tạo gốc (giờ Việt Nam); sau đó mã cũng không đổi.

## Notes

- **Đã xác nhận ngày 07/10/2026 (mã dự toán)**: định dạng, bảng ký tự, ngày theo giờ Việt Nam, mã không đổi và không cấp lại mã của bản đã xóa, cấp mã cho bản đã có theo ngày tạo gốc, nơi hiển thị và tìm theo mã (BR-PROJ-008 khoản 5). Mã chỉ để nhận diện, không phải mã bí mật và không cấp quyền truy cập.
- **Đã xác nhận ngày 01/10/2026 (điều chỉnh tạo nhanh)**: tên là dữ liệu bắt buộc để tạo; tọa độ có thể bổ sung sau. Đổi địa chỉ vẫn phải gửi đủ cặp tọa độ từ bản đồ. Không thay đổi tọa độ đã lưu của bản cũ.
- **Đề xuất chưa chốt**: chưa có đề xuất nghiệp vụ bổ sung trong phạm vi thay đổi tọa độ này.
- **Đã chốt US/BR trong hội thoại ngày 01/10/2026**: người dùng đã chốt phần bổ sung tọa độ của STORY-PROJ-001, BR-PROJ-003 và BR-PROJ-004. System Test và TDD đã được cập nhật; backend đã lên `develop` tại commit `5e396fc`. Phạm vi kiểm thử và phần chưa kiểm được ghi trong [bàn giao tọa độ](../discovery/estimate-coordinates-implementation.md). Xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.
- Quy tắc nói về thời điểm tạo và lưu, không có nghĩa tạo một bản dự toán mới sau mỗi ký tự trong ô tên.
- Lưu thông tin chưa đầy đủ không đồng nghĩa chấp nhận mọi giá trị không hợp lệ. Ràng buộc cho dữ liệu đã nhập sẽ được chốt theo từng trường.
- BR-PROJ-001 và BR-PROJ-002 quy định điều kiện đầy đủ trước khi gửi AI; thiếu dữ liệu theo hai quy tắc đó không tự chặn lưu tiến độ.
- Điều kiện quyền sở hữu, quyền tạo/lưu bản dự toán và hạn mức vẫn cần được áp dụng theo bộ quy tắc liên quan. Tự lưu không tạo ngoại lệ cho các điều kiện đó.
- Khoảng chờ trước mỗi lần lưu, xử lý yêu cầu đến sai thứ tự và cập nhật đồng thời thuộc thiết kế kỹ thuật; chưa đặt con số hoặc chọn cách giải quyết xung đột trong bản nháp này.
- Thông tin đầu vào bị khóa khi AI đang chạy; sau thất bại được sửa khi đủ điều kiện theo BR-PROJ-005. Bản dự toán đã thành công muốn phương án khác phải tạo dự toán mới; tự lưu không mở quyền thay thế kết quả cũ.
- Reviewer và Approver là Tân Trần theo xác nhận trong hội thoại; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
