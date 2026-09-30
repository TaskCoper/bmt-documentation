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

# STORY-LIB-001

## Metadata

- **Story**: Là người có quyền quản lý thư viện, tôi muốn quản lý mẫu và công bố từng phiên bản để cung cấp bản vẽ tham khảo cho khách.
- **Context**: Nội dung xem của mẫu 2D/3D cần được chia thành các section content có tên, mỗi section chứa một hoặc nhiều file. Sửa tại chỗ không tạo phiên bản; công bố phiên bản mới tách khỏi thao tác sửa. Mẫu dùng chung danh mục dự toán để tìm mẫu tham khảo phù hợp; mẫu 3D chọn nhiều phong cách kiến trúc và nội thất theo loại công trình.
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

- Người thao tác đăng nhập và có quyền quản lý thư viện mẫu theo STORY-RBAC-001; quyền này không gắn phân công.

### Trigger

Người quản lý tạo mẫu, sửa hoặc chuẩn bị phiên bản mới.

## Flow

### Main Flow

1. Nhập tên, loại 2D/3D, kích thước và phân loại dùng chung theo BR-LIB-001; thêm mô tả nếu có. Tạo các section content, tự nhập tên từng section; tạo hoặc chọn section cụ thể trước khi tải lên một hoặc nhiều file vào section đó. Tất cả ảnh và tệp PDF/DWG/DXF đều nằm trong section; không có bước upload file rời rồi phân nhóm sau. Chọn ảnh đại diện từ các ảnh của mẫu và lưu thứ tự ảnh. Cả 2D và 3D chọn loại công trình, tầng và tum theo cấu hình. Riêng 3D chọn nhiều phong cách kiến trúc và nội thất từ các mục được gán cho loại công trình trong danh mục dự toán; nhóm tắt không cho chọn.
2. Cho phép lưu nháp thiếu thông tin để nhập dần; dữ liệu đã nhập vẫn phải hợp lệ. Trước khi công bố, kiểm mẫu có section, mỗi section có tên và ít nhất một file, tất cả file nội dung đã được gắn vào section, cùng các điều kiện còn lại tại BR-LIB-001. Với 3D, mỗi nhóm phong cách đang bật phải có ít nhất một lựa chọn hợp lệ; công bố mẫu hợp lệ để khách tìm thấy.
3. Người quản lý được thay đổi và lưu thứ tự section trong bản nháp hoặc phiên bản hiện tại. Khi thêm section vào phiên bản hiện tại, nhập tên rồi upload file; section chỉ quản trị thấy tới khi file đầu tiên được lưu thành công. Khi cần đính chính phiên bản hiện tại, dùng Sửa; lưu thay đổi vào cùng phiên bản, không cấp phiên bản mới.
4. Khi cần phát hành bản mới, tạo bản nháp riêng, chỉnh sửa và Công bố phiên bản mới; bản trước khóa sửa và được giữ cho lịch sử.
5. Ẩn hoặc hiện lại mẫu khi cần; chỉ xóa bản nháp chưa công bố.

### Alternative Flow

#### ALT-01

Danh mục hoặc cấu hình tầng, tum, phong cách đã thay đổi.

1. Giữ phân loại cũ khi sửa nội dung khác; thay phân loại hoặc công bố phiên bản mới phải dùng lựa chọn hợp lệ hiện hành.

### Exception Flow

#### EXC-01

Thiếu quyền hoặc nội dung công bố không hợp lệ.

1. Từ chối thao tác, không báo thành công và không thay phiên bản đang phục vụ bằng dữ liệu lỗi. Upload thiếu section hợp lệ phải bị từ chối trước khi cho tải file lên, kể cả khi đang nhập bản nháp.

## Acceptance Criteria

#### AC-001

- **Given**: Người quản lý có mẫu đủ dữ liệu
- **When**: Công bố
- **Then**: Mẫu xuất hiện với nội dung theo BR-LIB-001; thiếu tên, ảnh, kích thước hoặc phân loại bắt buộc thì từ chối công bố. Điều kiện section và file khi công bố kiểm theo AC-015.

#### AC-002

- **Given**: Phiên bản hiện tại đã có người xem
- **When**: Sửa tên, phân loại, section, ảnh hoặc tệp hợp lệ
- **Then**: Giữ mã phiên bản và quyền xem; lịch sử phản ánh nội dung đã sửa, không tính thêm lượt.

#### AC-003

- **Given**: Có phiên bản đang công bố
- **When**: Chuẩn bị rồi công bố bản nháp mới
- **Then**: Trong lúc chuẩn bị khách vẫn xem bản hiện tại; sau công bố thư viện hiển thị bản mới, bản trước giữ nguyên và không sửa được.

#### AC-004

