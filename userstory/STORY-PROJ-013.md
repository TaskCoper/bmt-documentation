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

# STORY-PROJ-013

## Metadata

- **Story**: Là Admin, tôi muốn quản lý bộ quy tắc AI (theo loại công trình, mái, gói, miền, lời dặn chung) và phần lời dặn của prompt trên giao diện, để điều chỉnh cách AI thiết kế và lập dự toán mà không cần sửa mã nguồn.
- **Context**: Bộ quy tắc AI và prompt được lưu thành dữ liệu có phiên bản theo BR-PROJ-019 và BR-PROJ-014, nạp sẵn từ app demo `design-estimate`. Trong 4 đợt đầu, mã nguồn đọc quy tắc từ dữ liệu nhưng chưa có màn sửa; Story này là đợt riêng sau đợt 4 (người dùng chốt ngày 11/10/2026). Phần cấu trúc dữ liệu AI phải trả và các thuật toán vẫn nằm trong mã nguồn.
- **Sprint**: [Chưa xác định]
- **Priority**: Should
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

- Người thao tác đã đăng nhập và có quyền quản lý quy tắc AI theo STORY-RBAC-001; vai trò Admin có quyền này.
- Bộ quy tắc và prompt đã được nạp sẵn theo BR-PROJ-019 khoản 6.

### Trigger

Admin mở màn Quy tắc AI hoặc màn Prompt để xem, sửa, xem trước, chạy thử, lưu hoặc khôi phục.

## Flow

### Main Flow

1. Admin mở màn Quy tắc AI và chọn một nhóm: theo loại công trình, ma trận mái, gói × chặng, theo miền, hoặc lời dặn chung.
2. Hệ thống hiển thị phiên bản hiện hành của nhóm đó theo dạng bảng, cùng danh sách phiên bản.
3. Admin sửa con số hoặc lời dặn, thêm hoặc tắt dòng lời dặn chung, rồi nhập ghi chú thay đổi và bấm Lưu.
4. Backend kiểm tra quyền, phiên bản đang sửa và dữ liệu theo BR-PROJ-019 khoản 3. Khi hợp lệ, lưu phiên bản mới.
5. Phiên bản mới áp dụng cho lần chạy chặng tiếp nhận sau đó; kết quả đã có giữ nguyên.

### Alternative Flow

#### ALT-01

Admin sửa phần lời dặn của một prompt.

1. Admin mở màn Prompt, chọn mã prompt (ví dụ lập phương án 2D, lập dự toán, dựng ảnh phòng).
2. Hệ thống hiển thị phần lời dặn có thể sửa; phần cấu trúc dữ liệu AI phải trả chỉ được xem, không sửa được.
3. Admin sửa lời dặn và bấm Xem trước để thấy prompt đã ghép đủ với một bộ đầu vào mẫu, gồm cả quy tắc và lời dặn chung khớp phạm vi.
4. Admin bấm Chạy thử: hệ thống gọi AI với prompt đang sửa trên bộ đầu vào mẫu, hiển thị kết quả và cho biết kết quả có đủ các trường bắt buộc hay không.
5. Admin lưu; hệ thống tạo phiên bản mới của prompt theo BR-PROJ-014 khoản 4.

#### ALT-02

Admin khôi phục hoặc so sánh phiên bản.

1. Admin chọn hai phiên bản để xem phần khác nhau, hoặc chọn một phiên bản cũ và bấm Khôi phục.
2. Khôi phục tạo phiên bản mới có nội dung của phiên bản cũ; không xóa phiên bản ở giữa.

#### ALT-03

Admin sửa ma trận mái cho một phong cách kiến trúc mới.

1. Admin chọn cặp (ID loại công trình, ID phong cách kiến trúc); giao diện hiển thị tên và chỉ cho chọn phong cách thuộc loại đó và chọn kiểu mái trong danh sách cố định.
2. Hệ thống kiểm tra mỗi cặp chỉ có một kiểu mái; cặp không có dòng riêng dùng kiểu mái mặc định của chính ID loại công trình đó.

#### ALT-04

Admin thêm chủ đề lời dặn theo miền, với chặng áp dụng do Admin chọn (người dùng chốt ngày 13/10/2026).

