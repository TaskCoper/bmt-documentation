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

<!-- Mỗi file chứa một tài liệu. Thay mã STORY-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Priority: Must / Should / Could / Won't. Status: Todo / In Progress / Blocked / Done.
Heading cấp 4 trong Alternative Flow và Exception Flow chỉ chứa mã ngắn, duy nhất như ALT-01 hoặc EXC-01, tối đa 50 ký tự; không nối mô tả vào mã. Đặt mô tả ở đoạn bên dưới heading, tối đa 500 ký tự, trước danh sách bước.
AC dùng mã duy nhất như AC-001; giữ nhãn Given/When/Then/And và bám sát luồng, điều kiện, ngoại lệ cùng Business Rule đã xác nhận.
Tham chiếu dùng mã tài liệu, có thể thêm /section và : ghi chú. Xoá dòng tham chiếu không dùng.
Creator và Assignee chỉ là tên trong Markdown; phân công tài khoản và phê duyệt thực hiện trên giao diện sau import.
Sơ đồ thuộc TDD. Các trường quản trị, giả định và câu hỏi mở bổ sung trên giao diện.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Story được dùng làm tiêu đề tài liệu: 1–500 ký tự. Assignee: mỗi tên tối đa 200; vai trò được đọc: Frontend / Backend / Fullstack / Mobile / QA / DevOps / Designer / Reviewer / Other.
- ALT/EXC: mã không rỗng, tối đa 50 ký tự, duy nhất trên cả hai nhóm; mô tả bên dưới mã tối đa 500. Main Flow không có mã nhánh. Mỗi AC dùng mã duy nhất, tối đa 1.000 ký tự; mã AC được dùng trong tham chiếu section vẫn phải nằm trong giới hạn section 100 ký tự.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
ĐỐI CHIẾU FORM USER STORY (src/features/user-stories/validations.ts; form có thể chặt hơn import):
- Điền Story, Context, Creator, Trigger; ít nhất một Assignee có tên, một bước Main Flow và một AC có ít nhất một điều kiện. Mỗi nhánh ALT/EXC đã khai báo có mã và ít nhất một bước. Không để rỗng bước, điều kiện AC hoặc mục danh sách đã thêm.
- Sprint dùng số nguyên dương để đồng thời đáp ứng form yêu cầu số dương và parser đọc Int32 (tối đa 2.147.483.647). Không tự đặt Sprint hoặc số liệu khi chưa có nguồn.
-->


# STORY-SUB-001

## Metadata

**Phạm vi mới nhất:** chỉ quyền tạo thiết kế mới và tra cứu mẫu được triển khai logic sử dụng/tính lượt trong đợt này. Quyền 3D vẫn cấu hình bật/tắt và hiển thị, chưa điều khiển AI hoặc kiểm tra quyền tạo 3D. Các mô tả xử lý 3D bên dưới là thiết kế cho giai đoạn sau, không phải tiêu chí nghiệm thu hiện tại. Không suy ra AI hiện phải luôn trả hoặc luôn bỏ 3D.

**Cập nhật: đổi gói và đổi chu kỳ đều áp dụng ngay theo BR-SUB-021. Các khối được đánh dấu lịch sử bên dưới không còn dùng nghiệm thu.**

- **Story**: Là người dùng có subscription, tôi muốn dùng chung quyền lợi thiết kế của tài khoản cho các dự án của mình để không phải có một hạn mức riêng cho từng dự án.
- **Context**: Đổi gói hoặc chu kỳ áp dụng ngay theo BR-SUB-021, không phân loại nâng/hạ hoặc dùng bậc. Mỗi tài khoản có tối đa một subscription thiết kế đang hiệu lực; mỗi công trình có tối đa một gói giám sát đang hiệu lực. Các subscription này được dùng đồng thời. Thiết kế dùng subscription định kỳ theo tháng hoặc năm tính từ ngày bắt đầu có hiệu lực; giám sát là gói theo dự án, không chu kỳ và hoàn thành thủ công. Quyền lợi thiết kế dùng chung theo tài khoản; quyền lợi giám sát chỉ dùng cho công trình gắn với gói giám sát. Đây là bản nháp cho phần đã chốt; điều kiện hiệu lực còn đang trao đổi. Hạn mức làm mới theo kỳ subscription, không cộng dồn lượt dư. Luồng nhận gói và gia hạn sẽ thiết kế cùng thanh toán sau.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Tìm kiếm/xem danh sách mẫu không yêu cầu đăng nhập hoặc có subscription. Mở chi tiết mẫu và các thao tác trên dự án yêu cầu đăng nhập. Việc bắt đầu thao tác cần quyền subscription yêu cầu gói còn hiệu lực và đáp ứng quyền/lượt tương ứng; tác vụ đã bắt đầu hợp lệ có thể hoàn thành sau khi hết hạn, luồng xem dữ liệu cũ áp dụng cả sau khi hết hạn.
- Với thao tác trên dự án, các dự án đang xét thuộc tài khoản đó; tìm kiếm/xem danh sách mẫu không yêu cầu có dự án.
- Hiệu lực thời gian theo BR-SUB-014; quyền thao tác hiện tại theo BR-SUB-007, BR-SUB-008 và BR-SUB-017. Sự kiện kích hoạt và điều kiện giao dịch thiết kế cùng thanh toán; không hỏi lại các thao tác đã chốt.

### Trigger

Khách tìm kiếm/xem danh sách mẫu; tài khoản đã đăng nhập xem dự án hoặc kết quả đã tạo, hoặc yêu cầu sử dụng quyền lợi subscription.

## Flow

### Main Flow

1. Người dùng thực hiện một thao tác sử dụng quyền lợi trong dự án thuộc tài khoản. Đợt này quản lý riêng hai loại lượt tạo thiết kế mới và tra cứu mẫu; không tự lấy lượt loại khác để dùng thay khi một loại đã hết.
2. Hệ thống xác định quyền lợi và hạn mức theo nhóm dịch vụ: thiết kế dùng chung theo tài khoản, giám sát chỉ dùng cho công trình đã gắn.
3. Với yêu cầu tạo thiết kế có hạn mức hữu hạn, hệ thống giữ trước một lượt từ hạn mức chung khi tiếp nhận xử lý; lượt đang giữ không thể dùng cho yêu cầu khác.
4. Một lần Gen AI trả toàn bộ bộ kết quả cùng lúc; phối cảnh 3D là một phần trong đó theo quyền gói, không có thao tác tạo hoặc tính lượt riêng. Với tạo mới, khi AI tạo đủ kết quả, hệ thống đã lưu và khách có thể mở xem thì chuyển lượt giữ của đúng loại thành đã dùng, không trừ thêm lần nữa. Không chờ khách mở hoặc duyệt; khách không hài lòng không tự được hoàn lượt. Với tra cứu mẫu, tìm kiếm và xem danh sách không dùng lượt; mỗi lần hệ thống trả được nội dung chi tiết một mẫu tính 1 lượt tra cứu. Lỗi không tải được nội dung thì không mất lượt; giải phóng lượt tạm giữ nếu có.
5. Với quyền lợi thiết kế, các dự án khác của cùng tài khoản dùng số lượt còn lại này; không được cấp thêm một hạn mức riêng chỉ vì là dự án khác.

### Alternative Flow

#### ALT-01

Tài khoản bước sang kỳ subscription mới hợp lệ.

1. Hệ thống bỏ số lượt dư của kỳ trước.
2. Hạn mức dùng chung của tài khoản được làm mới bằng hạn mức của kỳ mới, không cộng dồn lượt dư.

#### ALT-02

Tác vụ tạo thiết kế giữ lượt ở kỳ cũ và hoàn thành sau khi tài khoản sang kỳ mới hợp lệ.

1. Lượt đã giữ tiếp tục thuộc kỳ cũ; hạn mức kỳ mới được cấp độc lập.
2. Nếu thành công, chuyển lượt đang giữ thành lượt đã dùng của kỳ cũ.
3. Nếu lỗi, giải phóng lượt giữ ở kỳ cũ, không cộng lượt vào kỳ mới.
4. Cả hai kết quả đều không thay đổi hạn mức kỳ mới.

#### ALT-03

Admin sửa quyền lợi của gói, lưu nháp để kiểm tra rồi bấm Công bố trong khi tài khoản đang sử dụng.

1. Tài khoản tiếp tục dùng quyền lợi và hạn mức của kỳ hiện tại; việc sửa gói không làm mới số lượt còn lại.
2. Khi kỳ subscription mới hợp lệ bắt đầu, tài khoản nhận đúng bản quyền lợi đã chốt khi đăng ký/gia hạn kỳ đó. Các lần Công bố sau khi chốt không thay thế bản này; bản nháp không được dùng để cấp quyền.
3. Lượt dư không cộng dồn; tác vụ đã giữ lượt ở kỳ cũ vẫn tính vào kỳ cũ.

#### ALT-04

Kiểm tra một quyền lợi dạng mức thay vì hạn mức lượt.

1. Hệ thống xác định mức tài khoản được cấp cho quyền lợi trong kỳ hiện tại.
2. Nếu mức được cấp bằng hoặc cao hơn mức tính năng yêu cầu, kiểm tra quyền theo mức đạt.
3. Nếu mức được cấp thấp hơn, kiểm tra quyền theo mức không đạt.
4. Việc kiểm tra mức không giữ hoặc trừ lượt. Điều kiện hạn mức riêng của thao tác, nếu có, còn cần xác định theo tính năng.

#### ALT-05

Subscription đã hết hạn và chưa gia hạn; khách xem dữ liệu cũ, tải tệp có sẵn hoặc xuất PDF từ kết quả thiết kế cũ.

1. Hệ thống kiểm tra quyền truy cập dự án và kết quả của tài khoản.
2. Cho khách xem dữ liệu cũ và tải tệp kết quả đã có sẵn trước khi hết hạn mà tài khoản có quyền truy cập; không yêu cầu gia hạn chỉ để xem hoặc tải lại. Không tạo hoặc xuất thêm tệp mới từ thao tác tải này.
3. Nếu khách yêu cầu xuất PDF từ kết quả thiết kế cũ có quyền truy cập, cho phép xuất dù trước đó chưa có PDF. Không cần gia hạn chỉ để xuất PDF này; không thực hiện Gen AI hoặc thay đổi thiết kế nguồn.

#### ALT-06

Tác vụ tạo thiết kế đã bắt đầu hợp lệ và giữ lượt trước khi subscription hết hạn; tài khoản chưa gia hạn.

1. Tác vụ được tiếp tục, không bị hủy chỉ vì subscription hết hạn.
2. Nếu thành công, tính lượt đã dùng vào kỳ đã giữ lượt.
3. Nếu lỗi, giải phóng lượt giữ ở kỳ đã hết hạn, không cho dùng lại lượt đó.
4. Không tự tạo kỳ mới hoặc cho phép bắt đầu tác vụ mới cần quyền subscription.

