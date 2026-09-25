# Kế hoạch nghiệp vụ thư viện mẫu

## Trạng thái

Các quyết định nghiệp vụ đã được xác nhận qua hội thoại. Người dùng đã xác nhận “chốt US và BR” cho ba User Story và ba Business Rule cùng quy tắc lượt liên quan. Đã bổ sung 28 đặc tả System Test; xem [bảng truy vết](library-system-test-coverage.md). Reviewer và Approver: Tân Trần. Creator, Assignee, Owner và ngày hiệu lực chưa được cung cấp; không tự gán. Chưa triển khai, chạy test hoặc chốt thiết kế dữ liệu/API.

## Cập nhật ngày 25/09/2026

- Quyền quản lý thư viện mẫu theo STORY-RBAC-001 có mã kỹ thuật `library.manage` trong TDD-RBAC-001. Quyền này không gắn phân công; hệ thống kiểm theo mã quyền, không theo tên vai trò Admin (BR-LIB-002).
- TDD-LIB-002 đổi thứ tự khóa khi mở mẫu lần đầu: khóa `AccountCommerceState` của khách trước, rồi tới kỳ và lượt, sau cùng là mẫu và phiên bản. Thứ tự này thống nhất với TDD-PAY-001 và TDD-PROJ-002, thay cho việc khóa dòng `User` trước đây.
- Thêm ST-LIB-028 kiểm việc mở mẫu lần đầu và thay đổi gói của cùng khách chạy đồng thời; ST-LIB-011 kiểm thêm vai trò tùy chỉnh có `library.manage`.

## Bộ tài liệu

| Phạm vi | User Story | Business Rule |
|---|---|---|
| Quản trị nội dung và phiên bản | [STORY-LIB-001](../userstory/STORY-LIB-001.md) | [BR-LIB-001](../businessrule/BR-LIB-001.md), [BR-LIB-002](../businessrule/BR-LIB-002.md) |
| Danh sách và bộ lọc công khai | [STORY-LIB-002](../userstory/STORY-LIB-002.md) | BR-LIB-001, BR-LIB-002, BR-LIB-003 |
| Lượt xem và lịch sử từng phiên bản | [STORY-LIB-003](../userstory/STORY-LIB-003.md) | [BR-LIB-003](../businessrule/BR-LIB-003.md) |

## Hiện trạng và thay đổi

- Trang tham khảo: https://vnz-bmt-savico-abcxyz.vercel.app/vi/handbook?tab=library. Đã quan sát danh sách 2D/3D, bộ lọc loại và quy mô, tìm kiếm, phân trang, nút Lưu mẫu và nhãn lượt theo ngày. Yêu cầu đã chốt thay quy mô bằng số tầng, bổ sung bộ lọc tum, bỏ Lưu mẫu; không lấy nhãn lượt theo ngày làm quy tắc hạn mức.
- Danh mục dùng chung đã được mô tả trong STORY-PROJ-005 và BR-PROJ-004. Không tạo danh mục loại công trình riêng cho thư viện.
- BR-SUB-017 và STORY-SUB-001 đã cập nhật nguyên tắc lượt theo tài khoản/phiên bản. Chưa hoàn tất rà soát toàn bộ chuỗi tài liệu phụ thuộc.
- TDD-SUB-002 còn mô tả tính lượt theo lần mở/Idempotency-Key. Thiết kế đó cần được sửa để phân biệt yêu cầu gửi lại với quyền xem lại phiên bản; không coi việc chống gửi trùng đã đáp ứng quyền xem lại.
- ST-SUB-051 và các ST/UT về tra cứu, hết hạn, thiếu quyền và gửi lại cần đối chiếu lại. Đã cập nhật ST-SUB-050–052 và thêm ST-LIB-001–027; phần phụ thuộc ngoài tập này vẫn cần rà soát khi thiết kế tích hợp.
- Khảo sát tên file backend chưa tìm thấy thành phần Library/BuildingType/Entitlement; chưa kiểm tra đầy đủ implementation, không khẳng định các tính năng đã tồn tại hoặc chưa tồn tại chỉ từ kết quả này.

