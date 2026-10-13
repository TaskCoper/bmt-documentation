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

# STORY-PROJ-010

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo bộ hồ sơ từ các kết quả 2D, dự toán và 3D đã có để nhận tệp PDF, Excel và chia sẻ cho người khác.
- **Context**: Chặng cuối trong bốn chặng (BR-PROJ-010). Chặng này không gọi AI: backend dựng bốn tệp từ kết quả đã lưu của ba chặng trước, gồm PDF hồ sơ gộp khổ A3 ngang theo thứ tự trang của app demo `design-estimate` (`savico/core/pdf.py`), PDF phối cảnh 3D, PDF bản vẽ kiến trúc (bản vẽ kỹ thuật và bản vẽ nội thất) và Excel dự toán theo mẫu `template.xlsx` của app demo. Không có bản vẽ kết cấu. Chia sẻ và tải tệp chỉ mở sau khi chặng này thành công. Thay cho quyết định ngày 26/09/2026 “AI trả URL PDF/Excel, backend không tự dựng tệp”. Người dùng chốt nghiệp vụ trong hội thoại ngày 10–11/10/2026; ngày 13/10/2026 xác nhận thứ tự trang gom theo từng tầng như demo.
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
- Chặng Phối cảnh 3D đã thành công theo STORY-PROJ-009.
- Chặng Hồ sơ chưa thành công, không có chặng nào đang chạy và chưa đạt giới hạn số lần lỗi theo BR-PROJ-011.

### Trigger

Khách bấm Tạo hồ sơ, hoặc bấm Chạy lại khi lần chạy trước bị lỗi.

## Flow

### Main Flow

1. Khách bấm Tạo hồ sơ.
2. Backend kiểm tra quyền sở hữu và điều kiện mở chặng theo BR-PROJ-010. Không kiểm tra gói hay lượt.
3. Backend tiếp nhận lần chạy và dựng bốn tệp từ kết quả đã lưu:
   - PDF hồ sơ gộp, khổ A3 ngang, theo thứ tự: trang bìa; trang thông tin dự án kèm tóm tắt dự toán; các trang theo từng tầng (2D kỹ thuật → 2D nội thất → phối cảnh 3D của tầng đó), sau cùng là phối cảnh 3D ngoại thất; các trang bản vẽ và phối cảnh có khung tên; trang dự toán chi tiết; trang cảm ơn.
   - PDF phối cảnh 3D.
   - PDF bản vẽ kiến trúc, gồm bản vẽ kỹ thuật và bản vẽ bố trí nội thất của từng tầng.
   - Excel dự toán theo mẫu của app demo.
4. Thông tin in trên tệp: tên khách và số điện thoại lấy từ tài khoản của chủ bản dự toán; tên dự án là tên bản dự toán lúc dựng; mã hồ sơ là mã dự toán `BUILDX-…` (BR-PROJ-003 khoản 12); thông tin thương hiệu lấy từ cấu hình hệ thống. Nội dung dự toán chỉ dùng phần gửi khách theo BR-PROJ-017. PDF dự toán và Excel giữ Vật liệu/Hãng đã lưu, như app demo.
5. Tệp dựng lỗi được dựng lại 1 lần theo BR-PROJ-011.
6. Khi cả bốn tệp đã được dựng và lưu, chặng Hồ sơ thành công.
7. Khách thấy danh sách bốn tệp, tải được từng tệp và dùng được các thao tác chia sẻ link, QR, email theo STORY-PROJ-004.

### Alternative Flow

#### ALT-01

Lần chạy Hồ sơ lỗi, chưa đạt giới hạn số lần lỗi.

1. Khách bấm Chạy lại; hệ thống dựng lại các tệp chưa có.
2. Quay lại bước 6 của Main Flow khi đủ bốn tệp.

#### ALT-02

Gói thiết kế của khách đã hết hạn hoặc hết lượt sau khi 2D thành công.

1. Vẫn cho chạy chặng Hồ sơ; không giữ, trừ hay hoàn lượt.

#### ALT-03

Khách đổi tên bản dự toán sau khi chặng Hồ sơ đã thành công.

1. Tên tệp khi tải về theo tên hiện tại; nội dung tệp giữ tên tại lúc dựng theo BR-PROJ-007 khoản 7.
2. Không dựng lại tệp.

### Exception Flow

#### EXC-01

Chặng 3D chưa thành công, chặng Hồ sơ đã thành công, đang có chặng chạy, đã đạt giới hạn số lần lỗi, hệ thống đang quá tải theo BR-PROJ-012, hoặc luồng bốn chặng đang tắt.

1. Từ chối yêu cầu trước khi tiếp nhận, kèm lý do; không tạo lần chạy và không tính vào bộ đếm lỗi.

#### EXC-02

Một tệp vẫn lỗi sau 1 lần dựng lại, hoặc lần chạy quá hạn chờ (mặc định 5 phút).

1. Lần chạy lỗi. Chặng Hồ sơ không gọi AI nên lỗi dựng tệp và quá hạn đều là lỗi hệ thống, không tăng bộ đếm lỗi (BR-PROJ-011 khoản 1).
2. Không công bố bộ tệp còn thiếu; chia sẻ và tải tệp vẫn chưa mở.