- **Given**: Mẫu đã có người trả lượt
- **When**: Ẩn mẫu rồi mở lại lịch sử
- **Then**: Mẫu không nhận lượt mới; người đã có quyền vẫn xem/tải được. Hiện lại không tạo phiên bản.

#### AC-005

- **Given**: Có bản nháp và bản đã công bố
- **When**: Yêu cầu xóa từng bản
- **Then**: Chỉ xóa bản nháp; từ chối xóa bản đã công bố.

#### AC-006

- **Given**: Loại công trình bật tầng/tum
- **When**: Công bố mẫu
- **Then**: Chọn đúng một loại, một số tầng, một giá trị tum; trường tắt là Không áp dụng.

#### AC-007

- **Given**: Nhập kích thước và tài nguyên
- **When**: Kiểm tra nội dung
- **Then**: Ba kích thước bắt buộc, dương, tối đa hai số lẻ; diện tích độc lập. Chỉ nhận định dạng đã chốt, không tự áp giới hạn nghiệp vụ số lượng/dung lượng.

#### AC-008

- **Given**: Người quản lý tạo hoặc sửa phân loại mẫu 3D; loại công trình đã có cấu hình phong cách trong danh mục dự toán.
- **When**: Chọn phong cách kiến trúc và phong cách nội thất.
- **Then**: Cho phép chọn nhiều mục trong từng nhóm, dùng lại ID của danh mục có sẵn; chỉ nhận mục đúng nhóm và được gán cho loại công trình theo cấu hình áp dụng.
- **And**: Không tạo danh mục phong cách riêng cho thư viện; từ chối lựa chọn sai nhóm hoặc không được gán cho loại công trình.

#### AC-009

- **Given**: Mẫu 3D có ít nhất một nhóm phong cách đang bật.
- **When**: Lưu nháp hoặc công bố mẫu.
- **Then**: Nháp được thiếu lựa chọn nhưng dữ liệu đã nhập phải hợp lệ; khi công bố, mỗi nhóm đang bật phải có ít nhất một phong cách hợp lệ.
- **And**: Thiếu phong cách của bất kỳ nhóm bắt buộc nào thì từ chối công bố, không thay phiên bản đang phục vụ.

#### AC-010

- **Given**: Mẫu 2D, hoặc mẫu 3D có nhóm phong cách bị tắt theo cấu hình loại công trình.
- **When**: Lưu phân loại mẫu.
- **Then**: Mẫu 2D không gắn phong cách kiến trúc/nội thất; mẫu 3D không cho gắn phong cách thuộc nhóm bị tắt, và không yêu cầu lựa chọn của nhóm đó khi công bố.

#### AC-011

- **Given**: Mẫu 3D đã công bố có các phong cách hợp lệ ở phiên bản danh mục đã áp dụng; Admin thay đổi danh mục hoặc cấu hình sau đó.
- **When**: Sửa tên, ảnh hoặc tệp; sửa phân loại; hoặc công bố phiên bản mẫu mới.
- **Then**: Chỉ sửa nội dung khác thì giữ phân loại cũ; sửa phân loại hoặc công bố phiên bản mới phải kiểm lựa chọn phong cách theo cấu hình hiện hành, cùng nguyên tắc với tầng/tum.

#### AC-012

- **Given**: Người quản lý đang nhập nội dung một mẫu 2D.
- **When**: Lưu các section “Tầng trệt”, “Tầng 2” và “Tầng 3”, mỗi section có một file nội dung hợp lệ.
- **Then**: Khi đọc lại nội dung quản trị, hệ thống trả đúng ba section, tên từng section và file thuộc section đó.
- **And**: Không gộp các file thành một danh sách làm mất thông tin section.

#### AC-013

- **Given**: Người quản lý đang nhập nội dung một mẫu 3D.
- **When**: Lưu section “Phòng khách” có hai file và section “Góc sofa” có ba file nội dung hợp lệ.
- **Then**: Khi đọc lại, “Phòng khách” có đúng hai file và “Góc sofa” có đúng ba file đã gắn; tên section được lưu riêng với tên từng file.
- **And**: Một section được chứa nhiều file, không phải tạo một section cho mỗi file.

#### AC-014

- **Given**: Người quản lý đang nhập nội dung một mẫu 2D hoặc 3D.
- **When**: Nhập tên section và gắn ảnh hoặc tệp PDF/DWG/DXF hợp lệ vào nội dung mẫu.
- **Then**: Tên section được tự nhập cho mẫu đó, không bắt chọn từ danh mục chung; mỗi file nội dung thuộc đúng một section của phiên bản.
- **And**: Không tạo danh sách tệp đính kèm riêng ở cấp mẫu; ảnh đại diện được chọn từ các ảnh trong section của mẫu.

#### AC-015

