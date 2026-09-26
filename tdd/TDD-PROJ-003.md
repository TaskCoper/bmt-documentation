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

<!-- Thay mã TDD-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Sơ đồ dùng mermaid, plantuml hoặc URL. Giữ heading Architecture, Sequence Diagram, Activity Diagram, State Diagram, Data Model.
Ví dụ API phải có heading trùng METHOD /path trong Endpoints. Xoá phần API/sơ đồ không dùng.
Version, Updated At và Change Log do lịch sử phiên bản quản lý, để trống khi nhập mới.
Author/Reviewer là tên hiển thị; gán tài khoản, phê duyệt, giả định và câu hỏi mở trên giao diện sau import.
Thiết kế phải truy vết về Story và Business Rule được cung cấp. Phân biệt thiết kế đề xuất với implementation đã kiểm chứng; không mô tả API/schema/sơ đồ ví dụ như hệ thống thật hoặc tự quyết định nghiệp vụ còn thiếu.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Feature tối đa 300 ký tự và được dùng làm tiêu đề tài liệu. Status được parser nhận: Draft / In Review / Approved / Deprecated; tài liệu nhập mới vẫn có trạng thái phê duyệt Draft.
- Endpoint: đường dẫn tối đa 500 ký tự; tên/mô tả endpoint được lưu trong Name tối đa 300. HTTP method: GET / POST / PUT / PATCH / DELETE / HEAD / OPTIONS.
- Error Codes: mã tối đa 100 ký tự, không trùng trong tài liệu; giữ dạng - **CODE** (400): mô tả. HTTP status trong Error Codes và Response phải là số nguyên đọc được bằng Int32; không tự đặt status khác hợp đồng API.
- URL sơ đồ tối đa 1.000 ký tự. Mục sơ đồ được sử dụng phải có khối mermaid/plantuml hoặc URL theo mẫu; không để một mục sơ đồ rỗng. Nhãn mục được tách từ bullet như tên External API Fields tối đa 300 ký tự.
- Heading Examples phải khớp METHOD /path ở Endpoints. Giữ các nhãn Request:, Response 200: (thay mã theo contract) và Error Response: để parser tách đúng dữ liệu.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
ĐỐI CHIẾU FORM TDD (src/features/tdds/validations.ts; form có thể chặt hơn import):
- Điền Feature, Author, Reviewer, Problem và ít nhất một Goal; các Goal/Non-goal/Notes đã thêm không được rỗng. API đã khai báo phải có endpoint và mô tả/mục đích; Fields phải có tên và ý nghĩa; Error Codes phải có mã, HTTP status và điều kiện.
- Form còn kiểm tra Version, Updated At và nội dung Change Log khi khai báo; với file nhập mới, giữ các trường lịch sử này trống theo hợp đồng import, không tự tạo dữ liệu lịch sử.
-->

# TDD-PROJ-003

## Document Info

- **Feature**: Tạo dự toán — hồ sơ, xuất tệp, chia sẻ và email
- **Author**: Codex
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

STORY-PROJ-003/004 cần xem kết quả, PDF/Excel, link/QR và email từ cùng bộ kết quả AI đã lưu. Người nhận có link hợp lệ xem/tải không cần đăng nhập; chủ sở hữu vẫn dùng đầy đủ hồ sơ cũ khi gói hết hạn. Thu hồi phải chặn yêu cầu tải mới, nên người xem không bao giờ nhận URL gốc của tệp: mọi lần xem/tải đi qua route của backend có kiểm link còn hiệu lực, và backend chuyển tiếp (stream) nội dung tệp.

Người dùng xác nhận bổ sung ngày 21/09/2026: mỗi bản có tối đa một link đang hiệu lực, dùng chung sao chép/QR/email; còn hiệu lực thì dùng lại, hết hạn hoặc thu hồi mới tạo khác. Ngày hết hạn dùng hết ngày theo Asia/Ho_Chi_Minh, 00:00 ngày kế tiếp hết hiệu lực, không chọn ngày đã qua.

Bổ sung ngày 25/09/2026: khi dùng lại link còn hiệu lực, backend trả link kèm ngày hết hạn hiện có và báo rõ ngày mới chọn không được áp dụng; muốn đổi ngày thì thu hồi rồi tạo lại (BR-PROJ-006 khoản 9, STORY-PROJ-004/AC-010). Chủ sở hữu đổi tên bản dự toán được bất cứ lúc nào theo TDD-PROJ-001; sau đó màn hình chủ sở hữu và trang xem qua link luôn hiện tên hiện tại, tệp PDF/Excel xuất theo tên cũ không được dùng nữa và lần tải tiếp theo xuất lại từ kết quả đã lưu với tên mới, không tính lượt, không gọi AI (BR-PROJ-007 khoản 7, STORY-PROJ-003/AC-006).

Người dùng xác nhận ngày 26/09/2026 về tệp: backend không có kho tệp riêng. AI service trả URL cho mọi tệp kết quả, kể cả PDF và Excel; backend chỉ lưu URL (bảng `EstimateResultFile` của TDD-PROJ-002), không tự dựng PDF/Excel. Khi tải qua link chia sẻ, backend chuyển tiếp tệp qua route có kiểm link còn hiệu lực và không lộ URL gốc, để thu hồi chặn được lần tải mới (BR-PROJ-006 khoản 4). Hợp đồng API của AI service vẫn chưa có. Backend đã có IMailService/MailKit SMTP dùng cho xác thực, nhưng chưa có hàng đợi gửi hồ sơ hoặc trạng thái tiếp nhận. Thiết kế dưới đây bổ sung phần điều phối; không khẳng định đã gửi email hay đã có tệp thật.

### Goals

- UI, PDF và Excel luôn dùng một kết quả nguồn; không tự tính lại số tiền, không sửa diện tích đầu vào bằng giá trị AI ước tính.
- Các thao tác hồ sơ không kiểm số dư/quyền tạo mới, không gọi AI tạo thiết kế và không thay trạng thái thành công của operation nguồn.
- Link/QR/email có cùng ngày hết hạn/quyền thu hồi; request xem/tải mới sau hết hạn/thu hồi bị chặn ở backend.
- Gửi lặp hoặc crash không báo sai đã gửi/đã phát email; tác vụ tệp thất bại có thể yêu cầu lại từ cùng nguồn.
- Hồ sơ và tệp luôn dùng tên hiện tại của bản dự toán: sau khi đổi tên, không phục vụ tệp xuất theo tên cũ; lần tải tiếp theo xuất lại mà không tính lượt, không gọi AI và không thay kết quả nguồn.

### Non-goals

- Tạo nội dung chuyên môn tại BMT; tự dựng PDF/Excel hoặc chọn thư viện xuất tệp, vì AI trả URL cho cả PDF và Excel (quyết định ngày 26/09/2026).
- Kho tệp riêng của backend: không tải tệp của AI về lưu lại.
- Đính kèm hồ sơ trong email, gửi nhiều người một lần, theo dõi đã đọc thư, thu hồi tệp đã tải (kể cả tệp mang tên cũ đã tải trước khi đổi tên).
- Nhiều link đồng thời, gia hạn/đổi ngày link đang có, chia sẻ quyền chỉnh sửa hoặc quyền tạo AI.
- Triển khai frontend/trình xem hồ sơ, dịch vụ thật hoặc Unit Test chi tiết ở bước TDD.

## Architecture

Module Results chỉ đọc `EstimateGenerationResult` và `EstimateResultFile` khi UsageOperation đã Succeeded. Module Exports chọn tệp PDF/Excel của nguồn đó theo tên hiện tại; module Sharing kiểm token và thời hạn; module Email xếp yêu cầu gửi link. Tất cả dùng cùng EstimateId/OperationId để không lẫn bản và không phụ thuộc trạng thái quota hiện tại.

