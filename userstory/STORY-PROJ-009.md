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

# STORY-PROJ-009

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo bộ ảnh phối cảnh 3D từ phương án 2D và dự toán đã có để hình dung ngoại thất và các phòng của ngôi nhà.
- **Context**: Chặng thứ ba trong bốn chặng (BR-PROJ-010). Backend tự dựng ảnh bằng AI (OpenAI) theo kế hoạch dựng của app demo `design-estimate` (commit `059e876`): ảnh ngoại thất ban ngày, ảnh ngoại thất ban đêm dựng từ ảnh ban ngày, và các ảnh phòng theo từng tầng. Khác app demo, prompt dùng cả hai nhóm phong cách: kiến trúc quyết định mái và ngoại thất; nội thất quyết định bảng màu, vật liệu và ảnh phòng (ô “Mô tả cho AI” của phong cách nội thất theo BR-PROJ-004). Chặng này không tính thêm lượt. Lần chạy thử ngày 11/10/2026 cho nhà phố 2 tầng ra 8 ảnh. Quyền Phối cảnh 3D trong gói đã tạm bỏ ngày 10/10/2026 (BR-SUB-008 khoản 9), nên chặng này không kiểm tra quyền 3D.
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

- Khách đã đăng nhập bằng tài khoản khách hàng và là chủ bản dự toán.
- Chặng Dự toán đã thành công theo STORY-PROJ-008.
- Chặng Phối cảnh 3D chưa thành công, không có chặng nào đang chạy và chưa đạt giới hạn số lần lỗi theo BR-PROJ-011.

### Trigger

Khách bấm Tạo phối cảnh 3D, hoặc bấm Chạy lại khi lần chạy trước bị lỗi.

## Flow

### Main Flow

1. Khách bấm Tạo phối cảnh 3D.
2. Backend kiểm tra quyền sở hữu và điều kiện mở chặng theo BR-PROJ-010. Không kiểm tra gói, lượt hay quyền 3D.
3. Backend tiếp nhận lần chạy và lập kế hoạch dựng từ phương án 2D đã lưu: ảnh ngoại thất ban ngày, ảnh ngoại thất ban đêm, và các ảnh phòng theo từng tầng. Với căn hộ, hai ảnh đầu là ảnh toàn cảnh căn hộ.
4. Hệ thống dựng từng ảnh dựa trên bản vẽ 2D, phong cách kiến trúc, phong cách nội thất, gói hoàn thiện và miền giá đã chốt. Ảnh ban đêm chỉ dựng sau khi có ảnh ban ngày.
5. Mỗi ảnh dựng xong được lưu ngay. Ảnh lỗi được dựng lại 1 lần theo BR-PROJ-011.
6. Trong lúc chạy, khách thấy tiến độ theo số ảnh đã xong trên tổng số ảnh và xem được các ảnh đã xong.
7. Khi mọi ảnh trong kế hoạch đã được dựng và lưu, chặng Phối cảnh 3D thành công. Chặng Hồ sơ chuyển sang Sẵn sàng; hệ thống không tự chạy tiếp.

### Alternative Flow

#### ALT-01

Lần chạy 3D lỗi khi đã dựng được một phần ảnh, chưa đạt giới hạn số lần lỗi.

1. Các ảnh đã dựng và lưu được giữ.
2. Khách bấm Chạy lại; hệ thống chỉ dựng các ảnh còn thiếu theo BR-PROJ-011 khoản 8, kể cả khi Admin đã đổi model hoặc lời dặn AI giữa hai lần chạy.
3. Khi đủ ảnh, quay lại bước 7 của Main Flow.

#### ALT-02

Gói thiết kế của khách đã hết hạn hoặc hết lượt sau khi 2D thành công.

1. Vẫn cho chạy chặng Phối cảnh 3D; không giữ, trừ hay hoàn lượt.

### Exception Flow

#### EXC-01

Chặng Dự toán chưa thành công, chặng 3D đã thành công, đang có chặng chạy, đã đạt giới hạn số lần lỗi, hệ thống đang quá tải hoặc dịch vụ AI đang tạm ngắt theo BR-PROJ-012, hoặc luồng bốn chặng đang tắt.

1. Từ chối yêu cầu trước khi tiếp nhận, kèm lý do; không tạo lần chạy và không tính vào bộ đếm lỗi.

#### EXC-02

Một ảnh vẫn lỗi sau 1 lần dựng lại, hoặc dịch vụ AI lỗi sau khi đã thử lại theo cấu hình.

1. Lần chạy lỗi. Tăng bộ đếm lỗi của chặng 3D khi một ảnh vẫn hỏng sau lần dựng lại; không tăng khi dịch vụ AI lỗi hoặc không phản hồi sau khi đã thử lại (BR-PROJ-011 khoản 1).
2. Giữ các ảnh đã dựng xong; không công bố chặng là thành công khi còn thiếu ảnh.

