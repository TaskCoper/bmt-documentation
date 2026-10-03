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
- **Context**: Công trình là hồ sơ xây dựng thuộc khách, tạo miễn phí và không giới hạn số lượng. Ngày 01/10/2026 người dùng chốt mở rộng diện tích đất, hiện trạng, địa chỉ ba phần, ngân sách, khởi công và phân loại dùng chung với dự toán. Có thể tạo độc lập hoặc từ một dự toán hoàn tất; dữ liệu lấy từ nguồn bị khóa. Tệp tùy chọn được quản lý theo STORY-SITE-004. Công trình không có trạng thái tiến độ riêng; gói giữ chỗ khóa sửa/xóa. Người dùng xác nhận chưa có dữ liệu công trình cần chuyển đổi.
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

- Khách đã đăng nhập bằng tài khoản khách hàng theo BR-RBAC-005.
- Khách không cần có gói thiết kế hay gói giám sát để tạo công trình.

### Trigger

Khách mở danh sách công trình của mình, hoặc yêu cầu tạo, sửa hay xóa một công trình.

## Flow

### Main Flow

1. Khách chọn tạo công trình độc lập; hệ thống hiển thị form và danh mục hiện hành, cùng lựa chọn dự toán đủ điều kiện theo BR-SITE-004 nếu khách muốn dùng nguồn.
2. Khách nhập tên, diện tích đất, hiện trạng, tỉnh/thành phố, phường/xã, số nhà–đường, ngân sách VND và dự kiến khởi công; chọn loại và các trường tầng/tum/phong cách áp dụng theo BR-SITE-005. Trường không áp dụng không cho chọn và không bắt nhập.
3. Frontend xác định vĩ độ, kinh độ cho địa chỉ đầy đủ. Khách có thể thêm bản vẽ hoặc ảnh hiện trạng theo STORY-SITE-004; được để trống tệp và nguồn.
4. Backend kiểm quyền khách hàng, dữ liệu theo BR-SITE-001, danh mục theo BR-SITE-005/006, trùng tên và tọa độ; nếu có tệp thì kiểm BR-SITE-007.
5. Khi hợp lệ, lưu công trình thuộc khách cùng cấu hình danh mục áp dụng và liên kết tệp hợp lệ. Không thu phí, trừ lượt hoặc gọi AI.
6. Công trình hiện trong danh sách của khách và có thể được gắn gói giám sát theo STORY-SUB-004.

### Alternative Flow

#### ALT-01

Khách xem danh sách và chi tiết công trình của mình.

1. Khách mở danh sách hoặc chi tiết công trình.
2. Hệ thống chỉ trả công trình thuộc tài khoản của khách. Danh sách giữ thông tin nhận diện công trình và gói; chi tiết hiển thị đầy đủ hồ sơ, nguồn nếu có và tệp theo BR-SITE-003.
3. Khách không thấy tên nhân viên phụ trách gói.

#### ALT-02

Khách sửa hồ sơ công trình không có gói giữ chỗ; các trường lấy từ dự toán vẫn bị khóa.

1. Khách sửa các trường không bị khóa bởi dự toán nguồn của công trình thuộc mình. Nếu đổi địa chỉ, frontend xác định lại kinh độ, vĩ độ và gửi cả hai cùng địa chỉ mới.
2. Hệ thống kiểm tra công trình không có gói đã gán hoặc đã hoàn thành theo BR-SITE-002, rồi kiểm tra lại theo BR-SITE-001. Tên mới không trùng công trình khác của khách; giữ đủ thông tin bắt buộc và dùng danh mục của hồ sơ theo BR-SITE-005/006.
3. Khi đổi địa chỉ, backend yêu cầu đủ cả kinh độ và vĩ độ; thiếu một trong hai thì từ chối toàn bộ yêu cầu sửa. Khi dữ liệu đạt yêu cầu, hệ thống lưu thông tin mới; nếu đổi địa chỉ thì lưu địa chỉ và tọa độ cùng nhau. Lịch sử của các gói đã gỡ hoặc đã hủy khỏi công trình vẫn giữ tên và địa chỉ tại lúc gỡ hoặc hủy.

#### ALT-03

Khách xóa công trình chưa có lời mời báo giá và không có gói giữ chỗ.

