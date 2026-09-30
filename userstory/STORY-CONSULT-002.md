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

# STORY-CONSULT-002

## Metadata

- **Story**: Là khách đã đăng nhập, tôi muốn gửi yêu cầu tư vấn với KTS và giờ mong muốn để admin gọi lại sắp xếp lịch.
- **Context**: Tư vấn miễn phí, độc lập với gói dịch vụ. Khách không cần trang theo dõi yêu cầu. Nghiệp vụ trước lần bổ sung này đã được người dùng chốt trong hội thoại. Ngày 29/09/2026, người dùng xác nhận hiển thị công ty, số sao và số đánh giá ở cả danh sách và chi tiết KTS; các giá trị do người có quyền quản lý tư vấn KTS nhập thủ công theo BR-CONSULT-001. Người dùng đã chốt bản cập nhật US/BR ngày 29/09/2026. Hồ sơ cũ chưa có Công ty vẫn hiển thị theo trạng thái hiện có, theo ngoại lệ ở BR-CONSULT-001. Metadata chưa đầy đủ. Còn cần làm rõ: danh sách giờ cụ thể, định dạng số liên lạc và giới hạn độ dài nội dung.
- **Sprint**:
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

- Khách đã đăng nhập.
- KTS được chọn đang hiển thị.

### Trigger

Khách chọn KTS và bắt đầu gửi yêu cầu tư vấn.

## Flow

### Main Flow

1. Khách xem các hồ sơ đang hiển thị; danh sách và trang chi tiết KTS đều có công ty, số sao và số đánh giá hiện tại. Khách chọn KTS, ngày và khung giờ mong muốn từ danh sách dùng chung trên frontend, theo giờ Việt Nam (UTC+7).
2. Hệ thống điền số liên lạc từ tài khoản nếu có; khách kiểm tra hoặc đổi số riêng cho đơn.
3. Khách có thể nhập nội dung cần tư vấn hoặc để trống rồi bấm gửi.
4. Backend kiểm tra đăng nhập, KTS còn hiển thị, có số liên lạc và thời gian mong muốn còn ở tương lai.
5. Hệ thống lưu yêu cầu ở trạng thái Chưa xử lý và thông báo đã tiếp nhận; admin sẽ gọi lại xác nhận lịch.
6. Hệ thống gửi email đến tài khoản với KTS, ngày giờ mong muốn, số liên lạc, nội dung nếu có và lời nhắc admin gọi lại.

### Alternative Flow

#### ALT-01

Tài khoản chưa có số điện thoại hoặc khách muốn dùng số khác.

1. Khách nhập số liên lạc cho yêu cầu.
2. Hệ thống lưu số trên yêu cầu, không cập nhật tài khoản.

#### ALT-02

Đã có khách khác chọn cùng KTS và khung giờ.

1. Hệ thống vẫn tiếp nhận yêu cầu nếu các điều kiện khác hợp lệ.
2. Admin liên hệ để sắp xếp; hệ thống không giữ chỗ.

### Exception Flow

#### EXC-01

Chưa đăng nhập hoặc phiên không còn hợp lệ.

1. Hệ thống yêu cầu đăng nhập hợp lệ trước khi gửi; không tiếp nhận yêu cầu chưa xác thực.

#### EXC-02

Thiếu số liên lạc, thời gian không còn ở tương lai hoặc KTS đã bị ẩn.

1. Hệ thống từ chối tiếp nhận và thông báo điều kiện cần sửa.

#### EXC-03

Gửi email thất bại sau khi đã tiếp nhận.

1. Giữ nguyên yêu cầu thành công để admin xử lý.
2. Hệ thống tự thử gửi lại email; không yêu cầu khách gửi lại đơn.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đã đăng nhập, KTS đang hiển thị và dữ liệu hợp lệ.
- **When**: Khách gửi yêu cầu.
- **Then**: Lưu đúng KTS, ngày giờ mong muốn, số liên lạc và nội dung nếu có; trạng thái Chưa xử lý.
- **And**: Thông báo tiếp nhận không khẳng định lịch đã được xác nhận.

#### AC-002

- **Given**: Khách chưa mua gói nhưng đã đăng nhập.
- **When**: Gửi yêu cầu hợp lệ.
- **Then**: Yêu cầu vẫn được tiếp nhận miễn phí.
- **And**: Không kiểm tra hoặc trừ lượt gói dịch vụ.

#### AC-003

