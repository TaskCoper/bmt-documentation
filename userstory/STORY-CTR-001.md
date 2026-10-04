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

# STORY-CTR-001

## Metadata

- **Story**: Là admin, tôi muốn tạo, cập nhật, xóa và quản lý hiển thị hồ sơ nhà thầu để công khai thông tin doanh nghiệp, năng lực và hồ sơ hợp tác cho khách.
- **Context**: Hồ sơ gồm thông tin doanh nghiệp, địa chỉ và tọa độ, năng lực nhận thi công, hoạt động, đánh giá do admin nhập, pháp lý và hợp tác BuildX. Chỉ admin quản lý trong giai đoạn hiện tại. Khi đưa lên web thì coi hồ sơ đã được xác minh; không có xác minh riêng từng thành phần. Nghiệp vụ đã được người dùng chốt trong hội thoại; chưa triển khai.
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

- Người thao tác đã đăng nhập với quyền admin.
- Danh mục loại công trình dùng chung và phạm vi thi công có dữ liệu để chọn.

### Trigger

Admin mở danh sách quản trị nhà thầu để tạo, cập nhật, xóa hoặc thay đổi trạng thái hiển thị của một hồ sơ.

## Flow

### Main Flow

1. Admin nhập tên công ty. Admin có thể bổ sung ngay hoặc sau khi lưu: địa chỉ tách tỉnh/thành, phường/xã, số nhà–đường, kinh độ, vĩ độ, các loại công trình và phạm vi thi công nhà thầu nhận làm.
2. Admin nhập các phần tùy chọn: giới thiệu, thông tin và ảnh doanh nghiệp, khu vực phục vụ, thời gian khảo sát, nhận dự án, bảo hành, điểm và số lượt đánh giá.
3. Admin có thể bổ sung thông tin pháp nhân, giấy phép, cam kết/bảo hành, bảo hiểm công trình và hợp tác theo BR-CTR-004. Ảnh và bản scan mở cho người có URL; khi ẩn hồ sơ, URL đã chia sẻ vẫn đọc được. Người liên hệ, số điện thoại và email dành cho admin.
4. Hệ thống lưu hồ sơ mới với trạng thái mặc định Ẩn. Hồ sơ chưa được công khai sau bước tạo.
5. Khi admin bật Hiển thị sau đó, hệ thống yêu cầu đủ các trường tại BR-CTR-002.
6. Hồ sơ được đưa lên web thì coi là đã xác minh; không xác minh riêng hoặc tự yêu cầu xác minh lại khi sửa. Mã số thuế đã nhập hiển thị đầy đủ theo BR-CTR-004.

### Alternative Flow

#### ALT-01

Admin sửa thông tin một hồ sơ đã có.

1. Admin mở hồ sơ và cập nhật các thông tin cần thay đổi.
2. Hệ thống lưu thay đổi theo cùng quy tắc; không phát sinh quy trình xác minh lại.

#### ALT-02

Admin ngừng công khai hồ sơ.

1. Admin đổi trạng thái tổng thể để hồ sơ không hiển thị cho khách.
2. Hồ sơ không còn xuất hiện trong danh sách và trang chi tiết công khai.

#### ALT-03

Admin xóa nhà thầu.

1. Admin yêu cầu xóa một nhà thầu.
2. Hệ thống kiểm tra quyền và điều kiện chưa có lời mời báo giá theo BR-RFQ-006. Nếu đã có lời mời thì từ chối xóa và admin có thể ẩn hồ sơ. Nếu đủ điều kiện, hệ thống xóa hồ sơ; nhà thầu không còn xuất hiện trong danh sách hoặc trang chi tiết công khai.
3. Các dự án, thông tin pháp lý và hợp tác thuộc nhà thầu cũng được xóa khỏi hồ sơ quản trị và trang công khai. Danh mục dùng chung vẫn giữ nguyên.

### Exception Flow

#### EXC-01

Người thao tác không có quyền admin.

1. Từ chối thao tác tạo, cập nhật hoặc xóa; không ghi thay đổi.

#### EXC-02

Hồ sơ thiếu trường bắt buộc khi đưa lên web.

1. Chỉ rõ phần còn thiếu và không công khai hồ sơ chưa đủ thông tin.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý và các danh mục đã có dữ liệu.
- **When**: Admin đưa hồ sơ có tên, địa chỉ, kinh độ, vĩ độ, ít nhất một loại công trình và một phạm vi thi công lên web.
- **Then**: Hồ sơ được công khai và coi là đã xác minh.
- **And**: Giới thiệu, ảnh doanh nghiệp, pháp lý và hợp tác có thể bổ sung sau.