1. Khách yêu cầu xóa công trình.
2. Hệ thống kiểm tra công trình chưa có lời mời báo giá theo BR-RFQ-003 và không có gói đã gán hoặc đã hoàn thành theo BR-SITE-002. Khi chưa có lời mời, công trình chưa từng có gói hoặc chỉ có gói đã gỡ hay đã hủy đều xóa được.
3. Hệ thống xóa công trình. Công trình không còn trong danh sách và không nhận gói giám sát được nữa. Lịch sử của các gói đã gỡ hoặc đã hủy vẫn giữ tên và địa chỉ công trình tại lúc gỡ hoặc hủy. Nếu có dự toán nguồn, xóa công trình giải phóng liên kết; không xóa dự toán nguồn.

#### ALT-04

Khách tạo công trình từ dự toán đã hoàn tất.

1. Chọn một dự toán của chính mình, trạng thái Succeeded, chưa xóa và chưa được công trình còn tồn tại sử dụng.
2. Hệ thống điền và khóa diện tích đất, địa chỉ ba phần, loại, số tầng, tum, phong cách kiến trúc/nội thất theo nguồn. Trường không áp dụng giữ Không áp dụng. Cấu hình dùng theo nguồn, không theo danh mục mới nhất.
3. Khách nhập riêng tên công trình, hiện trạng, ngân sách, khởi công; frontend xác định tọa độ địa chỉ nguồn. Tệp là tùy chọn và không sao từ kết quả dự toán.
4. Trước khi tạo, khách được đổi hoặc bỏ nguồn. Sau khi tạo, không đổi/gỡ nguồn, không bổ sung nguồn cho công trình độc lập và không sửa các trường nguồn.
5. Backend kiểm lại điều kiện nguồn, lưu liên kết cùng hồ sơ và bảo đảm một dự toán chỉ dùng cho một công trình còn tồn tại theo BR-SITE-004.

#### ALT-05

Admin đã đổi danh mục sau khi công trình hoặc dự toán nguồn được tạo.

1. Hồ sơ công trình tiếp tục dùng cấu hình được giữ của mình; tạo từ dự toán dùng cấu hình của dự toán đó.
2. Các tên/lựa chọn đã lưu không tự thay đổi. Hiện trạng đã ngừng cho chọn vẫn được giữ trên hồ sơ cũ khi sửa trường khác theo BR-SITE-006.

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

Khách sửa hoặc xóa công trình đang có gói giữ chỗ, tức gói đã gán hoặc đã hoàn thành.

1. Hệ thống từ chối, kể cả khi yêu cầu gửi trực tiếp tới API.
2. Công trình và gói giữ nguyên.
3. Nếu khách gõ sai trường nhập riêng, khách liên hệ tổng đài để xử lý gói theo STORY-SUB-006 rồi sửa khi không còn gói giữ chỗ. Việc gỡ vẫn phải đáp ứng điều kiện của gói; không mở khóa trường lấy từ dự toán nguồn.

#### EXC-04

Khách xem, sửa hoặc xóa công trình của khách khác, kể cả gửi yêu cầu trực tiếp tới API.

1. Hệ thống từ chối và không tiết lộ tên, địa chỉ hay gói của công trình đó.
2. Dữ liệu của công trình giữ nguyên.

#### EXC-05

Yêu cầu tạo công trình hoặc đổi địa chỉ thiếu kinh độ, vĩ độ hoặc cả hai.

1. Backend từ chối yêu cầu, kể cả khi gọi trực tiếp API.
2. Không tạo công trình mới hoặc lưu thay đổi; công trình đang sửa giữ nguyên dữ liệu cũ. Frontend cần xác định đủ tọa độ trước khi gửi lại.

#### EXC-06

Thiếu thông tin bắt buộc, diện tích/ngân sách sai điều kiện hoặc phân loại không hợp lệ.

1. Chỉ rõ trường không đạt theo BR-SITE-001/005/006, từ chối lưu và giữ nội dung khách đang nhập.
2. Không tự làm tròn diện tích, gán phong cách mặc định hoặc dùng giá trị Không áp dụng cho trường cần chọn.

#### EXC-07

Dự toán không còn đủ điều kiện hoặc yêu cầu cố đổi nguồn/trường đã khóa, kể cả qua API.

1. Kiểm lại chủ sở hữu, Succeeded, chưa xóa và chưa bị công trình khác sử dụng; từ chối khi không đạt mà không lộ dữ liệu người khác.
2. Từ chối đổi/gỡ/bổ sung nguồn sau khi tạo hoặc giả mạo thông tin lấy từ nguồn; không ghi thay đổi một phần.

