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


# STORY-SUB-002

## Metadata

**Phạm vi mới nhất:** chỉ quyền tạo thiết kế mới và tra cứu mẫu được triển khai logic sử dụng/tính lượt trong đợt này. Quyền 3D vẫn cấu hình bật/tắt và hiển thị, chưa điều khiển AI hoặc kiểm tra quyền tạo 3D. Các mô tả xử lý 3D bên dưới là thiết kế cho giai đoạn sau, không phải tiêu chí nghiệm thu hiện tại. Không suy ra AI hiện phải luôn trả hoặc luôn bỏ 3D.

**Cập nhật: đổi gói và đổi chu kỳ đều áp dụng ngay theo BR-SUB-021. Các khối được đánh dấu lịch sử bên dưới không còn dùng nghiệm thu.**

- **Story**: Là Admin, tôi muốn cấu hình quyền lợi của gói theo dạng bật/tắt, hạn mức lượt hoặc mức tính năng để mô tả đúng những gì tài khoản được sử dụng.
- **Context**: Đổi gói hoặc chu kỳ áp dụng ngay theo BR-SUB-021, không phân loại nâng/hạ hoặc dùng bậc. Phạm vi gồm cả gói thiết kế và giám sát; thiết kế giới hạn một subscription đang hiệu lực trên tài khoản, giám sát giới hạn một gói đang thực hiện cho mỗi công trình. Gói giám sát không dùng danh mục quyền lợi; gói được Công bố khi có tên, giá và mô tả dịch vụ tự do (người dùng xác nhận ngày 25/09/2026). Các gói thiết kế khác cả số lượt lẫn tính năng được dùng; đã chốt quyền tạo phối cảnh 3D chân thực dạng bật/tắt. Dự toán nội thất và bố trí công năng đều dùng cùng mức chi tiết giữa các gói, không cấu hình mức riêng theo gói cho hai phần này. Danh mục thiết kế không giới hạn đúng ba quyền theo AC-025: tạo mới và tra cứu là quyền dạng lượt, các quyền lợi còn lại bật/tắt để lưu và hiển thị; giá trị từng gói do Admin cấu hình. Hệ thống định nghĩa sẵn danh mục quyền lợi gắn với tính năng, hỗ trợ cả ba dạng quyền lợi. Admin chọn và cấu hình giá trị trong gói, không tự tạo quyền lợi mới. Admin lưu nháp, kiểm tra rồi Công bố; với thiết kế, giữ nguyên kỳ hiện tại và dùng bản quyền lợi đã chốt khi đăng ký/gia hạn cho từng kỳ mới. Giám sát không có chu kỳ; gói đã cấp giữ nguyên mô tả dịch vụ đã chốt theo đơn, thay đổi đã công bố chỉ dùng cho gói cấp mới. Danh mục cụ thể và các điều kiện công bố còn cần làm rõ.
- **Sprint**:
- **Priority**:
- **Status**: Todo
- **Creator**: [Chưa xác định]
- **Reviewer**: [Chưa xác định]
- **Approver**: [Chưa xác định]
- **Assignee**:
  - Backend: [Chưa xác định]
  - QA: [Chưa xác định]

## Conditions

### Preconditions

- Người thao tác có quyền cấu hình gói theo STORY-RBAC-001; vai trò Admin có quyền này.
- Có gói để cấu hình quyền lợi.

### Trigger

Admin mở gói để cấu hình hoặc sửa quyền lợi.

## Flow

### Main Flow

