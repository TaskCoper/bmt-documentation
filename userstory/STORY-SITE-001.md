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

# STORY-SITE-001

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo và quản lý các công trình của mình để gắn gói giám sát đúng nơi cần giám sát.
- **Context**: Công trình là nơi thi công thật mà khách muốn được giám sát. Đây là thực thể riêng, khác với bản dự toán. Khách tự tạo công trình miễn phí, không cần gói thiết kế và không giới hạn số lượng. Mỗi công trình chỉ có tên và địa chỉ, không có trạng thái riêng. Gói giám sát được gắn cố định vào công trình theo STORY-SUB-004; nhân viên phụ trách theo từng gói. Người dùng xác nhận các quyết định này ngày 25/09/2026.
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

- Khách đã đăng nhập bằng tài khoản khách hàng theo BR-RBAC-005.
- Khách không cần có gói thiết kế hay gói giám sát để tạo công trình.

### Trigger

Khách mở danh sách công trình của mình, hoặc yêu cầu tạo, sửa hay xóa một công trình.

## Flow

### Main Flow

1. Khách chọn tạo công trình, nhập tên và địa chỉ.
2. Hệ thống bỏ khoảng trắng đầu và cuối của hai trường, rồi kiểm tra theo BR-SITE-001: cả hai có nội dung, tên tối đa 200 ký tự, địa chỉ tối đa 500 ký tự và tên chưa trùng với công trình khác của khách.
3. Hệ thống lưu công trình thuộc tài khoản của khách. Không thu phí và không trừ lượt của gói nào.
4. Công trình mới hiện trong danh sách công trình của khách và có thể được gắn gói giám sát theo STORY-SUB-004.

### Alternative Flow

#### ALT-01

Khách xem danh sách và chi tiết công trình của mình.

1. Khách mở danh sách hoặc chi tiết công trình.
2. Hệ thống chỉ trả công trình thuộc tài khoản của khách. Mỗi công trình có tên, địa chỉ và các gói giám sát đã gắn, kèm trạng thái gói theo BR-SITE-003.
3. Khách không thấy tên nhân viên phụ trách gói.

#### ALT-02

Khách sửa tên hoặc địa chỉ của công trình, kể cả khi công trình đã có gói.

1. Khách sửa tên, địa chỉ hoặc cả hai của một công trình thuộc mình.
2. Hệ thống kiểm tra lại theo BR-SITE-001. Tên mới không được trùng với công trình khác của khách.
3. Hệ thống lưu thông tin mới. Gói đã gắn và người phụ trách gói giữ nguyên theo BR-SITE-002.

#### ALT-03

Khách xóa công trình chưa từng có gói giám sát gắn vào.

1. Khách yêu cầu xóa công trình.
2. Hệ thống kiểm tra công trình chưa từng có gói giám sát gắn vào theo BR-SITE-002.
3. Hệ thống xóa công trình. Công trình không còn trong danh sách và không nhận gói giám sát được nữa.

### Exception Flow

#### EXC-01

Tên hoặc địa chỉ trống, chỉ có khoảng trắng, hoặc dài hơn giới hạn sau khi bỏ khoảng trắng đầu và cuối.

1. Hệ thống từ chối và chỉ rõ trường không hợp lệ.
2. Hệ thống không tự cắt ngắn và không lưu thay đổi.

#### EXC-02

Tên trùng với một công trình khác của cùng khách, không phân biệt chữ hoa, chữ thường.

1. Hệ thống từ chối yêu cầu tạo hoặc sửa.
2. Không tạo công trình mới; công trình đang sửa giữ tên cũ.

#### EXC-03

Khách xóa công trình từng có gói giám sát gắn vào, kể cả khi gói đó đã hoàn thành hoặc đang bị hủy.

1. Hệ thống từ chối.
2. Công trình và các gói đã gắn giữ nguyên.

#### EXC-04

Khách xem, sửa hoặc xóa công trình của khách khác, kể cả gửi yêu cầu trực tiếp tới API.

1. Hệ thống từ chối và không tiết lộ tên, địa chỉ hay gói của công trình đó.
2. Dữ liệu của công trình giữ nguyên.

## Acceptance Criteria

#### AC-001

- **Given**: Khách U1 chưa có công trình và chưa mua gói nào.
- **When**: U1 tạo công trình tên “Nhà phố Quận 7”, địa chỉ “12 Nguyễn Thị Thập, Quận 7, TP.HCM”.
- **Then**: Tạo thành công; công trình thuộc U1 và hiện trong danh sách công trình của U1.
- **And**: Không thu phí và không yêu cầu gói thiết kế hay gói giám sát.

#### AC-002

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 tạo công trình với tên “  Nhà vườn  ” và địa chỉ “  Củ Chi, TP.HCM  ”.
- **Then**: Hệ thống lưu tên “Nhà vườn” và địa chỉ “Củ Chi, TP.HCM”.
- **And**: Khoảng trắng ở giữa chuỗi được giữ nguyên.

#### AC-003

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 lần lượt gửi yêu cầu tạo với tên trống, tên chỉ có khoảng trắng, và địa chỉ trống.
- **Then**: Cả ba yêu cầu bị từ chối, mỗi lần chỉ rõ trường không hợp lệ.
- **And**: Không có công trình nào được tạo.

#### AC-004

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 tạo công trình có tên đúng 200 ký tự và địa chỉ đúng 500 ký tự sau khi bỏ khoảng trắng đầu và cuối.
- **Then**: Tạo thành công.
- **And**: Tên và địa chỉ được lưu đủ, không bị cắt.

#### AC-005

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 lần lượt tạo công trình có tên 201 ký tự, rồi công trình có địa chỉ 501 ký tự, tính sau khi bỏ khoảng trắng đầu và cuối.
- **Then**: Cả hai yêu cầu bị từ chối.
- **And**: Hệ thống không tự cắt ngắn để lưu.

