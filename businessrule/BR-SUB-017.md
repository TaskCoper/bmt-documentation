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

# BR-SUB-017

## Rule Info

- **Name**: Chỉ quản lý hai loại lượt: tạo thiết kế mới và tra cứu mẫu.
- **Category**: Subscription thiết kế
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chốt chỉ có lượt tạo mới và tra cứu, bỏ chỉnh sửa sau Gen AI. Đã thành công muốn phương án khác phải tạo dự án mới; thất bại được thử lại cùng dự án, kiểm tra lại quyền/lượt. Mỗi lần trả chi tiết mẫu tính 1 lượt, lỗi không mất lượt. Khách chưa đăng nhập hoặc chưa có gói vẫn được tìm kiếm/xem danh sách, không mất lượt; mở chi tiết cần đăng nhập, gói còn hiệu lực, quyền tra cứu và lượt sẵn dùng.

## Statement

Hệ thống chỉ quản lý hai loại lượt: tạo thiết kế mới và tra cứu mẫu. Mỗi loại có hạn mức riêng, dùng chung cho các dự án của tài khoản. Không có tính năng chỉnh sửa thiết kế sau khi Gen AI trả kết quả; không cấu hình, cấp, kiểm tra, giữ, trừ hoặc hoàn lượt chỉnh sửa.

## When

Khách tìm kiếm/xem mẫu, Admin cấu hình quyền lợi của gói hoặc hệ thống cấp, kiểm tra, ghi nhận sử dụng lượt cho tài khoản.

## Then

1. Theo dõi riêng lượt tạo mới và lượt tra cứu. Sử dụng một loại không thay đổi số dư loại còn lại và không tự lấy lượt loại khác dùng thay khi hết lượt.
2. Tạo mới giữ 1 lượt khi yêu cầu hợp lệ được tiếp nhận, thành công mới tính đã dùng; lỗi/quá thời gian giải phóng lượt theo BR-SUB-003 và BR-SUB-016. Một lần Gen AI trả bộ kết quả cùng lúc; không có thao tác hoặc loại lượt tạo 3D riêng. Theo phạm vi mới nhất, cấu hình 3D chỉ hiển thị, chưa điều khiển kết quả AI.
3. Khách được tìm kiếm và xem danh sách mẫu kể cả khi chưa đăng nhập hoặc chưa có gói; không yêu cầu tài khoản, quyền tra cứu hay lượt để thực hiện hai thao tác này, không giữ hoặc trừ lượt. Mở chi tiết mẫu phải đăng nhập, có gói còn hiệu lực, quyền tra cứu và ít nhất một lượt sẵn dùng nếu hạn mức hữu hạn; quyền không giới hạn theo BR-SUB-005. Không trả nội dung chi tiết khi thiếu điều kiện, kể cả yêu cầu gửi trực tiếp. Mỗi lần trả được nội dung chi tiết một mẫu tính 1 lượt tra cứu. Lỗi không tải được nội dung thì không tính đã dùng, giải phóng lượt tạm giữ nếu có.
4. Danh mục quyền lợi không có lượt chỉnh sửa; Admin không thể cấu hình hoặc công bố hạn mức chỉnh sửa. Không hiển thị số dư chỉnh sửa cho khách và không xây luồng Gen AI chỉnh sửa kết quả.
5. Sau khi một dự án đã Gen AI thành công và có kết quả, muốn phương án khác khách phải tạo dự án mới, nhập thông tin rồi Gen AI. Từ chối yêu cầu Gen AI thêm phương án trên dự án đã thành công, kể cả gửi trực tiếp. Không sửa hoặc thay thế kết quả của dự án cũ. Dự án mới tiếp tục dùng hạn mức chung của tài khoản, không được cấp hạn mức riêng.
6. Nếu Gen AI đã thất bại và chưa có kết quả thành công, khách được chủ động thử lại trên cùng dự án, giữ thông tin đã nhập. Mỗi yêu cầu thử lại kiểm tra subscription còn hiệu lực, quyền và lượt sẵn dùng tại thời điểm tiếp nhận; đủ điều kiện thì giữ 1 lượt tạo mới theo BR-SUB-003. Không tự thử lại, không tạo dự án mới bắt buộc và không bỏ qua kiểm tra quyền vì yêu cầu trước từng hợp lệ.
7. Giá trị hạn mức thực tế do Admin nhập sau; các số trong test chỉ là ví dụ. Quyền dạng lượt đưa vào gói phải có hạn mức ít nhất 1 hoặc không giới hạn theo [BR-SUB-005](BR-SUB-005.md); không cho nhập 0.

