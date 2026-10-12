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

# STORY-PROJ-012

## Metadata

- **Story**: Là Admin, tôi muốn cấu hình model AI, hạn chờ và giới hạn của luồng tạo dự toán, xem chi tiết các lần chạy của một bản dự toán và đặt lại bộ đếm lỗi, để vận hành luồng AI và hỗ trợ khách khi một chặng lỗi nhiều lần.
- **Context**: Luồng bốn chặng (BR-PROJ-010) gọi AI của OpenAI và Anthropic. Người dùng chốt ngày 11/10/2026: Admin đổi model và tham số chất lượng trên giao diện, prompt có phiên bản và phần lời dặn do Admin sửa ở STORY-PROJ-013 (BR-PROJ-014); hạn chờ từng chặng (BR-PROJ-012), giới hạn số lần lỗi (BR-PROJ-011) và hạn mức soi ảnh (BR-PROJ-015) do Admin cấu hình; Admin có nút đặt lại bộ đếm lỗi; cờ cần rà chỉ lưu nội bộ (BR-PROJ-017). Người dùng chốt ngày 12/10/2026: đặt lại bộ đếm lỗi và xem nhật ký AI thô dùng quyền vận hành riêng, mỗi lần xem nhật ký thô đều được ghi lại; Admin cấu hình thêm ngưỡng hàng chờ, tạm ngắt dịch vụ AI, ngưỡng cảnh báo chi phí (BR-PROJ-012) và trần soi ảnh chung (BR-PROJ-015).
- **Sprint**: [Chưa xác định]
- **Priority**: Must
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

- Người thao tác đã đăng nhập và có quyền tương ứng theo STORY-RBAC-001, gồm ba quyền tách riêng: quyền cấu hình luồng AI để xem và sửa cấu hình; quyền xem chi tiết luồng AI để xem lần chạy và phần nội bộ; quyền vận hành luồng AI để đặt lại bộ đếm lỗi và xem nhật ký AI thô. Vai trò Admin có cả ba quyền; nhân viên hỗ trợ có thể chỉ được cấp quyền xem và quyền vận hành.

### Trigger

Admin mở màn Cấu hình AI để xem hoặc sửa cấu hình; hoặc mở chi tiết luồng AI của một bản dự toán để xem lần chạy, xem nhật ký AI thô và đặt lại bộ đếm lỗi.

## Flow

### Main Flow

1. Admin mở màn Cấu hình AI.
2. Hệ thống hiển thị cấu hình hiện hành:
   - model và tham số chất lượng cho từng việc: soi ảnh lô, lập phương án 2D, vẽ bản vẽ, lập dự toán, dựng 3D;
   - hạn chờ của bốn chặng;
   - giới hạn số lần lỗi mỗi chặng;
   - hạn mức soi ảnh mỗi tài khoản mỗi ngày và trần soi ảnh của cả hệ thống mỗi ngày;
   - ngưỡng hàng chờ, ngưỡng tạm ngắt khi dịch vụ AI lỗi liên tục cùng thời gian tạm ngắt, và ngưỡng cảnh báo chi phí mỗi lần chạy;
   - bảng giá token của từng model để tính chi phí AI.
3. Admin sửa và bấm Lưu.
4. Backend kiểm tra quyền, phiên bản đang sửa và giá trị: model phải thuộc danh sách hệ thống hỗ trợ; tham số trong khoảng cho phép; hạn chờ, giới hạn và hạn mức là số dương.
5. Khi hợp lệ, lưu thành phiên bản cấu hình mới; áp dụng cho lần chạy và lần soi ảnh được tiếp nhận sau đó theo BR-PROJ-014.

### Alternative Flow

#### ALT-01

Admin xem chi tiết luồng AI của một bản dự toán.

