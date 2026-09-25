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

# STORY-PUSH-001

## Metadata

- **Story**: Là khách hàng dùng mobile BMT, tôi muốn đăng ký nhận thông báo trên các thiết bị còn đăng nhập để hệ thống có thể gửi thông báo đến đúng tài khoản của tôi.
- **Context**: Phạm vi đã được người dùng chốt trong hội thoại: mobile chỉ dành cho khách hàng, hỗ trợ Android và iOS; lưu/cập nhật ExpoPushToken và chuẩn bị nền tảng gửi push, chưa gắn loại thông báo với sự kiện nghiệp vụ. Backend hiện có nhiều phiên Redis nhưng chưa có đăng ký token thiết bị. Đây là bản nháp tài liệu hóa phạm vi đã chốt, chưa triển khai.
- **Sprint**:
- **Priority**: Must
- **Status**: Todo
- **Creator**: Codex
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa phân công]
  - Mobile: [Chưa phân công]
  - QA: [Chưa phân công]

## Conditions

### Preconditions

- Khách hàng sử dụng mobile BMT trên Android hoặc iOS.
- Việc đăng ký token cho một tài khoản cần phiên đăng nhập hợp lệ của tài khoản đó.
- Việc gửi push cần cấu hình Expo và thông tin xác thực nền tảng tương ứng; cấu hình cụ thể sẽ được xác định trong TDD.

### Trigger

Khách hàng đăng nhập thành công; app đồng bộ đăng ký khi token hoặc quyền thông báo thay đổi. Nền tảng gửi xử lý khi nhận được yêu cầu gửi hợp lệ từ phía backend.

## Flow

### Main Flow

1. Khách hàng đăng nhập thành công trên app.
2. App kiểm tra quyền thông báo và lấy ExpoPushToken cho bản cài hiện tại khi được phép.
3. App gửi đăng ký token tới backend bằng phiên đã xác thực.
4. Backend xác định tài khoản từ phiên, lưu hoặc cập nhật đăng ký cho bản cài; gửi lại cùng đăng ký không tạo thêm đích gửi trùng.
5. Khi có yêu cầu gửi hợp lệ, backend chọn tất cả đăng ký của khách hàng đã bật thông báo và còn đăng nhập.
6. Bộ gửi nền kiểm tra lại điều kiện nhận trước khi chuyển yêu cầu sang Expo, theo dõi ticket/receipt và cập nhật kết quả xử lý.

### Alternative Flow

#### ALT-01

Khách hàng sử dụng nhiều thiết bị.

1. Mỗi bản cài app đăng ký token riêng.
2. Mọi thiết bị đã bật thông báo và còn đăng nhập đều là đích nhận; đăng nhập thiết bị mới không loại thiết bị cũ.

#### ALT-02

Token hoặc quyền thông báo thay đổi.

1. App đồng bộ trạng thái mới với backend.
2. Backend cập nhật đăng ký hiện tại; token cũ không tiếp tục được dùng sau khi được thay thế. Đăng ký không còn được phép nhận thì bị loại khỏi các lần gửi tiếp theo.

#### ALT-03

Khách hàng đăng xuất hoặc đổi tài khoản trên cùng thiết bị.

1. Khi backend xử lý đăng xuất thành công, đăng ký thuộc phiên đó ngừng đủ điều kiện nhận thông báo của tài khoản cũ.
2. Nếu đăng nhập tài khoản B sau tài khoản A, đăng ký mới chỉ nhận thông báo của B. Các thiết bị khác còn đăng nhập A vẫn nhận của A.
3. Yêu cầu cập nhật hoặc đăng xuất đến muộn của phiên A không được ghi đè hoặc vô hiệu hóa đăng ký mới của B.

### Exception Flow

#### EXC-01

Người dùng không cấp quyền thông báo hoặc app chưa lấy/đồng bộ được token.

1. Không đưa bản cài chưa đủ điều kiện vào danh sách gửi.
2. Đăng nhập vẫn thành công; app có thể đồng bộ lại khi điều kiện cho phép.

#### EXC-02

Expo hoặc mạng gặp lỗi trong quá trình gửi.

1. Ghi nhận lỗi và xử lý thử lại có giới hạn đối với lỗi tạm thời; thông số cụ thể thuộc thiết kế kỹ thuật.
2. Nếu Expo trả DeviceNotRegistered, dừng gửi đến token tương ứng cho tới khi đăng ký lại hợp lệ.
3. Phản hồi lỗi đến muộn của token cũ không làm vô hiệu hóa token mới.

#### EXC-03

Phiên đã hết hiệu lực, bị thu hồi hoặc không xác minh được khi chuẩn bị gửi.

1. Không chuyển thông báo tới đăng ký chưa xác minh đủ điều kiện nhận.
2. Cách phân biệt bỏ qua và tạm hoãn, cùng cơ chế phục hồi khi dịch vụ phiên lỗi, sẽ được quy định trong TDD.

## Acceptance Criteria

#### AC-001

- **Given**: Khách hàng đăng nhập hợp lệ trên Android hoặc iOS và đã bật thông báo.
- **When**: App gửi token của bản cài hiện tại.
- **Then**: Backend lưu đăng ký gắn với tài khoản và phiên được xác thực.
- **And**: Tài khoản nhân viên không được đăng ký nhận push qua tính năng mobile dành cho khách hàng.

