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

# STORY-PAY-001

## Metadata

- **Story**: Là khách hàng, tôi muốn mua từng gói thiết kế hoặc giám sát bằng QR để nhận gói tự động khi thanh toán hợp lệ.
- **Context**: Thanh toán dùng SePay. Phạm vi gồm tạo đơn, QR, cộng dồn chuyển thiếu, hủy đơn và cấp gói. Các quyết định mới nhất thay thế phương án cũ không cộng dồn. Đây là bản nháp nghiệp vụ; chưa xác minh hợp đồng webhook hoặc thiết kế API.
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

- Khách có tài khoản để sở hữu đơn và gói. Danh mục đã có gói bán hợp lệ; fixture tích hợp SePay được thiết kế trước khi chạy test.

### Trigger

Khách chọn gói hoặc quay lại đơn đang chờ thanh toán.

## Flow

### Main Flow

1. Khách chọn một gói đang bán và chu kỳ tháng/năm nếu là thiết kế.
2. Hệ thống kiểm tra giới hạn đơn chờ, lưu giá/quyền lợi, tạo đơn bằng VNĐ và QR có thời hạn 15 phút.
3. Khách chuyển khoản theo QR; hệ thống nhận webhook SePay và nhận diện giao dịch của đơn.
4. Hệ thống ghi nhận khoản tiền một lần, xét thời điểm phát sinh và tổng tiền hợp lệ của đơn.
5. Khi đủ hoặc dư tiền trong hạn, hệ thống ghi nhận lần mua và cấp đúng một gói theo bản đã lưu. Với thiết kế, áp dụng thứ tự đủ tiền và thay kỳ cũ nếu đây là lần mua sau.
6. Khách thấy gói đã mua; giám sát có thể chưa gán công trình. Không cần nhân viên tiếp nhận.

### Alternative Flow

#### ALT-01

Giữ giá và quyền lợi của đơn.

1. Admin công bố bản B giá 2.500.000 đồng và 20 lượt; khách trả đủ giá đơn A.
2. Khách nhận bản A với 10 lượt.
3. Số tiền yêu cầu vẫn là 2.000.000 đồng; không ghép quyền lợi A/B.

#### ALT-02

Ngừng bán sau khi tạo đơn.

1. Admin ngừng bán gói; khách thanh toán đủ trong hạn của đơn cũ.
2. Đơn cũ được hoàn tất theo giá và quyền lợi đã lưu.
3. Tạo đơn mới cho gói ngừng bán bị chặn.

#### ALT-03

Thiếu tiền và QR phần còn thiếu.

1. SePay thông báo khoản tiền này.
2. Hiển thị đã nhận 500.000, còn thiếu 1.500.000 và yêu cầu chuyển thêm; chưa cấp gói.
3. QR cập nhật 1.500.000, giữ nội dung nhận diện đơn; hạn vẫn 10:15.

#### ALT-04

Chuyển dư ngay lần đầu.

1. Một giao dịch 2.100.000 đồng được ghi nhận trong hạn.
2. Cấp đúng một gói theo đơn.
3. 100.000 đồng dư xử lý bên ngoài nếu khách khiếu nại; không tự hoàn tiền.

#### ALT-05

Chuyển bổ sung vượt giá.

1. Khách chuyển thêm 1.600.000 đồng trong hạn.
2. Tổng 2.100.000 đồng đủ điều kiện và chỉ cấp một gói.
3. Không yêu cầu tổng tiền phải khớp chính xác; phần dư xử lý bên ngoài khi khiếu nại.

#### ALT-06

Webhook đến muộn.

1. Webhook đến 10:16 sau khi màn hình đã báo hết hạn.
2. Vẫn công nhận thanh toán đúng hạn và cấp gói.
3. Kỳ thiết kế bắt đầu lúc thực sự cấp gói, không tính lùi về 10:14.

#### ALT-07

Webhook trùng không cộng tiền hai lần.

1. Cùng giao dịch được gửi webhook hai lần.
2. Chỉ ghi nhận 1.000.000 đồng, vẫn thiếu 1.000.000, chưa cấp gói.
3. Không coi lần gửi webhook thứ hai là chuyển bổ sung.

#### ALT-08

Webhook đảo thứ tự các khoản bổ sung.