1. Admin tìm bản dự toán theo mã `BUILDX-…` và mở chi tiết luồng AI.
2. Hệ thống hiển thị trạng thái từng chặng và danh sách lần chạy: thời điểm tiếp nhận và kết thúc, kết quả, mã lỗi, model, phiên bản prompt, phiên bản cấu hình, phiên bản luật giá, số token và chi phí AI ước tính.
3. Với chặng Dự toán, hệ thống hiển thị thêm phần nội bộ theo BR-PROJ-017: cờ cần rà, lý do và ghi chú chẩn đoán.

#### ALT-02

Admin đặt lại bộ đếm lỗi của một chặng.

1. Trong chi tiết luồng AI, chặng đang bị chặn vì đạt giới hạn có nút Đặt lại bộ đếm; nút chỉ hiện với người có quyền vận hành luồng AI.
2. Người thao tác bấm Đặt lại; backend kiểm tra quyền vận hành và ghi nhận người thao tác, thời điểm.
3. Khách chạy lại được chặng đó theo BR-PROJ-011 khoản 3. Hệ thống không tự chạy chặng.

#### ALT-03

Người có quyền vận hành xem nhật ký AI thô của một lời gọi AI.

1. Trong chi tiết luồng AI, người thao tác chọn một lời gọi AI và bấm Xem nhật ký thô.
2. Backend kiểm tra quyền vận hành, ghi lại người xem, thời điểm và lời gọi được xem, rồi mới trả nội dung: prompt đã gửi, tham số và phản hồi của AI. Ảnh trong nhật ký chỉ hiện thông tin nhận dạng, không hiện ảnh.
3. Nếu không ghi được dấu vết xem, hệ thống không trả nội dung.

### Exception Flow

#### EXC-01

Người thao tác không có quyền tương ứng.

1. Từ chối xem, sửa cấu hình, đặt lại bộ đếm hoặc xem nhật ký thô, kể cả yêu cầu gửi trực tiếp; không ghi thay đổi. Quyền xem chi tiết luồng AI không đủ để đặt lại bộ đếm hay xem nhật ký thô.

#### EXC-02

Giá trị cấu hình không hợp lệ, hoặc model nằm ngoài danh sách hỗ trợ.

1. Từ chối lưu và chỉ rõ trường gây lỗi; cấu hình hiện hành giữ nguyên.

#### EXC-03

Người khác đã lưu phiên bản cấu hình mới hơn trong lúc Admin đang sửa.

1. Từ chối lưu và báo cần tải lại; không ghi đè.

#### EXC-04

Admin đặt lại bộ đếm khi chặng đang chạy hoặc đã thành công.

1. Từ chối thao tác vì không có gì cần đặt lại; trạng thái chặng giữ nguyên.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền cấu hình luồng AI.
- **When**: Đổi model dựng 3D sang một model khác trong danh sách hỗ trợ và lưu.
- **Then**: Tạo phiên bản cấu hình mới; lần chạy 3D tiếp nhận sau đó dùng model mới.
- **And**: Lần chạy 3D đang diễn ra tiếp tục dùng cấu hình đã chốt lúc tiếp nhận.

#### AC-002

- **Given**: Admin nhập tên model không có trong danh sách hỗ trợ, hoặc hạn chờ bằng 0.
- **When**: Lưu cấu hình.
- **Then**: Từ chối và chỉ rõ trường gây lỗi.
- **And**: Cấu hình hiện hành giữ nguyên.

#### AC-003

- **Given**: Admin đổi giới hạn số lần lỗi từ 3 thành 5.
- **When**: Lưu thành công.
- **Then**: Chặng đang bị chặn vì đã lỗi 3 lần được khách chạy lại tiếp, tối đa tới 5 lần lỗi.
- **And**: Không tự chạy chặng nào.

#### AC-004

- **Given**: Chặng Dự toán của bản `BUILDX-20261011-Q7K2M9` đã lỗi 3 lần và đang bị chặn.
- **When**: Admin bấm Đặt lại bộ đếm.
- **Then**: Khách chạy lại được chặng Dự toán, tối đa bằng giới hạn đang cấu hình.
- **And**: Không cấp hay hoàn lượt; kết quả các chặng đã thành công giữ nguyên; người thao tác và thời điểm được ghi lại.

