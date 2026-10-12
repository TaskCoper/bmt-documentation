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

# STORY-PROJ-011

## Metadata

- **Story**: Là Admin, tôi muốn sửa luật giá và bảng miền giá theo tỉnh/thành trên giao diện, có lịch sử phiên bản, để dự toán AI dùng đúng mặt bằng giá hiện hành mà không cần sửa mã nguồn.
- **Context**: Chặng Dự toán (STORY-PROJ-008) dùng hai tài liệu luật giá của app demo `design-estimate` (`khung_gia_tham_khao.md`, `quy_tac_boc_tach.md`) và miền giá theo tỉnh/thành. App demo để luật trong tệp mã nguồn; hệ thống thật cho Admin sửa trên giao diện, giữ lịch sử phiên bản và tự tính lại dải giá (BR-PROJ-016, BR-PROJ-013). Người dùng chốt nghiệp vụ trong hội thoại ngày 11/10/2026.
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

- Người thao tác đã đăng nhập và có quyền quản lý luật giá theo STORY-RBAC-001; vai trò Admin có quyền này. Không mặc định mọi nhân viên có quyền.
- Hệ thống đã nạp sẵn phiên bản đầu tiên của hai tài liệu luật giá và bảng 34 tỉnh/thành theo BR-PROJ-016, BR-PROJ-013.

### Trigger

Admin mở màn Luật giá hoặc màn Miền giá theo tỉnh/thành để xem, sửa, xem trước, lưu hoặc khôi phục.

## Flow

### Main Flow

1. Admin mở màn Luật giá, chọn Khung giá tham khảo hoặc Quy tắc bóc tách.
2. Hệ thống hiển thị nội dung phiên bản hiện hành và danh sách phiên bản (người lưu, thời điểm, ghi chú).
3. Admin sửa nội dung. Với Khung giá tham khảo, Admin bấm Xem trước để thấy các dải giá sẽ được tính lại từ nội dung đang sửa, kèm so sánh với dải giá hiện hành.
4. Admin nhập ghi chú thay đổi và bấm Lưu.
5. Backend kiểm tra quyền, phiên bản đang sửa và nội dung theo BR-PROJ-016. Khi hợp lệ, lưu phiên bản mới cùng dải giá đã tính lại.
6. Phiên bản mới áp dụng cho lần chạy Dự toán được tiếp nhận sau đó; dự toán đã có giữ nguyên.

### Alternative Flow

#### ALT-01

Admin sửa bảng miền giá theo tỉnh/thành.

1. Admin mở màn Miền giá, thấy 34 tỉnh/thành cùng miền (Bắc, Trung, Nam). Các tỉnh Tây Nguyên thuộc miền Trung.
2. Admin đổi miền của một hay nhiều tỉnh/thành rồi lưu.
3. Backend kiểm tra quyền và dữ liệu; mỗi tỉnh/thành phải có đúng một miền.
4. Thay đổi áp dụng cho lần chạy 2D được tiếp nhận sau khi lưu theo BR-PROJ-013.

#### ALT-02

Admin khôi phục một phiên bản cũ của tài liệu luật giá.

1. Admin chọn phiên bản cũ, xem nội dung và bấm Khôi phục.
2. Hệ thống tạo phiên bản mới có cùng nội dung và dải giá tính lại từ nội dung đó; các phiên bản ở giữa giữ nguyên.

#### ALT-03

Admin so sánh hai phiên bản.

1. Admin chọn hai phiên bản; hệ thống hiển thị phần khác nhau giữa hai nội dung.

### Exception Flow

#### EXC-01

Người thao tác không có quyền quản lý luật giá.

1. Từ chối xem và sửa, kể cả yêu cầu gửi trực tiếp; không ghi thay đổi.

#### EXC-02

Nội dung Khung giá tham khảo không đọc được đơn giá ở mục 3–5, hoặc dải giá tính ra không hợp lệ.

1. Từ chối lưu, chỉ rõ mục hoặc dòng gây lỗi.
2. Phiên bản hiện hành giữ nguyên.

#### EXC-03

