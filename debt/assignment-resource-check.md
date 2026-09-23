# Nợ kỹ thuật: phân công không kiểm tài nguyên có thật

**Trạng thái: Đang mang nợ — chấp nhận trong đợt hiện tại, phải trả khi có module khách hàng hoặc module dự án.**

Khi tạo phân công, hệ thống chỉ kiểm `resourceType` thuộc tập giá trị đã biết. Giá trị `resourceId` được nhận vào mà không đối chiếu với bất kỳ bản ghi nào.

## Vì sao chấp nhận

Bảng `Assignment` cố ý không có khóa ngoại tới khách hàng hay dự án, để thêm loại tài nguyên mới, ví dụ lead, chỉ cần một giá trị mới trong `ResourceType` chứ không cần bảng mới. [TDD-RBAC-003](../tdd/TDD-RBAC-003.md) đã chốt đánh đổi này và ghi rõ việc kiểm tài nguyên tồn tại thuộc trách nhiệm của handler.

Vấn đề là handler cũng chưa kiểm được: module khách hàng và module dự án đều chưa tồn tại nên không có bảng nào để tra. Hai cách còn lại đều tệ hơn:

- Chặn mọi lần tạo phân công cho tới khi có module thì ba handler còn lại không chạy được luồng thật, và cả phần phân công coi như chưa bàn giao được.
- Dựng một bảng tài nguyên tạm do người quản trị nhập tay thì phát sinh dữ liệu phải chuyển đổi khi module thật ra đời.

## Hệ quả đang chấp nhận

- Gõ nhầm `resourceId` tạo ra dòng phân công trỏ tới tài nguyên không tồn tại, và không có gì báo.
- Phép kiểm quyền vẫn an toàn. `IsAssignedAsync` hỏi "người này có phụ trách tài nguyên kia không" chứ không hỏi ngược lại, nên một dòng mồ côi không cấp quyền cho ai trên tài nguyên có thật.
- Dòng mồ côi vẫn tính vào số phân công đang hiệu lực, nên nó chặn thu hồi vai trò theo BR-RBAC-007 dù tài nguyên không có thật.

## Cách trả nợ

`CreateAssignmentCommandHandler` gọi `AssignmentLookup.RequireAssignableStaffAsync` rồi tới `EnsureNotAlreadyAssignedAsync`. Chèn bước kiểm tài nguyên vào giữa hai bước đó, dùng cùng cổng `IResourceHierarchyReader` hoặc một cổng đọc tài nguyên mới, và trả 404 khi không tra được.

Cần làm cùng lúc: rà lại các dòng `Assignment` đang có, vì dòng mồ côi tạo trước đó sẽ không tự biến mất. Việc dọn dòng mồ côi khi xóa dự án là một phần nợ riêng, cũng chưa có cơ chế.

## Tài liệu liên quan

- [TDD-RBAC-003](../tdd/TDD-RBAC-003.md): mục Architecture nêu đánh đổi, mục Data Model nêu vấn đề dòng mồ côi.
- [STORY-RBAC-003](../userstory/STORY-RBAC-003.md): luồng phân công.

Chưa có thời hạn hay người phụ trách được giao. Mốc để trả nợ là lúc module khách hàng hoặc module dự án được dựng.
