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

# STORY-PROJ-007

## Metadata

- **Story**: Là khách hàng, tôi muốn chọn nhiều dự toán của mình để xóa cùng lúc và biết kết quả từng bản, nhằm bỏ những bản không còn cần dùng.
- **Context**: Người dùng xác nhận xóa các bản đủ điều kiện, chặn bản đang được AI xử lý, không hoàn lượt và vẫn cho xóa khi hết hạn gói hoặc hết lượt. Chọn tất cả chỉ áp dụng cho trang đang xem. Xóa thành công thì không mở lại, không khôi phục và ngừng truy cập qua link đã chia sẻ. Người dùng đã chốt bộ US/BR trong hội thoại ngày 30/09/2026; đã đồng ý đánh dấu xóa và giữ dữ liệu nội bộ, chưa tự động dọn tệp hoặc đặt thời hạn lưu. Thiết kế API ở TDD-PROJ-005, chưa triển khai. Metadata phân công còn thiếu.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Khách đã đăng nhập bằng tài khoản khách hàng theo BR-RBAC-005.
- Khách đã mở danh sách theo STORY-PROJ-006 và chọn ít nhất một bản để yêu cầu xóa.
- Không yêu cầu gói còn hiệu lực hoặc còn lượt. Quyền sở hữu và tình trạng AI được kiểm tra riêng cho từng bản lúc xử lý theo BR-PROJ-009.

### Trigger

Khách yêu cầu xóa các dự toán đã chọn trên trang đang xem.

## Flow

### Main Flow

1. Khách chọn một hoặc nhiều dự toán trên trang đang xem; Chọn tất cả chỉ chọn các bản của trang đó theo BR-PROJ-009.
2. Giao diện thể hiện các bản đang chọn và thông báo xóa xong không thể khôi phục, đồng thời hồ sơ đã chia sẻ sẽ không còn truy cập được.
3. Khách yêu cầu xóa các bản đã chọn.
4. Backend kiểm tra phiên khách hàng và kiểm tra lại từng bản: thuộc người gọi, chưa bị xóa và không đang được AI xử lý.
5. Hệ thống xóa các bản đủ điều kiện; giữ nguyên các bản không đủ điều kiện và báo kết quả riêng cho từng bản. Không từ chối toàn bộ chỉ vì một bản đang xử lý AI.
6. Với mỗi bản xóa thành công, hệ thống loại khỏi danh sách, ngăn chủ sở hữu mở lại và ngăn yêu cầu xem, tải mới qua link chia sẻ, QR hoặc link trong email.
7. Hệ thống không hoàn lượt đã dùng, không giữ hoặc trừ lượt mới và không hủy tác vụ AI.
8. Giao diện cập nhật danh sách theo kết quả thực tế, cho biết bản nào đã xóa và bản nào không xóa được cùng lý do có thể công bố.

### Alternative Flow

#### ALT-01

Danh sách chọn có cả bản đủ điều kiện và bản đang được AI xử lý.

1. Xóa các bản đủ điều kiện.
2. Giữ nguyên các bản đang xử lý, báo rõ không thể xóa khi AI đang chạy; tác vụ tiếp tục theo quy tắc hiện có.
3. Không báo tất cả đã xóa và không hoàn tác các bản đã xóa thành công chỉ vì có bản bị chặn.

#### ALT-02

Khách đã hết hạn gói hoặc hết lượt.

1. Vẫn kiểm tra và xử lý xóa từng bản theo quyền sở hữu, tình trạng chưa xóa và trạng thái AI.
2. Không yêu cầu gia hạn hoặc mua thêm lượt để xóa; không thay đổi số lượt đã dùng.

#### ALT-03

Khách chỉ chọn một bản hoặc toàn bộ bản đã chọn đều không đủ điều kiện.

1. Với một bản, áp dụng cùng quy tắc kiểm tra và trả kết quả cho bản đó.
2. Khi không bản nào đủ điều kiện, không xóa bản nào và báo rõ kết quả; không ghi nhận thành công giả.

### Exception Flow

#### EXC-01

Phiên đăng nhập không hợp lệ hoặc người gọi dùng tài khoản nhân viên.

1. Từ chối yêu cầu trước khi xóa và không thay đổi dự toán.

#### EXC-02

Yêu cầu chứa bản không thuộc khách, không tồn tại hoặc đã bị xóa, kể cả do giao diện cũ hoặc yêu cầu gửi trực tiếp.

