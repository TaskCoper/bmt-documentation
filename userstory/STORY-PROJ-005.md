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

# STORY-PROJ-005

## Metadata

- **Story**: Là Admin, tôi muốn quản lý loại công trình, cấu hình tầng/tum và hai danh mục phong cách để khách có các lựa chọn phù hợp khi tạo dự toán mới.
- **Context**: Thay danh mục năm loại cố định và phần phong cách gộp trên trang mẫu bằng danh mục có thể quản lý. Cấu hình phong cách chỉ điều khiển lựa chọn đầu vào; AI vẫn trả đủ kết quả. Bản dự toán cũ giữ cấu hình đã áp dụng, kể cả bản nháp.
- **Sprint**: [Chưa xác định]
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

- Người thao tác đăng nhập và có quyền quản lý danh mục loại công trình và phong cách theo STORY-RBAC-001; vai trò Admin có quyền này. Không mặc định mọi nhân viên có quyền.
- Quy tắc cấu hình và hiệu lực đối với bản dự toán áp dụng BR-PROJ-004.

### Trigger

Admin thêm loại công trình, sửa cấu hình theo loại hoặc thêm/sửa phong cách kiến trúc và phong cách nội thất.

## Flow

### Main Flow

1. Admin mở phần quản lý danh mục loại công trình. Trước khi khách sử dụng, Admin tự nhập và gán danh mục phong cách ban đầu theo các bước dưới; không có bộ phong cách chuẩn bị sẵn từ trang mẫu.
2. Admin thêm loại công trình hoặc chọn loại đã có để sửa cấu hình.
3. Admin bật/tắt riêng trường chọn tầng và trường chọn có tum; cấu hình các số tầng được chọn cho loại đó.
4. Admin quản lý riêng danh mục phong cách kiến trúc và danh mục phong cách nội thất, gồm tên và bắt buộc một ảnh minh họa JPG/PNG/WebP tối đa 5 MB theo BR-PROJ-004.
5. Với từng loại công trình, Admin bật/tắt từng nhóm lựa chọn phong cách và gán các phong cách được chọn cho nhóm tương ứng.
6. Backend kiểm tra quyền và yêu cầu mỗi danh sách chọn tầng/phong cách được bật phải có ít nhất một lựa chọn phù hợp theo BR-PROJ-004. Chỉ lưu cấu hình hợp lệ và báo thành công khi đã lưu được. Ảnh phong cách phải đáp ứng BR-PROJ-004; tên loại công trình và phong cách phải có nội dung sau trim, tối đa 200 ký tự; cho phép trùng tên.
7. Bản dự toán mới áp dụng cấu hình mới. Bản dự toán đã tạo tiếp tục dùng danh mục và cấu hình đã áp dụng trước đó theo BR-PROJ-004; không được chọn loại công trình Admin thêm sau thời điểm tạo. Muốn dùng loại mới thêm, khách phải tạo bản dự toán mới.
8. Khi khách chuẩn bị gửi AI, mỗi nhóm phong cách được bật phải có đúng một lựa chọn hợp lệ. Nhóm bị tắt không yêu cầu chọn; AI vẫn trả đầy đủ kết quả.

### Alternative Flow

#### ALT-01

Admin sửa cấu hình khi đã có bản dự toán sử dụng cấu hình trước đó.

1. Lưu thay đổi để áp dụng cho bản dự toán mới.
2. Giữ cấu hình và lựa chọn của các bản dự toán đã tạo, kể cả bản nháp, bản dự toán đang chạy AI, đã thất bại hoặc đã thành công.
3. Khi khách mở lại hoặc gửi/thử lại AI, dùng cấu hình của bản dự toán đó để kiểm tra lựa chọn; vẫn kiểm tra quyền, gói/lượt và khóa sửa theo các quy tắc hiện hành.

### Exception Flow

#### EXC-01

Người yêu cầu không có quyền quản lý danh mục loại công trình và phong cách.

1. Từ chối thao tác quản trị, kể cả yêu cầu gửi trực tiếp.
2. Không ghi thay đổi từ yêu cầu bị từ chối.

#### EXC-02

Lưu cấu hình hoặc ảnh minh họa thất bại.

