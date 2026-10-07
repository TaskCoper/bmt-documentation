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

# STORY-HB-003

## Metadata

- **Story**: Là người có quyền quản lý Tin tức, tôi muốn chọn và sắp xếp các bài nổi bật trong khối Bản tin để người đọc thấy ngay nội dung đáng chú ý nhất.
- **Context**: Khối Bản tin trên trang Cẩm nang hiện bốn bài nổi bật đánh số 01 đến 04, kèm danh sách bài liên quan do hệ thống suy ra. Thứ tự do người quản lý quyết định, không theo ngày công bố. Bản nháp soạn từ các quyết định người dùng xác nhận ngày 07/10/2026.
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

- Người thao tác đăng nhập và có quyền quản lý tin tức theo STORY-RBAC-001.
- Đã có bài tin tức để chọn.

### Trigger

Người quản lý mở phần quản lý khối Bản tin để thêm, gỡ hoặc đổi thứ tự bài nổi bật.

## Flow

### Main Flow

1. Mở danh sách bài nổi bật đang có, theo thứ tự đã lưu.
2. Chọn thêm bài vào khối; tìm bài theo tiêu đề rồi thêm vào cuối danh sách.
3. Kéo đổi thứ tự các bài trong danh sách.
4. Lưu; hệ thống ghi lại toàn bộ thứ tự mới trong một lần.
5. Phần công khai hiện bốn bài đầu tiên đang công bố, đánh số 01 đến 04 theo thứ tự hiển thị thực tế.

### Alternative Flow

#### ALT-01

Xếp dư bài để dự phòng.

1. Người quản lý thêm nhiều hơn bốn bài vào danh sách.
2. Bài nào không còn công bố thì bị bỏ qua khi hiển thị và các bài sau dồn lên, nên khối vẫn đủ bốn bài.

#### ALT-02

Gỡ một bài khỏi khối.

1. Người quản lý gỡ bài khỏi danh sách rồi lưu.
2. Các bài còn lại giữ nguyên thứ tự tương đối; bài bị gỡ vẫn tồn tại và vẫn đọc được như bài thường.

#### ALT-03

Bài trong khối tạm ngừng hiển thị.

1. Người quản lý ẩn bài hoặc chuyển bài về nháp theo STORY-NEWS-001.
2. Bài không còn hiện trong khối nhưng vẫn giữ vị trí đã xếp; công bố lại thì bài trở về đúng vị trí cũ.

#### ALT-04

Khối chưa có bài nổi bật nào đang công bố.

1. Trang công khai không hiển thị dải Bản tin; các phần còn lại của trang giữ nguyên.
2. Màn quản trị báo khối đang không hiển thị để người quản lý thêm bài hoặc công bố lại bài đã xếp.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý tin tức.

1. Từ chối thao tác và giữ nguyên danh sách hiện tại, kể cả khi yêu cầu gửi thẳng tới API.

#### EXC-02

Thêm một bài đã có trong khối.

1. Từ chối và giữ nguyên danh sách; một bài chỉ nằm ở một vị trí.

#### EXC-03

Lưu thứ tự có chứa bài đã bị xóa trong lúc người quản lý đang thao tác.

1. Từ chối lưu và chỉ rõ bài không còn tồn tại; người quản lý tải lại danh sách rồi xếp lại.

## Acceptance Criteria

#### AC-001

- **Given**: Người thao tác không có quyền quản lý tin tức
- **When**: Gọi trực tiếp thao tác thêm, gỡ hoặc đổi thứ tự bài nổi bật
- **Then**: Bị từ chối và danh sách không thay đổi.

#### AC-002

- **Given**: Người quản lý đã xếp bốn bài đang công bố theo thứ tự
- **When**: Người đọc mở trang Cẩm nang
- **Then**: Khối Bản tin hiện đúng bốn bài đó, đánh số 01 đến 04 theo thứ tự đã lưu.
- **And**: Thứ tự không phụ thuộc ngày công bố của bài.

#### AC-003

- **Given**: Người quản lý đã xếp sáu bài và bài ở vị trí 2 bị ẩn
- **When**: Người đọc mở trang Cẩm nang
- **Then**: Khối hiện các bài ở vị trí 1, 3, 4, 5 và đánh số 01 đến 04.

#### AC-004

- **Given**: Bài ở vị trí 2 đang bị ẩn và khối đang hiện bài vị trí 5
- **When**: Người quản lý công bố lại bài vị trí 2
- **Then**: Bài đó trở lại số 02 và bài vị trí 5 lui ra ngoài khối.
- **And**: Người quản lý không phải xếp lại thứ tự.

#### AC-005

- **Given**: Một bài đang nằm trong khối nổi bật
- **When**: Người quản lý xóa bài đó
- **Then**: Bài bị gỡ khỏi khối và các bài sau dồn lên.

#### AC-006

- **Given**: Một bài đã có trong khối
- **When**: Thêm lại chính bài đó
- **Then**: Bị từ chối; bài vẫn chỉ nằm ở một vị trí.

#### AC-007

- **Given**: Bài đứng đầu khối gắn danh mục "Vật liệu"
- **When**: Người đọc mở trang Cẩm nang
- **Then**: Danh sách bài liên quan gồm các bài đang công bố cùng danh mục với bài đứng đầu.
- **And**: Không chứa các bài đang hiển thị trong khối nổi bật và người quản lý không chọn tay danh sách này.

#### AC-008

- **Given**: Khối chưa có bài nào, hoặc mọi bài đã xếp đều đang ẩn hoặc đang nháp
- **When**: Người đọc mở trang Cẩm nang
- **Then**: Trang không hiển thị dải Bản tin; phần mở đầu, ba bước và khối bài viết vẫn hiển thị bình thường.
- **And**: Hệ thống không tự lấy bài khác lấp vào khối, và màn quản trị báo khối đang không hiển thị.

## References

### TDDs

- TDD-HB-002

### Rules

- BR-HB-002
- BR-NEWS-003

### Dependencies

- STORY-NEWS-001: tạo, công bố, ẩn và xóa bài tin tức.

## Non-Functional

- Kiểm quyền tại backend cho mọi thao tác quản trị.
- Sắp xếp lại khối không được làm đổi ngày sửa của các bài, vì danh sách quản trị bài xếp theo lần sửa gần nhất.

## Out of Scope

- Nhiều khối nổi bật khác nhau trên cùng một trang hoặc trên các trang khác.
- Chọn tay danh sách bài liên quan.
- Liên kết xem tất cả bài liên quan; khối chỉ hiện danh sách suy ra, không dẫn sang trang khác.
- Hẹn giờ đưa bài vào hoặc ra khỏi khối nổi bật.
