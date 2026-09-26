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

# STORY-PROJ-001

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo dự toán và được tự lưu thông tin đang nhập để có thể quay lại hoàn thiện đầu vào thiết kế.
- **Context**: Nhóm tính năng có tên Tạo dự toán; đối tượng được tạo và lưu gọi là bản dự toán. Mã STORY-PROJ-*** được giữ để bảo toàn tham chiếu. Đây là đổi tên nghiệp vụ, không đổi phạm vi thiết kế, dự toán và hồ sơ thi công đã chốt. Bản nháp cho phần tạo và lưu đầu vào trong tính năng gồm ba bước nhập liệu, nhận dự toán và hồ sơ thi công. Backend hiện chỉ có tài khoản/xác thực. AI service do bên khác phụ trách, chờ tích hợp. Người dùng đã chốt bộ US/BR trong hội thoại. Hợp đồng AI, thiết kế kỹ thuật và metadata phân công còn thiếu; xác nhận nghiệp vụ không có nghĩa đã triển khai hoặc phê duyệt trên hệ thống quản lý tài liệu.
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

- Khách đã đăng nhập bằng tài khoản khách hàng; tài khoản nhân viên không được tạo dự toán theo BR-RBAC-005.
- Khách có gói thiết kế còn hiệu lực, quyền tạo thiết kế và còn lượt sẵn dùng hoặc không giới hạn lượt theo BR-SUB-007.
- Khi mở lại và lưu một bản dự toán, khách có quyền truy cập dữ liệu bản dự toán đó.

### Trigger

Khách chọn Tạo dự toán mới hoặc mở lại bản dự toán đang chuẩn bị thông tin đầu vào.

## Flow

### Main Flow

1. Khách nhập tên bắt buộc, tối đa 200 ký tự sau khi bỏ khoảng trắng đầu/cuối, rồi thực hiện thao tác tạo dự toán theo BR-PROJ-003.
2. Backend kiểm tra điều kiện tạo theo BR-RBAC-005 và BR-SUB-007, sau đó tạo dự toán để khách nhập tiếp. Chưa yêu cầu đủ thông tin dùng để gửi AI.
3. Khách chọn loại công trình từ danh mục do Admin quản lý và nhập trường Diện tích chung, đơn vị m², theo BR-PROJ-001.
4. Khách cung cấp tối đa một ảnh JPG/PNG/HEIC không vượt 10 MB, hoặc Mô tả chi tiết không vượt 500 ký tự, hoặc cả hai, theo BR-PROJ-002. Mô tả chi tiết dùng để gửi AI khi khách cung cấp; không có trường ghi chú riêng.
5. Khách nhập địa chỉ và chọn gói hoàn thiện/nội thất. Thông tin phong cách kiến trúc và phong cách nội thất được tách riêng; số tầng và tum áp dụng theo cấu hình loại công trình tại BR-PROJ-004. Chỉ cho chọn những nhóm phong cách được bật theo loại; AI vẫn trả đủ kết quả dù một nhóm lựa chọn bị tắt. Trước khi gửi AI, mỗi nhóm được bật phải có đúng một phong cách hợp lệ; bản nháp vẫn được phép chưa chọn đủ.
6. Các thay đổi được tự lưu theo BR-PROJ-003. Backend kiểm tra lại quyền truy cập, điều kiện lưu và khóa đầu vào theo BR-PROJ-005 tại từng yêu cầu; bản nháp được phép chưa đủ dữ liệu.
7. Khi khách mở lại bản dự toán, hệ thống trả dữ liệu đã lưu thành công và cấu hình đã áp dụng cho bản dự toán để khách tiếp tục nhập. Thay đổi danh mục/cấu hình của Admin không làm mất lựa chọn hoặc thay điều kiện đầu vào của bản dự toán cũ theo BR-PROJ-004.
8. Luồng này kết thúc ở thông tin đầu vào đã lưu. Gửi AI, tiếp nhận kết quả và cung cấp hồ sơ thuộc các phần tiếp theo của tính năng, chưa có hợp đồng tích hợp.

### Alternative Flow

#### ALT-01

Khách rời biểu mẫu trước khi nhập đủ dữ liệu.

1. Giữ thông tin đã được lưu thành công trong bản dự toán.
2. Khi khách quay lại, trả thông tin đó để nhập tiếp; không tự khởi chạy AI vì đã có dữ liệu lưu.

#### ALT-02

Khách chỉ cung cấp ảnh hoặc chỉ cung cấp mô tả.