## Acceptance Criteria

Trong các AC dưới đây, dữ liệu không được nêu là thiếu/sai đều hợp lệ theo BR-SITE-001/005/006, có đủ tọa độ. Các AC-001 đến AC-021 về tên/địa chỉ dùng công trình độc lập; địa chỉ được nhập thành Tỉnh/Thành phố, Phường/Xã và Số nhà–đường.

#### AC-001

- **Given**: Khách U1 chưa có công trình và chưa mua gói nào.
- **When**: U1 tạo công trình tên “Nhà phố”, chọn tỉnh/thành phố và phường/xã hợp lệ, nhập số nhà–đường “12 Nguyễn Thị Thập”.
- **Then**: Tạo thành công; công trình thuộc U1 và hiện trong danh sách công trình của U1.
- **And**: Không thu phí và không yêu cầu gói thiết kế hay gói giám sát.

#### AC-002

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 tạo công trình với tên “  Nhà vườn  ” và số nhà–đường “  12 Đường A  ”, với tỉnh/xã hợp lệ đã chọn.
- **Then**: Hệ thống lưu tên “Nhà vườn” và số nhà–đường “12 Đường A”.
- **And**: Khoảng trắng ở giữa chuỗi được giữ nguyên.

#### AC-003

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 lần lượt gửi yêu cầu tạo với tên trống, tên chỉ có khoảng trắng, và địa chỉ trống.
- **Then**: Cả ba yêu cầu bị từ chối, mỗi lần chỉ rõ trường không hợp lệ.
- **And**: Không có công trình nào được tạo.

#### AC-004

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 tạo công trình có tên đúng 200 ký tự và số nhà–đường đúng 500 ký tự sau khi bỏ khoảng trắng đầu và cuối, đã chọn tỉnh/xã hợp lệ.
- **Then**: Tạo thành công.
- **And**: Tên và số nhà–đường được lưu đủ, không bị cắt.

#### AC-005

- **Given**: Khách U1 đã đăng nhập.
- **When**: U1 lần lượt tạo công trình có tên 201 ký tự, rồi công trình có số nhà–đường 501 ký tự, tính sau khi bỏ khoảng trắng đầu và cuối.
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

- **Given**: Công trình A của U1 có gói G1 đã hủy; sau đó U1 gắn gói G2 vào A và G2 ở trạng thái đã gán.
- **When**: U1 xem chi tiết A.
- **Then**: U1 thấy G1 với trạng thái đã hủy và G2 với trạng thái đã gán.

#### AC-011

- **Given**: Công trình A của U1 có gói G1 đã gán, do nhân viên N phụ trách; công trình C của U1 có gói G3 đã hoàn thành.
- **When**: U1 đổi tên A thành “Nhà phố mới” và đổi địa chỉ A; sau đó đổi địa chỉ C; kể cả gửi yêu cầu trực tiếp tới API.
- **Then**: Cả hai yêu cầu bị từ chối vì công trình đang có gói giữ chỗ.
- **And**: A và C giữ tên, địa chỉ cũ; G1 vẫn gắn A và N vẫn phụ trách G1; G3 vẫn gắn C.

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

- **Given**: Công trình B của U1 chưa từng có gói giám sát gắn vào và chưa có lời mời báo giá.
- **When**: U1 xóa B.
- **Then**: Xóa thành công; B không còn trong danh sách của U1.
- **And**: Yêu cầu gắn gói giám sát vào B sau đó bị từ chối.

#### AC-015

- **Given**: Công trình A của U1 có gói G1 đã hoàn thành; công trình B từng có gói G2 gắn vào, nay G2 đã hủy, B không có gói giữ chỗ và chưa có lời mời báo giá.
- **When**: U1 lần lượt yêu cầu xóa A và B.
- **Then**: Xóa A bị từ chối vì G1 đã hoàn thành vẫn giữ chỗ; xóa B thành công.
- **And**: A và G1 giữ nguyên; G2 vẫn ở trạng thái đã hủy và lịch sử của G2 giữ tên, địa chỉ của B tại lúc hủy.

#### AC-016

- **Given**: Công trình C thuộc khách U2.
- **When**: U1 xem chi tiết, sửa hoặc xóa C, kể cả gửi yêu cầu trực tiếp tới API.
- **Then**: Cả ba yêu cầu bị từ chối và không tiết lộ tên, địa chỉ hay gói của C.
- **And**: C giữ nguyên.

