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

# STORY-PROJ-002

## Metadata

- **Story**: Là khách hàng, tôi muốn gửi đầu vào của bản dự toán cho AI và theo dõi kết quả để nhận thiết kế, dự toán hoặc chủ động thử lại khi xử lý thất bại.
- **Context**: Bản nháp nghiệp vụ cho bước gửi AI và nhận kết quả. AI service do bên khác phụ trách; API, cơ chế nhận trạng thái/kết quả, định dạng tệp và danh sách đầu ra bắt buộc đang chờ tích hợp. Story mô tả hành vi đã có căn cứ, chưa phải hợp đồng kỹ thuật hoàn chỉnh.
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

- Khách đã đăng nhập bằng tài khoản khách hàng và có quyền truy cập bản dự toán.
- Có bản dự toán và thông tin đầu vào được chuẩn bị theo STORY-PROJ-001.
- Khách có gói thiết kế còn hiệu lực, quyền tạo thiết kế và lượt sẵn dùng hoặc không giới hạn theo BR-SUB-007.
- Bản dự toán chưa có kết quả thành công; không dùng luồng này để chỉnh sửa một thiết kế đã thành công theo BR-SUB-017.
- Tích hợp AI cần có hợp đồng và môi trường dùng được trước khi kiểm chứng xuyên suốt; hiện là phụ thuộc chưa sẵn sàng.

### Trigger

Khách bấm Nhận dự toán hoặc chủ động thử lại một lần tạo thiết kế đã thất bại.

## Flow

### Main Flow

1. Khách yêu cầu tạo thiết kế từ thông tin đã nhập của bản dự toán.
2. Backend kiểm tra quyền truy cập, điều kiện gói/lượt, tình trạng bản dự toán và tính đầy đủ/hợp lệ của đầu vào theo BR-PROJ-001, BR-PROJ-002 và BR-PROJ-004.
3. Khi tiếp nhận hợp lệ, giữ một lượt nếu hạn mức hữu hạn theo BR-SUB-003; khóa sửa đầu vào theo BR-PROJ-005 và gửi đúng dữ liệu đã tiếp nhận sang AI service.
4. Backend theo dõi quá trình xử lý và cung cấp trạng thái cho khách. Cơ chế trao đổi cụ thể với AI service chưa được chốt.
5. AI service tạo và trả nội dung thiết kế/dự toán. Backend không tự bóc khối lượng, quản lý đơn giá hoặc tính lại dự toán.
6. Backend kiểm tra kết quả theo hợp đồng, lưu đủ bộ kết quả và bảo đảm khách có thể mở xem. Chỉ lúc đó ghi nhận thành công và chuyển lượt giữ thành lượt đã dùng theo BR-SUB-003.
7. Khách xem kết quả đã lưu của bản dự toán và đi tiếp bước hồ sơ. Không cần khách bấm duyệt hoặc hài lòng mới tính thành công; không tính riêng lượt cho từng phần của cùng bộ kết quả.

### Alternative Flow

#### ALT-01

Tác vụ thất bại và bản dự toán chưa có kết quả thành công.

1. Khách mở lại thông tin đã nhập; được sửa khi đáp ứng điều kiện lưu theo BR-SUB-007 và BR-PROJ-005.
2. Khách chủ động thử lại trên cùng bản dự toán, có hoặc không sửa đầu vào.
3. Backend kiểm tra lại toàn bộ điều kiện tại lần yêu cầu mới, không dùng quyền hoặc lượt của yêu cầu cũ để bỏ qua kiểm tra.
4. Khi tiếp nhận hợp lệ, quay lại bước giữ lượt và khóa đầu vào của Main Flow.

#### ALT-02

Gói hết hạn hoặc sang kỳ mới trong khi một tác vụ đã được tiếp nhận hợp lệ còn đang xử lý.

1. Không hủy tác vụ chỉ vì hết hạn gói hoặc đổi kỳ.
2. Kết quả xử lý và lượt được ghi nhận vào kỳ đã giữ theo BR-SUB-003; thất bại không cộng lượt sang kỳ mới.
3. Giới hạn thời gian xử lý vẫn áp dụng theo BR-SUB-016.

### Exception Flow

#### EXC-01

Đầu vào thiếu hoặc không hợp lệ, khách không đủ quyền/lượt, hoặc bản dự toán đã có kết quả thành công.

1. Từ chối yêu cầu tương ứng trước khi bắt đầu tác vụ mới, không giữ/trừ lượt cho yêu cầu bị từ chối.
2. Không thay đổi kết quả đã thành công hoặc gửi AI để tạo thêm phương án cho bản dự toán đó.

#### EXC-02

Có yêu cầu thay đổi đầu vào trong lúc AI đang xử lý.

1. Từ chối cập nhật theo BR-PROJ-005, kể cả tự lưu đến muộn hoặc gửi trực tiếp.
2. Giữ nguyên đầu vào của tác vụ đang chạy.

#### EXC-03

Tạo thiết kế thất bại hoặc quá thời gian chờ được cấu hình.

