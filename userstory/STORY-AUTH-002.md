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

# STORY-AUTH-002

## Metadata

- **Story**: Là khách hàng BMT, tôi muốn đăng nhập bằng Google trên web và app để dùng tài khoản hiện có hoặc tự có tài khoản mới mà không phải điền lại thông tin đăng ký.
- **Context**: Phạm vi và các quy tắc chính được người dùng xác nhận trong hội thoại ngày 30/09/2026. Backend hiện có đăng nhập email/mật khẩu, phiên web bằng cookie và phiên mobile bằng token; chưa thấy luồng đăng nhập Google trong mã nguồn hiện tại. Email không được thay đổi qua chức năng sửa hồ sơ theo BR-AUTH-007. Khi không nhận được mật khẩu tạm, khách dùng Quên mật khẩu. Người dùng xác nhận hiện chỉ có tài khoản thử nghiệm, chưa có tài khoản khách hàng thật. Người dùng đã chốt nội dung Story và BR-AUTH-003 đến BR-AUTH-007 trong hội thoại ngày 30/09/2026. Chưa triển khai; việc chốt trong hội thoại không phải thao tác phê duyệt trên hệ thống tài liệu. Tân Trần là Reviewer và Approver.
- **Sprint**:
- **Priority**: [Chưa xác định]
- **Status**: Todo
- **Creator**: Codex
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa phân công]
  - Frontend: [Chưa phân công]
  - Mobile: [Chưa phân công]
  - QA: [Chưa phân công]

## Conditions

### Preconditions

- Khách có tài khoản Google và dùng web hoặc app BMT.
- Tài khoản BMT đích, nếu đã có, phải thuộc nhóm khách hàng, không bị khóa và chưa xóa.
- Chỉ dùng dữ liệu Google sau khi hệ thống xác thực kết quả do Google cấp; điều kiện xác minh chủ email theo BR-AUTH-003.

### Trigger

Khách chọn đăng nhập bằng Google trên màn hình đăng nhập BMT. Khách cũng có thể dùng mật khẩu tạm nhận qua email để đăng nhập bằng email/mật khẩu.

## Flow

### Main Flow

1. Khách chọn đăng nhập bằng Google trên web hoặc app và hoàn tất bước xác thực của Google.
2. Hệ thống xác thực kết quả Google, tìm liên kết hiện có và kiểm quyền sở hữu email theo BR-AUTH-003, BR-AUTH-004.
3. Nếu chưa có liên kết và email chưa thuộc tài khoản BMT nào, hệ thống tự tạo tài khoản khách hàng, ghi nhận email đã xác minh và cấp vai trò Khách hàng.
4. Hệ thống lấy tên và ảnh Google nếu có theo BR-AUTH-006; không yêu cầu nhập số điện thoại, địa chỉ hoặc đặt mật khẩu để hoàn tất đăng nhập Google.
5. Hệ thống sinh mật khẩu tạm và gửi đến email đã xác minh theo BR-AUTH-005.
6. Hệ thống cấp phiên Google của BMT theo nền tảng đang dùng. Khách sử dụng BMT bình thường, không bị buộc đổi mật khẩu tạm trong phiên Google.
7. Ở các lần đăng nhập bằng cùng Google sau, hệ thống dùng tài khoản đã liên kết; không tự tạo lại tài khoản, sinh lại mật khẩu hoặc ghi đè hồ sơ.

### Alternative Flow

#### ALT-01

Chưa có liên kết Google; email trùng với tài khoản khách hàng đã xác minh đúng email hiện tại.

1. Sau khi kiểm quyền sở hữu email, hệ thống tự liên kết Google với tài khoản BMT đó theo BR-AUTH-004.
2. Giữ mật khẩu, hồ sơ, gói, quyền lợi và dữ liệu của tài khoản.
3. Cấp phiên Google như Main Flow bước 6. Khách vẫn có thể dùng mật khẩu cũ.

#### ALT-02

Chưa có liên kết Google; email trùng với tài khoản khách hàng chưa xác minh email.

1. Hệ thống xác minh chủ email theo BR-AUTH-003 trước khi thay đổi tài khoản.
2. Vô hiệu mật khẩu cũ, các phiên cũ và các mã xác minh/khôi phục cũ có thể khôi phục quyền truy cập; liên kết Google và đánh dấu email đã xác minh theo BR-AUTH-004.
3. Sinh mật khẩu tạm mới, gửi đến email đã xác minh và cấp phiên Google như Main Flow bước 5 và bước 6.

#### ALT-03

Google không đủ căn cứ xác nhận quyền sở hữu email, chẳng hạn tài khoản dùng email của nhà cung cấp khác Gmail hoặc Google Workspace.

