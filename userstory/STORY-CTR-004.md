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

# STORY-CTR-004

## Metadata

- **Story**: Là khách, tôi muốn xem danh sách và hồ sơ nhà thầu công khai, lọc theo loại công trình, phạm vi thi công hoặc bán kính để tìm đơn vị phù hợp.
- **Context**: Danh sách hoạt động theo query tùy chọn, không có query thì lấy tất cả nhà thầu đang hiển thị. Hai query danh mục là mảng GUID, lọc theo năng lực admin chọn trong hồ sơ. Query bán kính dùng vị trí công trình của khách và cần đăng nhập. Công trình phải có tọa độ ngay khi tạo. Nghiệp vụ đã được người dùng chốt trong hội thoại; chưa triển khai.
- **Sprint**: [Chưa xác định]
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

- Khách được mở danh sách và hồ sơ công khai mà không cần đăng nhập. Danh sách có thể chưa có nhà thầu đang hiển thị.
- Chỉ khi tìm theo bán kính: khách đăng nhập, chọn công trình của mình và công trình có tọa độ.

### Trigger

Khách mở danh sách nhà thầu, gửi query lọc hoặc mở một hồ sơ.

## Flow

### Main Flow

1. Khách mở danh sách mà không truyền query lọc; không yêu cầu đăng nhập.
2. Hệ thống lấy toàn bộ tập nhà thầu đang hiển thị theo BR-CTR-005; API công khai không phân trang theo TDD-CTR-002 đã chốt.
3. Khách có thể truyền mảng GUID loại công trình, mảng GUID phạm vi thi công hoặc cả hai.
4. Hệ thống lọc theo năng lực admin khai báo: khớp ít nhất một giá trị mỗi mảng, và khớp cả hai nhóm khi có cả hai mảng.
5. Khách mở chi tiết nhà thầu để xem Tổng quan, vị trí công ty trên bản đồ, toàn bộ dự án đã thực hiện, Năng lực pháp lý và Hợp tác BuildX, gồm các bản scan đã tải lên. Không cung cấp thông tin liên hệ nội bộ.

### Alternative Flow

#### ALT-01

Khách đã đăng nhập và truyền query bán kính cùng công trình của mình.

1. Dùng kinh độ và vĩ độ công trình làm tâm; tính khoảng cách đường thẳng tới tọa độ công ty theo đơn vị km.
2. Lấy các nhà thầu đang hiển thị trong bán kính và tiếp tục áp dụng các query danh mục nếu có.

#### ALT-02

Khách tạo công trình để sử dụng cho tìm nhà thầu.

1. Khách nhập địa chỉ; frontend xác định và gửi cả kinh độ, vĩ độ ngay khi tạo công trình.
2. Hệ thống lưu kinh độ và vĩ độ cùng công trình; không chờ lần tìm nhà thầu đầu tiên mới bổ sung.

#### ALT-03

Không có nhà thầu khớp các query đã gửi.

1. Trả tập kết quả rỗng; không tự bỏ hoặc nới bộ lọc.

### Exception Flow

#### EXC-01

Khách chưa đăng nhập nhưng yêu cầu tìm theo bán kính quanh công trình.

1. Yêu cầu đăng nhập trước khi dùng công trình làm tâm tìm kiếm.

#### EXC-02

Khách yêu cầu dùng công trình của tài khoản khác.

1. Không cho dùng công trình đó làm tâm hoặc tiết lộ thông tin công trình.

#### EXC-03

Khách tạo công trình thiếu kinh độ hoặc vĩ độ.

1. Từ chối yêu cầu thiếu tọa độ; không tạo công trình không đủ vị trí.

## Acceptance Criteria

#### AC-001

- **Given**: Có nhà thầu đang hiển thị và nhà thầu không hiển thị.
- **When**: Khách chưa đăng nhập lấy danh sách không có query lọc.
- **Then**: Tập kết quả gồm toàn bộ nhà thầu đang hiển thị.
- **And**: Nhà thầu không hiển thị và thông tin liên hệ nội bộ không được công khai.

#### AC-002

- **Given**: Nhà thầu A nhận Nhà phố và Phần thô; chưa có dự án đã thực hiện phù hợp.
- **When**: Khách lọc loại [Nhà phố, Biệt thự] và phạm vi [Phần thô, Trọn gói].
- **Then**: Nhà thầu A khớp bộ lọc nếu đang hiển thị.
- **And**: Dữ liệu dự án không quyết định kết quả lọc.

#### AC-003

- **Given**: Nhà thầu B chỉ khớp một trong hai nhóm query được truyền.
- **When**: Khách lấy danh sách với cả query loại công trình và phạm vi thi công.
- **Then**: Nhà thầu B không nằm trong kết quả.
- **And**: Trong mỗi nhóm chỉ cần khớp ít nhất một GUID.

#### AC-004

- **Given**: Khách đăng nhập, chọn công trình có tọa độ; nhà thầu A ở trong và B ở ngoài bán kính, không nằm đúng đường biên.
- **When**: Khách truyền query bán kính theo km.
- **Then**: Chỉ nhà thầu ở trong bán kính và khớp các query danh mục nếu có được trả về.
- **And**: Khoảng cách dùng tọa độ công ty và công trình, không dùng quãng đường di chuyển.