1. Lưu dữ liệu được cung cấp khi yêu cầu lưu đáp ứng các điều kiện áp dụng.
2. Không yêu cầu bổ sung loại còn lại chỉ để đáp ứng điều kiện có ít nhất ảnh hoặc mô tả.

#### ALT-03

Khách chọn loại công trình được cấu hình không chọn số tầng và tum, như Căn hộ trên trang mẫu.

1. Dùng trường Diện tích chung, không thêm trường diện tích đất.
2. Không yêu cầu số tầng hoặc tum khi cấu hình loại không có các trường này. Hai phần lựa chọn phong cách áp dụng BR-PROJ-004; cấu hình lựa chọn không thu hẹp bộ kết quả AI.

#### ALT-04

Khách đổi loại công trình trong bản nháp khi còn đủ điều kiện lưu.

1. Khách chọn loại khác trong danh mục đã áp dụng khi tạo bản dự toán. Không cho chọn loại được Admin thêm sau đó; muốn dùng loại mới thêm phải tạo bản dự toán mới. Đối chiếu phong cách của từng nhóm, số tầng và tum với cấu hình loại được chọn áp dụng cho bản dự toán.
2. Giữ lựa chọn còn hợp lệ; xóa lựa chọn không phù hợp hoặc thuộc trường không áp dụng. Không chuyển phong cách kiến trúc sang nội thất hoặc ngược lại.
3. Giữ nguyên diện tích, địa chỉ, ảnh và mô tả. Tự lưu bản nháp theo điều kiện hiện hành; khách cần chọn lại phần bắt buộc còn thiếu trước khi gửi AI.

#### ALT-05

Khách đổi tỉnh/thành trong bản nháp khi còn đủ điều kiện lưu.

1. Xóa xã/phường đã chọn và cung cấp lựa chọn thuộc tỉnh/thành mới.
2. Giữ địa chỉ chi tiết để khách tự sửa; tự lưu theo điều kiện hiện hành.
3. Yêu cầu chọn lại xã/phường hợp lệ trước khi gửi AI; chưa chọn lại vẫn được lưu như bản nháp chưa đầy đủ.

### Exception Flow

#### EXC-01

Khách không đáp ứng điều kiện tạo hoặc lưu bản dự toán, kể cả khi điều kiện thay đổi sau khi mở biểu mẫu.

1. Từ chối yêu cầu theo BR-RBAC-005 hoặc BR-SUB-007; không tạo bản ghi hoặc ghi thay đổi từ yêu cầu bị từ chối.
2. Giữ nguyên dữ liệu đã lưu trước đó; không giữ/trừ lượt vì thao tác tạo/lưu.
3. Không tự lưu lại yêu cầu bị từ chối khi lượt hoặc quyền được khôi phục; xử lý yêu cầu tiếp theo theo điều kiện tại thời điểm tiếp nhận.
4. Riêng yêu cầu chỉ đổi tên bản dự toán đã có không phụ thuộc gói, quyền tạo thiết kế hay lượt, theo BR-SUB-007 khoản 11; vẫn kiểm tra quyền sở hữu và tên hợp lệ.

#### EXC-02

Khách yêu cầu mở hoặc lưu một bản dự toán mà tài khoản không có quyền truy cập.

1. Không trả dữ liệu của bản dự toán và không ghi thay đổi vào bản dự toán đó.

#### EXC-03

Một lần tự lưu không thành công.

1. Không xác nhận đã lưu cho phần thay đổi chưa được backend lưu thành công.
2. Nếu lỗi do mất mạng hoặc máy chủ, giữ nội dung đang nhập khi trang còn mở, báo chưa lưu và có nút Thử lại; tự thử lưu lại khi kết nối phục hồi theo BR-PROJ-003.
3. Mỗi yêu cầu thử lại kiểm tra lại quyền/gói/lượt, dữ liệu và trạng thái khóa. Không tự thử lại yêu cầu bị từ chối do không đủ quyền/gói/lượt; không tự gửi lại AI.
4. Sau khi đóng hoặc tải lại trang, chỉ trả dữ liệu đã lưu thành công; không hỗ trợ khôi phục phần chưa lưu.

#### EXC-04

Khách thay ảnh nhưng tải lên hoặc lưu ảnh mới thất bại.

