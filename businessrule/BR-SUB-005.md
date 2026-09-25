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


# BR-SUB-005

## Rule Info

- **Name**: Hỗ trợ ba dạng quyền lợi trong gói subscription.
- **Category**: Subscription và entitlement
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Source**: Người dùng chọn các gói khác cả số lượt lẫn tính năng. Hỗ trợ quyền bật/tắt, lượt và mức tính năng; mức cao bao gồm mức thấp của cùng quyền. Hạn mức ít nhất 1 hoặc không giới hạn; bắt đầu thao tác mới cần gói còn hiệu lực. Quyền bật/tắt chưa thêm hoặc đã tắt không cho dùng. Quyền dạng mức chưa cấu hình không cho dùng, không tự cấp mức thấp nhất. Danh mục tính năng và cách chia cho từng gói còn cần chốt.

## Statement

**Phạm vi cấu hình hiện tại:** hai quyền tạo mới/tra cứu dạng lượt; mọi quyền lợi còn lại bật/tắt để hiển thị. Các mô tả dạng mức và kiểm tra sử dụng tính năng bên dưới là khả năng thiết kế chung, chưa phải yêu cầu triển khai logic ngoài hai quyền dạng lượt trong đợt này.

**Phạm vi mới nhất:** chỉ quyền tạo thiết kế mới và tra cứu mẫu được triển khai logic sử dụng/tính lượt trong đợt này. Quyền 3D vẫn cấu hình bật/tắt và hiển thị, chưa điều khiển AI hoặc kiểm tra quyền tạo 3D. Các mô tả xử lý 3D bên dưới là thiết kế cho giai đoạn sau, không phải tiêu chí nghiệm thu hiện tại. Không suy ra AI hiện phải luôn trả hoặc luôn bỏ 3D.

Các gói thiết kế khác nhau về cả số lượt và tính năng được dùng. Gói subscription hỗ trợ quyền lợi dạng bật/tắt tính năng, hạn mức lượt và mức tính năng. Quyền lợi dạng lượt được đưa vào gói phải có hạn mức ít nhất 1 lượt hoặc chọn không giới hạn; không chấp nhận hạn mức 0. Hệ thống lưu và kiểm tra quyền lợi theo đúng dạng được cấu hình. Quyền bật/tắt chưa được thêm vào gói hoặc được đặt là tắt không cho phép sử dụng tính năng tương ứng; chỉ quyền đã thêm và bật mới đáp ứng điều kiện về quyền này. Quyền dạng mức chưa được cấu hình không cho phép dùng tính năng tương ứng; Admin phải chọn rõ mức được cấp. Với cùng một quyền lợi dạng mức, mức cao bao gồm các mức thấp hơn; mức thấp không đáp ứng yêu cầu của mức cao.

## When

Admin cấu hình quyền lợi của gói hoặc hệ thống kiểm tra quyền lợi đã cấp cho tài khoản.

## Then

1. Dạng bật/tắt chỉ cho phép dùng tính năng tương ứng khi quyền đã được thêm và bật trong bản quyền lợi áp dụng cho tài khoản. Quyền chưa được thêm hoặc đã tắt đều không cho phép sử dụng, kể cả khi khách gửi yêu cầu trực tiếp. Không tự coi thiếu cấu hình là bật. Quyền đã bật chỉ đáp ứng điều kiện của chính quyền đó; tài khoản vẫn phải đáp ứng hiệu lực gói và các điều kiện khác của thao tác.
2. Dạng hạn mức lượt cho phép Admin chọn hạn mức ít nhất 1 lượt hoặc không giới hạn. Không lưu cấu hình hoặc công bố quyền dạng lượt có hạn mức nhỏ hơn 1, kể cả khi gửi yêu cầu trực tiếp. Quy tắc áp dụng riêng cho cấu hình tháng và năm. Với số lượt cụ thể, hệ thống kiểm tra số lượt còn có thể sử dụng theo các quy tắc đã chốt. Với không giới hạn, không từ chối vì đã dùng hết số lượt; việc bắt đầu thao tác mới vẫn cần subscription còn hiệu lực và đáp ứng các quyền liên quan.
3. Dạng mức tính năng xác định mức được cấp, chẳng hạn cơ bản hoặc nâng cao. Nếu bản quyền lợi đang áp dụng chưa có cấu hình mức cho tính năng đó, từ chối sử dụng, kể cả yêu cầu gửi trực tiếp; không tự cấp mức thấp nhất. Admin phải thêm quyền và chọn rõ mức. Mức được cấp đáp ứng yêu cầu ở chính mức đó và các mức thấp hơn theo thứ tự của cùng quyền lợi; vẫn phải đáp ứng các điều kiện khác của thao tác. Không tự coi tên mức là một số lượt.
4. Không giữ hoặc trừ lượt chỉ vì kiểm tra quyền bật/tắt hay mức tính năng. Một thao tác có cần kiểm tra thêm quyền lợi hạn mức riêng hay không còn cần xác định theo từng tính năng.
5. Cả ba dạng tuân theo quy tắc lưu nháp, công bố và giữ nguyên quyền lợi kỳ hiện tại.
6. Admin có thể cấu hình quyền bật/tắt và mức tính năng khác nhau giữa các gói, ngoài việc đặt hạn mức lượt riêng. Quyền được dùng theo cấu hình, không tự suy ra từ tên hoặc giá gói. Trong cùng một bản gói thiết kế, lựa chọn tháng/năm vẫn dùng chung quyền bật/tắt và mức tính năng theo BR-SUB-015.

## Except