1. Admin chọn quyền lợi cần cấu hình từ danh mục do hệ thống định nghĩa sẵn. Quyền dạng lượt của thiết kế gồm tạo mới và tra cứu mẫu; không có lượt chỉnh sửa. Admin được chọn một trong hai hoặc cả hai; quyền dạng lượt không được chọn sẽ không được cấp cho khách theo bản gói đó.
2. Với gói thiết kế, không yêu cầu cấu hình bậc để phân loại nâng/hạ. Admin cấu hình hai lựa chọn tháng/năm, giá và hạn mức riêng; quyền bật/tắt và mức tính năng được cấu hình chung ở gói. Admin cấu hình bật/tắt, hạn mức lượt hoặc mức tính năng phù hợp với dạng quyền lợi. Với dạng lượt, Admin chọn số lượt cụ thể hoặc không giới hạn. Với dạng bật/tắt, chưa thêm quyền hoặc đặt là tắt đều không cho khách dùng tính năng; chỉ khi thêm và bật mới đáp ứng điều kiện về quyền đó. Với dạng mức tính năng, Admin phải thêm quyền và chọn mức cụ thể; thiếu cấu hình không tự cấp mức thấp nhất.
3. Admin nhập giá và hạn mức bán thực tế trên trang quản trị, lưu nháp và kiểm tra lại nội dung. Giá tháng và giá năm của gói thiết kế đều nhập bằng VNĐ và phải lớn hơn 0; đợt này không cấu hình loại tiền khác. Các con số không được cố định từ ví dụ trong tài liệu.
4. Admin bấm Công bố khi nội dung đã sẵn sàng. Gói thiết kế phải có ít nhất một quyền lợi hợp lệ; gói giám sát phải có tên, giá và mô tả dịch vụ, không dùng danh mục quyền lợi. Cả hai loại vẫn phải đáp ứng các điều kiện cấu hình đã chốt.
5. Với thiết kế, kỳ mới hợp lệ dùng bản đã công bố và đã chốt khi đăng ký/gia hạn kỳ đó; không tự thay bằng bản công bố sau khi chốt. Quyền lợi kỳ hiện tại được giữ nguyên. Với giám sát, gói đã cấp giữ nguyên mô tả dịch vụ đã chốt theo đơn, chỉ gói cấp mới nhận mô tả đã thay đổi và công bố.

### Alternative Flow

#### ALT-01

Admin đang chỉnh sửa và chỉ lưu nháp.

1. Hệ thống giữ nội dung nháp để Admin tiếp tục kiểm tra. Có thể lưu nháp khi chưa chọn quyền lợi, miễn các dữ liệu khác hợp lệ; không tự thêm quyền lợi mặc định.
2. Quyền lợi đang áp dụng không bị thay đổi bởi bản nháp.

#### ALT-02

Admin ngừng bán một gói trong danh mục.

1. Admin chọn gói và thực hiện ngừng bán.
2. Hệ thống ẩn gói khỏi danh sách đăng ký và chặn tạo đơn mới cho gói đó. Đơn đã tạo trước lúc ngừng bán vẫn được hoàn tất theo giá và quyền lợi đã lưu nếu thanh toán hợp lệ, theo BR-SUB-013 và BR-PAY-001.
3. Các gói đã cấp tiếp tục theo quyền lợi và vòng đời hiện có; không bị tự kết thúc hoặc thay đổi hạn mức.

### Exception Flow

#### EXC-01

Admin cấu hình quyền dạng lượt có hạn mức nhỏ hơn 1 thay vì chọn không giới hạn.

1. Hệ thống từ chối lưu cấu hình hoặc công bố giá trị đó; báo rõ quyền được đưa vào gói cần ít nhất 1 lượt hoặc chọn không giới hạn.
2. Không thay đổi quyền lợi đang áp dụng. Giá tháng/năm phải lớn hơn 0 theo EXC-04; các dữ liệu bắt buộc khác vẫn cần chốt; không bắt buộc đưa cả hai quyền tạo mới và tra cứu vào gói.

#### EXC-02

Yêu cầu cấu hình chứa quyền lợi không thuộc danh mục hệ thống.

1. Hệ thống từ chối cấu hình quyền lợi không hợp lệ.
2. Không tự tạo quyền lợi mới từ mã hoặc tên được gửi lên.

#### EXC-03

Yêu cầu cấp mới từ một gói đã ngừng bán.

1. Hệ thống từ chối cấp mới, kể cả khi yêu cầu gửi trực tiếp bằng mã gói.
2. Không tạo gói đã cấp hoặc quyền lợi mới từ yêu cầu bị từ chối; không ảnh hưởng gói đang dùng của khách.

#### EXC-04

Admin nhập giá tháng hoặc giá năm của gói thiết kế bằng 0 hay âm.

1. Hệ thống từ chối lưu/công bố cấu hình, kể cả khi gửi yêu cầu trực tiếp; báo rõ giá của lựa chọn nào phải lớn hơn 0.
2. Không thay đổi bản đang áp dụng từ yêu cầu bị từ chối.

#### EXC-05

Yêu cầu lưu/công bố giá gói thiết kế dùng loại tiền khác VNĐ.

1. Hệ thống từ chối cấu hình và báo chỉ hỗ trợ VNĐ.
2. Không tự quy đổi tiền hoặc coi giá đó là VNĐ; không thay đổi bản đang áp dụng.