1. Không xóa dữ liệu của người khác và không làm bản đã xóa xuất hiện trở lại.
2. Báo tình trạng không thể thao tác mà không làm lộ tên, trạng thái AI hoặc thông tin riêng của bản thuộc khách khác.
3. Tiếp tục xử lý các bản đủ điều kiện trong cùng yêu cầu.

#### EXC-03

Dự toán bắt đầu được AI xử lý sau lúc khách chọn nhưng trước lúc hệ thống xử lý xóa bản đó.

1. Kiểm tra lại trạng thái và chặn xóa bản đang được xử lý.
2. Giữ tác vụ, đầu vào và lượt đang giữ theo quy tắc hiện có; vẫn xử lý các bản khác đủ điều kiện.

#### EXC-04

Lỗi mạng hoặc máy chủ khiến chưa xác định được đầy đủ kết quả của lần xóa nhiều.

1. Không báo toàn bộ đã xóa hoặc toàn bộ còn nguyên khi chưa có căn cứ.
2. Thông báo chưa xác định được đầy đủ kết quả và cho khách tải lại danh sách để kiểm tra các bản còn lại.
3. Không khôi phục các bản đã xóa thành công hoặc hoàn lượt do lỗi phản hồi.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đang xem một trang trong danh sách có nhiều trang.
- **When**: Bấm Chọn tất cả rồi yêu cầu xóa các bản đã chọn.
- **Then**: Chỉ các bản của trang đang xem được đưa vào lựa chọn; không tự chọn toàn bộ kết quả ở các trang khác.
- **And**: Không xóa bản ngoài tập mà khách đã chọn; giao diện thông báo không thể khôi phục trước khi khách yêu cầu xóa.

#### AC-002

- **Given**: Khách chọn các bản nháp, thành công hoặc thất bại thuộc mình, chưa bị xóa và không có AI đang xử lý.
- **When**: Yêu cầu xóa.
- **Then**: Xóa các bản đủ điều kiện và trả kết quả cho từng bản theo BR-PROJ-009.
- **And**: Áp dụng được khi chọn một hoặc nhiều bản; không yêu cầu tất cả có cùng trạng thái.

#### AC-003

- **Given**: Khách chọn 10 bản thuộc mình; lúc xử lý, 8 bản đủ điều kiện và 2 bản đang được AI xử lý.
- **When**: Hệ thống xử lý yêu cầu xóa nhiều.
- **Then**: Xóa 8 bản đủ điều kiện, giữ nguyên 2 bản đang xử lý và báo rõ kết quả từng bản.
- **And**: Không hủy 2 tác vụ AI, không hoàn tác 8 bản đã xóa và không báo cả 10 bản đã xóa.

#### AC-004

- **Given**: Các bản được chọn đều đang được AI xử lý.
- **When**: Yêu cầu xóa.
- **Then**: Không xóa bản nào và báo lý do bị chặn cho các bản thuộc khách.
- **And**: AI tiếp tục theo quy tắc hiện hành; không giải phóng lượt giữ chỉ vì yêu cầu xóa bị từ chối.

#### AC-005

- **Given**: Khách đã hết hạn gói hoặc hết lượt, có bản thuộc mình đủ điều kiện xóa.
- **When**: Yêu cầu xóa bản đó.
- **Then**: Cho xóa mà không yêu cầu gia hạn hoặc bổ sung lượt.
- **And**: Không mở quyền sửa đầu vào, gửi AI hoặc sử dụng gói mới.

#### AC-006

- **Given**: Bản dự toán đã tạo thành công và đã tính lượt.
- **When**: Chủ sở hữu xóa bản thành công.
- **Then**: Số lượt đã dùng giữ nguyên, không hoàn lượt.
- **And**: Thao tác xóa không giữ hoặc tính thêm lượt và không tạo tác vụ AI mới.

#### AC-007

- **Given**: Một bản vừa được xóa thành công.
- **When**: Chủ sở hữu tải lại danh sách hoặc mở lại bản qua đường dẫn đã lưu.
- **Then**: Bản không còn trong danh sách, không mở được đầu vào, kết quả hoặc hồ sơ của bản đã xóa.
- **And**: Không có thùng rác, chức năng khôi phục hoặc đường thao tác khác làm bản xuất hiện trở lại.

#### AC-008

