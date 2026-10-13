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

# STORY-PROJ-008

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo dự toán chi phí từ bản vẽ 2D đã có để biết chi phí phần thô, hoàn thiện và nội thất của phương án.
- **Context**: Một trong hai chặng chạy cùng lúc sau chặng 2D (BR-PROJ-010): khách bấm Đi tiếp một lần thì Dự toán và Phối cảnh 3D (STORY-PROJ-009) cùng được tiếp nhận. Người dùng chốt ngày 13/10/2026 theo yêu cầu khách hàng giảm thời gian chờ. Backend tự lập dự toán bằng AI (Anthropic) theo hai tài liệu luật giá do Admin quản lý (BR-PROJ-016), miền giá đã chốt (BR-PROJ-013) và các lưới an toàn của app demo `design-estimate` (commit `059e876`, prompt `dt-14`). Chặng này không tính thêm lượt. Lần chạy thử app demo ngày 11/10/2026 cho nhà phố 2 tầng ra 84 dòng dự toán. Người dùng chốt nghiệp vụ trong hội thoại ngày 10–11/10/2026.
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
- Chặng Bản vẽ 2D đã thành công theo STORY-PROJ-002.
- Chặng Dự toán chưa thành công, không có lần chạy Dự toán đang chờ hoặc đang chạy và chưa đạt giới hạn số lần lỗi theo BR-PROJ-011.

### Trigger

Khách bấm Đi tiếp ở màn kết quả 2D (chạy cùng lúc với chặng Phối cảnh 3D), hoặc bấm Thử lại khi lần chạy trước bị lỗi.

## Flow

### Main Flow

1. Khách bấm Đi tiếp ở màn kết quả 2D.
2. Backend kiểm tra quyền sở hữu và điều kiện của cả hai chặng Dự toán và Phối cảnh 3D theo BR-PROJ-010 khoản 1–2. Hai chặng cùng được tiếp nhận hoặc cùng bị từ chối. Không kiểm tra gói hay lượt.
3. Backend tiếp nhận lần chạy, chốt phiên bản hai tài liệu luật giá cùng dải giá đi kèm (BR-PROJ-016) và cấu hình AI (BR-PROJ-014).
4. Hệ thống tính khối lượng tham chiếu từ phương án 2D đã lưu, rồi gọi AI lập dự toán theo luật giá, miền giá và gói hoàn thiện đã chốt.
5. Hệ thống áp dụng các lưới an toàn của app demo: hỏi lại AI khi thiếu trường bắt buộc; kiểm tra dòng 0 đồng, khối lượng lệch so với hình học, đơn giá so với sàn và trần của dải giá; tự nâng đơn giá khi dưới sàn. Cờ và ghi chú phát sinh được lưu nội bộ theo BR-PROJ-017.
6. Hệ thống lọc nội dung nội bộ khỏi phần gửi khách, rồi lưu kết quả gồm phần gửi khách và phần nội bộ.
7. Khi kết quả đủ trường bắt buộc và đã lưu, chặng Dự toán thành công. Nếu chặng Phối cảnh 3D cũng đã thành công, chặng Hồ sơ chuyển sang Sẵn sàng và khách chuyển được sang bước Hồ sơ (STORY-PROJ-010).
8. Trong lúc chạy, tab Dự toán của bước Dự toán & 3D hiện tiến độ và mẫu 3D tham khảo theo STORY-LIB-002. Khi xong, khách xem kết quả ở tab này ngay, không chờ chặng 3D: tổng dự toán, đơn giá trên m² sàn quy đổi, ba phần thô, hoàn thiện, nội thất với từng dòng (hạng mục, đơn vị, khối lượng, đơn giá, thành tiền), thời gian thi công dự kiến và nội dung tư vấn. Màn không hiện hãng vật tư, giống app demo. PDF dự toán và Excel của chặng Hồ sơ vẫn có Vật liệu/Hãng theo demo (người dùng xác nhận sửa lại phạm vi ngày 13/10/2026).

