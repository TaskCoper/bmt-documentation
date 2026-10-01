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

# STORY-SITE-004

## Metadata

- **Story**: Là khách hàng, tôi muốn tải bản vẽ và ảnh hiện trạng vào công trình để lưu tài liệu cho người có quyền xem hồ sơ.
- **Context**: Tệp tùy chọn, tổng tối đa 9 tệp/công trình và 10 MB/tệp. Tệp riêng tư theo quyền công trình, khóa quản lý khi có gói giữ chỗ. Bản vẽ kết quả dự toán chỉ mở qua nguồn, không tự sao vào nhóm tệp này.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Khách đăng nhập và sở hữu công trình, hoặc đang tạo công trình thuộc mình.
- Thêm/thay/xóa chỉ khi công trình không có gói giữ chỗ; quyền xem/tải theo BR-SITE-003.

### Trigger

Khách chọn tải tệp trong form tạo hoặc phần tài liệu của công trình.

## Flow

### Main Flow

1. Khách chọn nhóm Bản vẽ hoặc Ảnh hiện trạng và chọn tệp.
2. Hệ thống kiểm định dạng, tối đa 10 MB/tệp và tổng tối đa 9 tệp theo BR-SITE-007.
3. Upload và kiểm tra tệp; khi lưu, backend kiểm lại chủ sở hữu, khóa do gói và hạn mức.
4. Chỉ gắn tệp hợp lệ khi lưu thành công; nếu đang tạo thì hồ sơ và liên kết phải cùng thành công.
5. Người có quyền xem công trình mở/tải tệp qua đường có kiểm quyền; không công khai URL gốc.

### Alternative Flow

#### ALT-01

Khách tạo công trình không có tệp.

1. Không bắt buộc upload.
2. Tạo được nếu hồ sơ đáp ứng STORY-SITE-001.

#### ALT-02

Khách thay hoặc xóa tệp khi công trình không có gói giữ chỗ.

1. Kiểm quyền và điều kiện tại lúc lưu.
2. Khi thay, giữ tệp cũ đến khi tệp mới hợp lệ và lưu thành công; khi xóa thành công, không cấp truy cập mới qua liên kết đã gỡ.

#### ALT-03

Công trình được tạo từ dự toán.

1. Giữ liên kết mở kết quả nguồn theo BR-SITE-004.
2. Khách vẫn được tải tài liệu riêng nếu công trình không có gói giữ chỗ; tài liệu nguồn không chiếm hạn mức 9 tệp.

### Exception Flow

#### EXC-01

Tệp sai định dạng, quá 10 MB hoặc thao tác làm tổng vượt 9.

1. Từ chối tệp/thao tác không hợp lệ và chỉ rõ lý do; không làm mất tệp đã lưu.

#### EXC-02

Gói giám sát đã gán/hoàn thành, kể cả được gắn sau lúc bắt đầu upload.

1. Từ chối thêm, thay, xóa tệp khi lưu; upload tạm không vượt được khóa hồ sơ.

#### EXC-03

Người gọi ngoài phạm vi công trình, đã mất phân công hoặc chỉ có đường dẫn tệp.

1. Từ chối xem/tải; không lộ URL công khai, bản xem trước hoặc nội dung tệp.

#### EXC-04

Upload hoặc lưu liên kết gặp lỗi.

1. Không báo đã đính kèm thành công khi chưa lưu được; giữ tệp cũ nếu đang thay.
2. Cho thử lại; tệp tạm chưa được coi là tài liệu của công trình.

## Acceptance Criteria

#### AC-001

- **Given**: Hồ sơ hợp lệ chưa có tệp.
- **When**: Khách tạo công trình không upload.
- **Then**: Tạo thành công, danh sách tệp rỗng.

#### AC-002

- **Given**: Khách có công trình được quản lý tệp.
- **When**: Upload ảnh hiện trạng JPG/PNG/WebP; bản vẽ PDF/DWG/DXF/JPG/PNG/WebP, đáp ứng dung lượng và số lượng.
- **Then**: Chấp nhận đúng các định dạng của từng nhóm.

#### AC-003

- **Given**: Hồ sơ có 8 tệp, ví dụ 5 bản vẽ và 3 ảnh.
- **When**: Thêm một tệp hợp lệ rồi thêm tiếp tệp thứ mười.
- **Then**: Tệp thứ chín được nhận, tệp thứ mười bị từ chối; không tính hạn mức riêng từng nhóm.

#### AC-004

- **Given**: Tệp hợp lệ về định dạng, hồ sơ còn chỗ.
- **When**: Lần lượt gửi tệp đúng 10.000.000 byte và 10.000.001 byte.
- **Then**: Nhận tệp đầu, từ chối tệp sau; kiểm trên dung lượng thực.

