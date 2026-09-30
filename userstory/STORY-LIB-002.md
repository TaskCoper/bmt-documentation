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

# STORY-LIB-002

## Metadata

- **Story**: Là khách tham khảo, tôi muốn tìm và lọc mẫu 2D/3D, gồm tìm theo thông tin dự toán, để chọn bản vẽ phù hợp.
- **Context**: Danh sách công khai không dùng lượt; bộ lọc dùng chung danh mục tạo dự toán. Tìm mẫu từ dự toán phải khớp mọi thông tin áp dụng bằng ID danh mục ổn định và giá trị tầng/tum, không bắt buộc cùng phiên bản danh mục.
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

- Có các mẫu đã công bố; không yêu cầu đăng nhập.

### Trigger

Khách mở hoặc thay bộ lọc ở trang thư viện; hoặc frontend tự tìm mẫu theo đầu vào dự toán khi AI đang xử lý yêu cầu tạo thiết kế đã tiếp nhận.

## Flow

### Main Flow

1. Hiển thị phiên bản mới nhất của các mẫu không bị ẩn, chỉ gồm ảnh đại diện và thông tin tóm tắt.
2. Ở trang thư viện, cho chọn 2D/3D, loại công trình, số tầng, Có tum/Không tum hoặc Tất cả; thu hẹp số tầng theo loại. Với 3D, cho lọc thêm phong cách kiến trúc và nội thất. Chỉ lọc theo lựa chọn đã nhập, không đòi đủ mọi trường.
3. Ở trang tạo dự toán, khi AI đang xử lý, frontend tự tìm cả 2D và 3D bằng đúng phân loại của đầu vào đã gửi AI. Khi tìm bằng thông tin dự toán, dùng loại công trình và các giá trị tầng/tum áp dụng của dự toán làm điều kiện cho cả 2D và 3D. Với 3D, thêm phong cách kiến trúc và nội thất dự toán đã chọn trong các nhóm đang áp dụng; từng danh sách phong cách của mẫu phải chứa lựa chọn tương ứng. Kết hợp tất cả điều kiện theo BR-LIB-001 khoản 11–14.
4. Tìm theo tên, phân trang và xếp theo lần công bố gần nhất; không tính lượt.
5. Chuyển sang luồng mở chi tiết STORY-LIB-003 khi khách chọn mẫu.

### Alternative Flow

#### ALT-01

Mẫu công khai dùng số tầng đã bị bỏ khỏi cấu hình hiện hành.

1. Vẫn giữ giá trị này trong bộ lọc để khách tìm được mẫu.

#### ALT-02

Dự toán và mẫu dùng hai phiên bản danh mục khác nhau.

1. Đối chiếu ID ổn định của loại công trình và phong cách, cùng giá trị tầng/tum; không yêu cầu trùng phiên bản danh mục hoặc tên hiển thị.
2. Giữ cấu hình đã áp dụng cho từng bên; chỉ trả mẫu khớp tất cả điều kiện áp dụng của dự toán. Không tự ghép hai mục khác ID chỉ vì cùng tên.

### Exception Flow

#### EXC-01

Không có kết quả phù hợp.

1. Hiển thị danh sách trống, không tính lượt; không tự nới điều kiện để lấy mẫu gần giống.

## Acceptance Criteria

#### AC-001

- **Given**: Khách chưa đăng nhập
- **When**: Tìm, lọc hoặc chuyển trang
- **Then**: Được xem danh sách, không cấp gói hoặc tính lượt; không trả bộ ảnh/tệp chi tiết.

#### AC-002

- **Given**: Có mẫu có tum, không tum và không áp dụng
- **When**: Lọc Có tum hoặc Không tum
- **Then**: Chỉ trả mẫu khớp; không coi Không áp dụng là Không tum.

#### AC-003

- **Given**: Có mẫu dùng số tầng cũ
- **When**: Chọn loại và lọc tầng
- **Then**: Vẫn lọc được số tầng cũ đang có mẫu công khai của loại đó.

#### AC-004

- **Given**: Có mẫu mới công bố và mẫu chỉ sửa tại chỗ
- **When**: Xem danh sách
- **Then**: Thứ tự theo lần công bố gần nhất; sửa nhỏ không đẩy mẫu lên đầu. Tìm theo tên, không có tìm kích thước hoặc chọn sắp xếp.

#### AC-005

- **Given**: Dự toán có loại công trình và các giá trị tầng/tum hợp lệ theo cấu hình đã áp dụng.
- **When**: Tìm mẫu 2D tham khảo bằng thông tin dự toán.
- **Then**: Chỉ trả mẫu khớp ID loại công trình và mọi giá trị tầng/tum đang áp dụng; không xét hai nhóm phong cách.
- **And**: Trường bị tắt trong cấu hình của dự toán không tham gia điều kiện; Không áp dụng không được thay thế một giá trị cụ thể đang cần khớp.

#### AC-006