1. Hệ thống yêu cầu bước xác minh bổ sung theo BR-AUTH-003. Cách thực hiện và giới hạn của bước này được xác định trong TDD.
2. Chỉ sau khi xác minh thành công mới tiếp tục tạo tài khoản hoặc liên kết. Chưa xác minh thì chưa cấp phiên sử dụng thông thường.

#### ALT-04

Khách đăng nhập bằng email và mật khẩu tạm đã nhận.

1. Hệ thống kiểm mật khẩu và trạng thái tài khoản.
2. Mật khẩu tạm đúng thì khách chỉ được vào bước đổi mật khẩu trước khi dùng các chức năng khác.
3. Khách đặt mật khẩu mới hợp lệ. Khi thành công, mật khẩu tạm mất hiệu lực và các phiên BMT của tài khoản bị thu hồi theo BR-AUTH-005.
4. Khách đăng nhập lại bằng mật khẩu mới hoặc Google. Liên kết Google vẫn còn.

#### ALT-05

Khách không nhận được thư hoặc mất mật khẩu tạm.

1. Khách dùng Quên mật khẩu để nhận mã xác minh và tự đặt mật khẩu mới theo luồng hiện có.
2. Đặt lại thành công thì mật khẩu tạm mất hiệu lực và các phiên cũ bị thu hồi; khách đăng nhập lại bằng mật khẩu mới hoặc Google.

#### ALT-06

Tài khoản đang có mật khẩu tạm chưa đổi nhưng khách chọn đăng nhập Google.

1. Hệ thống xác thực Google và kiểm tài khoản như các lần đăng nhập Google thông thường.
2. Khách dùng BMT theo quyền của tài khoản, không bị bắt đổi mật khẩu chỉ vì mật khẩu tạm còn hiệu lực.

#### ALT-07

Khách đang có phiên sử dụng thông thường và sửa hồ sơ BMT.

1. Khách sửa tên, ảnh hoặc thông tin hồ sơ khác theo chức năng hiện có.
2. Email được giữ nguyên theo BR-AUTH-007. Hệ thống không nhận thay đổi email qua chức năng sửa hồ sơ.

### Exception Flow

#### EXC-01

Khách hủy bước Google hoặc Google không cung cấp kết quả xác thực hợp lệ.

1. Không tạo tài khoản, không liên kết và không cấp phiên. Khách có thể quay lại màn hình đăng nhập và thử lại.

#### EXC-02

Tài khoản đích là nhân viên, bị khóa hoặc đã xóa.

1. Từ chối đăng nhập Google; không tạo tài khoản khách hàng thay thế, chuyển nhóm, mở khóa hoặc khôi phục tài khoản.

#### EXC-03

Khách không hoàn tất bước xác minh email bổ sung.

1. Không tạo tài khoản, không thay mật khẩu, không hủy phiên cũ và không liên kết Google chỉ dựa trên email chưa được xác minh.
2. Khách cần hoàn tất xác minh trước khi tiếp tục.

#### EXC-04

Liên kết Google hiện có mâu thuẫn với tài khoản đích.

1. Không tự gộp tài khoản hoặc ghi đè liên kết; không cấp phiên cho tài khoản khác.
2. Cách thông báo và hỗ trợ trường hợp này còn thuộc phần thiết kế; bản nháp không bổ sung chức năng gỡ hoặc chuyển liên kết.

#### EXC-05

Gửi thư mật khẩu tạm thất bại hoặc thư chưa đến.

1. Khách dùng Quên mật khẩu theo ALT-05 để nhận mã xác minh và tự đặt mật khẩu mới.
2. Không yêu cầu khách dùng chức năng gửi lại mật khẩu tạm. Không báo thư đã đến hộp thư khi mới ghi nhận yêu cầu gửi.

## Acceptance Criteria

#### AC-001

- **Given**: Khách dùng web hoặc app, xác thực Google hợp lệ và đã đáp ứng điều kiện xác minh chủ email; chưa có liên kết hoặc tài khoản BMT tương ứng.
- **When**: Khách hoàn tất đăng nhập Google.
- **Then**: Hệ thống tự tạo tài khoản khách hàng đang hoạt động với email đã xác minh và vai trò Khách hàng.
- **And**: Khách được cấp phiên Google, không phải đặt mật khẩu, nhập số điện thoại hoặc địa chỉ trước khi dùng BMT.

#### AC-002

- **Given**: Một tài khoản Google đã liên kết với tài khoản khách hàng hợp lệ.
- **When**: Khách đăng nhập lại bằng cùng Google trên web hoặc app.
- **Then**: Hệ thống dùng đúng tài khoản đã liên kết và dữ liệu của tài khoản đó.
- **And**: Không tạo thêm tài khoản, không sinh lại mật khẩu tạm hoặc tự ghi đè hồ sơ.

