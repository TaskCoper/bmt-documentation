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


# BR-SUB-008

## Rule Info

- **Name**: Admin cấu hình gói bằng danh mục quyền lợi do hệ thống định nghĩa sẵn.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng xác nhận hệ thống định nghĩa danh mục; Admin chọn quyền và cấu hình gói. Gói chưa có quyền được lưu nháp, Công bố cần ít nhất một quyền lợi. Phối cảnh 3D chân thực là quyền riêng do Admin cấp theo gói, không mặc định chỉ PRO. Dự toán nội thất và bố trí công năng có cùng mức chi tiết giữa các gói, không phân cấp theo tên hoặc giá gói.

## Statement

Hệ thống định nghĩa sẵn danh mục quyền lợi gắn với các tính năng. Admin chọn quyền lợi trong danh mục và cấu hình giá trị cho từng gói; không tự tạo định nghĩa quyền lợi mới qua chức năng quản trị gói. Gói chưa có quyền lợi được lưu nháp; khi Công bố, gói thiết kế phải có ít nhất một quyền lợi. Gói giám sát không dùng danh mục quyền lợi; gói được Công bố khi có tên, giá và mô tả dịch vụ tự do.

## When

Admin chọn hoặc cấu hình quyền lợi trong bản nháp của gói, hoặc hệ thống kiểm tra quyền đã cấp theo gói thiết kế.

## Then

1. Chỉ nhận quyền lợi thuộc danh mục do hệ thống định nghĩa.
2. Giá trị của quyền lợi được cấu hình theo dạng đã định nghĩa: bật/tắt, hạn mức lượt hoặc mức tính năng. Với quyền bật/tắt, Admin phải thêm quyền và bật thì bản gói đó mới cho phép sử dụng tính năng; chưa thêm hoặc đã tắt đều không cho phép theo BR-SUB-005. Với quyền dạng mức, phải thêm quyền và chọn mức cụ thể; chưa cấu hình không cho phép dùng tính năng, không tự cấp mức thấp nhất.
3. Không tạo quyền lợi mới từ tên hoặc mã tùy ý mà Admin gửi lên. Cấu hình dùng quyền lợi không tồn tại trong danh mục không được chấp nhận.
4. Admin được chọn đưa quyền tạo thiết kế, quyền tra cứu mẫu hoặc cả hai vào gói thiết kế; không bắt buộc gói có đủ cả hai. Quyền dạng lượt được chọn phải có hạn mức ít nhất 1 hoặc không giới hạn theo BR-SUB-005.
5. Gói không có quyền tạo thiết kế thì không cho tạo/lưu thông tin dự án hoặc bắt đầu Gen AI; gói không có quyền tra cứu mẫu thì không cho mở chi tiết mẫu. Không tự cấp lượt mặc định cho quyền bị bỏ khỏi gói, không giữ/trừ lượt của quyền khác để thay thế.
6. Thêm, bỏ hoặc đổi giá trị quyền trong gói đều đi qua lưu nháp và Công bố; giữ nguyên quyền lợi của kỳ hiện tại theo BR-SUB-004.

7. Cho phép lưu nháp gói chưa chọn quyền lợi nếu các dữ liệu khác hợp lệ. Khi Công bố gói thiết kế, nếu danh sách quyền lợi rỗng thì từ chối và báo cần thêm ít nhất một quyền lợi, kể cả yêu cầu gửi trực tiếp. Không tự thêm quyền mặc định để vượt qua điều kiện này. Gói giám sát không xét điều kiện này: Công bố gói giám sát cần tên, giá và mô tả dịch vụ có nội dung; mô tả chỉ để hiển thị và được chốt theo đơn như nội dung tư vấn theo BR-SUB-004.
8. Sau khi thêm ít nhất một quyền lợi hợp lệ, gói đạt điều kiện về số lượng quyền lợi để Công bố; vẫn phải đáp ứng các điều kiện khác đã chốt. Công bố bị từ chối không thay thế bản đang áp dụng hoặc thay đổi quyền đã cấp.

9. Phối cảnh 3D chân thực vẫn là quyền dạng bật/tắt để Admin cấu hình và hiển thị theo gói; không mặc định chỉ PRO. Hiện chưa triển khai logic kiểm tra quyền 3D để quyết định AI tạo hay không tạo 3D. Không thêm lượt 3D hoặc điều kiện bắt buộc quyền tạo thiết kế chỉ vì bật cấu hình 3D.