- **Given**: Dự toán có phong cách kiến trúc và nội thất đã chọn ở hai nhóm đang bật; mẫu 3D có thể thuộc nhiều phong cách trong mỗi nhóm.
- **When**: Tìm mẫu 3D tham khảo bằng thông tin dự toán.
- **Then**: Mẫu phải khớp loại công trình, tầng/tum áp dụng và chứa ID phong cách đã chọn trong đúng từng nhóm. Khớp một nhóm nhưng không khớp nhóm còn lại thì bị loại.
- **And**: Mẫu có thêm các phong cách khác vẫn được lấy ra; nhóm bị tắt trong cấu hình của dự toán không tham gia điều kiện tìm kiếm.

#### AC-007

- **Given**: Dự toán dùng danh mục phiên bản A, mẫu dùng phiên bản B; các ID loại công trình/phong cách và mọi giá trị cần đối chiếu đều khớp.
- **When**: Tìm mẫu tham khảo từ dự toán.
- **Then**: Mẫu vẫn được lấy ra dù A khác B hoặc tên/ảnh của mục danh mục đã đổi; không dùng phiên bản danh mục làm điều kiện bắt buộc phải bằng nhau.
- **And**: Không chuyển danh mục hoặc cấu hình đã áp dụng của dự toán hay mẫu sang phiên bản khác.

#### AC-008

- **Given**: Dự toán và mẫu có loại công trình hoặc phong cách trùng tên nhưng khác ID.
- **When**: Đối chiếu điều kiện tương ứng khi tìm mẫu.
- **Then**: Không coi hai mục là một; loại mẫu nếu không khớp ID cần tìm, dù tên hiển thị giống nhau.

#### AC-009

- **Given**: Không có mẫu công khai nào khớp toàn bộ điều kiện áp dụng của dự toán.
- **When**: Tìm mẫu tham khảo.
- **Then**: Trả danh sách rỗng, không tự bỏ điều kiện hoặc bổ sung mẫu gần giống.
- **And**: Kết quả vẫn chỉ gồm phiên bản hiện hành của mẫu không ẩn, ảnh đại diện và thông tin tóm tắt; không tính lượt khi tìm. Mở chi tiết tuân theo STORY-LIB-003.

#### AC-010

- **Given**: Khách đang ở trang thư viện.
- **When**: Chọn, bỏ hoặc thay các bộ lọc loại công trình, số tầng/tum và phong cách của mẫu 3D.
- **Then**: Tìm theo các bộ lọc đã chọn; không chặn chỉ vì chưa chọn đủ thông tin như khi tạo dự toán.
- **And**: Hai nhóm phong cách dùng danh mục có sẵn và chỉ áp dụng với 3D; mỗi nhóm cho chọn nhiều mục; trong nhóm khớp ít nhất một mục đã chọn, giữa các nhóm và các điều kiện khác phải khớp đồng thời. Nhóm không chọn không lọc.

#### AC-011

- **Given**: Yêu cầu tạo thiết kế đã được tiếp nhận và AI đang xử lý.
- **When**: Frontend hiển thị mẫu tham khảo cho dự toán.
- **Then**: Frontend tự gọi tìm mẫu 2D và 3D theo phân loại của đúng đầu vào đã gửi AI, áp dụng AC-005–AC-009; không đợi kết quả AI mới tìm mẫu.
- **And**: Không dùng bộ lọc tự do ở trang thư viện để thay các điều kiện của dự toán; mẫu tìm thấy không được trình bày như kết quả do AI vừa tạo.

## References

### TDDs

- TDD-LIB-001
- TDD-LIB-002: Quyền xem, lượt và tải tài nguyên khi khách mở mẫu từ danh sách.

### Rules

- BR-LIB-001
- BR-LIB-002
- BR-LIB-003
- BR-PROJ-004: Cấu hình và lựa chọn danh mục đã áp dụng cho dự toán.

### Dependencies

- STORY-LIB-001: Phân loại mẫu, gồm nhiều phong cách cho mẫu 3D.
- STORY-PROJ-005: ID danh mục dùng chung và cấu hình theo loại công trình.
- STORY-PROJ-002: Thời điểm tác vụ AI được tiếp nhận và đang xử lý.
- ST-LIB-042–048: bộ lọc tự do, contract match và tự lấy mẫu khi AI đang xử lý; xem bảng truy vết tại discovery/library-system-test-coverage.md.

## Non-Functional

- Kiểm tra quyền tại backend, kể cả yêu cầu trực tiếp; không chỉ ẩn nút trên giao diện.
- Chưa đặt ngưỡng hiệu năng hay giới hạn hạ tầng khi chưa có căn cứ. Chưa triển khai hoặc thực thi test.
- Phần bổ sung ngày 30/09/2026 tại AC-005–AC-009 ghi nghiệp vụ đã chốt; đã bổ sung đặc tả System Test ST-LIB-032–041 sau khi người dùng chốt US/BR; Người dùng đã chốt TDD bổ sung trong hội thoại. Đã bổ sung ST-LIB-042–051 và UT-LIB-053–078; xem bảng truy vết trong discovery/library-system-test-coverage.md và discovery/library-unit-test-coverage.md. Chưa sửa mã ứng dụng hoặc thực thi các ca mới.

## Out of Scope

- Lưu mẫu yêu thích, mô hình 3D xoay trực tiếp, tìm kiếm kích thước, lựa chọn sắp xếp.
- Sửa phiên bản đã được thay thế; xóa phiên bản đã công bố.