#### AC-017

- **Given**: Công trình A của U1 từng có gói G1 nay đã hủy; công trình B của U1 từng có gói G2 nay đã bị nhân viên gỡ; A và B không có gói giữ chỗ.
- **When**: U1 đổi địa chỉ của A, gửi kèm đủ tọa độ mới do frontend xác định, và đổi tên của B.
- **Then**: Cả hai yêu cầu thành công.
- **And**: Lịch sử của G1 và G2 vẫn giữ tên, địa chỉ công trình tại lúc hủy hoặc gỡ.

#### AC-018

- **Given**: Khách đã đăng nhập và nhập tên, địa chỉ hợp lệ.
- **When**: Frontend xác định và gửi cả kinh độ, vĩ độ trong yêu cầu tạo.
- **Then**: Backend lưu công trình cùng hai giá trị tọa độ nếu các điều kiện tạo khác đều đạt.
- **And**: Không cần chờ đến lúc tìm nhà thầu mới bổ sung tọa độ.

#### AC-019

- **Given**: Các thông tin khác trong yêu cầu tạo đều hợp lệ.
- **When**: Yêu cầu thiếu kinh độ, thiếu vĩ độ hoặc thiếu cả hai, kể cả khi gọi trực tiếp API.
- **Then**: Backend từ chối tạo ở cả ba trường hợp.
- **And**: Không lưu công trình thiếu tọa độ.

#### AC-020

- **Given**: Công trình thuộc khách và được phép sửa theo BR-SITE-002; các thông tin mới đều hợp lệ.
- **When**: Khách đổi địa chỉ, frontend xác định lại và gửi đủ kinh độ, vĩ độ cùng địa chỉ mới.
- **Then**: Backend lưu địa chỉ và hai tọa độ mới cùng nhau.
- **And**: Tìm nhà thầu theo bán kính sau đó dùng tọa độ mới của công trình.

#### AC-021

- **Given**: Công trình thuộc khách, được phép sửa và đã có địa chỉ, tọa độ.
- **When**: Khách gửi yêu cầu đổi địa chỉ nhưng thiếu kinh độ, thiếu vĩ độ hoặc thiếu cả hai, kể cả gọi trực tiếp API.
- **Then**: Backend từ chối toàn bộ yêu cầu sửa ở cả ba trường hợp.
- **And**: Địa chỉ, tọa độ và các thông tin khác của công trình giữ nguyên.

#### AC-022

- **Given**: Khách chưa có gói, đã nhập đủ hồ sơ bắt buộc.
- **When**: Tạo công trình không chọn dự toán và không đính kèm tệp.
- **Then**: Tạo thành công, nguồn trống và danh sách tệp rỗng; không thu phí hoặc trừ lượt.

#### AC-023

- **Given**: Các trường khác hợp lệ.
- **When**: Lần lượt gửi AreaM2=100.25, 0, -1 và 100.251.
- **Then**: Chấp nhận 100.25 m²; từ chối ba giá trị còn lại, không làm tròn 100.251.

#### AC-024

- **Given**: Các trường khác hợp lệ.
- **When**: Gửi ngân sách 2000000000; sau đó thử bỏ trống, 0, số âm hoặc 2000000000.5.
- **Then**: Giá trị đầu hợp lệ và hiển thị 2.000.000.000 ₫; các giá trị còn lại bị từ chối.

#### AC-025

- **Given**: Các trường khác hợp lệ.
- **When**: Lần lượt bỏ hiện trạng, loại hoặc dự kiến khởi công; sau đó chọn Chưa xác định cho khởi công.
- **Then**: Các trường bắt buộc bỏ trống bị từ chối; Chưa xác định là lựa chọn hợp lệ.

#### AC-026

- **Given**: Loại Nhà phố trong cấu hình áp dụng không cho chọn phong cách nội thất hoặc không có lựa chọn nội thất.
- **When**: Khách chọn loại đó và hoàn tất các trường còn áp dụng.
- **Then**: Phong cách nội thất không cho chọn, hiển thị Không áp dụng và không cản tạo; không tự gán phong cách.

#### AC-027

- **Given**: Loại cho chọn tầng, tum và cả hai nhóm phong cách với danh sách hợp lệ.
- **When**: Khách tạo hồ sơ thiếu một trường áp dụng hoặc chọn sai loại/nhóm.
- **Then**: Từ chối; khi chọn đủ hợp lệ thì tạo được. Không tum là câu trả lời hợp lệ, khác Không áp dụng.