#### AC-005

- **Given**: Công trình có gói đã gán hoặc đã hoàn thành.
- **When**: Khách thêm, thay hoặc xóa tệp qua giao diện/API.
- **Then**: Tất cả bị từ chối; tệp đã lưu giữ nguyên.

#### AC-006

- **Given**: Gói giữ chỗ đã gỡ/hủy và không còn gói giữ chỗ khác.
- **When**: Khách quản lý tệp hợp lệ.
- **Then**: Được thao tác; các trường hồ sơ lấy từ dự toán vẫn bị khóa.

#### AC-007

- **Given**: Chủ công trình hoặc nhân viên có quyền xem đang hợp lệ.
- **When**: Xem/tải tệp công trình.
- **Then**: Được truy cập; quyền xem không cấp quyền quản lý tệp cho nhân viên.

#### AC-008

- **Given**: Người ngoài chỉ có đường dẫn hoặc mã tệp.
- **When**: Mở tệp hoặc bản xem trước, kể cả không qua giao diện.
- **Then**: Bị từ chối; không có URL công khai bỏ qua quyền.

#### AC-009

- **Given**: Nhân viên từng có quyền theo phân công, nay đã chuyển giao/kết thúc và không có quyền xem rộng hơn.
- **When**: Gửi yêu cầu xem/tải mới bằng đường dẫn đã biết.
- **Then**: Bị từ chối theo phạm vi hiện tại.

#### AC-010

- **Given**: Khách thay một tệp đang lưu.
- **When**: Upload tệp mới hoặc lưu thay thế thất bại.
- **Then**: Tệp cũ vẫn gắn với hồ sơ; không báo thay thành công.

#### AC-011

- **Given**: Công trình được tạo từ dự toán có các tệp kết quả.
- **When**: Mở phần tài liệu công trình.
- **Then**: Chỉ tệp tải riêng hiện trong nhóm đính kèm; mở kết quả qua nguồn theo quyền dự toán.

#### AC-012

- **Given**: Công trình có 8 tệp và hai yêu cầu thêm tệp đồng thời.
- **When**: Lưu cả hai yêu cầu.
- **Then**: Tổng tệp cuối không vượt 9; không ghi nhận cả hai thành công.

#### AC-013

- **Given**: Khách bắt đầu upload khi chưa có gói; gói đã giữ chỗ trước lúc lưu liên kết.
- **When**: Hoàn tất thao tác gắn tệp.
- **Then**: Từ chối gắn theo trạng thái hiện tại, không dùng quyền tại lúc bắt đầu để vượt khóa.

#### AC-014

- **Given**: Tệp của công trình khác hoặc khách khác tồn tại.
- **When**: Khách sửa mã tệp/đường dẫn để gắn vào hồ sơ của mình.
- **Then**: Từ chối, không tạo liên kết trái quyền.

#### AC-015

- **Given**: Công trình hoặc liên kết tệp vừa được xóa hợp lệ.
- **When**: Gửi yêu cầu đọc mới qua công trình đó.
- **Then**: Không cấp quyền đọc; không cam kết thu hồi bản tải về trước đó.

## References

### TDDs

- TDD-SITE-005

### Rules

- BR-SITE-007/Then
- BR-SITE-002/Then
- BR-SITE-003/Then
- BR-SITE-004/Then

### Dependencies

- STORY-SITE-001: Form tạo và quản lý công trình.
- STORY-SITE-002: Phạm vi nhân viên xem công trình.
- STORY-MEDIA-001: Cơ chế upload hiện có cần mở rộng cho tệp riêng tư.

## Non-Functional

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Giới hạn dung lượng, số lượng, quyền và khóa do gói phải được kiểm ở backend, kể cả yêu cầu đồng thời.
- Đường mở/tải và bản xem trước đều phải tôn trọng quyền hiện hành; không công khai tệp gốc.
- Nghiệp vụ được người dùng xác nhận ngày 01/10/2026; bản US/BR cụ thể đã được chốt, System Test đã được soạn. Chưa có TDD cho phần mở rộng này và chưa có kết quả kiểm thử thực thi.

## Out of Scope

- Công khai/chia sẻ tệp bằng đường dẫn không kiểm quyền; nhân viên quản lý tệp thay khách.
- Tự sao tệp từ dự toán, chỉnh sửa nội dung bản vẽ, chuyển đổi định dạng hoặc xem CAD trực tiếp trong trình duyệt.
- Đặt thời hạn lưu/xóa vật lý mới cho mọi bản vẽ; vòng đời tệp tạm được làm rõ ở TDD.