### Alternative Flow

#### ALT-01

Lần chạy Dự toán lỗi, chưa đạt giới hạn số lần lỗi.

1. Khách bấm Thử lại; hệ thống chạy lại chặng Dự toán, cùng chặng 3D nếu chặng đó cũng đang lỗi và còn được chạy lại (BR-PROJ-010 khoản 3).
2. Backend kiểm tra điều kiện mở chặng; không kiểm tra gói hay lượt.
3. Quay lại bước 3 của Main Flow. Lần chạy lại chốt phiên bản luật giá và cấu hình đang áp dụng tại thời điểm chạy lại.

#### ALT-02

Gói thiết kế của khách đã hết hạn hoặc hết lượt sau khi 2D thành công.

1. Vẫn cho chạy chặng Dự toán theo BR-PROJ-010 khoản 5.
2. Không giữ, trừ hay hoàn lượt.

### Exception Flow

#### EXC-01

Chặng 2D chưa thành công, chặng Dự toán đã thành công, đang có lần chạy Dự toán chờ hoặc đang chạy, đã đạt giới hạn số lần lỗi, hệ thống đang quá tải hoặc dịch vụ AI đang tạm ngắt theo BR-PROJ-012, luồng bốn chặng đang tắt, hoặc chặng Phối cảnh 3D được yêu cầu chạy cùng lần bấm không được tiếp nhận.

1. Từ chối yêu cầu trước khi tiếp nhận, kèm lý do. Khi chặng 3D cũng được yêu cầu trong cùng lần bấm, từ chối cả hai chặng theo BR-PROJ-010 khoản 2; không tạo lần chạy và không tính vào bộ đếm lỗi.

#### EXC-02

Lượt AI chính trả thiếu trường bắt buộc, bị cắt ngang, từ chối trả lời, hoặc dịch vụ AI lỗi sau khi đã thử lại theo cấu hình; hoặc kết quả cuối vẫn thiếu trường bắt buộc sau các lượt hỏi lại.

1. Lần chạy lỗi. Tăng bộ đếm lỗi của chặng Dự toán khi AI trả thiếu hoặc từ chối trả lời. Không tăng khi kết quả AI bị cắt ngang vì vượt giới hạn độ dài đầu ra, hoặc khi dịch vụ AI lỗi hay không phản hồi sau khi đã thử lại; đây là lỗi hệ thống (BR-PROJ-011 khoản 1).
2. Khi kết quả AI bị cắt ngang, hệ thống gửi cảnh báo vận hành (Discord) để Admin nâng giới hạn độ dài đầu ra.
3. Lượt hỏi lại của các lưới an toàn bị lỗi (kể cả bị cắt ngang, hết tiền hoặc dịch vụ AI lỗi) thì làm như app demo: giữ kết quả trước lượt hỏi lại, ghi chú nội bộ và chạy tiếp; cảnh báo vận hành vẫn được gửi. Lần chạy chỉ lỗi nếu kết quả cuối vẫn không đạt kiểm tra.
4. Không công bố phần dự toán dở dang; kết quả 2D giữ nguyên.

#### EXC-03

Lần chạy quá hạn chờ (mặc định 20 phút).

1. Lần chạy lỗi do quá hạn theo BR-PROJ-012; kết quả đến muộn bị bỏ.
2. Không ảnh hưởng lượt và không tăng bộ đếm lỗi.

#### EXC-04

Dịch vụ AI báo hết tiền hoặc khóa truy cập bị từ chối.

1. Ở lượt AI chính: lần chạy lỗi ngay, không thử lại và không tăng bộ đếm lỗi. Ở lượt hỏi lại: giữ kết quả trước lượt hỏi lại và chạy tiếp như app demo.
2. Hệ thống tạm ngắt nhận lần chạy mới dùng dịch vụ đó theo BR-PROJ-012 khoản 9 và gửi cảnh báo cho Admin qua kênh cảnh báo hiện có; khách thấy thông báo lỗi chung và chạy lại sau.