1. Ghi nhận thất bại và giải phóng lượt đang giữ theo BR-SUB-003 và BR-SUB-016; không tính đã dùng.
2. Không công bố một phần kết quả dở dang như bộ kết quả thành công.
3. Giữ thông tin đầu vào; cho khách sửa/thử lại theo ALT-01 khi còn đủ điều kiện. Không tự gửi lại tác vụ.

#### EXC-04

Nhận kết quả sau khi tác vụ đã thất bại do quá thời gian.

1. Giữ trạng thái thất bại, không công bố kết quả muộn và không tính lại lượt theo BR-SUB-016.
2. Không gắn kết quả này vào lần thử lại của khách.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đủ điều kiện, bản dự toán chưa thành công và đầu vào hợp lệ.
- **When**: Backend tiếp nhận yêu cầu tạo thiết kế.
- **Then**: Giữ một lượt nếu hạn mức hữu hạn, khóa sửa đầu vào và gửi đúng thông tin tiếp nhận sang AI service.
- **And**: Chưa ghi lượt đã dùng chỉ vì đã gửi yêu cầu.

#### AC-002

- **Given**: AI đang xử lý một yêu cầu đã tiếp nhận hợp lệ.
- **When**: Có yêu cầu lưu thay đổi đầu vào, kể cả yêu cầu gửi trực tiếp hoặc tự lưu đến muộn.
- **Then**: Backend từ chối thay đổi đó.
- **And**: Đầu vào tác vụ đang chạy giữ nguyên.

#### AC-003

- **Given**: AI đã trả đủ kết quả theo hợp đồng tích hợp.
- **When**: Hệ thống lưu thành công và khách có thể mở xem bộ kết quả.
- **Then**: Ghi thành công và chuyển lượt giữ thành đã dùng theo BR-SUB-003.
- **And**: Không chờ khách mở thực tế hoặc duyệt, không trừ thêm lượt cho từng phần của bộ kết quả.

#### AC-004

- **Given**: Tác vụ thất bại hoặc quá thời gian, chưa có kết quả thành công.
- **When**: Backend kết thúc tác vụ với trạng thái thất bại.
- **Then**: Giải phóng lượt đang giữ, giữ thông tin đầu vào và mở lại khả năng sửa khi khách còn đủ điều kiện.
- **And**: Chỉ gửi lại AI khi khách chủ động thử lại; mỗi lần thử lại kiểm tra quyền/lượt và đầu vào một lần nữa.

#### AC-005

- **Given**: Tác vụ đã thất bại do quá thời gian và giải phóng lượt giữ.
- **When**: Kết quả của tác vụ đó đến muộn.
- **Then**: Không công bố hoặc chuyển thành công, không tính lại lượt.
- **And**: Không đưa kết quả vào lần thử lại khác.

#### AC-006

- **Given**: Một bản dự toán đã có kết quả thành công.
- **When**: Khách yêu cầu tạo thêm hoặc sửa thiết kế bằng AI trên bản dự toán đó.
- **Then**: Từ chối theo BR-SUB-017; muốn phương án khác phải tạo dự toán mới.
- **And**: Kết quả cũ không bị thay thế và không giữ/trừ lượt cho yêu cầu bị từ chối.

#### AC-007

- **Given**: Tác vụ đã được tiếp nhận hợp lệ rồi gói hết hạn hoặc chuyển kỳ.
- **When**: Tác vụ kết thúc thành công hoặc thất bại trong giới hạn xử lý áp dụng.
- **Then**: Xử lý lượt tại kỳ đã giữ theo BR-SUB-003.
- **And**: Không cộng lượt lỗi sang kỳ mới hoặc hủy tác vụ chỉ vì gói hết hạn.

## References

### TDDs

- TDD-PROJ-002

### Rules

- BR-PROJ-001
- BR-PROJ-002
- BR-PROJ-004
- BR-PROJ-005
- BR-SUB-003
- BR-SUB-007
- BR-SUB-016
- BR-SUB-017

### Dependencies

- STORY-PROJ-001

## Non-Functional

- Bảo vệ dữ liệu đầu vào đã tiếp nhận khỏi thay đổi trong lúc AI xử lý bằng kiểm tra tại backend.
- [Chờ hợp đồng AI để xác định cấu trúc đầu ra bắt buộc, cách nhận trạng thái và phân loại lỗi; giá trị thời gian chờ chưa được chốt.]
- [Cơ chế xử lý yêu cầu mạng gửi lặp, cập nhật đồng thời và phản hồi trùng sẽ mô tả trong thiết kế; chưa có API hoặc phương án kỹ thuật được chốt.]

## Out of Scope

- Xây AI service hoặc tự tính dự toán tại backend BMT.
- Khách hàng hủy tác vụ; chỉnh sửa thiết kế sau thành công; tự thử lại tác vụ thất bại.
- Xuất PDF/Excel, link chia sẻ, QR và email thuộc phần hồ sơ kế tiếp trong phạm vi tổng thể; chưa loại bỏ thao tác nào khỏi kế hoạch.
- Cam kết chất lượng hoặc số lượng tệp đầu ra khi chưa có hợp đồng AI service; chưa được coi là tích hợp đã kiểm chứng.