#### EXC-06

Admin bấm Công bố gói thiết kế chưa có bất kỳ quyền lợi nào, hoặc gói giám sát thiếu mô tả dịch vụ.

1. Hệ thống từ chối Công bố, kể cả yêu cầu gửi trực tiếp; báo gói thiết kế cần thêm ít nhất một quyền lợi, hoặc gói giám sát cần mô tả dịch vụ có nội dung.
2. Giữ bản nháp để Admin bổ sung; không thay thế bản đang áp dụng hoặc thay đổi quyền đã cấp.

## Acceptance Criteria

#### AC-001

**Không nghiệm thu trong phạm vi hiện tại:** theo BR-SUB-008 khoản 12, đợt này chưa triển khai logic kiểm tra sử dụng quyền bật/tắt hoặc quyền dạng mức; chỉ hai quyền tạo mới và tra cứu có logic sử dụng. Người dùng xác nhận ngày 25/09/2026. Nội dung giữ để dùng khi danh mục mức được chốt.

- **Given**: Admin đang cấu hình hai gói với ba quyền lợi thử, lần lượt thuộc dạng bật/tắt, hạn mức lượt và mức tính năng.
- **When**: Admin đặt gói A là tắt, 5 lượt, cơ bản; gói B là bật, 20 lượt, nâng cao; sau đó lưu nháp và mở lại từng gói.
- **Then**: Hệ thống lưu đúng dạng và giá trị của từng quyền lợi ở mỗi gói; quyền bật/tắt và mức tính năng có thể khác giữa hai gói, không chỉ khác số lượt.
- **And**: Nội dung nháp không thay đổi quyền lợi đang áp dụng cho tài khoản.

#### AC-002

- **Given**: Admin đang cấu hình quyền lợi dạng lượt trong bản nháp của gói thiết kế.
- **When**: Admin chọn không giới hạn lượt, lưu nháp và mở lại.
- **Then**: Hệ thống lưu đúng lựa chọn không giới hạn, phân biệt với hạn mức có số lượt cụ thể.
- **And**: Bản nháp không thay đổi quyền lợi đang áp dụng; bản đã công bố chỉ áp dụng cho kỳ mới nếu là bản đã chốt cho lần đăng ký/gia hạn đó.

#### AC-003

- **Given**: Hệ thống có danh mục quyền lợi được định nghĩa sẵn và Admin đang sửa gói.
- **When**: Admin chọn quyền lợi để cấu hình hoặc gửi mã quyền lợi không tồn tại trong danh mục.
- **Then**: Danh sách lựa chọn chỉ gồm quyền lợi do hệ thống định nghĩa; mã không tồn tại bị từ chối.
- **And**: Không tạo thêm định nghĩa quyền lợi từ yêu cầu của Admin.

#### AC-004

- **Given**: Gói giám sát của khách hàng đã được cấp với mô tả dịch vụ A; Admin có bản nháp thay đổi mô tả thành B.
- **When**: Admin công bố B, sau đó một gói giám sát mới được cấp hợp lệ từ đơn tạo sau khi công bố.
- **Then**: Gói đã cấp trước đó giữ nguyên A; gói cấp mới nhận B.
- **And**: Nếu B mới chỉ được lưu nháp, gói cấp mới vẫn nhận mô tả từ bản đang công bố, không dùng bản nháp.

#### AC-005

- **Given**: Một gói đang được hiển thị để đăng ký trong danh mục.
- **When**: Admin ngừng bán gói đó, sau đó có yêu cầu tạo đơn mới cho gói này.
- **Then**: Gói không còn trong danh sách đăng ký; yêu cầu tạo đơn mới bị từ chối kể cả khi dùng mã gói trực tiếp.
- **And**: Không cấp thêm quyền lợi hoặc tạo gói mới từ yêu cầu bị từ chối.

#### AC-006

- **Given**: Khách đã có subscription thiết kế trong kỳ hiện tại hoặc gói giám sát đang thực hiện theo công trình.
- **When**: Admin ngừng bán gói tương ứng trong danh mục.
- **Then**: Quyền lợi, trạng thái và thời hạn hiện có của gói đã cấp không bị thay đổi bởi việc ngừng bán.
- **And**: Thiết kế không mất hoặc được làm mới lượt; giám sát không tự hết hạn hay hoàn thành. Chính sách gia hạn thiết kế nằm ngoài tiêu chí này.

