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

# STORY-PROJ-004

## Metadata

- **Story**: Là chủ sở hữu bản dự toán, tôi muốn chia sẻ hồ sơ qua link, QR hoặc email và kiểm soát thời hạn để người nhận xem, tải hồ sơ mà không cần tài khoản.
- **Context**: Chủ sở hữu bản dự toán chọn ngày hết hạn và có thể thu hồi sớm. Email gửi link thay vì tệp đính kèm. Quyền chia sẻ hồ sơ cũ vẫn được dùng khi gói thiết kế hết hạn; AI và dịch vụ email là các phụ thuộc cần tích hợp.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Chủ sở hữu bản dự toán đã đăng nhập và có hồ sơ có thể cung cấp theo STORY-PROJ-003.
- Người nhận không phải đăng nhập; quyền xem/tải căn cứ vào link còn hiệu lực theo BR-PROJ-006.

### Trigger

Chủ sở hữu bản dự toán tạo link/QR, gửi email chứa link hoặc thu hồi link; người nhận mở link hoặc quét QR.

## Flow

### Main Flow

1. Chủ sở hữu bản dự toán chọn chia sẻ hồ sơ và chọn ngày hết hạn.
2. Backend kiểm tra quyền chủ sở hữu bản dự toán và hồ sơ. Nếu đã có link đang hiệu lực thì dùng lại; nếu chưa có, đã hết hạn hoặc bị thu hồi thì tạo link với ngày hết hạn hợp lệ theo BR-PROJ-006.
3. Chủ sở hữu bản dự toán nhận link hoặc QR dẫn tới cùng quyền truy cập hồ sơ đó.
4. Người nhận mở link hoặc quét QR; backend kiểm tra hiệu lực và việc thu hồi link.
5. Khi link hợp lệ, cung cấp hồ sơ để người nhận xem và tải mà không yêu cầu đăng nhập.

### Alternative Flow

#### ALT-01

Chủ sở hữu bản dự toán chọn gửi email chứa link.

1. Chủ sở hữu bản dự toán nhập một địa chỉ email nhận hợp lệ và thực hiện thao tác gửi; có thể gửi cho người nhận khác với email tài khoản của mình.
2. Hệ thống gửi link hồ sơ còn hiệu lực; không đính kèm PDF/Excel hoặc tạo quyền truy cập riêng.
3. Người nhận mở link trong email và tiếp tục từ bước kiểm tra hiệu lực của Main Flow.

#### ALT-02

Chủ sở hữu bản dự toán thu hồi link trước ngày hết hạn.

1. Backend kiểm tra quyền chủ sở hữu bản dự toán và ghi nhận thu hồi.
2. Từ chối yêu cầu xem/tải mới qua link đó, kể cả từ QR hoặc email đã gửi.
3. Giữ hồ sơ nguồn và quyền truy cập riêng của chủ sở hữu bản dự toán.

#### ALT-03

Gói thiết kế của chủ sở hữu bản dự toán đã hết hạn.

1. Vẫn cho phép tạo link/QR, gửi email và thu hồi link đối với hồ sơ đã có theo BR-SUB-007.
2. Không tự hết hạn link chỉ vì gói hết hạn; người nhận vẫn được xem/tải khi link còn hiệu lực.
3. Không khởi chạy AI tạo thiết kế mới hoặc tính lượt cho các thao tác này.

### Exception Flow

#### EXC-01

Link không hợp lệ, đã hết hạn hoặc bị thu hồi.

1. Từ chối xem và tải hồ sơ qua link đó, không trả nội dung hay tệp hồ sơ.
2. Áp dụng cùng kết quả khi truy cập từ QR hoặc email.

#### EXC-02

Người yêu cầu không phải chủ sở hữu bản dự toán nhưng muốn quản lý chia sẻ.

1. Từ chối tạo link/QR, gửi email hoặc thu hồi link của bản dự toán.
2. Có link công khai chỉ cấp quyền xem/tải, không đủ quyền quản lý.

#### EXC-03

Dịch vụ email không tiếp nhận được yêu cầu gửi.

1. Báo gửi email chưa thành công, không xác nhận người nhận đã nhận thư.
2. Giữ hồ sơ và trạng thái chia sẻ đã lưu; lỗi gửi thư không làm mất hồ sơ hoặc tạo lại thiết kế.
3. Cơ chế gửi lại, chống gửi trùng và phân biệt đã tiếp nhận/đã phát thư sẽ xác định trong thiết kế tích hợp email.

#### EXC-04

Chủ sở hữu bản dự toán nhập địa chỉ email không hợp lệ hoặc gửi nhiều địa chỉ trong một lần.

1. Từ chối yêu cầu gửi và báo cần một địa chỉ email hợp lệ.
2. Không gửi thư hoặc thay đổi hồ sơ nguồn.

#### EXC-05

Chủ sở hữu chọn ngày hết hạn đã qua theo giờ Việt Nam.

1. Từ chối tạo link và yêu cầu chọn ngày hiện tại hoặc ngày sau đó.
2. Không tạo link hoặc thay đổi quyền chia sẻ đã lưu.

## Acceptance Criteria

#### AC-001

