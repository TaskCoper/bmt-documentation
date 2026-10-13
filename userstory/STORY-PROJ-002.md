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

# STORY-PROJ-002

## Metadata

- **Story**: Là khách hàng, tôi muốn tạo bản vẽ 2D từ đầu vào của bản dự toán, theo dõi tiến độ và chạy lại khi lỗi để có phương án mặt bằng làm nền cho dự toán, phối cảnh 3D và hồ sơ.
- **Context**: Chặng đầu tiên trong bốn chặng của BR-PROJ-010: Bản vẽ 2D, rồi Dự toán và Phối cảnh 3D chạy cùng lúc, sau cùng là Hồ sơ; thay cho bước “gửi AI một lần, nhận toàn bộ kết quả” trước đây. Luồng chuyển từ app demo `design-estimate` (commit `059e876`, prompt `2d-13`) do khách hàng dựng để mô tả hành vi mong muốn; backend BMT tự gọi OpenAI, không còn AI service của bên khác. Một lượt tạo dự toán được giữ khi tiếp nhận chặng này và tính là đã dùng khi 2D thành công; các chặng sau không tính thêm lượt. Người dùng chốt nghiệp vụ trong hội thoại ngày 10–11/10/2026; xác nhận hội thoại không thay cho phê duyệt trên hệ thống quản lý tài liệu.
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
- Đầu vào của bản dự toán được chuẩn bị theo STORY-PROJ-001 và đủ điều kiện theo BR-PROJ-001, BR-PROJ-002, BR-PROJ-004; ảnh lô đất đang gắn có kết quả soi Đạt theo BR-PROJ-015.
- Khách có gói thiết kế còn hiệu lực, quyền tạo thiết kế và lượt sẵn dùng hoặc không giới hạn theo BR-SUB-007.
- Chặng 2D của bản dự toán chưa thành công, không có chặng nào đang chạy và chưa đạt giới hạn số lần lỗi theo BR-PROJ-011.
- Luồng bốn chặng đang được bật trên môi trường theo BR-PROJ-018.

### Trigger

Khách bấm Tạo bản vẽ 2D, hoặc bấm Chạy lại khi lần chạy 2D trước bị lỗi.

## Flow

### Main Flow

1. Khách bấm Tạo bản vẽ 2D.
2. Backend kiểm tra quyền sở hữu, điều kiện mở chặng theo BR-PROJ-010, đầu vào, kết quả soi ảnh lô, gói và lượt.
3. Khi hợp lệ, backend tiếp nhận lần chạy: giữ một lượt nếu hạn mức hữu hạn theo BR-SUB-003, khóa sửa đầu vào theo BR-PROJ-005, chốt đầu vào cùng miền giá (BR-PROJ-013) và cấu hình AI (BR-PROJ-014) cho lần chạy.
4. Hệ thống lập phương án mặt bằng từ ảnh lô, mô tả và các ô đầu vào: số tầng, danh sách phòng, diện tích từng phòng theo luật lô đất và luật phương án của app demo.
5. Hệ thống vẽ, cho từng tầng, một bản vẽ kỹ thuật và một bản vẽ bố trí nội thất theo mức đồ đạc của gói hoàn thiện. Bản vẽ AI không trả được ảnh, hoặc có viền nhà lệch tỷ lệ quá ngưỡng so với lô, được vẽ lại 1 lần theo BR-PROJ-011 khoản 7.
6. Trong lúc chạy, khách thấy chặng 2D ở trạng thái Đang chạy kèm tiến độ và mẫu 2D tham khảo phù hợp với dự án theo STORY-LIB-002; khách được rời trang và quay lại xem tiếp.
7. Khi phương án và toàn bộ bản vẽ đã được tạo, kiểm tra và lưu, chặng 2D thành công. Lượt đang giữ chuyển thành lượt đã dùng theo BR-SUB-003; đầu vào bị khóa vĩnh viễn theo BR-PROJ-005.
8. Khách xem kết quả 2D: phương án theo từng tầng (tên phòng, diện tích) và các bản vẽ. Chặng Dự toán và chặng Phối cảnh 3D chuyển sang Sẵn sàng; khách bấm Đi tiếp để chạy cùng lúc hai chặng này (STORY-PROJ-008, STORY-PROJ-009). Hệ thống không tự chạy tiếp.

### Alternative Flow

#### ALT-01

Lần chạy 2D bị lỗi hoặc quá hạn, bản dự toán chưa có 2D thành công và chưa đạt giới hạn số lần lỗi.