1. Admin mở nhóm Theo miền, chọn chặng 2D, Dự toán hoặc Phối cảnh 3D, chọn miền và nhập chủ đề/lời dặn.
2. Hệ thống kiểm dữ liệu và lưu thành phiên bản mới; không cần thêm chủ đề vào mã nguồn.
3. Các lần chạy tiếp nhận sau đó chỉ ghép lời dặn khi cả chặng và miền đều khớp. Chủ đề thời tiết đã có tiếp tục thuộc chặng 3D.

#### ALT-05

Admin cấu hình cho một loại công trình đã tồn tại nhưng chưa sẵn sàng cho AI.

1. Mở nhóm Theo loại công trình; thấy toàn bộ loại trong danh mục hiện hành cùng trạng thái và phần thiếu.
2. Chọn Cấu hình tại một loại; nhập quy tắc lô đất, mái mặc định và các mái theo phong cách cần dùng trong cùng form. Có thể chủ động sao chép lô đất và mái mặc định từ loại đã cấu hình rồi chỉnh lại; không tự kế thừa lúc chạy.
3. Nhập ghi chú và lưu. Hệ thống kiểm toàn bộ cấu hình rồi tạo một phiên bản; sai bất kỳ phần nào thì không lưu phần còn lại.
4. Khách thấy loại chưa đủ cấu hình nhưng không chọn được, kèm nhãn Chưa hỗ trợ. Sau khi đủ cấu hình và tải lại danh sách, khách chọn được. Các lỗi mã loại/phong cách hoặc mô tả nội thất được chỉ rõ để Admin sửa tại Danh mục công trình.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý quy tắc AI.

1. Từ chối xem và sửa, kể cả yêu cầu gửi trực tiếp; không ghi thay đổi.

#### EXC-02

Dữ liệu không hợp lệ: phạm vi trỏ tới mục không tồn tại, số ngoài khoảng, hoặc trùng phạm vi trong nhóm chỉ cho phép một dòng.

1. Từ chối lưu, chỉ rõ dòng và trường gây lỗi; phiên bản hiện hành giữ nguyên.

#### EXC-03

Người khác đã lưu phiên bản mới hơn trong lúc Admin đang sửa.

1. Từ chối lưu và báo cần tải lại; không ghi đè.

#### EXC-04

Chạy thử prompt cho kết quả thiếu trường bắt buộc, hoặc dịch vụ AI lỗi.

1. Hiển thị kết quả chạy thử và lỗi; vẫn cho Admin sửa tiếp.
2. Hệ thống cảnh báo trước khi lưu một prompt mà lần chạy thử gần nhất không đạt.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý quy tắc AI.
- **When**: Đổi kích thước lô mặc định của ID loại công trình Nhà phố từ 5×20 m thành 4×18 m và lưu.
- **Then**: Tạo phiên bản bộ quy tắc mới; lần chạy 2D tiếp nhận sau đó, với bản dự toán nhà phố không có kích thước lô từ khách, ảnh hay mô tả, dùng lô 4×18 m.
- **And**: Bản dự toán đã chạy 2D trước đó giữ kết quả và phiên bản quy tắc cũ.

#### AC-002

- **Given**: Admin thêm phong cách kiến trúc “Indochine” và gán ID phong cách này cho ID loại công trình Villa.
- **When**: Admin đặt kiểu mái cho cặp (Villa, Indochine) là mái ngói/mansard tân cổ điển và lưu.
- **Then**: Chặng Dự toán tính phần mái theo mái ngói và chặng 3D vẽ mái ngói cho bản dự toán chọn cặp này.
- **And**: Thêm dòng thứ hai cho cùng cặp ID bị từ chối; phong cách thuộc loại khác bị từ chối. Hai loại cùng mã gốc vẫn có quy tắc riêng.

#### AC-003

- **Given**: Admin thêm một dòng lời dặn chung với phạm vi chặng 3D, miền Trung: “Thể hiện mái đua sâu và máng xối chống bão”.
- **When**: Lưu và một bản dự toán ở miền Trung chạy chặng 3D.
- **Then**: Lời dặn được ghép vào prompt dựng ảnh của bản đó; bản ở miền khác không nhận lời dặn này.
- **And**: Tắt dòng thì lần chạy sau không ghép nữa; không cần sửa mã nguồn hay cấu trúc dữ liệu.

