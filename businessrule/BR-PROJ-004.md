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

# BR-PROJ-004

## Rule Info

- **Name**: Loại công trình do Admin quản lý, với cấu hình tầng/tum và hai nhóm phong cách riêng.
- **Category**: Tạo dự toán — danh mục và thông tin đầu vào
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Người dùng bổ sung ngày 2026-09-20 và xác nhận: Admin thêm loại công trình; bật/tắt riêng tầng/tum và cấu hình số tầng; quản lý hai danh mục phong cách với tên, ảnh, gán theo loại. Cấu hình phong cách chỉ điều khiển lựa chọn; AI vẫn trả đủ kết quả. Thay quy tắc danh mục cố định và phong cách gộp theo trang mẫu trước đó.

## Statement

Admin được thêm loại công trình và cấu hình theo từng loại việc chọn số tầng, chọn có tum hay không. Phong cách kiến trúc và phong cách nội thất là hai phần riêng. Admin cấu hình việc cho khách chọn phong cách kiến trúc và phong cách nội thất theo loại công trình; không dùng một trường phong cách chung cho cả hai. Cấu hình này chỉ điều khiển lựa chọn đầu vào; AI vẫn trả đầy đủ kết quả.

## When

Admin quản lý loại công trình và cấu hình của loại đó; khách nhập thông tin bản dự toán hoặc hệ thống kiểm tra đầu vào trước khi chạy chặng 2D.

## Then

1. Cho Admin thêm loại công trình ngoài năm loại đã có trên trang mẫu. Không giới hạn danh mục nghiệp vụ ở đúng năm loại.
2. Admin bật/tắt riêng việc cho chọn số tầng và việc cho chọn Có tum/Không tum theo loại công trình. Khi bật chọn tầng, Admin cấu hình các số tầng được chọn cho loại đó; khách chọn trong danh sách áp dụng. Phân biệt cấu hình cho phép chọn tum với giá trị có/không tum mà khách chọn cho bản dự toán.
3. Tách thông tin phong cách kiến trúc và phong cách nội thất. Admin được thêm/sửa cả hai danh mục, gồm tên, ảnh minh họa và gán các phong cách được chọn cho từng loại công trình. Khách chọn trong danh mục đúng nhóm và đúng loại. Không tự sao chép một giá trị phong cách chung thành cả hai lựa chọn.
4. Admin cấu hình theo từng loại công trình việc cho chọn phong cách kiến trúc và phong cách nội thất. Khi một nhóm lựa chọn không được bật, khách không phải chọn phong cách cho nhóm đó; AI vẫn trả đầy đủ kết quả, không loại phần kiến trúc hoặc nội thất vì nhóm lựa chọn bị tắt.
5. Địa chỉ vẫn gồm tỉnh/thành phố, xã/phường và địa chỉ chi tiết. Xã/phường phải thuộc tỉnh/thành đã chọn. Bản dự toán lưu thêm kinh độ và vĩ độ do frontend lấy từ dịch vụ bản đồ; tạo nhanh được để trống cả cặp theo BR-PROJ-003 khoản 10, nhưng trước khi chạy chặng 2D phải có đủ tọa độ hợp lệ.
6. Gói hoàn thiện và nội thất vẫn gồm Cơ bản, Tiêu chuẩn và VIP. Đây là đầu vào thiết kế, không phải gói đăng ký cấp quyền hoặc lượt sử dụng.
7. Trước khi chạy chặng 2D, yêu cầu đủ địa chỉ, loại công trình và gói hoàn thiện/nội thất; lựa chọn phong cách, số tầng và tum cần khớp cấu hình áp dụng. Mỗi nhóm phong cách được bật phải có đúng một lựa chọn hợp lệ trước khi chạy chặng 2D; bật cả hai nhóm thì chọn một phong cách kiến trúc và một phong cách nội thất. Nhóm bị tắt không yêu cầu khách nhập phong cách.
8. Cấu hình danh mục không thay thế quyền sở hữu bản dự toán, điều kiện gói/lượt hoặc quy tắc khóa đầu vào khi chặng đang chạy.
9. Bản dự toán đã tạo giữ cấu hình loại công trình và phong cách đã áp dụng cho bản dự toán đó. Khi Admin sửa cấu hình hoặc danh mục phong cách, thay đổi chỉ áp dụng cho bản dự toán mới; không làm mất lựa chọn hoặc thay điều kiện đầu vào của bản dự toán cũ, kể cả bản nháp.
10. Khi mở lại, lưu tiếp, chạy hoặc chạy lại chặng 2D trên bản dự toán cũ, kiểm tra các lựa chọn theo cấu hình của bản dự toán đó. Các điều kiện quyền, gói/lượt và khóa sửa vẫn được kiểm tra theo quy tắc hiện hành; giữ cấu hình danh mục không có nghĩa giữ quyền hoặc lượt cũ.

