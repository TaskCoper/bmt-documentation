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

# BR-PROJ-011

## Rule Info

- **Name**: Chặng lỗi được chạy lại có giới hạn; phần hỏng trong một lần chạy được tự làm lại một lần.
- **Category**: Tạo dự toán — xử lý theo chặng
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng chốt trong hội thoại ngày 10–11/10/2026: chặng lỗi được chạy lại miễn phí, tối đa 3 lần mỗi chặng, Admin cấu hình được; quá giới hạn thì báo khách liên hệ hỗ trợ và Admin có nút đặt lại bộ đếm. Người dùng chốt ngày 12/10/2026: chỉ lỗi do kết quả AI với đầu vào của khách mới tính vào bộ đếm, lỗi hệ thống không tính; bộ đếm riêng từng chặng; đặt lại bộ đếm cần quyền vận hành riêng; lần chạy lại dùng lại phần đã xong kể cả khi Admin đã đổi model hoặc lời dặn; bản vẽ 2D hỏng được vẽ lại 1 lần; ảnh 3D lỗi được dựng lại 1 lần và lần chạy lại chỉ dựng ảnh còn thiếu; tệp hồ sơ lỗi được dựng lại 1 lần.

## Statement

Khi một chặng lỗi, chủ bản dự toán được chạy lại chặng đó cho tới khi đạt giới hạn số lần lỗi của chặng; mặc định 3 lần và Admin cấu hình được riêng cho từng chặng. Chỉ lần chạy lỗi vì kết quả AI không dùng được với đầu vào của khách mới bị đếm; lỗi do hệ thống không bị đếm. Chạy lại các chặng sau 2D không tính lượt. Trong một lần chạy, bản vẽ 2D, ảnh 3D hoặc tệp hồ sơ bị hỏng được hệ thống tự làm lại một lần trước khi kết luận lần chạy lỗi.

## When

Một lần chạy chặng kết thúc lỗi, khách yêu cầu chạy lại chặng lỗi, Admin đặt lại bộ đếm lỗi hoặc đổi giới hạn, hoặc một phần kết quả trong lần chạy không đạt kiểm tra.

## Then

1. Mỗi chặng của một bản dự toán có bộ đếm lỗi riêng. Lỗi ở chặng này không làm giảm số lần chạy lại của chặng khác.
   - **Tính một lần vào bộ đếm** khi lần chạy lỗi vì kết quả AI không dùng được với đầu vào của khách: AI vẫn trả thiếu hoặc sai cấu trúc sau lượt hỏi lại, AI từ chối xử lý nội dung khách gửi, bản vẽ 2D hoặc ảnh 3D vẫn hỏng sau lần làm lại ở khoản 7.
   - **Không tính** khi lần chạy lỗi vì hệ thống: dịch vụ AI lỗi hoặc không phản hồi sau khi đã thử lại, tài khoản AI hết tiền hoặc khóa truy cập bị từ chối, quá hạn chờ theo BR-PROJ-012 (kể cả do phải chờ trong hàng), kết quả AI bị cắt ngang vì vượt giới hạn độ dài đầu ra, lỗi lưu tệp, lỗi dựng tệp hồ sơ, lỗi nội bộ hoặc phiên bản xử lý đã ngừng dùng.
   - Khi kết quả AI bị cắt ngang vì vượt giới hạn độ dài đầu ra, lần chạy lỗi và hệ thống gửi cảnh báo vận hành (Discord) để Admin nâng giới hạn.

   Ví dụ: dịch vụ AI gián đoạn 2 tiếng, khách bấm chạy 2D ba lần và cả ba lần đều lỗi vì dịch vụ AI: chặng 2D vẫn còn đủ 3 lần.