| Thành phần dự kiến | Vai trò |
|---|---|
| EstimateResultReader | Kiểm owner hoặc share grant, JOIN nguồn đã Succeeded rồi trả DTO an toàn; tên bản dự toán đọc từ Estimate.Name tại lúc yêu cầu. |
| RequestEstimateExportHandler / EstimateExportWorker | Tạo/nhận lại một export theo operation, format và NameVersion hiện tại; lấy URL tệp PDF/Excel do AI trả qua IEstimateExportFileSource rồi ghi vào OutputUrl. Không tự dựng tệp. |
| IEstimateExportFileSource | Cổng trả URL tệp của một export: lấy từ `EstimateResultFile` của nguồn khi AI đã trả tệp đúng định dạng, hoặc theo thao tác của AI nếu hợp đồng cho tạo tệp sau. Hiện thực chờ hợp đồng AI và câu hỏi mở về tên trong tệp (Architecture/Notes). |
| CreateEstimateShareHandler / RevokeEstimateShareHandler | Khóa Estimate để kiểm slot link hiện hành, tạo/thu hồi trong transaction; không thay quota hoặc inputVersion. |
| EstimateShareAccessPolicy | Kiểm đúng share, token hash, current pointer, chưa thu hồi và EffectiveNow < ExpiresAtUtc trên mỗi lần xem/tải. |
| EstimateFileReader | Sau khi kiểm quyền và đúng liên kết result/export, đọc tệp từ URL đã lưu qua `IEstimateResultFileClient` (TDD-PROJ-002) và chuyển tiếp nội dung cho người gọi; không redirect và không trả URL gốc. |
| QueueEstimateEmailHandler / EstimateEmailWorker | Lưu một yêu cầu gửi một email; worker kiểm link còn hiệu lực rồi gửi ngoài transaction, cập nhật Accepted/Rejected/Unknown theo chứng cứ SMTP. |
| IEstimateShareTokenProtector | Sinh token ngẫu nhiên, lưu hash để kiểm; mã hóa bản token để owner lấy lại link/QR và email dùng cùng quyền. Keyring chia sẻ giữa instance và cần backup. |

```mermaid
flowchart LR
    O[Chủ sở hữu] --> API[Hồ sơ API]
    V[Người có link] --> PUB[API chia sẻ và kiểm token]
    API --> R[Result reader]
    PUB --> R
    R --> DB[(Nguồn Succeeded và metadata)]
    API --> EX[Export worker]
    PUB --> EX
    EX --> DB
    R --> FR[EstimateFileReader]
    FR -->|Đọc theo URL đã lưu, chuyển tiếp nội dung| S[Tệp do AI lưu]
    API --> EM[Email request và worker]
    EM --> SMTP[SMTP ngoài SQL]
    EM --> DB
```

**Notes**:

- **Một nguồn kết quả**: OperationId xác định bộ AI thành công duy nhất. Reader trả tổng, phần thô/hoàn thiện/nội thất, tư vấn và hồ sơ theo contract đã lưu; không cộng lại số tiền để “sửa” AI. Cấu trúc này áp dụng cho mọi loại công trình trong danh mục (BR-PROJ-007 khoản 1); không cắt nhóm phần thô của Căn hộ hay loại nào khác. Nếu payload thiếu trường bắt buộc thì phải bị ngăn từ giai đoạn finalization, không điền số 0 tại reader.
- **Tệp xuất do AI trả, backend không dựng** (quyết định ngày 26/09/2026): unique `(OperationId,Format,NameVersion)` cho Pdf/Xlsx; mọi export đều trỏ cùng nguồn. AI trả URL cho PDF và Excel như mọi tệp kết quả; export chỉ ghi URL tệp đúng định dạng vào OutputUrl rồi chuyển Ready. Backend không render, không chọn thư viện xuất tệp. Nếu hợp đồng cho AI tạo PDF/Excel sau khi thành công, `IEstimateExportFileSource` gọi thao tác đó theo hợp đồng; thao tác này không phải operation tạo thiết kế và không tính lượt. Không gọi lại model thiết kế. Tệp nào bắt buộc có lúc thành công là mục chờ AI (TDD-PROJ-002/Architecture), không tự coi PDF chưa có là thất bại nguồn.
- **Tệp xuất gắn với tên tại lúc xuất**: tên bản dự toán in trong PDF/Excel, nên mỗi export ghi `NameVersion` của Estimate tại lúc tiếp nhận yêu cầu (TDD-PROJ-001/Architecture). Hệ thống chỉ phục vụ export có NameVersion bằng Estimate.NameVersion hiện tại. Cách này giải quyết BR-PROJ-007 khoản 7 mà không để thao tác đổi tên phải khóa hoặc sửa các dòng export: đổi tên chỉ tăng NameVersion, còn export cũ tự hết hiệu lực khi so sánh.
  - Cách chạy: POST export đọc Estimate.NameVersion hiện tại rồi tìm export `(OperationId,Format,NameVersion)`; chưa có thì tạo Pending mới với AttemptNumber=1. Worker khi nhận việc đọc Estimate.Name và NameVersion trong cùng một lần đọc; nếu NameVersion đã lớn hơn của export thì không lấy tệp, đóng export thành Failed với FailureCode=EstimateRenamed. Nếu khớp, worker gọi `IEstimateExportFileSource` với OperationId, Format, NameVersion và đúng tên vừa đọc, đọc thử URL nhận được rồi gắn OutputUrl. Tên có thể đổi trong lúc này; export đó vẫn Ready đúng với NameVersion của nó nhưng không còn được phục vụ, và lần tải kế tiếp tạo export theo NameVersion mới.
  - Ví dụ: D1 có EX1 (Pdf, NameVersion=1) Ready. Chủ sở hữu đổi tên, D1.NameVersion=2. Người nhận link bấm tải PDF: không có export Pdf cho NameVersion=2 nên tạo EX2 Pending; worker lấy tệp cho tên mới từ J1 theo cách được chốt ở câu hỏi mở bên dưới rồi Ready. EX1 và URL FP1 giữ làm lịch sử, không phục vụ nữa. Không tính lượt, không gọi AI tạo thiết kế, J1 không đổi.
  - Đánh đổi: mỗi lần đổi tên có thể sinh thêm dòng export khi có người tải lại (và thêm tệp phía AI nếu chốt phương án (b) bên dưới). Backend không xóa tệp của AI. Tệp đã tải về máy trước khi đổi tên không thu hồi được (BR-PROJ-007 khoản 7).
- **Câu hỏi mở: tên bản dự toán trong tệp PDF/Excel (chưa chốt)**. BR-PROJ-007 khoản 7 và STORY-PROJ-003/AC-006 yêu cầu: sau khi đổi tên, lần tải tiếp theo xuất lại từ kết quả đã lưu với tên mới, không tính lượt và không gọi AI. Theo quyết định ngày 26/09/2026, PDF/Excel do AI trả và backend không tự dựng, nên backend không tự in tên mới vào tệp được. Mặt khác tên không có trong snapshot gửi AI (BR-SUB-007 khoản 11), nên tệp AI trả lúc thành công cũng không chứa tên bản dự toán. Các cách xử lý:
  - (a) Tên chỉ nằm ở tên tệp tải về: khi chuyển tiếp, backend đặt `Content-Disposition` theo tên hiện tại, nội dung tệp giữ nguyên của AI; `IEstimateExportFileSource` trả lại URL tệp của nguồn. Không gọi AI, không tính lượt. Hệ quả: bước kiểm “trong tệp ghi tên mới” của ST-PROJ-069 phải đổi thành kiểm tên tệp, cần người dùng xác nhận vẫn đúng ý AC-006.
  - (b) AI service có thao tác dựng lại tệp với tên mới từ kết quả đã lưu, không phải tạo thiết kế và không tính lượt. Hệ quả: tên trở thành dữ liệu gửi AI cho thao tác này, nên cụm “không gọi AI” ở BR-PROJ-007 khoản 7 và BR-SUB-007 khoản 11 cần người dùng làm rõ thành “không gọi AI tạo thiết kế”.
  - (c) Backend tự chèn tên vào tệp: trái quyết định không tự dựng PDF/Excel; chỉ nêu để đủ lựa chọn.
  - Đề xuất (a) vì không phụ thuộc hợp đồng AI và giữ đúng “không gọi AI”; chưa chốt. Cơ chế export theo NameVersion ở trên dùng được cho cả (a) và (b), nên chỉ phần hiện thực `IEstimateExportFileSource` và ST-PROJ-069 phụ thuộc câu trả lời.