#### AC-007

- **Given**: Admin đang cấu hình một gói thiết kế có hai lựa chọn tháng và năm.
- **When**: Admin đặt giá và hạn mức khác nhau cho hai lựa chọn, lưu nháp rồi mở lại.
- **Then**: Một gói vẫn chứa đủ hai lựa chọn với đúng giá và hạn mức đã nhập; không tạo thành hai gói riêng.
- **And**: Giá và hạn mức năm không bị tự tính lại thành 12 lần giá và hạn mức tháng; bản đang áp dụng không bị thay đổi do lưu nháp.

#### AC-008

- **Given**: Admin đang cấu hình một gói thiết kế với hai lựa chọn tháng/năm trong cùng bản nháp.
- **When**: Admin đặt quyền bật/tắt và mức tính năng ở gói, đặt giá và hạn mức riêng cho hai lựa chọn, lưu và mở lại.
- **Then**: Hai lựa chọn dùng cùng quyền bật/tắt và mức tính năng đã cấu hình ở gói; giá và hạn mức vẫn giữ riêng.
- **And**: Không có cấu hình quyền bật/tắt hoặc mức tính năng khác nhau theo tháng/năm trong cùng bản gói; các kỳ đã cấp trước đó không bị cập nhật do sửa bản nháp.

#### AC-009

- **Given**: Admin có quyền cấu hình gói thiết kế từ danh mục quyền lợi hệ thống.
- **When**: Admin mở danh mục và cấu hình các quyền dạng lượt.
- **Then**: Có lượt tạo mới và tra cứu mẫu; không có lựa chọn hoặc trường hạn mức lượt chỉnh sửa.
- **And**: Yêu cầu gửi trực tiếp để cấu hình lượt chỉnh sửa bị từ chối như quyền không thuộc danh mục, không tạo quyền lợi đó trong gói.

#### AC-010

- **Given**: Admin cấu hình một quyền dạng lượt trong gói thiết kế; các dữ liệu khác đáp ứng điều kiện hiện có.
- **When**: Admin nhập hạn mức 0 cho lựa chọn tháng hoặc năm rồi lưu/công bố; sau đó thử lại với hạn mức 1 hoặc chọn không giới hạn.
- **Then**: Hạn mức 0 bị từ chối, không được dùng để cấp quyền; hạn mức 1 và lựa chọn không giới hạn đạt điều kiện về hạn mức.
- **And**: Không sửa quyền lợi kỳ đã cấp. Số lượt còn lại của khách về 0 do đã dùng/giữ hết không bị coi là cấu hình gói sai.

#### AC-011

- **Given**: Admin cấu hình riêng một gói chỉ có quyền tạo thiết kế và một gói chỉ có quyền tra cứu mẫu; quyền được chọn có hạn mức hợp lệ, các điều kiện khác đáp ứng.
- **When**: Admin lưu, mở lại và công bố từng gói; tài khoản thử nghiệm có kỳ subscription hợp lệ dùng đúng bản gói đó rồi thử Gen AI và mở chi tiết mẫu.
- **Then**: Hệ thống lưu đúng quyền được chọn, không bắt buộc thêm quyền còn lại. Gói chỉ có tạo thiết kế cho phép Gen AI khi đủ điều kiện và từ chối mở chi tiết mẫu; gói chỉ có tra cứu cho phép mở chi tiết khi đủ điều kiện và từ chối Gen AI.
- **And**: Quyền không được chọn không nhận lượt mặc định, không dùng lượt của quyền khác thay thế. Việc sửa/công bố không thay đổi quyền lợi của kỳ đã cấp trước đó.

#### AC-012

- **Given**: Admin cấu hình giá tháng và năm cho một gói thiết kế; các dữ liệu khác hợp lệ.
- **When**: Admin lưu/công bố với một trong hai giá bằng 0 hoặc âm, sau đó thử lại với cả hai giá lớn hơn 0.
- **Then**: Giá bằng 0 hoặc âm bị từ chối và có thông báo chỉ rõ lựa chọn cần sửa; cả hai giá lớn hơn 0 đạt điều kiện về giá.
- **And**: Kiểm tra áp dụng cả với yêu cầu gửi trực tiếp. Cấu hình bị từ chối không thay đổi bản đang áp dụng hoặc quyền lợi đã cấp.

#### AC-013