#### AC-005

- **Given**: Admin có quyền xem chi tiết luồng AI.
- **When**: Mở chi tiết luồng AI của một bản dự toán.
- **Then**: Thấy danh sách lần chạy từng chặng với kết quả, mã lỗi, model, các phiên bản đã dùng, số token và chi phí AI ước tính.
- **And**: Với chặng Dự toán thấy thêm cờ cần rà và ghi chú nội bộ.

#### AC-006

- **Given**: Khách là chủ bản dự toán.
- **When**: Gửi yêu cầu trực tiếp tới API chi tiết luồng AI hoặc API cấu hình.
- **Then**: Từ chối; khách không thấy model, phiên bản prompt, chi phí AI hay ghi chú nội bộ.
- **And**: Khách vẫn xem được trạng thái chặng và số lần chạy lại còn lại của bản dự toán của mình.

#### AC-007

- **Given**: Nhân viên hỗ trợ chỉ có quyền xem chi tiết luồng AI, không có quyền vận hành.
- **When**: Gửi yêu cầu đặt lại bộ đếm lỗi hoặc mở nhật ký AI thô của một bản dự toán.
- **Then**: Bị từ chối; bộ đếm giữ nguyên và không trả nội dung nhật ký thô.
- **And**: Nhân viên vẫn xem được danh sách lần chạy, mã lỗi, model và chi phí ước tính.

#### AC-008

- **Given**: Người có quyền vận hành mở nhật ký AI thô của một lời gọi lập phương án 2D.
- **When**: Hệ thống trả nội dung nhật ký.
- **Then**: Có một dòng ghi vết gồm người xem, thời điểm và mã lời gọi được xem.
- **And**: Mỗi lần mở lại tạo thêm một dòng ghi vết; dòng ghi vết không sửa hay xóa được.

#### AC-009

- **Given**: Admin có quyền cấu hình luồng AI.
- **When**: Đổi ngưỡng hàng chờ, ngưỡng và thời gian tạm ngắt, ngưỡng cảnh báo chi phí hoặc trần soi ảnh chung rồi lưu.
- **Then**: Tạo phiên bản cấu hình mới; giá trị mới áp dụng cho các yêu cầu chạy chặng và yêu cầu soi ảnh đến sau đó theo BR-PROJ-012 và BR-PROJ-015.
- **And**: Giá trị không phải số dương bị từ chối kèm trường gây lỗi.

## References

### TDDs

- TDD-PROJ-006
- TDD-PROJ-008

### Rules

- BR-PROJ-011
- BR-PROJ-012
- BR-PROJ-014
- BR-PROJ-015
- BR-PROJ-017

### Dependencies

- STORY-PROJ-002
- STORY-RBAC-001

## Non-Functional

- Kiểm tra quyền quản trị tại backend, không chỉ ẩn thao tác trên giao diện.
- Chi phí AI hiển thị là ước tính theo bảng giá token Admin nhập, không phải số liệu hóa đơn của nhà cung cấp.
- Khóa truy cập (API key) của OpenAI và Anthropic nằm trong biến môi trường của máy chủ; không hiển thị và không sửa trên giao diện.
- Nhật ký AI, gồm cả bản thô, được giữ vĩnh viễn, kể cả khi khách xóa tài khoản (người dùng chốt ngày 12/10/2026 và chấp nhận rủi ro pháp lý so với yêu cầu xóa dữ liệu cá nhân). Vì vậy quyền xem bản thô được giới hạn và mọi lần xem đều được ghi vết.

## Out of Scope

- Sửa prompt và bộ quy tắc AI trên giao diện; thuộc STORY-PROJ-013.
- Admin chạy chặng thay khách hoặc sửa kết quả của khách.
- Báo cáo tổng hợp chi phí AI theo ngày, tháng hoặc theo khách.