- Yêu cầu export mới lưu Pending rồi worker lấy URL tệp ngoài SQL; thành công mới gắn OutputUrl và Ready. Gửi lại lúc Pending/Ready của NameVersion hiện tại trả công việc đó. Failed chỉ khởi động lại khi có yêu cầu mới chủ động; cùng idempotency key không tạo lại việc. `AttemptNumber` tăng khi chủ động thử lại và `LeaseToken` chặn worker cũ gắn output sang lần mới. Retry thuần I/O cùng nguồn có thể do worker xử lý, không đổi quota; retry policy cần cấu hình theo adapter.
- **Một link hiệu lực**: Estimate bổ sung CurrentShareId. CreateShare kiểm `expiryDate` hợp lệ trước (sai định dạng hoặc ngày đã qua vẫn trả 422), rồi khóa Estimate và đọc link current. Còn hiệu lực thì trả 200 với chính link đó và ExpiryDate/ExpiresAtUtc hiện có, kèm `requestedExpiryApplied=false` nếu ngày yêu cầu khác ngày đang có, hoặc `requestedExpiryApplied=true` nếu trùng ngày đang có (người dùng xác nhận ngày 25/09/2026); không đổi hạn và không trả lỗi. Frontend dùng cờ này để báo ngày mới chọn không được áp dụng và hướng dẫn thu hồi rồi tạo lại nếu muốn đổi ngày (BR-PROJ-006 khoản 9). Hết hạn/đã thu hồi thì tạo share mới với ngày yêu cầu, trả 201 với `requestedExpiryApplied=true`, và cập nhật con trỏ cùng transaction. Đường đọc công khai yêu cầu shareId là CurrentShareId, nên chỉ một link được chấp nhận kể cả có dữ liệu cũ trong bảng. Giữ lịch sử share, không tái sử dụng token bị thu hồi.
- **Chống gửi lặp khi xuất/chia sẻ/email**: key dài 1–100 ký tự; hash chứa tên thao tác, estimateId, resourceId liên quan và body chuẩn hóa. Quyền owner hoặc quyền share được kiểm trước tra key; cùng key khác hash trả 409 IdempotencyConflict. Receipt chủ sở hữu đã commit được trả lại mà không tạo quyền mới, kể cả share cũ đã hết hạn/thu hồi; trả trạng thái hiện tại của chính share đó. Không tự đổi receipt sang link mới. Tạo share dùng lại link hiện hành cũng lưu receipt. Export request đã nhận được replay bằng đúng ExportId đã ghi, kể cả khi export đó đã cũ vì bản dự toán vừa đổi tên; client thấy `isCurrent=false` thì gửi yêu cầu xuất mới với key mới. Với export công khai, link hết hiệu lực bị từ chối trước cả replay. Email replay trả trạng thái yêu cầu cũ, không đòi gửi lại và không chuyển sang share mới.
- Mọi thời gian lưu UTC. `expiryDate` là ngày Việt Nam; server đổi thành thời điểm bắt đầu ngày kế tiếp tại Asia/Ho_Chi_Minh rồi sang UTC. Ví dụ 25/09/2026 → `2026-09-25T17:00:00Z`. Hợp lệ khi `now < expiresAt`; bằng mốc thì hết hạn. Không lấy timezone cá nhân hoặc giờ thiết bị để kéo dài link. Ngày hôm nay được phép đến hết ngày, ngày đã qua bị từ chối; không tự đặt số ngày tối đa.
- **Token link**: sinh 32 byte bằng bộ sinh ngẫu nhiên mật mã, encode base64url không padding; DB lưu SHA-256 để tra/so sánh và token ciphertext có xác thực để owner lấy lại. Token không chứa email hoặc thông tin khách. Mã hóa dùng Data Protection purpose riêng cho EstimateShare v1; keyring bền vững, dùng chung và quản lý truy cập. Ciphertext không thay hash kiểm quyền; mất keyring có thể làm owner không lấy lại link, nên cần phục hồi keyring trước mở tính năng.
- Đường dẫn frontend đề xuất `{ConfiguredPublicWebBase}/vi/estimates/shared/{shareId}#token=...`. Token ở fragment không tự gửi vào request log/referrer; frontend lấy rồi gửi qua header `X-Estimate-Share-Token` cho API. QR mã hóa chính URL đó, email chứa chính URL đó. Không lấy Host request làm base URL và không gọi dịch vụ QR bên ngoài làm lộ token. Route frontend là hợp đồng bàn giao, chưa có code trong workspace.
- **Thu hồi**: cập nhật RevokedAtUtc dưới khóa Estimate/Share rồi commit; kiểm public access đọc DB primary mỗi request, không cache quyền positive. Lấy giờ/đọc quyền ngay trước mở stream; request bắt đầu sau commit thu hồi hoặc sau expiry bị từ chối. Một stream đã được cấp quyền trước thu hồi có thể đang truyền, và bytes đã tải không thu hồi được; không hứa xóa nội dung khỏi máy người nhận.
- **Tải tệp qua backend, không lộ URL gốc** (người dùng xác nhận ngày 26/09/2026): tệp không có đường tải công khai riêng. Mỗi GET/HEAD/Range mới kiểm grant hoặc owner, result Succeeded và tệp cùng nguồn; sau đó `EstimateFileReader` đọc tệp từ URL đã lưu qua `IEstimateResultFileClient` và chuyển tiếp nội dung trong response. Backend không redirect và không đưa URL gốc vào DTO, header, lỗi hay log, nên thu hồi hoặc hết hạn chặn được mọi lần tải mới qua link (BR-PROJ-006 khoản 4). Ví dụ: S1 bị thu hồi lúc 02:00Z; request tải lúc 02:00:01Z bị 404 ShareUnavailable trước khi backend mở kết nối tới URL tệp. Range chỉ hỗ trợ khi máy chủ tệp của AI hỗ trợ; nếu không thì trả cả tệp. Giới hạn: nếu URL gốc là công khai và lộ ra bằng đường khác ngoài backend, backend không chặn được đường đó; thời hạn và quyền truy cập URL của AI là câu hỏi mở ở TDD-PROJ-002. Response no-store, Referrer-Policy=no-referrer, X-Content-Type-Options=nosniff; không đưa token vào query, log, metric, exception hoặc analytics. Nội dung AI dùng DTO/HTML đã escape; không render raw script. Lỗi share sai/thu hồi/hết hạn cùng 404 ShareUnavailable để không tiết lộ nội dung.
- **Tên hiện tại trên hồ sơ**: DTO hồ sơ của chủ sở hữu, trang xem qua link và nội dung email đều đọc Estimate.Name tại thời điểm xử lý yêu cầu hoặc lúc worker dựng thư. Share, receipt và email request không lưu bản sao tên, nên đổi tên có hiệu lực ngay ở lần đọc mới mà không cần sửa link.
- Không yêu cầu subscription cho owner xem/xuất/share/email/revoke hoặc người nhận có link. Các endpoint chủ sở hữu vẫn cần verified session/owner; link công khai không cấp quyền gọi mutation owner. Header share chỉ có tác dụng trên route public, không dùng như token đăng nhập.
- **Email bền vững, trạng thái trung thực**: request hợp lệ lưu Queued rồi trả 202, chưa báo đã gửi. Worker claim bằng lease token, kiểm lại share hiện hành/hạn, dựng HTML từ dữ liệu đã escape và gọi SMTP ngoài SQL. Nhận phản hồi SMTP tiếp nhận thì Accepted, không phải Delivered. Nếu link đã hết hạn/thu hồi tại lần kiểm cuối ngay trước bắt đầu gửi thì Skipped, không gửi link khác tự động. Thu hồi xảy ra sau lần kiểm này có thể vẫn có thư tới nơi; link trong thư vẫn bị chặn tại thời điểm người nhận truy cập.
- SMTP hiện tại gọi SendAsync rồi DisconnectAsync trong một Task; nếu Disconnect lỗi sau khi server nhận thư, Task lỗi không chứng minh thư chưa được nhận. Adapter mới phải ghi nhận riêng mốc SendAsync đã được xác nhận; không biến lỗi đóng kết nối thành Rejected hoặc tự gửi lại. `IMailService` hiện không trả receipt/cancellation nên cần adapter riêng `IEstimateMailSender`, dùng cùng cấu hình MailKit mà không thay hợp đồng mail xác thực ngầm.
- Crash/mất phản hồi sau gửi trước ghi Accepted → Unknown; không tự gửi lại với SMTP vốn không bảo đảm idempotency. Sending hết lease mà chưa có bằng chứng tiếp nhận được chuyển Unknown, không nhận lại như Queued. Cùng key trả cùng request/trạng thái. Khách có thể chủ động gửi yêu cầu mới với key mới; UI phải cho biết lần trước chưa rõ để tránh hiểu rằng chắc chắn chưa gửi. Không tuyên bố exactly-once email. Rejected (được từ chối rõ ràng) cũng không tự lặp hàng loạt; chủ sở hữu chủ động yêu cầu lại.
- Danh sách một người nhận: parse đúng một địa chỉ mailbox; từ chối mảng, danh sách dấu phẩy/chấm phẩy, newline/CRLF và chuỗi thiếu địa chỉ. Không dùng giới hạn Email=50 của bảng User cho người nhận bên ngoài. Recipient text lưu địa chỉ đã chuẩn hóa; giới hạn request kỹ thuật không thay quy tắc chỉ một email.

