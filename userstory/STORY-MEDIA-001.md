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

# STORY-MEDIA-001

## Metadata

- **Story**: Là người dùng đã đăng nhập, tôi muốn upload ảnh trực tiếp lên BizFly bằng URL do BMT cấp để dùng ảnh trong các chức năng của hệ thống.
- **Context**: Người dùng đã chốt bộ US/BR trong hội thoại ngày 30/09/2026. Backend BMT bổ sung chức năng cấp presigned URL dùng chung cho quản trị viên và khách hàng. Người tạo và người phụ trách Backend/QA là Tân Trần; Sprint chưa được phân công. Việc dọn ảnh mới và ảnh cũ thuộc STORY-MEDIA-002.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: Tân Trần
  - QA: Tân Trần

## Conditions

### Preconditions

- Người dùng đã đăng nhập BMT.
- BMT đã được cấu hình để cấp quyền upload vào kho BizFly của BMT.

### Trigger

Người dùng chọn ảnh để sử dụng trong một chức năng của BMT.

## Flow

### Main Flow

1. Người dùng chọn ảnh theo purpose Image mặc định: JPG/PNG/WebP ≤5 MiB. Admin quản lý nhà thầu dùng ContractorImage ≤10 MiB hoặc ContractorScan PDF/JPG/PNG ≤20 MiB.
2. Frontend gửi yêu cầu xin URL upload đến backend BMT.
3. Backend kiểm tra phiên đăng nhập và điều kiện cấp URL theo BR-MEDIA-001.
4. Backend trả thông tin để frontend upload ảnh trực tiếp lên BizFly bằng presigned URL có thời hạn.
5. Frontend PUT vào URL đã cấp rồi gọi complete. Chỉ khi state=Completed, frontend lấy fileUrl HTTPS cố định để dùng trong hồ sơ.
6. Frontend gửi URL ảnh vào API nghiệp vụ tương ứng.
7. API nghiệp vụ kiểm tra quyền và điều kiện lưu của chức năng đó trước khi lưu URL. Có quyền upload không đồng nghĩa có quyền sửa nội dung.
8. Khi CDN đã được cấu hình và bật, hệ thống lưu URL CDN vào cột URL hiện có; API đọc nguyên giá trị đã lưu để frontend hiển thị. URL ảnh cũ được chuyển một lần bằng migration khi triển khai; src của ảnh trong bài cũng được lưu lại. URL gốc và CDN vẫn chỉ cùng một ảnh.

### Alternative Flow

#### ALT-01

Người dùng upload ảnh rồi bỏ form, hoặc upload thành công nhưng chưa lưu được nội dung.

1. Ảnh chưa được coi là đang dùng chỉ vì upload đã thành công.
2. Việc xác định ảnh không còn được dùng và thời điểm dọn tuân theo STORY-MEDIA-002 và BR-MEDIA-002.

#### ALT-02

Người dùng thay một ảnh đang được sử dụng bằng ảnh mới.

1. Người dùng upload ảnh mới và lưu thay đổi qua API nghiệp vụ.
2. Chỉ sau khi lưu thành công, liên kết tới ảnh cũ ở nội dung này mới được thay bằng ảnh mới.
3. Ảnh cũ được xét dọn theo BR-MEDIA-002; phải giữ nếu còn nơi khác sử dụng.

### Exception Flow

#### EXC-01

Người dùng chưa đăng nhập hoặc phiên đăng nhập không còn hợp lệ lúc xin URL.

1. Backend từ chối cấp URL upload và yêu cầu đăng nhập lại.

#### EXC-02

Ảnh không thuộc định dạng cho phép hoặc vượt quá 5 MiB.

1. Hệ thống báo điều kiện ảnh không đạt và không chấp nhận ảnh đó qua luồng upload mới.
2. Người dùng chọn ảnh đáp ứng điều kiện rồi thử lại.

#### EXC-03

URL upload đã hết hạn hoặc upload gặp lỗi.

1. Frontend báo upload chưa thành công; không lưu URL đó như một ảnh đã upload thành công.
2. Người dùng có thể thử lại, xin URL mới nếu URL cũ đã hết hạn.

#### EXC-04

Upload thành công nhưng API nghiệp vụ từ chối lưu nội dung hoặc gặp lỗi.

