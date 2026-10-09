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


# STORY-CMS-003

## Metadata

- **Story**: Là người quản lý nội dung, tôi muốn biên tập các bảng và mục giới thiệu gói giám sát để cập nhật trang công khai mà không sửa mã ứng dụng.
- **Context**: Trang `/plans/supervision` hiện có các bảng và danh sách cố định. Người dùng yêu cầu CMS tương tự Gói tư vấn cho sáu phần trong ảnh, cho thêm, sửa, xóa và sắp xếp nội dung; các ô số được nhập riêng, độc lập với cấu hình gói thật.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Fullstack: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người quản lý đã đăng nhập và có quyền CMS `news.manage` như màn Gói tư vấn.
- Trang công khai `/plans/supervision` và cơ chế CMS PageSection đang hoạt động.

### Trigger

Người quản lý mở Nội dung → Gói tư vấn → tab Gói giám sát để cập nhật nội dung giới thiệu trên trang gói giám sát.

## Flow

### Main Flow

1. Người quản lý mở Nội dung → Gói tư vấn, chọn tab Gói giám sát rồi chọn ngôn ngữ. Tab Gói thiết kế nằm cùng màn và giữ nội dung riêng.
2. Hệ thống hiển thị sáu phần có thể đóng/mở: Bảng so sánh chi tiết; Dịch vụ thêm và phụ phí; Nguyên tắc phạm vi dịch vụ; Hành trình giám sát; Giá trị khách hàng nhận được; Ghi chú cuối trang.
3. Trong bảng so sánh, người quản lý thêm, sửa, xóa, sắp xếp nhóm và hạng mục. Mỗi ô Tự quản lý/An tâm/Toàn diện chọn Có (✓), Không (–) hoặc nhập chữ/số.
4. Trong bảng phụ phí, người quản lý thêm, sửa, xóa, sắp xếp hàng với các cột Hạng mục/Giá đề xuất/Ghi chú.
5. Người quản lý thêm, sửa, xóa, sắp xếp các nguyên tắc phạm vi và các bước hành trình. Mỗi bước có tiêu đề, mô tả và icon xem trước; số bước hiển thị theo thứ tự hiện tại.
6. Trong bảng giá trị khách hàng, người quản lý thêm, sửa, xóa, sắp xếp hạng mục; nhập tên, chọn icon và nhập chữ riêng cho ba cột.
7. Người quản lý sửa ba ghi chú cuối trang, các tiêu đề phần, tên cột hạng mục và chữ nút chọn gói. Các phần có icon cho chọn icon từ thư viện đang dùng.
8. Khi lưu từng phần hoặc lưu tất cả phần đã thay đổi, hệ thống kiểm dữ liệu, quyền và phiên bản; lưu riêng theo ngôn ngữ và cập nhật cache công khai như CMS Gói tư vấn.
9. Trang công khai hiển thị đúng nội dung đã lưu. Tên và giá ở đầu cột vẫn theo nguồn cấu hình gói hiện có; thao tác mua dùng đúng gói thật và giữ công trình đang chọn.

### Alternative Flow

#### ALT-01

Chưa có nội dung CMS của ngôn ngữ đang chọn.

1. Hệ thống dùng nội dung mặc định hiện có của chính ngôn ngữ đó làm điểm bắt đầu; không tự ghi dữ liệu khi mở màn hình.
2. Lưu thành công tạo bản biên tập cho phần tương ứng và hiển thị trạng thái Đã biên tập.

#### ALT-02

Người quản lý xóa hết các mục của một danh sách động.

1. Hệ thống cho lưu danh sách rỗng; trang công khai giữ tiêu đề và không tự thêm lại các mục mặc định.
2. Bảng so sánh vẫn giữ đầu bảng và hàng chọn gói. Nhóm chưa có hạng mục chỉ hiện trong quản trị, theo hành vi Gói tư vấn.

#### ALT-03

Người quản lý khôi phục mặc định một phần hoặc đổi ngôn ngữ.

1. Khôi phục chỉ áp dụng cho phần và ngôn ngữ đã chọn; nội dung ở ngôn ngữ khác giữ nguyên.
2. Hệ thống dùng cơ chế xử lý bản đang sửa và trạng thái mặc định/đã biên tập của CMS hiện có.

### Exception Flow

#### EXC-01

Dữ liệu sai cấu trúc hoặc vượt giới hạn CMS đang dùng.

1. Hệ thống báo lỗi, không tự cắt chữ hoặc thay bản đã lưu bằng dữ liệu không hợp lệ.
2. Người quản lý sửa dữ liệu và lưu lại.

#### EXC-02

Bản nội dung đã được người khác cập nhật hoặc tài khoản không có quyền.

1. Hệ thống từ chối theo cơ chế phiên bản hoặc phân quyền hiện có; không ghi đè bản mới hơn và không báo lưu thành công.
2. Người quản lý tải lại nội dung hoặc dùng tài khoản có quyền phù hợp trước khi tiếp tục.

#### EXC-03

Icon chưa chọn, không tồn tại hoặc dữ liệu vẽ icon không tải được.

1. Hệ thống dùng icon dự phòng của frontend; giữ nguyên chữ đã nhập.
2. Lỗi tải icon không làm mất bản đang sửa hoặc nội dung công khai đã lưu.

## Acceptance Criteria

#### AC-001

- **Given**: Người có quyền CMS mở section Nội dung trên admin.
- **When**: Chọn Gói tư vấn rồi mở tab Gói giám sát.
- **Then**: Có sáu phần đúng phạm vi ảnh, với ngôn ngữ, lưu, khôi phục và trạng thái mặc định/đã biên tập tương tự Gói tư vấn.
- **And**: Đóng/mở phần không làm mất nội dung đang nhập.

