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

# STORY-CMS-001

## Metadata

- **Story**: Là người quản lý nội dung, tôi muốn thêm, xóa và sắp xếp nhóm và hạng mục trong bảng so sánh BASIC/PLUS/PRO để cập nhật trang bảng giá mà không sửa mã ứng dụng.
- **Context**: Bảng so sánh trong quản trị đã có dạng bảng nhưng số nhóm, số hàng và nhiều giá trị vẫn cố định theo mã hoặc cấu hình gói. Người dùng yêu cầu quản lý động nội dung này như CMS, độc lập với quyền sử dụng và hạn mức thuê bao.
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

- Người quản lý đã đăng nhập và có quyền quản lý nội dung `news.manage`, theo màn CMS bảng giá hiện có.
- Màn quản trị Nội dung → Gói tư vấn → tab Gói thiết kế có phần Bảng so sánh chi tiết.

### Trigger

Người quản lý mở phần Bảng so sánh chi tiết để thay đổi nội dung hoặc cấu trúc của bảng.

## Flow

### Main Flow

1. Người quản lý chọn tab Gói thiết kế, chọn ngôn ngữ và mở Bảng so sánh chi tiết tại `/admin/plans-content`.
2. Hệ thống hiển thị bảng bốn cột Hạng mục, BASIC, PLUS, PRO với nội dung hiện có của ngôn ngữ được chọn.
3. Người quản lý thêm nhóm, sửa tên nhóm và thêm hạng mục vào nhóm.
4. Với từng hạng mục, người quản lý sửa tên hàng và chọn Có (✓), Không (–) hoặc nhập chữ/số riêng cho mỗi cột BASIC, PLUS, PRO.
5. Người quản lý đổi thứ tự các nhóm và các hạng mục trong nhóm, hoặc xóa mục không còn dùng.
6. Người quản lý lưu phần Bảng so sánh chi tiết. Hệ thống kiểm nội dung, quyền và phiên bản trước khi lưu theo ngôn ngữ.
7. Sau khi lưu thành công, trang `/plans` hiển thị nhóm, hàng và các ô theo nội dung và thứ tự CMS đã lưu.
8. Cấu hình quyền sử dụng, hạn mức, giá và các thuê bao đã mua giữ nguyên sau thao tác CMS.

### Alternative Flow

#### ALT-01

Người quản lý sửa hai hàng số lượt hiện có.

1. Người quản lý nhập chữ/số riêng cho BASIC, PLUS, PRO ở hàng Số phương án thiết kế mới hoặc Tra cứu thư viện mẫu.
2. Khi lưu, hệ thống cập nhật nội dung bảng CMS; không cập nhật hạn mức sử dụng của gói và không cấp thêm lượt cho khách hàng.

#### ALT-02

Người quản lý đổi ngôn ngữ hoặc khôi phục mặc định theo cơ chế CMS hiện có.

1. Hệ thống đọc hoặc khôi phục phần comparison của đúng ngôn ngữ được chọn.
2. Nội dung đã lưu của ngôn ngữ khác giữ nguyên; không mượn chữ của ngôn ngữ khác để thay thế.

#### ALT-03

Người quản lý xóa một nhóm đang có hạng mục.

1. Hệ thống bỏ nhóm và các hạng mục thuộc nhóm khỏi nội dung đang sửa.
2. Thay đổi chỉ xuất hiện trên trang công khai khi lưu thành công; không xóa hay sửa định nghĩa quyền lợi thuê bao.

### Exception Flow

#### EXC-01

Nội dung không đúng cấu trúc hoặc vượt giới hạn chữ hiện có của CMS.

1. Hệ thống báo lỗi ở phần nhập liên quan và không thay thế nội dung đã lưu.
2. Người quản lý sửa nội dung và thử lưu lại; hệ thống không tự cắt chữ để vượt kiểm tra.

#### EXC-02

Nội dung đã bị người khác sửa sau lần tải hiện tại.

1. Hệ thống báo xung đột phiên bản theo cơ chế CMS hiện có.
2. Bản đã lưu của người khác được giữ; người quản lý tải lại nội dung trước khi tiếp tục sửa.

#### EXC-03

Tài khoản không có quyền hoặc yêu cầu lưu thất bại.

1. Hệ thống từ chối thao tác hoặc hiển thị lỗi hiện có; không báo đã lưu thành công.
2. Bảng công khai tiếp tục dùng nội dung đã lưu trước đó; không thay cấu hình gói.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản lý đang mở bảng so sánh trong quản trị.
- **When**: Thêm một nhóm và thêm hạng mục vào nhóm đó.
- **Then**: Có thể nhập tên nhóm, tên hạng mục và giá trị riêng của ba cột ngay trong bảng.
- **And**: Việc thêm nhóm hoặc hạng mục không yêu cầu sửa mã ứng dụng hoặc tạo quyền lợi thuê bao.