1. Giữ ảnh cũ đang gắn với bản dự toán, báo thay ảnh chưa thành công.
2. Chỉ thay ảnh cũ sau khi ảnh mới hợp lệ, tải lên và lưu thay thế thành công; bản dự toán vẫn chỉ có một ảnh đầu vào.
3. Nếu chưa có ảnh cũ, bản dự toán vẫn chưa có ảnh hợp lệ; điều kiện có ít nhất ảnh hoặc mô tả vẫn áp dụng trước khi gửi AI.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đáp ứng điều kiện tạo dự toán và đã nhập tên.
- **When**: Khách thực hiện thao tác tạo.
- **Then**: Bản dự toán được tạo để nhập thông tin chi tiết dù chưa có diện tích, ảnh hoặc mô tả thiết kế.
- **And**: Chưa khởi chạy AI và không giữ/trừ lượt vì tạo dự toán.

#### AC-002

- **Given**: Khách có bản dự toán đang nhập và đáp ứng điều kiện lưu.
- **When**: Khách thay đổi thông tin rồi lần tự lưu thành công.
- **Then**: Mở lại bản dự toán trả dữ liệu đã lưu mà không cần bấm Lưu nháp.
- **And**: Bản nháp có thể chưa đủ dữ liệu để gửi AI.

#### AC-003

- **Given**: Khách nhập thông tin cho một loại công trình trong danh mục.
- **When**: Hệ thống nhận và trả dữ liệu diện tích.
- **Then**: Dùng một trường Diện tích, đơn vị m², không giới hạn tính năng ở năm loại ban đầu.
- **And**: Không yêu cầu riêng diện tích đất/diện tích xây dựng hoặc tự thay giá trị bằng diện tích nhân số tầng.

#### AC-004

- **Given**: Khách cung cấp chỉ ảnh, chỉ mô tả hoặc cả hai.
- **When**: Hệ thống kiểm tra điều kiện có ảnh/mô tả.
- **Then**: Cả ba trường hợp đều đáp ứng điều kiện về sự hiện diện của dữ liệu này.
- **And**: Không có cả hai được lưu như thông tin chưa hoàn tất, nhưng chưa đủ để gửi AI.

#### AC-005

- **Given**: Khách chọn một loại công trình.
- **When**: Hệ thống cung cấp các lựa chọn đầu vào tương ứng.
- **Then**: Địa chỉ, gói hoàn thiện và các lựa chọn theo loại khớp BR-PROJ-004; phong cách kiến trúc và nội thất là hai phần riêng.
- **And**: Số tầng và tum phụ thuộc cấu hình loại; gói hoàn thiện không được dùng thay quyền của gói đăng ký.

#### AC-006

- **Given**: Khách không còn đủ điều kiện tạo/lưu do hết hạn gói, thiếu quyền hoặc không còn lượt sẵn dùng theo BR-SUB-007.
- **When**: Có yêu cầu tạo hoặc tự lưu, kể cả từ biểu mẫu đã mở trước đó.
- **Then**: Từ chối yêu cầu, không tạo dự toán hoặc ghi thay đổi và giữ nguyên dữ liệu đã lưu.
- **And**: Không giữ/trừ thêm lượt hoặc tự cấp quyền/lượt. Yêu cầu chỉ đổi tên một bản dự toán đã có vẫn được chấp nhận theo BR-SUB-007 khoản 11.

#### AC-007

- **Given**: Tài khoản nhân viên hoặc tài khoản không có quyền truy cập bản dự toán gửi yêu cầu.
- **When**: Nhân viên tạo dự toán hoặc tài khoản yêu cầu mở/lưu bản dự toán không được phép.
- **Then**: Thao tác tương ứng bị từ chối.
- **And**: Không làm lộ dữ liệu hoặc ghi thay đổi trái quyền.

#### AC-008

- **Given**: Backend chưa lưu thành công một phần thay đổi đầu vào.
- **When**: Trả kết quả cho lần lưu đó hoặc khách mở lại bản dự toán.
- **Then**: Không báo phần thay đổi đó đã được lưu thành công.
- **And**: Khi trang còn mở, giữ nội dung và báo chưa lưu; lỗi mạng/máy chủ có nút Thử lại và tự thử lưu khi kết nối phục hồi. Sau khi đóng hoặc tải lại trang, chỉ trả dữ liệu đã lưu; không khôi phục phần chưa lưu.

#### AC-009

- **Given**: Khách cung cấp ảnh hoặc mô tả thiết kế cho bản dự toán.
- **When**: Hệ thống kiểm tra dữ liệu được cung cấp.
- **Then**: Chỉ tối đa một ảnh JPG/PNG/HEIC không vượt 10 MB và mô tả không vượt 500 ký tự mới đáp ứng các giới hạn tương ứng.
- **And**: Có cả ảnh và mô tả vẫn phải tuân thủ từng giới hạn; không bắt buộc bổ sung loại dữ liệu còn lại nếu đã có một loại hợp lệ. Ghi chú kỹ thuật, không đổi tiêu chí: định dạng và dung lượng ảnh do frontend kiểm trước khi tải ảnh lên qua presign; với ảnh, backend chỉ kiểm URL là URL https thuộc tên miền được phép trong `UploadedFileOption__AllowedHosts` (TDD-PROJ-001).