#### ALT-07

Tài khoản được cấp quyền lợi không giới hạn lượt trong kỳ hiện tại.

1. Hệ thống kiểm tra hiệu lực subscription và các quyền liên quan đến thao tác.
2. Khi đủ điều kiện, cho phép bắt đầu thao tác mà không chặn theo số lượt đã dùng.
3. Nếu subscription hết hạn, từ chối bắt đầu thao tác mới cần quyền subscription dù quyền lợi được cấu hình không giới hạn.

#### ALT-08

Tài khoản đang có subscription của một nhóm và nhận subscription hợp lệ của nhóm còn lại.

1. Hệ thống kiểm tra giới hạn subscription theo tài khoản đối với thiết kế và theo công trình đối với giám sát.
2. Cho phép một subscription thiết kế và một gói giám sát cùng có hiệu lực khi đáp ứng các điều kiện khác.
3. Không tự kết thúc subscription nhóm đang dùng để tiếp nhận nhóm còn lại.

#### ALT-09

Xác định ngày kết thúc kỳ subscription thiết kế.

1. Lấy ngày bắt đầu có hiệu lực và lựa chọn tháng hoặc năm đã ghi nhận cho subscription trong gói.
2. Kỳ tháng kết thúc cùng ngày tháng sau; kỳ năm kết thúc cùng ngày/tháng năm sau.
3. Nếu ngày tương ứng không tồn tại, lấy ngày cuối tháng đích; giữ nguyên giờ bắt đầu theo giờ Việt Nam.
4. Không tự tạo kỳ mới hoặc gia hạn từ thao tác tính ngày kết thúc.

#### ALT-10

Khách muốn một phương án thiết kế khác sau khi dự án đã Gen AI thành công và có kết quả.

1. Khách tạo dự án mới và nhập thông tin.
2. Khách bấm Gen AI trên dự án mới; hệ thống kiểm tra quyền và hạn mức chung của tài khoản trước khi tiếp nhận.
3. Tính lượt tạo mới theo quy tắc hiện có; giữ nguyên dự án và kết quả cũ.
4. Không cho Gen AI thêm phương án trên dự án đã có kết quả thành công, kể cả yêu cầu trực tiếp; yêu cầu bị từ chối không giữ/trừ lượt.

#### ALT-11

Gen AI đã thất bại, chưa có kết quả thành công; khách muốn thử lại.

1. Khách bấm thử lại trên cùng dự án, dùng thông tin đã nhập.
2. Hệ thống kiểm tra lại hiệu lực subscription, quyền và lượt sẵn dùng tại thời điểm tiếp nhận.
3. Nếu đủ điều kiện, tiếp nhận yêu cầu và giữ 1 lượt tạo mới; thành công tính đã dùng, lỗi giải phóng theo quy tắc hiện có.
4. Nếu không đủ điều kiện, từ chối, không giữ/trừ lượt và không bắt đầu tác vụ. Không tự gia hạn, cấp lượt hoặc tạo dự án mới.

#### ALT-12

Khách đã đăng nhập nhưng chưa có gói muốn tìm kiếm và xem danh sách mẫu.

1. Hệ thống cho tìm kiếm và trả danh sách mẫu, không yêu cầu gói, quyền tra cứu hoặc lượt.
2. Không tự cấp gói hay giữ/trừ lượt; không yêu cầu tạo dự án trước khi tìm kiếm.
3. Nếu khách yêu cầu mở chi tiết mẫu, hệ thống kiểm tra gói, quyền và lượt; khách chưa có gói bị từ chối, không trả nội dung chi tiết.

#### ALT-13

Khách chưa đăng nhập muốn tìm kiếm hoặc xem danh sách mẫu.

1. Hệ thống cho tìm kiếm và trả danh sách mà không bắt đăng nhập, tạo tài khoản, dự án hoặc nhận gói.
2. Khi khách chọn mở chi tiết mẫu, yêu cầu đăng nhập; không trả nội dung chi tiết khi chưa đăng nhập, kể cả yêu cầu gửi trực tiếp.
3. Sau đăng nhập, vẫn kiểm tra gói còn hiệu lực, quyền tra cứu và lượt sẵn dùng hoặc không giới hạn trước khi cho mở chi tiết.

#### ALT-14

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách nâng gói thiết kế khi gói cũ còn hiệu lực. Nếu đang có yêu cầu hạ gói hoặc chỉ đổi chu kỳ đang chờ, khách phải hủy yêu cầu đó thành công trước; nếu chưa hủy, xử lý theo EXC-10.

1. Khi việc nâng gói đã hoàn tất hợp lệ, hệ thống áp dụng quyền của gói đích ngay từ thời điểm đó. Điều kiện hoàn tất còn cần chốt cùng các bước nghiệp vụ tiếp theo và phần thanh toán liên quan.
2. Các yêu cầu sử dụng tính năng sau thời điểm chuyển kiểm tra quyền gói mới, không chờ ngày hết hạn gói cũ; vẫn chỉ có một subscription thiết kế đang hiệu lực.
3. Khách được chọn chu kỳ tháng/năm của gói đích trong lần nâng, kể cả khác chu kỳ hiện tại. Bắt đầu kỳ mới theo lựa chọn đó tại thời điểm nâng hoàn tất, tính ngày kết thúc theo BR-SUB-014; không giữ ngày hết hạn cũ hoặc cộng thời gian còn lại. Nâng kèm đổi chu kỳ không phải chờ hết kỳ cũ.
4. Cấp đủ hạn mức của các quyền trong gói đích cho kỳ mới, bỏ lượt dư chưa dùng của kỳ cũ. Không trừ số lượt đã dùng ở kỳ cũ vào hạn mức mới.
5. Tác vụ đã giữ lượt xử lý ở kỳ cũ theo BR-SUB-003; không dùng hoặc hoàn vào kỳ mới. Khách trả đủ giá kỳ gói mới theo AC-052, không trừ tiền cho thời gian gói cũ còn lại.

#### ALT-15

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách yêu cầu hạ gói thiết kế khi kỳ hiện tại chưa kết thúc.

1. Khách chọn gói đích và chu kỳ tháng/năm trong cùng yêu cầu hạ gói, kể cả chu kỳ khác hiện tại. Ghi nhận lựa chọn cho kỳ tiếp theo; không đổi quyền, mốc hết hạn hoặc làm mới lượt của kỳ đang dùng.
2. Khách tiếp tục dùng quyền gói hiện tại đến hết kỳ, trong giới hạn lượt còn sẵn dùng.
3. Giữ giá và toàn bộ quyền lợi đã chốt trong yêu cầu theo BR-SUB-004, không lấy bản công bố mới hơn. Đến mốc kết thúc kỳ hiện tại, chuyển sang gói đích nếu yêu cầu chưa bị hủy và đã đủ điều kiện chuyển hợp lệ. Kỳ đích bắt đầu theo chu kỳ đã chọn từ mốc này, tính ngày hết hạn theo BR-SUB-014; bỏ lượt dư cũ và cấp hạn mức tháng/năm tương ứng. Yêu cầu sử dụng mới kiểm tra gói đích; giữ giới hạn một subscription thiết kế đang hiệu lực.
4. Khách được tự hủy yêu cầu đang chờ trước thời điểm chuyển. Hủy thành công thì không đổi gói hoặc chu kỳ theo yêu cầu đó; quyền, lượt và ngày hết hạn hiện tại không đổi. Tiếp tục gói hiện tại ở kỳ sau vẫn theo điều kiện gia hạn, không tự gia hạn chỉ vì hủy yêu cầu hạ gói.
5. Muốn đổi gói đích của yêu cầu hạ gói đang chờ, khách phải hủy yêu cầu cũ thành công rồi chọn lại. Không sửa/thay thế trực tiếp; yêu cầu mới được kiểm tra điều kiện hợp lệ và vẫn áp dụng từ kỳ tiếp theo. Nếu chưa tạo được yêu cầu mới thì yêu cầu cũ vẫn đã hủy.
6. Nếu gói đích ngừng bán sau khi yêu cầu đã ghi nhận hợp lệ, vẫn giữ và thực hiện yêu cầu theo lịch khi chưa bị hủy và đủ điều kiện khác theo BR-SUB-013. Điều kiện hoàn tất chuyển và xử lý thanh toán còn cần thiết kế. Không coi việc gửi yêu cầu là xác nhận thanh toán hoặc tự cấp kỳ mới.

#### ALT-16

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách đổi từ tháng sang năm trong cùng gói thiết kế.

1. Ghi nhận lựa chọn năm với giá và toàn bộ quyền lợi đã chốt cho yêu cầu, không tự thay bằng bản công bố sau đó; giữ nguyên quyền, lượt và ngày hết hạn kỳ tháng hiện tại.
2. Tại mốc hết kỳ tháng, nếu yêu cầu chưa bị hủy và đủ điều kiện chuyển hợp lệ, bắt đầu kỳ năm từ mốc đó. Ngày hết hạn kỳ năm tính theo BR-SUB-014.
3. Bỏ lượt tháng còn dư và cấp hạn mức năm đã xác định hợp lệ; hạn mức dùng cho cả kỳ năm, không làm mới hàng tháng. Tác vụ đã giữ lượt vẫn xử lý ở kỳ cũ theo BR-SUB-003.
4. Khách được tự bấm “Hủy yêu cầu đổi chu kỳ” trước thời điểm chuyển khi yêu cầu còn đang chờ. Khi xử lý thành công, hủy yêu cầu ngay; giữ nguyên quyền, lượt và ngày hết hạn hiện tại. Đến mốc đã hẹn, không chuyển theo yêu cầu đã hủy.
5. Hủy yêu cầu không hủy gói đang dùng, không tự gia hạn hoặc cấp lượt mới. Không coi yêu cầu đổi chu kỳ là xác nhận thanh toán. Các thay đổi lựa chọn đang chờ đã chốt hủy rồi tạo lại theo EXC-10, EXC-11 và ALT-18; điều kiện giao dịch và thu tiền thiết kế cùng thanh toán.

#### ALT-17

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách đổi từ năm sang tháng trong cùng gói thiết kế.

