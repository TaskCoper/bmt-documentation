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

# STORY-GUIDE-001

## Metadata

- **Story**: Là người có quyền Quản lý hướng dẫn, tôi muốn tạo, chỉnh sửa, xuất bản, ẩn, xóa và sắp xếp video hướng dẫn để người dùng xem được nội dung sử dụng BMT phù hợp.
- **Context**: Trang hướng dẫn hiện có các mục video nhưng mục đã kiểm tra còn hiển thị thông báo đang biên tập. Cần quản lý nội dung từ admin, gán video đã lưu trên YouTube và chủ động quyết định khi nào hiển thị. Bản nháp được soạn từ các quyết định ngày 30/09/2026; đã được người dùng chốt toàn văn trong hội thoại.
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

- Người quản lý đã đăng nhập và được cấp quyền Quản lý hướng dẫn trong hệ thống phân quyền hiện có.
- Khi gán video hoặc xuất bản, người quản lý có link tới video trên YouTube. Có thể bắt đầu lưu nháp trước khi có link.

### Trigger

Người quản lý mở phần quản lý hướng dẫn để thêm mới hoặc thay đổi một hướng dẫn.

## Flow

### Main Flow

1. Người quản lý mở danh sách hướng dẫn và chọn tạo mới.
2. Hệ thống hiển thị form tiêu đề tối đa 200 ký tự, mô tả ngắn tối đa 2.000 ký tự và link của một video YouTube đã đăng (gồm Shorts); nội dung được quản lý bằng một bản tiếng Việt.
3. Người quản lý nhập nội dung và dán link YouTube.
4. Hệ thống lấy ảnh đại diện và thời lượng của video từ YouTube, hiển thị để người quản lý kiểm tra.
5. Người quản lý lưu hướng dẫn. Hệ thống tạo bản Nháp, chưa cung cấp cho người dùng công khai.
6. Người quản lý chọn Xuất bản khi nội dung đã sẵn sàng.
7. Hệ thống kiểm tra đủ tiêu đề, mô tả ngắn, link hợp lệ và kết quả kiểm tra video theo BR-GUIDE-002.
8. Nếu đủ điều kiện, hệ thống chuyển hướng dẫn sang Xuất bản. Người dùng có thể xem hướng dẫn theo STORY-GUIDE-002.
9. Người quản lý sắp xếp hướng dẫn theo thứ tự muốn hiển thị.

### Alternative Flow

#### ALT-01

Người quản lý chưa nhập đủ thông tin và muốn lưu lại để làm tiếp.

1. Hệ thống cho lưu Nháp khi còn thiếu thông tin.
2. Hướng dẫn chưa đủ điều kiện không được xuất bản; người quản lý có thể mở lại và bổ sung.

#### ALT-02

Người quản lý sửa hướng dẫn đang xuất bản.

1. Người quản lý mở hướng dẫn, thay đổi nội dung rồi lưu.
2. Hệ thống kiểm tra bản sửa vẫn đủ tiêu đề, mô tả ngắn và link hợp lệ; nếu thay video, kiểm tra video mới theo BR-GUIDE-002. Nếu không đạt, chuyển EXC-05.
3. Sau khi lưu thành công, nội dung sửa có hiệu lực ngay trên phần công khai; không tạo bản sửa chờ xuất bản riêng.
4. Nếu cần chuẩn bị nội dung trước, người quản lý ẩn hướng dẫn rồi mới sửa.

#### ALT-03

Người quản lý muốn ngừng hiển thị một hướng dẫn.

1. Người quản lý chọn Ẩn đối với hướng dẫn đang xuất bản.
2. Hệ thống chuyển sang Ẩn và ngừng cung cấp hướng dẫn trên phần công khai.

#### ALT-04

Người quản lý muốn hiển thị lại hướng dẫn đã ẩn.

1. Người quản lý bổ sung hoặc sửa nội dung nếu cần, rồi chọn Xuất bản.
2. Hệ thống kiểm lại điều kiện xuất bản như Main Flow; chỉ chuyển sang Xuất bản khi đủ điều kiện.

#### ALT-05

Người quản lý muốn xóa hẳn hướng dẫn đang nháp hoặc đã ẩn.

1. Người quản lý chọn Xóa và xác nhận thao tác trên giao diện.
2. Hệ thống xóa hướng dẫn khỏi BMT. Thao tác không xóa video trên YouTube.

#### ALT-06

Người quản lý muốn thay đổi thứ tự hiển thị.

1. Người quản lý sắp xếp các hướng dẫn và lưu thứ tự.
2. Phần công khai hiển thị các hướng dẫn đang xuất bản theo thứ tự đã lưu.

#### ALT-07

YouTube tạm thời lỗi trong lúc lấy thông tin video cho bản nháp.