- **Given**: Tài khoản có số điện thoại.
- **When**: Khách mở đơn và thay số liên lạc rồi gửi.
- **Then**: Ban đầu điền số tài khoản; đơn lưu số khách thay.
- **And**: Số trong tài khoản giữ nguyên.

#### AC-004

- **Given**: Tài khoản không có số điện thoại.
- **When**: Khách gửi đơn.
- **Then**: Phải nhập số liên lạc mới được tiếp nhận.
- **And**: Không tự ghi số vừa nhập về tài khoản.

#### AC-005

- **Given**: Các trường bắt buộc hợp lệ.
- **When**: Khách để trống nội dung tư vấn.
- **Then**: Vẫn tiếp nhận yêu cầu.
- **And**: Email không cần có phần nội dung tư vấn trống.

#### AC-006

- **Given**: Khách khác đã gửi cùng KTS và khung giờ.
- **When**: Khách gửi yêu cầu hợp lệ tại khung giờ đó.
- **Then**: Yêu cầu vẫn được tiếp nhận.
- **And**: Không khóa hoặc giữ chỗ theo khung giờ.

#### AC-007

- **Given**: Ngày giờ bằng hoặc trước thời điểm backend kiểm tra.
- **When**: Khách gửi yêu cầu, kể cả gửi trực tiếp tới backend.
- **Then**: Backend từ chối thời điểm không còn ở tương lai.
- **And**: Backend không cần danh mục khung giờ lưu trữ.

#### AC-008

- **Given**: KTS bị ẩn sau khi khách mở trang.
- **When**: Khách gửi yêu cầu.
- **Then**: Hệ thống từ chối yêu cầu mới.
- **And**: Yêu cầu cũ của KTS vẫn được giữ.

#### AC-009

- **Given**: Yêu cầu đã được tiếp nhận.
- **When**: Hệ thống gửi email.
- **Then**: Email đến địa chỉ trong tài khoản, gồm thông tin đã chốt và lời nhắc admin gọi lại xác nhận lịch.
- **And**: Không chứa ghi chú nội bộ hoặc khẳng định đã giữ chỗ.

#### AC-010

- **Given**: Yêu cầu đã lưu và lần gửi email thất bại.
- **When**: Hệ thống xử lý lỗi email.
- **Then**: Yêu cầu vẫn thành công và có trong danh sách admin; tự thử gửi lại email.
- **And**: Khách không phải tạo yêu cầu khác.

#### AC-011

- **Given**: Khách chưa đăng nhập.
- **When**: Gửi yêu cầu.
- **Then**: Hệ thống từ chối tiếp nhận.
- **And**: Phải đăng nhập hợp lệ trước khi gửi.

#### AC-012

- **Given**: Một yêu cầu có ngày giờ mong muốn đã được tiếp nhận.
- **When**: Hiển thị thời gian trên form, email và trang quản trị.
- **Then**: Các nơi này dùng thống nhất giờ Việt Nam (UTC+7).
- **And**: Không làm thay đổi thời điểm khách đã chọn do múi giờ máy đang sử dụng.

#### AC-013

- **Given**: KTS đang hiển thị và đã lưu công ty, số sao, số đánh giá hợp lệ theo BR-CONSULT-001.
- **When**: Khách xem danh sách hoặc mở chi tiết KTS để lựa chọn người tư vấn.
- **Then**: Cả hai nơi hiển thị đúng công ty, số sao và số đánh giá hiện tại do người có quyền quản lý nhập; hai số bằng 0 vẫn được hiển thị.
- **And**: Việc xem các số liệu này không yêu cầu khách gửi đánh giá và không thay đổi điều kiện gửi yêu cầu tư vấn.

## References

### TDDs

- TDD-CONSULT-001

### Rules

- BR-CONSULT-001
- BR-CONSULT-002
- BR-CONSULT-003
- BR-CONSULT-004
- BR-CONSULT-005

### Dependencies

- STORY-CONSULT-001

## Non-Functional

- Chức năng quản trị chỉ dành cho người có quyền quản lý tư vấn KTS theo STORY-RBAC-001; ghi chú nội bộ không được công khai hoặc gửi cho khách. Chưa xác định chỉ tiêu hiệu năng định lượng.

## Out of Scope

- Trang khách theo dõi, sửa hoặc hủy yêu cầu.
- Thanh toán, điều kiện mua gói, giữ chỗ, lịch rảnh riêng của KTS và tự xác nhận lịch.
- Danh mục khung giờ hoặc màn hình quản trị khung giờ ở backend.
- Chức năng khách gửi đánh giá KTS; tự tính số sao và số đánh giá.