1. Ghi nhận lựa chọn tháng với giá và toàn bộ quyền lợi đã chốt cho yêu cầu, không tự thay bằng bản công bố sau đó; giữ quyền, lượt và mốc hết hạn của kỳ năm đang dùng. Không cấp lượt tháng ngay khi nhận yêu cầu.
2. Tại mốc hết kỳ năm, nếu yêu cầu chưa bị hủy và đủ điều kiện chuyển hợp lệ, bắt đầu kỳ tháng từ mốc đó. Ngày hết hạn kỳ tháng tính theo BR-SUB-014.
3. Bỏ lượt năm còn dư và cấp hạn mức tháng đã xác định hợp lệ. Tác vụ đã giữ lượt vẫn xử lý ở kỳ cũ theo BR-SUB-003.
4. Khách được tự bấm “Hủy yêu cầu đổi chu kỳ” trước thời điểm chuyển khi yêu cầu còn đang chờ. Khi xử lý thành công, hủy yêu cầu ngay; giữ nguyên quyền, lượt và ngày hết hạn hiện tại. Đến mốc đã hẹn, không chuyển theo yêu cầu đã hủy.
5. Hủy yêu cầu không hủy gói đang dùng, không tự gia hạn hoặc cấp lượt mới. Không coi yêu cầu đổi chu kỳ là xác nhận thanh toán. Các thay đổi lựa chọn đang chờ đã chốt hủy rồi tạo lại theo EXC-10, EXC-11 và ALT-18; điều kiện giao dịch và thu tiền thiết kế cùng thanh toán.

#### ALT-18

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách giữ nguyên gói đích của yêu cầu hạ đang chờ nhưng muốn đổi tháng/năm của gói đích.

1. Yêu cầu khách hủy yêu cầu cũ thành công trước khi tạo lại; từ chối sửa trực tiếp và giữ nguyên yêu cầu cũ nếu chưa hủy.
2. Kiểm tra yêu cầu tạo lại theo điều kiện hiện tại. Khi hợp lệ, ghi nhận yêu cầu mới từ cuối kỳ hiện tại; giữ nguyên quyền, lượt và ngày hết hạn hiện tại.
3. Nếu gói đích đã ngừng bán, từ chối yêu cầu tạo lại theo BR-SUB-013. Không tự khôi phục yêu cầu đã hủy hoặc gia hạn.

#### ALT-19

Đổi gói hoặc chu kỳ ngay khi hoàn tất hợp lệ, theo BR-SUB-021.

1. Khách chọn gói đích và chu kỳ, chỉ đổi chu kỳ hoặc mua lại cùng gói/cùng chu kỳ; trước khi hoàn tất giữ nguyên kỳ đang dùng.
2. Tại T khi hoàn tất hợp lệ, kết thúc kỳ cũ và bắt đầu kỳ mới; không so giá hoặc bậc để chọn thời điểm chuyển.
3. Tính đủ chu kỳ từ T, cấp đủ hạn mức đích, bỏ thời gian/lượt dư cũ và trả đủ giá kỳ đích; lượt đang giữ vẫn thuộc kỳ cũ.
4. Tác vụ AI đã tiếp nhận giữ bộ quyền lúc tiếp nhận theo AC-069; lượt vẫn thuộc kỳ cũ. Thao tác mới dùng quyền gói mới. Không xóa kết quả cũ hoặc tự chuyển gói giám sát. Khách được mua lại cùng gói/cùng chu kỳ theo AC-070, áp dụng cùng cách bắt đầu kỳ mới ngay.

### Exception Flow

#### EXC-01

Tạo thiết kế lỗi trong cùng kỳ subscription còn hiệu lực.

1. Hệ thống giải phóng lượt đang giữ cho yêu cầu lỗi.
2. Lượt đó có thể dùng lại; yêu cầu lỗi không được tính là đã dùng lượt.

#### EXC-02

Tác vụ tạo thiết kế chưa có kết quả cuối cùng khi vượt thời gian chờ được cấu hình.

1. Hệ thống tự đánh dấu thất bại do quá thời gian và giải phóng lượt giữ.
2. Nếu kỳ còn hiệu lực, lượt dùng lại được; nếu kỳ hết hạn, không dùng lại hoặc cộng sang kỳ mới.
3. Kết quả đến muộn không tự trừ lượt lại, không đưa cho khách; tác vụ giữ trạng thái thất bại.

#### EXC-03

Một thay đổi hiệu lực sẽ làm tài khoản có hai subscription thiết kế cùng hiệu lực.

1. Hệ thống không chấp nhận thay đổi gây chồng hiệu lực.
2. Không cấp thêm quyền lợi hoặc hạn mức từ subscription thứ hai; không tự đổi gói hoặc kết thúc subscription đang hiệu lực.

#### EXC-04

Khách bắt đầu thao tác mới cần quyền subscription khi subscription đã hết hạn và chưa gia hạn.

1. Hệ thống từ chối bắt đầu thao tác, kể cả khi kỳ hết hạn còn lượt chưa dùng.
2. Không giữ hoặc tính lượt cho yêu cầu bị từ chối.

#### EXC-05

Khách sửa hoặc lưu thông tin dự án khi subscription thiết kế đã hết hạn và chưa có kỳ mới hợp lệ.

1. Hệ thống từ chối yêu cầu sửa/lưu, kể cả yêu cầu gửi trực tiếp hoặc từ biểu mẫu đã mở trước khi hết hạn.
2. Giữ nguyên dữ liệu đã lưu, không tính lượt và không tự cấp kỳ mới.
3. Khách vẫn được xem, tải tệp đã có và xuất PDF từ kết quả cũ theo quyền truy cập.

#### EXC-06

Khách mới chưa từng có gói thiết kế yêu cầu tạo dự án hoặc lưu thông tin dự án.

1. Hệ thống từ chối, báo cần có gói còn hiệu lực trước; kiểm tra cả yêu cầu gửi trực tiếp và lưu nháp.
2. Không tạo dự án hoặc lưu dữ liệu từ yêu cầu bị từ chối, không giữ/trừ lượt và không tự cấp gói.

#### EXC-07

Khách có gói thiết kế còn hiệu lực nhưng chỉ có quyền tra cứu, không có quyền tạo thiết kế, yêu cầu tạo/lưu dự án.

1. Hệ thống từ chối, báo cần quyền tạo thiết kế; áp dụng cả lưu nháp và yêu cầu gửi trực tiếp.
2. Không tạo dự án hoặc lưu thay đổi từ yêu cầu bị từ chối, không giữ/trừ lượt; quyền tra cứu vẫn giữ nguyên.

#### EXC-08

Gói thiết kế còn hiệu lực và có quyền tạo thiết kế, nhưng khách đã dùng hết lượt tạo trong kỳ.

1. Hệ thống từ chối tạo dự án hoặc lưu thông tin dự án, kể cả lưu nháp, yêu cầu gửi trực tiếp hoặc biểu mẫu mở trước khi hết lượt.
2. Báo đã hết lượt tạo thiết kế; không tạo bản ghi hoặc lưu thay đổi từ yêu cầu bị từ chối, giữ nguyên dữ liệu đã lưu. Không giữ/trừ thêm lượt hoặc tự cấp lượt mới.

#### EXC-09

Khách có gói còn hiệu lực và quyền tạo thiết kế nhưng toàn bộ lượt còn lại đang được giữ cho tác vụ AI chạy.

1. Tạm từ chối tạo/lưu dự án, gồm lưu nháp và yêu cầu gửi trực tiếp; không ghi thay đổi hoặc giữ/trừ thêm lượt.
2. Tác vụ đang chạy tiếp tục xử lý bình thường. Nếu lỗi và trả lại lượt trong kỳ còn hiệu lực, khách được gửi lại yêu cầu tạo/lưu khi đủ quyền và có lượt sẵn dùng; không tự lưu yêu cầu đã bị từ chối.

#### EXC-10

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách muốn nâng gói trong lúc còn yêu cầu hạ gói hoặc chỉ đổi chu kỳ đang chờ.

1. Hệ thống từ chối yêu cầu nâng, kể cả yêu cầu gửi trực tiếp, và báo khách cần hủy yêu cầu đang chờ trước.
2. Giữ nguyên yêu cầu đang chờ, gói đang dùng, kỳ, quyền và số lượt; không tự hủy hoặc thay thế yêu cầu đó để tiếp tục nâng.
3. Sau khi khách hủy thành công, khách có thể gửi yêu cầu nâng mới và phải đáp ứng các điều kiện hợp lệ. Nếu chưa nâng hoặc nâng không hoàn tất, không tự khôi phục yêu cầu đã hủy.

#### EXC-11

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

Khách đang chờ hạ gói nhưng muốn chỉ đổi chu kỳ trong cùng gói, hoặc đang chờ chỉ đổi chu kỳ nhưng muốn hạ gói.

1. Khi chưa hủy yêu cầu cũ, từ chối yêu cầu mới, kể cả yêu cầu gửi trực tiếp; hướng dẫn khách hủy trước. Giữ nguyên yêu cầu cũ, kỳ hiện tại, quyền và lượt.
2. Sau khi khách hủy thành công, kiểm tra yêu cầu mới theo điều kiện hợp lệ tại thời điểm gửi. Yêu cầu mới hợp lệ vẫn chờ đến hết kỳ hiện tại theo BR-SUB-015 hoặc BR-SUB-019.
3. Hủy không tự tạo yêu cầu mới hoặc gia hạn. Nếu chưa tạo được yêu cầu mới, không tự khôi phục yêu cầu đã hủy.

## Acceptance Criteria

#### AC-001

- **Given**: Tài khoản có hai dự án và còn N lượt dùng chung cho một quyền lợi thiết kế, với N lớn hơn 0.
- **When**: Một dự án sử dụng quyền lợi đó và hệ thống đã ghi nhận trừ một lượt hợp lệ.
- **Then**: Số lượt còn lại dùng chung của tài khoản là N - 1.
- **And**: Dự án còn lại dùng hạn mức N - 1 này; không có thêm N lượt riêng cho dự án đó.

#### AC-002

- **Given**: Tài khoản còn 3 lượt ở kỳ cũ và có kỳ subscription mới hợp lệ với hạn mức 10 lượt.
- **When**: Kỳ subscription mới bắt đầu.
- **Then**: Tài khoản có 10 lượt dùng chung cho các dự án, không phải 13 lượt.
- **And**: Kỳ làm mới trùng với kỳ subscription được cấu hình theo tháng hoặc năm.

#### AC-003

- **Given**: Tài khoản có 10 lượt tạo thiết kế sẵn dùng trong một kỳ còn hiệu lực, không có tác vụ khác.
- **When**: Một yêu cầu tạo thiết kế được tiếp nhận và sau đó hoàn thành thành công trong kỳ này.
- **Then**: Trong lúc xử lý có 9 lượt sẵn dùng và 1 lượt đang giữ, chưa tính là đã dùng.
- **And**: Khi thành công có 9 lượt sẵn dùng, 1 lượt đã dùng và không còn lượt đang giữ cho yêu cầu đó.

#### AC-004

