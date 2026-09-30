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

# STORY-CONSULT-001

## Metadata

- **Story**: Là admin, tôi muốn quản lý hồ sơ KTS để khách chọn người phù hợp khi gửi yêu cầu tư vấn.
- **Context**: Admin phụ trách thông tin KTS và việc hiển thị hồ sơ. Người dùng xác nhận ảnh đại diện là đường dẫn có sẵn, backend không có luồng upload; ngày 26/09/2026 người dùng chốt thêm đường dẫn phải thuộc kho ảnh của hệ thống (BR-CONSULT-001 khoản 9). Bộ US/BR trước lần bổ sung này đã được chốt trong hội thoại. Ngày 29/09/2026, người dùng xác nhận thêm Công ty bắt buộc nhập tên, Số sao và Số đánh giá nhập thủ công; cả ba hiển thị ở danh sách và chi tiết KTS. Hồ sơ có mười nhóm thông tin; hai số mới mặc định bằng 0. Người dùng đã chốt bản cập nhật US/BR ngày 29/09/2026. Hồ sơ cũ chưa có Công ty được giữ nguyên cùng trạng thái hiển thị; phải bổ sung tên công ty ở lần sửa tiếp theo. Metadata chưa đầy đủ. Giới hạn kỹ thuật của tên công ty sẽ xác định khi thiết kế. Người tạo chủ động chọn Ẩn/Hiện khi tạo hồ sơ.
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

- Người thao tác có quyền quản lý tư vấn KTS theo STORY-RBAC-001; vai trò Admin có quyền này.

### Trigger

Admin mở chức năng quản lý hồ sơ KTS.

## Flow

### Main Flow

1. Người có quyền tạo thêm hồ sơ với ảnh đại diện, họ tên, chức danh, chuyên môn chọn từ danh mục category, số năm kinh nghiệm, số công trình, giới thiệu, công ty, số sao và số đánh giá; một KTS có thể chọn nhiều category. Công ty nhập tên trực tiếp và bắt buộc có nội dung. Số sao và số đánh giá nhập thủ công, mặc định bằng 0 khi tạo hồ sơ.
2. Người tạo chọn Ẩn/Hiện. Hệ thống kiểm tra thông tin bắt buộc và các quy tắc tại BR-CONSULT-001: số sao từ 0 đến 5, tối đa một chữ số thập phân; số đánh giá là số nguyên không âm; khi số đánh giá bằng 0 thì số sao phải bằng 0. Hồ sơ hợp lệ được lưu trực tiếp, không qua phê duyệt. Người có quyền có thể đổi trạng thái sau khi tạo.
3. Khách xem công ty, số sao và số đánh giá ở cả danh sách và trang chi tiết của các hồ sơ đang hiển thị, rồi chọn KTS để gửi yêu cầu.

### Alternative Flow

#### ALT-01

Admin cập nhật hồ sơ đã có.

1. Admin sửa thông tin, gồm công ty, số sao và số đánh giá khi cần, rồi lưu.
2. Hệ thống kiểm tra dữ liệu theo các quy tắc khi tạo và sử dụng thông tin đã cập nhật cho hồ sơ. Công ty, số sao và số đánh giá mới được dùng thống nhất ở danh sách và chi tiết KTS.

#### ALT-02

Admin ẩn hoặc hiện lại hồ sơ.

1. Admin thay đổi trạng thái hiển thị.
2. Hồ sơ bị ẩn không nhận yêu cầu mới; yêu cầu cũ được giữ để xử lý. Hồ sơ hiện lại được chọn cho yêu cầu mới.

#### ALT-03

Người có quyền quản lý danh mục chuyên môn.

1. Người có quyền tạo hoặc sửa category để dùng cho phần chuyên môn của KTS.
2. Người có quyền được xóa category khi không còn KTS nào sử dụng, kể cả KTS đang bị ẩn.

#### ALT-04

Hồ sơ đã có trước khi bổ sung Công ty, Số sao và Số đánh giá.

1. Hệ thống giữ nguyên hồ sơ và trạng thái Ẩn/Hiện; không tự điền tên công ty. Hai số mới được khởi tạo bằng 0.
2. Hồ sơ đang Hiện vẫn xuất hiện cho khách khi chưa bổ sung Công ty. Hồ sơ đang Ẩn vẫn được giữ ở trạng thái Ẩn.
3. Khi sửa hồ sơ lần tiếp theo, người có quyền phải nhập tên công ty hợp lệ mới lưu được.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý tư vấn KTS.