10. Phần dự toán nội thất trong bộ kết quả Gen AI có cùng mức chi tiết giữa các gói thiết kế có quyền tạo thiết kế. Không cấu hình mức sơ bộ/chi tiết khác nhau theo gói, không tự giảm mức chi tiết vì tên hoặc giá gói thấp hơn. Quy tắc này không cấp quyền Gen AI cho gói chỉ có tra cứu. Mức chi tiết chung và nội dung dự toán cụ thể còn cần xác định trong đặc tả tính năng.

11. Phần bố trí công năng trong bộ kết quả Gen AI có cùng mức chi tiết giữa các gói có quyền tạo thiết kế. Không cấu hình mức cơ bản/nâng cao khác nhau theo gói, không giảm mức chi tiết chỉ vì tên hoặc giá gói. Nội dung bố trí cụ thể vẫn phụ thuộc dữ liệu từng dự án; tiêu chí về mức chi tiết chung sẽ xác định trong đặc tả tính năng. Quy tắc này không cấp thêm quyền tạo phối cảnh 3D chân thực hoặc quyền Gen AI cho gói chỉ có tra cứu.

12. Trong phạm vi hiện tại, chỉ Số phương án thiết kế mới và Tra cứu thư viện mẫu là quyền dạng lượt có logic kiểm tra sử dụng, giữ/trừ/giải phóng lượt theo quy tắc riêng đã chốt. Các quyền lợi còn lại cấu hình bật/tắt theo gói để lưu và hiển thị; không triển khai kiểm tra quyền sử dụng, bộ đếm hoặc logic tính năng tương ứng. Không cấu hình mức tính năng cụ thể trong đợt này. Bật một quyền hiển thị không tự cấp thêm lượt tạo/tra cứu hoặc bắt đầu dịch vụ. Khả năng hỗ trợ dạng mức trong thiết kế chung không tạo nghĩa vụ triển khai tính năng phân mức hiện tại.

## Except

Không có luồng Admin tự tạo định nghĩa quyền lợi trong phạm vi đã chốt. Cách bổ sung hoặc thay đổi danh mục khi hệ thống có tính năng mới thuộc thiết kế và triển khai của tính năng đó.

## Notes

- Người dùng xác nhận ngày 25/09/2026: gói giám sát không dùng danh mục quyền lợi vì lịch và lượt giám sát đang vận hành offline; điều kiện ít nhất một quyền lợi ở khoản 7 chỉ áp dụng cho gói thiết kế.

- Quyết định mới nhất mở rộng cách cấu hình chỉ bật/tắt từ 3D sang mọi quyền lợi khác ngoài hai quyền dạng lượt. Tư vấn vẫn vận hành offline, nội dung mô tả tự do đã chốt được giữ; bật/tắt mục tư vấn không phải danh sách mức Online/Ưu tiên/1:1. Không tự nhập toàn bộ nội dung website thành danh mục bán. Xem STORY-SUB-002/AC-026 và ST-SUB-108.

- Quyết định mới thay phạm vi danh mục đúng ba quyền và việc kiểm tra 3D lúc Gen AI. STORY-SUB-002/AC-025 và ST-SUB-107 mô tả phạm vi hiện tại. Các ghi chú ba quyền và kiểm tra AI 3D bên dưới là lịch sử; tư vấn vẫn theo mô tả tự do đã chốt, không tự chuyển thành mức cố định.

- Người dùng chọn phương án 2 cho tư vấn offline: Admin nhập nội dung mô tả tự do trong gói; không có trường chọn mức tư vấn riêng hoặc danh sách mức cố định. Không chuyển nội dung mô tả thành entitlement, hạn mức hay logic phân quyền. Giữ nội dung tư vấn đã chốt cho kỳ đã mua theo BR-SUB-004; mô tả mới chỉ áp dụng lần mua mới.

- Người dùng xác nhận tư vấn Online, Ưu tiên và Chuyên gia 1:1 trên bảng gói là dịch vụ đang vận hành offline; cam kết tư vấn của gói không được quản lý trên nền tảng. Các mức này là mô tả dịch vụ, không phải quyền dạng mức để kiểm tra truy cập hoặc tính lượt trên nền tảng trong danh mục tạm thời. Không tự thêm lịch hẹn, bộ đếm tư vấn hoặc chức năng tư vấn gắn với gói. Yêu cầu tư vấn KTS miễn phí theo STORY-CONSULT-002 là kênh riêng, độc lập với gói và không thay cam kết tư vấn của gói (người dùng xác nhận ngày 25/09/2026). Không đồng nhất với trợ lý AI/chatbot hiển thị trên trang.