## Thứ tự công việc đề xuất

1. Đã chốt toàn văn ba US và ba BR cùng thay đổi nghiệp vụ gói/lượt trong hội thoại.
2. Đã viết 27 System Test theo các AC đã chốt và cập nhật ST-SUB-050–052; bao phủ phiên bản, xem lại sau hết hạn, ẩn mẫu, quyền truy cập ảnh/tệp, xác nhận lượt, lỗi và mở đồng thời.
3. Khảo sát code và đọc đủ phụ thuộc quyền/gói/lượt/danh mục. Thiết kế TDD bằng các skill chuyên môn về thiết kế kỹ thuật, kiến trúc dữ liệu, schema, ERD và chuẩn hóa; bổ sung bảo mật, luồng lưu tài nguyên/tính lượt khi thiết kế các phần tương ứng. Chưa quyết định schema/API trong kế hoạch nghiệp vụ này.
4. Trong TDD, làm rõ cách lưu bản nháp, định danh phiên bản, giữ tệp của bản cũ, chống trừ trùng theo tài khoản/phiên bản, thay đổi đồng thời lúc xác nhận lượt và giới hạn truyền tải hạ tầng. Không tự đặt giới hạn nghiệp vụ tải lên.
5. Sau khi chốt TDD mới viết đặc tả Unit Test. Người dùng chốt TDD ngày 25/09/2026; đã có UT-LIB-001 đến UT-LIB-050, chưa có mã test. Triển khai chỉ khi người dùng giao triển khai; kiểm chứng bằng test phù hợp rồi bàn giao kết quả thực chạy.

## Phần không làm

Không có yêu thích, xoay mô hình 3D trên web, tìm theo kích thước, lựa chọn sắp xếp, sửa phiên bản đã được thay thế hoặc xóa phiên bản đã công bố. Không tự thêm quy trình duyệt của người khác.

## Bản thiết kế kỹ thuật để chốt

Đã tách thiết kế thành [TDD-LIB-001 — Quản lý thư viện](../tdd/TDD-LIB-001.md) và [TDD-LIB-002 — Tra cứu và lịch sử](../tdd/TDD-LIB-002.md), đồng thời cập nhật phần tra cứu của [TDD-SUB-002](../tdd/TDD-SUB-002.md). Người dùng chốt hai TDD ngày 25/09/2026. Đặc tả Unit Test: UT-LIB-001 đến UT-LIB-032 theo TDD-LIB-001, UT-LIB-033 đến UT-LIB-050 theo TDD-LIB-002; chưa có mã test hoặc kết quả chạy.

Thiết kế tách Template, Version, Asset/link, Access và receipt quản trị; dùng lại catalog PROJ và quota SUB. Dữ liệu kỳ/quota, chứng từ lượt và quyền xem ghi cùng transaction. Permission library.manage không yêu cầu Assignment. Đã xác nhận cho lưu nháp thiếu thông tin; kiểm đủ trước khi công bố.

Phần cần cấu hình trước production: nhà cung cấp object store, bộ kiểm file/thumbnail, giới hạn truyền tải và timeout, sao lưu/khôi phục. Không tự đặt hạn mức file nghiệp vụ. Chưa triển khai hoặc chạy kiểm thử ứng dụng. Chuỗi phụ thuộc ngoài LIB chưa được rà soát toàn bộ; UT-SUB về replay cũ cần cập nhật sau khi TDD được chốt.

Phân chia nguồn thiết kế: TDD-LIB-001 sở hữu năm bảng Template/Version/Asset/VersionAsset/MutationReceipt; TDD-LIB-002 sở hữu LibraryAccess và bổ sung UsageOperation. Schema và API không được nhân đôi khi tách. Việc tách không phát sinh mã ứng dụng; đặc tả Unit Test được viết sau khi người dùng chốt TDD ngày 25/09/2026.