#### AC-005

- **Given**: Hồ sơ nhà thầu đang hiển thị có dự án và bản scan pháp lý, hợp tác.
- **When**: Khách chưa đăng nhập mở chi tiết.
- **Then**: Khách xem các nhóm thông tin công khai, toàn bộ dự án và các bản scan đã tải lên.
- **And**: Không trả người liên hệ, số điện thoại hoặc email. Khi hồ sơ chuyển sang Ẩn, hồ sơ và các bản scan không còn được cung cấp công khai.

#### AC-006

- **Given**: Khách đã đăng nhập và tạo công trình mới.
- **When**: Khách cung cấp hoặc bỏ thiếu một trong hai tọa độ khi tạo.
- **Then**: Chỉ trường hợp có đủ tọa độ hợp lệ được lưu.
- **And**: Công trình lưu kinh độ và vĩ độ ngay khi tạo.

#### AC-007

- **Given**: Hồ sơ nhà thầu đang hiển thị có kinh độ và vĩ độ công ty đã được admin lưu.
- **When**: Khách xem vị trí công ty trên bản đồ trong hồ sơ.
- **Then**: Vị trí trên bản đồ dùng đúng cặp tọa độ đã lưu của công ty.
- **And**: Không yêu cầu khách đăng nhập chỉ để xem vị trí công ty; đăng nhập áp dụng khi chọn công trình để tìm theo bán kính.

#### AC-008

- **Given**: Chưa có nhà thầu nào đang hiển thị.
- **When**: Khách mở danh sách mà không truyền query lọc.
- **Then**: Trả danh sách rỗng.
- **And**: Không yêu cầu phải có sẵn nhà thầu mới cho khách mở danh sách.

#### AC-009

- **Given**: Hồ sơ nhà thầu đang hiển thị có mã số thuế do admin nhập.
- **When**: Khách chưa đăng nhập mở phần thông tin pháp nhân.
- **Then**: Mã số thuế được hiển thị đầy đủ, không che một phần.
- **And**: Người liên hệ, số điện thoại và email vẫn chỉ dành cho admin; bản scan hiển thị theo trạng thái chung của hồ sơ.

## References

### TDDs

- [TDD-CTR-002](../tdd/TDD-CTR-002.md)
- [TDD-SITE-002](../tdd/TDD-SITE-002.md)

### Rules

- BR-CTR-001
- BR-CTR-002
- BR-CTR-003
- BR-CTR-004
- BR-CTR-005
- BR-CTR-006
- BR-CTR-007

### Dependencies

- STORY-CTR-001: Hồ sơ nhà thầu và thông tin công khai.
- STORY-CTR-002: Dự án đã thực hiện.
- STORY-CTR-003: Danh mục phạm vi thi công.

## Non-Functional

- Đặc tả kiểm thử liên quan: [ST-CTR-025](../systemtest/ST-CTR-025.md), [ST-CTR-026](../systemtest/ST-CTR-026.md), [ST-CTR-027](../systemtest/ST-CTR-027.md), [ST-CTR-028](../systemtest/ST-CTR-028.md), [ST-CTR-029](../systemtest/ST-CTR-029.md), [ST-CTR-030](../systemtest/ST-CTR-030.md), [ST-CTR-031](../systemtest/ST-CTR-031.md), [ST-CTR-032](../systemtest/ST-CTR-032.md). Các ca chưa chạy.

- Kiểm tra quyền sử dụng công trình ở backend; không chỉ dựa vào công trình client gửi lên.
- Không cung cấp dữ liệu liên hệ nội bộ hoặc tài liệu thuộc hồ sơ đang Ẩn qua phản hồi công khai.
- STORY-SITE-001 và BR-SITE-001 đã bổ sung yêu cầu frontend xác định tọa độ từ địa chỉ và backend từ chối tạo nếu thiếu kinh độ hoặc vĩ độ. Đã bổ sung đặc tả ST-SITE-033 đến ST-SITE-037 cho tọa độ khi tạo và đổi địa chỉ. Đã có thiết kế TDD-SITE-002; API, code và fixture của bộ kiểm thử SITE cũ còn cần cập nhật. Người dùng xác nhận chưa có dữ liệu thật cần giữ, hoặc chỉ có dữ liệu thử. Chưa rà hết tham chiếu chéo ngoài phạm vi trực tiếp.
- TDD-CTR-002 đã chốt get all không phân trang, thứ tự ổn định, xử lý query lỗi và bao gồm điểm đúng đường biên. Chưa có chỉ tiêu hiệu năng được đo hoặc SLA; metadata phân công chưa xác định.
- Đã soạn đặc tả System Test theo nghiệp vụ được người dùng chốt; chưa chạy kiểm thử. Người dùng đã chốt TDD và đã soạn đặc tả Unit Test liên kết trong TDD; chưa triển khai mã ứng dụng hoặc chạy bộ test của tính năng.

## Out of Scope

- Mời báo giá, thêm vào so sánh, đặt lịch khảo sát hoặc hệ thống xếp hạng mức phù hợp.
- Tính khoảng cách theo đường đi và chọn vị trí hiện tại của thiết bị làm tâm thay cho công trình.