- **Given**: Admin cấu hình giá tháng/năm của gói thiết kế; số tiền dương và các dữ liệu khác hợp lệ.
- **When**: Admin lưu và mở lại giá bằng VNĐ, sau đó thử gửi trực tiếp cấu hình giá bằng USD cho từng lựa chọn.
- **Then**: Giá VNĐ được lưu và hiển thị đúng số tiền, đơn vị; yêu cầu dùng USD bị từ chối.
- **And**: Không tự quy đổi hoặc đổi nhãn USD thành VNĐ. Yêu cầu bị từ chối không thay đổi bản đang áp dụng hay quyền lợi đã cấp.

#### AC-014

- **Given**: Admin có bản nháp gói thiết kế chưa có quyền lợi; các dữ liệu khác hợp lệ.
- **When**: Admin lưu, mở lại rồi yêu cầu Công bố; sau đó thêm một quyền lợi hợp lệ và Công bố lại.
- **Then**: Lưu nháp được khi danh sách quyền lợi rỗng, nhưng Công bố bị từ chối và báo cần thêm quyền lợi. Sau khi thêm một quyền lợi hợp lệ, gói đạt điều kiện về số lượng quyền lợi để Công bố.
- **And**: Không tự thêm quyền mặc định, không bắt buộc có cả hai quyền tạo mới và tra cứu. Yêu cầu Công bố bị từ chối không thay đổi bản đang áp dụng hoặc quyền đã cấp.

#### AC-015

**Không nghiệm thu trong phạm vi hiện tại:** theo BR-SUB-008 khoản 12, đợt này chưa triển khai logic kiểm tra sử dụng quyền bật/tắt hoặc quyền dạng mức; chỉ hai quyền tạo mới và tra cứu có logic sử dụng. Người dùng xác nhận ngày 25/09/2026. Nội dung giữ để dùng khi danh mục mức được chốt.

- **Given**: Có ba tài khoản với subscription thiết kế còn hiệu lực; bản quyền lợi đã cấp lần lượt chưa có quyền bật/tắt đang xét, có quyền nhưng tắt và có quyền đã bật. Các điều kiện khác để dùng tính năng đều đáp ứng.
- **When**: Mỗi tài khoản yêu cầu sử dụng tính năng tương ứng, gồm cả yêu cầu gửi trực tiếp.
- **Then**: Hai tài khoản chưa có quyền hoặc có quyền đã tắt bị từ chối; tài khoản có quyền đã bật được dùng tính năng khi đủ điều kiện. Không tự bật quyền do thiếu cấu hình.
- **And**: Kiểm tra theo bản quyền lợi đã cấp; việc Admin sửa/công bố gói không đổi quyền của kỳ đang dùng. Chỉ kiểm tra quyền bật/tắt không giữ hoặc trừ lượt.

#### AC-016

**Không nghiệm thu trong phạm vi hiện tại:** theo BR-SUB-008 khoản 12, đợt này chưa triển khai logic kiểm tra sử dụng quyền bật/tắt hoặc quyền dạng mức; chỉ hai quyền tạo mới và tra cứu có logic sử dụng. Người dùng xác nhận ngày 25/09/2026. Nội dung giữ để dùng khi danh mục mức được chốt.

- **Given**: Tài khoản có subscription thiết kế còn hiệu lực nhưng bản quyền lợi đã cấp không có cấu hình mức cho tính năng đang xét; các điều kiện khác để dùng tính năng đều đáp ứng.
- **When**: Khách yêu cầu dùng tính năng ở mức thấp nhất hoặc mức cao hơn, gồm cả yêu cầu gửi trực tiếp.
- **Then**: Hệ thống từ chối cả hai yêu cầu vì chưa được cấp quyền; không tự cấp mức thấp nhất.
- **And**: Kiểm tra theo bản quyền lợi đã cấp. Admin thêm và chọn mức trong bản nháp rồi Công bố không mở quyền ngay cho kỳ đang dùng. Chỉ kiểm tra quyền dạng mức không giữ hoặc trừ lượt.

#### AC-017

**Không nghiệm thu trong phạm vi hiện tại:** phần kiểm tra AI theo quyền 3D hoặc giới hạn danh mục đúng ba quyền đã được thay thế bởi STORY-SUB-002/AC-025. Nội dung giữ để tra lịch sử.