1. Không báo dữ liệu chưa lưu là đã lưu thành công.
2. Không làm thay đổi cấu hình đã áp dụng cho bản dự toán cũ vì lỗi này.
3. Cách giữ cấu hình hiện hành khi cập nhật nhiều phần hoặc tải ảnh lỗi sẽ được xác định trong thiết kế; chưa cam kết đã có cơ chế lưu đồng thời toàn bộ thay đổi.

#### EXC-03

Admin lưu cấu hình có danh sách chọn tầng hoặc nhóm phong cách được bật nhưng không có lựa chọn.

1. Từ chối lưu và báo rõ danh sách cần thêm ít nhất một lựa chọn phù hợp.
2. Không lưu cấu hình không hợp lệ; nếu đang sửa, giữ cấu hình đã lưu trước đó.
3. Áp dụng cả khi thêm cấu hình và khi bỏ lựa chọn cuối cùng khỏi danh sách đang bật; danh sách bị tắt không bắt buộc có lựa chọn theo điều kiện này.

## Acceptance Criteria

#### AC-001

- **Given**: Admin có quyền quản lý danh mục.
- **When**: Thêm một loại công trình và lưu thành công.
- **Then**: Loại đó có thể được dùng cho bản dự toán mới theo cấu hình của nó, không bị giới hạn bởi danh sách năm loại ban đầu.
- **And**: Các quy tắc chung của bản dự toán vẫn áp dụng.

#### AC-002

- **Given**: Admin cấu hình tầng/tum cho một loại công trình.
- **When**: Bật/tắt từng trường và cấu hình danh sách số tầng rồi lưu thành công.
- **Then**: Bản dự toán mới dùng đúng việc bật/tắt riêng và danh sách tầng của loại đó.
- **And**: Không quyết định có trường tầng/tum chỉ bằng tên loại công trình như Căn hộ.

#### AC-003

- **Given**: Admin quản lý phong cách kiến trúc và nội thất.
- **When**: Thêm/sửa tên, ảnh minh họa và gán phong cách theo loại công trình thành công.
- **Then**: Hai danh mục được cung cấp riêng; bản dự toán mới chỉ chọn phong cách thuộc đúng nhóm và loại áp dụng.
- **And**: Một giá trị phong cách chung không tự được dùng thay hai lựa chọn.

#### AC-004

- **Given**: Loại công trình bật một hoặc cả hai nhóm lựa chọn phong cách.
- **When**: Khách gửi AI từ bản dự toán dùng cấu hình đó.
- **Then**: Mỗi nhóm được bật phải có đúng một phong cách hợp lệ; nhóm bị tắt không yêu cầu chọn.
- **And**: Cấu hình lựa chọn không cắt phần kiến trúc, nội thất, dự toán hoặc hồ sơ khỏi bộ kết quả AI.

#### AC-005

- **Given**: Đã có bản dự toán dùng cấu hình cũ, kể cả bản dự toán còn là bản nháp.
- **When**: Admin sửa cấu hình loại công trình hoặc phong cách và lưu thành công.
- **Then**: Bản dự toán cũ giữ cấu hình đã áp dụng và lựa chọn đã lưu; chỉ bản dự toán mới dùng thay đổi.
- **And**: Kiểm tra lựa chọn của bản dự toán cũ khi lưu hoặc gửi/thử lại AI không buộc khách tuân theo cấu hình mới; quyền và lượt vẫn được kiểm tra ở thời điểm yêu cầu.

#### AC-006

- **Given**: Người thao tác không có quyền quản lý danh mục, hoặc thao tác lưu bị lỗi.
- **When**: Backend phản hồi yêu cầu cập nhật.
- **Then**: Không báo đã lưu khi chưa lưu thành công; yêu cầu trái quyền không ghi thay đổi.
- **And**: Cấu hình đã áp dụng cho bản dự toán cũ không bị thay đổi bởi yêu cầu này.

#### AC-007

