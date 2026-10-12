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

# STORY-PROJ-003

## Metadata

- **Story**: Là chủ sở hữu bản dự toán, tôi muốn xem kết quả của từng chặng đã xong và tải bốn tệp hồ sơ để sử dụng kết quả thiết kế đã tạo.
- **Context**: Bao phủ việc xem kết quả của bốn chặng Bản vẽ 2D, Dự toán, Phối cảnh 3D, Hồ sơ (BR-PROJ-010) và tải bốn tệp do chặng Hồ sơ dựng (STORY-PROJ-010). Giao diện có thanh tiến trình 5 bước: Nhập liệu → Bản vẽ 2D → Dự toán → Phối cảnh 3D → Hồ sơ; mỗi chặng có màn hình kết quả riêng. Kết quả do backend tạo và lưu theo BR-PROJ-007; xem và tải không gọi AI. Cập nhật theo quyết định của người dùng ngày 10–11/10/2026.
- **Sprint**: [Chưa xác định]
- **Priority**: Must
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - Frontend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Khách đã đăng nhập và là chủ sở hữu bản dự toán.
- Bản dự toán có ít nhất một chặng đã thành công để xem kết quả; tải tệp cần chặng Hồ sơ đã thành công.

### Trigger

Khách mở một bước trên thanh tiến trình của bản dự toán, hoặc yêu cầu tải một tệp hồ sơ.

## Flow

### Main Flow

1. Khách mở bản dự toán; hệ thống hiển thị thanh tiến trình 5 bước cùng trạng thái từng chặng theo BR-PROJ-010.
2. Khách chọn một chặng đã thành công. Backend kiểm tra quyền truy cập và trả kết quả đã lưu:
   - Bản vẽ 2D: phương án từng tầng (tên phòng, diện tích), bản vẽ kỹ thuật và bản vẽ bố trí nội thất của từng tầng.
   - Dự toán: tổng dự toán, đơn giá trên m² sàn quy đổi, ba phần thô, hoàn thiện, nội thất với các dòng chi tiết, thời gian thi công dự kiến và tư vấn; chỉ phần gửi khách theo BR-PROJ-017.
   - Phối cảnh 3D: các ảnh ngoại thất và ảnh phòng kèm nhãn.
   - Hồ sơ: danh sách bốn tệp gồm PDF hồ sơ gộp, PDF phối cảnh 3D, PDF bản vẽ kiến trúc và Excel dự toán.
3. Khi khách yêu cầu tải một tệp, backend kiểm tra quyền và chuyển nội dung tệp của đúng bản dự toán; tên tệp theo tên hiện tại của bản dự toán.
4. Việc xem và tải không chạy lại chặng nào, không gọi AI và không giữ hay trừ lượt.

### Alternative Flow

#### ALT-01

Gói thiết kế đã hết hạn nhưng bản dự toán có kết quả cũ.

1. Vẫn cho chủ sở hữu xem kết quả các chặng đã xong và tải tệp theo BR-SUB-007.
2. Không cấp kỳ hoặc lượt mới chỉ vì dùng các thao tác này.

#### ALT-02

Khách dừng sau một chặng, chẳng hạn đã xong 2D và Dự toán nhưng chưa chạy 3D.

1. Hiển thị kết quả của 2D và Dự toán; chặng 3D ở trạng thái Sẵn sàng, chặng Hồ sơ ở trạng thái Chưa mở.
2. Không có tệp để tải và chưa mở chia sẻ cho tới khi chặng Hồ sơ thành công.

### Exception Flow

#### EXC-01

Khách không có quyền truy cập bản dự toán được yêu cầu.

1. Từ chối trả nội dung kết quả hoặc tệp của bản dự toán đó.
2. Luồng người nhận link công khai được kiểm tra riêng theo STORY-PROJ-004, không bỏ qua kiểm tra quyền của luồng chủ sở hữu.

#### EXC-02

Chặng được yêu cầu chưa thành công (chưa mở, đang chạy hoặc lỗi).

1. Trả trạng thái thực tế của chặng; không trả kết quả dở dang hay dữ liệu minh họa như kết quả hoàn chỉnh.
2. Với chặng 3D đang chạy, khách xem được các ảnh đã dựng xong theo STORY-PROJ-009.

#### EXC-03

Tải tệp thất bại sau khi chặng Hồ sơ đã thành công.

1. Báo tải chưa thành công, giữ nguyên tệp và kết quả đã lưu.
2. Không trừ lượt, không chạy lại chặng Hồ sơ; khách tải lại được.

#### EXC-04

Khách yêu cầu tải tệp khi chặng Hồ sơ chưa thành công.

1. Từ chối và báo cần hoàn tất chặng Hồ sơ trước.

## Acceptance Criteria

#### AC-001

- **Given**: Chủ sở hữu có bản dự toán với chặng 2D và Dự toán đã thành công.
- **When**: Mở bước Bản vẽ 2D rồi bước Dự toán.
- **Then**: Nhận đúng kết quả đã lưu của bản dự toán đó cho từng chặng theo BR-PROJ-007.
- **And**: Backend không tính lại số tiền, không gọi AI và không dùng dữ liệu mẫu thay cho kết quả thật.

