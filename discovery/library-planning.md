# Kế hoạch nghiệp vụ thư viện mẫu

## Trạng thái

**Cập nhật 30/09/2026:** đã cập nhật [BR-LIB-001](../businessrule/BR-LIB-001.md), [STORY-LIB-001](../userstory/STORY-LIB-001.md) và [STORY-LIB-002](../userstory/STORY-LIB-002.md) theo quyết định mới trong hội thoại. Mẫu 3D dùng nhiều phong cách kiến trúc/nội thất từ danh mục dự toán; mỗi nhóm đang bật cần ít nhất một lựa chọn khi công bố. Tìm mẫu từ dự toán phải khớp tất cả điều kiện áp dụng bằng ID ổn định và giá trị tầng/tum, không yêu cầu cùng phiên bản danh mục. Cùng tên nhưng khác ID không được tự ghép; không có kết quả thì trả rỗng. Mẫu 2D không áp dụng phong cách.

Người dùng đã chốt US/BR và TDD bổ sung trong hội thoại. Đã có 20 đặc tả System Test ST-LIB-032–051 cho phần bổ sung, trong đó 10 ca ST-LIB-042–051 viết sau khi chốt hai luồng tìm; thêm 26 đặc tả Unit Test UT-LIB-053–078 theo TDD. Xem [bảng System Test](library-system-test-coverage.md) và [bảng Unit Test bổ sung](library-unit-test-coverage.md). Đã triển khai phần BE trong workspace, thêm migration và chạy kiểm thử backend. Người dùng yêu cầu chỉ tập trung BE; chưa triển khai giao diện hoặc áp migration lên môi trường dùng chung. Kết quả chi tiết ở bảng Unit Test bổ sung; các ghi nhận lịch sử bên dưới thuộc những đợt trước.

Thiết kế dùng bảng liên kết nhiều phong cách theo phiên bản mẫu và ID danh mục ổn định để tìm qua các phiên bản danh mục. Người dùng xác nhận chưa có mẫu 3D nên không cần backfill; trước triển khai vẫn phải kiểm lại môi trường đích. Trang thư viện lọc tự do: nhiều mục mỗi nhóm, khớp ít nhất một trong nhóm và đồng thời giữa các nhóm. Khi AI đang xử lý, FE tự lấy mẫu 2D/3D theo đúng đầu vào dự toán đã được tiếp nhận; bỏ phản hồi cũ, không dùng filter handbook thay đầu vào đó.

Không còn quyết định nghiệp vụ mở trong nhóm trao đổi này. Tần suất tự làm mới khi thư viện đổi trong lúc chờ chưa được đặt ra; không tự thêm polling thư viện. Hai trang tham khảo không truy cập được bằng công cụ trong lần đối chiếu này; mô tả của người dùng là nguồn nghiệp vụ. Chuỗi phụ thuộc ngoài LIB và danh mục trực tiếp chưa được rà soát đầy đủ trong lần cập nhật này.

**Ghi nhận giai đoạn chuẩn bị ban đầu:** các quyết định nghiệp vụ đã được xác nhận qua hội thoại. Người dùng đã xác nhận “chốt US và BR” cho ba User Story và ba Business Rule cùng quy tắc lượt liên quan. Đã bổ sung 28 đặc tả System Test; xem [bảng truy vết](library-system-test-coverage.md). Reviewer và Approver: Tân Trần. Creator, Assignee, Owner và ngày hiệu lực chưa được cung cấp; không tự gán. Chưa triển khai, chạy test hoặc chốt thiết kế dữ liệu/API.

## Cập nhật ngày 25/09/2026

- Quyền quản lý thư viện mẫu theo STORY-RBAC-001 có mã kỹ thuật `library.manage` trong TDD-RBAC-001. Quyền này không gắn phân công; hệ thống kiểm theo mã quyền, không theo tên vai trò Admin (BR-LIB-002).
- TDD-LIB-002 đổi thứ tự khóa khi mở mẫu lần đầu: khóa `AccountCommerceState` của khách trước, rồi tới kỳ và lượt, sau cùng là mẫu và phiên bản. Thứ tự này thống nhất với TDD-PAY-001 và TDD-PROJ-002, thay cho việc khóa dòng `User` trước đây.
- Thêm ST-LIB-028 kiểm việc mở mẫu lần đầu và thay đổi gói của cùng khách chạy đồng thời; ST-LIB-011 kiểm thêm vai trò tùy chỉnh có `library.manage`.

## Cập nhật ngày 26/09/2026 — triển khai TDD-LIB-001