- **Given**: Tài khoản có 10 lượt tạo thiết kế sẵn dùng trong một kỳ còn hiệu lực, không có tác vụ khác.
- **When**: Một yêu cầu được giữ 1 lượt để xử lý nhưng tạo thiết kế lỗi trong cùng kỳ.
- **Then**: Lượt đang giữ được giải phóng; tài khoản có lại 10 lượt sẵn dùng.
- **And**: Yêu cầu lỗi không được ghi nhận là đã dùng lượt.

#### AC-005

- **Given**: Tác vụ tạo thiết kế đã giữ 1 lượt ở kỳ cũ; tài khoản sang kỳ mới có 10 lượt sẵn dùng và không có tác vụ khác.
- **When**: Tác vụ hoàn thành thành công trong kỳ mới.
- **Then**: Lượt đang giữ được chuyển thành lượt đã dùng của kỳ cũ.
- **And**: Kỳ mới vẫn có 10 lượt sẵn dùng; không trừ lượt của kỳ mới.

#### AC-006

- **Given**: Tác vụ tạo thiết kế đã giữ 1 lượt ở kỳ cũ; tài khoản sang kỳ mới có 10 lượt sẵn dùng và không có tác vụ khác.
- **When**: Tác vụ kết thúc lỗi trong kỳ mới.
- **Then**: Lượt đang giữ ở kỳ cũ được giải phóng, không ghi nhận lượt đã dùng từ tác vụ lỗi.
- **And**: Kỳ mới vẫn có 10 lượt sẵn dùng, không phải 11; lượt giải phóng không chuyển sang kỳ mới.

#### AC-007

- **Given**: Kỳ hiện tại có 10 lượt, đã dùng 2, không có lượt đang giữ. Bản B đã công bố và được chốt cho lần đăng ký/gia hạn kỳ mới có 20 lượt và quyền 3D bật.
- **When**: Admin công bố bản C có 30 lượt, quyền 3D tắt trước khi kỳ mới bắt đầu; sau đó kỳ mới hợp lệ bắt đầu.
- **Then**: Kỳ hiện tại giữ nguyên hạn mức 10 và 8 lượt sẵn dùng, không thay đổi quyền lợi.
- **And**: Kỳ mới dùng đúng B: 20 lượt và quyền 3D bật, không dùng C hoặc ghép quyền từ C; không cộng 8 lượt dư. Các số chỉ là dữ liệu kiểm thử.

#### AC-008

- **Given**: Bản A đã công bố có hạn mức 10 lượt; tài khoản đang dùng A và kỳ mới hợp lệ cũng đã được chốt dùng A.
- **When**: Admin lưu nháp hạn mức 20 lượt nhưng chưa bấm Công bố, và tài khoản bước sang kỳ tiếp theo hợp lệ.
- **Then**: Kỳ mới vẫn được cấp 10 lượt theo bản đã công bố, không phải 20 lượt từ bản nháp.
- **And**: Việc lưu nháp không thay đổi quyền lợi hoặc số lượt của kỳ hiện tại.

#### AC-009

- **Given**: Một quyền lợi có thứ tự mức cơ bản < nâng cao; tài khoản được cấp mức nâng cao còn hiệu lực.
- **When**: Hệ thống kiểm tra quyền đối với yêu cầu mức cơ bản và yêu cầu mức nâng cao của quyền lợi này.
- **Then**: Cả hai yêu cầu đều đạt điều kiện về mức.
- **And**: Việc kiểm tra mức không giữ hoặc trừ lượt.

#### AC-010

- **Given**: Một quyền lợi có thứ tự mức cơ bản < nâng cao; tài khoản được cấp mức cơ bản còn hiệu lực.
- **When**: Hệ thống kiểm tra quyền đối với yêu cầu mức nâng cao của quyền lợi này.
- **Then**: Yêu cầu không đạt điều kiện về mức; mức cơ bản không cấp quyền dùng mức nâng cao.
- **And**: Việc kiểm tra mức không giữ hoặc trừ lượt.

#### AC-011

- **Given**: Tài khoản đã có một subscription thiết kế đang hiệu lực.
- **When**: Hệ thống nhận thay đổi khiến một subscription thiết kế thứ hai của tài khoản có hiệu lực chồng với subscription hiện có.
- **Then**: Thay đổi gây chồng hiệu lực không được chấp nhận; tài khoản vẫn chỉ có một subscription thiết kế đang hiệu lực.
- **And**: Quyền lợi và hạn mức của subscription thứ hai không được cấp thêm; subscription hiện có không bị tự kết thúc hoặc thay thế.

#### AC-012

- **Given**: Subscription đã hết hạn, chưa gia hạn; tài khoản có dự án và kết quả đã tạo mà tài khoản có quyền truy cập.
- **When**: Khách mở xem dự án và kết quả đó.
- **Then**: Khách vẫn xem được dữ liệu cũ, không phải gia hạn để xem.
- **And**: Việc xem không tự tạo kỳ subscription mới hoặc làm mới hạn mức.

#### AC-013

- **Given**: Subscription đã hết hạn, chưa có kỳ mới hợp lệ và kỳ cũ còn 3 lượt chưa dùng.
- **When**: Khách gửi yêu cầu tạo thiết kế mới cần quyền subscription.
- **Then**: Hệ thống không bắt đầu tác vụ mới.
- **And**: Không giữ hoặc tính lượt cho yêu cầu bị từ chối.

#### AC-014

- **Given**: Tác vụ tạo thiết kế đã bắt đầu hợp lệ và giữ 1 lượt trước khi subscription hết hạn; tài khoản chưa gia hạn.
- **When**: Subscription hết hạn trong lúc tác vụ đang chạy, rồi tác vụ hoàn thành thành công.
- **Then**: Tác vụ không bị hủy chỉ vì hết hạn; lượt đã dùng được ghi nhận vào kỳ đã giữ lượt.
- **And**: Không tự tạo kỳ mới hoặc cấp quyền bắt đầu tác vụ mới cần subscription.

#### AC-015

- **Given**: Tác vụ tạo thiết kế đã bắt đầu hợp lệ và giữ 1 lượt trước khi subscription hết hạn; tài khoản chưa gia hạn.
- **When**: Subscription hết hạn trong lúc tác vụ đang chạy, rồi tác vụ kết thúc lỗi.
- **Then**: Lượt giữ được giải phóng ở kỳ cũ, không ghi nhận lượt đã dùng từ tác vụ lỗi.
- **And**: Lượt đã hết hạn không được dùng lại; không tự cấp kỳ mới hoặc hạn mức mới.

#### AC-016

- **Given**: Tài khoản có quyền tạo thiết kế không giới hạn lượt, đáp ứng các điều kiện khác và subscription còn hiệu lực.
- **When**: Tài khoản gửi thêm yêu cầu tạo thiết kế sau nhiều lần sử dụng thành công.
- **Then**: Yêu cầu không bị từ chối do hết lượt.
- **And**: Khi subscription hết hạn và chưa gia hạn, yêu cầu mới vẫn bị từ chối; không giới hạn lượt không kéo dài hiệu lực subscription.

#### AC-017

- **Given**: Tài khoản có một subscription thiết kế đang hiệu lực và chưa có gói giám sát đang hiệu lực.
- **When**: Một gói giám sát đáp ứng các điều kiện hợp lệ được tiếp nhận cho tài khoản.
- **Then**: Hai subscription được phép cùng có hiệu lực; subscription thiết kế không bị tự kết thúc.
- **And**: Quy tắc tương tự áp dụng khi tài khoản có giám sát trước và tiếp nhận thiết kế sau.

#### AC-018

- **Given**: Kỳ thiết kế bắt đầu ngày 15/09/2026, được cấu hình theo tháng hoặc năm.
- **When**: Hệ thống xác định ngày kết thúc kỳ.
- **Then**: Kỳ tháng kết thúc ngày 15/10/2026; kỳ năm kết thúc ngày 15/09/2027.
- **And**: Không chốt về cuối tháng/năm chung và không tự tạo kỳ tiếp theo.

#### AC-019

- **Given**: Kỳ tháng bắt đầu ngày 31/01/2027 hoặc 31/01/2028; kỳ năm bắt đầu ngày 29/02/2028.
- **When**: Hệ thống xác định ngày kết thúc mà tháng/năm đích không có ngày tương ứng.
- **Then**: Hai kỳ tháng lần lượt kết thúc ngày 28/02/2027 và 29/02/2028; kỳ năm kết thúc ngày 28/02/2029.
- **And**: Thời điểm kết thúc giữ nguyên giờ bắt đầu theo giờ Việt Nam, kể cả khi phải điều chỉnh ngày cuối tháng.

#### AC-020

- **Given**: Kỳ thiết kế bắt đầu lúc 14:30 ngày 15/09/2026, kết thúc lúc 14:30 ngày 15/10/2026 theo giờ Việt Nam; chưa có kỳ tiếp theo, quyền và hạn mức còn đáp ứng yêu cầu.
- **When**: Hệ thống kiểm tra yêu cầu mới trước mốc kết thúc, tại đúng mốc kết thúc và sau mốc kết thúc.
- **Then**: Trước mốc kết thúc, kỳ vẫn còn hiệu lực; từ đúng 14:30 ngày 15/10/2026, kỳ cũ hết hiệu lực và không cho bắt đầu thao tác mới dựa trên kỳ cũ.
- **And**: Không kéo dài đến hết ngày, không tự tạo kỳ mới; tác vụ đã bắt đầu hợp lệ vẫn được xử lý theo quy tắc hoàn thành sau khi hết hạn.

#### AC-021

- **Given**: Một gói thiết kế đã công bố có lựa chọn tháng 10 lượt và lựa chọn năm 150 lượt; tài khoản được cấp hợp lệ lựa chọn năm, không có sử dụng hoặc thay đổi khác.
- **When**: Hệ thống cấp quyền cho kỳ năm và kiểm tra hạn mức khi đi qua một tháng nhưng vẫn trong kỳ năm.
- **Then**: Kỳ năm được cấp 150 lượt, không lấy 10 lượt hoặc tự tính thành 120 lượt.
- **And**: Không tự chia hoặc làm mới lượt hàng tháng; khi chưa sử dụng, hạn mức vẫn là 150 lượt cho kỳ năm. Tài khoản chỉ có một subscription thiết kế đang hiệu lực.

#### AC-022

- **Given**: Tài khoản có 10 lượt trong kỳ còn hiệu lực; tác vụ tạo thiết kế đã giữ 1 lượt, còn 9 lượt sẵn dùng và chưa có kết quả cuối cùng.
- **When**: Tác vụ vượt thời gian chờ được cấu hình và hệ thống xử lý quá thời gian.
- **Then**: Tác vụ thất bại do quá thời gian; lượt giữ được giải phóng, tài khoản có lại 10 lượt sẵn dùng.
- **And**: Không cần nhân viên xử lý thủ công và không ghi nhận lượt đã dùng từ tác vụ này.

