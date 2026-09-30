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

# STORY-PROJ-006

## Metadata

- **Story**: Là khách hàng, tôi muốn xem, tìm kiếm và lọc các dự toán của mình để tìm lại bản đang làm hoặc mở kết quả đã tạo.
- **Context**: Bổ sung màn hình Dự toán của tôi cho nhóm PROJ. Người dùng đã xác nhận hiển thị đủ trạng thái, thông tin dạng danh sách không có ảnh đại diện, phân trang, tìm theo tên, lọc trạng thái và sắp xếp theo lần sửa gần nhất. Người dùng đã chốt bộ US/BR trong hội thoại ngày 30/09/2026; đã bổ sung quyết định tìm không phân biệt hoa/thường và dấu. Thiết kế ở TDD-PROJ-004, chưa triển khai API danh sách. Metadata phân công còn thiếu.
- **Sprint**: [Chưa xác định]
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

- Khách đã đăng nhập bằng tài khoản khách hàng theo BR-RBAC-005.
- Không yêu cầu khách có gói thiết kế còn hiệu lực hoặc còn lượt chỉ để xem danh sách theo BR-PROJ-008 và quyền xem dữ liệu cũ tại BR-SUB-007.

### Trigger

Khách mở Dự toán của tôi, đổi trang, tìm theo tên hoặc lọc trạng thái.

## Flow

### Main Flow

1. Khách mở Dự toán của tôi.
2. Hệ thống xác định tài khoản đang đăng nhập và chỉ lấy các bản dự toán chưa bị xóa của tài khoản đó theo BR-PROJ-008.
3. Hệ thống hiển thị danh sách có phân trang, mặc định gồm đủ trạng thái nháp, đang xử lý, thành công và thất bại; bản sửa gần nhất đứng trước.
4. Mỗi bản hiển thị tên, loại công trình nếu đã chọn, trạng thái, ngày tạo và lần sửa gần nhất. Không hiển thị ảnh đại diện hoặc bổ sung cột địa chỉ.
5. Khách có thể tìm một phần tên không phân biệt hoa/thường hoặc dấu và lọc theo trạng thái. Danh sách trả về đồng thời đáp ứng các điều kiện đang chọn, vẫn giữ phạm vi tài khoản và thứ tự mặc định.
6. Khách chọn một bản để mở thông tin hoặc kết quả hiện có. Hệ thống kiểm lại quyền truy cập và tình trạng đã xóa; việc mở không tự gửi AI hoặc cấp quyền sửa đầu vào.
7. Khi khách muốn xóa một hoặc nhiều bản, chuyển sang STORY-PROJ-007.

### Alternative Flow

#### ALT-01

Khách chưa có dự toán hoặc không có bản phù hợp điều kiện tìm kiếm, bộ lọc.

1. Hiển thị danh sách rỗng đúng tình trạng thực tế.
2. Không dùng dự toán của khách khác hoặc dữ liệu mẫu để lấp danh sách.

#### ALT-02

Khách hết hạn gói hoặc hết lượt nhưng vẫn có dự toán chưa bị xóa.

1. Vẫn cho xem, tìm kiếm, lọc và mở bản thuộc sở hữu của khách.
2. Quyền sửa đầu vào và gửi AI được kiểm tra riêng theo các quy tắc hiện hành; việc xem danh sách không mở các quyền này.

#### ALT-03

Bản nháp chưa được chọn loại công trình.

1. Vẫn hiển thị bản nháp trong danh sách khi phù hợp bộ lọc.
2. Thể hiện loại công trình là chưa chọn, không tự điền một loại hoặc bỏ bản nháp khỏi danh sách.

### Exception Flow

#### EXC-01

Phiên đăng nhập không hợp lệ hoặc tài khoản không thuộc nhóm khách hàng.

1. Từ chối yêu cầu theo điều kiện xác thực và loại tài khoản; không trả dữ liệu dự toán.

#### EXC-02

Bản đã bị xóa ở phiên khác sau khi danh sách được tải, hoặc yêu cầu mở nhắm tới bản không thuộc khách.

1. Không trả dữ liệu của bản đó và báo không thể mở.
2. Khi tải lại danh sách, bản đã xóa không còn xuất hiện; không làm lộ thông tin dự toán của khách khác.

#### EXC-03

Không tải được danh sách do lỗi mạng hoặc máy chủ.

1. Báo không tải được dữ liệu; không trình bày lỗi tải như kết luận khách chưa có dự toán.
2. Khách có thể tải lại danh sách với điều kiện tìm kiếm và bộ lọc đang dùng.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đã đăng nhập và có các bản dự toán ở trạng thái nháp, đang xử lý, thành công và thất bại.
- **When**: Mở danh sách mà không lọc trạng thái.
- **Then**: Các bản chưa bị xóa ở cả bốn trạng thái đều thuộc tập kết quả có phân trang.
- **And**: Không trả dự toán của tài khoản khác hoặc dự toán đã xóa theo BR-PROJ-008.

#### AC-002