#### AC-002

- **Given**: Người quản lý mở Bảng so sánh chi tiết.
- **When**: Thêm, sửa, xóa hoặc sắp xếp nhóm và hạng mục rồi lưu.
- **Then**: Bảng quản trị và công khai dùng đúng cấu trúc, nội dung và thứ tự đã lưu.
- **And**: Các ô Có/Không/Nhập chữ hiển thị tương ứng ✓/–/chữ thuần.

#### AC-003

- **Given**: Bảng có các hàng số lần kiểm tra, thời gian áp dụng và chi phí.
- **When**: Người quản lý sửa nội dung từng ô rồi lưu CMS.
- **Then**: Chỉ nội dung các ô CMS thay đổi; tên và giá đầu cột vẫn theo nguồn cấu hình gói hiện có.
- **And**: Giá bán, vòng đời gói, liên kết công trình, đơn mua, lịch và số lượt sử dụng không bị cập nhật bởi CMS.

#### AC-004

- **Given**: Người quản lý mở bảng phụ phí hoặc danh sách nguyên tắc phạm vi.
- **When**: Thêm, sửa, xóa và sắp xếp mục rồi lưu.
- **Then**: Công khai hiển thị đúng nội dung và thứ tự; bảng phụ phí có ba cột Hạng mục/Giá đề xuất/Ghi chú.
- **And**: Giá đề xuất là chữ trình bày, không tạo phụ phí thanh toán tự động.

#### AC-005

- **Given**: Người quản lý mở Hành trình giám sát.
- **When**: Thêm bước, sửa tiêu đề/mô tả/icon và đổi thứ tự rồi lưu.
- **Then**: Trang công khai dùng nội dung, icon, số bước và thứ tự mới; không cố định bốn bước.
- **And**: Không tạo lịch kiểm tra hoặc chuyển trạng thái gói thật.

#### AC-006

- **Given**: Người quản lý mở Giá trị khách hàng nhận được.
- **When**: Thêm, sửa, xóa hoặc sắp xếp hàng và chọn icon rồi lưu.
- **Then**: Quản trị có dạng bảng; công khai hiển thị tên, icon và chữ riêng cho ba lựa chọn đã lưu.

#### AC-007

- **Given**: Người quản lý sửa các tiêu đề, chữ nút, icon hoặc ba ghi chú cuối trang.
- **When**: Lưu thành công.
- **Then**: Trang công khai hiển thị các thay đổi tương ứng; bố cục và cột Toàn diện màu cam tiếp tục như hiện tại.
- **And**: Nút mua vẫn chọn đúng gói có giá, giữ công trình đang chọn; lựa chọn Tự quản lý vẫn không có nút mua.

#### AC-008

- **Given**: Có bản CMS tiếng Việt đã lưu.
- **When**: Lưu hoặc khôi phục một phần tiếng Anh.
- **Then**: Nội dung tiếng Việt giữ nguyên; ngôn ngữ chưa biên tập dùng mặc định của chính ngôn ngữ đó.
- **And**: Danh sách đã lưu rỗng không tự thay bằng mặc định; chỉ thao tác khôi phục riêng mới đưa nội dung mặc định trở lại.

#### AC-009

- **Given**: Gửi dữ liệu sai cấu trúc, không có quyền hoặc dùng phiên bản cũ.
- **When**: Lưu bằng quản trị hoặc gọi trực tiếp API.
- **Then**: Backend từ chối phù hợp và giữ bản đã lưu trước đó.
- **And**: Icon lỗi dùng dự phòng, chữ được render an toàn; trên điện thoại vẫn cuộn và sửa được cả ba cột và các thao tác danh sách.

## References

### TDDs

- TDD-HB-003/Architecture: Tái sử dụng cơ chế CMS PageSection, lưu theo ngôn ngữ, kiểm phiên bản và khôi phục; phần giám sát sẽ bổ sung sau khi chốt nghiệp vụ.
- TDD-SUB-001/Architecture: Nguồn cấu hình gói giám sát hiện có; không thay đổi module quản lý gói.

### Rules

- BR-CMS-003

### Dependencies

- STORY-CMS-001: Hành vi bảng so sánh động của Gói tư vấn được dùng làm mẫu.
- STORY-CMS-002: Hành vi bảng giá trị khách hàng và chọn icon được dùng làm mẫu.

## Non-Functional

- Áp dụng quyền `news.manage`, kiểm phiên bản, giới hạn chữ và cập nhật cache theo CMS hiện có.
- Ô nhập, chọn icon và các nút thêm, xóa, sắp xếp dùng được bằng bàn phím; bảng cuộn ngang khi màn hình nhỏ.
- Không đưa toàn bộ thư viện component icon vào bundle client chỉ để chọn icon.

## Out of Scope

- Phần tiêu đề đầu trang, thẻ giá và giới thiệu gói nằm ngoài sáu phần trong ảnh.
- Thay giá bán, cấu hình quyền, hạn mức hoặc vòng đời gói đã mua; quản lý lịch, giữ/trừ lượt kiểm tra, phân công kỹ sư và thanh toán phụ phí thật.
- Tải lên icon tùy chỉnh; thiết kế lại trang công khai hoặc thêm luồng mua mới.
- Metadata Creator, Assignee và Sprint chưa được cung cấp; bản nháp chưa đủ metadata để nhập hoặc phê duyệt như tài liệu hoàn chỉnh.

<!-- Người dùng chốt cả US và BR ngày 09/10/2026; xác nhận Reviewer và Approver đều là Tân Trần. Không tự xác nhận phê duyệt trên hệ thống. -->