#### AC-023

- **Given**: Tác vụ đã được xử lý thất bại do quá thời gian và đã giải phóng lượt giữ.
- **When**: Kết quả thành công đến muộn, kể cả được gửi lại nhiều lần.
- **Then**: Tác vụ vẫn thất bại do quá thời gian; kết quả muộn không xuất hiện trong kết quả của khách qua giao diện hoặc API.
- **And**: Không tự giữ/trừ lại lượt hoặc thay đổi hạn mức kỳ cũ/kỳ mới; không gắn kết quả muộn vào yêu cầu mới của khách.

#### AC-024

- **Given**: Tác vụ giữ lượt ở kỳ cũ, chưa có kết quả và kỳ đó đã hết hạn.
- **When**: Tác vụ vượt thời gian chờ và hệ thống xử lý quá thời gian.
- **Then**: Giải phóng lượt giữ ở kỳ cũ, không làm lượt hết hạn dùng lại được.
- **And**: Nếu có kỳ mới hợp lệ, hạn mức kỳ mới không thay đổi; nếu chưa gia hạn, không tự tạo kỳ mới.

#### AC-025

- **Given**: Tài khoản có kỳ còn hiệu lực, đủ quyền tạo thiết kế, còn 10 lượt tạo mới và 20 lượt tra cứu; không có tác vụ khác.
- **When**: Một yêu cầu tạo thiết kế mới được tiếp nhận, giữ 1 lượt và hoàn thành thành công.
- **Then**: Lượt tạo mới chuyển từ 10 sẵn dùng sang 9 sẵn dùng và 1 đang giữ, rồi thành 9 sẵn dùng và 1 đã dùng.
- **And**: Lượt tra cứu vẫn là 20 trong suốt quá trình; không gộp hai loại hoặc trừ từ loại khác.

#### AC-030

- **Given**: Tài khoản có quyền tra cứu, còn 20 lượt tra cứu và không có thao tác sử dụng lượt khác.
- **When**: Khách tìm kiếm mẫu và xem danh sách kết quả, chưa mở chi tiết mẫu nào.
- **Then**: Vẫn còn 20 lượt tra cứu; không giữ hoặc trừ lượt vì tìm kiếm hoặc xem danh sách.
- **And**: Lượt tạo mới không thay đổi.

#### AC-031

- **Given**: Tài khoản có kỳ còn hiệu lực, đủ quyền tra cứu, 20 lượt tra cứu và không có thao tác sử dụng lượt khác.
- **When**: Khách mở chi tiết mẫu A thành công, đóng lại rồi chủ động mở chi tiết A thành công lần nữa.
- **Then**: Lần đầu còn 19 lượt tra cứu, lần thứ hai còn 18; mỗi lần mở chi tiết tính 1 lượt, không cấp quyền xem lại miễn lượt chỉ vì mẫu đã từng mở.
- **And**: Lượt tạo mới không thay đổi. Ca này không xác định cách tính khi tải lỗi, tải lại trang hoặc hệ thống tự gửi lại yêu cầu.

#### AC-032

- **Given**: Tài khoản có kỳ còn hiệu lực, đủ quyền tra cứu, 20 lượt tra cứu và không có thao tác sử dụng lượt khác.
- **When**: Khách yêu cầu mở chi tiết mẫu nhưng hệ thống báo lỗi, không trả được nội dung chi tiết.
- **Then**: Không tính lượt đã dùng cho lần mở lỗi; sau xử lý lỗi vẫn có 20 lượt tra cứu sẵn dùng, không còn lượt tạm giữ cho yêu cầu nếu có.
- **And**: Lượt tạo mới không thay đổi.

#### AC-033

- **Given**: Yêu cầu Gen AI tạo mới đã được tiếp nhận hợp lệ, giữ 1 lượt đúng loại và chưa bị kết thúc do lỗi/quá thời gian.
- **When**: AI tạo đủ kết quả, hệ thống đã lưu và khách có thể mở xem; khách chưa mở hoặc duyệt kết quả.
- **Then**: Chuyển lượt giữ thành 1 lượt đã dùng của đúng loại, không tiếp tục giữ để chờ khách chấp nhận.
- **And**: Sau đó khách mở xem nhưng không hài lòng cũng không tự hoàn lượt; không thay đổi lượt tra cứu.

#### AC-034

- **Given**: Subscription đã hết hạn, chưa gia hạn; tài khoản có quyền truy cập một tệp kết quả đã được tạo và lưu sẵn trước khi hết hạn.
- **When**: Khách tải tệp kết quả đó.
- **Then**: Tải được tệp đã có sẵn, không cần gia hạn chỉ để tải lại.
- **And**: Không tạo hoặc xuất thêm tệp mới, không tự tạo kỳ mới hoặc thay đổi các loại lượt.

#### AC-035

- **Given**: Subscription đã hết hạn, chưa gia hạn; khách có quyền truy cập kết quả thiết kế cũ và chưa từng xuất PDF cho kết quả này.
- **When**: Khách yêu cầu xuất PDF từ kết quả đó.
- **Then**: Hệ thống cho phép xuất PDF, không yêu cầu gia hạn hoặc phải có tệp PDF từ trước.
- **And**: Nội dung lấy từ kết quả cũ; không gọi Gen AI để tạo thiết kế, không tự tạo kỳ mới hoặc làm mới hạn mức.

#### AC-036

- **Given**: Subscription thiết kế đã hết hạn, chưa gia hạn; khách có quyền xem dự án của mình.
- **When**: Khách yêu cầu lưu thay đổi tên dự án, ghi chú hoặc thông tin đầu vào, kể cả gửi yêu cầu trực tiếp.
- **Then**: Yêu cầu bị từ chối và dữ liệu đã lưu giữ nguyên; không chỉ dựa vào việc khóa nút trên giao diện.
- **And**: Không tính lượt hoặc tự tạo kỳ mới; quyền xem, tải tệp đã có và xuất PDF từ kết quả cũ vẫn giữ nguyên.

#### AC-037

- **Given**: Dự án A đã Gen AI thành công và có kết quả; tài khoản còn hiệu lực, đủ quyền và 2 lượt tạo mới sẵn dùng.
- **When**: Khách yêu cầu Gen AI thêm trên A, sau đó tạo dự án B, nhập thông tin và gửi Gen AI hợp lệ trên B.
- **Then**: Yêu cầu trên A bị từ chối, không giữ/trừ lượt. Yêu cầu trên B được tiếp nhận và giữ 1 lượt từ hạn mức chung; khi thành công chỉ còn 1 lượt sẵn dùng.
- **And**: Dự án A và kết quả cũ giữ nguyên; B không có hạn mức riêng. Tiêu chí này không áp dụng cho thử lại khi lần Gen AI trước thất bại.

#### AC-038

- **Given**: Dự án A có yêu cầu Gen AI đã thất bại và lượt giữ đã được giải phóng; chưa có kết quả thành công. Tài khoản còn hiệu lực, đủ quyền và 2 lượt tạo mới sẵn dùng.
- **When**: Khách bấm thử lại trên A và yêu cầu mới hoàn thành thành công.
- **Then**: Dùng lại thông tin của A, không bắt buộc tạo dự án mới; giữ 1 lượt khi tiếp nhận rồi chuyển thành đã dùng khi thành công, còn 1 lượt sẵn dùng.
- **And**: Không trừ thêm cho yêu cầu thất bại trước đó; lượt tra cứu không đổi.

#### AC-039

- **Given**: Dự án có yêu cầu Gen AI đã thất bại, chưa có kết quả thành công; lúc khách thử lại thì subscription đã hết hạn hoặc không còn lượt tạo mới sẵn dùng.
- **When**: Khách yêu cầu thử lại trên cùng dự án.
- **Then**: Từ chối yêu cầu, không bắt đầu tác vụ hoặc giữ/trừ lượt.
- **And**: Giữ thông tin dự án, không tự gia hạn hoặc cấp thêm lượt.

#### AC-040

- **Given**: Tài khoản có subscription còn hiệu lực, quyền tạo thiết kế và phối cảnh 3D chân thực, còn 2 lượt tạo mới; dự án chưa có kết quả thành công.
- **When**: Khách bấm Gen AI một lần và tác vụ hoàn thành đủ bộ kết quả, đã lưu và có thể mở xem.
- **Then**: Khách nhận các phần kết quả, gồm phối cảnh 3D, cùng một lần hoàn tất; không phải bấm tạo 3D riêng. Tác vụ giữ 1 lượt khi bắt đầu và chuyển đúng 1 lượt thành đã dùng khi thành công.
- **And**: Trước khi đủ bộ kết quả, không trả riêng một phần như kết quả thành công. Không trừ thêm lượt cho 3D; sau thành công còn 1 lượt tạo sẵn dùng, lượt tra cứu không đổi. Nếu tác vụ lỗi hoặc quá thời gian thì xử lý theo BR-SUB-003 và BR-SUB-016.

#### AC-041

- **Given**: Hai tài khoản dùng hai gói thiết kế khác nhau, đều còn hiệu lực, có quyền tạo thiết kế và đủ lượt; mỗi tài khoản có dự án mới với dữ liệu đầu vào tương đương.
- **When**: Cả hai bấm Gen AI và nhận đủ bộ kết quả thành công, trong đó có dự toán nội thất.
- **Then**: Hai gói nhận dự toán với cùng mức chi tiết theo đặc tả dự toán chung; không chia sơ bộ/chi tiết theo tên hoặc giá gói. Không yêu cầu có gợi ý giảm chi phí vì phần này đã hoãn; thiếu gợi ý đó không làm bộ kết quả bị coi là thất bại.
- **And**: Không yêu cầu các số tiền hoặc câu chữ do AI tạo phải giống hệt nhau. Mỗi bộ kết quả vẫn trả cùng lúc và tính một lượt tạo thiết kế; không có lượt dự toán riêng.

#### AC-042

- **Given**: Hai tài khoản dùng hai gói thiết kế khác nhau, đều còn hiệu lực, có quyền tạo thiết kế và đủ lượt; mỗi tài khoản có dự án mới với dữ liệu đầu vào tương đương.
- **When**: Cả hai bấm Gen AI và nhận đủ bộ kết quả thành công, trong đó có bố trí công năng.
- **Then**: Phần bố trí công năng của hai gói đáp ứng cùng mức chi tiết theo đặc tả chung; không chia cơ bản/nâng cao theo tên hoặc giá gói.
- **And**: Không yêu cầu phương án bố trí phải giống hệt nhau. Các phần kết quả vẫn trả cùng lúc, mỗi bộ thành công tính một lượt tạo thiết kế. Quyền tạo phối cảnh 3D chân thực vẫn được kiểm tra riêng theo gói.

#### AC-043