1. Hệ thống báo chưa lấy hoặc kiểm tra được thông tin video.
2. Người quản lý vẫn được lưu link đúng định dạng vào bản Nháp; hệ thống không trình bày thông tin của video cũ như thông tin của link mới.
3. Người quản lý thử lấy thông tin lại sau. Hướng dẫn chỉ được xuất bản khi kiểm tra video thành công và đủ các trường bắt buộc.

### Exception Flow

#### EXC-01

Người thao tác chưa đăng nhập hoặc không có quyền Quản lý hướng dẫn.

1. Hệ thống từ chối thao tác quản trị và không thay đổi dữ liệu, kể cả khi yêu cầu gửi trực tiếp đến API.

#### EXC-02

Hướng dẫn thiếu tiêu đề, mô tả ngắn hoặc link hợp lệ khi yêu cầu xuất bản.

1. Hệ thống chỉ ra thông tin còn thiếu hoặc không hợp lệ, không chuyển trạng thái.
2. Người quản lý bổ sung thông tin rồi thử xuất bản lại.

#### EXC-03

Không kiểm tra được video do YouTube lỗi, video không tồn tại hoặc không cho phát trên website.

1. Hệ thống báo vấn đề cho người quản lý và không xuất bản hướng dẫn.
2. Trạng thái trước yêu cầu được giữ nguyên; người quản lý sửa link hoặc thử lại khi dịch vụ hoạt động.

#### EXC-04

Người quản lý yêu cầu xóa hướng dẫn đang xuất bản.

1. Hệ thống từ chối xóa, giữ nguyên hướng dẫn và yêu cầu ẩn trước.
2. Sau khi ẩn thành công, người quản lý có thể thực hiện ALT-05.

#### EXC-05

Người quản lý lưu sửa hướng dẫn đang xuất bản nhưng thiếu thông tin bắt buộc, link không hợp lệ hoặc video mới chưa kiểm tra được.

1. Hệ thống từ chối lưu và chỉ rõ thông tin còn thiếu hoặc vấn đề kiểm tra video.
2. Toàn bộ bản đang hiển thị được giữ nguyên, gồm nội dung, link video, ảnh đại diện và thời lượng.
3. Người quản lý sửa thông tin và thử lại; nếu cần chuẩn bị trước, người quản lý có thể ẩn hướng dẫn rồi sửa.

#### EXC-06

Nội dung vượt giới hạn hoặc video là livestream đang diễn ra hay sắp phát.

1. Hệ thống báo trường vượt giới hạn, không lưu và không tự cắt nội dung.
2. Khi kiểm tra phát hiện livestream đang diễn ra hoặc sắp phát, hệ thống báo loại video chưa được hỗ trợ và từ chối gán video đó.
3. Yêu cầu bị từ chối không thay đổi dữ liệu hoặc trạng thái đang lưu.

## Acceptance Criteria

#### AC-001

- **Given**: Người thao tác không có quyền Quản lý hướng dẫn.
- **When**: Người đó yêu cầu đọc hoặc thay đổi dữ liệu quản trị hướng dẫn.
- **Then**: Hệ thống từ chối truy cập phần quản trị.
- **And**: Dữ liệu hướng dẫn không thay đổi.

#### AC-002

- **Given**: Người quản lý đang tạo hướng dẫn và chưa nhập đủ thông tin.
- **When**: Người đó lưu nháp.
- **Then**: Hệ thống lưu hướng dẫn ở trạng thái Nháp.
- **And**: Hướng dẫn không xuất hiện trên phần công khai.

#### AC-003

- **Given**: Người quản lý dán link tới một video và YouTube trả được thông tin.
- **When**: Hệ thống lấy thông tin video.
- **Then**: Ảnh đại diện và thời lượng hiển thị đúng theo video được gán.
- **And**: Người quản lý không phải tải video lên BMT hoặc nhập tay ảnh đại diện, thời lượng.

#### AC-004

- **Given**: Hướng dẫn Nháp hoặc Ẩn đã có tiêu đề, mô tả ngắn, link hợp lệ và video đáp ứng điều kiện xuất bản.
- **When**: Người quản lý chọn Xuất bản.
- **Then**: Hệ thống chuyển hướng dẫn sang Xuất bản.
- **And**: Người dùng công khai có thể truy cập hướng dẫn.

#### AC-005

- **Given**: Hướng dẫn thiếu thông tin bắt buộc hoặc việc kiểm tra video thất bại.
- **When**: Người quản lý chọn Xuất bản.
- **Then**: Hệ thống thông báo lý do không thể xuất bản.
- **And**: Trạng thái của hướng dẫn không thay đổi.

#### AC-006

- **Given**: Hướng dẫn đang xuất bản.
- **When**: Người quản lý lưu sửa thành công.
- **Then**: Nội dung mới có hiệu lực trên phần công khai.
- **And**: Không cần một lần xuất bản riêng cho bản sửa.