#### AC-028

- **Given**: Khách có dự toán hoàn tất, chưa xóa, chưa liên kết và còn giữ cấu hình cũ.
- **When**: Chọn nguồn rồi nhập đủ các trường riêng và tạo công trình.
- **Then**: Điền/khóa đúng các trường nguồn theo cấu hình cũ; lưu liên kết với nguồn.
- **And**: Tên công trình nhập riêng; không tự sao tệp dự toán hoặc trừ lượt AI.

#### AC-029

- **Given**: Có dự toán nháp, đang xử lý, thất bại, đã xóa, của khách khác hoặc đã được công trình sử dụng.
- **When**: Khách tìm nguồn hoặc gửi trực tiếp yêu cầu tạo bằng các bản đó.
- **Then**: Không cho dùng bất kỳ bản nào trong các trường hợp trên; không tiết lộ dữ liệu khách khác.

#### AC-030

- **Given**: Form đang chọn nguồn A, chưa lưu công trình.
- **When**: Khách đổi sang B rồi bỏ nguồn.
- **Then**: Trường nguồn đổi đúng theo B; khi bỏ nguồn được nhập trực tiếp theo danh mục công trình độc lập. Chỉ tạo khi toàn bộ hồ sơ hợp lệ.

#### AC-031

- **Given**: Công trình đã tạo từ nguồn và không có gói giữ chỗ.
- **When**: Khách đổi/gỡ nguồn hoặc sửa một trường lấy từ nguồn qua API.
- **Then**: Từ chối, hồ sơ giữ nguyên; chỉ các trường riêng được sửa nếu đáp ứng BR-SITE-002.

#### AC-032

- **Given**: Công trình được tạo độc lập.
- **When**: Khách yêu cầu bổ sung dự toán nguồn sau khi tạo.
- **Then**: Từ chối; công trình tiếp tục không có nguồn.

#### AC-033

- **Given**: Hai yêu cầu tạo dùng cùng dự toán đủ điều kiện được xử lý đồng thời.
- **When**: Hệ thống lưu hồ sơ.
- **Then**: Tối đa một công trình được liên kết; yêu cầu còn lại không để lại hồ sơ một phần.

#### AC-034

- **Given**: Dự toán là nguồn của công trình chưa có gói.
- **When**: Khách xóa dự toán; sau đó xóa công trình hợp lệ và thử lại hoặc dùng nguồn tạo công trình mới.
- **Then**: Lần xóa dự toán đầu bị chặn. Sau khi xóa công trình, nguồn được xóa hoặc dùng lại nếu vẫn đủ điều kiện.

#### AC-035

- **Given**: Hồ sơ hoặc dự toán nguồn đã có cấu hình; Admin đổi tên/cấu hình hoặc thêm loại mới.
- **When**: Khách xem/sửa hồ sơ được phép, hoặc tạo công trình từ dự toán nguồn.
- **Then**: Giữ cấu hình tương ứng, không tự đổi lựa chọn hoặc chuyển sang danh mục mới nhất.

#### AC-036

- **Given**: Công trình độc lập được sửa; hiện trạng cũ đã ngừng cho chọn.
- **When**: Khách sửa ngân sách mà giữ hiện trạng, hoặc tạo công trình mới chọn hiện trạng đã ngừng.
- **Then**: Cho lưu trường hợp giữ hiện trạng cũ; từ chối chọn mục đã ngừng cho công trình mới.

#### AC-037

- **Given**: Khách đang nhập địa chỉ và các trường khác hợp lệ.
- **When**: Chọn xã không thuộc tỉnh, bỏ một phần địa chỉ hoặc đổi địa chỉ mà thiếu tọa độ mới.
- **Then**: Từ chối lưu; không tạo hoặc cập nhật địa chỉ/tọa độ một phần.

#### AC-038

- **Given**: Các thông tin khác của hồ sơ hợp lệ.
- **When**: Lần lượt chọn Càng sớm càng tốt, Trong 1–3 tháng tới, Trong 3–6 tháng tới và Chưa xác định rồi lưu.
- **Then**: Cả bốn lựa chọn được chấp nhận; hồ sơ giữ đúng lựa chọn, không tự đổi thành một ngày khởi công hoặc tự chuyển lựa chọn khi thời gian trôi qua.