1. Khách mở lại đầu vào; được sửa khi đáp ứng điều kiện lưu theo BR-SUB-007 và BR-PROJ-005.
2. Khách bấm Chạy lại, có hoặc không sửa đầu vào.
3. Backend kiểm tra lại toàn bộ điều kiện như lần đầu, kể cả gói, lượt và kết quả soi ảnh; không dùng quyền hay lượt của lần trước.
4. Khi hợp lệ, quay lại bước 3 của Main Flow với một lượt mới được giữ.

#### ALT-02

Gói hết hạn hoặc sang kỳ mới trong khi lần chạy 2D đã được tiếp nhận còn đang chạy.

1. Không hủy lần chạy chỉ vì hết hạn gói hoặc đổi kỳ.
2. Lượt được ghi nhận vào kỳ đã giữ theo BR-SUB-003; lỗi thì hoàn ở kỳ đó, không cộng sang kỳ mới.
3. Hạn chờ của chặng vẫn áp dụng theo BR-PROJ-012.

#### ALT-03

Hệ thống bị gián đoạn kỹ thuật giữa lần chạy, chẳng hạn máy chủ khởi động lại.

1. Hệ thống tự tiếp tục lần chạy từ bước đã lưu gần nhất, trong hạn chờ còn lại.
2. Không tính là lỗi, không giữ thêm lượt và không gọi lại các bước AI đã có kết quả lưu.

### Exception Flow

#### EXC-01

Đầu vào thiếu hoặc không hợp lệ, ảnh lô chưa Đạt, khách không đủ quyền, gói hoặc lượt, chặng 2D đã thành công, đang có chặng chạy, đã đạt giới hạn số lần lỗi, hệ thống đang quá tải hoặc dịch vụ AI đang tạm ngắt theo BR-PROJ-012, hoặc luồng bốn chặng đang tắt.

1. Từ chối yêu cầu trước khi tiếp nhận, kèm lý do; không giữ hay trừ lượt, không tạo lần chạy và không tính vào bộ đếm lỗi.
2. Không thay đổi kết quả 2D đã thành công.

#### EXC-02

Có yêu cầu thay đổi đầu vào sau khi lần chạy 2D đã được tiếp nhận.

1. Từ chối thay đổi theo BR-PROJ-005, kể cả tự lưu đến muộn hoặc gửi trực tiếp.
2. Giữ nguyên đầu vào đã chốt của lần chạy.
3. Đổi tên bản dự toán vẫn được phép theo BR-SUB-007 khoản 11.

#### EXC-03

Lần chạy 2D lỗi, kể cả bản vẽ vẫn hỏng sau 1 lần vẽ lại, hoặc quá hạn chờ.

1. Ghi nhận lỗi và hoàn lượt đang giữ theo BR-SUB-003, BR-SUB-016. Chỉ tăng bộ đếm lỗi của chặng 2D khi lỗi do kết quả AI với đầu vào của khách (ví dụ bản vẽ vẫn không có ảnh sau lần vẽ lại); lỗi hệ thống như dịch vụ AI không phản hồi hoặc quá hạn chờ không tăng bộ đếm (BR-PROJ-011 khoản 1).
2. Không công bố phần kết quả dở dang.
3. Mở lại khả năng sửa đầu vào; khách chạy lại theo ALT-01 khi còn đủ điều kiện. Hệ thống không tự chạy lại.

#### EXC-04

Kết quả đến sau khi lần chạy đã lỗi do quá hạn.

1. Giữ trạng thái lỗi; không công bố kết quả muộn, không tính lượt theo BR-PROJ-012 và BR-SUB-016.
2. Không gắn kết quả này vào lần chạy lại.

#### EXC-05

Chặng 2D đã đạt giới hạn số lần lỗi.

1. Từ chối chạy lại và báo khách liên hệ hỗ trợ theo BR-PROJ-011.
2. Sau khi Admin đặt lại bộ đếm, khách chạy lại theo ALT-01.

## Acceptance Criteria

#### AC-001

- **Given**: Khách đủ điều kiện; chặng 2D chưa thành công; đầu vào hợp lệ và ảnh lô đã soi Đạt.
- **When**: Backend tiếp nhận yêu cầu Tạo bản vẽ 2D.
- **Then**: Giữ một lượt nếu hạn mức hữu hạn, khóa sửa đầu vào và chốt đầu vào, miền giá, cấu hình AI cho lần chạy.
- **And**: Chưa ghi lượt đã dùng chỉ vì đã tiếp nhận.

#### AC-002