#### AC-007

- **Given**: Hướng dẫn đang xuất bản.
- **When**: Người quản lý ẩn thành công.
- **Then**: Hướng dẫn không còn xuất hiện trong danh sách công khai.
- **And**: Yêu cầu đọc công khai mới không lấy được nội dung của hướng dẫn đó.

#### AC-008

- **Given**: Hướng dẫn đang xuất bản.
- **When**: Người quản lý yêu cầu xóa.
- **Then**: Hệ thống từ chối xóa và yêu cầu ẩn trước.
- **And**: Hướng dẫn được giữ nguyên.

#### AC-009

- **Given**: Hướng dẫn đang nháp hoặc đã ẩn.
- **When**: Người quản lý xác nhận xóa và thao tác thành công.
- **Then**: Hướng dẫn bị xóa hẳn khỏi BMT.
- **And**: Video trên YouTube không bị xóa bởi thao tác này.

#### AC-010

- **Given**: Có nhiều hướng dẫn đang xuất bản.
- **When**: Người quản lý lưu thứ tự mới.
- **Then**: Danh sách công khai thể hiện thứ tự mới đó.
- **And**: Việc sắp xếp không làm xuất bản các hướng dẫn đang nháp hoặc ẩn.

#### AC-011

- **Given**: Người quản lý đang lưu bản Nháp với link YouTube đúng định dạng nhưng YouTube tạm thời lỗi.
- **When**: Người đó chọn lưu nháp.
- **Then**: Hệ thống lưu bản Nháp cùng link đã nhập và thông báo chưa kiểm tra được video.
- **And**: Không xuất bản hướng dẫn hoặc hiển thị ảnh, thời lượng của video cũ như dữ liệu của link mới.

#### AC-012

- **Given**: Hướng dẫn đang xuất bản và bản sửa thiếu tiêu đề, mô tả ngắn hoặc link hợp lệ.
- **When**: Người quản lý chọn lưu.
- **Then**: Hệ thống từ chối lưu và thông báo thông tin không hợp lệ.
- **And**: Toàn bộ bản đang hiển thị được giữ nguyên.

#### AC-013

- **Given**: Hướng dẫn đang xuất bản và người quản lý thay sang video mới nhưng chưa kiểm tra được video đó.
- **When**: Người quản lý chọn lưu.
- **Then**: Hệ thống từ chối lưu và thông báo lỗi kiểm tra video.
- **And**: Nội dung, link video, ảnh đại diện và thời lượng của bản đang hiển thị không thay đổi.

#### AC-014

- **Given**: Bản Nháp đã lưu link nhưng chưa kiểm tra được video do YouTube tạm thời lỗi.
- **When**: Người quản lý thử lấy thông tin lại và YouTube trả kết quả thành công.
- **Then**: Hệ thống hiển thị ảnh đại diện và thời lượng của đúng video được gán.
- **And**: Hướng dẫn vẫn là Nháp cho đến khi người quản lý chủ động xuất bản và các điều kiện xuất bản được đáp ứng.

#### AC-015

- **Given**: Người quản lý nhập tiêu đề và mô tả ngắn.
- **When**: Người đó lưu nháp, sửa hoặc xuất bản.
- **Then**: Giới hạn là 200 ký tự cho tiêu đề và 2.000 ký tự cho mô tả.
- **And**: Vượt một trong hai giới hạn thì từ chối, chỉ rõ trường lỗi, giữ dữ liệu cũ và không tự cắt nội dung.

#### AC-016

- **Given**: Người quản lý gán link YouTube và hệ thống kiểm tra được video.
- **When**: Video đã đăng, bao gồm Shorts, đáp ứng các điều kiện còn lại.
- **Then**: Hệ thống chấp nhận video.
- **And**: Nếu là livestream đang diễn ra hoặc sắp phát, hệ thống từ chối và báo loại video chưa được hỗ trợ; dữ liệu đang lưu không đổi.

## References

### TDDs

- TDD-GUIDE-001

### Rules

- BR-GUIDE-001
- BR-GUIDE-002
- BR-GUIDE-003

### Dependencies

- STORY-GUIDE-002: xem danh sách và phát video hướng dẫn công khai.

## Non-Functional

- Kiểm quyền tại backend cho mọi thao tác quản trị; ẩn nút trên giao diện không thay thế kiểm quyền.
- Lỗi lấy thông tin YouTube phải được thông báo, không thể hiện như đã kiểm tra video thành công.

## Out of Scope

- Danh mục, bài viết hướng dẫn kèm theo, nhiều bản ngôn ngữ và tải video lên BMT.
- Bản sửa riêng chờ xuất bản, thùng rác và khôi phục hướng dẫn đã xóa hẳn.
- Quản lý hoặc xóa video trên tài khoản YouTube.