- **Given**: Danh mục có quyền tạo phối cảnh 3D chân thực dạng bật/tắt. Ba tài khoản có kỳ thiết kế còn hiệu lực, còn lượt tạo thiết kế và các điều kiện khác đều đáp ứng; bản quyền lợi đã cấp lần lượt có quyền 3D bật, có quyền 3D tắt và không có quyền 3D.
- **When**: Từng tài khoản bấm Gen AI trên dự án chưa có kết quả thành công, với yêu cầu bộ kết quả gồm ảnh phối cảnh 3D chân thực; kiểm tra cả yêu cầu gửi trực tiếp. Đây là cùng một lần tạo thiết kế, không phải thao tác tạo 3D riêng.
- **Then**: Tài khoản có quyền 3D bật đạt điều kiện về quyền này; hai tài khoản còn lại không được tạo ảnh 3D chân thực. Không tự mở quyền vì tên hoặc giá gói.
- **And**: Admin chọn gói được cấp quyền, không cố định riêng PRO. Quyền 3D dùng chung giữa tháng/năm trong cùng bản gói; thay đổi cấu hình không sửa quyền của kỳ đã cấp. Không tạo loại lượt 3D riêng.

#### AC-018

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Admin đã cấu hình thứ tự BASIC < PLUS < PRO hợp lệ. BASIC năm giá 1.200.000đ, PRO tháng giá 300.000đ; tên và giá chỉ là dữ liệu thử.
- **When**: Hệ thống xác định loại thay đổi cho BASIC năm → PRO tháng, PRO tháng → BASIC năm và PRO tháng → PRO năm.
- **Then**: Lần lượt xác định là nâng gói, hạ gói và chỉ đổi chu kỳ trong cùng gói. Giá kỳ đích thấp hơn hoặc cao hơn không đảo ngược kết quả này.
- **And**: Cả tháng và năm của PRO dùng chung vị trí trong thứ tự. Phân loại theo thứ tự không tự cộng quyền từ các gói khác. Các quy tắc hiệu lực sau phân loại vẫn theo BR-SUB-018, BR-SUB-019 và BR-SUB-015.

#### AC-019

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Admin đang cấu hình thứ tự cho các gói thiết kế khác nhau. Gói A đã có bậc 1; hệ thống có cấu hình hợp lệ trước đó. Các số bậc chỉ là dữ liệu thử.
- **When**: Admin gửi cấu hình đặt gói B cũng ở bậc 1, kể cả yêu cầu gửi trực tiếp.
- **Then**: Hệ thống từ chối cấu hình trùng bậc, báo cần đặt bậc riêng cho từng gói và giữ nguyên cấu hình hợp lệ trước đó. Không tự đổi bậc hoặc so sánh giá để quyết định nâng/hạ.
- **And**: Khi Admin đặt B ở một bậc khác chưa trùng gói nào, cấu hình đạt điều kiện không trùng bậc; vẫn phải đáp ứng các điều kiện hợp lệ khác. Tháng và năm của A cùng dùng bậc 1 của A, không bị coi là hai gói trùng bậc.

#### AC-020

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: Gói PLUS ở bậc 2 đã có khách sử dụng; bậc 4 chưa được gói nào dùng. Các bậc và tên chỉ là dữ liệu thử.
- **When**: Admin thử đổi PLUS sang bậc 4, kể cả yêu cầu gửi trực tiếp hoặc công bố bản cấu hình làm đổi bậc.
- **Then**: Hệ thống từ chối, báo gói đã có khách sử dụng nên không được đổi bậc và vẫn giữ PLUS ở bậc 2. Không thay đổi quyền, kỳ sử dụng hoặc yêu cầu chuyển đang chờ do thao tác bị từ chối.
- **And**: Quy tắc vẫn áp dụng nếu các kỳ cũ đã hết hạn. Admin có thể tạo gói mới ở bậc riêng hợp lệ, nhưng việc đó không tự chuyển khách sang gói mới. Sửa giá/quyền lợi vẫn theo quy trình hiện có; khóa bậc không khóa toàn bộ cấu hình.

#### AC-021

**Lịch sử — đã được thay thế bởi BR-SUB-021; không dùng nghiệm thu hoặc triển khai.**

- **Given**: BASIC đang bán và đã có yêu cầu chuyển gói thiết kế hợp lệ sang BASIC, chờ áp dụng ở kỳ tiếp theo.
- **When**: Admin ngừng bán BASIC trước thời điểm chuyển.
- **Then**: BASIC bị ẩn khỏi danh sách đăng ký và chặn yêu cầu mới. Yêu cầu chuyển đã ghi nhận vẫn được giữ và thực hiện theo lịch nếu chưa hủy và đủ điều kiện chuyển hợp lệ.
- **And**: Không chuyển sớm hoặc thay đổi quyền/lượt kỳ đang dùng vì thao tác ngừng bán. Yêu cầu mới tạo sau khi khách hủy yêu cầu cũ không được hưởng ngoại lệ này.