1. Webhook 1.500.000 đến trước; webhook 500.000 đến sau.
2. Sau khi nhận cả hai, công nhận đủ tiền tại 10:14 và cấp một gói.
3. Thời điểm đủ tiền không lấy theo webhook cuối cùng đến.

#### ALT-09

Khách hủy đơn chưa nhận tiền.

1. Khách hủy đơn rồi tạo đơn cho gói khác.
2. Hủy thành công và được tạo đơn mới.
3. Đơn mới dùng giá/quyền lợi tại lúc tạo; không tự cấp gói.

#### ALT-10

Đủ tiền trước hủy nhưng webhook muộn.

1. Webhook đến 10:07 và xác nhận giao dịch 10:05.
2. Công nhận lần mua hợp lệ dù đơn đã bị hủy.
3. Nếu có đơn thiết kế mua sau, áp dụng thứ tự thời điểm đủ tiền.

#### ALT-11

Hai đơn thiết kế và webhook đến ngược.

1. Webhook B đến và cấp B; sau đó webhook A đến.
2. Ghi nhận cả hai lần mua; B giữ hiệu lực, A không thay thế B.
3. Không tự đóng B hoặc hoàn tiền A chỉ vì webhook A đến sau.

#### ALT-12

Hủy đơn giám sát dùng chung quy tắc.

1. Khách lần lượt yêu cầu hủy từng đơn.
2. Cho hủy đơn chưa nhận tiền; từ chối hủy đơn đã nhận một phần.
3. Cả hai áp dụng hạn 15 phút và ngoại lệ đủ tiền trước hủy như thiết kế.

### Exception Flow

#### EXC-01

Giới hạn đơn thiết kế chờ.

1. Khách yêu cầu tạo thêm đơn thiết kế đang chờ.
2. Không tạo thêm đơn chờ cho khách.
3. Đơn cũ và số tiền đã nhận giữ nguyên.

#### EXC-02

Tiền thực tế về muộn.

1. Khách chuyển đủ lúc 10:16 và webhook đến.
2. Không tự cấp gói.
3. Nhân viên xử lý tiền bên ngoài; hệ thống không chuyển tiền hoặc đánh dấu hoàn tiền.

#### EXC-03

Bổ sung sau hạn.

1. Khoản 1.500.000 đồng tiếp theo phát sinh 10:16.
2. Không cấp gói dù tổng tiền thực tế đã bằng giá đơn.
3. Khoản bổ sung không gia hạn đơn; nhân viên xử lý bên ngoài.

#### EXC-04

Chặn hủy đơn nhận một phần.

1. Khách yêu cầu hủy.
2. Từ chối hủy và giữ khoản tiền đã ghi nhận.
3. Khách có thể trả phần thiếu trước hạn ban đầu; không kéo dài 15 phút.

#### EXC-05

Đủ tiền sau hủy.

1. Khách chuyển đủ lúc 10:06; webhook đến.
2. Không cấp gói dù giao dịch trước 10:15.
3. Nhân viên xử lý tiền bên ngoài.

#### EXC-06

Hai lần chuyển đủ cho cùng đơn.

1. Khách chuyển thêm một khoản đủ giá cho chính đơn đó.
2. Không cấp gói thứ hai và không làm mới thời hạn hoặc lượt.
3. Nhân viên xử lý hoàn khoản chuyển thêm bên ngoài, không ghi nhận hoàn tiền trong hệ thống.

#### EXC-07

Gói cũ giữ nguyên khi tiền chưa đủ.

1. Webhook thông báo khoản thiếu và đơn chưa đủ điều kiện.
2. Gói cũ vẫn giữ hiệu lực, quyền lợi và thời hạn.
3. Chưa cấp kỳ hoặc làm mới lượt từ việc tạo QR/nhận một phần tiền.

#### EXC-08

Tài khoản nhân viên gửi yêu cầu tạo hoặc hủy đơn mua gói, kể cả gửi trực tiếp tới API.

1. Hệ thống từ chối với mã 403 theo BR-RBAC-005, trước mọi thao tác ghi.
2. Không tạo đơn, không sinh QR và không đổi trạng thái đơn nào.
3. Nhân viên muốn dùng thử sản phẩm như khách phải dùng một tài khoản khách hàng riêng.

## Acceptance Criteria

#### AC-001

- **Given**: Khách chọn gói thiết kế tháng niêm yết 2.000.000 đồng.
- **When**: Khách tạo đơn.
- **Then**: Đơn chứa một gói, giá 2.000.000 VNĐ, không giảm giá hoặc cộng phí.
- **And**: QR được tạo; hạn chờ là 15 phút từ lúc tạo.