#### AC-004

- **Given**: Admin sửa lời dặn của prompt lập dự toán.
- **When**: Bấm Xem trước rồi Chạy thử trên bộ đầu vào mẫu.
- **Then**: Thấy prompt đã ghép đủ và kết quả chạy thử, kèm thông tin kết quả có đủ trường bắt buộc hay không.
- **And**: Chạy thử không tạo kết quả cho khách, không tính lượt; chi phí AI của lần chạy thử được ghi lại cho Admin xem.

#### AC-005

- **Given**: Admin mở prompt lập dự toán.
- **When**: Tìm cách sửa phần cấu trúc dữ liệu AI phải trả (các trường của công cụ `submit_estimate`).
- **Then**: Phần này chỉ được xem, không sửa được trên giao diện.
- **And**: Yêu cầu gửi trực tiếp nhằm sửa phần này bị từ chối.

#### AC-006

- **Given**: Đang có phiên bản 7 của bộ quy tắc.
- **When**: Admin khôi phục phiên bản 4.
- **Then**: Tạo phiên bản 8 có nội dung của phiên bản 4; phiên bản 5–7 vẫn còn.
- **And**: Lần chạy đang diễn ra tiếp tục dùng phiên bản đã chốt lúc tiếp nhận.

#### AC-007

- **Given**: Khách là chủ bản dự toán, hoặc nhân viên không có quyền quản lý quy tắc AI.
- **When**: Gửi yêu cầu trực tiếp tới API quy tắc AI hoặc prompt.
- **Then**: Từ chối; không trả nội dung quy tắc hay prompt.
- **And**: Không ghi thay đổi.

#### AC-008

- **Given**: Admin thêm chủ đề “Chống nóng”, chọn chặng 2D và miền Trung.
- **When**: Lưu rồi chạy các dự án mẫu ở miền Trung và miền Nam.
- **Then**: Prompt 2D của dự án miền Trung có lời dặn; prompt 2D miền Nam và prompt Dự toán/3D không nhận lời dặn này.
- **And**: Thêm chủ đề không cần sửa mã nguồn; trùng chặng, miền và chủ đề bị từ chối; kết quả đã có giữ nguyên.

#### AC-009

- **Given**: Một loại đã tồn tại trong danh mục dự toán nhưng chưa có quy tắc lô đất hoặc mái mặc định.
- **When**: Admin mở Quy tắc AI và khách mở danh sách loại công trình.
- **Then**: Admin thấy loại đó cùng phần thiếu và nút Cấu hình; khách thấy loại đó bị khóa chọn với nhãn Chưa hỗ trợ.
- **And**: Lưu đủ lô đất và mái trong một lần tạo đúng một phiên bản mới; lỗi, gửi lặp và xung đột phiên bản không gây lưu một phần hoặc mất quy tắc của loại khác. Khi danh sách được tải lại, loại đủ cấu hình được chọn.
- **And**: API từ chối lựa chọn mới chưa đủ cấu hình và chặn tiếp nhận 2D trước khi giữ lượt. Bản nháp cũ giữ dữ liệu; lần chạy đã nhận và kết quả cũ không thay đổi.

## References

### TDDs

- TDD-PROJ-012

- TDD-PROJ-008

### Rules

- BR-PROJ-014
- BR-PROJ-016
- BR-PROJ-019

### System Tests

- ST-PROJ-215

### Dependencies

- STORY-PROJ-012
- STORY-RBAC-001

## Non-Functional

- Kiểm tra quyền quản trị tại backend, không chỉ ẩn thao tác trên giao diện.
- Chạy thử dùng bộ đầu vào mẫu do nhóm dự án nạp sẵn, không dùng dữ liệu của khách.

## Out of Scope

- Sửa thuật toán (thứ tự ưu tiên kích thước lô, đo hình học, quyết định đủ chỗ phòng hoặc gara, lưới an toàn dự toán).
- Sửa phần cấu trúc dữ liệu AI phải trả.
- Thêm kiểu mái ngoài danh sách cố định của hệ thống, thêm mã gốc loại công trình mới.
- Quản lý luật giá (STORY-PROJ-011), miền giá theo tỉnh (STORY-PROJ-011) và cấu hình model (STORY-PROJ-012).