11. Khi khách đổi loại công trình trong bản nháp, giữ các lựa chọn phong cách kiến trúc, phong cách nội thất, số tầng và tum còn hợp lệ với loại mới theo cấu hình áp dụng cho bản dự toán. Xóa lựa chọn không còn phù hợp, gồm giá trị của trường không áp dụng cho loại mới; yêu cầu chọn lại phần bắt buộc trước khi chạy chặng 2D. Không tự chuyển phong cách giữa hai nhóm. Giữ nguyên diện tích, địa chỉ, ảnh và mô tả.
12. Khi khách đổi tỉnh/thành, xóa xã/phường đã chọn và yêu cầu chọn lại xã/phường thuộc tỉnh/thành mới trước khi chạy chặng 2D. Giữ địa chỉ chi tiết để khách tự sửa; không tự xóa hoặc viết lại địa chỉ chi tiết. Lần lưu thay đổi địa chỉ phải gửi đủ cặp tọa độ do frontend lấy lại từ bản đồ theo khoản 18, kể cả khi bản nháp chưa chọn lại xã/phường.

13. Khi Admin bật chọn tầng, danh sách số tầng phải có ít nhất một lựa chọn. Khi bật nhóm phong cách kiến trúc hoặc nội thất, nhóm đó phải được gán ít nhất một phong cách thuộc đúng nhóm cho loại công trình. Nếu bất kỳ danh sách được bật nào trống, từ chối lưu cấu hình và chỉ rõ danh sách cần bổ sung; không lưu cấu hình không hợp lệ hoặc thay cấu hình đã lưu trước đó. Điều kiện này áp dụng cả khi tạo cấu hình và khi sửa/xóa lựa chọn cuối cùng trong danh sách đang bật.

14. Bản dự toán chỉ được chọn các loại công trình thuộc danh mục đã áp dụng khi tạo bản dự toán đó. Nếu Admin thêm loại công trình sau thời điểm này, bản dự toán cũ không được đổi sang loại mới thêm; muốn sử dụng loại đó, khách phải tạo bản dự toán mới. Việc đổi loại trong bản nháp không làm bản dự toán chuyển sang danh mục hoặc cấu hình mới nhất.

15. Mỗi phong cách trong danh mục kiến trúc hoặc nội thất bắt buộc có một ảnh minh họa JPG, PNG hoặc WebP, dung lượng tối đa 5 MiB (5.242.880 byte) cho upload mới theo BR-MEDIA-001. Không chấp nhận lưu phong cách thiếu ảnh hợp lệ, ảnh ngoài định dạng hoặc vượt giới hạn. Quy tắc này dành cho ảnh danh mục của Admin, tách khỏi ảnh đầu vào bản dự toán.

16. Danh mục phong cách kiến trúc và nội thất ban đầu do Admin tự nhập tên, ảnh minh họa và gán theo loại công trình trước khi khách sử dụng. Không chuẩn bị sẵn hoặc tự phân loại danh sách phong cách gộp trên trang mẫu thành dữ liệu khởi tạo. Các cấu hình được lưu vẫn phải đáp ứng điều kiện có ít nhất một lựa chọn cho mỗi nhóm đang bật theo khoản 13.

