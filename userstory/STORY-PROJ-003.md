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

- **Story**: Là chủ sở hữu bản dự toán, tôi muốn xem kết quả dự toán, hồ sơ và tải PDF/Excel để sử dụng kết quả thiết kế đã tạo.
- **Context**: Bao phủ bước dự toán và hồ sơ của trang mẫu. Nội dung do AI service trả; hợp đồng dữ liệu và cách bàn giao hoặc xuất tệp đang chờ tích hợp. Story không đặt công thức hoặc bảng đơn giá cho backend BMT.
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

- Khách đã đăng nhập và là chủ sở hữu bản dự toán.
- Bản dự toán có kết quả đã lưu và có thể mở xem theo STORY-PROJ-002.
- Hợp đồng AI phải xác định dữ liệu và tệp đầu ra trước khi kiểm chứng tích hợp thật.

### Trigger

Khách mở bước Nhận dự toán hoặc Hồ sơ thi công, yêu cầu chuẩn bị/tải PDF hay tải Excel dự toán.

## Flow

### Main Flow

1. Khách mở kết quả dự toán của bản dự toán đã tạo.
2. Backend kiểm tra quyền truy cập và trả kết quả đã lưu theo BR-PROJ-007: tổng dự toán, các nhóm phần thô/hoàn thiện/nội thất và thông tin chi tiết, tư vấn theo dữ liệu AI trả.
3. Khách chuyển sang hồ sơ để xem thông tin bản dự toán và nội dung kết quả thiết kế/dự toán.
4. Khi khách yêu cầu PDF hoặc Excel, hệ thống cung cấp tệp của đúng kết quả nguồn; chuẩn bị tệp nếu hợp đồng tích hợp yêu cầu xuất từ dữ liệu đã lưu.
5. Khi tệp sẵn sàng, khách tải xuống. Việc xem, xuất và tải không khởi chạy lại AI tạo thiết kế hoặc giữ/trừ lượt.

### Alternative Flow

#### ALT-01

Gói thiết kế đã hết hạn nhưng bản dự toán có kết quả cũ.

1. Vẫn cho chủ sở hữu bản dự toán xem kết quả và tải tệp.
2. Cho phép xuất PDF/Excel từ kết quả cũ dù chưa có tệp trước khi gói hết hạn, theo BR-SUB-007.
3. Không cấp kỳ hoặc lượt mới chỉ vì dùng các thao tác này.

### Exception Flow

#### EXC-01

Khách không có quyền truy cập bản dự toán được yêu cầu.

1. Từ chối trả nội dung kết quả hoặc tệp của bản dự toán đó.
2. Luồng người nhận link công khai được kiểm tra riêng theo STORY-PROJ-004, không bỏ qua kiểm tra quyền của luồng chủ sở hữu bản dự toán.

#### EXC-02

Kết quả chưa sẵn sàng hoặc tác vụ AI đã thất bại.

1. Trả tình trạng thực tế của bản dự toán theo STORY-PROJ-002.
2. Không lấy dữ liệu minh họa hoặc kết quả chưa đủ làm hồ sơ hoàn chỉnh để xuất/tải.

#### EXC-03

Chuẩn bị hoặc tải tệp thất bại sau khi kết quả thiết kế đã thành công.

1. Báo thao tác tệp chưa thành công, giữ nguyên kết quả nguồn.
2. Không trừ lượt hoặc gửi lại yêu cầu AI tạo thiết kế để thay thế thao tác xuất tệp bị lỗi.
3. Cho phép khách yêu cầu lại thao tác tệp; cách xử lý kỹ thuật và lỗi nhà cung cấp còn chờ thiết kế/hợp đồng.

## Acceptance Criteria

#### AC-001