#### AC-002

- **Given**: Chặng Hồ sơ đã thành công.
- **When**: Chủ sở hữu tải từng tệp trong bốn tệp hồ sơ.
- **Then**: Nhận đúng tệp của bản dự toán, cùng kết quả đang xem.
- **And**: Không chạy lại chặng nào, không giữ hay trừ lượt.

#### AC-003

- **Given**: Gói đã hết hạn; bản dự toán có kết quả cũ thuộc khách.
- **When**: Khách xem kết quả hoặc tải tệp.
- **Then**: Cho phép theo BR-SUB-007.
- **And**: Không tự gia hạn gói hay làm mới lượt.

#### AC-004

- **Given**: Khách không có quyền truy cập bản dự toán trong luồng chủ sở hữu.
- **When**: Yêu cầu kết quả hoặc tệp của bản dự toán đó.
- **Then**: Từ chối, không trả dữ liệu hoặc tệp.
- **And**: Quyền qua link chia sẻ chỉ được xét qua luồng có kiểm tra link còn hiệu lực.

#### AC-005

- **Given**: Chặng Hồ sơ đã thành công nhưng một lần tải tệp bị lỗi.
- **When**: Hệ thống phản hồi lần tải bị lỗi.
- **Then**: Không báo đã tải thành công khi chưa có căn cứ.
- **And**: Giữ tệp và kết quả, không tính thêm lượt; khách tải lại được.

#### AC-006

- **Given**: Bản dự toán “Phương án 3” đã xong chặng Hồ sơ và đang được chia sẻ qua link còn hiệu lực.
- **When**: Chủ sở hữu đổi tên thành “Phương án chốt”, rồi chủ sở hữu và người nhận link lần lượt mở hồ sơ và tải PDF.
- **Then**: Cả hai đều thấy tên “Phương án chốt” trên màn hình; tệp PDF tải về có tên tệp “Phương án chốt”, còn nội dung tệp giữ nguyên như lúc dựng ở chặng Hồ sơ theo BR-PROJ-007 khoản 7.
- **And**: Việc tải lại không tính lượt, không gọi AI và không dựng lại tệp.

#### AC-007

- **Given**: Bản dự toán có mã `BUILDX-20261005-Q7K2M9` và đã có ít nhất một chặng thành công.
- **When**: Chủ sở hữu mở kết quả, kể cả sau khi đổi tên bản dự toán.
- **Then**: Màn hình kết quả hiển thị mã `BUILDX-20261005-Q7K2M9` cùng tên hiện tại theo BR-PROJ-003 khoản 12.
- **And**: Đổi tên không đổi mã; mở kết quả không tính lượt và không gọi AI.

#### AC-008

- **Given**: Bản dự toán đã xong 2D và Dự toán, chưa chạy 3D.
- **When**: Khách mở bản dự toán.
- **Then**: Thanh tiến trình cho thấy Nhập liệu, Bản vẽ 2D, Dự toán đã xong; Phối cảnh 3D Sẵn sàng; Hồ sơ Chưa mở.
- **And**: Không có nút tải tệp hay chia sẻ; yêu cầu tải gửi trực tiếp tới backend bị từ chối.

#### AC-009

- **Given**: Chặng Dự toán đã thành công và có cờ “cần kỹ sư rà” trong phần nội bộ.
- **When**: Chủ sở hữu mở bước Dự toán hoặc tải Excel.
- **Then**: Chỉ thấy phần gửi khách; không thấy cờ, ghi chú nội bộ, tên model hay chi phí AI theo BR-PROJ-017.
- **And**: Cờ không làm thay đổi trạng thái chặng.

## References

### TDDs

- TDD-PROJ-002
- TDD-PROJ-003
- TDD-PROJ-001: Thao tác đổi tên dùng cho AC-006; tên tệp tải về theo tên hiện tại thiết kế ở TDD-PROJ-003.

### Rules

- BR-PROJ-003/Then: Khoản 12, mã dự toán hiển thị ở kết quả của chủ sở hữu.
- BR-PROJ-006
- BR-PROJ-007
- BR-PROJ-010
- BR-PROJ-017
- BR-SUB-007

### Dependencies

- STORY-PROJ-002
- STORY-PROJ-008
- STORY-PROJ-009
- STORY-PROJ-010
- STORY-PROJ-004

## Non-Functional

- Kiểm tra quyền truy cập trước khi cung cấp nội dung hoặc tệp; không để tên hay mã bản dự toán là căn cứ duy nhất cấp quyền.
- Tệp của luồng bốn chặng lưu riêng tư ở kho. Mỗi lần xem hoặc tải, backend kiểm quyền rồi cấp link tải dùng được 15 phút (BR-PROJ-006 khoản 12); không ai mở được tệp chỉ bằng đường dẫn kho.

## Out of Scope

- Sửa kết quả của một chặng đã thành công; so sánh hoặc tối ưu nhiều phương án.
- Tải tệp riêng lẻ của từng chặng trước khi xong chặng Hồ sơ.
- Gửi link và quản lý quyền chia sẻ thuộc STORY-PROJ-004.