#### AC-003

- **Given**: Google chưa liên kết; email đã xác minh quyền sở hữu trùng tài khoản khách hàng đã xác minh đúng email hiện tại.
- **When**: Khách đăng nhập Google.
- **Then**: Hệ thống tự liên kết vào tài khoản cũ, giữ mã tài khoản, mật khẩu, hồ sơ, gói và dữ liệu.
- **And**: Không yêu cầu khách nhập mật khẩu cũ để liên kết.

#### AC-004

- **Given**: Google chưa liên kết; email đã xác minh quyền sở hữu trùng tài khoản khách hàng chưa xác minh email.
- **When**: Khách hoàn tất đăng nhập Google.
- **Then**: Hệ thống liên kết Google, đánh dấu email đã xác minh, vô hiệu mật khẩu và phiên cũ, rồi cấp phiên Google mới.
- **And**: Hệ thống sinh mật khẩu tạm mới gửi đến email đã xác minh; mã xác minh hoặc khôi phục cũ không khôi phục được quyền truy cập trước đó.

#### AC-005

- **Given**: Google không đủ căn cứ xác nhận quyền sở hữu email và chưa có liên kết Google với BMT.
- **When**: Khách chưa hoàn tất bước xác minh bổ sung.
- **Then**: Hệ thống chưa tạo tài khoản, liên kết, thay mật khẩu hoặc hủy phiên của tài khoản trùng email.
- **And**: Chưa cấp phiên sử dụng thông thường.

#### AC-006

- **Given**: Khách đã nhận mật khẩu tạm và chưa đổi hoặc đặt lại mật khẩu thành công.
- **When**: Khách dùng mật khẩu tạm đúng sau một khoảng thời gian bất kỳ.
- **Then**: Mật khẩu không bị từ chối chỉ vì thời gian đã trôi qua, nếu tài khoản vẫn đủ điều kiện đăng nhập.
- **And**: Phiên này chỉ cho khách đổi mật khẩu trước khi dùng chức năng nghiệp vụ; làm mới phiên không bỏ giới hạn đó.

#### AC-007

- **Given**: Tài khoản khách hàng hợp lệ còn mật khẩu tạm chưa đổi.
- **When**: Khách đăng nhập bằng Google, sau đó làm mới phiên Google.
- **Then**: Khách dùng được các chức năng theo quyền tài khoản mà không bị bắt đổi mật khẩu tạm.
- **And**: Một phiên đăng nhập bằng mật khẩu tạm của cùng tài khoản vẫn phải đổi mật khẩu trước khi tiếp tục.

#### AC-008

- **Given**: Tài khoản có mật khẩu tạm và có các phiên BMT trên web hoặc app.
- **When**: Khách đổi hoặc đặt lại mật khẩu thành công.
- **Then**: Mật khẩu tạm và các phiên BMT cũ không còn dùng được, gồm cả phiên cấp qua Google.
- **And**: Khách đăng nhập lại được bằng mật khẩu mới hoặc Google; liên kết Google không bị gỡ.

#### AC-009

- **Given**: Khách không nhận được thư vì gửi thất bại, thư chưa đến hoặc đã mất mật khẩu tạm, nhưng truy cập được email của tài khoản.
- **When**: Khách hoàn tất Quên mật khẩu và đặt mật khẩu mới.
- **Then**: Khách đăng nhập được bằng mật khẩu mới.
- **And**: Không cần xem lại hoặc gửi lại mật khẩu tạm.

#### AC-010

- **Given**: Google đã xác thực cung cấp tên và ảnh đại diện cho một khách chưa có tài khoản BMT.
- **When**: Hệ thống tạo tài khoản khách hàng.
- **Then**: Hồ sơ ban đầu nhận tên và ảnh đó; thiếu ảnh không chặn đăng nhập.
- **And**: Những lần đăng nhập Google sau không ghi đè tên hoặc ảnh đã lưu tại BMT.

#### AC-011

- **Given**: Tài khoản đích là nhân viên, bị khóa hoặc đã xóa.
- **When**: Có yêu cầu đăng nhập Google tương ứng trên web hoặc app.
- **Then**: Không cấp phiên và không tự tạo tài khoản khách hàng thay thế.
- **And**: Loại tài khoản, trạng thái và dữ liệu hiện có không bị đổi bởi yêu cầu này.

#### AC-012

- **Given**: Khách hủy bước Google hoặc kết quả xác thực Google không hợp lệ.
- **When**: Luồng đăng nhập kết thúc.
- **Then**: Không tạo tài khoản, liên kết Google hoặc phiên BMT mới.

#### AC-013