#### AC-010

- **Given**: Khách nhập giá trị Diện tích.
- **When**: Hệ thống kiểm tra giá trị này.
- **Then**: Chỉ chấp nhận số lớn hơn 0 và có tối đa hai chữ số thập phân; không tự làm tròn giá trị vượt độ chính xác để chấp nhận.
- **And**: Chưa đặt trần diện tích theo nghiệp vụ; thiếu diện tích được lưu trong bản nháp nhưng không đủ điều kiện gửi AI.

#### AC-011

- **Given**: Bản dự toán đang được AI xử lý sau khi tiếp nhận yêu cầu hợp lệ.
- **When**: Có yêu cầu tự lưu hoặc cập nhật trực tiếp đầu vào.
- **Then**: Từ chối thay đổi theo BR-PROJ-005, giữ đầu vào của tác vụ đang chạy.
- **And**: Sau thất bại chỉ được sửa khi đáp ứng lại điều kiện lưu; bản dự toán đã thành công muốn phương án khác phải tạo mới.

#### AC-012

- **Given**: Loại công trình bật lựa chọn phong cách kiến trúc, nội thất hoặc cả hai theo BR-PROJ-004.
- **When**: Kiểm tra đầu vào trước khi gửi AI.
- **Then**: Mỗi nhóm được bật phải có đúng một phong cách thuộc nhóm và loại công trình đang áp dụng; thiếu lựa chọn, chọn nhiều hoặc chọn không phù hợp thì chưa đủ điều kiện gửi AI.
- **And**: Nhóm bị tắt không yêu cầu lựa chọn; bản nháp được lưu khi chưa chọn đủ. Việc tắt nhóm lựa chọn không làm AI trả thiếu phần kết quả tương ứng.

#### AC-013

- **Given**: Bản dự toán đã được tạo trước khi Admin sửa cấu hình loại công trình hoặc phong cách.
- **When**: Khách mở lại, lưu tiếp hoặc kiểm tra đầu vào để gửi/thử lại AI.
- **Then**: Áp dụng cấu hình của bản dự toán cũ; thay đổi của Admin không làm mất lựa chọn hoặc buộc khách đáp ứng cấu hình mới, kể cả khi bản dự toán còn là bản nháp.
- **And**: Bản dự toán mới dùng cấu hình mới. Quyền, gói/lượt và khóa sửa vẫn được kiểm tra theo quy tắc áp dụng, không dùng quyền/lượt cũ thay kiểm tra hiện tại.

#### AC-014

- **Given**: Bản dự toán ở trạng thái được sửa, có các lựa chọn phong cách, tầng/tum và khách đáp ứng điều kiện lưu.
- **When**: Khách đổi loại công trình.
- **Then**: Giữ lựa chọn còn hợp lệ theo cấu hình loại mới áp dụng cho bản dự toán; xóa lựa chọn không phù hợp hoặc thuộc trường không áp dụng và yêu cầu chọn lại phần bắt buộc trước khi gửi AI.
- **And**: Giữ nguyên diện tích, địa chỉ, ảnh và mô tả; không chuyển lựa chọn giữa nhóm phong cách kiến trúc và nội thất. Bản nháp được phép chưa chọn đủ.

#### AC-015

- **Given**: Bản nháp có tỉnh/thành, xã/phường và địa chỉ chi tiết, khách đáp ứng điều kiện lưu.
- **When**: Khách đổi tỉnh/thành.
- **Then**: Xóa xã/phường đã chọn, yêu cầu chọn lại xã/phường thuộc tỉnh/thành mới trước khi gửi AI.
- **And**: Giữ địa chỉ chi tiết để khách tự sửa; cho lưu bản nháp khi chưa chọn lại xã/phường.

#### AC-016

- **Given**: Bản dự toán đã có ảnh đầu vào; khách có quyền sửa và chọn ảnh thay thế.
- **When**: Ảnh mới đang tải hoặc tải/lưu thay thế thất bại.
- **Then**: Ảnh cũ vẫn là ảnh đầu vào đã lưu; không báo thay ảnh thành công.
- **And**: Chỉ sau khi ảnh mới hợp lệ, tải và lưu thay thế thành công thì ảnh mới trở thành ảnh đầu vào duy nhất của bản dự toán.

