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

# BR-LIB-001

## Rule Info

- **Name**: Nội dung và phân loại mẫu.
- **Category**: Thư viện mẫu
- **Status**: Draft
- **Version**:
- **Effective Date**:
- **Owner**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Source**: Quyết định ngày 23/09/2026 về nội dung và phiên bản; bổ sung ngày 30/09/2026 về phong cách 3D và tìm mẫu; ngày 01/10/2026 người dùng yêu cầu section content cho cả 2D/3D, tên tự nhập theo mẫu, tất cả file nằm trong section.

## Statement

Mẫu 2D và 3D độc lập; nội dung xem của mỗi mẫu được chia thành các section content (nhóm nội dung có tên), mỗi section chứa một hoặc nhiều file. Mẫu dùng chung danh mục với phần tạo dự toán. Mẫu 3D có thể thuộc nhiều phong cách kiến trúc và nhiều phong cách nội thất. Tìm mẫu tham khảo từ dự toán phải khớp tất cả thông tin áp dụng, không loại mẫu chỉ vì khác phiên bản danh mục.

## When

Người quản lý cấu hình thư viện hoặc khách tìm kiếm, mở và xem lại mẫu.

## Then

1. Mẫu bắt buộc có tên, ít nhất một ảnh, một ảnh đại diện được chọn trong các ảnh của mẫu và thứ tự hiển thị ảnh. Nội dung tổ chức theo section content tại khoản 17; ảnh đại diện được chọn từ các ảnh nằm trong section của mẫu. Mô tả và tệp đính kèm không bắt buộc; khi có tệp đính kèm thì tệp cũng phải nằm trong section.
2. Ảnh nhận JPG/PNG/WebP; tệp đính kèm nhận PDF/DWG/DXF. Không đặt giới hạn nghiệp vụ về số lượng hoặc dung lượng tải lên. Giới hạn truyền tải của hạ tầng phải được làm rõ trong thiết kế, không tự biến thành hạn mức sản phẩm.
3. Chiều ngang và chiều dài tính bằng mét, diện tích tính bằng m²; cả ba bắt buộc, lớn hơn 0 và tối đa hai chữ số thập phân. Diện tích nhập độc lập, không bắt buộc bằng ngang nhân dài.
4. Mỗi mẫu thuộc đúng một loại công trình. Dùng chung danh mục và cấu hình tại BR-PROJ-004. Khi bật chọn tầng, chọn đúng một số tầng hợp lệ; khi bật tum, chọn đúng một giá trị Có tum hoặc Không tum. Trường bị tắt là Không áp dụng, không quy đổi thành 0 tầng hoặc Không tum.
5. Mẫu đã công bố giữ phân loại khi cấu hình chung thay đổi. Khi sửa phân loại, lựa chọn mới phải hợp lệ theo cấu hình hiện hành; chỉ sửa tên, ảnh hoặc tệp không buộc đổi phân loại cũ. Khi công bố phiên bản mới, kiểm tra phân loại theo cấu hình hiện hành.
6. Danh sách công khai chỉ hiển thị ảnh đại diện và thông tin tóm tắt. Toàn bộ ảnh và tệp đính kèm thuộc nội dung chi tiết có kiểm tra quyền xem.
7. Lọc theo 2D/3D, loại công trình, số tầng và Tất cả/Có tum/Không tum. Không áp dụng không đồng nghĩa Không tum. Bộ lọc tầng giữ cả giá trị cũ đang có mẫu công khai sử dụng; thu hẹp theo loại công trình được chọn.
8. Tìm bằng từ khóa chỉ tìm theo tên; kết hợp với các điều kiện lọc, gồm điều kiện từ dự toán tại khoản 11–14. Có phân trang, không tìm theo kích thước và không có lựa chọn sắp xếp danh sách mẫu. Mặc định mẫu có lần công bố gần nhất đứng trước; sửa tại chỗ không đẩy mẫu lên đầu. Thứ tự section bên trong từng mẫu do người quản lý điều chỉnh theo khoản 19.
9. Mẫu 2D dùng loại công trình, số tầng và tum; không áp dụng phong cách kiến trúc hoặc phong cách nội thất. Mẫu 3D dùng thêm hai nhóm phong cách này từ đúng danh mục Admin đang quản lý cho dự toán, không tạo hoặc sao chép thành danh mục riêng cho thư viện.
10. Mỗi mẫu 3D được chọn nhiều phong cách trong từng nhóm. Chỉ chọn mục thuộc đúng nhóm và được gán cho loại công trình theo cấu hình áp dụng; nhóm tắt không cho chọn. Khi công bố, mỗi nhóm đang bật phải có ít nhất một lựa chọn hợp lệ. Nháp được thiếu lựa chọn bắt buộc, nhưng lựa chọn đã nhập vẫn phải hợp lệ. Quy tắc giữ phân loại cũ, sửa phân loại và công bố phiên bản mới tại khoản 5 áp dụng cả hai nhóm phong cách.
11. Khi tìm mẫu tham khảo bằng thông tin của dự toán, mẫu 2D phải khớp loại công trình và các giá trị số tầng, tum áp dụng cho dự toán đó. Mẫu 3D phải khớp các điều kiện này và chứa phong cách dự toán đã chọn trong từng nhóm đang áp dụng. Không xét phong cách khi tìm 2D; không đưa trường hoặc nhóm bị tắt trong cấu hình của dự toán vào điều kiện tìm kiếm. Không áp dụng không được dùng thay cho giá trị cụ thể đang cần khớp.
12. Với mẫu 3D, mỗi nhóm phong cách được đối chiếu riêng: danh sách phong cách kiến trúc của mẫu phải chứa phong cách kiến trúc dự toán đã chọn; danh sách phong cách nội thất phải chứa phong cách nội thất dự toán đã chọn. Khi cả hai nhóm áp dụng, phải khớp cả hai. Mẫu có thêm phong cách khác vẫn được lấy ra.
13. Đối chiếu loại công trình và phong cách bằng ID ổn định của mục danh mục; số tầng và tum bằng giá trị. Không yêu cầu dự toán và mẫu trùng phiên bản danh mục. Phiên bản danh mục vẫn xác định cấu hình và nội dung đã áp dụng cho từng bên; việc tìm mẫu không chuyển bên nào sang phiên bản danh mục của bên kia. Cùng ID nhưng khác tên hoặc ảnh qua các phiên bản vẫn khớp; cùng tên nhưng khác ID không được coi là một mục.
14. Chỉ trả mẫu khớp tất cả điều kiện áp dụng; không tự nới điều kiện hoặc lấy mẫu gần giống. Không có mẫu phù hợp thì trả danh sách rỗng. Vẫn chỉ lấy phiên bản hiện hành của mẫu công khai, không ẩn; xem danh sách không dùng lượt, mở chi tiết tuân theo BR-LIB-003.