- **Given**: Khách mới đăng ký tài khoản, chưa từng được cấp gói thiết kế.
- **When**: Khách yêu cầu tạo dự án hoặc lưu thông tin đầu vào, gồm thao tác lưu nháp và yêu cầu gửi trực tiếp.
- **Then**: Từ chối và thông báo cần có gói còn hiệu lực trước khi tạo/lưu; không tạo bản ghi dự án hoặc lưu thông tin từ yêu cầu bị từ chối.
- **And**: Không tự cấp gói, không giữ hoặc trừ lượt. Quyết định này không cho phép Gen AI trước khi đủ điều kiện về gói, quyền và lượt.

#### AC-044

- **Given**: Tài khoản có subscription thiết kế còn hiệu lực với quyền tra cứu và còn lượt tra cứu, nhưng không có quyền tạo thiết kế.
- **When**: Khách tạo dự án hoặc lưu thông tin dự án, gồm lưu nháp và yêu cầu gửi trực tiếp.
- **Then**: Từ chối vì thiếu quyền tạo thiết kế; không tạo bản ghi dự án hoặc lưu thay đổi từ yêu cầu bị từ chối.
- **And**: Không giữ/trừ lượt hoặc tự cấp thêm quyền. Subscription và quyền tra cứu hiện có giữ nguyên; có lượt tra cứu không thay thế được quyền tạo thiết kế.

#### AC-045

- **Given**: Tài khoản có gói thiết kế còn hiệu lực, có quyền tạo thiết kế với hạn mức hữu hạn, đã dùng hết lượt trong kỳ; không có lượt đang giữ.
- **When**: Khách tạo dự án hoặc lưu thông tin, gồm lưu nháp, yêu cầu gửi trực tiếp và lưu biểu mẫu đã mở khi còn lượt.
- **Then**: Từ chối vì hết lượt tạo thiết kế; không tạo dự án hoặc ghi thay đổi từ yêu cầu bị từ chối, giữ nguyên thông tin đã lưu trước đó.
- **And**: Không tự cấp lượt hoặc giữ/trừ thêm lượt; gói và các quyền khác vẫn giữ nguyên. Đây là điều kiện cho phép tạo/lưu thông tin, không phải tính lượt cho thao tác nhập thông tin.

#### AC-046

- **Given**: Tài khoản có gói còn hiệu lực và quyền tạo thiết kế hữu hạn; lượt cuối đang được giữ cho tác vụ ở dự án A, không còn lượt sẵn dùng.
- **When**: Khách yêu cầu tạo/lưu dự án B; sau đó tác vụ A thất bại, trả lượt trong cùng kỳ còn hiệu lực và khách gửi lại yêu cầu trên B.
- **Then**: Yêu cầu đầu bị từ chối, không ghi dữ liệu hoặc giữ/trừ thêm lượt. Sau khi trả lượt, yêu cầu mới được xử lý nếu các điều kiện khác đều đáp ứng.
- **And**: Không tự lưu lại yêu cầu bị từ chối. Tạo/lưu thông tin B không giữ hoặc trừ lượt; lượt chỉ được giữ khi Gen AI hợp lệ được tiếp nhận. Tác vụ A không bị chặn xử lý vì không còn lượt sẵn dùng.

#### AC-047

- **Given**: Khách đã đăng nhập, chưa có gói và chưa có dự án.
- **When**: Khách tìm kiếm/xem danh sách mẫu, sau đó yêu cầu mở chi tiết một mẫu, gồm cả yêu cầu gửi trực tiếp.
- **Then**: Tìm kiếm và danh sách hoạt động mà không cần gói hoặc dự án; yêu cầu mở chi tiết bị từ chối vì chưa có gói, không trả nội dung chi tiết.
- **And**: Không tự cấp subscription, quyền tra cứu hoặc lượt miễn phí, không giữ/trừ lượt khi tìm kiếm/xem danh sách hoặc từ chối mở chi tiết.

#### AC-048

- **Given**: Khách truy cập khi chưa đăng nhập; có mẫu trong danh sách tìm kiếm hợp lệ.
- **When**: Khách tìm kiếm/xem danh sách rồi yêu cầu mở chi tiết một mẫu, gồm yêu cầu gửi trực tiếp.
- **Then**: Tìm kiếm và danh sách hoạt động mà không bắt đăng nhập; mở chi tiết yêu cầu đăng nhập và không trả nội dung chi tiết cho khách chưa đăng nhập.
- **And**: Không tự tạo tài khoản, gói, quyền hoặc lượt. Đăng nhập thành công không tự đủ điều kiện mở chi tiết: vẫn kiểm tra gói, quyền tra cứu và lượt theo quy tắc đã chốt.

#### AC-049

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản đang dùng gói A chưa hết hạn và chưa có quyền phối cảnh 3D chân thực. Một yêu cầu nâng lên gói B có quyền 3D được xác nhận hoàn tất hợp lệ tại thời điểm T trước ngày hết hạn cũ; các dữ liệu cần thiết để kiểm tra quyền gói đích đã được xác định.
- **When**: Hệ thống xử lý việc nâng gói hoàn tất và kiểm tra quyền tính năng của tài khoản ngay sau T.
- **Then**: Tài khoản có quyền 3D theo gói B ngay sau T, không phải đợi ngày hết hạn cũ; tại cùng thời điểm chỉ có một subscription thiết kế hiệu lực.
- **And**: Chỉ bấm chọn gói chưa được coi là hoàn tất nâng gói. Ngày hết hạn mới theo AC-050; tiêu chí này không quy định số lượt được cấp; đủ điều kiện chạy Gen AI vẫn cần kiểm tra riêng.

#### AC-050

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Gói A theo tháng đang hiệu lực, dự kiến hết hạn ngày 30/9 lúc 10:00 theo giờ Việt Nam. Yêu cầu nâng lên B theo tháng được xác nhận hoàn tất ngày 15/9 lúc 10:00.
- **When**: Hệ thống áp dụng việc nâng gói.
- **Then**: Kỳ B bắt đầu 15/9 lúc 10:00 và kết thúc 15/10 lúc 10:00; không giữ mốc 30/9 hoặc cộng thời gian chưa dùng của A. Tại mốc chuyển chỉ B còn hiệu lực để tiếp nhận thao tác mới.
- **And**: Nếu tháng đích không có ngày tương ứng thì lấy ngày cuối tháng, giữ giờ theo BR-SUB-014. Lượt mới và lượt dư theo AC-051; số tiền khi nâng theo AC-052.

#### AC-051

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Gói A có hạn mức 10 lượt tạo, đã dùng 7, còn 3 lượt chưa dùng, không có lượt đang giữ. Gói đích B có hạn mức 20 lượt tạo cho chu kỳ được chọn.
- **When**: Yêu cầu nâng từ A lên B hoàn tất hợp lệ và kỳ B bắt đầu.
- **Then**: Kỳ B có đủ 20 lượt tạo sẵn dùng; không cộng 3 lượt dư thành 23 và không trừ 7 lượt đã dùng ở A thành 13.
- **And**: 3 lượt dư của A không còn dùng được; lịch sử 7 lượt đã dùng được giữ nguyên ở kỳ cũ. Không tự cấp thêm quyền không có trong B. Các số chỉ là dữ liệu minh họa.

#### AC-052

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản đang dùng gói A còn thời gian sử dụng. Giá kỳ gói đích B đã xác định hợp lệ là 600.000đ.
- **When**: Hệ thống xác định số tiền phải trả để nâng từ A lên B và bắt đầu kỳ mới.
- **Then**: Số tiền phải trả là 600.000đ; không trừ tiền cho phần thời gian chưa dùng của A.
- **And**: Đây là dữ liệu minh họa, không phải giá bán đã chốt. Tiêu chí này không tự thu tiền, xác nhận thanh toán hoặc cấp gói; tích hợp thanh toán sẽ làm sau. Chính sách hoàn tiền riêng chưa được quyết định bởi tiêu chí này.

#### AC-053

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Gói PRO có quyền tạo phối cảnh 3D, kết thúc ngày 30/9 lúc 10:00 theo giờ Việt Nam. Ngày 15/9, khách yêu cầu chuyển sang BASIC không có quyền này ở kỳ tiếp theo. Hai gói và quyền chỉ là dữ liệu thử.
- **When**: Hệ thống ghi nhận yêu cầu hạ gói và kiểm tra quyền trước mốc hết hạn, sau đó tại đúng mốc hết hạn khi đã đủ điều kiện chuyển hợp lệ.
- **Then**: Trước mốc hết hạn, quyền PRO vẫn giữ nguyên, gồm quyền tạo phối cảnh 3D. Từ mốc chuyển, áp dụng quyền BASIC cho yêu cầu sử dụng mới; không còn quyền tạo phối cảnh 3D.
- **And**: Gửi yêu cầu hạ gói không làm mới lượt hoặc đổi ngày hết hạn hiện tại. Không có hai subscription thiết kế cùng hiệu lực. Tiêu chí này kiểm tra thời điểm chuyển quyền, không thực thi thanh toán; điều kiện chuyển thực tế sẽ bổ sung sau.

#### AC-054

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản đang dùng PRO đến 30/9 lúc 10:00, có yêu cầu chuyển xuống BASIC vào mốc này. Chưa có một lần gia hạn hợp lệ khác được xác nhận.
- **When**: Ngày 20/9, khách tự hủy yêu cầu hạ gói của mình khi yêu cầu vẫn đang chờ.
- **Then**: Yêu cầu được hủy, không cần nhân viên xử lý thay. Quyền, lượt và ngày hết hạn hiện tại không đổi; đến mốc chuyển không áp dụng BASIC theo yêu cầu đã hủy.
- **And**: Hủy yêu cầu không tự tạo kỳ PRO mới, thu tiền hoặc làm mới lượt. Việc dùng PRO ở kỳ sau vẫn theo điều kiện gia hạn; đây không phải thao tác hoàn tác một lần hạ gói đã có hiệu lực.

#### AC-055

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: PRO tháng kết thúc 30/9/2026 lúc 10:00 theo giờ Việt Nam. Ngày 15/9, khách yêu cầu chuyển sang PRO năm. Bản quyền lợi đích đã xác định hợp lệ có 100 lượt tạo cho cả năm.
- **When**: Hệ thống kiểm tra kỳ hiện tại sau khi nhận yêu cầu, sau đó xử lý chuyển tại mốc hết kỳ tháng khi đủ điều kiện hợp lệ.
- **Then**: Trước mốc chuyển vẫn dùng kỳ tháng, không cấp lượt năm hoặc đổi mốc hết hạn. Kỳ năm bắt đầu 30/9/2026 lúc 10:00 và kết thúc 30/9/2027 lúc 10:00.
- **And**: Nếu kỳ tháng còn 3 lượt chưa dùng tại mốc chuyển, bỏ 3 lượt này và cấp 100 lượt cho kỳ năm, không phải 103; không làm mới hàng tháng. Tác vụ đang giữ lượt vẫn theo kỳ đã giữ. Các số là dữ liệu thử; tiêu chí không xác nhận thanh toán hoặc quyết định bản quyền lợi đích được chốt ở bước nào.