#### AC-002

- **Given**: Hồ sơ chưa đủ một trong các trường bắt buộc.
- **When**: Admin yêu cầu đưa hồ sơ lên web.
- **Then**: Yêu cầu không được công khai; hệ thống chỉ rõ phần thiếu.
- **And**: Không tự thêm thông tin giả để hồ sơ đủ điều kiện.

#### AC-003

- **Given**: Một hồ sơ đang được hiển thị.
- **When**: Admin sửa thông tin hoặc bản scan.
- **Then**: Thông tin được cập nhật mà không yêu cầu xác minh lại.
- **And**: Không tạo trạng thái xác minh riêng cho phần vừa sửa.

#### AC-004

- **Given**: Admin quản lý điểm và số lượt đánh giá của nhà thầu.
- **When**: Admin lưu điểm từ 0 đến 5 với tối đa một chữ số thập phân và số lượt là số nguyên không âm.
- **Then**: Hệ thống lưu và dùng hai giá trị admin nhập để hiển thị đánh giá.
- **And**: Không đòi hỏi có các lượt đánh giá của khách trong phạm vi này.

#### AC-005

- **Given**: Người dùng không có quyền admin.
- **When**: Người dùng yêu cầu tạo, sửa hoặc xóa hồ sơ.
- **Then**: Từ chối và không ghi thay đổi.
- **And**: Nhà thầu tự cập nhật bằng tài khoản riêng là phạm vi tương lai.

#### AC-006

- **Given**: Admin chỉ nhập tên cho hồ sơ mới, chưa nhập địa chỉ, tọa độ hoặc danh mục năng lực.
- **When**: Admin lưu hồ sơ mới thành công.
- **Then**: Hồ sơ mặc định Ẩn và chưa xuất hiện trên trang công khai.
- **And**: Admin có thể bật Hiển thị sau; việc tạo hồ sơ không tự bật Hiển thị.

#### AC-007

- **Given**: Admin có quyền quản lý một nhà thầu chưa có lời mời báo giá.
- **When**: Admin xóa nhà thầu thành công.
- **Then**: Nhà thầu không còn xuất hiện trong danh sách và trang chi tiết công khai.
- **And**: Các dự án, thông tin pháp lý và hợp tác thuộc hồ sơ cũng bị xóa; danh mục dùng chung vẫn giữ nguyên. Quyền xóa được kiểm tra ở backend.

#### AC-008

- **Given**: Hồ sơ nhà thầu chưa được admin nhập đánh giá.
- **When**: Khách xem hồ sơ công khai.
- **Then**: Không hiển thị đánh giá.
- **And**: Không tự tạo điểm hoặc số lượt đánh giá thay cho dữ liệu chưa nhập.

#### AC-009

- **Given**: Admin cập nhật đánh giá nhà thầu.
- **When**: Điểm nhỏ hơn 0, lớn hơn 5, có hơn một chữ số thập phân, hoặc số lượt âm hay không phải số nguyên.
- **Then**: Từ chối lưu và chỉ rõ giá trị không hợp lệ.
- **And**: Giữ nguyên đánh giá đã lưu trước đó.

#### AC-010

- **Given**: Admin tạo hoặc sửa nhà thầu.
- **When**: Admin chọn tỉnh/thành hợp lệ và lưu hồ sơ.
- **Then**: Hệ thống lưu mã tỉnh và xác định miền theo BR-CTR-008.
- **And**: Có thể bổ sung tỉnh sau; nhà thầu cũ chưa có tỉnh vẫn giữ thông tin và trạng thái hiển thị. Không nhập miền độc lập với tỉnh.

#### AC-011

- **Given**: Nhà thầu đã có tỉnh hợp lệ.
- **When**: Admin gửi mã tỉnh ngoài danh mục hỗ trợ.
- **Then**: Hệ thống từ chối và giữ dữ liệu đã lưu.
- **And**: Không suy đoán tỉnh từ địa chỉ hoặc tự gán miền mặc định.

#### AC-012

- **Given**: Admin bổ sung địa chỉ công ty cho nhà thầu.
- **When**: Admin chọn tỉnh/thành, chọn phường/xã thuộc tỉnh và nhập số nhà–đường.
- **Then**: Hệ thống lưu riêng các phần địa chỉ, mã và tên tỉnh/phường đã xác minh cùng phiên bản danh mục.
- **And**: Địa chỉ đầy đủ để hiển thị được ghép từ ba phần; miền vẫn xác định từ mã tỉnh.

#### AC-013