#### AC-002

- **Given**: Đơn đã tạo với bản A giá 2.000.000 đồng và 10 lượt; chưa hết hạn.
- **When**: Admin công bố bản B giá 2.500.000 đồng và 20 lượt; khách trả đủ giá đơn A.
- **Then**: Khách nhận bản A với 10 lượt.
- **And**: Số tiền yêu cầu vẫn là 2.000.000 đồng; không ghép quyền lợi A/B.

#### AC-003

- **Given**: Khách đã tạo đơn hợp lệ lúc gói còn bán.
- **When**: Admin ngừng bán gói; khách thanh toán đủ trong hạn của đơn cũ.
- **Then**: Đơn cũ được hoàn tất theo giá và quyền lợi đã lưu.
- **And**: Tạo đơn mới cho gói ngừng bán bị chặn.

#### AC-004

- **Given**: Khách có một đơn thiết kế chưa nhận tiền hoặc đã nhận một phần.
- **When**: Khách yêu cầu tạo thêm đơn thiết kế đang chờ.
- **Then**: Không tạo thêm đơn chờ cho khách.
- **And**: Đơn cũ và số tiền đã nhận giữ nguyên.

#### AC-005

- **Given**: Khách chưa có công trình, chọn cùng loại gói giám sát hai lần.
- **When**: Khách tạo hai đơn riêng và thanh toán đủ từng đơn.
- **Then**: Hai đơn được phép chờ đồng thời và cấp hai gói riêng.
- **And**: Không cần kiểm tra địa điểm/diện tích/phạm vi phục vụ hoặc nhân viên tiếp nhận.

#### AC-006

- **Given**: Đơn 2.000.000 đồng tạo 10:00; giao dịch 500.000 đồng lúc 10:05.
- **When**: SePay thông báo khoản tiền này.
- **Then**: Hiển thị đã nhận 500.000, còn thiếu 1.500.000 và yêu cầu chuyển thêm; chưa cấp gói.
- **And**: QR cập nhật 1.500.000, giữ nội dung nhận diện đơn; hạn vẫn 10:15.

#### AC-007

- **Given**: Đơn 2.000.000 đồng tạo 10:00 đã nhận 500.000 lúc 10:05.
- **When**: Khách chuyển tiếp 500.000 lúc 10:08 và 1.000.000 lúc 10:14; webhook hợp lệ đến.
- **Then**: Tổng 2.000.000 đồng đủ điều kiện; cấp gói một lần.
- **And**: Thời điểm đủ tiền là 10:14; không cần nhân viên xác nhận.

#### AC-008

- **Given**: Đơn giá 2.000.000 đồng còn hạn.
- **When**: Một giao dịch 2.100.000 đồng được ghi nhận trong hạn.
- **Then**: Cấp đúng một gói theo đơn.
- **And**: 100.000 đồng dư xử lý bên ngoài nếu khách khiếu nại; không tự hoàn tiền.

#### AC-009

- **Given**: Đơn 2.000.000 đồng đã nhận 500.000 đồng trong hạn.
- **When**: Khách chuyển thêm 1.600.000 đồng trong hạn.
- **Then**: Tổng 2.100.000 đồng đủ điều kiện và chỉ cấp một gói.
- **And**: Không yêu cầu tổng tiền phải khớp chính xác; phần dư xử lý bên ngoài khi khiếu nại.

#### AC-010

- **Given**: Đơn tạo 10:00; khách chuyển đủ lúc 10:14.
- **When**: Webhook đến 10:16 sau khi màn hình đã báo hết hạn.
- **Then**: Vẫn công nhận thanh toán đúng hạn và cấp gói.
- **And**: Kỳ thiết kế bắt đầu lúc thực sự cấp gói, không tính lùi về 10:14.

#### AC-011

- **Given**: Đơn tạo 10:00, không nhận tiền trước 10:15.
- **When**: Khách chuyển đủ lúc 10:16 và webhook đến.
- **Then**: Không tự cấp gói.
- **And**: Nhân viên xử lý tiền bên ngoài; hệ thống không chuyển tiền hoặc đánh dấu hoàn tiền.

#### AC-012