- **Given**: Lần chạy 2D đã được tiếp nhận và chưa kết thúc.
- **When**: Có yêu cầu lưu thay đổi đầu vào, kể cả gửi trực tiếp hoặc tự lưu đến muộn.
- **Then**: Backend từ chối thay đổi đó.
- **And**: Đầu vào đã chốt của lần chạy giữ nguyên; đổi tên bản dự toán vẫn được phép.

#### AC-003

- **Given**: Phương án và bản vẽ kỹ thuật, bản vẽ nội thất của mọi tầng đã được tạo, kiểm tra và lưu.
- **When**: Hệ thống kết thúc lần chạy 2D.
- **Then**: Chặng 2D thành công; lượt đang giữ chuyển thành lượt đã dùng theo BR-SUB-003.
- **And**: Không chờ khách mở xem hay duyệt; các chặng sau không trừ thêm lượt.

#### AC-004

- **Given**: Lần chạy 2D lỗi hoặc quá hạn, chưa có 2D thành công.
- **When**: Backend kết thúc lần chạy với trạng thái lỗi.
- **Then**: Hoàn lượt đang giữ, tăng bộ đếm lỗi của chặng 2D, giữ đầu vào và mở lại khả năng sửa.
- **And**: Chỉ chạy lại khi khách bấm Chạy lại; mỗi lần chạy lại kiểm tra lại quyền, gói, lượt, đầu vào và giữ một lượt mới.

#### AC-005

- **Given**: Lần chạy 2D đã lỗi do quá hạn và đã hoàn lượt.
- **When**: Kết quả của lần chạy đó đến muộn.
- **Then**: Không công bố, không đổi lần chạy thành thành công, không tính lại lượt.
- **And**: Không đưa kết quả vào lần chạy lại.

#### AC-006

- **Given**: Chặng 2D của bản dự toán đã thành công.
- **When**: Khách yêu cầu tạo lại 2D hoặc sửa đầu vào.
- **Then**: Từ chối; kết quả 2D cũ giữ nguyên và đầu vào vẫn khóa.
- **And**: Không giữ hay trừ lượt cho yêu cầu bị từ chối; muốn phương án khác phải tạo bản dự toán mới.

#### AC-007

- **Given**: Lần chạy 2D đã được tiếp nhận rồi gói hết hạn hoặc chuyển kỳ.
- **When**: Lần chạy kết thúc thành công hoặc lỗi trong hạn chờ.
- **Then**: Xử lý lượt tại kỳ đã giữ theo BR-SUB-003.
- **And**: Không cộng lượt lỗi sang kỳ mới và không hủy lần chạy chỉ vì gói hết hạn.

#### AC-008

- **Given**: Ảnh lô đang gắn với bản dự toán có kết quả soi Không đạt, đang soi hoặc soi lỗi.
- **When**: Khách bấm Tạo bản vẽ 2D.
- **Then**: Từ chối kèm lý do ảnh lô chưa đạt theo BR-PROJ-015.
- **And**: Không giữ lượt, không khóa đầu vào và không tạo lần chạy.

#### AC-009

- **Given**: Trong lần chạy 2D, AI không trả được ảnh cho bản vẽ của một tầng, hoặc viền nhà trên bản vẽ lệch tỷ lệ quá 5% so với lô.
- **When**: Hệ thống xử lý bản vẽ đó.
- **Then**: Vẽ lại đúng bản vẽ đó 1 lần. AI vẫn không trả được ảnh thì lần chạy lỗi theo EXC-03; viền vẫn lệch thì giữ bản có độ lệch nhỏ hơn, gắn cờ nội bộ và lần chạy tiếp tục.
- **And**: Lần vẽ lại không tính vào bộ đếm lỗi và không kéo dài hạn chờ; khi Admin tắt cờ vẽ lại vì viền lệch, viền lệch chỉ được ghi lại, không vẽ lại.

#### AC-010

- **Given**: Chặng 2D đã lỗi đúng bằng giới hạn đang cấu hình (mặc định 3 lần).
- **When**: Khách bấm Chạy lại.
- **Then**: Từ chối và báo khách liên hệ hỗ trợ.
- **And**: Sau khi Admin đặt lại bộ đếm theo STORY-PROJ-012, khách chạy lại được và lần chạy kiểm tra đủ điều kiện như AC-001.

#### AC-011

- **Given**: Lần chạy 2D của bản dự toán đang chờ hoặc đang chạy.
- **When**: Có thêm yêu cầu Tạo bản vẽ 2D, kể cả gửi lặp hoặc từ thiết bị khác.
- **Then**: Từ chối yêu cầu thêm; chỉ có một lần chạy và một lượt được giữ.
- **And**: Yêu cầu gửi lặp có cùng khóa chống trùng (Idempotency-Key) nhận lại kết quả của lần đầu, không bị coi là lần chạy mới.