- **Given**: Admin gửi phường/xã không thuộc tỉnh đã chọn.
- **When**: Hệ thống xử lý lần lưu.
- **Then**: Từ chối và chỉ rõ lựa chọn không hợp lệ; không ghi một phần thay đổi.
- **And**: Form đổi tỉnh phải bỏ lựa chọn phường cũ và dùng nguồn địa chỉ chung với dự toán/công trình.

#### AC-014

- **Given**: Có hồ sơ nhà thầu cũ chỉ lưu địa chỉ dạng chữ.
- **When**: Triển khai phần địa chỉ tách riêng và đọc hồ sơ cũ.
- **Then**: Giữ nguyên địa chỉ, tọa độ và trạng thái đã lưu; không tự suy đoán phường hoặc số nhà–đường.
- **And**: Khi sửa trường khác, giữ địa chỉ đã lưu; chuyển sang ba phần bằng bộ địa chỉ đầy đủ. Chỉ hồ sơ Hidden được xóa toàn bộ địa chỉ theo TDD-CTR-003.

## References

### TDDs

- [TDD-CTR-003](../tdd/TDD-CTR-003.md): bổ sung địa chỉ ba phần đã được người dùng chốt.

- [TDD-CTR-001](../tdd/TDD-CTR-001.md)

### Rules

- [BR-CTR-009](../businessrule/BR-CTR-009.md): địa chỉ ba phần; giữ riêng các đề xuất chưa chốt.

- [BR-CTR-008](../businessrule/BR-CTR-008.md)

- BR-CTR-001
- BR-RFQ-006/Then: Chặn xóa nhà thầu đã có lời mời; vẫn được ẩn.
- BR-CTR-002
- BR-CTR-004
- BR-CTR-007

### Dependencies

- STORY-CTR-002: Quản lý các dự án của nhà thầu.
- STORY-CTR-003: Cấu hình danh mục phạm vi thi công.

## Non-Functional

- Địa chỉ ba phần được người dùng yêu cầu ngày 2026-10-04. Các chi tiết tương thích và điều kiện lưu ở TDD-CTR-003/BR-CTR-009 đã được người dùng chốt và triển khai trong workspace. Kết quả kiểm backend nằm ở TDD-CTR-003/References; ST-CTR-041 đến ST-CTR-046 chưa được chạy đầy đủ qua giao diện.

- Kiểm thử bổ sung cho tỉnh/miền: [ST-CTR-035](../systemtest/ST-CTR-035.md), [ST-CTR-036](../systemtest/ST-CTR-036.md). Đặc tả chưa chạy.

- Bổ sung tỉnh/thành và AC-010, AC-011 đã được người dùng chốt trong hội thoại; System Test được bổ sung tại ST-CTR-033 đến ST-CTR-040. Chưa triển khai mã ứng dụng.

- Đặc tả kiểm thử liên quan: [ST-CTR-001](../systemtest/ST-CTR-001.md), [ST-CTR-002](../systemtest/ST-CTR-002.md), [ST-CTR-003](../systemtest/ST-CTR-003.md), [ST-CTR-004](../systemtest/ST-CTR-004.md), [ST-CTR-005](../systemtest/ST-CTR-005.md), [ST-CTR-006](../systemtest/ST-CTR-006.md), [ST-CTR-007](../systemtest/ST-CTR-007.md), [ST-CTR-008](../systemtest/ST-CTR-008.md), [ST-CTR-009](../systemtest/ST-CTR-009.md), [ST-CTR-010](../systemtest/ST-CTR-010.md). Các ca chưa chạy.

- Quyền quản trị được kiểm tra tại backend; không chỉ ẩn nút trên giao diện.
- Thiết kế phải cho phép bổ sung tài khoản nhà thầu tự cập nhật hồ sơ trong giai đoạn sau. Giai đoạn hiện tại chỉ admin có quyền tạo và cập nhật; chưa chốt cách liên kết tài khoản hoặc phân quyền cho giai đoạn sau.
- Giới hạn trường/tệp và xử lý cập nhật đồng thời theo TDD-CTR-001 đã được người dùng chốt. Metadata phân công vẫn chưa xác định; chưa phê duyệt trên hệ thống tài liệu.
- Đã soạn đặc tả System Test theo nghiệp vụ được người dùng chốt; chưa chạy kiểm thử. Người dùng đã chốt TDD và đã soạn đặc tả Unit Test liên kết trong TDD; chưa triển khai mã ứng dụng hoặc chạy bộ test của tính năng.

## Out of Scope

- Mời báo giá, so sánh, tài khoản nhà thầu tự cập nhật và khách gửi đánh giá.
- Xác minh từng thành phần, tự hủy xác minh khi sửa, lịch sử kiểm duyệt hoặc quy trình duyệt nhiều bước.