Tạo thiết kế cần quyền tạo và lượt sẵn dùng; đợt này chưa kiểm tra quyền 3D khi Gen AI theo BR-SUB-008 khoản 9. Mở chi tiết mẫu cần quyền tra cứu và lượt theo BR-SUB-017. Chỉ làm rõ cách kết hợp bổ sung nếu danh mục còn lại thực sự có thao tác cần nhiều quyền. Không giới hạn lượt không đồng nghĩa với quyền sử dụng sau khi subscription hết hạn.

## Notes

- Quản lý số dư và giữ/trừ lượt giám sát trên nền tảng đã hoãn theo [nợ nghiệp vụ](../debt/supervision-offline.md). Chế độ hữu hạn/không giới hạn đã chốt không tạo nghĩa vụ triển khai bộ đếm giám sát trong đợt này. Quy tắc hiệu lực theo thời gian áp dụng cho thiết kế; giám sát theo [BR-SUB-011](BR-SUB-011.md).
- Quyền lợi được chọn từ danh mục hệ thống theo [BR-SUB-008](BR-SUB-008.md); Admin cấu hình giá trị cho gói.
- Dự toán nội thất và bố trí công năng đều dùng cùng mức chi tiết giữa các gói theo [BR-SUB-008](BR-SUB-008.md); không dùng hai phần này làm ví dụ về quyền phân mức của BMT. Khả năng hỗ trợ quyền dạng mức vẫn giữ cho tính năng khác nếu được chốt.
- Đã chốt cần phân biệt tính năng giữa các gói; chưa chốt tên từng tính năng, mức và gói nào có quyền nào. Tên quyền lợi, số lượt và tên mức trong ví dụ chưa phải danh mục chính thức của BMT. [ST-SUB-009](../systemtest/ST-SUB-009.md) kiểm tra cấu hình khác nhau giữa hai gói bằng dữ liệu minh họa. Đã rút khỏi nghiệm thu ngày 25/09/2026 vì STORY-SUB-002/AC-001 không nghiệm thu đợt này theo BR-SUB-008 khoản 12.
- Ví dụ cơ bản < nâng cao: được cấp nâng cao thì dùng được cả hai mức; được cấp cơ bản thì không dùng được tính năng yêu cầu nâng cao. Chỉ so sánh mức trong cùng quyền lợi; không suy ra quyền sử dụng một quyền lợi khác. Tên mức và thứ tự cụ thể trong danh mục còn cần xác định.
- Tham chiếu [STORY-SUB-002](../userstory/STORY-SUB-002.md), [BR-SUB-001](BR-SUB-001.md), [BR-SUB-002](BR-SUB-002.md), [BR-SUB-003](BR-SUB-003.md), [BR-SUB-004](BR-SUB-004.md) và [ST-SUB-009](../systemtest/ST-SUB-009.md).
- [ST-SUB-010](../systemtest/ST-SUB-010.md) và [ST-SUB-011](../systemtest/ST-SUB-011.md) kiểm tra quyền theo thứ tự mức. Đã rút khỏi nghiệm thu ngày 25/09/2026 vì STORY-SUB-001/AC-009 và AC-010 không nghiệm thu đợt này theo BR-SUB-008 khoản 12.
- Không giới hạn là một lựa chọn riêng; không tự quy ước số 0 hoặc số âm có nghĩa là không giới hạn. Cách lưu dữ liệu và ghi nhận số lượt sử dụng sẽ được thiết kế trong TDD.
- [ST-SUB-017](../systemtest/ST-SUB-017.md) và [ST-SUB-018](../systemtest/ST-SUB-018.md) kiểm tra cấu hình và sử dụng quyền không giới hạn lượt.
- Quy tắc tối thiểu 1 áp dụng cho hạn mức Admin cấu hình, không áp dụng cho số lượt còn lại sau sử dụng. Số dư về 0 do đã dùng/giữ hết lượt là hợp lệ. Không giới hạn vẫn là lựa chọn riêng, không biểu diễn bằng hạn mức 0.
- Admin được chọn quyền tạo mới, tra cứu hoặc cả hai vào gói theo [BR-SUB-008](BR-SUB-008.md). Quyền dạng lượt không được đưa vào gói không cấp quyền sử dụng tương ứng; không tự thêm hạn mức mặc định.
- [ST-SUB-062](../systemtest/ST-SUB-062.md) kiểm tra hạn mức 0 bị từ chối, hạn mức 1 và lựa chọn không giới hạn hợp lệ khi các điều kiện khác đáp ứng.
- [ST-SUB-067](../systemtest/ST-SUB-067.md) kiểm tra ba trường hợp quyền bật/tắt: chưa thêm, đã tắt và đã bật. Sửa bản nháp hoặc Công bố không đổi quyền của kỳ đang dùng; kiểm tra theo bản đã cấp, không theo bản nháp mới nhất. Đã rút khỏi nghiệm thu ngày 25/09/2026 vì STORY-SUB-002/AC-015 không nghiệm thu đợt này theo BR-SUB-008 khoản 12.
- [ST-SUB-068](../systemtest/ST-SUB-068.md) kiểm tra thiếu cấu hình mức không được tự cấp mức thấp nhất. Đã rút khỏi nghiệm thu ngày 25/09/2026 vì STORY-SUB-002/AC-016 không nghiệm thu đợt này theo BR-SUB-008 khoản 12. Ví dụ xem/xoay mô hình 3D trong hội thoại chỉ để giải thích, không xác nhận các mức xem/xoay 3D của BMT. Quyền tạo ảnh phối cảnh 3D chân thực đã được xác nhận riêng ở dạng bật/tắt theo [BR-SUB-008](BR-SUB-008.md).
- Bản nháp còn thiếu metadata; chưa được phê duyệt.