1. Hệ thống từ chối thêm, sửa hoặc đổi trạng thái hiển thị.

#### EXC-02

Hồ sơ thiếu thông tin bắt buộc.

1. Hệ thống từ chối lưu và chỉ rõ thông tin còn thiếu, gồm cả trường hợp chưa chọn category nào.
2. Người thao tác bổ sung dữ liệu rồi lưu lại.

#### EXC-03

Category cần xóa còn được gán cho KTS.

1. Hệ thống từ chối xóa, giữ nguyên category và các liên kết hiện có.
2. Người có quyền phải bỏ gán category khỏi tất cả hồ sơ trước khi xóa.

#### EXC-04

Số sao hoặc số đánh giá không hợp lệ.

1. Hệ thống từ chối lưu nếu số sao ngoài khoảng 0–5 hoặc có hơn một chữ số thập phân; số đánh giá âm hoặc không phải số nguyên; hoặc số đánh giá bằng 0 nhưng số sao khác 0.
2. Hệ thống chỉ rõ trường cần sửa, không lưu một phần thay đổi của hồ sơ.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý.
- **When**: Admin thêm hoặc sửa hồ sơ.
- **Then**: Lưu và đọc lại được đầy đủ mười nhóm thông tin đã xác nhận, gồm công ty, số sao và số đánh giá nhập thủ công.
- **And**: Không tự thêm tài khoản đăng nhập cho KTS.

#### AC-002

- **Given**: KTS đang hiển thị và đã có yêu cầu tư vấn.
- **When**: Admin ẩn KTS.
- **Then**: Khách không thể gửi yêu cầu mới cho KTS này.
- **And**: Các yêu cầu cũ vẫn xem và xử lý được.

#### AC-003

- **Given**: Hồ sơ đang bị ẩn.
- **When**: Admin cho hiện lại.
- **Then**: Khách có thể chọn KTS để gửi yêu cầu mới.
- **And**: Thông tin hồ sơ vẫn được giữ.

#### AC-004

- **Given**: Người thao tác không có quyền quản lý tư vấn KTS.
- **When**: Gửi thao tác quản lý hồ sơ.
- **Then**: Hệ thống từ chối thao tác.
- **And**: Hồ sơ không bị thay đổi.

#### AC-005

- **Given**: Danh mục có nhiều category chuyên môn và người thao tác có quyền sửa hồ sơ.
- **When**: Gán nhiều category cho một KTS rồi lưu.
- **Then**: Hồ sơ đọc lại có các category đã chọn.
- **And**: Chuyên môn dùng danh mục category, không nhập thành các nhãn riêng trên từng hồ sơ.

#### AC-006

- **Given**: Người thao tác có quyền tạo hồ sơ.
- **When**: Lưu hồ sơ hợp lệ.
- **Then**: Hệ thống tạo hồ sơ trực tiếp.
- **And**: Người tạo chọn Ẩn/Hiện ngay khi tạo, không yêu cầu người khác phê duyệt.

#### AC-007

- **Given**: Người thao tác có quyền tạo hoặc sửa category.
- **When**: Tạo hoặc sửa category chuyên môn.
- **Then**: Thay đổi được lưu trong danh mục dùng cho hồ sơ KTS.
- **And**: Người không có quyền tương ứng không được thực hiện thao tác đó.

#### AC-008

- **Given**: Người thao tác có quyền lưu hồ sơ KTS.
- **When**: Thêm hoặc sửa hồ sơ thiếu một trong bảy nhóm thông tin ban đầu, thiếu tên công ty hoặc chưa chọn category nào.
- **Then**: Hệ thống từ chối lưu và chỉ rõ phần còn thiếu.
- **And**: Khi đủ thông tin hợp lệ, hồ sơ được lưu trực tiếp, không qua phê duyệt.

#### AC-009

- **Given**: Category đang được gán cho ít nhất một KTS, kể cả hồ sơ bị ẩn.
- **When**: Người có quyền yêu cầu xóa category.
- **Then**: Hệ thống từ chối xóa.
- **And**: Giữ nguyên category và các liên kết với KTS.