## Sequence Diagram

```mermaid
sequenceDiagram
    actor O as Chủ sở hữu
    actor V as Người nhận
    participant API as Hồ sơ API
    participant DB as PostgreSQL
    participant S as Tệp do AI lưu (URL)
    O->>API: Tạo link với expiryDate và key
    API->>DB: Kiểm owner, source Succeeded, khóa Estimate
    alt Link current còn hiệu lực
      DB-->>API: Trả link hiện hành, không đổi hạn
      API-->>O: 200 URL, QR và ngày hết hạn hiện có, requestedExpiryApplied cho biết ngày mới có được áp dụng
    else Chưa có hoặc đã hết hiệu lực
      API->>DB: Insert share và đổi CurrentShareId cùng commit
      API-->>O: 201 URL, QR và ngày hết hạn vừa chọn
    end
    V->>API: GET public dossier với share header
    API->>DB: Kiểm hash, current, revoked, expiry và nguồn
    DB-->>API: Grant còn hiệu lực
    API-->>V: DTO hồ sơ, đường tải có kiểm quyền
    V->>API: GET file qua route public với share header
    API->>DB: Kiểm lại grant, export hiện hành và nguồn
    API->>S: Đọc tệp theo URL đã lưu
    S-->>API: Nội dung tệp
    API-->>V: Chuyển tiếp nội dung, không lộ URL gốc
    O->>API: Thu hồi share
    API->>DB: RevokedAtUtc, commit
    V->>API: Yêu cầu tải mới
    API->>DB: Kiểm lại grant
    API-->>V: 404 ShareUnavailable, không gọi tới URL tệp
```

## Activity Diagram

```mermaid
flowchart TD
    A[Yêu cầu gửi một email và key] --> B{Owner và share hợp lệ?}
    B -->|Không| X[Từ chối, không gửi]
    B -->|Có| C{Key đã được nhận?}
    C -->|Có, cùng nội dung| R[Trả request cũ]
    C -->|Khác nội dung| X
    C -->|Chưa có| D[Lưu Queued, trả 202]
    D --> E[Worker claim, kiểm lại share]
    E --> F{Link còn hiệu lực?}
    F -->|Không| G[Skipped]
    F -->|Có| H[Gửi SMTP ngoài SQL]
    H -->|Xác nhận tiếp nhận| I[Accepted]
    H -->|Từ chối rõ| J[Rejected]
    H -->|Không rõ đã nhận| K[Unknown, không tự gửi lại]
```

## State Diagram

```mermaid
stateDiagram-v2
    state Share {
      [*] --> Active: Tạo với ngày hợp lệ
      Active --> Revoked: Chủ sở hữu thu hồi
      Active --> Expired: now tới ExpiresAtUtc
    }
    state Export {
      [*] --> Pending: Yêu cầu xuất
      Pending --> Ready: Có URL tệp và đọc thử được
      Pending --> Failed: Lỗi chuẩn bị cuối
      Pending --> Failed: Tên đã đổi trước khi lấy tệp, EstimateRenamed
      Failed --> Pending: Yêu cầu chủ động mới cùng NameVersion, tăng AttemptNumber
    }
    state Email {
      [*] --> Queued: Lưu yêu cầu hợp lệ
      Queued --> Sending: Worker claim
      Queued --> Skipped: Link không còn hiệu lực
      Sending --> Skipped: Chưa gửi và kiểm link thất bại
      Sending --> Accepted: SMTP xác nhận nhận
      Sending --> Rejected: SMTP từ chối rõ
      Sending --> Unknown: Mất xác nhận hoặc worker dừng sau gửi
    }
```

Active/Expired là hiệu lực tính khi đọc từ mốc UTC, không cần cron mới hết hạn. Revoked giữ sự kiện thu hồi; không tái kích hoạt share cũ. Ready của export chỉ được trả cùng quyền truy cập nguồn; file lỗi về sau không đổi operation AI thành Failed. Export có NameVersion nhỏ hơn Estimate.NameVersion là tệp theo tên cũ: không lưu thêm trạng thái “cũ”, mà tính khi đọc bằng phép so NameVersion; Ready cũ không được phục vụ và không quay lại Pending. Failed do EstimateRenamed không được khởi động lại, vì yêu cầu mới sẽ tạo export của NameVersion hiện tại.

## Data Model

Nguồn dùng lại: Estimate ở TDD-PROJ-001; EstimateGenerationResult/EstimateResultFile và UsageOperation ở TDD-PROJ-002. TDD này không tạo bảng tiền, không có bảng tệp riêng và không copy payload AI vào share/email. Tệp chỉ được tham chiếu bằng URL do AI trả.

| Bảng mới/thay đổi | Ý nghĩa một dòng và nơi cập nhật |
|---|---|
| Estimate bổ sung CurrentShareId | Con trỏ tới quyền chia sẻ hiện hành hoặc NULL khi chưa chia sẻ. Không phải quyền truy cập của owner và không thay InputVersion. |
| EstimateExport | Một bản PDF hoặc XLSX của một kết quả nguồn theo một tên bản dự toán (NameVersion); worker cập nhật việc chuẩn bị, URL tệp, lease và attempt. OutputUrl NULL khi chưa Ready. Đổi tên không sửa dòng này; dòng có NameVersion cũ chỉ còn là lịch sử. |
| EstimateExportRequest | Một hành động yêu cầu xuất với key, nhận diện lần gửi lại kể cả khi export đã thất bại; không phải bản sao bytes hoặc operation AI. |
| EstimateShare | Một lần cấp quyền link có token và ngày hết hạn. Tạo mới sau khi link cũ hết hiệu lực; RevokedAtUtc NULL khi chưa thu hồi. |
| EstimateShareReceipt | Một hành động Create/Revoke đã commit; kết quả chỉ shareId/trạng thái, không lưu plaintext token hoặc hồ sơ. |
| EstimateEmailRequest | Một yêu cầu gửi link tới đúng một email; worker ghi trạng thái tiếp nhận. Lưu người nhận để gửi nhưng không coi là contact người dùng. |