#### AC-056

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: PRO năm kết thúc 30/9/2027 lúc 10:00 theo giờ Việt Nam. Ngày 15/3/2027, khách yêu cầu chuyển sang PRO tháng. Bản quyền lợi đích đã xác định hợp lệ cấp 10 lượt tạo cho kỳ tháng.
- **When**: Hệ thống kiểm tra kỳ hiện tại sau khi nhận yêu cầu, sau đó xử lý chuyển tại mốc hết kỳ năm khi đủ điều kiện hợp lệ.
- **Then**: Trước mốc chuyển vẫn dùng kỳ năm, không đổi mốc hết hạn hoặc cấp lượt tháng. Kỳ tháng bắt đầu 30/9/2027 lúc 10:00 và kết thúc 30/10/2027 lúc 10:00.
- **And**: Nếu còn 25 lượt năm chưa dùng tại mốc chuyển, bỏ 25 lượt này và cấp 10 lượt tháng, không phải 35. Tác vụ đang giữ lượt vẫn theo kỳ đã giữ. Các số là dữ liệu thử; tiêu chí không thực thi thanh toán hoặc quyết định thời điểm chốt bản quyền lợi đích.

#### AC-057

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản có kỳ thiết kế còn hiệu lực và một yêu cầu đổi chu kỳ đang chờ, áp dụng sau khi hết kỳ hiện tại. Áp dụng cho cả tháng sang năm và năm sang tháng; chưa có lần gia hạn hợp lệ khác được xác nhận.
- **When**: Trước thời điểm chuyển, khách bấm “Hủy yêu cầu đổi chu kỳ” của mình và hệ thống xử lý thành công.
- **Then**: Yêu cầu bị hủy ngay, không cần nhân viên xử lý thay hoặc chờ đến cuối kỳ. Khách tiếp tục dùng kỳ hiện tại với quyền, lượt và ngày hết hạn không đổi. Tại mốc đã hẹn, không đổi chu kỳ theo yêu cầu đã hủy.
- **And**: Không tự hủy subscription hiện tại, thu tiền, gia hạn hoặc cấp lượt mới. Tiếp tục sử dụng ở kỳ sau vẫn theo điều kiện gia hạn. Nút này không hoàn tác lần đổi chu kỳ đã có hiệu lực.

#### AC-058

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản đang dùng PRO, có yêu cầu chuyển xuống BASIC đang chờ từ kỳ tiếp theo. PLUS cũng là gói đích hạ hợp lệ trong dữ liệu thử.
- **When**: Khách muốn đổi gói đích từ BASIC sang PLUS.
- **Then**: Khách phải hủy yêu cầu BASIC thành công rồi tạo yêu cầu PLUS. Thao tác sửa/thay thế trực tiếp bị từ chối, kể cả gửi trực tiếp; yêu cầu BASIC giữ nguyên nếu chưa hủy.
- **And**: Sau khi hủy, BASIC không còn được áp dụng theo yêu cầu cũ. Chỉ ghi nhận PLUS khi yêu cầu mới hợp lệ, áp dụng từ kỳ tiếp theo mà không đổi ngày hết hạn hiện tại. Nếu chưa tạo được yêu cầu mới, không tự khôi phục BASIC. Các tên gói chỉ là ví dụ, không xác định thứ tự gói bán chính thức.

#### AC-059

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản dùng BASIC tháng, dự kiến hết hạn 30/9/2026 lúc 10:00. Khách chọn nâng lên PRO năm; giá kỳ năm đã xác định hợp lệ là 600.000đ, hạn mức năm là 100 lượt tạo.
- **When**: Việc nâng gói kèm đổi chu kỳ hoàn tất hợp lệ ngày 15/9/2026 lúc 10:00, giờ Việt Nam.
- **Then**: Kỳ PRO năm bắt đầu ngay tại mốc này, kết thúc 15/9/2027 lúc 10:00; không chờ 30/9. Số tiền phải trả là đủ 600.000đ, cấp 100 lượt dùng cả năm và không làm mới hàng tháng.
- **And**: Bỏ lượt dư kỳ cũ, không cộng thời gian cũ hoặc khấu trừ tiền theo phần thời gian còn lại. Lượt đã giữ vẫn xử lý ở kỳ cũ. Khi nâng từ chu kỳ năm sang tháng, dùng đúng giá, hạn mức và thời hạn tháng của gói đích theo cùng quy tắc nâng. Các số chỉ là dữ liệu thử; tiêu chí không thực thi thanh toán hoặc xác định thứ tự gói.

#### AC-060

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản dùng PRO năm đến 30/9/2027 lúc 10:00 theo giờ Việt Nam. Ngày 15/3, khách chọn hạ xuống BASIC tháng trong một yêu cầu; BASIC tháng là lựa chọn hợp lệ, cấp 10 lượt tạo.
- **When**: Hệ thống ghi nhận yêu cầu, sau đó xử lý tại mốc hết kỳ năm khi yêu cầu chưa bị hủy và đủ điều kiện chuyển hợp lệ.
- **Then**: Trước mốc hết kỳ vẫn dùng PRO năm. Kỳ BASIC tháng bắt đầu 30/9/2027 lúc 10:00, kết thúc 30/10/2027 lúc 10:00; bỏ lượt dư cũ và cấp 10 lượt tháng. Không bắt đầu một kỳ BASIC năm trung gian.
- **And**: Chiều PRO tháng xuống BASIC năm cũng cho chọn trong một yêu cầu và chờ hết kỳ hiện tại; cấp hạn mức năm dùng cả kỳ. Nếu hủy yêu cầu đang chờ thì không thực hiện cả đổi gói lẫn đổi chu kỳ theo yêu cầu đó. Tác vụ giữ lượt vẫn xử lý ở kỳ cũ. Các tên, số liệu chỉ là dữ liệu thử; điều kiện và thời điểm thu tiền vẫn thiết kế sau.

#### AC-061

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản đang dùng PLUS, có yêu cầu hạ xuống BASIC đang chờ đến cuối kỳ. PRO là gói nâng hợp lệ theo thứ tự Admin đặt. Các tên gói chỉ là dữ liệu thử.
- **When**: Khách gửi yêu cầu nâng lên PRO mà chưa hủy yêu cầu hạ, kể cả gửi trực tiếp.
- **Then**: Từ chối nâng và báo phải hủy yêu cầu hạ trước. Yêu cầu BASIC vẫn đang chờ, kỳ PLUS, quyền và lượt giữ nguyên; không tự thay thế yêu cầu cũ.
- **And**: Sau khi hủy yêu cầu BASIC thành công, khách được gửi yêu cầu nâng PRO mới theo điều kiện hợp lệ; không tự nâng chỉ vì hủy. Nếu chưa nâng hoặc nâng không hoàn tất, BASIC vẫn đã hủy. Quy tắc cũng áp dụng cho yêu cầu hạ cũ có kèm đổi chu kỳ.

#### AC-062

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Tài khoản dùng PLUS tháng, có yêu cầu chuyển sang PLUS năm đang chờ đến cuối kỳ. PRO là gói nâng hợp lệ theo thứ tự Admin đặt.
- **When**: Khách yêu cầu nâng PRO khi chưa hủy yêu cầu đổi chu kỳ, kể cả gửi trực tiếp.
- **Then**: Từ chối nâng và báo phải hủy yêu cầu đổi chu kỳ trước. Giữ nguyên yêu cầu PLUS năm, kỳ PLUS tháng, quyền và lượt; không tự thay thế hoặc hủy yêu cầu cũ.
- **And**: Sau khi khách hủy thành công, kiểm tra yêu cầu nâng mới theo điều kiện hợp lệ. Không tự nâng vì đã hủy, không tự khôi phục yêu cầu đổi chu kỳ nếu chưa nâng hoặc nâng không hoàn tất. Quy tắc tương tự áp dụng khi đang chờ đổi năm sang tháng. Tên gói chỉ là dữ liệu thử.

#### AC-063

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Yêu cầu chuyển PLUS xuống BASIC đã được ghi nhận hợp lệ ngày 20/9, dự kiến áp dụng ngày 30/9 lúc 10:00. Ngày 25/9, Admin ngừng bán BASIC.
- **When**: Đến mốc chuyển, yêu cầu chưa bị hủy và đã đủ các điều kiện chuyển hợp lệ khác.
- **Then**: Vẫn chuyển sang BASIC theo yêu cầu đã ghi nhận; không hủy hoặc từ chối chỉ vì BASIC ngừng bán. Trước mốc chuyển vẫn giữ kỳ PLUS hiện tại.
- **And**: Yêu cầu mới tạo sau lúc BASIC ngừng bán bị chặn, kể cả khách hủy yêu cầu cũ rồi chọn lại. Ngoại lệ này không tự xác nhận thanh toán, không quyết định gia hạn các kỳ sau hoặc cho đăng ký mới vào gói ngừng bán. Các tên và ngày chỉ là ví dụ.

#### AC-064

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Khách có kỳ thiết kế còn hiệu lực và một yêu cầu chuyển đang chờ đến cuối kỳ.
- **When**: Khách đang chờ hạ gói nhưng muốn chỉ đổi chu kỳ trong cùng gói, hoặc đang chờ chỉ đổi chu kỳ nhưng muốn hạ gói.
- **Then**: Phải hủy yêu cầu cũ thành công trước khi tạo yêu cầu mới. Khi chưa hủy, từ chối yêu cầu mới, kể cả gửi trực tiếp; giữ nguyên yêu cầu cũ, kỳ hiện tại, quyền và lượt.
- **And**: Sau khi hủy, yêu cầu mới được kiểm tra điều kiện hợp lệ và vẫn áp dụng từ cuối kỳ hiện tại. Hủy không tự tạo yêu cầu mới hoặc gia hạn; nếu chưa tạo được yêu cầu mới, không tự khôi phục yêu cầu cũ. Sửa riêng chu kỳ của gói đích trong yêu cầu hạ đang chờ được kiểm tra ở AC-065.