- Phần quản trị và danh sách công khai (TDD-LIB-001) đã có code ở nhánh `feature/library-admin` của `bmt-be`, commit `66e4671`, tách từ `develop` tại `79faf34`, chưa merge. Gồm năm bảng nội dung/quản trị, migration `20260926102541_LibraryTemplates`, mã quyền `library.manage` seed cho vai trò `admin`, API quản trị `/api/v1/admin/library/templates` và API công khai `GET /api/v1/design-templates`, `GET /api/v1/design-templates/filters`.
- Migration mới áp dụng lên PostgreSQL trong container kiểm thử, chưa áp dụng lên database dùng chung.
- Đặc tả UT-LIB-001 đến UT-LIB-032 đã có mã test; tên test ghi trong từng đặc tả. Integration test PostgreSQL kiểm ràng buộc, khóa ngoại tới danh mục, công bố/ẩn đồng thời, lọc và phân trang công khai; test API kiểm quyền `library.manage`.
- Phần tra cứu của TDD-LIB-002 (quyền xem, tính lượt, lịch sử, tải nội dung được bảo vệ) chưa triển khai. Người quản lý hiện chưa có API đọc danh sách tài nguyên của một phiên bản; đây là câu hỏi còn mở, ghi trong báo cáo triển khai.
- System Test ST-LIB chưa chạy trên môi trường thử; bảng ở [library-system-test-coverage.md](library-system-test-coverage.md) ghi các test tự động phía backend liên quan.

## Cập nhật ngày 26/09/2026 — đọc tài nguyên cho người quản lý

- Người dùng xác nhận hai câu hỏi mở sau đợt triển khai TDD-LIB-001: thêm route `GET /api/v1/admin/library/templates/{templateId}/versions/{versionId}/assets` cho người có `library.manage` (phân trang, URL gốc, loại, vị trí, cover), và làm log thao tác quản trị cùng đợt TDD-LIB-002.
- Route đã có ở nhánh `feature/library-admin-assets` của `bmt-be`, commit `9e02c4d`, chưa merge; không cần migration. Đặc tả mới: UT-LIB-051, UT-LIB-052, ST-LIB-031.

## Cập nhật ngày 26/09/2026 — triển khai TDD-LIB-002

- Phần tra cứu (TDD-LIB-002) đã có code ở nhánh `feature/library-access` của `bmt-be`, commit `0263297`, tách từ `develop` tại `1d39450`, chưa merge. Gồm bảng `LibraryAccess`, phần mở rộng `UsageOperation`, migration `20260926115537_LibraryAccess` và sáu route: access-info, mở phiên bản (`POST /api/v1/design-templates/{templateId}/open`), lịch sử `/api/v1/me/library-history`, chi tiết phiên bản, trang tài nguyên và route tải tệp chuyển tiếp qua backend.
- Mở lần đầu tính đúng một lượt và ghi quyền xem cùng transaction, theo thứ tự khóa `AccountCommerceState` → kỳ/quota → mẫu → phiên bản; mở lại phiên bản đã có quyền xem không tính lượt và không phụ thuộc gói. Nội dung được bảo vệ chỉ tải qua backend, không lộ URL gốc; backend chỉ đọc URL thuộc `UploadedFileOption__AllowedHosts`.
- Log cấp quyền xem và log thao tác quản trị của TDD-LIB-001 đã làm cùng đợt.
- Migration mới chạy thử trên PostgreSQL tạm và container kiểm thử, chưa áp dụng lên database dùng chung. Đặc tả UT-LIB-033 đến UT-LIB-050 đã có mã test; System Test vẫn chưa chạy trên môi trường thử. Bảng test tự động phía backend ở [library-system-test-coverage.md](library-system-test-coverage.md).
- Còn chờ bên vận hành kho presign: kho có nhận HEAD và Range không, thời gian chờ khi thăm dò và chuyển tiếp tệp (code đang dùng giá trị tạm 30 giây, cấu hình bằng `LibraryFileOption__TimeoutSeconds`).

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

Cập nhật ngày 26/09/2026: backend không có kho tệp riêng. Frontend kiểm định dạng và dung lượng tệp, upload qua dịch vụ presigned URL; backend chỉ lưu URL https thuộc `UploadedFileOption__AllowedHosts`, không nhận bytes, không tạo ảnh thu nhỏ. Nội dung được bảo vệ do backend chuyển tiếp tệp qua route có kiểm quyền, không lộ URL gốc; ảnh đại diện công khai trả thẳng URL gốc của ảnh cover để trình duyệt/CDN cache (TDD-LIB-001, TDD-LIB-002; người dùng xác nhận ngày 26/09/2026).

Phần cần cấu hình trước production: tên miền kho presign trong `UploadedFileOption__AllowedHosts`; với bên cung cấp dịch vụ presign cần làm rõ giới hạn dung lượng upload, việc hỗ trợ HEAD và Range, timeout khi backend thăm dò và chuyển tiếp tệp, việc dọn tệp mồ côi và sao lưu/khôi phục tệp. Không tự đặt hạn mức file nghiệp vụ. Chưa triển khai hoặc chạy kiểm thử ứng dụng. Chuỗi phụ thuộc ngoài LIB chưa được rà soát toàn bộ; UT-SUB về replay cũ cần cập nhật sau khi TDD được chốt.

Phân chia nguồn thiết kế: TDD-LIB-001 sở hữu năm bảng Template/Version/Asset/VersionAsset/MutationReceipt; TDD-LIB-002 sở hữu LibraryAccess và bổ sung UsageOperation. Schema và API không được nhân đôi khi tách. Việc tách không phát sinh mã ứng dụng; đặc tả Unit Test được viết sau khi người dùng chốt TDD ngày 25/09/2026.