| Bảng | Cột, kiểu và ràng buộc |
|---|---|
| Estimate bổ sung | CurrentShareId uuid NULL; FK(Id,CurrentShareId) → EstimateShare(EstimateId,Id), tạo sau bảng share hoặc gắn pointer sau insert. |
| EstimateExport | Id uuid PK; EstimateId uuid NN; OperationId uuid NN; Format varchar(8) NN CHECK Pdf/Xlsx; NameVersion bigint NN CHECK>0; State varchar(16) NN CHECK Pending/Ready/Failed; AttemptNumber int NN DEFAULT 1 CHECK>0; LeaseToken uuid NULL; LeaseUntilUtc timestamptz NULL; OutputUrl varchar(2048) NULL CHECK NULL hoặc bắt đầu bằng https:// (không phân biệt hoa thường); FailureCode varchar(100) NULL; CreatedAtUtc,UpdatedAtUtc timestamptz NN; UNIQUE(OperationId,Format,NameVersion); UNIQUE(Id,EstimateId); FK(OperationId,EstimateId) → EstimateGenerationResult(OperationId,EstimateId). CHECK Ready có OutputUrl, Pending/Failed OutputUrl NULL. OutputUrl là URL tệp do AI trả: chép từ `EstimateResultFile.FileUrl` của cùng nguồn, hoặc URL AI trả sau nếu hợp đồng cho tạo tệp sau; không có khóa ngoại tới bảng tệp. Tên máy chủ kiểm như URL tệp kết quả của TDD-PROJ-002. URL này không bao giờ được trả cho client. NameVersion lấy từ Estimate.NameVersion lúc tạo dòng; không có FK tới phiên bản tên vì tên cũ không được lưu lại, handler bảo đảm giá trị không lớn hơn Estimate.NameVersion. |
| EstimateExportRequest | Id uuid PK; EstimateId uuid NN; ExportId uuid NN; ActorId uuid NULL FK User; ShareId uuid NULL; RequestKey varchar(100) NN; RequestHash char(64) NN; AcceptedAttemptNumber int NN CHECK>0; CreatedAtUtc timestamptz NN; CHECK đúng một ActorId/ShareId có giá trị; FK(ExportId,EstimateId) → EstimateExport(Id,EstimateId); FK(ActorId,EstimateId) → Estimate(OwnerId,Id); FK(EstimateId,ShareId) → EstimateShare(EstimateId,Id). Hai unique index có điều kiện: (ActorId,RequestKey) WHERE ActorId IS NOT NULL và (ShareId,RequestKey) WHERE ShareId IS NOT NULL. |
| EstimateShare | Id uuid PK; EstimateId uuid NN FK Estimate; OperationId uuid NN; CreatedBy uuid NN; TokenHash char(64) NN UNIQUE; ProtectedToken text NN; ExpiryDate date NN; ExpiresAtUtc timestamptz NN; CreatedAtUtc timestamptz NN; RevokedAtUtc timestamptz NULL; UNIQUE(EstimateId,Id); FK(CreatedBy,EstimateId) → Estimate(OwnerId,Id); FK(OperationId,EstimateId) → EstimateGenerationResult(OperationId,EstimateId); CHECK ExpiresAtUtc>CreatedAtUtc và revoked NULL hoặc >=CreatedAtUtc. Không lưu trạng thái Active/Expired riêng. |
| EstimateShareReceipt | Id uuid PK; ActorId uuid NN FK User; EstimateId uuid NN; ShareId uuid NN; Operation varchar(16) NN CHECK Create/Revoke; RequestKey varchar(100) NN; RequestHash char(64) NN; CreatedAtUtc timestamptz NN; UNIQUE(ActorId,Operation,RequestKey); FK(ActorId,EstimateId) → Estimate(OwnerId,Id); FK(EstimateId,ShareId) → EstimateShare(EstimateId,Id). |
| EstimateEmailRequest | Id uuid PK; EstimateId uuid NN; ShareId uuid NN; RequestedBy uuid NN; Recipient text NN; RequestKey varchar(100) NN; RequestHash char(64) NN; State varchar(16) NN CHECK Queued/Sending/Accepted/Rejected/Unknown/Skipped; LeaseToken uuid NULL; LeaseUntilUtc timestamptz NULL; CreatedAtUtc,UpdatedAtUtc timestamptz NN; AcceptedAtUtc timestamptz NULL; ProviderReceipt text NULL; FailureCode varchar(100) NULL; UNIQUE(RequestedBy,RequestKey); FK(RequestedBy,EstimateId) → Estimate(OwnerId,Id); FK(EstimateId,ShareId) → EstimateShare(EstimateId,Id); CHECK Accepted có AcceptedAtUtc; các trạng thái khác AcceptedAtUtc NULL. |

FK dùng RESTRICT để không mất hồ sơ/lịch sử vì xóa cha. Không xóa mềm lịch sử share/receipt theo Entity cơ sở. ExportRequest tách vì export có thể được yêu cầu bởi owner hoặc share: một hàng export vẫn có nhiều hành động yêu cầu từ các nguồn khác nhau, mỗi action chỉ được nhận một lần. Hai unique index tách yêu cầu của chủ sở hữu và của link công khai. Các FK ghép bảo đảm export, chủ sở hữu hoặc share cùng một bản dự toán; danh tính do backend xác minh, không nhận ActorId/ShareId tùy ý từ body.

**Ràng buộc một link**: FK CurrentShareId cùng Estimate và policy public yêu cầu pointer khớp bảo đảm không có hai quyền current cho một bản. Create/revoke khóa Estimate trước Share; email enqueue/claim cần đọc đúng Share, không lấy Share lock rồi quay lại Estimate. Không tạo partial index chứa `now()`. Sau link S1 hết hạn, S2 được tạo và pointer sang S2; S1 giữ lịch sử, mọi cách dùng S1 đều không được cấp quyền mới.

```mermaid
erDiagram
    Estimate ||--o{ EstimateShare : share_history
    Estimate o|--o| EstimateShare : current_pointer
    EstimateGenerationResult ||--o{ EstimateShare : exact_source
    EstimateGenerationResult ||--o{ EstimateExport : formats
    EstimateExport ||--o{ EstimateExportRequest : requests
    EstimateShare ||--o{ EstimateShareReceipt : idempotent_actions
    EstimateShare ||--o{ EstimateEmailRequest : one_recipient_each
```

**Mẫu lưu trữ** — UUID dưới dạng bí danh, ciphertext/hash chỉ ghi ký hiệu, không chứa token thật; UTC cho timestamp, date theo Việt Nam. D1/U1/J1 là bản thành công từ hai TDD trước.