15. Có hai luồng tìm mẫu riêng. Ở trang thư viện, khách tự chọn hoặc bỏ các bộ lọc loại công trình, tầng/tum và, với mẫu 3D, phong cách kiến trúc/nội thất. Chỉ áp dụng bộ lọc đã chọn; không yêu cầu nhập đủ như đầu vào dự toán. Bộ lọc phong cách dùng ID của danh mục trong hệ thống. Mỗi nhóm phong cách được chọn nhiều mục: mẫu chỉ cần chứa ít nhất một mục đã chọn trong từng nhóm có bộ lọc. Hai nhóm kết hợp với nhau và với loại/tầng/tum bằng điều kiện đồng thời; nhóm không chọn mục nào không lọc.
16. Ở trang tạo dự toán, sau khi yêu cầu tạo thiết kế được tiếp nhận và AI đang xử lý, frontend tự gọi tìm mẫu 2D và 3D bằng phân loại của đúng đầu vào dự toán đã gửi AI. Luồng này khớp mọi điều kiện áp dụng theo khoản 11–14; không dùng các bộ lọc tùy ý trên trang thư viện để thay đầu vào dự toán. Mẫu tham khảo là nội dung thư viện có sẵn, không phải kết quả AI của tác vụ.

17. Nội dung của cả mẫu 2D và mẫu 3D được tổ chức thành các section content (nhóm nội dung). Người quản lý tự nhập tên section cho từng mẫu, không chọn từ danh mục section chung. “Tầng trệt”, “Tầng 2”, “Tầng 3”, “Phòng khách” và “Góc sofa” chỉ là ví dụ, không phải danh sách tên cố định. Mỗi section chứa một hoặc nhiều file; tất cả ảnh và tệp PDF/DWG/DXF đều thuộc section, không có nhóm tệp đính kèm riêng ở cấp mẫu. Hệ thống lưu tên section riêng với tên từng file và lưu được các file thuộc section đó. Khi công bố, mẫu phải có section, mỗi section có tên và ít nhất một file hợp lệ; việc lưu nháp thiếu thông tin tiếp tục theo Except.
18. Trước khi upload file nội dung thư viện, người quản lý phải tạo hoặc chọn một section content cụ thể của phiên bản mẫu đang sửa. Trong một phiên bản, mỗi file nội dung phải thuộc đúng một section. Không cho upload file rời ở cấp mẫu hoặc upload trước rồi chọn section sau. Yêu cầu thiếu section, section không tồn tại, thuộc mẫu/phiên bản khác hoặc không được phép sửa phải bị từ chối trước khi cho upload. Quy tắc này áp dụng cả với bản nháp; lưu nháp thiếu thông tin không cho phép bỏ qua section khi upload.

19. Người có quyền quản lý thư viện được thay đổi và lưu thứ tự các section content trong bản nháp hoặc phiên bản hiện tại của cả mẫu 2D và 3D. Khi đọc lại ở màn quản trị hoặc xem chi tiết, các section phải hiển thị đúng thứ tự đã lưu. Đổi thứ tự section không đổi tên section, không chuyển file sang section khác và không đổi thứ tự file bên trong từng section. Thao tác này là sửa nội dung cùng phiên bản theo BR-LIB-002, không tạo phiên bản mới hoặc tính thêm lượt; phiên bản đã bị thay thế giữ nguyên thứ tự và không được sửa.