#### AC-010

- **Given**: Category không còn được gán cho KTS nào và người thao tác có quyền xóa.
- **When**: Người thao tác xóa category.
- **Then**: Category được xóa khỏi danh mục chuyên môn.
- **And**: Không còn được chọn khi gán chuyên môn cho hồ sơ KTS.

#### AC-011

- **Given**: Người thao tác có quyền lưu hồ sơ KTS và các thông tin khác hợp lệ.
- **When**: Thêm hoặc sửa hồ sơ với tên công ty bị thiếu, rỗng hoặc chỉ có khoảng trắng.
- **Then**: Hệ thống từ chối lưu và thông báo phải nhập tên công ty.
- **And**: Công ty được nhập bằng tên trực tiếp, không yêu cầu chọn từ danh mục công ty.

#### AC-012

- **Given**: Người thao tác có quyền lưu hồ sơ và các thông tin khác hợp lệ.
- **When**: Nhập số sao trong khoảng 0–5, tối đa một chữ số thập phân, và số đánh giá là số nguyên không âm; nếu số đánh giá bằng 0 thì số sao cũng bằng 0.
- **Then**: Hệ thống lưu và đọc lại đúng hai giá trị đã nhập, ví dụ 4,8 sao và 120 đánh giá.
- **And**: Hệ thống không tự tính hai giá trị từ yêu cầu tư vấn hoặc đánh giá của khách.

#### AC-013

- **Given**: Người thao tác có quyền tạo hồ sơ và các thông tin bắt buộc hợp lệ.
- **When**: Tạo hồ sơ và giữ giá trị mặc định của số sao và số đánh giá.
- **Then**: Hồ sơ được lưu với số sao bằng 0 và số đánh giá bằng 0.
- **And**: Hồ sơ đã có trước khi bổ sung hai trường cũng được khởi tạo hai số bằng 0.

#### AC-014

- **Given**: Người thao tác có quyền thêm hoặc sửa hồ sơ KTS.
- **When**: Nhập số sao ngoài khoảng 0–5 hoặc hơn một chữ số thập phân; nhập số đánh giá âm hoặc không phải số nguyên; hoặc nhập số đánh giá bằng 0 và số sao khác 0.
- **Then**: Hệ thống từ chối lưu và chỉ rõ trường không hợp lệ.
- **And**: Không tạo hồ sơ mới hoặc lưu một phần thay đổi vào hồ sơ đã có.

#### AC-015

- **Given**: Hồ sơ KTS đang hiển thị và đã lưu công ty, số sao, số đánh giá hợp lệ.
- **When**: Khách xem danh sách KTS hoặc trang chi tiết của KTS đó.
- **Then**: Cả hai nơi đều hiển thị công ty, số sao và số đánh giá đúng theo hồ sơ hiện tại, kể cả hai số bằng 0.
- **And**: Sau khi người có quyền cập nhật thành công, lần đọc tiếp theo ở cả hai nơi dùng các giá trị đã cập nhật.

#### AC-016

- **Given**: Hồ sơ KTS đã tồn tại trước khi thêm ba trường, chưa có tên công ty.
- **When**: Áp dụng thay đổi rồi đọc lại hồ sơ.
- **Then**: Giữ nguyên hồ sơ và trạng thái Ẩn/Hiện; không tự điền tên công ty; số sao và số đánh giá bằng 0.
- **And**: Hồ sơ đang Hiện vẫn xem được ở danh sách và chi tiết; lần sửa tiếp theo phải bổ sung công ty hợp lệ mới lưu được.

## References

### TDDs

- TDD-CONSULT-001

### Rules

- BR-CONSULT-001

### Dependencies



## Non-Functional

- Chức năng quản trị chỉ dành cho người có quyền quản lý tư vấn KTS theo STORY-RBAC-001; ghi chú nội bộ không được công khai hoặc gửi cho khách. Chưa xác định chỉ tiêu hiệu năng định lượng.

## Out of Scope

- Xóa hồ sơ; tài khoản và giao diện riêng cho KTS.
- Quản lý lịch rảnh riêng của từng KTS.
- Danh mục công ty; chức năng khách gửi đánh giá và tự tính số sao hoặc số đánh giá.