| Bảng | Dòng minh họa |
|---|---|
| EstimateExport | EX1; EstimateId=D1; OperationId=J1; Format=Pdf; NameVersion=1 (D1 đang tên “Nhà A”); State=Pending; AttemptNumber=1; OutputUrl=NULL; CreatedAtUtc=UpdatedAtUtc=2026-09-21T04:00Z. |
| EstimateExportRequest | ER1; EstimateId=D1; ExportId=EX1; ActorId=U1; ShareId=NULL; RequestKey=pdf-1; RequestHash=HER1; AcceptedAttemptNumber=1; CreatedAtUtc=04:00Z. Sau khi đổi tên, yêu cầu tải qua link S1 tạo ER2: ExportId=EX2; ActorId=NULL; ShareId=S1; RequestKey=pdf-share-1; RequestHash=HER2; AcceptedAttemptNumber=1; CreatedAtUtc=2026-09-21T05:05:00Z. |
| EstimateExport sau khi có tệp | EX1: State=Ready; OutputUrl=FP1, với FP1 là bí danh của https://ai-files.example.test/j1/dossier.pdf, chép từ dòng RF2 (RoleKey=dossier-pdf) của TDD-PROJ-002/Data Model; UpdatedAtUtc=04:01Z; LeaseToken/LeaseUntilUtc=NULL. Backend không tải tệp về lưu. Một dòng Format=Xlsx dùng cùng J1 nhưng URL tệp khác (RF3). |
| Estimate sau đổi tên | Lúc 2026-09-21T05:00:00Z chủ sở hữu đổi tên D1 thành “Nhà A - phương án chốt”: D1.NameVersion=2, cùng sự kiện ở TDD-PROJ-001/Data Model. EX1/FP1 không bị sửa nhưng từ đây không được phục vụ vì NameVersion=1. |
| EstimateExport sau khi tải lại | EX2; EstimateId=D1; OperationId=J1; Format=Pdf; NameVersion=2; State=Ready; AttemptNumber=1; OutputUrl=FP2; CreatedAtUtc=2026-09-21T05:05:00Z; UpdatedAtUtc=05:06Z. Tạo khi người nhận link bấm tải PDF; tệp tải về mang tên “Nhà A - phương án chốt”. FP2 phụ thuộc câu hỏi mở ở Architecture/Notes: theo phương án (a), FP2 chính là URL của FP1 và tên mới nằm ở tên tệp tải về; theo phương án (b), FP2 là URL tệp mới AI dựng với tên mới. PeriodQuota và J1 không đổi. |
| EstimateShare | S1; EstimateId=D1; OperationId=J1; CreatedBy=U1; TokenHash=HT1; ProtectedToken=CT1; ExpiryDate=2026-09-25; ExpiresAtUtc=2026-09-25T17:00:00Z; CreatedAtUtc=2026-09-21T04:02:00Z; RevokedAtUtc=NULL. |
| Estimate bổ sung | D1.CurrentShareId=S1; InputVersion giữ giá trị trước chia sẻ. |
| EstimateShareReceipt | SR1; ActorId=U1; EstimateId=D1; ShareId=S1; Operation=Create; RequestKey=share-1; RequestHash=HSR1; CreatedAtUtc=04:02Z. |
| EstimateEmailRequest | M1; EstimateId=D1; ShareId=S1; RequestedBy=U1; Recipient=recipient@example.test; RequestKey=mail-1; RequestHash=HM1; State=Queued; CreatedAtUtc=UpdatedAtUtc=2026-09-21T04:02:30Z; LeaseToken=LeaseUntilUtc=AcceptedAtUtc=ProviderReceipt=FailureCode=NULL. |
| EstimateEmailRequest sau SMTP nhận | M1.State=Accepted; AcceptedAtUtc=2026-09-21T04:03:00Z; ProviderReceipt chứa mã tiếp nhận nếu SMTP có; lease NULL. Không có DeliveredAtUtc giả. |
| Thu hồi | S1.RevokedAtUtc=2026-09-22T02:00:00Z; receipt SR2 Operation=Revoke; D1/J1/EX1/FP1 không đổi. |

Khi lấy lại link đang hiệu lực, giải mã CT1 cho owner, không tạo token khác. Nếu owner gửi POST tạo link với ngày 2026-09-30 trong lúc S1 còn hiệu lực, không tạo dòng share mới: trả 200 với S1, ExpiryDate=2026-09-25 và requestedExpiryApplied=false; chỉ thêm một dòng EstimateShareReceipt Operation=Create trỏ S1. Sau thu hồi S1, POST tạo với ngày mới có thể tạo S2 và đổi pointer. Request M1 vẫn tham chiếu S1: không đổi sang S2 rồi tự gửi cho người nhận cũ. Người nhận đã tải FP1 giữ tệp trên máy; yêu cầu tải mới bằng S1 bị từ chối.

**Notes**:

- Chuẩn hóa: kết quả chuyên môn chỉ ở J1; export là bản chuyển định dạng có FK nguồn; share chỉ là quyền; email chỉ là ý định gửi. Ngày và ExpiresAtUtc lưu cùng nhau để giải thích lựa chọn và kiểm nhanh, store tính một lần theo múi giờ cố định, không cho client gửi hai giá trị mâu thuẫn. ProtectedToken/hash là hai biểu diễn cùng secret cho hai mục đích, cùng ghi lúc tạo và không sửa độc lập.
- Index: export unique source/format/NameVersion, State/UpdatedAtUtc/lease cho worker; token hash unique; share (EstimateId,CreatedAtUtc DESC,Id); receipt unique actor/operation/key; email (State,LeaseUntilUtc,CreatedAtUtc,Id) cho nhận việc và (RequestedBy,CreatedAtUtc DESC,Id) cho đọc trạng thái owner. Không index Recipient hoặc JSON toàn bộ khi chưa có truy vấn.
- Tất cả key tiếp nhận giữ lâu bằng lịch sử nghiệp vụ hiện có; retention chưa chốt, không tự TTL khiến cùng key có thể gửi lại hoặc tạo tác động lặp. Rate limit theo owner/share/IP cần cấu hình trước public launch, không biến thành quota thiết kế; no-store cho DTO/tệp có dữ liệu riêng tư.
- Ranh giới external: gọi AI lấy tệp, đọc tệp theo URL và gọi SMTP đều không nằm trong transaction giữ khóa. DB commit lỗi sau khi lấy được URL tệp thì export vẫn Pending; worker lần sau lấy lại cùng URL từ cùng nguồn, không có tệp mồ côi phía backend vì backend không ghi tệp. DB lỗi sau SMTP acceptance là Unknown nếu chưa ghi được bằng chứng, không rollback email.
- URL tệp mở được lúc chốt không bảo đảm AI giữ tệp mãi. Khi đọc tệp từ URL lỗi sau thành công, trả 503 DependencyUnavailable, giữ kết quả nguồn và cho yêu cầu lại; không hoàn/giữ thêm lượt. Không gán Ready chỉ vì URL có đuôi .pdf hoặc .xlsx; phải đọc thử được.
- Migration: thêm bảng export/share/email và FK con trước con trỏ CurrentShareId; giữ NULL cho bản chưa chia sẻ. EstimateExport tạo sẵn cột NameVersion và unique ba cột, phụ thuộc cột Estimate.NameVersion của TDD-PROJ-001. Backend chưa từng trả URL tệp gốc cho client, nên không có URL cũ nào cần thu hồi; thu hồi link chỉ có hiệu lực vì mọi lần tải đi qua route của backend. Chưa có dữ liệu thật để chọn kế hoạch chuyển; không chạy migration trong tác vụ này.
- Vận hành: metric queue age/export failures/mail Unknown/share denied; log requestId/exportId/shareId/operationId và mã lỗi, không log token/link đầy đủ/người nhận/địa chỉ/URL tệp. Readiness tệp/mail tách khỏi liveness API; mail hỏng không chặn owner xem nguồn. Ngưỡng cảnh báo, retry timeout, retention và người vận hành còn cần cấu hình. Không gửi email thật trong lúc soạn tài liệu.
- Kiểm chứng bằng ST-PROJ-033–047 và 057, ST-PROJ-059–060 kiểm biên hết ngày và một link đang hiệu lực. ST-PROJ-069 kiểm STORY-PROJ-003/AC-006 (đổi tên rồi tải lại PDF qua owner và link); ST-PROJ-070 kiểm nhánh dùng lại link với ngày khác trả 200 kèm ngày hiện có; UT-PROJ-038 đã được sửa theo nhánh này ngày 25/09/2026. Integration dùng DB, một máy chủ tệp giả lập phục vụ URL tệp và SMTP sandbox cho lỗi commit, gửi lặp/Unknown, truy cập Range sau thu hồi; browser thật để chứng minh QR/fragment/header/download hoạt động. Đặc tả Unit Test chi tiết nằm trong `unittest/` (UT-PROJ-*).

## Internal API

### Endpoints

JSON thành công dùng Result<T>; binary stream không bọc JSON. Idempotency-Key cho mọi POST mutation, no-store cho các response. Policy owner gồm verified Customer và cùng OwnerId, kiểm Origin theo [TDD-AUTH-001](TDD-AUTH-001.md) khi dùng cookie; không yêu cầu subscription. Policy public chỉ xét share/header, bỏ qua cookie để không phát sinh quyền owner từ token chia sẻ.

