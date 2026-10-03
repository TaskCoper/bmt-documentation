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

# STORY-RFQ-003

## Metadata

- **Story**: Là người có quyền cấu hình, tôi muốn thay đổi giới hạn nhà thầu được mời trên mỗi hồ sơ để điều chỉnh số lượt gửi dùng chung.
- **Context**: Giới hạn ban đầu là 3 nhà thầu khác nhau/hồ sơ. Thay đổi áp dụng ngay cho mọi hồ sơ, giữ nguyên các lời mời cũ. Người dùng đã xác nhận giới hạn là số nguyên từ 1 trở lên, Admin và người được cấp quyền cấu hình riêng được sửa. Đây là bản nháp tổng hợp các quyết định trong hội thoại ngày 03/10/2026; người dùng đã chốt cả bộ US/BR và giao triển khai. Đã soạn đặc tả System Test; chưa chạy các ca. Reviewer và Approver đã được xác nhận; các metadata phân công còn thiếu.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác là Admin hoặc được cấp quyền cấu hình riêng để thay đổi giới hạn.

### Trigger

Người có quyền cần thay đổi số nhà thầu tối đa được mời trên mỗi hồ sơ.

## Flow

### Main Flow

1. Người có quyền đọc giới hạn hiện tại; giá trị ban đầu là 3.
2. Người thao tác nhập giới hạn mới là số nguyên từ 1 trở lên. Hệ thống kiểm tra quyền và giá trị trước khi lưu trong cấu hình dùng chung.
3. Các lần gửi mới dùng giới hạn mới ngay sau thay đổi; không tính lại hoặc xóa các lời mời đã tiếp nhận.
4. Mỗi cặp hồ sơ–nhà thầu vẫn chỉ có một lời mời, dù giới hạn được tăng.

### Alternative Flow

#### ALT-01

Tăng giới hạn từ 3 lên 5.

1. Hồ sơ đã mời 3 nhà thầu được mời thêm tối đa 2 nhà thầu khác.

#### ALT-02

Giảm giới hạn từ 3 xuống 2.

1. Giữ mọi lời mời cũ. Hồ sơ đã mời từ 2 nhà thầu trở lên không được gửi thêm.

### Exception Flow

#### EXC-01

Người thao tác không có quyền cấu hình.

1. Từ chối thay đổi; giữ giới hạn hiện tại.

#### EXC-02

Giới hạn mới là 0, số âm hoặc số không nguyên.

1. Từ chối thay đổi, thông báo phải nhập số nguyên từ 1 trở lên và giữ giới hạn hiện tại.
2. Không tự làm tròn hoặc hiểu 0 là tạm dừng gửi hay không giới hạn.

## Acceptance Criteria

#### AC-001

- **Given**: Cấu hình dùng giá trị ban đầu.
- **When**: Khách gửi lời mời từ một hồ sơ.
- **Then**: Giới hạn là 3 nhà thầu khác nhau.
- **And**: Không cộng lượt của hồ sơ khác và không cho gửi lặp một nhà thầu.

#### AC-002

- **Given**: Hồ sơ đã mời A, B, C; giới hạn hiện tại là 3.
- **When**: Người có quyền đổi giới hạn thành 5.
- **Then**: Hồ sơ được mời thêm D và E nếu các điều kiện còn lại hợp lệ.
- **And**: Không được mời lại A hoặc tạo lời mời cho nhà thầu thứ sáu.

#### AC-003

- **Given**: Hồ sơ đã mời 3 nhà thầu.
- **When**: Người có quyền giảm giới hạn xuống 2.
- **Then**: Cả ba lời mời cũ vẫn tồn tại và tiếp tục xử lý.
- **And**: Hồ sơ bị chặn gửi thêm; không tự xóa hoặc hủy lời mời cũ.

#### AC-004

- **Given**: Có nhiều hồ sơ được tạo trước và sau lần sửa cấu hình.
- **When**: Giới hạn được đổi thành 5.
- **Then**: Mọi hồ sơ dùng giới hạn mới cho các lần gửi sau đó.
- **And**: Không giữ riêng giới hạn cũ theo thời điểm tạo hồ sơ.

#### AC-005

- **Given**: Người gọi không có quyền cấu hình.
- **When**: Yêu cầu thay đổi giới hạn.
- **Then**: Thao tác bị từ chối.
- **And**: Giá trị hiện tại không thay đổi.

#### AC-006

- **Given**: Người có quyền cấu hình và giới hạn hiện tại là 3.
- **When**: Người đó lần lượt gửi giá trị 0, -1 hoặc 2,5.
- **Then**: Từ chối từng yêu cầu; giới hạn vẫn là 3.
- **And**: Không tự làm tròn hoặc dùng 0 với ý nghĩa đặc biệt.

#### AC-007

- **Given**: Người thao tác là Admin hoặc người được cấp quyền cấu hình riêng.
- **When**: Lưu giới hạn bằng 1.
- **Then**: Chấp nhận giá trị nhỏ nhất theo nghiệp vụ và áp dụng cho các lần gửi mới trên mọi hồ sơ.
- **And**: Người chỉ có quyền quản lý lời mời mà không có quyền cấu hình không được sửa giới hạn.

## References

### TDDs

- TDD-RFQ-001

### Rules

- BR-RFQ-002

### Dependencies

- STORY-RFQ-001: Kiểm giới hạn tại thời điểm gửi lời mời.

## Non-Functional

- Đặc tả kiểm thử: [ST-RFQ-022](../systemtest/ST-RFQ-022.md), [ST-RFQ-023](../systemtest/ST-RFQ-023.md), [ST-RFQ-024](../systemtest/ST-RFQ-024.md), [ST-RFQ-025](../systemtest/ST-RFQ-025.md), [ST-RFQ-026](../systemtest/ST-RFQ-026.md), [ST-RFQ-027](../systemtest/ST-RFQ-027.md), [ST-RFQ-028](../systemtest/ST-RFQ-028.md). Chưa chạy; đây không phải kết quả Pass.

- Cấu hình phải được lưu bền vững, không chỉ thay đổi một giá trị ở frontend.
- Không tự thêm một hệ thống cấu hình tổng quát với các nghiệp vụ ngoài giới hạn mời báo giá.
- Giới hạn là số nguyên từ 1 trở lên; Admin và người được cấp quyền cấu hình riêng được sửa theo BR-RFQ-002. Quyền quản lý lời mời không tự bao gồm quyền cấu hình. Mã quyền, giới hạn biểu diễn kỹ thuật và giao diện cụ thể thuộc thiết kế.

## Out of Scope

- Giới hạn riêng theo khách, hồ sơ hoặc gói dịch vụ; hoàn lại lượt; đặt lại lượt theo ngày/tháng.
