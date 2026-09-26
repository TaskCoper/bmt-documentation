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

Admin quản lý loại công trình và cấu hình của loại đó; khách nhập thông tin bản dự toán hoặc hệ thống kiểm tra đầu vào trước khi gửi AI.

## Then

1. Cho Admin thêm loại công trình ngoài năm loại đã có trên trang mẫu. Không giới hạn danh mục nghiệp vụ ở đúng năm loại.
2. Admin bật/tắt riêng việc cho chọn số tầng và việc cho chọn Có tum/Không tum theo loại công trình. Khi bật chọn tầng, Admin cấu hình các số tầng được chọn cho loại đó; khách chọn trong danh sách áp dụng. Phân biệt cấu hình cho phép chọn tum với giá trị có/không tum mà khách chọn cho bản dự toán.
3. Tách thông tin phong cách kiến trúc và phong cách nội thất. Admin được thêm/sửa cả hai danh mục, gồm tên, ảnh minh họa và gán các phong cách được chọn cho từng loại công trình. Khách chọn trong danh mục đúng nhóm và đúng loại. Không tự sao chép một giá trị phong cách chung thành cả hai lựa chọn.
4. Admin cấu hình theo từng loại công trình việc cho chọn phong cách kiến trúc và phong cách nội thất. Khi một nhóm lựa chọn không được bật, khách không phải chọn phong cách cho nhóm đó; AI vẫn trả đầy đủ kết quả, không loại phần kiến trúc hoặc nội thất vì nhóm lựa chọn bị tắt.
5. Địa chỉ vẫn gồm tỉnh/thành phố, xã/phường và địa chỉ chi tiết. Xã/phường phải thuộc tỉnh/thành đã chọn.
6. Gói hoàn thiện và nội thất vẫn gồm Cơ bản, Tiêu chuẩn và VIP. Đây là đầu vào thiết kế, không phải gói đăng ký cấp quyền hoặc lượt sử dụng.
7. Trước khi gửi AI, yêu cầu đủ địa chỉ, loại công trình và gói hoàn thiện/nội thất; lựa chọn phong cách, số tầng và tum cần khớp cấu hình áp dụng. Mỗi nhóm phong cách được bật phải có đúng một lựa chọn hợp lệ trước khi gửi AI; bật cả hai nhóm thì chọn một phong cách kiến trúc và một phong cách nội thất. Nhóm bị tắt không yêu cầu khách nhập phong cách.
8. Cấu hình danh mục không thay thế quyền sở hữu bản dự toán, điều kiện gói/lượt hoặc quy tắc khóa đầu vào khi AI đang chạy.
9. Bản dự toán đã tạo giữ cấu hình loại công trình và phong cách đã áp dụng cho bản dự toán đó. Khi Admin sửa cấu hình hoặc danh mục phong cách, thay đổi chỉ áp dụng cho bản dự toán mới; không làm mất lựa chọn hoặc thay điều kiện đầu vào của bản dự toán cũ, kể cả bản nháp.
10. Khi mở lại, lưu tiếp hoặc gửi/thử lại AI trên bản dự toán cũ, kiểm tra các lựa chọn theo cấu hình của bản dự toán đó. Các điều kiện quyền, gói/lượt và khóa sửa vẫn được kiểm tra theo quy tắc hiện hành; giữ cấu hình danh mục không có nghĩa giữ quyền hoặc lượt cũ.

11. Khi khách đổi loại công trình trong bản nháp, giữ các lựa chọn phong cách kiến trúc, phong cách nội thất, số tầng và tum còn hợp lệ với loại mới theo cấu hình áp dụng cho bản dự toán. Xóa lựa chọn không còn phù hợp, gồm giá trị của trường không áp dụng cho loại mới; yêu cầu chọn lại phần bắt buộc trước khi gửi AI. Không tự chuyển phong cách giữa hai nhóm. Giữ nguyên diện tích, địa chỉ, ảnh và mô tả.
12. Khi khách đổi tỉnh/thành, xóa xã/phường đã chọn và yêu cầu chọn lại xã/phường thuộc tỉnh/thành mới trước khi gửi AI. Giữ địa chỉ chi tiết để khách tự sửa; không tự xóa hoặc viết lại địa chỉ chi tiết.

