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

# TDD-001

## Document Info

- **Feature**: [Tên tính năng]
- **Author**: [Tên tác giả]
- **Reviewer**: [Tên người review]
- **Approver**: [Tên người phê duyệt]
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

[Vấn đề kỹ thuật và bối cảnh của Story]

### Goals

- [Mục tiêu kỹ thuật đo được]

### Non-goals

- [Nội dung ngoài phạm vi thiết kế]

## Architecture

[Mô tả thành phần và trách nhiệm. Giải thích các kỹ thuật quan trọng: nghĩa là gì, giải quyết vấn đề nào, dùng ở đâu và hoạt động thế nào trong luồng này; kèm ví dụ và giới hạn/đánh đổi.]

```mermaid
flowchart LR
    UI[Giao diện] --> API[Dịch vụ]
    API --> DB[(Cơ sở dữ liệu)]
```

**Notes**:
- [Quyết định kiến trúc và đánh đổi]

## Sequence Diagram

[Tương tác giữa các thành phần]

```mermaid
sequenceDiagram
    actor U as Người dùng
    participant API as Dịch vụ
    U->>API: Gửi yêu cầu
    API-->>U: Trả kết quả
```

## Activity Diagram

[Luồng xử lý và điều kiện rẽ nhánh]

```mermaid
flowchart TD
    A[Nhận yêu cầu] --> B{Hợp lệ?}
    B -->|Có| C[Xử lý]
    B -->|Không| D[Báo lỗi]
```

## State Diagram

[Vòng đời thực thể và điều kiện chuyển trạng thái]

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Completed: Dữ liệu hợp lệ
    Completed --> [*]
```

## Data Model

[Giải thích từng bảng: lưu việc gì, một dòng đại diện cho gì, khi nào được tạo/cập nhật, quan hệ với bảng khác và ý nghĩa NULL. Sau đó mô tả cột, kiểu dữ liệu, khóa, ràng buộc; phân biệt dữ liệu lưu thật với dữ liệu tính khi đọc.]

[Đưa mẫu bản ghi cho từng bảng mới/thay đổi, dùng chung một tình huống với ID/khóa ngoại nhất quán; giải thích dữ liệu trước/sau các bước quan trọng. Ghi rõ dữ liệu giả định, đơn vị, múi giờ và cột được lược bớt. Bảng dùng lại dẫn tới schema/mẫu nguồn. Nếu chỉ có projection/DTO thì minh họa dữ liệu nguồn → kết quả đọc, không tạo bảng mới. Ví dụ API không thay cho mẫu dữ liệu lưu trữ.]

```mermaid
erDiagram
    DOCUMENT {
        uuid id PK
        string title
    }
```

**Notes**:
- [Ràng buộc, index và chiến lược migration; giải thích mục đích, cách áp dụng và giới hạn của kỹ thuật được chọn bằng tình huống dùng chính dữ liệu mẫu.]

## Internal API

### Endpoints

- **POST** `/api/example` — [Mô tả endpoint]

### Examples

#### POST /api/example

```
Request:
{"name": "Ví dụ"}

Response 200:
{"id": "example-id"}

Error Response:
{"code": "INVALID_INPUT"}
```

### Error Codes

- **INVALID_INPUT** (400): [Điều kiện gây lỗi]

## External API

### Endpoints

- **Dịch vụ đối tác** — [Mục đích và hợp đồng tích hợp]

### Fields

- **external_id** — [Ý nghĩa trường và ràng buộc]

### Error Handling

[Timeout, retry, idempotency và ánh xạ lỗi]

### Quirks

- [Đặc điểm cần lưu ý của đối tác]

## References

### User Stories

- STORY-001

### Business Rules

- BR-001

### Use Cases

### Others

- Tài liệu kỹ thuật: [Nguồn tham khảo]

## Change Log
