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

# STORY-MEDIA-002

## Metadata

- **Story**: Là người vận hành BMT, tôi muốn hệ thống tự dọn ảnh không còn được sử dụng, gồm cả ảnh cũ, để kho BizFly không giữ mãi những ảnh đã bị bỏ.
- **Context**: Người dùng đã chốt bộ US/BR trong hội thoại ngày 30/09/2026. Người dùng xác nhận ảnh cũ nằm trong bucket riêng của BMT và yêu cầu xử lý trong cùng đợt với API presign mới. Với ảnh cũ thiếu lịch sử, chờ 24 giờ từ lần đầu xác định ảnh không còn được dùng. Ảnh đã đăng ký cho mẫu thư viện nhưng không còn phiên bản hay nội dung nào sử dụng cũng được dọn sau 24 giờ. Người tạo và người phụ trách Backend/QA là Tân Trần theo xác nhận bổ sung của người dùng.
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

- Đã xác định bucket riêng của BMT và cấu hình để hệ thống đối soát ảnh trong kho.
- Hệ thống xác định được các nơi trong BMT còn sử dụng một ảnh; nếu đối soát chưa đầy đủ thì chưa đủ điều kiện xóa.
- Với ảnh cũ thiếu lịch sử, hệ thống ghi nhận được thời điểm đối soát đầu tiên xác định ảnh không còn được dùng để tính thời gian chờ.

### Trigger

Hệ thống chạy lượt đối soát và dọn ảnh theo lịch được cấu hình.

## Flow

### Main Flow

1. Hệ thống đối soát các ảnh trong bucket riêng của BMT, gồm ảnh được upload qua API mới và ảnh cũ.
2. Hệ thống kiểm tra nơi sử dụng của từng ảnh, gồm nội dung hiện tại, bản nháp, nội dung đang ẩn và phiên bản lịch sử còn được lưu.
3. Với ảnh upload nhưng không được dùng, hệ thống áp dụng thời gian chờ 24 giờ theo BR-MEDIA-002.
4. Với ảnh từng được dùng nhưng đã bị thay hoặc gỡ khỏi mọi nơi, hệ thống áp dụng thời gian chờ 24 giờ kể từ khi không còn nơi nào sử dụng.
5. Khi ảnh đủ thời gian chờ, hệ thống kiểm tra lại điều kiện sử dụng trước khi xóa file trên BizFly.
6. Hệ thống ghi nhận kết quả từng lần xóa để theo dõi và xử lý lại nếu kho gặp lỗi.

### Alternative Flow

#### ALT-01

Ảnh đang được nhiều nội dung hoặc phiên bản cùng sử dụng.

1. Gỡ ảnh ở một nội dung không làm ảnh đủ điều kiện xóa nếu nơi khác còn dùng.
2. Hệ thống giữ file trên BizFly cho đến khi không còn nơi nào sử dụng và đã đủ thời gian chờ.

#### ALT-02

Ảnh được dùng trở lại trong thời gian chờ dọn.

1. Hệ thống hủy trạng thái chờ dọn của ảnh khi ghi nhận nơi sử dụng mới.
2. Nếu ảnh lại không còn được dùng, thời gian chờ bắt đầu lại từ lần không còn nơi sử dụng đó.

#### ALT-03

Ảnh cũ trong bucket không còn URL trong dữ liệu nghiệp vụ.

1. Hệ thống vẫn đưa ảnh vào đối soát; không giới hạn danh sách đối soát ở các URL còn lưu trong database.
2. Khi xác định ảnh thuộc BMT và không còn được dùng, hệ thống áp dụng chính sách ảnh cũ tại BR-MEDIA-002.
3. Nếu không biết thời điểm ảnh ngừng được dùng, bắt đầu chờ 24 giờ từ lần đối soát đầu tiên xác định ảnh không còn được dùng. Không xóa ngay chỉ vì file đã được tạo từ lâu.
4. Khi đủ thời gian chờ, kiểm tra lại nơi sử dụng trước khi xóa; nếu ảnh đã được dùng trở lại thì hủy lần chờ.

#### ALT-04

Ảnh đã đăng ký cho mẫu thư viện nhưng chưa gắn hoặc đã được gỡ khỏi mọi phiên bản của mẫu đó.

1. Hệ thống kiểm tra xem ảnh có còn được nội dung nào khác sử dụng không. Bản ghi đăng ký ảnh cho mẫu không tự được tính là một nơi sử dụng.
2. Nếu không còn nơi nào sử dụng, áp dụng thời gian chờ 24 giờ theo BR-MEDIA-002/Then khoản 10.
3. Sau khi đủ thời gian chờ và kiểm tra lại vẫn không có nơi sử dụng, hệ thống dọn ảnh. Muốn dùng lại ảnh đã dọn, người dùng phải upload lại.

### Exception Flow

#### EXC-01

Không đọc được danh sách ảnh, dữ liệu nơi sử dụng hoặc thông tin cần để đối soát.

1. Hệ thống ghi nhận lỗi và giữ các ảnh chưa xác định đầy đủ điều kiện xóa.
2. Hệ thống đối soát lại khi thành phần gặp lỗi hoạt động trở lại.

#### EXC-02

