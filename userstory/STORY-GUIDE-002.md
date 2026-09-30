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

# STORY-GUIDE-002

## Metadata

- **Story**: Là người truy cập BMT, tôi muốn tìm kiếm, xem danh sách và phát video hướng dẫn ngay trên trang để biết cách sử dụng các chức năng mà không cần đăng nhập.
- **Context**: Dùng trang hướng dẫn hiện tại để hiển thị nội dung do người có quyền quản lý xuất bản. Video phát từ YouTube; nội dung hướng dẫn có một bản tiếng Việt, không chia danh mục. Bản nháp được soạn từ các quyết định ngày 30/09/2026; đã được người dùng chốt toàn văn trong hội thoại.
- **Sprint**:
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

- Người dùng truy cập được trang hướng dẫn của BMT; không yêu cầu đăng nhập.

### Trigger

Người dùng mở trang hướng dẫn, nhập từ khóa tìm kiếm hoặc chọn một hướng dẫn trong danh sách.

## Flow

### Main Flow

1. Người dùng mở trang hướng dẫn.
2. Hệ thống hiển thị các hướng dẫn đang xuất bản theo thứ tự admin đã sắp xếp, kèm tiêu đề, ảnh đại diện và thời lượng lấy từ YouTube.
3. Người dùng chọn một hướng dẫn.
4. Hệ thống hiển thị tiêu đề, mô tả ngắn và trình phát video YouTube ngay trong trang.
5. Người dùng bấm phát để xem video.

### Alternative Flow

#### ALT-01

Chưa có hướng dẫn nào đang xuất bản.

1. Hệ thống hiển thị trạng thái danh sách trống để người dùng biết chưa có hướng dẫn.
2. Không lấy bản nháp hoặc bản ẩn để lấp danh sách.

#### ALT-02

Người dùng muốn tìm hướng dẫn theo từ khóa.

1. Người dùng nhập từ khóa vào ô tìm kiếm.
2. Hệ thống tìm trong tiêu đề và mô tả ngắn của các hướng dẫn đang xuất bản, hiển thị các hướng dẫn khớp ở ít nhất một trong hai trường theo thứ tự admin đã sắp xếp.
3. Người dùng chọn kết quả để tiếp tục bước 4 của Main Flow hoặc xóa từ khóa để xem lại danh sách.

#### ALT-03

Không có hướng dẫn đang xuất bản khớp từ khóa tìm kiếm.

1. Hệ thống hiển thị thông báo không tìm thấy hướng dẫn phù hợp.
2. Người dùng đổi hoặc xóa từ khóa; hệ thống cập nhật lại danh sách theo điều kiện mới.

### Exception Flow

#### EXC-01

Hướng dẫn đã bị ẩn hoặc xóa trước yêu cầu đọc công khai mới.

1. Hệ thống thông báo hướng dẫn không còn khả dụng và không trả nội dung quản trị hoặc nội dung chưa xuất bản.
2. Người dùng có thể quay về danh sách hướng dẫn còn hiển thị.

#### EXC-02

Video của hướng dẫn đang xuất bản không phát được do YouTube hoặc cấu hình video thay đổi.

1. Hệ thống hiển thị thông báo không phát được video; không báo người dùng xem thành công.
2. Người dùng có thể đóng trình phát và chọn hướng dẫn khác.
3. Hướng dẫn không tự chuyển sang Ẩn; người quản lý chủ động sửa hoặc ẩn theo STORY-GUIDE-001.

## Acceptance Criteria

#### AC-001

- **Given**: Người dùng chưa đăng nhập.
- **When**: Người đó mở trang hướng dẫn.
- **Then**: Hệ thống cho xem danh sách hướng dẫn đang xuất bản.
- **And**: Không yêu cầu đăng nhập để chọn và xem video.

#### AC-002

- **Given**: Có các hướng dẫn ở trạng thái Nháp, Xuất bản và Ẩn.
- **When**: Người dùng tải danh sách công khai.
- **Then**: Chỉ hướng dẫn đang Xuất bản được trả về.
- **And**: Thứ tự khớp cách sắp xếp của người quản lý.