8. Khi hệ thống đã ghi nhận một lần mở chi tiết mẫu thành công nhưng đường truyền bị ngắt trước khi khách nhận phản hồi, giữ một lượt đã tính. Gửi lại cùng lần mở trả lại kết quả đã ghi nhận và không trừ thêm lượt. Lần mở mới, kể cả cùng mẫu, vẫn tính lượt riêng. Lỗi tải hoặc chuẩn bị nội dung trước khi ghi nhận thành công không tính lượt.

## Except

Chế độ không giới hạn vẫn theo BR-SUB-005. Bỏ lượt chỉnh sửa không có nghĩa cho chỉnh sửa thiết kế miễn phí hoặc không giới hạn; tính năng đó không tồn tại trong phạm vi hiện tại. Đây không phải khoản nợ để tự triển khai ở đợt sau. Không suy ra mọi quyền bật/tắt hoặc mức tính năng đã được chốt.

## Notes

- Quyết định này thay thế các trao đổi trước về lượt chỉnh sửa, gồm thời điểm giữ/trừ và xử lý lỗi. Không dùng những quyết định cũ để triển khai.
- [BR-SUB-003](BR-SUB-003.md), [BR-SUB-016](BR-SUB-016.md) quy định tạo mới; [BR-SUB-001](BR-SUB-001.md) quy định dùng chung theo tài khoản; [BR-SUB-005](BR-SUB-005.md) quy định không giới hạn.
- [STORY-SUB-001](../userstory/STORY-SUB-001.md) và [ST-SUB-045](../systemtest/ST-SUB-045.md) kiểm tra hai loại lượt riêng. [STORY-SUB-002](../userstory/STORY-SUB-002.md) và [ST-SUB-058](../systemtest/ST-SUB-058.md) kiểm tra danh mục không có lượt chỉnh sửa.
- [ST-SUB-050](../systemtest/ST-SUB-050.md), [ST-SUB-051](../systemtest/ST-SUB-051.md), [ST-SUB-052](../systemtest/ST-SUB-052.md) kiểm tra tra cứu mẫu. Mốc xác nhận trả nội dung, yêu cầu mạng gửi lặp và giữ lượt tạm thời sẽ mô tả trong TDD.
- Lượt giám sát vẫn hoãn theo [nợ nghiệp vụ](../debt/supervision-offline.md), khác với lượt chỉnh sửa đã bị loại bỏ.
- [ST-SUB-059](../systemtest/ST-SUB-059.md) kiểm tra muốn phương án khác phải tạo dự án mới. Quy tắc này xét dự án đã có kết quả thành công. Khi thất bại chưa có kết quả, được thử lại cùng dự án; [ST-SUB-060](../systemtest/ST-SUB-060.md) và [ST-SUB-061](../systemtest/ST-SUB-061.md) kiểm tra thử lại hợp lệ và từ chối khi không còn đủ điều kiện.
- [ST-SUB-077](../systemtest/ST-SUB-077.md) kiểm tra khách đã đăng nhập chưa có gói được tìm kiếm/xem danh sách nhưng không được mở chi tiết. Không tự cấp gói miễn phí hoặc lượt từ việc xem danh sách. Người dùng đã xác nhận khách chưa đăng nhập cũng được tìm kiếm/xem danh sách. Khi mở chi tiết, yêu cầu đăng nhập trước rồi kiểm tra gói, quyền và lượt; chưa đăng nhập thì không trả nội dung chi tiết. [ST-SUB-078](../systemtest/ST-SUB-078.md) kiểm tra ranh giới này.
- Bản nháp chưa đủ metadata; test chưa chạy.