17. Tên loại công trình và tên phong cách kiến trúc/nội thất bắt buộc có nội dung sau khi bỏ khoảng trắng đầu/cuối, tối đa 200 ký tự. Không tự cắt ngắn tên vượt giới hạn. Cho phép trùng tên; tên không thay thế mã định danh của từng mục.

18. Khi khách thay đổi tỉnh/thành, xã/phường hoặc địa chỉ chi tiết của bản dự toán, frontend gọi dịch vụ bản đồ lấy lại kinh độ và vĩ độ cho địa chỉ đang sửa, rồi gửi cả hai trong cùng yêu cầu lưu. Backend chỉ lưu khi có đủ cặp tọa độ; lưu địa chỉ và tọa độ cùng nhau. Nếu chưa lấy được đủ tọa độ thì frontend chưa gửi lần lưu thay đổi địa chỉ; yêu cầu trực tiếp thiếu tọa độ bị từ chối và giữ nguyên toàn bộ dữ liệu đã lưu. Giá trị tọa độ bản đồ trả về có thể trùng với giá trị cũ; yêu cầu là lấy lại và gửi đủ cặp, không bắt buộc hai con số phải thay đổi.

19. Mỗi loại công trình gắn đúng một mã gốc trong năm mã của app demo `design-estimate`: `nha-pho`, `villa-biet-thu`, `nha-mai`, `nha-vuon-cap4`, `can-ho`. Loại do Admin thêm phải chọn một mã gốc; luồng AI dùng mã gốc cho thuật toán phương án và khung giá demo, còn luật lô đất và ma trận mái được chọn bằng ID theo khoản 24. Loại chưa gắn mã gốc được lưu nhưng bản dự toán dùng loại đó không chạy được chặng 2D, kèm lý do danh mục chưa đủ cấu hình.

20. Mỗi phong cách kiến trúc gắn một mã phong cách gốc của app demo, thuộc đúng mã gốc của loại công trình được gán. Phong cách chưa gắn mã gốc thì bản dự toán chọn nó không chạy được chặng 2D, kèm lý do như khoản 19.

21. Mỗi phong cách nội thất có ô “Mô tả cho AI” bắt buộc, mô tả bảng màu, vật liệu và đồ đạc đặc trưng bằng lời. Luồng AI dùng ô này để vẽ bản vẽ nội thất và dựng ảnh phòng. Mô tả có nội dung sau khi bỏ khoảng trắng đầu/cuối, tối đa 1.000 ký tự; thiếu mô tả hoặc vượt giới hạn thì từ chối lưu phong cách nội thất, không tự cắt ngắn. Nhóm dự án viết sẵn mô tả cho các phong cách nội thất đang có trước khi bật luồng bốn chặng.

22. Số tầng được chọn nằm trong khoảng 1 đến 4, cộng tum khi loại cho phép. Admin cấu hình danh sách số tầng của từng loại trong khoảng này; lưu số tầng ngoài khoảng bị từ chối. Loại có mã gốc `can-ho` luôn là một sàn, không chọn tầng hay tum.

23. Gói hoàn thiện và nội thất mặc định là Tiêu chuẩn khi khách chưa chọn. Gói quyết định mức hoàn thiện, mức đồ đạc trên bản vẽ nội thất và khung giá của chặng Dự toán.

24. Đặc điểm theo ID loại công trình (lô mặc định, số tầng tối đa, có mái, tum, phần thô hay không, ngưỡng lô đất, gara, phòng ngủ), kiểu mái theo cặp ID loại công trình và ID phong cách kiến trúc, lời dặn theo gói và theo miền được lưu trong bộ quy tắc AI theo BR-PROJ-019, không viết cố định theo tên loại công trình.

## Except

Bản nháp được phép chưa chọn đủ thông tin theo BR-PROJ-003, kể cả chưa có tọa độ ở bước tạo nhanh; khi đổi địa chỉ vẫn phải gửi đủ kinh độ và vĩ độ. Việc giữ cấu hình cũ không cho phép sửa đầu vào khi chặng đang chạy hoặc thay thế kết quả thành công; BR-PROJ-005 vẫn áp dụng.