13. Khi Admin bật chọn tầng, danh sách số tầng phải có ít nhất một lựa chọn. Khi bật nhóm phong cách kiến trúc hoặc nội thất, nhóm đó phải được gán ít nhất một phong cách thuộc đúng nhóm cho loại công trình. Nếu bất kỳ danh sách được bật nào trống, từ chối lưu cấu hình và chỉ rõ danh sách cần bổ sung; không lưu cấu hình không hợp lệ hoặc thay cấu hình đã lưu trước đó. Điều kiện này áp dụng cả khi tạo cấu hình và khi sửa/xóa lựa chọn cuối cùng trong danh sách đang bật.

14. Bản dự toán chỉ được chọn các loại công trình thuộc danh mục đã áp dụng khi tạo bản dự toán đó. Nếu Admin thêm loại công trình sau thời điểm này, bản dự toán cũ không được đổi sang loại mới thêm; muốn sử dụng loại đó, khách phải tạo bản dự toán mới. Việc đổi loại trong bản nháp không làm bản dự toán chuyển sang danh mục hoặc cấu hình mới nhất.

15. Mỗi phong cách trong danh mục kiến trúc hoặc nội thất bắt buộc có một ảnh minh họa JPG, PNG hoặc WebP, dung lượng tối đa 5 MB. Không chấp nhận lưu phong cách thiếu ảnh hợp lệ, ảnh ngoài định dạng hoặc vượt giới hạn. Quy tắc này dành cho ảnh danh mục của Admin, tách khỏi ảnh đầu vào bản dự toán.

16. Danh mục phong cách kiến trúc và nội thất ban đầu do Admin tự nhập tên, ảnh minh họa và gán theo loại công trình trước khi khách sử dụng. Không chuẩn bị sẵn hoặc tự phân loại danh sách phong cách gộp trên trang mẫu thành dữ liệu khởi tạo. Các cấu hình được lưu vẫn phải đáp ứng điều kiện có ít nhất một lựa chọn cho mỗi nhóm đang bật theo khoản 13.

17. Tên loại công trình và tên phong cách kiến trúc/nội thất bắt buộc có nội dung sau khi bỏ khoảng trắng đầu/cuối, tối đa 200 ký tự. Không tự cắt ngắn tên vượt giới hạn. Cho phép trùng tên; tên không thay thế mã định danh của từng mục.

## Except

Bản nháp được phép chưa chọn đủ thông tin theo BR-PROJ-003. Việc giữ cấu hình cũ không cho phép sửa đầu vào khi AI đang chạy hoặc thay thế kết quả thành công; BR-PROJ-005 vẫn áp dụng.

## Notes