#### AC-012

- **Given**: Chặng 2D đã thành công.
- **When**: Khách mở màn Bản vẽ 2D.
- **Then**: Thấy phương án từng tầng (tên phòng, diện tích) và cho mỗi tầng một bản vẽ kỹ thuật, một bản vẽ bố trí nội thất.
- **And**: Chặng Dự toán và chặng Phối cảnh 3D ở trạng thái Sẵn sàng, có một nút Đi tiếp để chạy cả hai; hệ thống không tự chạy hai chặng này.

#### AC-013

- **Given**: Lần chạy 2D đang chạy thì máy chủ khởi động lại.
- **When**: Hệ thống hoạt động trở lại trong hạn chờ.
- **Then**: Lần chạy tiếp tục từ bước đã lưu gần nhất; các bước AI đã có kết quả không bị gọi lại.
- **And**: Không tính lỗi và không giữ thêm lượt.

#### AC-014

- **Given**: Số lần chạy đang chờ của toàn hệ thống đã đạt ngưỡng Admin cấu hình, hoặc dịch vụ AI đang tạm ngắt.
- **When**: Khách bấm Tạo bản vẽ 2D.
- **Then**: Bị từ chối ngay với thông báo hệ thống đang bận hoặc dịch vụ AI đang tạm gián đoạn, kèm gợi ý thử lại sau.
- **And**: Không tạo lần chạy, không giữ lượt, không tính vào bộ đếm lỗi; đầu vào vẫn sửa được.

#### AC-015

- **Given**: Chặng 2D đã lỗi 3 lần liên tiếp vì dịch vụ AI không phản hồi sau khi đã thử lại.
- **When**: Khách xem trạng thái chặng 2D.
- **Then**: Chặng ở trạng thái Lỗi và vẫn còn đủ số lần chạy lại theo giới hạn, vì lỗi hệ thống không bị đếm.
- **And**: Mỗi lần lỗi đều đã hoàn lượt đang giữ.

## References

### TDDs

- TDD-PROJ-006
- TDD-PROJ-007
- TDD-PROJ-008
- TDD-PROJ-001: Đổi tên bản dự toán không bị khóa khi chặng đang chạy (EXC-02).

### Rules

- BR-PROJ-001
- BR-PROJ-002
- BR-PROJ-004
- BR-PROJ-005
- BR-PROJ-010
- BR-PROJ-011
- BR-PROJ-012
- BR-PROJ-013
- BR-PROJ-014
- BR-PROJ-015
- BR-PROJ-018
- BR-PROJ-019
- BR-SUB-003
- BR-SUB-007
- BR-SUB-016
- BR-SUB-017

### Dependencies

- STORY-PROJ-001
- STORY-PROJ-012: Admin đặt lại bộ đếm lỗi và cấu hình hạn chờ, model AI.
- STORY-LIB-002: mẫu 2D tham khảo trong lúc chờ.

## Non-Functional

- Bảo vệ đầu vào đã chốt khỏi thay đổi bằng kiểm tra tại backend, không chỉ khóa giao diện.
- Hạn chờ chặng 2D mặc định 20 phút, Admin cấu hình được (BR-PROJ-012). Lần chạy thử app demo ngày 11/10/2026 cho cả bộ nhà phố 2 tầng mất khoảng 15 phút; chưa có số đo riêng cho chặng 2D trên hệ thống thật.
- Mỗi bước AI tốn tiền được lưu ngay khi xong để khi gián đoạn chỉ mất tối đa một lời gọi đang chạy dở (TDD-PROJ-002).
- Kết quả 2D trên hệ thống thật phải khớp hành vi app demo ở mốc `059e876` trên các bộ dữ liệu đối chiếu; sai số đo đường bao dưới 1%.

## Out of Scope

- Khách hủy lần chạy đang diễn ra.
- Sửa phương án hoặc vẽ lại 2D sau khi chặng đã thành công.
- Nhiều ảnh đầu vào (mỗi ảnh một tầng) như app demo; hệ thống thật giữ một ảnh lô cho mỗi bản dự toán.
- Bản vẽ kết cấu.
- Sửa các lỗi chất lượng đã thấy ở app demo (tự thêm giếng trời, `wc_count = 0`); sẽ làm sau bằng phiên bản prompt mới theo BR-PROJ-014.