#### AC-065

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Khách có kỳ PRO còn hiệu lực và yêu cầu hạ xuống BASIC tháng đang chờ cuối kỳ; tên gói chỉ là ví dụ.
- **When**: Khách muốn giữ BASIC nhưng đổi chu kỳ đích thành năm.
- **Then**: Phải hủy yêu cầu cũ thành công rồi tạo lại. Từ chối sửa trực tiếp, kể cả gửi trực tiếp; khi chưa hủy, giữ nguyên yêu cầu cũ, kỳ hiện tại, quyền và lượt.
- **And**: Yêu cầu tạo lại phải đáp ứng điều kiện hiện tại và vẫn chờ hết kỳ; BASIC đã ngừng bán thì chặn tạo lại theo BR-SUB-013. Không tự gia hạn hoặc khôi phục yêu cầu đã hủy khi chưa tạo được yêu cầu mới. Quy tắc áp dụng cho việc đổi tháng/năm của gói đích trong yêu cầu hạ đang chờ.

#### AC-066

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Yêu cầu hạ gói hoặc chỉ đổi chu kỳ hợp lệ đang chờ cuối kỳ, đã chốt bản A gồm giá và toàn bộ quyền lợi. Sau đó Admin công bố bản B khác A trước ngày chuyển.
- **When**: Đến mốc chuyển, yêu cầu chưa hủy và đủ điều kiện chuyển hợp lệ.
- **Then**: Kỳ đích dùng đúng giá, hạn mức, quyền bật/tắt và mức tính năng của A; không lấy B hoặc ghép quyền từ hai bản. Bản quyền lợi của kỳ hiện tại giữ nguyên trong lúc chờ.
- **And**: Ví dụ A có giá 200.000đ và 10 lượt, B có giá 250.000đ và 8 lượt thì yêu cầu vẫn dùng A. Quy tắc áp dụng cho cả hạ gói kèm đổi chu kỳ và chỉ đổi chu kỳ. Không tự xác nhận thanh toán hoặc quyết định bản cho các kỳ gia hạn về sau.

#### AC-067

- **Given**: Tài khoản có kỳ A còn hiệu lực; B là gói đích hợp lệ, có thể đắt hơn, rẻ hơn hoặc bằng giá A, nhiều hoặc ít quyền hơn.
- **When**: Đổi sang B hoàn tất hợp lệ tại T, có hoặc không kèm đổi chu kỳ.
- **Then**: Kỳ A kết thúc, B bắt đầu ngay tại T; chỉ một gói hiệu lực, không so bậc/giá hoặc chờ cuối kỳ. Tính đủ chu kỳ từ T, trả đủ giá đích, cấp đủ hạn mức, bỏ thời gian và lượt dư A.
- **And**: Lượt đã giữ vẫn quyết toán ở A, không cộng vào B; không xóa dữ liệu cũ. Trước khi hoàn tất không đổi kỳ hoặc cấp lượt. Mốc chốt giá và thanh toán còn cần thiết kế; quyền đầu ra tác vụ đang chạy theo AC-069.

#### AC-068

- **Given**: Tài khoản dùng PLUS năm còn 10 tháng và 25 lượt dư; khách chọn PLUS tháng giá 100.000đ, hạn mức 10 lượt. Các số là dữ liệu thử.
- **When**: Đổi chu kỳ hoàn tất hợp lệ ngày 18/9/2026 lúc 10:00 giờ Việt Nam.
- **Then**: Kỳ tháng bắt đầu ngay, kết thúc 18/10/2026 lúc 10:00. Bỏ 10 tháng và 25 lượt dư, cấp 10 lượt, trả đủ 100.000đ; không khấu trừ thời gian cũ hoặc giữ lịch chuyển cuối kỳ.
- **And**: Chiều tháng sang năm cũng áp dụng ngay, tính đủ năm từ T và cấp hạn mức cả năm, không làm mới mỗi tháng. Lượt đang giữ thuộc kỳ cũ; không có hai gói đồng thời hiệu lực.

#### AC-069

**Không nghiệm thu trong phạm vi hiện tại:** phần kiểm tra AI theo quyền 3D hoặc giới hạn danh mục đúng ba quyền đã được thay thế bởi STORY-SUB-002/AC-025. Nội dung giữ để tra lịch sử.

- **Given**: Tác vụ AI J đã được tiếp nhận hợp lệ ở kỳ A có quyền tạo thiết kế và 3D; J giữ một lượt của A. Gói B có quyền tạo thiết kế nhưng không có 3D.
- **When**: Khách đổi sang B hoàn tất khi J đang chạy; sau đó J thành công, đủ kết quả đã lưu và mở được, trước khi bị xử lý timeout.
- **Then**: J vẫn trả đủ kết quả gồm 3D theo quyền lúc tiếp nhận và tính lượt vào A. Không trừ lượt của B. Yêu cầu tạo mới hợp lệ sau khi chuyển dùng quyền B, không tạo 3D.
- **And**: Nếu J lỗi, giải phóng lượt ở A, không cộng vào B. Nếu đã timeout, không trả kết quả muộn. Tác vụ tiếp nhận ở gói không có 3D không tự được bổ sung 3D khi chuyển sang gói có quyền. Đổi chu kỳ cũng không thay bộ quyền đã ghi nhận cho tác vụ.

#### AC-070

- **Given**: Khách dùng PLUS tháng còn 20 ngày; chuẩn bị hai trường hợp hết lượt hoặc còn 3 lượt dư. Kỳ mua lại hợp lệ có giá 100.000đ và 10 lượt; đây là dữ liệu thử.
- **When**: Mua lại PLUS tháng hoàn tất ngày 18/9/2026 lúc 10:00 giờ Việt Nam.
- **Then**: Kỳ cũ kết thúc; kỳ tháng mới bắt đầu ngay, hết 18/10/2026 lúc 10:00. Trả đủ 100.000đ, cấp 10 lượt, không cộng 3 lượt hoặc 20 ngày cũ. Chỉ một subscription hiệu lực.
- **And**: Trước hoàn tất không làm mới lượt hoặc mất kỳ cũ. Lượt giữ và quyền tác vụ đang chạy vẫn thuộc kỳ cũ; giữ dữ liệu/lịch sử. Áp dụng tương tự cho mua lại năm: cấp đủ hạn mức cả năm, không reset hàng tháng. Không tự mua lại hoặc biến thành mua thêm lượt.

#### AC-071

- **Given**: Khách đủ điều kiện tra cứu; hệ thống đã ghi nhận lần mở K thành công và tính 1 lượt, nhưng phản hồi bị mất do ngắt mạng.
- **When**: Khách gửi lại cùng lần mở K.
- **Then**: Trả lại kết quả của K, giữ đúng 1 lượt đã tính và không trừ thêm. Không coi retry truyền mạng là lần mở mới.
- **And**: Chủ động mở lại mẫu bằng lần mở mới tính lượt mới nếu đủ điều kiện. Lỗi tải/chuẩn bị nội dung trước khi ghi nhận thành công không tính lượt. Không cho tài khoản khác dùng K để lấy nội dung hoặc thao tác với số dư.

## References

### TDDs

- [TDD-SUB-002](../tdd/TDD-SUB-002.md): Thiết kế kỹ thuật bản nháp; Reviewer/Approver Tân Trần.

### Rules

- BR-SUB-013/Then: Ngừng bán chặn yêu cầu mới và giữ gói đã cấp; giao dịch đang xử lý thiết kế sau.

- BR-SUB-021/Statement: Đổi gói và chu kỳ ngay; bỏ phân loại nâng/hạ và lịch chuyển.



- BR-SUB-008/Then: Quyền tạo phối cảnh 3D riêng theo gói; dự toán nội thất và bố trí công năng có cùng mức chi tiết giữa các gói.

- BR-SUB-017/Statement: Chỉ có hai loại lượt tạo mới và tra cứu; loại bỏ lượt chỉnh sửa.

- BR-SUB-001/Statement: Quyền và hạn mức dùng chung theo tài khoản.
- BR-SUB-002/Statement: Làm mới hạn mức theo kỳ, không cộng dồn lượt dư.
- BR-SUB-003/Statement: Giữ trước lượt tạo thiết kế, chỉ tính lượt khi thành công.

- BR-SUB-004/Statement: Kỳ mới dùng bản quyền lợi đã chốt khi đăng ký/gia hạn, giữ nguyên kỳ hiện tại.

- BR-SUB-005/Statement: Mức cao bao gồm mức thấp của cùng quyền lợi.

- BR-SUB-006/Statement: Thiết kế giới hạn theo tài khoản, giám sát giới hạn theo công trình.

- BR-SUB-007/Statement: Xem dữ liệu cũ, tải tệp đã có sau hết hạn; chặn thao tác mới cần subscription.

- BR-SUB-014/Statement: Kỳ thiết kế tính từ ngày bắt đầu, điều chỉnh về cuối tháng khi thiếu ngày.

- BR-SUB-015/Statement: Giá/hạn mức riêng tháng/năm; đổi chu kỳ ngay theo BR-SUB-021.

- BR-SUB-016/Statement: Tự xử lý tác vụ quá thời gian, kết quả muộn không tự trừ lượt lại.

### Dependencies

- BR-SUB-011/Statement: Giám sát không áp dụng chu kỳ, hết hạn và cơ chế lượt thiết kế.

- STORY-SUB-003: Quyền giám sát gắn với công trình.

## Non-Functional

- [Chưa chốt yêu cầu đo được về hiệu năng và xử lý đồng thời.]

## Out of Scope

- So sánh các kết quả thiết kế đã hoãn; không triển khai giao diện so sánh hoặc quyền riêng theo gói trong đợt này. Khách vẫn xem từng kết quả riêng. Xem [nợ nghiệp vụ](../debt/design-comparison.md).

- AI gợi ý giảm chi phí/tối ưu ngân sách đã hoãn; không triển khai tính năng hoặc quyền riêng cho phần này trong đợt hiện tại. Dự toán nội thất vẫn giữ như đã chốt. Xem [nợ nghiệp vụ](../debt/ai-budget-optimization.md).

- Chỉnh sửa thiết kế sau khi Gen AI trả kết quả và toàn bộ cơ chế lượt chỉnh sửa đã bị loại bỏ theo quyết định mới nhất. AC-026 đến AC-029 đã rút khỏi bộ tiêu chí; không tái sử dụng các mã này.

- Khách hàng hủy tác vụ tạo thiết kế đang chạy: chưa hỗ trợ trong đợt này; không bổ sung nút hoặc API hủy dành cho khách hàng. Việc đóng trang không phải yêu cầu hủy tác vụ.

- Tích hợp thanh toán, luồng tài khoản nhận gói và gia hạn thuộc giai đoạn sau.
- Bản nháp này chưa quy định giá bán cụ thể hoặc cách cấp/gia hạn. Tra cứu thuộc phạm vi hiện tại: mỗi lần trả được nội dung chi tiết mẫu tính 1 lượt, lỗi không tải được nội dung thì không mất lượt; tìm kiếm và xem danh sách không dùng lượt. Không hỗ trợ nhiều subscription thiết kế cùng tài khoản hoặc nhiều gói giám sát cùng công trình đồng thời có hiệu lực.