- **Given**: Một trang có cả bản đã chọn loại công trình và bản nháp chưa chọn loại.
- **When**: Hiển thị danh sách.
- **Then**: Mỗi bản có tên, loại công trình nếu đã chọn, trạng thái, ngày tạo và lần sửa gần nhất; bản chưa chọn loại vẫn xuất hiện và thể hiện đúng tình trạng chưa chọn.
- **And**: Không hiển thị ảnh đại diện hoặc cột địa chỉ; không lấy loại công trình do khách chưa chọn làm dữ liệu thật.

#### AC-003

- **Given**: Các bản trong tập kết quả có thời điểm sửa khác nhau.
- **When**: Mở danh sách hoặc chuyển trang.
- **Then**: Bản sửa gần nhất đứng trước trên toàn bộ tập kết quả, không chỉ sắp xếp riêng từng trang.
- **And**: Phân trang áp dụng sau khi giới hạn theo chủ sở hữu, tình trạng chưa xóa và các điều kiện tìm kiếm, lọc.

#### AC-004

- **Given**: Khách nhập tên cần tìm và chọn trạng thái cần lọc.
- **When**: Hệ thống trả kết quả.
- **Then**: Tìm tên không phân biệt hoa/thường hoặc dấu, ví dụ “nha” tìm được “Nhà”. Mỗi bản trả về đáp ứng đồng thời điều kiện tìm theo tên và trạng thái.
- **And**: Vẫn chỉ trả các bản chưa bị xóa của khách; giữ phân trang và thứ tự sửa gần nhất.

#### AC-005

- **Given**: Khách không có bản dự toán hoặc không có bản phù hợp các điều kiện đang chọn.
- **When**: Tải danh sách thành công.
- **Then**: Hiển thị danh sách rỗng.
- **And**: Không báo lỗi hệ thống hoặc thêm dữ liệu mẫu để thay kết quả rỗng.

#### AC-006

- **Given**: Khách đã hết hạn gói hoặc hết lượt và có dự toán chưa bị xóa thuộc sở hữu của mình.
- **When**: Xem danh sách, tìm kiếm, lọc hoặc mở một bản.
- **Then**: Cho phép thao tác đọc theo BR-PROJ-008 và BR-SUB-007.
- **And**: Không giữ, trừ hoặc hoàn lượt; không tự cấp quyền sửa đầu vào hay gửi AI.

#### AC-007

- **Given**: Khách chọn một dự toán chưa bị xóa thuộc sở hữu của mình từ danh sách.
- **When**: Mở dự toán.
- **Then**: Hiển thị thông tin hoặc kết quả đã lưu của đúng bản, với trạng thái và quyền thao tác được kiểm tra lại.
- **And**: Không tự gửi yêu cầu tạo hoặc thử lại AI chỉ vì mở bản dự toán.

#### AC-008

- **Given**: Dự toán vừa bị xóa thành công, kể cả xóa từ phiên khác.
- **When**: Khách tải lại danh sách hoặc yêu cầu mở bản qua đường dẫn đã lưu.
- **Then**: Bản không còn trong danh sách và không mở lại được theo BR-PROJ-009.
- **And**: Không có chức năng khôi phục.

#### AC-009

- **Given**: Người gọi chưa đăng nhập, phiên không hợp lệ hoặc dùng tài khoản nhân viên.
- **When**: Yêu cầu danh sách Dự toán của tôi.
- **Then**: Yêu cầu bị từ chối theo điều kiện xác thực hoặc loại tài khoản.
- **And**: Không trả dữ liệu dự toán của khách hàng.

#### AC-010

- **Given**: Yêu cầu tải danh sách gặp lỗi mạng hoặc máy chủ.
- **When**: Giao diện chưa nhận được kết quả thành công.
- **Then**: Thông báo chưa tải được danh sách và cho tải lại.
- **And**: Không kết luận khách chưa có dự toán từ một lần tải bị lỗi.

## References

### TDDs

- TDD-PROJ-004: Thiết kế danh sách, tìm không dấu, trạng thái và phân trang; đã được người dùng chốt trong hội thoại ngày 30/09/2026, chưa triển khai.

### Rules

- BR-PROJ-008
- BR-PROJ-009
- BR-SUB-007
- BR-RBAC-005

### Dependencies

- STORY-PROJ-001: Mở thông tin đầu vào đã lưu và kiểm tra quyền sửa.
- STORY-PROJ-003: Mở kết quả và hồ sơ đã có.
- STORY-PROJ-007: Xóa các bản được chọn từ danh sách.

## Non-Functional

- Kiểm tra tài khoản và quyền sở hữu ở backend; không tin mã chủ sở hữu do người gọi tự cung cấp.
- Không đưa dữ liệu của bản đã xóa hoặc tài khoản khác vào danh sách, kết quả tìm kiếm hoặc thông tin phân trang.
- Tìm tên không phân biệt hoa/thường và dấu theo BR-PROJ-008. Kích thước trang, thứ tự phụ khi trùng thời điểm sửa và hợp đồng API theo TDD-PROJ-004; chưa đặt ngưỡng hiệu năng không có căn cứ.

## Out of Scope

- Danh sách dự toán của khách khác dành cho Admin hoặc nhân viên.
- Thay đổi điều kiện sửa đầu vào, tạo thiết kế hoặc tính lượt hiện có.
- Ảnh đại diện, cột địa chỉ và bộ lọc nâng cao ngoài tên, trạng thái.
- Thùng rác, xem lại hoặc khôi phục dự toán đã xóa.