## Acceptance Criteria

#### AC-001

- **Given**: Chặng 2D đã thành công; chặng Dự toán và chặng 3D chưa chạy.
- **When**: Khách bấm Đi tiếp.
- **Then**: Backend tiếp nhận cùng lúc lần chạy Dự toán và lần chạy 3D; lần chạy Dự toán chốt phiên bản luật giá, dải giá và cấu hình AI đang áp dụng.
- **And**: Không kiểm tra gói, không giữ hay trừ lượt.

#### AC-002

- **Given**: Chặng 2D chưa thành công.
- **When**: Khách yêu cầu chạy chặng Dự toán, kể cả gửi trực tiếp tới backend.
- **Then**: Từ chối kèm lý do chặng trước chưa xong.
- **And**: Không tạo lần chạy.

#### AC-003

- **Given**: AI trả dự toán đủ trường bắt buộc và hệ thống đã lưu kết quả.
- **When**: Lần chạy kết thúc.
- **Then**: Chặng Dự toán thành công; khách xem được tổng dự toán, đơn giá trên m² sàn quy đổi, ba phần thô, hoàn thiện, nội thất cùng từng dòng chi tiết, thời gian thi công dự kiến và tư vấn.
- **And**: Khách xem được kết quả Dự toán ngay khi chặng này xong, kể cả khi chặng 3D còn đang chạy; chặng Hồ sơ chỉ chuyển sang Sẵn sàng khi chặng 3D cũng đã thành công.

#### AC-004

- **Given**: AI trả đơn giá tổng dưới sàn dải giá của loại công trình và gói (ví dụ 4,42 triệu/m² so với sàn 6,96 triệu/m²).
- **When**: Hệ thống áp lưới an toàn giá.
- **Then**: Đơn giá được tự nâng tới sàn, giữ nguyên khối lượng; khách thấy số liệu sau khi nâng.
- **And**: Lý do và hệ số nâng chỉ nằm trong phần nội bộ theo BR-PROJ-017.

#### AC-005

- **Given**: Kết quả AI có câu nhắc tới khung giá nội bộ, chỉ tiêu nội bộ hoặc số dòng tối thiểu.
- **When**: Hệ thống lưu phần gửi khách.
- **Then**: Các câu hoặc vế câu đó bị lọc khỏi nội dung khách thấy trên màn hình, Excel, PDF và trang chia sẻ.
- **And**: Cờ “cần kỹ sư rà” hoặc “cần nhân viên rà” nếu có chỉ lưu nội bộ và không chặn chặng thành công.

#### AC-006

- **Given**: Kết quả cuối vẫn thiếu trường bắt buộc sau lượt hỏi lại, hoặc ở lượt AI chính: AI từ chối xử lý nội dung khách gửi, kết quả bị cắt ngang vì vượt giới hạn độ dài đầu ra, hoặc dịch vụ AI lỗi sau khi đã thử lại.
- **When**: Hệ thống kết thúc lần chạy.
- **Then**: Lần chạy lỗi; không công bố dự toán dở dang. Bộ đếm lỗi của chặng Dự toán chỉ tăng khi AI vẫn thiếu trường bắt buộc hoặc AI từ chối xử lý nội dung khách gửi. Kết quả bị cắt ngang và dịch vụ AI lỗi là lỗi hệ thống nên không tăng bộ đếm (BR-PROJ-011 khoản 1).
- **And**: Khi kết quả bị cắt ngang, Admin nhận cảnh báo vận hành (Discord) để nâng giới hạn. Kết quả 2D giữ nguyên; khách được chạy lại khi chưa đạt giới hạn.

#### AC-007