BizFly từ chối xóa hoặc không xác định được kết quả xóa.

1. Hệ thống không ghi nhận thành công khi chưa xác định được kết quả.
2. Lần xử lý tiếp theo phải kiểm tra lại điều kiện sử dụng trước khi thử xóa lại.

## Acceptance Criteria

#### AC-001

- **Given**: Một ảnh upload qua luồng mới không được lưu vào nội dung nào và chưa đủ 24 giờ tính từ khi upload thành công.
- **When**: Hệ thống chạy dọn ảnh.
- **Then**: Ảnh được giữ lại.
- **And**: Upload xong chưa đồng nghĩa ảnh đã được sử dụng.

#### AC-002

- **Given**: Một ảnh upload qua luồng mới không được sử dụng ở đâu, đã qua ít nhất 24 giờ tính từ khi upload thành công và có đủ dữ liệu đối soát.
- **When**: Lượt dọn kiểm tra lại và vẫn xác định ảnh không được dùng.
- **Then**: Hệ thống xóa file khỏi BizFly.
- **And**: Kết quả xóa được ghi nhận.

#### AC-003

- **Given**: Một ảnh đã bị thay hoặc gỡ khỏi nơi sử dụng cuối cùng.
- **When**: Chưa đủ 24 giờ kể từ khi ảnh không còn được sử dụng.
- **Then**: Hệ thống chưa xóa ảnh.
- **And**: Nếu ảnh được dùng trở lại, thời gian chờ cũ không được dùng để xóa ảnh sau đó.

#### AC-004

- **Given**: Một ảnh đã được đưa vào diện chờ dọn và đủ thời gian chờ 24 giờ.
- **When**: Hệ thống đối soát đầy đủ và tiến hành dọn.
- **Then**: Hệ thống xóa file ảnh khỏi BizFly nếu ảnh vẫn không còn nơi nào sử dụng.
- **And**: Nếu ảnh vẫn được nội dung khác, bản nháp, nội dung đang ẩn hoặc phiên bản lịch sử sử dụng thì phải giữ ảnh.

#### AC-005

- **Given**: Bucket riêng của BMT chứa ảnh có từ trước khi triển khai API presign mới.
- **When**: Hệ thống đối soát kho.
- **Then**: Ảnh cũ được kiểm tra cả khi không còn URL trong database.
- **And**: Ảnh đang được sử dụng được giữ; ảnh thiếu thông tin không tự động bị coi là đủ điều kiện xóa.

#### AC-006

- **Given**: Lượt đối soát gặp lỗi hoặc chưa xác định đầy đủ nơi sử dụng của một ảnh.
- **When**: Hệ thống xét ảnh đó để dọn.
- **Then**: Hệ thống giữ ảnh và ghi nhận vấn đề cần xử lý lại.
- **And**: Không coi việc thiếu kết quả đối soát là bằng chứng ảnh không được sử dụng.

#### AC-007

- **Given**: Bucket BMT có cả ảnh và file không phải ảnh.
- **When**: Chức năng dọn ảnh chạy.
- **Then**: File không phải ảnh không bị xóa bởi chức năng này.
- **And**: Ảnh cũ không bị xóa chỉ vì khác định dạng hoặc vượt giới hạn của API upload mới.

#### AC-008

- **Given**: Một ảnh cũ không biết thời điểm ngừng sử dụng, và lần đối soát đầy đủ đầu tiên xác định ảnh không còn được dùng tại thời điểm T.
- **When**: Hệ thống xét ảnh đó để dọn.
- **Then**: Hệ thống chỉ được xóa từ T cộng 24 giờ trở đi, sau khi kiểm tra lại và vẫn không có nơi nào sử dụng ảnh.
- **And**: Ngày tạo file từ trước T không làm rút ngắn thời gian chờ; nếu ảnh được dùng trở lại thì hủy lần chờ.

#### AC-009

- **Given**: Một ảnh còn bản ghi đăng ký cho mẫu thư viện nhưng không còn phiên bản hay nội dung nào sử dụng.
- **When**: Ảnh đã đủ thời gian chờ 24 giờ theo BR-MEDIA-002/Then khoản 10 và lượt dọn kiểm tra lại vẫn không có nơi sử dụng.
- **Then**: Hệ thống xóa file ảnh khỏi BizFly.
- **And**: Bản ghi đăng ký tài nguyên không làm ảnh được giữ vô thời hạn; muốn dùng lại ảnh đã dọn phải upload lại.

## References

### TDDs

- TDD-MEDIA-001

### Rules

- BR-MEDIA-002

### Dependencies

- STORY-MEDIA-001

## Non-Functional

- Thao tác lưu nơi sử dụng của ảnh và thao tác dọn không được dẫn đến việc lưu thành công một tham chiếu tới ảnh vừa bị chức năng dọn xóa.
- Lỗi đối soát hoặc lỗi kho phải có thông tin để người vận hành phân biệt với một lượt dọn thành công.

## Out of Scope

- Dọn file không phải ảnh hoặc file thuộc kho của dự án khác.
- Xóa ảnh còn được dùng chỉ vì nội dung không còn công khai.
- Cam kết thu hồi các bản ảnh người khác đã tải hoặc sao chép trước khi file bị xóa.