- **Given**: Đơn 2.000.000 đồng tạo 10:00, nhận 500.000 lúc 10:10.
- **When**: Khoản 1.500.000 đồng tiếp theo phát sinh 10:16.
- **Then**: Không cấp gói dù tổng tiền thực tế đã bằng giá đơn.
- **And**: Khoản bổ sung không gia hạn đơn; nhân viên xử lý bên ngoài.

#### AC-013

- **Given**: Đơn 2.000.000 đồng; một giao dịch thực tế 1.000.000 đồng trong hạn.
- **When**: Cùng giao dịch được gửi webhook hai lần.
- **Then**: Chỉ ghi nhận 1.000.000 đồng, vẫn thiếu 1.000.000, chưa cấp gói.
- **And**: Không coi lần gửi webhook thứ hai là chuyển bổ sung.

#### AC-014

- **Given**: Đơn 2.000.000 đồng tạo 10:00; giao dịch 500.000 lúc 10:05 và 1.500.000 lúc 10:14.
- **When**: Webhook 1.500.000 đến trước; webhook 500.000 đến sau.
- **Then**: Sau khi nhận cả hai, công nhận đủ tiền tại 10:14 và cấp một gói.
- **And**: Thời điểm đủ tiền không lấy theo webhook cuối cùng đến.

#### AC-015

- **Given**: Đơn thiết kế chưa ghi nhận tiền; gói đang bán.
- **When**: Khách hủy đơn rồi tạo đơn cho gói khác.
- **Then**: Hủy thành công và được tạo đơn mới.
- **And**: Đơn mới dùng giá/quyền lợi tại lúc tạo; không tự cấp gói.

#### AC-016

- **Given**: Đơn đã ghi nhận 500.000 trên giá 2.000.000 đồng.
- **When**: Khách yêu cầu hủy.
- **Then**: Từ chối hủy và giữ khoản tiền đã ghi nhận.
- **And**: Khách có thể trả phần thiếu trước hạn ban đầu; không kéo dài 15 phút.

#### AC-017

- **Given**: Đơn tạo 10:00, khách chuyển đủ 10:05, hủy lúc 10:06 khi chưa có webhook.
- **When**: Webhook đến 10:07 và xác nhận giao dịch 10:05.
- **Then**: Công nhận lần mua hợp lệ dù đơn đã bị hủy.
- **And**: Nếu có đơn thiết kế mua sau, áp dụng thứ tự thời điểm đủ tiền.

#### AC-018

- **Given**: Đơn tạo 10:00 và được hủy lúc 10:05 khi chưa nhận tiền.
- **When**: Khách chuyển đủ lúc 10:06; webhook đến.
- **Then**: Không cấp gói dù giao dịch trước 10:15.
- **And**: Nhân viên xử lý tiền bên ngoài.

#### AC-019

- **Given**: Đơn đã được trả đủ và cấp gói.
- **When**: Khách chuyển thêm một khoản đủ giá cho chính đơn đó.
- **Then**: Không cấp gói thứ hai và không làm mới thời hạn hoặc lượt.
- **And**: Nhân viên xử lý hoàn khoản chuyển thêm bên ngoài, không ghi nhận hoàn tiền trong hệ thống.

#### AC-020

- **Given**: Đơn A đủ tiền 10:14; sau khi A hiển thị hết hạn, khách tạo B và trả đủ 10:17.
- **When**: Webhook B đến và cấp B; sau đó webhook A đến.
- **Then**: Ghi nhận cả hai lần mua; B giữ hiệu lực, A không thay thế B.
- **And**: Không tự đóng B hoặc hoàn tiền A chỉ vì webhook A đến sau.

#### AC-021

- **Given**: Khách đang có kỳ thiết kế còn thời gian/lượt dư, có lượt đang giữ ở kỳ cũ.
- **When**: Khách thanh toán hợp lệ cho gói khác, chu kỳ khác hoặc mua lại cùng gói/cùng chu kỳ.
- **Then**: Kỳ mới bắt đầu lúc cấp gói, nhận đủ hạn mức; bỏ thời gian/lượt dư cũ, không khấu trừ tiền.
- **And**: Lượt đang giữ tiếp tục thuộc kỳ cũ; không có hai kỳ hiệu lực.

#### AC-022

- **Given**: Khách có đơn giám sát chưa nhận tiền và một đơn giám sát đã nhận một phần.
- **When**: Khách lần lượt yêu cầu hủy từng đơn.
- **Then**: Cho hủy đơn chưa nhận tiền; từ chối hủy đơn đã nhận một phần.
- **And**: Cả hai áp dụng hạn 15 phút và ngoại lệ đủ tiền trước hủy như thiết kế.