- **GET** `/api/v1/estimates/{estimateId}/result` — Owner nhận `{operationId,contractVersion,estimateName,dossier,exportAvailability}` từ nguồn Succeeded. `estimateName` là Estimate.Name hiện tại; `exportAvailability` chỉ tính export của NameVersion hiện tại. DTO dossier chi tiết chờ AI; mỗi tệp trong dossier chỉ có `fileId` để gọi route result-files bên dưới, không trả URL tệp gốc, raw provider credential hoặc toàn bộ envelope tùy ý.
- **POST** `/api/v1/estimates/{estimateId}/exports` — `{format:"Pdf"|"Xlsx"}` + key; 202 khi Pending, 200 khi đã Ready hoặc replay; `{exportId,state,attemptNumber}`. Tìm hoặc tạo export theo NameVersion hiện tại, nên sau đổi tên sẽ tạo export mới. Failed + key mới chủ động → attempt mới; cùng key trả attempt đã nhận và currentState.
- **GET** `/api/v1/estimates/{estimateId}/exports/{exportId}` — Owner đọc trạng thái tệp/attempt, mã lỗi và `isCurrent` (false khi export thuộc tên cũ); Ready và isCurrent=true mới có đường tải backend.
- **GET** `/api/v1/estimates/{estimateId}/exports/{exportId}/file` — Owner tải binary đúng nguồn Ready của NameVersion hiện tại, application/pdf hoặc XLSX MIME chuẩn; backend đọc tệp từ OutputUrl rồi chuyển tiếp, không redirect. 409 ExportNotReady nếu chưa sẵn sàng, 409 ExportOutdated nếu export thuộc tên cũ, 503 nếu không đọc được tệp từ URL đã lưu.
- **GET** `/api/v1/estimates/{estimateId}/result-files/{fileId}` — Owner xem/tải ảnh/bản vẽ thuộc kết quả Succeeded; `fileId` là `EstimateResultFile.Id` của TDD-PROJ-002. Backend đọc tệp từ FileUrl rồi chuyển tiếp, không đọc candidate và không trả URL gốc.
- **POST** `/api/v1/estimates/{estimateId}/shares` — `{expiryDate:"YYYY-MM-DD"}` + key; trả `{shareId,url,expiryDate,expiresAtUtc,state,requestedExpiryApplied}`. 201 khi tạo share mới với ngày yêu cầu (requestedExpiryApplied=true). 200 khi dùng lại link còn hiệu lực: expiryDate/expiresAtUtc là ngày hiện có của link; requestedExpiryApplied=false nếu ngày yêu cầu khác, true nếu trùng ngày đang có. Không trả 409 cho trường hợp này; muốn đổi ngày thì thu hồi rồi tạo lại.
- **GET** `/api/v1/estimates/{estimateId}/shares/current` — Owner đọc link current và trạng thái; chưa có trả value=null; không tự gia hạn/tạo lại.
- **GET** `/api/v1/estimates/{estimateId}/shares/{shareId}/qr` — Owner nhận PNG QR của đúng URL current còn hiệu lực; không gọi dịch vụ QR ngoài.
- **POST** `/api/v1/estimates/{estimateId}/shares/{shareId}/revoke` — Body `{}` + key; 200 trạng thái thực, thu hồi lặp không tạo quyền mới. Không đổi inputVersion/quota.
- **POST** `/api/v1/estimates/{estimateId}/shares/{shareId}/emails` — `{recipient}` + key; 202 `{emailRequestId,state:"Queued"}`, replay 200 trạng thái đã có. Phải là link current còn hiệu lực; không chọn token khác tự động.
- **GET** `/api/v1/estimates/{estimateId}/emails/{emailRequestId}` — Owner đọc state và failureCode; Accepted nghĩa SMTP đã nhận, Unknown không biết đã nhận; không có dữ liệu đã mở thư.
- **GET** `/api/v1/public/estimate-shares/{shareId}` — Header X-Estimate-Share-Token, không cần login; trả dossier cùng whitelist dữ liệu như hồ sơ đã chia sẻ, gồm tên hiện tại của bản dự toán; không trả owner email/quota/input riêng chưa thuộc hồ sơ.
- **POST** `/api/v1/public/estimate-shares/{shareId}/exports` — Header share + key + `{format}`; chuẩn bị tải từ cùng nguồn theo NameVersion hiện tại nếu chưa có file, không AI/quota. Không ambient cookie, không cấp quyền khác; rate limit riêng.
- **GET** `/api/v1/public/estimate-shares/{shareId}/exports/{exportId}` — Header share, đọc trạng thái export và `isCurrent` của cùng operation được chia sẻ.
- **GET** `/api/v1/public/estimate-shares/{shareId}/exports/{exportId}/file` — Header share, stream sau kiểm grant hiện tại, source và export của NameVersion hiện tại; export thuộc tên cũ trả 409 ExportOutdated. Backend chuyển tiếp nội dung đọc từ OutputUrl; không trả URL tệp gốc.
- **GET** `/api/v1/public/estimate-shares/{shareId}/files/{fileId}` — Header share; ảnh/bản vẽ phải thuộc chính result đã chia sẻ và Succeeded. Backend kiểm link còn hiệu lực rồi mới đọc tệp từ FileUrl và chuyển tiếp.

Các GET file nếu hỗ trợ HEAD hoặc Range phải dùng cùng policy, không tạo route bỏ kiểm token. Frontend tải qua fetch có share header; không gắn trực tiếp URL tệp gốc vào img/download, và API không trả URL đó. Cơ chế stream/trình xem frontend cần triển khai cùng hợp đồng này, chưa có mã frontend trong workspace.

### Examples

#### POST /api/v1/estimates/{estimateId}/shares

```
Request:
Idempotency-Key: share-estimate-1
Origin: https://app.example.test
{"expiryDate":"2026-09-25"}

Response 201:
{"value":{"shareId":"33333333-3333-4333-8333-333333333333","url":"https://example.test/vi/estimates/shared/33333333-3333-4333-8333-333333333333#token=<random-token>","expiryDate":"2026-09-25","expiresAtUtc":"2026-09-25T17:00:00Z","state":"Active","requestedExpiryApplied":true},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Validation Error","code":"InvalidExpiryDate","status":422,"detail":"Chọn ngày hết hạn từ hôm nay trở đi theo giờ Việt Nam.","messageCode":"InvalidExpiryDate","errors":null}
```

Domain example.test và token là placeholder minh họa, không phải URL triển khai. Ngày tạo ví dụ là 21/09/2026. Nếu sau đó owner gửi `{"expiryDate":"2026-09-30"}` với key mới khi link này còn hiệu lực, response là 200 với cùng shareId/url, `"expiryDate":"2026-09-25"`, `"expiresAtUtc":"2026-09-25T17:00:00Z"` và `"requestedExpiryApplied":false`; giao diện báo ngày 30/09 không được áp dụng.

#### POST /api/v1/estimates/{estimateId}/shares/{shareId}/emails

```
Request:
Idempotency-Key: email-estimate-1
Origin: https://app.example.test
{"recipient":"recipient@example.test"}

Response 202:
{"value":{"emailRequestId":"44444444-4444-4444-8444-444444444444","state":"Queued"},"isSuccess":true,"isFailure":false,"error":{"code":"","message":""}}

Error Response:
{"title":"Validation Error","code":"InvalidRecipient","status":422,"detail":"Nhập đúng một địa chỉ email hợp lệ.","messageCode":"InvalidRecipient","errors":null}
```

#### GET /api/v1/public/estimate-shares/{shareId}/exports/{exportId}/file

```
Request:
X-Estimate-Share-Token: <random-token>

Response 200:
Content-Type: application/pdf
Cache-Control: private, no-store
Content-Disposition: attachment; filename="estimate.pdf"
<binary PDF của đúng kết quả được chia sẻ>

Error Response:
{"title":"Not Found","code":"ShareUnavailable","status":404,"detail":"Link không khả dụng.","messageCode":"ShareUnavailable","errors":null}
```

### Error Codes