#### AC-002

- **Given**: Một hạng mục có ba ô BASIC, PLUS, PRO.
- **When**: Người quản lý lần lượt chọn Có, Không và Nhập chữ cho ba ô rồi lưu.
- **Then**: Trang công khai hiển thị ✓, – và đúng chữ đã nhập ở các cột tương ứng.
- **And**: Chữ CMS hiển thị an toàn như chữ thuần, không thực thi HTML hoặc mã do người soạn nhập.

#### AC-003

- **Given**: Có nhiều nhóm và nhiều hạng mục trong một nhóm.
- **When**: Người quản lý đổi thứ tự nhóm và hạng mục rồi lưu.
- **Then**: Quản trị và trang công khai dùng đúng thứ tự đã lưu.

#### AC-004

- **Given**: Một nhóm có hạng mục đã được lưu.
- **When**: Người quản lý xóa hạng mục hoặc xóa cả nhóm rồi lưu.
- **Then**: Mục bị xóa không còn xuất hiện trong bảng công khai đã cập nhật.
- **And**: Xóa nhóm đồng thời bỏ các hạng mục thuộc nhóm khỏi bảng CMS.

#### AC-005

- **Given**: Hạn mức thật của BASIC đã được cấu hình trong quản lý gói.
- **When**: Người quản lý nhập nội dung khác vào ô BASIC của hàng Số phương án thiết kế mới hoặc Tra cứu thư viện mẫu rồi lưu CMS.
- **Then**: Bảng công khai hiển thị nội dung CMS đã nhập.
- **And**: Hạn mức thật, lượt đã sử dụng và quyền đã cấp cho khách không thay đổi.

#### AC-006

- **Given**: Có nội dung CMS tiếng Việt đã lưu.
- **When**: Người quản lý lưu hoặc khôi phục phần comparison tiếng Anh.
- **Then**: Phần comparison tiếng Việt giữ nguyên.
- **And**: Đóng/mở phần comparison không làm mất nội dung đang sửa trong form, theo hành vi hiện có.

#### AC-007

- **Given**: Nội dung gửi lên sai kiểu, vượt giới hạn chữ hoặc dùng phiên bản cũ.
- **When**: Người quản lý lưu bảng.
- **Then**: Hệ thống từ chối với lỗi phù hợp và giữ nội dung đã lưu trước đó.
- **And**: Thiếu quyền `news.manage` cũng bị backend từ chối, kể cả khi gọi trực tiếp API.

#### AC-008

- **Given**: Bảng chưa có bản CMS động nhưng có nội dung theo cấu trúc cũ.
- **When**: Người quản lý mở bảng để biên tập và lưu lần đầu.
- **Then**: Nội dung cũ là điểm bắt đầu, gồm các câu chữ đã sửa và giá trị đang hiển thị.
- **And**: Các ô sau khi lưu là nội dung CMS độc lập, không tự đổi theo hạn mức thật.

#### AC-009

- **Given**: Bảng có nhóm và hạng mục.
- **When**: Người quản lý xóa hết nhóm rồi lưu.
- **Then**: Trang công khai không tự thêm lại nhóm/hàng mặc định; vẫn giữ tiêu đề và hàng nút chọn gói.
- **And**: Nhóm không có hạng mục được giữ trong quản trị để tiếp tục sửa nhưng không hiện ở trang công khai.

## References

### TDDs

- TDD-HB-003/Architecture: Cơ chế PageSection dùng chung và CMS bảng giá hiện có; phần bổ sung mô tả dữ liệu nhóm/hàng và tương thích nội dung cũ.

### Rules

- BR-CMS-001

### Dependencies

- BR-SUB-008/Then: Khoản 12 phân biệt hai quyền có logic tính lượt với cấu hình chỉ để lưu và hiển thị.

## Non-Functional

- Dùng cơ chế quyền, lưu theo ngôn ngữ và kiểm phiên bản của CMS hiện có.
- Giữ bảng bốn cột, cho cuộn ngang trên màn hình nhỏ; thao tác nhập và sắp xếp phải dùng được bằng bàn phím.
- Nội dung CMS là chữ thuần; giữ giới hạn chữ hiện có và báo lỗi thay vì tự cắt nội dung.

## Out of Scope

- Tạo định nghĩa quyền lợi thuê bao, cấp quyền, thay hạn mức, trừ lượt hoặc sửa thuê bao đã mua.
- Thay giá, thẻ gói, thao tác chọn gói hoặc quy trình mua/Công bố gói.
- Thay đổi bốn phần CMS khác của trang bảng giá.
- Tạo lịch sử phiên bản nội dung, hẹn giờ xuất bản hoặc thêm quy trình duyệt trong sản phẩm.