## Notes

- **Bổ sung đã xác nhận ngày 13/10/2026:** trước khi khách chọn loại, hệ thống tự kiểm điều kiện cấu hình AI theo BR-PROJ-019 khoản 9–11. Loại thiếu cấu hình vẫn hiện nhưng khóa chọn, nhãn Chưa hỗ trợ; thay quy định chỉ phát hiện thiếu cấu hình khi chạy 2D. Cổng tạo dự toán vẫn do người vận hành bật; không thay quyền/gói. Danh mục đã ghim, dữ liệu bản nháp và kết quả cũ giữ nguyên.


- **Cập nhật ngày 11/10/2026 theo luồng bốn chặng (người dùng đã chốt):** mã gốc cho loại công trình và phong cách kiến trúc (khoản 19, 20); ô “Mô tả cho AI” bắt buộc cho phong cách nội thất (khoản 21); giữ hai nhóm phong cách — kiến trúc quyết định mái và ngoại thất, nội thất quyết định bảng màu, vật liệu và ảnh phòng; số tầng 1–4 cộng tum, căn hộ luôn một sàn (khoản 22); gói mặc định Tiêu chuẩn (khoản 23). “Gửi AI” đổi thành “chạy chặng 2D” (BR-PROJ-010).
- **Đã xác nhận (11/10/2026, khi duyệt TDD):** bản dự toán tạo trước khi Admin gắn mã gốc giữ revision danh mục cũ theo khoản 9, nên bị chặn tạo 2D với lý do danh mục chưa đủ cấu hình; khách tạo bản dự toán mới. Không sửa revision danh mục cũ.
- **Đã xác nhận (11/10/2026):** loại hoặc phong cách chưa gắn mã gốc vẫn lưu được nhưng chặn chạy 2D (khoản 19, 20); ô “Mô tả cho AI” tối đa 1.000 ký tự (khoản 21).
- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Phần tọa độ bổ sung ngày 01/10/2026 đã được người dùng chốt cùng STORY-PROJ-001 và BR-PROJ-003: yêu cầu lấy tọa độ trước khi tạo đã được thay bằng tạo nhanh chỉ cần tên ở BR-PROJ-003; đổi địa chỉ phải gửi lại đủ cặp tọa độ; chưa có dữ liệu thật cần giữ. System Test và TDD đã được cập nhật; backend đã lên `develop` tại commit `5e396fc`. Phạm vi kiểm thử và phần chưa kiểm được ghi trong [bàn giao tọa độ](../discovery/estimate-coordinates-implementation.md). Xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Hiện trạng trang mẫu: Nhà phố, Villa/Biệt thự, Nhà mái, Nhà vườn/Nhà cấp 4 có chọn tầng và tum; Căn hộ không có. Đây là cấu hình đang quan sát, không phải ngoại lệ phải viết cố định theo tên Căn hộ cho mọi phiên bản.
- Danh sách tầng trên trang mẫu là Trệt đến Trệt + 4 lầu. Người dùng đã chốt Admin được cấu hình các số tầng theo loại; danh sách mẫu không còn là giới hạn cố định. Chưa tự đặt số tầng tối đa nghiệp vụ mới.
- Trang mẫu gộp kiến trúc và nội thất, được người dùng xác nhận là chưa chính xác. Bảng phong cách gộp trong phiên bản nháp trước không còn là danh mục triển khai. Admin tự nhập và gán danh mục ban đầu; không tự phân loại Hiện đại, Wabi-sabi, Tối giản hoặc các mục khác từ giao diện mẫu vào hai danh mục mới.
- Ảnh danh mục và ảnh đầu vào dự toán có vai trò khác nhau; upload mới cùng áp dụng JPG/PNG/WebP tối đa 5 MiB theo [BR-MEDIA-001](BR-MEDIA-001.md). Ảnh cũ trong revision còn được giữ vẫn là nơi sử dụng theo [BR-MEDIA-002](BR-MEDIA-002.md).
- Tầng thực thi hiện tại: frontend PUT theo URL BMT cấp; backend kiểm bytes tại complete theo [TDD-MEDIA-001](../tdd/TDD-MEDIA-001.md). API danh mục vẫn nhận URL https thuộc kho đã cấu hình và lưu reference cùng transaction; sao revision giữ nơi sử dụng của mọi ảnh được sao. Quyết định MEDIA thay phần chỉ frontend kiểm định dạng và dung lượng trước đây.
- Người dùng đã xác nhận cấu hình chỉ điều khiển lựa chọn phong cách, AI vẫn trả đủ kết quả. Không dùng cấu hình này để cắt phần thiết kế, nhóm chi phí dự toán hoặc nội dung hồ sơ. Danh sách đầu ra cụ thể vẫn chờ hợp đồng AI; quy tắc thành công và tính lượt không thay đổi.
- Khi khách không được chọn một nhóm phong cách, cách biểu diễn phần không có lựa chọn trong prompt được xác định trong TDD-PROJ-002; chưa tự chọn phong cách mặc định. Chưa tự chọn phong cách mặc định hoặc sao chép từ nhóm còn lại.
- Người dùng đã chốt giữ cấu hình của bản dự toán cũ, chỉ áp dụng thay đổi cho bản dự toán mới. Phương án lưu và xác định phiên bản cấu hình thuộc thiết kế kỹ thuật; chưa chốt bảng dữ liệu hoặc cách sao lưu danh mục.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về nhóm lựa chọn bị tắt: tắt chọn tầng, chọn tum hoặc một nhóm phong cách không xóa danh sách đã cấu hình; nhóm tắt được phép có danh sách rỗng hoặc có dữ liệu, để khi Admin bật lại thì danh sách cũ hiện ra. Khách vẫn chỉ thấy và chọn nhóm đang bật theo khoản 2 và 4; dữ liệu của nhóm tắt không được coi là lựa chọn hợp lệ khi lưu, khi đổi loại theo khoản 11 và khi kiểm trước khi chạy chặng 2D theo khoản 7. Khoản 13 giữ nguyên: nhóm bật phải có ít nhất một lựa chọn. Tum chỉ có cờ bật/tắt, không có danh sách.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về mở tính năng: người vận hành bật cổng tạo bản dự toán cho khách sau khi Admin xác nhận danh mục đủ theo khoản 16; hệ thống không tự kiểm danh mục đã đủ hay chưa.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về nguồn địa chỉ của khoản 5 và 12: dùng dịch vụ bên thứ ba provinces.open-api.vn theo địa giới mới (34 tỉnh/thành, hai cấp tỉnh–xã). Khi dịch vụ lỗi, hệ thống dùng dữ liệu đã lưu trước đó; chỉ báo nguồn chưa sẵn sàng khi chưa có dữ liệu nào. Bản nháp đang giữ xã đã bị gộp hoặc ngừng dùng vẫn giữ và hiển thị xã đó, nhưng trước khi chạy chặng 2D khách phải chọn lại xã theo dữ liệu mới. Thiết kế ở TDD-PROJ-001.
- Cách xử lý lựa chọn cũ khi đổi loại công trình hoặc tỉnh/thành đã được chốt tại khoản 11 và 12. Đã chốt bản dự toán cũ chỉ dùng danh mục tại thời điểm tạo; không được chọn loại mới thêm sau đó theo khoản 14.
- Reviewer và Approver là Tân Trần; tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kiểm thử thực thi.

- Bổ sung ngày 01/10/2026: công trình dùng lại danh mục này theo [BR-SITE-005](BR-SITE-005.md); tạo từ dự toán giữ cấu hình của nguồn, tạo độc lập giữ cấu hình lúc tạo. Không thay quy tắc nhập/gửi AI của PROJ hoặc cho phép Admin lưu nhóm bật rỗng ở khoản 13.