- **Given**: Chặng Dự toán đã lỗi; gói của khách đã hết hạn.
- **When**: Khách bấm Thử lại.
- **Then**: Lần chạy được tiếp nhận; không kiểm tra gói, không giữ hay trừ lượt.
- **And**: Lần chạy lại dùng phiên bản luật giá đang áp dụng lúc chạy lại.

#### AC-008

- **Given**: Chặng Dự toán đã thành công với luật giá phiên bản 3.
- **When**: Admin lưu luật giá phiên bản 4.
- **Then**: Dự toán đã có giữ nguyên số liệu và vẫn ghi phiên bản 3.
- **And**: Khách không chạy lại được chặng Dự toán đã thành công.

#### AC-009

- **Given**: Dịch vụ AI báo hết tiền hoặc từ chối khóa truy cập.
- **When**: Hệ thống gọi lượt AI chính của lần chạy Dự toán. (Ở lượt hỏi lại, lần chạy giữ kết quả trước đó và chạy tiếp như app demo; Admin vẫn nhận cảnh báo.)
- **Then**: Lần chạy lỗi ngay, không thử lại; Admin nhận cảnh báo.
- **And**: Khách thấy thông báo lỗi chung, không thấy tên nhà cung cấp hay chi tiết khóa.

#### AC-010

- **Given**: Khách đã bấm Đi tiếp; lần chạy Dự toán lỗi và chặng Dự toán đã đạt giới hạn số lần lỗi; lần chạy 3D cũng lỗi nhưng chặng 3D còn được chạy lại.
- **When**: Khách bấm Thử lại.
- **Then**: Chỉ chặng 3D được chạy lại; chặng Dự toán không chạy lại và khách được báo liên hệ hỗ trợ cho chặng Dự toán.
- **And**: Chặng Hồ sơ vẫn Chưa mở.

## References

### TDDs

- TDD-PROJ-006
- TDD-PROJ-008
- TDD-PROJ-009

### Rules

- BR-PROJ-007
- BR-PROJ-010
- BR-PROJ-011
- BR-PROJ-012
- BR-PROJ-013
- BR-PROJ-014
- BR-PROJ-016
- BR-PROJ-017
- BR-PROJ-019

### Dependencies

- STORY-PROJ-002
- STORY-PROJ-009: chạy cùng lúc với chặng Dự toán.
- STORY-PROJ-011: Admin quản lý luật giá và vùng giá.
- STORY-LIB-002

## Non-Functional

- Hạn chờ chặng Dự toán mặc định 20 phút, Admin cấu hình được (BR-PROJ-012).
- Với cùng phản hồi AI đã ghi, kết quả sau lưới an toàn của hệ thống thật phải khớp app demo ở mốc `059e876` (số dòng, thành tiền, tổng, cờ nội bộ), kể cả cách làm tròn số.
- Bộ nhớ đệm prompt phía nhà cung cấp (prompt cache) mặc định tắt như app demo: số đo của app demo cho thấy khi các dự toán cách nhau quá 5 phút, bật lên làm tiền token vào của lượt chính tăng khoảng 9% (khoảng 3% chi phí của lượt đó) mà không lần nào trúng bộ nhớ đệm. Admin bật được trong cấu hình AI khi lượng dự toán đủ dày. Đây là ghi chú kỹ thuật, không đổi tiêu chí; thiết kế ở TDD-PROJ-009.

## Out of Scope

- Khách sửa dòng dự toán, đổi gói hay đổi hãng vật tư sau khi chặng đã thành công.
- Bước duyệt của kỹ sư hoặc nhân viên trước khi khách xem.
- Chạy riêng chặng Dự toán mà không kèm chặng 3D khi cả hai chặng đều chưa thành công.
- Tải Excel dự toán ở chặng này; Excel thuộc bộ tệp của chặng Hồ sơ (STORY-PROJ-010).
- Sửa các lỗi chất lượng đã thấy ở app demo (dòng mái bị loại vì 0 đồng); sẽ làm sau bằng phiên bản prompt hoặc luật mới theo BR-PROJ-014.