## References

### TDDs

- TDD-SITE-003
- TDD-SITE-004
- TDD-SITE-005

- [TDD-SITE-002](../tdd/TDD-SITE-002.md): Thiết kế tọa độ hiện có; cần cập nhật theo hồ sơ mở rộng ngày 01/10/2026.

- [TDD-SITE-001](../tdd/TDD-SITE-001.md): Bảng công trình, API tạo, xem, sửa, xóa của khách, kiểm trùng tên và điều kiện xóa.

### Rules

- BR-SITE-001/Then
- BR-SITE-002/Then
- BR-RFQ-003/Then: Giữ hồ sơ gốc khi đã có lời mời báo giá.
- BR-SITE-003/Then
- BR-RBAC-005/Then
- BR-RBAC-011/Then
- BR-SUB-022/Then
- BR-SUB-006/Then
- BR-SUB-024/Then: Gói đã hủy không khóa việc sửa hoặc xóa công trình.
- BR-SUB-026/Then: Gói đã gỡ không còn giữ chỗ trên công trình cũ.

- BR-SITE-004/Then
- BR-SITE-005/Then
- BR-SITE-006/Then
- BR-SITE-007/Then
- BR-PROJ-009/Then

### Dependencies

- STORY-SUB-004: Khách gắn gói giám sát vào công trình của mình.
- STORY-SUB-006: Nhân viên gỡ gói khi khách gán nhầm hoặc cần sửa công trình.
- STORY-SITE-002: Nhân viên xem công trình theo quyền.

- STORY-SITE-003: Admin quản lý hiện trạng.
- STORY-SITE-004: Khách quản lý bản vẽ và ảnh hiện trạng.
- STORY-PROJ-007: Chặn xóa dự toán đang là nguồn công trình.

## Non-Functional

- Người dùng đã chốt bản US/BR trong hội thoại ngày 01/10/2026. System Test đã được cập nhật; xem [bảng độ phủ](../discovery/construction-site-system-test-coverage.md). Chưa chạy các ca; xác nhận hội thoại không thay cho phê duyệt/import trên hệ thống tài liệu.

- Kiểm quyền, khóa trường nguồn, tính duy nhất nguồn, giới hạn tệp và khóa do gói ở backend; không chỉ vô hiệu hóa giao diện.
- Tạo công trình và xóa dự toán đồng thời không được để công trình dùng nguồn đã xóa. Hai yêu cầu dùng cùng nguồn không được cùng thành công. Các yêu cầu tạo/sửa/gán gói giữ các bảo đảm đồng thời hiện có theo BR-SITE-002.
- Tọa độ phải ứng với địa chỉ hiện tại; không dùng kết quả xác định tọa độ đến muộn cho địa chỉ cũ.
- Người dùng xác nhận không có dữ liệu công trình cần chuyển đổi. Không chạy migration hoặc xóa dữ liệu trong tác vụ tài liệu.
- Phần mở rộng ngày 01/10/2026 đã chốt nghiệp vụ qua hội thoại; bản US/BR cụ thể đã được chốt và ST-SITE đã cập nhật. TDD-SITE-001/002 và UT-SITE cần cập nhật ở giai đoạn sau; các kết quả kiểm thử cũ không chứng minh phần mở rộng đạt.
- Chưa chốt chỉ tiêu hiệu năng, cách phân trang, thứ tự sắp xếp hoặc tìm kiếm danh sách công trình.

## Out of Scope

- Trạng thái tiến độ thi công; chia công trình thành tầng/hạng mục/công việc; nhân viên tạo, sửa, xóa hồ sơ hoặc quản lý tệp thay khách; hiển thị tên nhân viên phụ trách cho khách.
- Tìm kiếm/gợi ý nhà thầu, mẫu thiết kế hoặc thông tin liên quan từ các trường mới; đợt này chỉ lưu dữ liệu để phục vụ về sau.
- Gắn gói theo STORY-SUB-004; phân công nhân viên theo STORY-RBAC-003. Không đổi quy tắc thanh toán hoặc cấp gói.
- Đồng bộ tự động từ dự toán hoặc danh mục mới; thay nguồn sau khi tạo; sao tệp kết quả dự toán sang tệp công trình.
- Thiết kế schema/API và triển khai code thuộc giai đoạn sau. Sprint, Priority và người thực hiện chưa được phân công.