#### EXC-03

Lần chạy quá hạn chờ (mặc định 40 phút).

1. Lần chạy lỗi do quá hạn theo BR-PROJ-012, không tăng bộ đếm lỗi; ảnh đến sau hạn bị bỏ.
2. Ảnh đã lưu trước hạn được giữ cho lần chạy lại.

## Acceptance Criteria

#### AC-001

- **Given**: Chặng Dự toán đã thành công; chặng 3D chưa thành công và không có chặng nào đang chạy.
- **When**: Khách bấm Tạo phối cảnh 3D.
- **Then**: Backend tiếp nhận lần chạy và lập kế hoạch dựng gồm ảnh ngoại thất ban ngày, ảnh ngoại thất ban đêm và các ảnh phòng theo từng tầng của phương án 2D.
- **And**: Không kiểm tra gói, lượt hay quyền 3D.

#### AC-002

- **Given**: Bản dự toán nhà phố 2 tầng có phương án 2D và dự toán đã thành công.
- **When**: Chặng 3D chạy xong mọi ảnh trong kế hoạch.
- **Then**: Chặng 3D thành công; khách xem được toàn bộ ảnh kèm nhãn (ví dụ “Ngoại thất — Ban ngày”, “Tầng 1 — Phòng khách”).
- **And**: Chặng Hồ sơ chuyển sang Sẵn sàng; hệ thống không tự chạy Hồ sơ.

#### AC-003

- **Given**: Bản dự toán chọn phong cách kiến trúc và phong cách nội thất khác nhau.
- **When**: Hệ thống dựng ảnh.
- **Then**: Ảnh ngoại thất theo phong cách kiến trúc (mái, mặt tiền); ảnh phòng theo phong cách nội thất (bảng màu, vật liệu, đồ đạc) dùng ô “Mô tả cho AI” của phong cách nội thất.
- **And**: Kiểu mái trên ảnh khớp kiểu mái mà chặng Dự toán đã tính tiền.

#### AC-004

- **Given**: Kế hoạch có 8 ảnh; lần chạy dựng xong 6 ảnh thì ảnh thứ 7 lỗi cả 2 lần thử.
- **When**: Khách bấm Chạy lại.
- **Then**: Hệ thống chỉ dựng ảnh 7 và 8; 6 ảnh cũ giữ nguyên.
- **And**: Lần lỗi trước tính một lần vào bộ đếm lỗi của chặng 3D.

#### AC-005

- **Given**: Ảnh ngoại thất ban ngày chưa có.
- **When**: Hệ thống xử lý ảnh ngoại thất ban đêm.
- **Then**: Không dựng ảnh ban đêm trước ảnh ban ngày.
- **And**: Ảnh ban đêm dựng từ chính ảnh ban ngày đã lưu.

#### AC-006

- **Given**: Lần chạy 3D đang dựng ảnh.
- **When**: Khách mở màn Phối cảnh 3D.
- **Then**: Thấy tiến độ dạng số ảnh đã xong trên tổng số ảnh và xem được các ảnh đã xong.
- **And**: Chặng chưa được tính thành công cho tới khi đủ ảnh.

#### AC-007

- **Given**: Lần chạy 3D trước đã dựng xong 5/8 ảnh rồi lỗi; sau đó Admin đổi model dựng 3D.
- **When**: Khách bấm Chạy lại chặng 3D.
- **Then**: Hệ thống giữ 5 ảnh đã có và chỉ dựng 3 ảnh còn thiếu bằng model mới.
- **And**: Admin xem được từng ảnh được dựng bằng model nào.

## References

### TDDs

- TDD-PROJ-006
- TDD-PROJ-008
- TDD-PROJ-010

### Rules

- BR-PROJ-004
- BR-PROJ-010
- BR-PROJ-011
- BR-PROJ-012
- BR-PROJ-014
- BR-PROJ-019
- BR-SUB-008

### Dependencies

- STORY-PROJ-008

## Non-Functional

- Hạn chờ chặng 3D mặc định 40 phút, Admin cấu hình được (BR-PROJ-012).
- Ảnh phối cảnh khổ 16:9 như app demo (1280×720).
- Mỗi ảnh được lưu ngay khi dựng xong để gián đoạn chỉ mất tối đa một ảnh đang dựng dở.

## Out of Scope

- Khách chọn số lượng hay danh sách ảnh cần dựng, hoặc dựng thêm ảnh sau khi chặng đã thành công.
- Dựng lại một ảnh đã đạt vì khách không thích.
- Xem hoặc xoay mô hình 3D.