#### AC-006

- **Given**: Khách U1 có công trình “Nhà Phố”.
- **When**: U1 tạo công trình tên “ nhà phố ”.
- **Then**: Từ chối vì trùng tên với công trình khác của U1.
- **And**: U1 vẫn chỉ có một công trình “Nhà Phố”.

#### AC-007

- **Given**: Khách U2 có công trình “Nhà phố”; khách U1 chưa có công trình nào tên như vậy.
- **When**: U1 tạo công trình tên “Nhà phố”.
- **Then**: Tạo thành công vì khách khác nhau được đặt trùng tên.

#### AC-008

- **Given**: Khách U1 đã có 20 công trình.
- **When**: U1 tạo thêm công trình tên “Kho Bình Dương”.
- **Then**: Tạo thành công; hệ thống không giới hạn số công trình của một khách.

#### AC-009

- **Given**: Khách U1 có công trình A đang có gói G1 ở trạng thái đã gán, do nhân viên N phụ trách, và công trình B chưa có gói; khách U2 có công trình C.
- **When**: U1 mở danh sách công trình.
- **Then**: U1 thấy A và B, không thấy C; A hiện gói G1 với trạng thái đã gán.
- **And**: Không hiện tên nhân viên N.

#### AC-010

- **Given**: Công trình A của U1 có gói G1 đang bị hủy; sau đó U1 gắn gói G2 vào A và G2 ở trạng thái đã gán.
- **When**: U1 xem chi tiết A.
- **Then**: U1 thấy G1 với trạng thái đang bị hủy và G2 với trạng thái đã gán.

#### AC-011

- **Given**: Công trình A của U1 có gói G1 đã gán, do nhân viên N phụ trách.
- **When**: U1 đổi tên A thành “Nhà phố mới” và đổi địa chỉ.
- **Then**: Lưu thành công tên và địa chỉ mới.
- **And**: G1 vẫn gắn với A và N vẫn phụ trách G1.

#### AC-012

- **Given**: Khách U1 có công trình A tên “Nhà phố” và công trình B tên “Nhà vườn”.
- **When**: U1 đổi tên B thành “NHÀ PHỐ”.
- **Then**: Từ chối vì trùng tên với A.
- **And**: B giữ tên “Nhà vườn”.

#### AC-013

- **Given**: Khách U1 có công trình A tên “nhà phố”.
- **When**: U1 đổi tên A thành “Nhà Phố”.
- **Then**: Lưu thành công vì tên chỉ được so với các công trình khác của U1, không so với chính A.

#### AC-014

- **Given**: Công trình B của U1 chưa từng có gói giám sát gắn vào.
- **When**: U1 xóa B.
- **Then**: Xóa thành công; B không còn trong danh sách của U1.
- **And**: Yêu cầu gắn gói giám sát vào B sau đó bị từ chối.

#### AC-015

- **Given**: Công trình A của U1 có gói G1 đã hoàn thành; công trình B từng có gói G2 gắn vào, nay G2 đang bị hủy và B không có gói giữ chỗ.
- **When**: U1 lần lượt yêu cầu xóa A và B.
- **Then**: Cả hai yêu cầu bị từ chối.
- **And**: A, B và các gói G1, G2 giữ nguyên.

#### AC-016

- **Given**: Công trình C thuộc khách U2.
- **When**: U1 xem chi tiết, sửa hoặc xóa C, kể cả gửi yêu cầu trực tiếp tới API.
- **Then**: Cả ba yêu cầu bị từ chối và không tiết lộ tên, địa chỉ hay gói của C.
- **And**: C giữ nguyên.

## References

### TDDs

- [TDD-SITE-001](../tdd/TDD-SITE-001.md): Bảng công trình, API tạo, xem, sửa, xóa của khách, kiểm trùng tên và điều kiện xóa.

### Rules

- BR-SITE-001/Then
- BR-SITE-002/Then
- BR-SITE-003/Then
- BR-RBAC-005/Then
- BR-RBAC-011/Then
- BR-SUB-022/Then
- BR-SUB-006/Then

### Dependencies

- STORY-SUB-004: Khách gắn gói giám sát vào công trình của mình.
- STORY-SITE-002: Nhân viên xem công trình theo quyền.

## Non-Functional

- Kiểm tra trùng tên và điều kiện xóa phải đúng cả khi có yêu cầu đồng thời. Hai yêu cầu tạo công trình trùng tên của cùng khách gửi cùng lúc thì chỉ một yêu cầu thành công. Yêu cầu xóa công trình và yêu cầu gắn gói vào chính công trình đó gửi cùng lúc không được để lại gói gắn vào công trình đã xóa. Cơ chế kỹ thuật thuộc TDD.
- Mọi yêu cầu được kiểm quyền ở server, kể cả yêu cầu gửi trực tiếp tới API, theo BR-RBAC-011.
- Chưa chốt chỉ tiêu hiệu năng, cách phân trang, thứ tự sắp xếp hay tìm kiếm trong danh sách công trình.

## Out of Scope

- Trạng thái công trình (đang thi công, đã xong); nhân viên tạo, sửa hoặc xóa hộ khách; hiện tên nhân viên phụ trách cho khách; địa chỉ tách tỉnh/thành, quận/huyện, phường/xã; gắn công trình với bản dự toán hoặc tạo công trình từ bản dự toán; giới hạn số công trình.
- Gắn gói giám sát vào công trình thuộc STORY-SUB-004; phân công nhân viên cho gói thuộc STORY-RBAC-003.
- Chưa có TDD và System Test; System Test được viết sau khi User Story và Business Rule được chốt. Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority, Creator và người thực hiện chưa được phân công.