- **Lịch sử:** Người dùng từng đồng ý danh mục tạm thời ba quyền; STORY-SUB-002/AC-022 và [ST-SUB-104](../systemtest/ST-SUB-104.md) kiểm tra. Quyết định này đã được thay bởi STORY-SUB-002/AC-025; danh mục không còn giới hạn đúng ba quyền.

- Với tác vụ AI đã tiếp nhận trước khi đổi gói, quyền 3D được xác định theo bộ quyền lúc tiếp nhận theo BR-SUB-003 và BR-SUB-021. Gói mới không cắt hoặc bổ sung quyền cho tác vụ đang chạy; yêu cầu mới kiểm tra gói mới. Xem [ST-SUB-102](../systemtest/ST-SUB-102.md).

- Người dùng xác nhận Admin được chọn quyền dạng lượt đưa vào gói; không bắt buộc có cả tạo thiết kế và tra cứu. Gói không có bất kỳ quyền lợi nào chỉ được lưu nháp, không được Công bố. Điều kiện này không có nghĩa phải thêm cả hai quyền dạng lượt.
- Quy tắc này không cố định sẵn số gói hay giá trị quyền lợi của từng gói.
- [ST-SUB-063](../systemtest/ST-SUB-063.md) kiểm tra gói chỉ có một trong hai quyền và việc từ chối thao tác của quyền không được cấp.
- Đã chốt quyền tạo phối cảnh 3D chân thực dạng bật/tắt; dự toán nội thất và bố trí công năng không phân mức giữa các gói. Những tính năng khác, các mức và cách chia quyền cho từng gói còn cần chốt; mã kỹ thuật sẽ xác định trong TDD. Không coi toàn bộ bảng trên trang tham khảo là danh mục đã được duyệt.
- Quyền 3D không tạo thêm loại lượt mới; hệ thống vẫn chỉ có lượt tạo thiết kế và tra cứu mẫu. Người dùng xác nhận toàn bộ kết quả AI trả cùng lúc, phối cảnh 3D là một phần trong bộ kết quả theo quyền gói. Không tách 3D thành thao tác tạo riêng hoặc trả bổ sung sau một lần tạo đã báo thành công. Tiêu chí đủ kết quả và tính lượt theo [BR-SUB-003](BR-SUB-003.md). Quyền này không mở thêm luồng Gen AI trên dự án đã có kết quả thành công; quy tắc hiện tại theo [BR-SUB-017](BR-SUB-017.md) vẫn giữ nguyên.
- [ST-SUB-069](../systemtest/ST-SUB-069.md) kiểm tra quyền tạo phối cảnh 3D chân thực theo bản quyền lợi đã cấp. Quyền bật/tắt này dùng chung cho tháng/năm trong cùng bản gói theo [BR-SUB-015](BR-SUB-015.md).
- Tham chiếu [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-005](BR-SUB-005.md), [BR-SUB-004](BR-SUB-004.md) và [ST-SUB-019](../systemtest/ST-SUB-019.md).
- [ST-SUB-066](../systemtest/ST-SUB-066.md) kiểm tra lưu nháp gói chưa có quyền lợi và từ chối Công bố khi danh sách quyền lợi rỗng.
- [ST-SUB-071](../systemtest/ST-SUB-071.md) kiểm tra cùng mức chi tiết dự toán giữa các gói. Quyết định này không xác định hai dự án phải có cùng số tiền hoặc nội dung dự toán; kết quả vẫn phụ thuộc dữ liệu dự án. Người dùng đã hoãn tính năng AI gợi ý giảm chi phí/tối ưu ngân sách theo [nợ nghiệp vụ](../debt/ai-budget-optimization.md). Không đưa quyền này vào danh mục đang triển khai; dự toán nội thất vẫn giữ nguyên, không yêu cầu gợi ý giảm chi phí trong bộ kết quả hiện tại.
- [ST-SUB-072](../systemtest/ST-SUB-072.md) kiểm tra bố trí công năng không phân mức giữa các gói. Phân mức cơ bản/2D và 3D/nâng cao quan sát trên trang không còn là căn cứ chia quyền bố trí công năng. Quyền phối cảnh 3D chân thực vẫn là quyền riêng đã chốt, không đồng nhất với cách trang gọi mức bố trí công năng.
- Tính năng so sánh kết quả thiết kế đã hoãn theo [nợ nghiệp vụ](../debt/design-comparison.md). Không đưa quyền so sánh vào danh mục đang triển khai; khách vẫn xem từng kết quả riêng theo quyền truy cập hiện có.
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