- **Given**: Admin bật chọn tầng, phong cách kiến trúc hoặc phong cách nội thất cho một loại công trình.
- **When**: Lưu cấu hình có ít nhất một danh sách đang bật nhưng không có lựa chọn phù hợp, kể cả sau khi bỏ lựa chọn cuối cùng.
- **Then**: Từ chối lưu, chỉ rõ danh sách cần bổ sung và yêu cầu ít nhất một lựa chọn cho từng danh sách đang bật.
- **And**: Không ghi cấu hình không hợp lệ hoặc thay cấu hình đã lưu trước đó; danh sách bị tắt không bắt buộc có lựa chọn theo điều kiện này.

#### AC-008

- **Given**: Admin thêm hoặc sửa một phong cách kiến trúc hoặc nội thất.
- **When**: Lưu thông tin phong cách.
- **Then**: Phong cách phải có một ảnh minh họa JPG/PNG/WebP tối đa 5 MB; thiếu ảnh, sai định dạng hoặc vượt dung lượng thì từ chối lưu.
- **And**: Không dùng giới hạn ảnh đầu vào bản dự toán để thay thế quy tắc ảnh danh mục; yêu cầu không hợp lệ không thay dữ liệu đã lưu. Ghi chú kỹ thuật, không đổi tiêu chí: định dạng và dung lượng ảnh do frontend kiểm trước khi tải ảnh lên qua presign; backend kiểm có ảnh và chỉ kiểm URL ảnh là URL https thuộc tên miền được phép trong `UploadedFileOption__AllowedHosts` (TDD-PROJ-001).

#### AC-009

- **Given**: Hệ thống được chuẩn bị để khách sử dụng tính năng Tạo dự toán.
- **When**: Admin thiết lập danh mục phong cách ban đầu.
- **Then**: Admin tự nhập tên, ảnh minh họa và gán phong cách kiến trúc/nội thất theo loại công trình; khách dùng danh mục và cấu hình đã được lưu hợp lệ.
- **And**: Không tự tạo bộ phong cách từ danh sách gộp trên trang mẫu; mỗi nhóm đang bật phải có ít nhất một lựa chọn trước khi lưu cấu hình theo BR-PROJ-004.

#### AC-010

- **Given**: Admin thêm hoặc sửa loại công trình, phong cách kiến trúc hoặc phong cách nội thất.
- **When**: Lưu tên danh mục.
- **Then**: Bỏ khoảng trắng đầu/cuối và yêu cầu tên có nội dung, tối đa 200 ký tự; từ chối tên rỗng, chỉ có khoảng trắng hoặc vượt giới hạn.
- **And**: Cho phép trùng tên; không tự cắt ngắn tên hoặc thay mục khác vì trùng tên.

## References

### TDDs

- TDD-PROJ-001
- TDD-PROJ-002: Kiểm lựa chọn phong cách theo cấu hình khi gửi AI (AC-004).
- TDD-LIB-001: Thư viện mẫu dùng chung danh mục loại công trình.

### Rules

- BR-PROJ-004

### Dependencies

## Non-Functional

- Kiểm tra quyền quản trị tại backend, không chỉ ẩn thao tác trên giao diện.
- Phải giữ được cấu hình đã áp dụng cho bản dự toán cũ khi danh mục thay đổi. Phương án lưu, xử lý cập nhật đồng thời và thời điểm chọn cấu hình cho bản dự toán sẽ được mô tả trong thiết kế kỹ thuật.
- Tên và ảnh danh mục có giới hạn theo BR-PROJ-004. Schema/API, xác thực ảnh, phân quyền và cập nhật đồng thời được đề xuất trong TDD-PROJ-001; chưa triển khai hoặc chạy kiểm thử.

## Out of Scope

- Xây giao diện Admin trong tác vụ chỉ lập kế hoạch backend và tài liệu này.
- Thu hẹp bộ kết quả AI theo việc bật/tắt lựa chọn phong cách; tự tạo phong cách mặc định cho nhóm bị tắt.
- Xóa/ngừng dùng danh mục hoặc chuyển bản dự toán cũ sang cấu hình mới: chưa được xác nhận, không tự bổ sung thao tác.
- Chuẩn bị sẵn hoặc tự phân loại danh sách phong cách gộp trên trang mẫu thành danh mục khởi tạo. Admin tự nhập và cấu hình danh mục ban đầu theo quyết định đã chốt.