#### AC-022

**Không nghiệm thu trong phạm vi hiện tại:** phần kiểm tra AI theo quyền 3D hoặc giới hạn danh mục đúng ba quyền đã được thay thế bởi STORY-SUB-002/AC-025. Nội dung giữ để tra lịch sử.

- **Given**: Admin cấu hình gói thiết kế bằng danh mục quyền lợi tạm thời.
- **When**: Mở danh mục quyền có thể chọn.
- **Then**: Có đúng ba quyền: tạo thiết kế và mở chi tiết mẫu dạng lượt; phối cảnh 3D chân thực dạng bật/tắt. Không có quyền chỉnh sửa, so sánh, tối ưu ngân sách hoặc quyền phân mức cụ thể trong danh mục này.
- **And**: Admin chọn một hoặc cả hai quyền dạng lượt như đã chốt; tháng/năm có hạn mức riêng, 3D dùng chung trong cùng bản gói. Không tự điền hạn mức/giá từ website hoặc mặc định chỉ PRO có 3D. Hệ thống vẫn hỗ trợ dạng mức để dùng khi có danh mục được chốt sau này; giám sát không thuộc giới hạn ba quyền thiết kế này.

#### AC-023

- **Given**: Admin có quyền sửa nội dung gói; tư vấn đang vận hành offline.
- **When**: Admin nhập hoặc sửa mô tả tư vấn của gói.
- **Then**: Cho nhập nội dung tự do, không bắt chọn từ ba mức Online, Ưu tiên, Chuyên gia 1:1 và không có trường mức tư vấn riêng.
- **And**: Nội dung này không thêm quyền tính năng, lượt tư vấn hoặc logic phân quyền vào danh mục thiết kế tạm thời. Quy trình lưu nháp/Công bố vẫn theo gói; cam kết tư vấn của kỳ đã mua được giữ theo AC-024.

#### AC-024

- **Given**: Khách đã mua kỳ PLUS với mô tả tư vấn “Tư vấn ưu tiên”. Admin có bản nháp mô tả mới “Tư vấn online”.
- **When**: Admin công bố mô tả mới trong lúc kỳ của khách còn hiệu lực.
- **Then**: Cam kết tư vấn của kỳ đã mua vẫn là “Tư vấn ưu tiên” đến khi kỳ đó kết thúc; không đổi theo mô tả mới. Lần mua mới sử dụng nội dung mới đã được chốt hợp lệ cho lần mua đó.
- **And**: Mô tả tự do không tạo entitlement, mức hoặc lượt tư vấn. Sửa mô tả không tự đổi gói, gia hạn hoặc làm mới lượt. Nội dung tư vấn chốt lúc tạo đơn theo BR-PAY-001; không tự mở rộng quy tắc này sang quà tặng.

#### AC-025

- **Given**: Admin cấu hình quyền lợi gói thiết kế; quyền 3D vẫn thuộc danh mục hệ thống.
- **When**: Admin bật hoặc tắt 3D, lưu bản nháp và công bố hợp lệ.
- **Then**: Cấu hình 3D được lưu và hiển thị theo bản gói đã công bố; không tự thêm logic kiểm tra quyền 3D, thay đổi đầu ra AI hoặc phát sinh lượt 3D.
- **And**: Hiện chỉ tạo thiết kế mới và tra cứu mẫu có logic sử dụng/tính lượt. Bật 3D không tự cấp lượt tạo, không thay hạn mức hai quyền này. Danh mục không bị giới hạn đúng ba mục; quyền bổ sung ngoài hai quyền dạng lượt đều bật/tắt để hiển thị; tên danh mục cụ thể cần đối chiếu riêng. Không coi quyết định này là luôn tạo hoặc luôn bỏ 3D trong AI.

#### AC-026