#### AC-023

- **Given**: Khách có gói thiết kế đang hiệu lực; tạo đơn mới rồi chuyển thiếu.
- **When**: Webhook thông báo khoản thiếu và đơn chưa đủ điều kiện.
- **Then**: Gói cũ vẫn giữ hiệu lực, quyền lợi và thời hạn.
- **And**: Chưa cấp kỳ hoặc làm mới lượt từ việc tạo QR/nhận một phần tiền.

#### AC-024

- **Given**: Đơn tạo 10:00:00, hết hạn 10:15:00, chưa có tiền.
- **When**: Nhận giao dịch đủ tiền phát sinh đúng 10:15:00.
- **Then**: Không cấp gói vì đúng mốc hết hạn không hợp lệ.
- **And**: Tiền vẫn được ghi nhận để nhân viên xử lý bên ngoài.

#### AC-025

- **Given**: Đơn tạo 10:00:00, hủy thành công 10:05:00, chưa ghi nhận tiền.
- **When**: Webhook báo giao dịch đủ tiền phát sinh đúng 10:05:00.
- **Then**: Không cấp gói vì giao dịch không phát sinh trước lúc hủy.
- **And**: Không thay quy tắc chỉ chấp nhận giao dịch trước hạn và trước lúc hủy.

#### AC-026

- **Given**: Hai đơn thiết kế đều hợp lệ và có cùng thời điểm đủ tiền theo độ chính xác SePay; B được tạo sau A.
- **When**: Xử lý webhook hai đơn ở các thứ tự khác nhau.
- **Then**: B được coi là lần mua sau và quyết định gói hiệu lực.
- **And**: Không lấy thứ tự webhook để phân xử.

#### AC-027

- **Given**: B đã đủ tiền 10:16; A được ghi nhận đủ tiền 10:17 và đã thay B.
- **When**: Nhận khoản cũ đến muộn chứng minh A thực ra đã đủ tiền 10:14.
- **Then**: Giữ A đang hiệu lực, không tự chuyển lại B.
- **And**: Ghi nhận lệch thứ tự để tra cứu; nhân viên xử lý bên ngoài, không tự hoàn tiền hoặc làm mới lượt.

#### AC-028

- **Given**: Nhân viên N đăng nhập bằng tài khoản nhân viên đang hoạt động; khách K có một đơn thiết kế đang chờ thanh toán; gói thiết kế đang bán.
- **When**: N gửi yêu cầu tạo đơn mua gói và yêu cầu hủy đơn của K, kể cả gửi trực tiếp tới API.
- **Then**: Hệ thống từ chối cả hai yêu cầu với mã 403.
- **And**: Không tạo đơn hay QR cho N; đơn của K giữ nguyên trạng thái chờ.

## References

### TDDs

- [TDD-PAY-001](../tdd/TDD-PAY-001.md): Thiết kế kỹ thuật bản nháp và đặc tả Unit Test liên quan.

### Rules

- BR-PAY-001/Then
- BR-PAY-002/Then
- BR-PAY-003/Then
- BR-PAY-004/Then
- BR-SUB-021/Then
- BR-RBAC-005/Then

### Dependencies

- [Quyết định, hiện trạng và các điểm còn mở](../discovery/payment-packages.md).

## Non-Functional

- Giữ các ràng buộc về số gói hiệu lực, quyền riêng và một lần ghi nhận/cấp gói cả khi yêu cầu lặp hoặc đồng thời. Cơ chế kỹ thuật và ca tích hợp chi tiết bổ sung trong TDD.
- Chưa chốt chỉ tiêu định lượng về hiệu năng hoặc thời gian phản hồi. Không tự đặt SLA cho webhook SePay.

## Out of Scope

- Giảm giá; phí cộng thêm; tự hoàn tiền; ghi nhận đã hoàn tiền; nhân viên xác nhận thủ công để cấp gói; API/schema và tích hợp SePay chi tiết; tự động gia hạn hoặc lưu phương thức trừ tiền định kỳ.
- Chưa triển khai, chạy test hoặc phê duyệt tài liệu. Sprint, Priority và người thực hiện chưa được phân công; không lấy ví dụ trong template làm giá trị thật.