- **Given**: Khách đăng nhập Google thành công trên web hoặc app.
- **When**: Hệ thống trả phiên BMT.
- **Then**: Web nhận phiên qua cookie, không nhận token trong nội dung phản hồi; app nhận token trực tiếp và không có cookie phiên.
- **And**: Cơ chế làm mới, đăng xuất và thu hồi phiên giữ các quy tắc đang áp dụng theo nền tảng; giới hạn phiên mật khẩu tạm theo BR-AUTH-005 không bị mất khi làm mới.

#### AC-014

- **Given**: Liên kết Google hiện có mâu thuẫn với tài khoản mà yêu cầu đang nhắm tới.
- **When**: Hệ thống xử lý yêu cầu.
- **Then**: Không tự gộp tài khoản hoặc ghi đè liên kết.
- **And**: Không cấp phiên cho tài khoản khác.

#### AC-015

- **Given**: Khách có phiên sử dụng thông thường và mở chức năng sửa hồ sơ trên web hoặc app.
- **When**: Khách cập nhật tên, ảnh hoặc thông tin hồ sơ khác, giữ nguyên email.
- **Then**: Hệ thống cho cập nhật theo các kiểm tra hiện có và giữ nguyên email.
- **And**: Yêu cầu cố thay email qua API bị từ chối; không đổi email hoặc ghi một phần dữ liệu của yêu cầu đó.

## References

### TDDs

- TDD-AUTH-003/Architecture: thiết kế tích hợp Google, liên kết, mật khẩu tạm và phiên; người dùng đã chốt trong hội thoại ngày 30/09/2026, chưa triển khai.
- TDD-AUTH-001/Architecture: cơ chế chống CSRF và cookie hiện có; chưa phải thiết kế tích hợp Google.
- TDD-AUTH-002/Architecture: cấp phiên web/mobile và làm mới phiên hiện có.

### Rules

- BR-AUTH-003/Then: phạm vi Google và tạo tài khoản khách hàng.
- BR-AUTH-004/Then: liên kết và tiếp nhận tài khoản chưa xác minh.
- BR-AUTH-005/Then: mật khẩu tạm và giới hạn theo cách đăng nhập.
- BR-AUTH-006/Then: thông tin hồ sơ lấy từ Google.
- BR-AUTH-007/Then: không cho đổi email khi tự sửa hồ sơ.
- BR-AUTH-002/Then: cách trả token, vòng đời phiên mobile và thu hồi phiên khi đổi mật khẩu.
- BR-RBAC-005/Then: tài khoản khách hàng và nhân viên tách biệt.

### Dependencies

- STORY-AUTH-001: luồng phiên mobile và Quên mật khẩu đã có; Google là phạm vi mới của Story này.

## Non-Functional

- Backend phải xác thực kết quả Google; không tin email hoặc mã tài khoản do client tự khai. Điều kiện chữ ký, ứng dụng nhận kết quả và hạn hiệu lực thuộc TDD, theo hướng dẫn chính thức của Google.
- Không ghi mật khẩu, nội dung thư có mật khẩu, token hoặc mã xác minh vào log; không trả mật khẩu tạm trong phản hồi đăng nhập.
- Các yêu cầu tạo tài khoản hoặc liên kết gửi lại hay đồng thời không được tạo nhiều tài khoản hoặc liên kết mâu thuẫn cho cùng danh tính. Không sinh thêm mật khẩu chỉ vì gửi lại cùng thao tác đã hoàn tất.
- Phiên web tiếp tục có bảo vệ CSRF; phiên mobile giữ cách dùng token hiện có. Thời hạn phiên không bị thay đổi chỉ vì dùng Google.
- Chưa đặt ngưỡng hiệu năng, thời gian gửi thư hoặc số lần thử mới. Các lựa chọn kỹ thuật và phần bảo vệ nội dung thư chờ gửi cần được xác định khi soạn TDD.

## Out of Scope

- Đăng nhập Google cho nhân viên; đăng nhập Apple hoặc nhà cung cấp khác.
- Tính năng quản lý nhiều tài khoản Google, gỡ hoặc chuyển liên kết; các trường hợp cần chức năng đó chưa được chốt.
- Thay đổi cách cấp mật khẩu cho nhân viên.
- Luồng đổi email và xác minh email mới; chức năng sửa hồ sơ giữ nguyên email theo BR-AUTH-007.
- Tự triển khai code, chạy migration hoặc cấu hình dịch vụ Google trong bước chuẩn bị US/BR này.
- Code triển khai và mã kiểm thử tự động; bộ System Test được soạn thành tài liệu riêng từ Story và các Business Rule đã chốt. Thiết kế kỹ thuật và đặc tả Unit Test thuộc bước tiếp theo.