2. Khi bộ đếm của chặng đã bằng hoặc vượt giới hạn đang cấu hình, từ chối yêu cầu chạy lại chặng đó và báo khách liên hệ hỗ trợ. Kết quả của các chặng đã thành công giữ nguyên.
3. Người có quyền vận hành luồng AI (tách khỏi quyền sửa cấu hình, STORY-PROJ-012) được đặt lại bộ đếm lỗi của một chặng trên một bản dự toán. Sau khi đặt lại, khách chạy lại được tối đa bằng giới hạn đang cấu hình. Đặt lại không tự chạy chặng, không đổi kết quả đã có và không cấp hay hoàn lượt.
4. Admin cấu hình giới hạn số lần lỗi cho từng chặng; giá trị mới áp dụng từ lần kiểm tra kế tiếp. Nếu giới hạn mới nhỏ hơn số lần lỗi đã có, chặng bị chặn chạy lại cho tới khi Admin đặt lại bộ đếm.
5. Chạy lại chặng 2D: sau khi 2D lỗi, đầu vào được mở sửa theo BR-PROJ-005. Lần chạy lại kiểm tra lại đầy đủ quyền, gói, lượt và đầu vào rồi giữ một lượt mới theo BR-SUB-003, vì lượt của lần lỗi đã được hoàn.
6. Chạy lại chặng Dự toán, Phối cảnh 3D hoặc Hồ sơ: không kiểm tra gói hay lượt, không giữ, trừ hay hoàn lượt.
7. Tự làm lại trong một lần chạy:
   - Chặng 2D: một bản vẽ được vẽ lại 1 lần khi AI không trả được ảnh, hoặc khi viền ngoài của nhà trên bản vẽ lệch tỷ lệ quá ngưỡng so với lô (mặc định 5%, đo bằng code, không gọi AI). Sau lần vẽ lại: AI vẫn không trả được ảnh thì lần chạy 2D lỗi; viền vẫn lệch thì giữ bản có độ lệch nhỏ hơn và gắn cờ nội bộ, lần chạy không lỗi vì lý do này. Admin có cờ tắt việc vẽ lại vì viền lệch; khi tắt, viền lệch chỉ được ghi lại như mặc định của app demo.
   - Chặng 3D: ảnh phối cảnh lỗi thì được dựng lại 1 lần; vẫn lỗi thì lần chạy 3D lỗi.
   - Chặng Hồ sơ: dựng tệp lỗi thì được dựng lại 1 lần; vẫn lỗi thì lần chạy Hồ sơ lỗi.

   Việc tự làm lại không tính vào bộ đếm lỗi và không kéo dài hạn chờ của lần chạy.
8. Chạy lại chặng 3D chỉ dựng các ảnh chưa có; ảnh đã dựng và lưu thành công ở lần trước được giữ và dùng tiếp. Phần đã xong vẫn được dùng lại khi Admin đã đổi model, lời dặn hoặc cấu hình AI giữa hai lần chạy; cấu hình mới chỉ áp dụng cho phần còn thiếu. Mỗi ảnh và tệp ghi lại model và phiên bản đã dùng để tạo ra nó (BR-PROJ-014).

## Except

- Gián đoạn kỹ thuật mà hệ thống tự tiếp tục được trong hạn chờ, chẳng hạn máy chủ khởi động lại giữa chừng, không làm lần chạy lỗi và không tính vào bộ đếm.
- Yêu cầu chạy lại bị từ chối trước khi tiếp nhận (chưa đủ điều kiện, đang có chặng chạy, đã đạt giới hạn, hệ thống đang quá tải hoặc dịch vụ AI đang tạm ngắt theo BR-PROJ-012) không tính vào bộ đếm.
- Lỗi hệ thống không bị giới hạn số lần chạy lại. Chi phí AI của các lần này được kiểm soát bằng việc tạm ngắt nhận lần chạy mới và cảnh báo chi phí theo BR-PROJ-012.

## Notes

- **Đã xác nhận (12/10/2026):** lỗi hệ thống không tính vào bộ đếm, chỉ tính lỗi do kết quả AI với đầu vào của khách; bộ đếm và giới hạn riêng từng chặng; đặt lại bộ đếm dùng quyền vận hành riêng `estimate.pipeline.operate`; phần đã xong được dùng lại dù Admin đổi model hoặc lời dặn giữa hai lần chạy.
- **Đã xác nhận (12/10/2026, lượt sau):** Người dùng chốt kết quả AI bị cắt ngang vì vượt giới hạn độ dài đầu ra là lỗi hệ thống: lần chạy lỗi nhưng không tính vào bộ đếm của khách, và hệ thống gửi cảnh báo vận hành (Discord) để Admin nâng giới hạn.
- **Đã xác nhận (11/10/2026, lượt sau):** bản vẽ 2D "hỏng" gồm AI không trả được ảnh và viền nhà lệch tỷ lệ quá ngưỡng; có cờ tắt phần viền lệch. Viền vẫn lệch sau lần vẽ lại thì giữ bản tốt hơn, không làm chặng lỗi (theo cách app demo giữ bản tốt hơn); người dùng xác nhận khi chốt TDD-PROJ-007 ngày 11/10/2026.
- **Đã xác nhận (11/10/2026):** giới hạn 3 lần mỗi chặng, Admin cấu hình được; quá giới hạn báo liên hệ hỗ trợ; Admin đặt lại bộ đếm; tự làm lại 1 lần cho bản vẽ 2D, ảnh 3D và tệp hồ sơ; chạy lại 3D chỉ dựng ảnh còn thiếu.
- Ví dụ: chặng 3D cần 8 ảnh. Lần chạy đầu dựng được 6 ảnh; ảnh thứ 7 lỗi cả 2 lần thử nên lần chạy lỗi (bộ đếm 3D = 1). Khách bấm chạy lại: hệ thống chỉ dựng ảnh 7 và 8.
- Quyền đặt lại bộ đếm được thiết kế cùng màn quản trị ở STORY-PROJ-012. Cách đếm và lưu lần chạy thuộc TDD-PROJ-006.
- Reviewer và Approver là Tân Trần; việc ghi tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kết quả kiểm thử.