- **Given**: Chủ sở hữu bản dự toán có hồ sơ và chọn ngày hết hạn.
- **When**: Tạo link hoặc QR.
- **Then**: Cung cấp quyền xem/tải đúng hồ sơ, có thời hạn đã chọn.
- **And**: QR dẫn tới cùng quyền truy cập với link.

#### AC-002

- **Given**: Người nhận có link còn hiệu lực, chưa bị thu hồi.
- **When**: Mở link trực tiếp, từ email hoặc quét QR.
- **Then**: Được xem và tải hồ sơ không cần đăng nhập.
- **And**: Không được sửa đầu vào, gửi AI, quản lý chia sẻ hay xem bản dự toán khác.

#### AC-003

- **Given**: Chủ sở hữu bản dự toán đã thu hồi link hoặc link đến hạn.
- **When**: Có yêu cầu xem/tải mới qua link, QR hoặc email đã gửi.
- **Then**: Từ chối truy cập hồ sơ qua link đó.
- **And**: Hồ sơ nguồn vẫn còn; bản tệp đã tải trước đó không thuộc phạm vi thu hồi.

#### AC-004

- **Given**: Chủ sở hữu bản dự toán gửi email và dịch vụ tiếp nhận thành công.
- **When**: Email được tạo để gửi.
- **Then**: Nội dung chứa link hồ sơ, không có tệp hồ sơ đính kèm.
- **And**: Link dùng cùng ngày hết hạn và quyền thu hồi; tiếp nhận gửi không chứng minh người nhận đã nhận hoặc mở thư.

#### AC-005

- **Given**: Gói thiết kế đã hết hạn, chủ sở hữu bản dự toán có hồ sơ cũ.
- **When**: Chủ sở hữu bản dự toán tạo link/QR, gửi email hoặc thu hồi link.
- **Then**: Cho phép theo BR-SUB-007, không yêu cầu gia hạn hoặc còn lượt.
- **And**: Gói hết hạn không tự thay đổi thời hạn của link đang có.

#### AC-006

- **Given**: Người yêu cầu chỉ có link công khai hoặc không sở hữu bản dự toán.
- **When**: Yêu cầu quản lý chia sẻ của bản dự toán.
- **Then**: Từ chối thao tác quản lý.
- **And**: Không thay đổi ngày hết hạn hoặc việc thu hồi của link hiện có.

#### AC-007

- **Given**: Dịch vụ email từ chối hoặc chưa xác nhận tiếp nhận yêu cầu.
- **When**: Backend trả trạng thái gửi thư.
- **Then**: Không báo đã gửi thành công hoặc đã phát đến người nhận khi chưa có căn cứ.
- **And**: Giữ hồ sơ và trạng thái chia sẻ đã lưu.

#### AC-008

- **Given**: Chủ sở hữu bản dự toán chọn Gửi email.
- **When**: Nhập người nhận và yêu cầu gửi.
- **Then**: Mỗi lần chỉ nhận một địa chỉ email hợp lệ; không giới hạn ở email tài khoản chủ sở hữu bản dự toán.
- **And**: Từ chối địa chỉ không hợp lệ hoặc nhiều địa chỉ trong cùng yêu cầu, không gửi thư từ yêu cầu bị từ chối.

#### AC-009

- **Given**: Chủ sở hữu chọn ngày hết hạn để tạo link.
- **When**: Backend kiểm tra ngày và hiệu lực truy cập.
- **Then**: Chỉ nhận ngày hiện tại hoặc ngày tương lai theo giờ Việt Nam; link dùng được hết ngày đã chọn, từ 00:00 ngày kế tiếp thì hết hiệu lực.
- **And**: Cùng mốc này áp dụng cho xem và tải qua link, QR hoặc email; không chờ tác vụ định kỳ mới chặn truy cập.

#### AC-010

- **Given**: Một bản dự toán có hồ sơ có thể chia sẻ.
- **When**: Chủ sở hữu yêu cầu link, QR hoặc gửi email, kể cả nhiều yêu cầu đồng thời.
- **Then**: Chỉ có một link đang hiệu lực dùng chung; khi còn hiệu lực thì dùng lại, sau khi hết hạn hoặc thu hồi mới tạo link khác.
- **And**: Tạo link mới không khôi phục hiệu lực link cũ hoặc đường truy cập qua QR/email cũ.

## References

### TDDs

- TDD-PROJ-003

### Rules

- BR-PROJ-006
- BR-PROJ-007
- BR-SUB-007

### Dependencies

- STORY-PROJ-003

## Non-Functional

- Kiểm tra ngày hết hạn và việc thu hồi cho cả yêu cầu xem lẫn tải; không dùng địa chỉ tải tệp độc lập để vô hiệu hóa quyền thu hồi.
- Mốc hết ngày Việt Nam và một link đang hiệu lực đã chốt tại BR-PROJ-006. Cơ chế token, lưu tệp, chống yêu cầu đồng thời và email được đề xuất trong TDD-PROJ-003; giới hạn vận hành còn chờ cấu hình, gia hạn link chưa thuộc phạm vi.

## Out of Scope

- Gửi tệp hồ sơ đính kèm email; thu hồi tệp đã được người nhận tải về.
- Cấp quyền chỉnh sửa bản dự toán qua link hoặc giới hạn người nhận phải có tài khoản.
- Tự gửi email trong tác vụ khảo sát/lập kế hoạch này.
