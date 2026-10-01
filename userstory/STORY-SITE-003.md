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

# STORY-SITE-003

## Metadata

- **Story**: Là Admin, tôi muốn quản lý danh mục hiện trạng để khách chọn đúng tình trạng đất hoặc nhà khi lập hồ sơ công trình.
- **Context**: Danh mục hiện trạng mới gồm Đất trống, Có nhà cũ cần phá dỡ và Cải tạo. Admin thêm, đổi tên, sắp xếp và ngừng cho chọn; hồ sơ đã dùng giữ dữ liệu đã lưu theo BR-SITE-006. Không tạo trạng thái tiến độ thi công.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: Tân Trần
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người gọi đăng nhập bằng tài khoản Admin.

### Trigger

Admin mở phần quản lý hiện trạng công trình.

## Flow

### Main Flow

1. Admin xem danh mục theo thứ tự hiện hành.
2. Admin thêm mục có tên hoặc sửa tên/thứ tự của mục hiện có.
3. Hệ thống kiểm quyền và lưu thay đổi danh mục theo BR-SITE-006.
4. Form tạo công trình dùng các mục còn cho chọn; hồ sơ cũ không tự thay đổi.

### Alternative Flow

#### ALT-01

Admin ngừng cho chọn một mục.

1. Hệ thống bỏ mục đó khỏi lựa chọn mới của khách.
2. Hồ sơ đã sử dụng vẫn giữ mục đó; sửa trường khác không bắt buộc đổi hiện trạng.

#### ALT-02

Admin đổi tên hoặc thứ tự một mục đã được sử dụng.

1. Lựa chọn mới phản ánh tên/thứ tự mới.
2. Thông tin hiện trạng đã lưu trên công trình cũ không tự bị viết lại.

### Exception Flow

#### EXC-01

Người không phải Admin yêu cầu thay đổi danh mục hoặc Admin yêu cầu xóa mục đã được sử dụng.

1. Từ chối thao tác tương ứng; không đổi danh mục hoặc hồ sơ.

#### EXC-02

Yêu cầu thêm hoặc đổi tên không có nội dung.

1. Không lưu mục không có tên; chỉ rõ thông tin cần nhập.

## Acceptance Criteria

#### AC-001

- **Given**: Danh mục khởi tạo.
- **When**: Admin và khách mở danh mục trong phạm vi quyền của mình.
- **Then**: Có ba mục Đất trống, Có nhà cũ cần phá dỡ, Cải tạo; khách chọn một mục.

#### AC-002

- **Given**: Admin có phiên hợp lệ.
- **When**: Thêm một hiện trạng có tên rồi thay đổi thứ tự.
- **Then**: Mục được lưu và hiển thị đúng thứ tự cho lựa chọn mới.

#### AC-003

- **Given**: Công trình đã chọn một hiện trạng.
- **When**: Admin đổi tên mục.
- **Then**: Lựa chọn mới dùng tên mới; công trình cũ giữ thông tin hiện trạng đã lưu.

#### AC-004

- **Given**: Mục đang được một công trình sử dụng.
- **When**: Admin ngừng cho chọn mục; khách tạo mới hoặc sửa ngân sách hồ sơ cũ.
- **Then**: Không cho chọn mục đó cho hồ sơ mới; hồ sơ cũ vẫn giữ mục khi sửa ngân sách hợp lệ.

#### AC-005

- **Given**: Mục đã được công trình sử dụng.
- **When**: Yêu cầu xóa mục trực tiếp.
- **Then**: Từ chối, không mất liên kết hoặc thông tin hiện trạng.

#### AC-006

- **Given**: Người gọi là khách hoặc nhân viên không phải Admin.
- **When**: Gửi yêu cầu thêm, đổi tên, sắp xếp hoặc ngừng mục.
- **Then**: Tất cả bị từ chối, không chỉ ẩn nút trên giao diện.

#### AC-007

- **Given**: Admin đang thêm/đổi tên.
- **When**: Gửi tên rỗng hoặc chỉ khoảng trắng.
- **Then**: Từ chối lưu, danh mục cũ giữ nguyên.

## References

### TDDs

- TDD-SITE-004

### Rules

- BR-SITE-006/Then
- BR-SITE-001/Then
- BR-RBAC-011/Then

### Dependencies

- STORY-SITE-001: Khách chọn hiện trạng trên hồ sơ.

## Non-Functional

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Kiểm quyền Admin ở backend; cập nhật danh mục không ghi đè dữ liệu công trình.
- Nghiệp vụ được người dùng xác nhận ngày 01/10/2026; bản US/BR cụ thể đã được chốt, System Test đã được soạn. Chưa có TDD cho phần mở rộng này và chưa có kết quả kiểm thử thực thi.

## Out of Scope

- Nhân viên tạo/sửa công trình hộ khách; quản lý tiến độ; thêm quy trình phê duyệt danh mục.
- Xóa mục chưa dùng hoặc khôi phục mục đã ngừng chưa nằm trong các thao tác được chốt.