- Hiện trạng trang mẫu: Nhà phố, Villa/Biệt thự, Nhà mái, Nhà vườn/Nhà cấp 4 có chọn tầng và tum; Căn hộ không có. Đây là cấu hình đang quan sát, không phải ngoại lệ phải viết cố định theo tên Căn hộ cho mọi phiên bản.
- Danh sách tầng trên trang mẫu là Trệt đến Trệt + 4 lầu. Người dùng đã chốt Admin được cấu hình các số tầng theo loại; danh sách mẫu không còn là giới hạn cố định. Chưa tự đặt số tầng tối đa nghiệp vụ mới.
- Trang mẫu gộp kiến trúc và nội thất, được người dùng xác nhận là chưa chính xác. Bảng phong cách gộp trong phiên bản nháp trước không còn là danh mục triển khai. Admin tự nhập và gán danh mục ban đầu; không tự phân loại Hiện đại, Wabi-sabi, Tối giản hoặc các mục khác từ giao diện mẫu vào hai danh mục mới.
- Phạm vi quản lý cả hai danh mục phong cách đã được xác nhận riêng. Ảnh minh họa phong cách là dữ liệu danh mục của Admin; giới hạn một ảnh đầu vào bản dự toán tối đa 10 MB tại BR-PROJ-002 không tự trở thành giới hạn ảnh danh mục. Ảnh danh mục đã được chốt riêng: bắt buộc một ảnh JPG/PNG/WebP tối đa 5 MB theo khoản 15.
- Tầng thực thi của khoản 15 (người dùng xác nhận ngày 26/09/2026): frontend kiểm ảnh phong cách JPG/PNG/WebP tối đa 5 MB trước khi upload qua dịch vụ presigned URL; backend lưu URL ảnh, bắt buộc có URL và kiểm URL tuyệt đối dùng https, không kiểm định dạng hay dung lượng. Bổ sung cùng ngày (lần 2): URL ảnh mới phải thuộc tên miền của kho presign, danh sách tên miền đọc từ cấu hình. Nghĩa của khoản 15 không đổi. Thiết kế ở TDD-PROJ-001.
- Người dùng đã xác nhận cấu hình chỉ điều khiển lựa chọn phong cách, AI vẫn trả đủ kết quả. Không dùng cấu hình này để cắt phần thiết kế, nhóm chi phí dự toán hoặc nội dung hồ sơ. Danh sách đầu ra cụ thể vẫn chờ hợp đồng AI; quy tắc thành công và tính lượt không thay đổi.
- Khi khách không được chọn một nhóm phong cách, cách biểu diễn phần không có lựa chọn trong yêu cầu gửi AI cần thống nhất với bên tích hợp. Chưa tự chọn phong cách mặc định hoặc sao chép từ nhóm còn lại.
- Người dùng đã chốt giữ cấu hình của bản dự toán cũ, chỉ áp dụng thay đổi cho bản dự toán mới. Phương án lưu và xác định phiên bản cấu hình thuộc thiết kế kỹ thuật; chưa chốt bảng dữ liệu hoặc cách sao lưu danh mục.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về nhóm lựa chọn bị tắt: tắt chọn tầng, chọn tum hoặc một nhóm phong cách không xóa danh sách đã cấu hình; nhóm tắt được phép có danh sách rỗng hoặc có dữ liệu, để khi Admin bật lại thì danh sách cũ hiện ra. Khách vẫn chỉ thấy và chọn nhóm đang bật theo khoản 2 và 4; dữ liệu của nhóm tắt không được coi là lựa chọn hợp lệ khi lưu, khi đổi loại theo khoản 11 và khi kiểm trước khi gửi AI theo khoản 7. Khoản 13 giữ nguyên: nhóm bật phải có ít nhất một lựa chọn. Tum chỉ có cờ bật/tắt, không có danh sách.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về mở tính năng: người vận hành bật cổng tạo bản dự toán cho khách sau khi Admin xác nhận danh mục đủ theo khoản 16; hệ thống không tự kiểm danh mục đã đủ hay chưa.
- Người dùng xác nhận ngày 26/09/2026 (lần 2) về nguồn địa chỉ của khoản 5 và 12: dùng dịch vụ bên thứ ba provinces.open-api.vn theo địa giới mới (34 tỉnh/thành, hai cấp tỉnh–xã). Khi dịch vụ lỗi, hệ thống dùng dữ liệu đã lưu trước đó; chỉ báo nguồn chưa sẵn sàng khi chưa có dữ liệu nào. Bản nháp đang giữ xã đã bị gộp hoặc ngừng dùng vẫn giữ và hiển thị xã đó, nhưng trước khi gửi AI khách phải chọn lại xã theo dữ liệu mới. Thiết kế ở TDD-PROJ-001.
- Cách xử lý lựa chọn cũ khi đổi loại công trình hoặc tỉnh/thành đã được chốt tại khoản 11 và 12. Đã chốt bản dự toán cũ chỉ dùng danh mục tại thời điểm tạo; không được chọn loại mới thêm sau đó theo khoản 14.
- Reviewer và Approver là Tân Trần; tên không có nghĩa tài liệu đã được phê duyệt. Owner và ngày hiệu lực chưa xác định; chưa có kiểm thử thực thi.