- **Given**: Admin cấu hình các quyền lợi đã được định nghĩa trong danh mục gói.
- **When**: Admin cấu hình tạo thiết kế mới, tra cứu mẫu và các quyền lợi còn lại.
- **Then**: Hai quyền tạo mới/tra cứu dùng hạn mức riêng theo tháng/năm hoặc không giới hạn; các quyền lợi còn lại dùng bật/tắt để lưu và hiển thị theo gói, không có mức cụ thể hoặc bộ đếm sử dụng riêng trong đợt này.
- **And**: Bật/tắt các quyền hiển thị không thay số dư, cấp thêm lượt hoặc điều khiển AI/dịch vụ. Tư vấn vẫn offline với nội dung mô tả tự do; không bắt chọn Online/Ưu tiên/1:1. Lưu nháp/Công bố vẫn áp dụng, không tự thay quyền hoặc cam kết của kỳ đã mua.

#### AC-027

- **Given**: Admin có bản nháp gói giám sát với tên và giá hợp lệ, chưa chọn quyền lợi nào.
- **When**: Admin Công bố khi mô tả dịch vụ còn trống, sau đó nhập mô tả "6 buổi kỹ sư kiểm tra tại công trình" và Công bố lại.
- **Then**: Lần đầu bị từ chối vì thiếu mô tả dịch vụ; lần sau Công bố thành công dù gói không có quyền lợi nào trong danh mục.
- **And**: Mô tả chỉ để hiển thị và được chốt theo đơn; không tạo lượt, bộ đếm hoặc logic sử dụng trên nền tảng.

## References

### TDDs

- [TDD-SUB-001](../tdd/TDD-SUB-001.md): Thiết kế kỹ thuật bản nháp; Reviewer/Approver Tân Trần.
- [TDD-SUB-002](../tdd/TDD-SUB-002.md): Logic sử dụng của hai quyền dạng lượt tạo mới và tra cứu theo AC-026.

### Rules

- BR-SUB-021/Statement: Đổi gói và chu kỳ ngay; bỏ phân loại nâng/hạ và lịch chuyển.

- BR-SUB-017/Statement: Hai loại lượt tạo mới và tra cứu, không có lượt chỉnh sửa.

- BR-SUB-005/Statement: Ba dạng quyền lợi được hỗ trợ.
- BR-SUB-004/Statement: Lưu nháp và công bố thay đổi quyền lợi.

- BR-SUB-008/Statement: Admin chọn quyền lợi từ danh mục hệ thống, không tự tạo định nghĩa mới.

- BR-SUB-013/Statement: Ngừng bán chặn yêu cầu mới, giữ gói đã cấp.

- BR-SUB-015/Statement: Hai lựa chọn tháng/năm trong cùng gói thiết kế, giá và hạn mức riêng.

### Dependencies

- STORY-SUB-001: Sử dụng quyền lợi và hạn mức theo tài khoản.

## Non-Functional

- [Chưa chốt yêu cầu đo được về hiệu năng và thao tác đồng thời.]

## Out of Scope

- Quản lý cam kết tư vấn của gói trên nền tảng: ba mức Online, Ưu tiên và Chuyên gia 1:1 vẫn được thực hiện offline. Phần mô tả quyền lợi dịch vụ không tự tạo entitlement dạng mức, lịch hẹn hoặc lượt tư vấn trên nền tảng. Yêu cầu tư vấn KTS miễn phí ở STORY-CONSULT-002 là kênh riêng, không thay cam kết tư vấn của gói (người dùng xác nhận ngày 25/09/2026). Danh mục thiết kế theo AC-025.

- So sánh các kết quả thiết kế đã hoãn; không triển khai giao diện so sánh hoặc quyền riêng theo gói trong đợt này. Khách vẫn xem từng kết quả riêng. Xem [nợ nghiệp vụ](../debt/design-comparison.md).

- AI gợi ý giảm chi phí/tối ưu ngân sách đã hoãn; không triển khai tính năng hoặc quyền riêng cho phần này trong đợt hiện tại. Dự toán nội thất vẫn giữ như đã chốt. Xem [nợ nghiệp vụ](../debt/ai-budget-optimization.md).

- Chốt hoặc tự nhập dữ liệu giá/hạn mức bán ban đầu: Admin sẽ nhập sau. Giá và số lượt trong các AC, test là dữ liệu minh họa; không phải cấu hình bán đã được duyệt.

- Luồng mua và nhận gói thuộc STORY-PAY-001; không có tự động gia hạn.
- Tên cụ thể của các quyền lợi hiển thị ngoài hai quyền dạng lượt, các điều kiện công bố chi tiết và quy trình thay đổi danh mục trong các lần phát triển sau chưa được chốt.