1. Hệ thống giữ nguyên nội dung đã lưu trước đó và báo lỗi của thao tác lưu.
2. Ảnh vừa upload được xét dọn theo BR-MEDIA-002 nếu không được sử dụng ở đâu khác.

## Acceptance Criteria

#### AC-001

- **Given**: Quản trị viên hoặc khách hàng có phiên đăng nhập hợp lệ.
- **When**: Người dùng xin URL upload với ảnh đáp ứng BR-MEDIA-001.
- **Then**: Backend BMT cấp thông tin upload trực tiếp lên BizFly.
- **And**: Purpose Image không bắt buộc vai trò quản trị; ContractorImage và ContractorScan bắt buộc admin.

#### AC-002

- **Given**: Người dùng không có phiên đăng nhập hợp lệ.
- **When**: Người dùng xin URL upload.
- **Then**: Backend từ chối cấp URL.
- **And**: Không có quyền upload mới được cấp từ yêu cầu đó.

#### AC-003

- **Given**: Ảnh được gửi qua luồng upload mới.
- **When**: Hệ thống kiểm tra định dạng và dung lượng.
- **Then**: Áp giới hạn tương ứng purpose theo BR-MEDIA-001; luồng mặc định vẫn chỉ JPG/PNG/WebP ≤5.242.880 byte.
- **And**: Ảnh vượt giới hạn hoặc không thuộc định dạng cho phép bị từ chối.

#### AC-004

- **Given**: Ảnh thuộc luồng công khai BR-MEDIA-001 đã upload thành công và chưa bị dọn khỏi kho; không phải tệp công trình BR-SITE-007.
- **When**: Một người có URL xem ảnh mở URL đó.
- **Then**: Người đó xem được ảnh mà không cần đăng nhập BMT.
- **And**: URL xem ảnh cố định, không hết hạn theo thời hạn của URL upload. Khi có cấu hình và bật CDN, ảnh công khai được trả qua CDN với đường dẫn giữ nguyên, ảnh mới lưu trực tiếp và ảnh cũ chuyển bằng migration khi triển khai, không đổi URL lúc xuất JSON; không chuyển URL upload có chữ ký hoặc tệp riêng tư.

#### AC-005

- **Given**: Người dùng đã upload ảnh nhưng không có quyền sửa nội dung đích.
- **When**: Người dùng gửi URL ảnh vào API lưu nội dung đó.
- **Then**: API nghiệp vụ từ chối theo quy tắc phân quyền hiện có.
- **And**: Quyền xin URL upload không bỏ qua quyền của chức năng nghiệp vụ.

#### AC-006

- **Given**: Nội dung đang dùng ảnh cũ và người dùng đã upload ảnh mới.
- **When**: Thao tác lưu thay ảnh thất bại.
- **Then**: Nội dung đã lưu vẫn dùng ảnh cũ.
- **And**: Ảnh cũ không bị dọn vì thao tác thay ảnh thất bại này.

## References

### TDDs

- TDD-SITE-005

- TDD-MEDIA-001

### Rules

- BR-MEDIA-001
- BR-MEDIA-002

- BR-SITE-007/Then: Ngoại lệ cho tệp công trình, không dùng URL xem công khai.

### Dependencies

- STORY-MEDIA-002

- STORY-SITE-004: Luồng tệp công trình cần quyền truy cập riêng và giới hạn riêng.

## Non-Functional

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- URL upload phải có thời hạn; thời lượng cụ thể sẽ được xác định khi thiết kế kỹ thuật, chưa lấy mốc 2 phút của Taskcoper làm yêu cầu đã chốt.
- Thông tin cấp cho frontend để upload không được chứa khóa bí mật của tài khoản BizFly.

- TDD-MEDIA-001 cần bổ sung luồng SITE riêng tư theo US/BR mở rộng đã được chốt; luồng công khai hiện tại không phải bằng chứng đã bảo vệ bản vẽ/ảnh hiện trạng.

## Out of Scope

- Upload các định dạng ngoài danh sách của từng purpose tại BR-MEDIA-001.
- Chuyển các luồng ảnh công khai hiện có sang riêng tư. Tệp công trình mới là phạm vi riêng tư riêng theo STORY-SITE-004 và BR-SITE-007; không áp dụng AC-004 của Story này cho tệp công trình.
- Thay đổi quyền tạo hoặc sửa nội dung của các chức năng sử dụng ảnh.
