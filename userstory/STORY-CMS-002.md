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

# STORY-CMS-002

## Metadata

- **Story**: Là người quản lý nội dung, tôi muốn thêm, sửa, xóa và sắp xếp hạng mục, chọn icon cho từng hàng của bảng Giá trị khách hàng nhận được để cập nhật CMS Bảng giá.
- **Context**: Bảng quản trị đã có bốn cột nhưng chỉ sửa được bốn hàng cố định; icon ở trang công khai đang viết cố định trong mã. Người dùng yêu cầu thêm hạng mục và chọn icon, đồng thời xác nhận cho phép xóa và sắp xếp hàng.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Fullstack: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người quản lý đã đăng nhập và có quyền `news.manage` của CMS Bảng giá hiện có.
- Màn Nội dung → Gói tư vấn → tab Gói thiết kế có phần Giá trị khách hàng nhận được.

### Trigger

Người quản lý cần cập nhật các hạng mục và icon trong bảng giá trị khách hàng.

## Flow

### Main Flow

1. Người quản lý chọn tab Gói thiết kế, chọn ngôn ngữ và mở Giá trị khách hàng nhận được tại `/admin/plans-content`.
2. Hệ thống hiển thị tiêu đề và bảng Hạng mục/BASIC/PLUS/PRO.
3. Người quản lý thêm hạng mục, nhập tên hàng, chọn icon có xem trước và nhập chữ riêng trong ba cột.
4. Người quản lý sửa, xóa hoặc sắp xếp các hạng mục.
5. Người quản lý lưu phần nội dung. Hệ thống kiểm dữ liệu, quyền và phiên bản trước khi cập nhật CMS của ngôn ngữ đang sửa.
6. Trang `/plans` hiển thị các hàng, nội dung, thứ tự và icon đã lưu. Thao tác CMS không đổi quyền, giá hoặc hạn mức thuê bao.

### Alternative Flow

#### ALT-01

Bảng chưa có danh sách hạng mục động đã lưu.

1. hệ thống đưa bốn hàng hiện có cùng nội dung CMS đã nhập và bốn icon cũ vào bảng làm điểm bắt đầu.
2. Khi lưu, hệ thống ghi danh sách động; mở bảng chưa tự ghi dữ liệu.

#### ALT-02

Người quản lý xóa hạng mục cuối cùng.

1. hệ thống cho lưu danh sách rỗng.
2. Trang công khai giữ tiêu đề và không tự thêm lại bốn hàng mặc định. Khôi phục mặc định là thao tác riêng theo ngôn ngữ hiện có.

### Exception Flow

#### EXC-01

Nội dung sai định dạng, không đủ quyền hoặc phiên bản đã bị thay đổi.

1. Hệ thống áp dụng thông báo validation, quyền và cơ chế xử lý xung đột phiên bản của CMS hiện có.
2. Lỗi tải danh sách icon hoặc icon không tìm thấy không được làm mất chữ đã nhập; dùng icon dự phòng khi tên không tra được.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản lý đang sửa bảng giá trị khách hàng.
- **When**: Thêm một hạng mục và nhập tên, icon cùng chữ trong BASIC/PLUS/PRO rồi lưu.
- **Then**: Mở lại admin và trang công khai thấy hàng mới với các giá trị đã nhập.
- **And**: Không tạo hoặc sửa quyền sử dụng, giá và hạn mức thật.

#### AC-002

- **Given**: Bảng có nhiều hạng mục.
- **When**: Người quản lý sửa, xóa hoặc sắp xếp hàng rồi lưu.
- **Then**: Trang công khai hiển thị nội dung và thứ tự đã lưu, không còn hàng đã xóa.

#### AC-003

- **Given**: Người quản lý chọn icon của một hàng.
- **When**: Chọn một icon trong thư viện icon đang dùng của frontend.
- **Then**: Có xem trước trong admin và trang công khai hiển thị icon đã chọn sau khi lưu.

#### AC-004

- **Given**: Bảng chưa có danh sách động; người dùng đã sửa chữ trong bốn hàng cũ.
- **When**: Mở bảng mới.
- **Then**: Giữ toàn bộ chữ đã nhập, thứ tự và bốn icon cũ làm dữ liệu ban đầu.

#### AC-005

- **Given**: Người quản lý đã xóa hết hạng mục.
- **When**: Lưu bảng rỗng.
- **Then**: Giữ tiêu đề công khai, không phục hồi hàng mặc định; khôi phục mặc định chỉ diễn ra qua thao tác riêng.

#### AC-006

- **Given**: Người quản lý đang sửa một ngôn ngữ.
- **When**: Lưu hoặc khôi phục bảng.
- **Then**: Chỉ nội dung của ngôn ngữ đang sửa thay đổi.
- **And**: Đóng/mở section giữ nội dung đang nhập; điện thoại cuộn và sửa được cả cột PRO; kiểm quyền, độ dài chữ và phiên bản theo CMS hiện có.

## References

### TDDs

- TDD-HB-003/Architecture: CMS PageSection và khối plans/value hiện có; danh sách động và tương thích dữ liệu cũ.

### Rules

- BR-CMS-002
- BR-SUB-008/Then: CMS không tạo entitlement hoặc đổi hai hạn mức sử dụng thật.

### Dependencies

- STORY-CMS-001: Dùng lại màn CMS Bảng giá và cơ chế ngôn ngữ, lưu, khôi phục đã có.

## Non-Functional

- Giữ quyền CMS `news.manage`, kiểm phiên bản và giới hạn chữ hiện có.
- Không đưa toàn bộ mã component icon của thư viện vào bundle client chỉ để chọn icon.
- Ô nhập và thao tác chọn icon, thêm, xóa, sắp xếp phải dùng được bằng bàn phím.

## Out of Scope

- Thay cấu hình gói, giá, quyền sử dụng và hạn mức thuê bao.
- Thay các khối CMS khác của trang bảng giá.
- Tải lên icon tùy chỉnh hoặc ảnh riêng; yêu cầu này dùng thư viện icon của frontend.