20. Người quản lý được thêm section có tên trực tiếp vào phiên bản hiện tại. Section chưa có file ở trạng thái chuẩn bị, chỉ người có quyền quản lý thư viện thấy. Sau khi file đầu tiên được upload, kiểm tra và lưu thành công vào đúng section, section tự xuất hiện cho khách có quyền xem phiên bản đó. Chỉ cấp URL upload hoặc tải bytes lên kho chưa làm section xuất hiện; upload lỗi hoặc bị hủy thì section vẫn chỉ quản trị thấy. Không cần công bố phiên bản mới hoặc tính thêm lượt cho khách đã có quyền xem.

## Except

Bản nháp chưa công bố không được cung cấp cho khách. Cho phép lưu nháp khi chưa đủ thông tin, gồm section chưa có file, ảnh, kích thước và phân loại; chỉ kiểm tra đủ các trường bắt buộc trước khi công bố. Dữ liệu đã nhập vẫn phải hợp lệ. Mọi thao tác upload file cho bản nháp vẫn phải chỉ rõ section hợp lệ theo khoản 18. Phiên bản hiện tại được có thêm section chuẩn bị chưa có file theo khoản 20; các section này không phải nội dung đang cung cấp cho khách. Ngoại lệ này không bỏ điều kiện kiểm đủ section khi công bố một bản nháp.

## Notes

- Người dùng đã chọn phương án 1: thêm section trực tiếp vào mẫu đang công bố, chỉ quản trị thấy tới khi file đầu tiên được lưu thành công. Quy tắc được ghi tại khoản 20; không còn chờ lựa chọn giữa sửa trực tiếp và tạo phiên bản mới.
- Bổ sung ngày 01/10/2026 theo yêu cầu tổ chức nội dung mẫu thành section content: khoản 17–20; xem STORY-LIB-001/AC-012–AC-018 và STORY-LIB-003/AC-008. Đã xác nhận: áp dụng cho cả 2D/3D, tên section do người quản lý tự nhập cho từng mẫu, mỗi section chứa một hoặc nhiều file, tất cả ảnh và tệp PDF/DWG/DXF nằm trong section; bắt buộc chọn section cụ thể trước khi upload; người quản lý được thay đổi thứ tự section. Người dùng đã giao tiếp tục triển khai phạm vi này. Đã có ST-LIB-052–066 và bản bổ sung TDD-LIB-001/002, TDD-MEDIA-001 để chốt; chưa viết code cho section.
- Cần làm rõ trước khi chuyển dữ liệu cũ: môi trường đích có file thư viện chưa chia section hay không, và cách phân chúng vào section thực tế. Không coi yêu cầu bắt buộc section cho upload là xác nhận phương án tự gom file cũ vào section “Nội dung”; không tự đổi hay xóa file cũ. Thiết kế migration hiện đề xuất dừng trước khi thay dữ liệu nếu còn file chưa có section. Chưa tạo hoặc chạy migration.
- Bổ sung ngày 30/09/2026 theo nội dung người dùng đã chốt trong hội thoại: khoản 9–14; xem STORY-LIB-001/AC-008–AC-011 và STORY-LIB-002/AC-005–AC-009. Người dùng đã chốt phần US/BR bổ sung; đã có đặc tả System Test ST-LIB-032–041, chưa thực thi. Sau khi người dùng chốt TDD bổ sung, đã viết ST-LIB-042–051 và UT-LIB-053–078, gồm cả khoản 15–16 về hai luồng tìm. Chưa cập nhật mã ứng dụng hoặc thực thi các ca mới.
- Người dùng đã xác nhận “chốt US và BR” cho bộ LIB trong hội thoại. Trạng thái phê duyệt trên hệ thống chưa được cập nhật; tên Reviewer/Approver không thay cho thao tác phê duyệt.
- Người dùng xác nhận hiện chưa có mẫu 3D; không cần chính sách giữ mẫu 3D cũ thiếu phong cách. Mẫu 3D công bố mới phải tuân thủ khoản 10.
- Owner và ngày hiệu lực chưa xác định. Chưa triển khai hoặc chạy kiểm thử.
- Tầng thực thi khoản 2 (người dùng xác nhận ngày 26/09/2026, không đổi nghĩa quy tắc): frontend kiểm định dạng và dung lượng tệp trước khi tải lên kho qua presign, như với ảnh của bản dự toán. Backend chỉ lưu URL tệp và chỉ nhận URL https thuộc tên miền trong `UploadedFileOption__AllowedHosts`; backend không tải tệp về để kiểm định dạng, nên request gọi thẳng API với URL đúng tên miền nhưng trỏ tới tệp sai định dạng không bị backend chặn. Thiết kế ở TDD-LIB-001/Architecture.