#### AC-003

- **Given**: Có hướng dẫn đang xuất bản với thông tin video đã lấy từ YouTube.
- **When**: Hệ thống hiển thị mục hướng dẫn.
- **Then**: Người dùng nhìn thấy tiêu đề, ảnh đại diện và thời lượng đúng với hướng dẫn đó.
- **And**: Các mục không chia danh mục.

#### AC-004

- **Given**: Hướng dẫn đang xuất bản và video có thể phát trong môi trường của người xem.
- **When**: Người dùng chọn hướng dẫn rồi bấm phát.
- **Then**: Video phát bằng trình phát YouTube trong trang BMT.
- **And**: Tiêu đề và mô tả ngắn của hướng dẫn được hiển thị.

#### AC-005

- **Given**: Hướng dẫn đã ẩn hoặc đã xóa.
- **When**: Người dùng gửi yêu cầu đọc công khai mới tới hướng dẫn đó.
- **Then**: Hệ thống không cung cấp nội dung hướng dẫn.
- **And**: Phần danh sách công khai mới tải không chứa hướng dẫn đó.

#### AC-006

- **Given**: Hướng dẫn đang xuất bản nhưng video không phát được.
- **When**: Trình phát báo lỗi.
- **Then**: Người dùng nhìn thấy thông báo không phát được video.
- **And**: Hướng dẫn giữ trạng thái hiện tại để admin chủ động xử lý.

#### AC-007

- **Given**: Không có hướng dẫn nào đang xuất bản.
- **When**: Người dùng mở trang hướng dẫn.
- **Then**: Hệ thống hiển thị trạng thái danh sách trống.
- **And**: Không hiển thị dữ liệu nháp, ẩn hoặc nội dung minh họa như hướng dẫn đã xuất bản.

#### AC-008

- **Given**: Có hướng dẫn đang xuất bản có tiêu đề khớp từ khóa và hướng dẫn đang nháp hoặc ẩn cũng khớp từ khóa đó.
- **When**: Người dùng tìm kiếm bằng từ khóa này.
- **Then**: Kết quả bao gồm hướng dẫn đang xuất bản khớp tiêu đề.
- **And**: Không bao gồm hướng dẫn đang nháp hoặc ẩn; kết quả giữ thứ tự quản trị.

#### AC-009

- **Given**: Có hướng dẫn đang xuất bản chỉ khớp từ khóa trong mô tả ngắn, không khớp trong tiêu đề.
- **When**: Người dùng tìm kiếm bằng từ khóa này.
- **Then**: Hướng dẫn đó xuất hiện trong kết quả.
- **And**: Hướng dẫn khớp đồng thời cả tiêu đề và mô tả chỉ xuất hiện một lần.

#### AC-010

- **Given**: Không có hướng dẫn đang xuất bản khớp từ khóa trong tiêu đề hoặc mô tả ngắn.
- **When**: Người dùng tìm kiếm.
- **Then**: Hệ thống hiển thị thông báo không tìm thấy hướng dẫn phù hợp.
- **And**: Người dùng có thể xóa từ khóa để xem lại danh sách hướng dẫn đang xuất bản theo thứ tự quản trị.

## References

### TDDs

- TDD-GUIDE-001

### Rules

- BR-GUIDE-001
- BR-GUIDE-002
- BR-GUIDE-003

### Dependencies

- STORY-GUIDE-001: người quản lý chuẩn bị, xuất bản, ẩn và sắp xếp nội dung.

## Non-Functional

- Việc truy cập phần công khai không làm lộ nội dung hướng dẫn đang nháp hoặc ẩn qua yêu cầu đọc mới.
- Lỗi một video không ngăn người dùng đóng trình phát hoặc chọn hướng dẫn khác.

## Out of Scope

- Đăng nhập bắt buộc, danh mục, bài viết kèm theo và nhiều bản ngôn ngữ.
- Bảo vệ video như nội dung trả phí; ẩn hướng dẫn trên BMT không thu hồi link YouTube đã được biết hoặc dữ liệu đã tải trước đó.