#### EXC-03

Khách yêu cầu tải tệp hoặc tạo link chia sẻ khi chặng Hồ sơ chưa thành công.

1. Từ chối và báo cần hoàn tất chặng Hồ sơ trước.

## Acceptance Criteria

#### AC-001

- **Given**: Chặng 3D đã thành công; chặng Hồ sơ chưa thành công và không có chặng nào đang chạy.
- **When**: Khách bấm Tạo hồ sơ.
- **Then**: Backend tiếp nhận lần chạy và dựng bốn tệp: PDF hồ sơ gộp, PDF phối cảnh 3D, PDF bản vẽ kiến trúc và Excel dự toán.
- **And**: Không gọi AI, không kiểm tra gói, không giữ hay trừ lượt.

#### AC-002

- **Given**: Bản dự toán có kết quả 2D, dự toán và 3D đã lưu.
- **When**: Hệ thống dựng PDF hồ sơ gộp.
- **Then**: Tệp khổ A3 ngang có các trang theo thứ tự: bìa, thông tin dự án kèm tóm tắt dự toán; theo từng tầng là 2D kỹ thuật → 2D nội thất → phối cảnh 3D của tầng đó; sau các tầng là phối cảnh 3D ngoại thất; dự toán chi tiết; trang cảm ơn.
- **And**: Không có trang bản vẽ kết cấu. Bảng dự toán chi tiết của cả ba phần có cột Vật liệu/Hãng với nội dung từ kết quả đã lưu; không thay số liệu dự toán.

#### AC-003

- **Given**: Tài khoản của chủ bản dự toán có tên “Nguyễn Văn A” và số điện thoại; bản dự toán tên “Nhà phố Thủ Đức”, mã `BUILDX-20261011-Q7K2M9`.
- **When**: Hệ thống dựng các tệp hồ sơ.
- **Then**: Trang bìa và trang thông tin in tên khách, số điện thoại, tên dự án “Nhà phố Thủ Đức” và mã hồ sơ `BUILDX-20261011-Q7K2M9`.
- **And**: Nội dung dự toán trên PDF và Excel không chứa ghi chú nội bộ hay cờ cần rà theo BR-PROJ-017.

#### AC-004

- **Given**: Cả bốn tệp đã được dựng và lưu.
- **When**: Lần chạy kết thúc.
- **Then**: Chặng Hồ sơ thành công; khách thấy danh sách bốn tệp và tải được từng tệp.
- **And**: Các thao tác chia sẻ link, QR và email được mở theo STORY-PROJ-004.

#### AC-005

- **Given**: Chặng Hồ sơ chưa thành công, kể cả khi 2D, dự toán và 3D đã xong.
- **When**: Khách yêu cầu tải tệp hồ sơ hoặc tạo link chia sẻ, kể cả gửi trực tiếp tới backend.
- **Then**: Từ chối và báo cần hoàn tất chặng Hồ sơ trước.
- **And**: Không tạo link, không gửi email.

#### AC-006

- **Given**: Một tệp dựng lỗi ở lần đầu.
- **When**: Hệ thống xử lý tệp đó.
- **Then**: Dựng lại 1 lần; vẫn lỗi thì lần chạy lỗi hệ thống, không tăng bộ đếm lỗi của chặng Hồ sơ.
- **And**: Không công bố bộ tệp còn thiếu.

#### AC-007

- **Given**: Chặng Hồ sơ đã thành công với tên dự án “Phương án 3”.
- **When**: Khách đổi tên thành “Phương án chốt” rồi tải PDF.
- **Then**: Tệp tải về có tên tệp theo “Phương án chốt”; nội dung trong tệp giữ “Phương án 3”.
- **And**: Không dựng lại tệp, không tạo lần chạy mới.

## References

### TDDs

- TDD-PROJ-011
- TDD-PROJ-006
- TDD-PROJ-003

### Rules

- BR-PROJ-003/Then: Khoản 12, mã dự toán in trên hồ sơ.
- BR-PROJ-006
- BR-PROJ-007
- BR-PROJ-010
- BR-PROJ-011
- BR-PROJ-012
- BR-PROJ-017

### Dependencies

- STORY-PROJ-009
- STORY-PROJ-003
- STORY-PROJ-004

## Non-Functional

- Hạn chờ chặng Hồ sơ mặc định 5 phút, Admin cấu hình được (BR-PROJ-012).
- PDF dựng trực tiếp tại backend, không cần trình duyệt; font tiếng Việt được nhúng vào tệp. Bố cục bám mẫu của app demo nhưng không bắt buộc khớp từng điểm ảnh.
- Excel giữ cấu trúc sheet, cột và công thức của `template.xlsx` trong app demo.
- Tệp lưu ở kho với đường dẫn khó đoán; khách và người nhận link tải qua backend theo BR-PROJ-006.

## Out of Scope

- Bản vẽ kết cấu, bản vẽ điện nước.
- Khách chọn trang hoặc bố cục hồ sơ.
- Dựng lại tệp sau khi chặng đã thành công để cập nhật tên hay thông tin tài khoản.
- Dựng PDF ở trình duyệt của khách như giao diện hiện tại.