- **Given**: Bản đã có link chia sẻ chưa hết hạn, được dùng qua link trực tiếp, QR hoặc email.
- **When**: Chủ sở hữu xóa thành công, sau đó người nhận gửi yêu cầu xem hoặc tải hồ sơ qua link đó.
- **Then**: Từ chối truy cập mới; ngày hết hạn cũ của link không làm hồ sơ đã xóa còn khả dụng.
- **And**: Không thu hồi được tệp mà người nhận đã tải về trước đó.

#### AC-009

- **Given**: Yêu cầu xóa có bản thuộc khách đang gọi và bản không thuộc khách, không tồn tại hoặc đã xóa.
- **When**: Backend xử lý yêu cầu trực tiếp.
- **Then**: Chỉ xóa các bản đủ điều kiện thuộc người gọi; báo kết quả riêng đối với các bản không thể thao tác.
- **And**: Không làm lộ tên, trạng thái AI hoặc nội dung của dự toán thuộc tài khoản khác.

#### AC-010

- **Given**: Một bản không đang xử lý khi tải danh sách nhưng đã được tiếp nhận AI trước lúc xử lý xóa.
- **When**: Backend kiểm tra bản đó để xóa.
- **Then**: Chặn xóa theo trạng thái hiện tại, không dựa vào trạng thái cũ từ giao diện.
- **And**: Những bản khác đủ điều kiện vẫn được xóa; bản đã xóa không thể tiếp nhận yêu cầu AI mới.

#### AC-011

- **Given**: Người gọi chưa đăng nhập, phiên không hợp lệ hoặc dùng tài khoản nhân viên.
- **When**: Gửi yêu cầu xóa một hoặc nhiều dự toán.
- **Then**: Từ chối yêu cầu và không xóa dữ liệu.
- **And**: Không trả nội dung dự toán của khách hàng.

#### AC-012

- **Given**: Lỗi mạng hoặc máy chủ khiến khách chưa nhận đủ kết quả của lần xóa nhiều.
- **When**: Giao diện phản hồi lỗi.
- **Then**: Thông báo chưa xác định được đầy đủ kết quả và cho tải lại danh sách.
- **And**: Không tự kết luận tất cả đã xóa hoặc tất cả còn nguyên; các bản đã xóa thành công không được khôi phục hay hoàn lượt.

## References

### TDDs

- TDD-PROJ-005: Thiết kế xóa bằng dấu thời gian, biên nhận từng lô và chặn mọi truy cập mới; đã được người dùng chốt trong hội thoại ngày 30/09/2026, chưa triển khai.

### Rules

- BR-PROJ-009
- BR-PROJ-008
- BR-PROJ-005
- BR-PROJ-006
- BR-SUB-003
- BR-SUB-007
- BR-RBAC-005

### Dependencies

- STORY-PROJ-006: Chọn các dự toán trên trang danh sách đang xem.
- STORY-PROJ-002: Trạng thái AI và kết quả của tác vụ đang chạy.
- STORY-PROJ-004: Quyền truy cập qua link, QR và email.

## Non-Functional

- Kiểm quyền sở hữu và trạng thái lúc xử lý từng bản ở backend, kể cả khi gửi yêu cầu trực tiếp hoặc thao tác từ nhiều phiên.
- Kết quả từng bản phải phản ánh dữ liệu đã được xử lý thành công; không dùng một thông báo thành công chung che các bản bị chặn hoặc lỗi chưa xác định.
- Quyền truy cập bản đã xóa phải được kiểm tra trên cả đường của chủ sở hữu và đường chia sẻ; không chỉ ẩn một dòng trên giao diện.
- Hợp đồng kết quả từng bản, xử lý yêu cầu gửi lại và lỗi kỹ thuật sẽ được làm rõ trong TDD; chưa cam kết ngưỡng thời gian hoặc số bản mỗi yêu cầu.

## Out of Scope

- Thùng rác, khôi phục, hủy tác vụ AI hoặc hoàn lượt do xóa dự toán.
- Xóa toàn bộ kết quả tìm kiếm ở các trang khác bằng Chọn tất cả.
- Xóa dự toán của khách khác hoặc bổ sung quyền quản trị dự toán cho nhân viên.
- Cam kết xóa vật lý mọi dữ liệu hoặc tệp tại dịch vụ AI. Đã thống nhất giữ dữ liệu nội bộ để đối soát, không cho khách khôi phục. Đợt này chưa tự động dọn tệp hoặc đặt thời hạn lưu.