- **Given**: Bản nháp thiếu section hoặc có section thiếu tên hay chưa có file.
- **When**: Lưu nháp rồi yêu cầu công bố.
- **Then**: Cho lưu nháp thiếu thông tin theo BR-LIB-001/Except nhưng từ chối công bố cho đến khi mẫu có section, mỗi section có tên và ít nhất một file hợp lệ, tất cả file nội dung thuộc section.
- **And**: Không thay phiên bản đang phục vụ khi công bố bị từ chối.

#### AC-016

- **Given**: Người quản lý muốn upload ảnh hoặc tệp PDF/DWG/DXF cho mẫu 2D hoặc 3D, kể cả bản nháp.
- **When**: Yêu cầu upload không chỉ rõ section, hoặc section không tồn tại, thuộc mẫu/phiên bản khác hay không được phép sửa.
- **Then**: Từ chối trước khi cho upload; không nhận file rời ở cấp mẫu và không hẹn phân nhóm sau.
- **And**: Chỉ cho upload khi đã chọn section hợp lệ của phiên bản đang sửa; file được gắn vào đúng section đó.

#### AC-017

- **Given**: Người có quyền quản lý thư viện đang sửa bản nháp hoặc phiên bản hiện tại của mẫu 2D/3D có nhiều section.
- **When**: Thay đổi thứ tự section, lưu rồi đọc lại nội dung.
- **Then**: Màn quản trị và màn xem chi tiết có quyền truy cập hiển thị các section đúng thứ tự đã lưu; tên, các file thuộc từng section và thứ tự file bên trong section không đổi.
- **And**: Đổi thứ tự không tạo phiên bản mới hoặc tính thêm lượt. Người thiếu quyền quản lý hoặc yêu cầu sửa thứ tự của phiên bản đã bị thay thế bị từ chối, thứ tự đã lưu giữ nguyên.

#### AC-018

- **Given**: Người có quyền quản lý đang sửa phiên bản hiện tại của mẫu 2D/3D; khách đã có quyền xem phiên bản này.
- **When**: Thêm section có tên rồi upload file vào section đó.
- **Then**: Trước khi file đầu tiên được lưu thành công, chỉ quản trị thấy section. Sau khi lưu thành công, khách đọc lại thấy section và file theo đúng thứ tự đã lưu, không cần công bố phiên bản mới.
- **And**: Upload đang chạy, lỗi hoặc bị hủy không làm lộ section chuẩn bị; quyền xem và lượt của khách giữ nguyên.

## References

### TDDs

- TDD-LIB-001
- TDD-LIB-002: Đọc/preview và bảo vệ tài nguyên; phối hợp khóa với tra cứu.

### Rules

- BR-LIB-001
- BR-LIB-002
- BR-PROJ-004

### Dependencies

- STORY-PROJ-005: Danh mục dùng chung và cấu hình phong cách theo loại công trình.
- STORY-LIB-002: Dùng phân loại của mẫu để tìm mẫu tham khảo từ dự toán.

## Non-Functional

- Bổ sung ngày 01/10/2026: AC-012–AC-018 mô tả yêu cầu section content theo BR-LIB-001 khoản 17–20. Đã xác nhận tên tự nhập theo mẫu, tất cả file nằm trong section, phải chọn section trước khi upload kể cả bản nháp, và người quản lý được thay đổi thứ tự section. Việc xử lý dữ liệu cũ còn mở tại BR-LIB-001/Notes; không tự áp phương án gom nhóm. Người dùng đã giao triển khai. Đã bổ sung ST-LIB-052–066 và TDD-LIB-001/002, TDD-MEDIA-001 ở dạng thiết kế để chốt; chưa có code hoặc kết quả kiểm thử phần section. Người dùng đã chọn thêm section trực tiếp vào current và chỉ cho khách thấy sau khi file đầu tiên được lưu thành công, xem BR-LIB-001 khoản 20.
- Kiểm tra quyền tại backend, kể cả yêu cầu trực tiếp; không chỉ ẩn nút trên giao diện.
- Chưa đặt ngưỡng hiệu năng hay giới hạn hạ tầng khi chưa có căn cứ. Chưa triển khai hoặc thực thi test.
- Phần bổ sung ngày 30/09/2026 tại AC-008–AC-011 ghi nghiệp vụ đã chốt; đã bổ sung đặc tả System Test ST-LIB-032–041 sau khi người dùng chốt US/BR; Người dùng đã chốt TDD bổ sung trong hội thoại. Đã bổ sung ST-LIB-042–051 và UT-LIB-053–078; xem bảng truy vết trong discovery/library-system-test-coverage.md và discovery/library-unit-test-coverage.md. Chưa sửa mã ứng dụng hoặc thực thi các ca mới.

## Out of Scope

- Lưu mẫu yêu thích, mô hình 3D xoay trực tiếp, tìm kiếm kích thước, lựa chọn sắp xếp danh sách mẫu.
- Sửa phiên bản đã được thay thế; xóa phiên bản đã công bố.