Người khác đã lưu phiên bản mới hơn trong lúc Admin đang sửa.

1. Từ chối lưu và báo cần tải lại nội dung mới nhất.
2. Không ghi đè phiên bản của người khác.

#### EXC-04

Bảng miền giá có tỉnh/thành thiếu miền.

1. Từ chối lưu và chỉ rõ tỉnh/thành cần bổ sung.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý luật giá.
- **When**: Mở màn Luật giá.
- **Then**: Thấy nội dung hiện hành của hai tài liệu và danh sách phiên bản gồm người lưu, thời điểm và ghi chú.
- **And**: Khách và nhân viên không có quyền không xem được nội dung luật giá hay dải giá.

#### AC-002

- **Given**: Admin đang sửa mục 3 của Khung giá tham khảo.
- **When**: Bấm Xem trước.
- **Then**: Thấy các dải giá sẽ được tính lại theo loại công trình và gói, so với dải giá hiện hành.
- **And**: Xem trước không lưu phiên bản và không ảnh hưởng lần chạy Dự toán nào.

#### AC-003

- **Given**: Nội dung Khung giá tham khảo hợp lệ.
- **When**: Admin lưu.
- **Then**: Tạo phiên bản mới gồm nội dung và dải giá đã tính lại; lần chạy Dự toán tiếp nhận sau đó dùng phiên bản này.
- **And**: Dự toán đã thành công trước đó giữ nguyên số liệu và phiên bản luật cũ.

#### AC-004

- **Given**: Admin xóa một dòng đơn giá cần thiết ở mục 4.
- **When**: Admin lưu.
- **Then**: Từ chối lưu và chỉ rõ mục, dòng gây lỗi.
- **And**: Phiên bản hiện hành và dải giá giữ nguyên.

#### AC-005

- **Given**: Hai Admin cùng mở phiên bản 5; Admin A lưu thành phiên bản 6.
- **When**: Admin B lưu nội dung sửa từ phiên bản 5.
- **Then**: Từ chối lưu của Admin B và báo cần tải lại.
- **And**: Phiên bản 6 của Admin A giữ nguyên.

#### AC-006

- **Given**: Đang có phiên bản 7; Admin chọn khôi phục phiên bản 4.
- **When**: Admin xác nhận khôi phục.
- **Then**: Tạo phiên bản 8 có nội dung của phiên bản 4 và dải giá tính lại từ nội dung đó.
- **And**: Phiên bản 5, 6, 7 vẫn còn trong lịch sử.

#### AC-007

- **Given**: Bảng miền giá đang xếp Lâm Đồng vào miền Trung.
- **When**: Admin đổi một tỉnh/thành sang miền khác và lưu.
- **Then**: Lần chạy 2D tiếp nhận sau đó dùng miền mới; bản dự toán đã chạy 2D trước đó giữ miền đã chốt.
- **And**: Lưu bảng có tỉnh/thành thiếu miền bị từ chối.

#### AC-008

- **Given**: Admin lưu Quy tắc bóc tách.
- **When**: Lưu thành công.
- **Then**: Tạo phiên bản mới của Quy tắc bóc tách; dải giá không đổi.
- **And**: Lần chạy Dự toán tiếp nhận sau đó ghi phiên bản mới của Quy tắc bóc tách.

## References

### TDDs

- TDD-PROJ-008

### Rules

- BR-PROJ-013
- BR-PROJ-014
- BR-PROJ-016

### Dependencies

- STORY-PROJ-008
- STORY-RBAC-001

## Non-Functional

- Kiểm tra quyền quản trị tại backend, không chỉ ẩn thao tác trên giao diện.
- Phiên bản nạp sẵn phải cho ra đúng 36 dải giá như app demo ở mốc `059e876`.

## Out of Scope

- Sửa prompt AI và bộ quy tắc AI trên giao diện; thuộc STORY-PROJ-013.
- Quản lý bảng đơn giá vật tư dạng bảng dữ liệu; luật giá vẫn là tài liệu văn bản như app demo.
- Thêm tài liệu luật giá thứ ba hoặc đổi cấu trúc mục của hai tài liệu.