#### AC-002

- **Given**: Bản cài đã có đăng ký.
- **When**: App gửi lại cùng đăng ký hoặc cập nhật token.
- **Then**: Không tạo đích gửi trùng; lần gửi tiếp theo dùng token hiện tại.

#### AC-003

- **Given**: Khách hàng có nhiều thiết bị đã bật thông báo và còn đăng nhập.
- **When**: Backend xử lý yêu cầu gửi cho khách hàng đó.
- **Then**: Tất cả thiết bị đủ điều kiện đều được chọn làm đích nhận.

#### AC-004

- **Given**: Khách hàng A đăng nhập trên hai thiết bị.
- **When**: A đăng xuất thành công ở một thiết bị.
- **Then**: Backend ngừng gửi thông báo của A đến đăng ký thuộc phiên vừa đăng xuất.
- **And**: Thiết bị còn đăng nhập A vẫn đủ điều kiện nhận.

#### AC-005

- **Given**: Cùng bản cài đã chuyển từ tài khoản A sang B.
- **When**: Có yêu cầu gửi hoặc yêu cầu cập nhật/đăng xuất đến muộn từ phiên A.
- **Then**: Không gửi thông báo của A qua đăng ký mới của B và không ghi đè đăng ký mới đó.

#### AC-006

- **Given**: App bị từ chối quyền thông báo hoặc chưa đồng bộ token được.
- **When**: Khách hàng đăng nhập.
- **Then**: Lỗi liên quan đến push không làm đăng nhập thất bại.

#### AC-007

- **Given**: Đăng ký không còn đủ điều kiện nhận vì phiên hết hiệu lực, bị thu hồi hoặc quyền thông báo đã được đồng bộ về tắt.
- **When**: Bộ gửi chuẩn bị chuyển thông báo sang Expo.
- **Then**: Đăng ký đó không được gửi, kể cả yêu cầu đã nằm trong hàng đợi từ trước.

#### AC-008

- **Given**: Expo trả DeviceNotRegistered cho token đã gửi.
- **When**: Backend xử lý ticket hoặc receipt tương ứng.
- **Then**: Backend ngừng dùng đúng token lỗi; không làm mất đăng ký mới đã thay thế token đó.

#### AC-009

- **Given**: Yêu cầu gửi có lỗi tạm thời hoặc có phản hồi thành công từ Expo.
- **When**: Bộ gửi xử lý kết quả.
- **Then**: Lỗi tạm thời được xử lý theo chính sách thử lại có giới hạn; kết quả phân biệt việc Expo tiếp nhận với việc FCM/APNs tiếp nhận.
- **And**: Không ghi nhận người dùng đã nhận hoặc đã đọc chỉ từ ticket/receipt thành công.

#### AC-010

- **Given**: Khách hàng cài lại app trên cùng thiết bị; Expo trả lại token đang gắn với đăng ký của bản cài cũ.
- **When**: Khách hàng đăng nhập hợp lệ và app gửi token đó cùng mã bản cài mới.
- **Then**: Backend chuyển token sang đăng ký của bản cài mới, không từ chối đăng ký; lần gửi tiếp theo đến thiết bị qua đăng ký mới.
- **And**: Đăng ký cũ ngừng nhận thông báo; không tạo hai đích gửi cho cùng token.

## References

### TDDs

Các chi tiết Expo trong bộ tài liệu là phương án đề xuất, chưa xác nhận mobile đang dùng Expo hoặc đã có cấu hình push. Cần kiểm tra source mobile trước khi chốt nhà cung cấp và cập nhật đồng bộ Story, BR, TDD và test nếu phương án thay đổi.

Người dùng xác nhận ngày 25/09/2026: đội mobile chưa chọn công nghệ cho app, nên story này tạm hoãn. Chưa viết đặc tả Unit Test và chưa triển khai code cho tới khi chốt app có dùng Expo hay không.

- TDD-PUSH-001/Architecture

### Rules

- BR-PUSH-001/Then
- BR-PUSH-002/Then
- BR-PUSH-003/Then

### Dependencies

## Non-Functional

- Quyền đăng ký và cập nhật phải được kiểm tra bằng phiên xác thực; mã bản cài và token push không phải bằng chứng danh tính.
- Không để lộ token qua phản hồi danh sách công khai hoặc log thông thường.
- Dữ liệu đăng ký phải được lưu bền vững; thời điểm lưu dùng UTC. Thiết kế lưu trữ, ràng buộc chống trùng và liên kết với phiên thuộc TDD.
- Không cam kết giao push đúng một lần hoặc bảo đảm thiết bị đã hiển thị. Không thu hồi được thông báo đã chuyển sang dịch vụ push trước khi đăng xuất.

## Out of Scope

- Thông báo website: **Pending — technical debt**, theo xác nhận của người dùng ngày 2026-09-25. Chưa triển khai trong đợt này; loại thông báo trên website, phạm vi và lịch xử lý sẽ được xác định sau.

- Chọn sự kiện nghiệp vụ, nội dung và lịch gửi thông báo cụ thể.
- Push cho nhân viên, web push, chiến dịch marketing hoặc gửi cho khách chưa đăng nhập.
- Hộp thông báo trong app, trạng thái đã đọc và màn hình quản lý thiết bị.
- Triển khai code, tạo hoặc chạy migration trong bước chuẩn bị tài liệu này.