#### AC-017

- **Given**: Một lần tự lưu thất bại do mạng hoặc máy chủ, trang còn mở và giữ nội dung chưa lưu.
- **When**: Kết nối phục hồi hoặc khách bấm Thử lại.
- **Then**: Gửi lại yêu cầu lưu, kiểm tra lại quyền/gói/lượt, dữ liệu và khóa đầu vào; chỉ báo đã lưu khi backend lưu thành công.
- **And**: Không tự thử lại yêu cầu bị từ chối do quyền/gói/lượt, không vượt qua khóa sửa và không khởi chạy lại AI.

#### AC-018

- **Given**: Bản nháp được tạo trước khi Admin thêm một loại công trình vào danh mục.
- **When**: Khách xem danh sách loại có thể chọn hoặc yêu cầu đổi bản nháp sang loại mới thêm, kể cả gửi yêu cầu trực tiếp.
- **Then**: Chỉ cho chọn các loại thuộc danh mục đã áp dụng khi tạo bản dự toán; từ chối chuyển sang loại mới thêm và giữ dữ liệu đã lưu.
- **And**: Muốn dùng loại mới thêm, khách phải tạo bản dự toán mới và đáp ứng điều kiện tạo hiện hành.

#### AC-019

- **Given**: Khách tạo bản dự toán hoặc gửi yêu cầu lưu tên khi được phép sửa.
- **When**: Backend kiểm tra tên sau khi bỏ khoảng trắng đầu/cuối.
- **Then**: Chỉ chấp nhận tên có nội dung và không vượt 200 ký tự; lưu tên đã bỏ khoảng trắng đầu/cuối. Tên rỗng, chỉ có khoảng trắng hoặc quá dài bị từ chối, không tự cắt ngắn.
- **And**: Yêu cầu tạo bị từ chối không tạo bản dự toán; yêu cầu sửa bị từ chối giữ tên đã lưu trước đó. Yêu cầu tạo vẫn cần điều kiện quyền/gói/lượt; đổi tên bản dự toán đã có chỉ cần quyền sở hữu và tên hợp lệ, không bị chặn bởi hết hạn gói, hết lượt hay khóa sửa khi AI đang xử lý theo BR-SUB-007 khoản 11.

#### AC-020

- **Given**: Khách tạo bản dự toán và nhập thông tin thiết kế.
- **When**: Cung cấp nội dung mô tả để gửi AI.
- **Then**: Chỉ có một trường Mô tả chi tiết tối đa 500 ký tự; nội dung được dùng làm đầu vào AI khi khách cung cấp, không thêm trường ghi chú riêng.
- **And**: Trước khi gửi AI vẫn chỉ yêu cầu có ít nhất ảnh hoặc mô tả hợp lệ; chỉ có ảnh thì không bắt buộc nhập mô tả. Bản nháp được phép chưa có cả hai.

## References

### TDDs

- TDD-PROJ-001
- TDD-PROJ-002

### Rules

- BR-PROJ-001
- BR-PROJ-002
- BR-PROJ-003
- BR-PROJ-004
- BR-PROJ-005
- BR-SUB-007
- BR-RBAC-005

### Dependencies

## Non-Functional

- Kiểm tra quyền truy cập và điều kiện tạo/lưu ở backend, kể cả yêu cầu gửi trực tiếp; không chỉ khóa biểu mẫu.
- [Chưa chốt yêu cầu định lượng về độ trễ tự lưu, kích thước dữ liệu và xử lý cập nhật đồng thời.]

## Out of Scope

- Thao tác Admin quản lý loại công trình và cấu hình thuộc STORY-PROJ-005; vẫn nằm trong phạm vi tính năng mở rộng.

- Gửi tác vụ AI, tính lượt khi AI chạy, tạo dự toán và nhận kết quả: thuộc phần tiếp theo của tính năng, không bị loại khỏi phạm vi tổng thể.
- Xuất hoặc chia sẻ hồ sơ: thuộc phần hồ sơ thi công trong phạm vi tổng thể, chờ làm rõ định dạng kết quả từ AI service và hành vi liên quan.
- Xây AI service, bảng đơn giá, công thức tính dự toán hoặc tự tạo kết quả chuyên môn tại backend BMT.
- Triển khai frontend, thay đổi mã ứng dụng, tạo/chạy migration hoặc triển khai dịch vụ trong tác vụ lập kế hoạch này.