- **Given**: Chủ sở hữu bản dự toán có kết quả AI hợp lệ đã lưu.
- **When**: Mở dự toán hoặc hồ sơ.
- **Then**: Nhận nội dung của đúng bản dự toán và kết quả nguồn, theo BR-PROJ-007.
- **And**: Backend không tự tính lại số tiền hoặc dùng dữ liệu mẫu thay cho dữ liệu AI.

#### AC-002

- **Given**: Có kết quả nguồn đủ để cung cấp PDF/Excel theo hợp đồng tích hợp.
- **When**: Chủ sở hữu bản dự toán yêu cầu xuất và tải tệp.
- **Then**: Cung cấp tệp đúng bản dự toán và cùng kết quả đang xem khi tệp sẵn sàng.
- **And**: Không tạo thiết kế AI mới hoặc giữ/trừ lượt.

#### AC-003

- **Given**: Gói đã hết hạn, bản dự toán có kết quả cũ thuộc khách.
- **When**: Khách xem, xuất PDF/Excel hoặc tải tệp kết quả cũ.
- **Then**: Cho phép theo BR-SUB-007 dù tệp chưa được xuất trước khi hết hạn.
- **And**: Không tự gia hạn gói hay làm mới lượt.

#### AC-004

- **Given**: Khách không có quyền truy cập bản dự toán trong luồng chủ sở hữu bản dự toán.
- **When**: Yêu cầu nội dung hoặc tệp của bản dự toán đó.
- **Then**: Từ chối, không trả dữ liệu hoặc tệp.
- **And**: Quyền qua link chia sẻ chỉ được xét qua luồng có kiểm tra link còn hiệu lực.

#### AC-005

- **Given**: Kết quả thiết kế đã thành công nhưng thao tác xuất/tải tệp bị lỗi.
- **When**: Hệ thống phản hồi thao tác bị lỗi.
- **Then**: Không báo tệp đã sẵn sàng hoặc đã tải thành công khi chưa có căn cứ.
- **And**: Giữ kết quả nguồn, không tính thêm lượt; khách có thể yêu cầu lại thao tác tệp.

#### AC-006

- **Given**: Bản dự toán "Phương án 3" đã có tệp PDF do AI tạo và đang được chia sẻ qua link còn hiệu lực.
- **When**: Chủ sở hữu đổi tên thành "Phương án chốt", rồi chủ sở hữu và người nhận link lần lượt mở hồ sơ và tải PDF.
- **Then**: Cả hai đều thấy tên "Phương án chốt" trên hồ sơ; tệp PDF tải về có tên tệp "Phương án chốt", còn nội dung tệp giữ nguyên như lúc AI tạo theo BR-PROJ-007 khoản 7 (người dùng xác nhận ngày 26/09/2026).
- **And**: Việc tải lại không tính lượt, không gọi AI, không tự dựng lại tệp và không thay kết quả nguồn.

## References

### TDDs

- TDD-PROJ-002
- TDD-PROJ-003
- TDD-PROJ-001: Thao tác đổi tên dùng cho AC-006; tên tệp tải về theo tên hiện tại thiết kế ở TDD-PROJ-003.

### Rules

- BR-PROJ-007
- BR-SUB-003
- BR-SUB-007

### Dependencies

- STORY-PROJ-002
- STORY-PROJ-004

## Non-Functional

- Kiểm tra quyền truy cập trước khi cung cấp nội dung hoặc tệp; không để tên/mã bản dự toán là căn cứ duy nhất cấp quyền.
- [Chờ hợp đồng AI về cấu trúc dự toán, ảnh/bản vẽ, tệp, đơn vị tiền và phần kết quả bắt buộc. Chưa chốt công nghệ xuất tệp hoặc cam kết thời gian xử lý.]

## Out of Scope

- Backend tự tạo nội dung chuyên môn, quản lý đơn giá hoặc tính lại dự toán.
- Thay đổi một thiết kế đã thành công; đối chiếu hoặc tối ưu nhiều phương án.
- Gửi link và quản lý quyền chia sẻ thuộc STORY-PROJ-004.