- **Unauthorized** (401): phiên owner không hợp lệ.
- **AccessForbidden** (403): không đáp ứng policy tài khoản cho thao tác owner.
- **CsrfInvalid** (403): mutation dùng cookie có `Origin`/`Referer` ngoài danh sách được phép, hoặc thiếu cả hai; theo [TDD-AUTH-001](TDD-AUTH-001.md).
- **EstimateNotFound** (404): bản không thuộc owner.
- **ResultNotReady** (409): chưa có operation Succeeded đủ kết quả.
- **ExportNotFound** (404): export hoặc tệp kết quả không thuộc đúng bản hoặc nguồn.
- **ExportNotReady** (409): Pending/Failed chưa có tệp được cung cấp.
- **ExportOutdated** (409): export được xuất theo tên cũ trước khi bản dự toán đổi tên; client gửi yêu cầu xuất mới để nhận tệp mang tên hiện tại.
- **InvalidExportFormat** (422): format khác Pdf/Xlsx.
- **InvalidExpiryDate** (422): ngày sai định dạng hoặc đã qua theo giờ Việt Nam.
- **ShareUnavailable** (404): share/token sai, không current, hết hạn hoặc đã thu hồi; không trả nội dung.
- **EmailRequestNotFound** (404): email request không thuộc bản/owner.
- **InvalidRecipient** (422): không phải đúng một địa chỉ hợp lệ.
- **IdempotencyConflict** (409): cùng key khác body hoặc phạm vi tài nguyên.
- **DependencyUnavailable** (503): nguồn tệp xuất/token protector/mail adapter chưa sẵn sàng trước tiếp nhận, hoặc không đọc được tệp từ URL đã lưu.

Email đã được nhận vào hàng đợi nhưng sau đó thất bại trả trạng thái Rejected/Unknown/Skipped qua GET 200, không dùng 200 để nói đã gửi thành công. 409/503 và format lỗi cần nối vào middleware theo TDD-PROJ-001; không trả thông tin SMTP nhạy cảm.

## External API

### Endpoints

- **Tệp kết quả của AI (ảnh, bản vẽ, PDF, Excel) — đọc theo URL** — AI trả URL cho mọi tệp; backend lưu URL, đọc tệp qua `IEstimateResultFileClient` và chuyển tiếp cho người có quyền. Backend không tự dựng PDF/Excel và không có kho tệp riêng. Hợp đồng về endpoint AI, thời hạn URL, xác thực và cách có tệp mang tên mới (Architecture/Notes) chưa có.
- **SMTP — cấu hình mail hiện có** — Tái sử dụng MailKit và MailOption qua adapter có trạng thái tiếp nhận; không public endpoint gửi email tùy ý hoặc attachment.

### Fields

- **sourceOperationId** — Ràng buộc tất cả tệp với đúng bộ kết quả đã thành công.
- **recipient** — Một địa chỉ đã được kiểm tra; không BCC/CC hoặc tự gửi thêm email tài khoản.
- **shareUrl** — URL của share đã lưu, hạn/thu hồi không thay khi đưa vào email/QR.
- **acceptanceEvidence** — Xác nhận SMTP tiếp nhận nếu có; không chứng minh phát hoặc đọc thư.

### Error Handling

Không giữ SQL transaction trong quá trình gọi SMTP, gọi AI lấy tệp hoặc đọc tệp theo URL. Đọc tệp chỉ gọi URL https thuộc tên máy chủ cho phép, không theo chuyển hướng sang máy chủ khác (TDD-PROJ-002/External API). Export lỗi giữ nguồn; Unknown mail không tự gửi lại; link bị thu hồi trong khi email đã gửi vẫn chặn request mới qua policy. Không trả đường tải trực tiếp độc lập làm mất hiệu lực thu hồi. Retry việc xuất chỉ dùng dữ liệu đã lưu, không gọi lại tạo thiết kế AI.

### Quirks

- Thời hạn link và thời hạn subscription độc lập.
- Chỉ hứa chặn yêu cầu truy cập mới; không hứa thu hồi bytes đã tải hoặc thư đã gửi.
- MailService hiện không phân biệt lỗi SendAsync và lỗi DisconnectAsync; cần adapter bổ sung trước khi dùng kết quả Task để hiển thị trạng thái gửi.
- Chưa có API AI thực nên phần DTO chuyên môn và hiện thực `IEstimateExportFileSource` chưa thể hoàn chỉnh; schema điều phối và quyền đọc không phụ thuộc hợp đồng đó.
- Backend chuyển tiếp tệp nên băng thông tải tệp đi qua API; chưa có số liệu dung lượng tệp để đặt giới hạn.

## References

### User Stories

- STORY-PROJ-003
- STORY-PROJ-004

### Business Rules

- BR-PROJ-006/Then
- BR-PROJ-007/Then
- BR-SUB-003/Then
- BR-SUB-007/Then

### Use Cases

- STORY-PROJ-003/Main Flow
- STORY-PROJ-003/ALT-01
- STORY-PROJ-003/AC-006
- STORY-PROJ-004/Main Flow
- STORY-PROJ-004/ALT-01
- STORY-PROJ-004/ALT-02
- STORY-PROJ-004/ALT-03
- STORY-PROJ-004/AC-010

### Others

- TDD-PROJ-001/Data Model
- TDD-PROJ-001/Architecture
- TDD-PROJ-002/Data Model
- TDD-PROJ-002/Architecture
- TDD-PROJ-002/External API
- [Truy vết và điểm còn mở](../discovery/estimate-technical-design.md).
- [IMailService](../../bmt-be/src/bmt-be.application/abstractions/IMailService.cs), [MailService](../../bmt-be/src/bmt-be.infrastructure/mail/MailService.cs), [Cookie helper](../../bmt-be/src/bmt-be.presentation/abstractions/AuthCookieHelper.cs).

## Change Log

- 2026-09-26 (lưu URL tệp): Theo quyết định người dùng ngày 26/09/2026, backend không có kho tệp riêng và không tự dựng PDF/Excel; AI trả URL cho mọi tệp kết quả. Bỏ `EstimateExport.OutputAssetId` cùng khóa ngoại tới `EstimateAsset`, thay bằng cột `OutputUrl`; dữ liệu mẫu FP1/FP2 là URL. Đổi route `result-assets/{assetId}` thành `result-files/{fileId}` và `public/estimate-shares/{shareId}/assets/{assetId}` thành `public/estimate-shares/{shareId}/files/{fileId}` (`fileId` là `EstimateResultFile.Id` của TDD-PROJ-002). `EstimateFileReader` đọc tệp từ URL đã lưu và chuyển tiếp sau khi kiểm quyền, không redirect và không lộ URL gốc, để thu hồi link chặn được lần tải mới (BR-PROJ-006 khoản 4). Thay `IEstimateDocumentExporter` bằng `IEstimateExportFileSource`; bỏ "Private storage" khỏi External API. Thêm câu hỏi mở về cách có tệp PDF/Excel mang tên mới sau khi đổi tên (BR-PROJ-007 khoản 7, STORY-PROJ-003/AC-006), đề xuất đặt tên mới ở tên tệp tải về, chưa chốt.
- 2026-09-26 (CSRF): Chống CSRF dẫn tới [TDD-AUTH-001](TDD-AUTH-001.md); ví dụ bỏ header `X-CSRF-Token`, thay bằng `Origin`.
- 2026-09-25: Dùng lại link còn hiệu lực trả 200 kèm ngày hết hạn hiện có và cờ `requestedExpiryApplied`, bỏ mã lỗi `ExistingShareActive` (BR-PROJ-006 khoản 9, STORY-PROJ-004/AC-010). Thêm `EstimateExport.NameVersion`, đổi unique thành `(OperationId,Format,NameVersion)` và mã lỗi `ExportOutdated` (409) để tệp xuất theo tên cũ không được phục vụ sau khi đổi tên; lần tải tiếp theo xuất lại từ kết quả đã lưu, không tính lượt, không gọi AI (BR-PROJ-007 khoản 7, STORY-PROJ-003/AC-006). Hồ sơ của chủ sở hữu, trang xem qua link và email luôn đọc tên hiện tại. Ghi rõ cấu trúc dự toán áp dụng cho mọi loại công trình trong danh mục; bổ sung Use Cases và mẫu dữ liệu tương ứng.
