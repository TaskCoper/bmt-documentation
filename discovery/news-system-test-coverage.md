# Phạm vi và độ phủ System Test Tin tức

Người dùng đã chốt STORY-NEWS-001–003 và BR-NEWS-001–003 trong hội thoại. Bộ đặc tả gồm 31 ca, phủ 23 tiêu chí nghiệm thu, các nhánh thay thế, ngoại lệ và yêu cầu hiển thị rich text an toàn. Đây là đặc tả chưa thực thi, không phải kết quả Pass.

**Cập nhật 26/09/2026:** người dùng xác nhận giới hạn tiêu đề 200, mô tả ngắn 500 và nội dung 200.000 ký tự (BR-NEWS-001 khoản 9, STORY-NEWS-001/AC-009, EXC-04); ST-NEWS-030 kiểm các biên này.

**Cập nhật 25/09/2026:** người dùng xác nhận tên danh mục tối đa 200 ký tự, ghi tại BR-NEWS-002 khoản 1 và STORY-NEWS-002/AC-008; ST-NEWS-028 kiểm biên 200/201 ký tự. Quyền quản lý tin tức theo STORY-RBAC-001 có mã kỹ thuật `news.manage` trong TDD-RBAC-001, không gắn phân công; ST-NEWS-011 và ST-NEWS-019 kiểm thêm vai trò tùy chỉnh được cấp mã này. ST-NEWS-029 kiểm lọc theo danh mục không còn tồn tại trả danh sách rỗng.

**Cập nhật 29/09/2026:** bỏ mô tả ngắn, số phút đọc do người viết nhập. ST-NEWS-031 kiểm dữ liệu mới và ngoại lệ bài cũ.

## Phạm vi đã chốt

- Bài rich text có ảnh cloud qua URL; lưu nháp thiếu thông tin, công bố kiểm đủ; sửa tại chỗ, ẩn/hiện và xóa mọi trạng thái, không khôi phục; tiêu đề tối đa 200, nội dung 200.000 ký tự; số phút đọc nhập tay theo BR-NEWS-001 khoản 10.
- Danh mục riêng đa cấp không giới hạn số cấp; tên tối đa 200 ký tự; một bài nhiều danh mục; kiểm tên cùng cha, ngăn vòng lặp và chặn xóa danh mục đang dùng.
- Đọc công khai miễn phí; tìm tiêu đề và lọc một nhánh, loại trùng trước phân trang; giữ ngày công bố đầu tiên.

## Đối chiếu tiêu chí nghiệm thu

| Tiêu chí | Đặc tả kiểm thử |
| --- | --- |
| [STORY-NEWS-001/AC-001](../userstory/STORY-NEWS-001.md#ac-001) | [ST-NEWS-001](../systemtest/ST-NEWS-001.md) |
| [STORY-NEWS-001/AC-002](../userstory/STORY-NEWS-001.md#ac-002) | [ST-NEWS-002](../systemtest/ST-NEWS-002.md), [ST-NEWS-003](../systemtest/ST-NEWS-003.md), [ST-NEWS-007](../systemtest/ST-NEWS-007.md), [ST-NEWS-008](../systemtest/ST-NEWS-008.md) |
| [STORY-NEWS-001/AC-003](../userstory/STORY-NEWS-001.md#ac-003) | [ST-NEWS-002](../systemtest/ST-NEWS-002.md) |
| [STORY-NEWS-001/AC-004](../userstory/STORY-NEWS-001.md#ac-004) | [ST-NEWS-004](../systemtest/ST-NEWS-004.md), [ST-NEWS-005](../systemtest/ST-NEWS-005.md), [ST-NEWS-027](../systemtest/ST-NEWS-027.md) |
| [STORY-NEWS-001/AC-005](../userstory/STORY-NEWS-001.md#ac-005) | [ST-NEWS-006](../systemtest/ST-NEWS-006.md), [ST-NEWS-007](../systemtest/ST-NEWS-007.md) |
| [STORY-NEWS-001/AC-006](../userstory/STORY-NEWS-001.md#ac-006) | [ST-NEWS-009](../systemtest/ST-NEWS-009.md) |
| [STORY-NEWS-001/AC-007](../userstory/STORY-NEWS-001.md#ac-007) | [ST-NEWS-010](../systemtest/ST-NEWS-010.md) |
| [STORY-NEWS-001/AC-008](../userstory/STORY-NEWS-001.md#ac-008) | [ST-NEWS-011](../systemtest/ST-NEWS-011.md) |
| [STORY-NEWS-001/AC-009](../userstory/STORY-NEWS-001.md#ac-009) | [ST-NEWS-030](../systemtest/ST-NEWS-030.md) |
| [STORY-NEWS-002/AC-001](../userstory/STORY-NEWS-002.md#ac-001) | [ST-NEWS-012](../systemtest/ST-NEWS-012.md) |
| [STORY-NEWS-002/AC-002](../userstory/STORY-NEWS-002.md#ac-002) | [ST-NEWS-013](../systemtest/ST-NEWS-013.md) |
| [STORY-NEWS-002/AC-003](../userstory/STORY-NEWS-002.md#ac-003) | [ST-NEWS-014](../systemtest/ST-NEWS-014.md) |
| [STORY-NEWS-002/AC-004](../userstory/STORY-NEWS-002.md#ac-004) | [ST-NEWS-015](../systemtest/ST-NEWS-015.md) |
| [STORY-NEWS-002/AC-005](../userstory/STORY-NEWS-002.md#ac-005) | [ST-NEWS-016](../systemtest/ST-NEWS-016.md) |
| [STORY-NEWS-002/AC-006](../userstory/STORY-NEWS-002.md#ac-006) | [ST-NEWS-017](../systemtest/ST-NEWS-017.md), [ST-NEWS-018](../systemtest/ST-NEWS-018.md) |
| [STORY-NEWS-002/AC-007](../userstory/STORY-NEWS-002.md#ac-007) | [ST-NEWS-019](../systemtest/ST-NEWS-019.md) |
| [STORY-NEWS-002/AC-008](../userstory/STORY-NEWS-002.md#ac-008) | [ST-NEWS-028](../systemtest/ST-NEWS-028.md) |
| [STORY-NEWS-003/AC-001](../userstory/STORY-NEWS-003.md#ac-001) | [ST-NEWS-020](../systemtest/ST-NEWS-020.md) |
| [STORY-NEWS-003/AC-002](../userstory/STORY-NEWS-003.md#ac-002) | [ST-NEWS-021](../systemtest/ST-NEWS-021.md), [ST-NEWS-024](../systemtest/ST-NEWS-024.md) |
| [STORY-NEWS-003/AC-003](../userstory/STORY-NEWS-003.md#ac-003) | [ST-NEWS-022](../systemtest/ST-NEWS-022.md), [ST-NEWS-023](../systemtest/ST-NEWS-023.md), [ST-NEWS-024](../systemtest/ST-NEWS-024.md), [ST-NEWS-029](../systemtest/ST-NEWS-029.md) |
| [STORY-NEWS-003/AC-004](../userstory/STORY-NEWS-003.md#ac-004) | [ST-NEWS-025](../systemtest/ST-NEWS-025.md) |
| [STORY-NEWS-003/AC-005](../userstory/STORY-NEWS-003.md#ac-005) | [ST-NEWS-026](../systemtest/ST-NEWS-026.md) |
| [STORY-NEWS-001/AC-010](../userstory/STORY-NEWS-001.md#ac-010) | [ST-NEWS-031](../systemtest/ST-NEWS-031.md) |

## Luồng và ngoại lệ

- STORY-NEWS-001/ALT-01: ST-NEWS-001.
- STORY-NEWS-001/EXC-02: ST-NEWS-003, ST-NEWS-007, ST-NEWS-008.
- STORY-NEWS-001/EXC-03: ST-NEWS-005.
- STORY-NEWS-001/EXC-04: ST-NEWS-030.
- STORY-NEWS-001/EXC-05: ST-NEWS-031.
- STORY-NEWS-001/ALT-02: ST-NEWS-009.
- STORY-NEWS-001/ALT-03: ST-NEWS-010.
- STORY-NEWS-001/EXC-01: ST-NEWS-011.
- STORY-NEWS-002/EXC-01: ST-NEWS-013, ST-NEWS-014, ST-NEWS-016, ST-NEWS-028.
- STORY-NEWS-002/ALT-01: ST-NEWS-015.
- STORY-NEWS-002/EXC-02: ST-NEWS-017.
- STORY-NEWS-002/ALT-02: ST-NEWS-018.
- STORY-NEWS-002/EXC-03: ST-NEWS-019.
- STORY-NEWS-003/ALT-01: ST-NEWS-022.
- STORY-NEWS-003/ALT-02: ST-NEWS-023, ST-NEWS-029.
- STORY-NEWS-003/EXC-01: ST-NEWS-026.
- STORY-NEWS-001/Non-Functional: ST-NEWS-027.
- STORY-NEWS-002/Non-Functional: ST-NEWS-027.
- STORY-NEWS-003/Non-Functional: ST-NEWS-027.

## Điều kiện trước khi chạy

- Cần môi trường có triển khai Tin tức, cloud thử nghiệm, tài khoản đủ/thiếu quyền và dữ liệu bài/danh mục độc lập. Backend đã có ở nhánh `feature/news`; System Test chưa chạy.
- API, định dạng lưu rich text, cách cấp quyền upload cloud, xử lý ảnh không còn được dùng và kiểm soát nội dung nguy hiểm thuộc bước TDD; không tự chốt nhà cung cấp, hạn upload hoặc định dạng lưu ở nghiệp vụ.
- Ca nhiều cấp dùng 13 cấp để phát hiện giới hạn thấp; một bộ dữ liệu hữu hạn không chứng minh khả năng xử lý vô hạn hoặc hiệu năng. Không đặt ngưỡng hiệu năng khi chưa có căn cứ.
- Đối chiếu lượt cần quan sát dữ liệu gói trước/sau; không chỉ dựa vào việc giao diện không hiện thông báo trừ lượt.
- Owner kiểm thử, phân công triển khai, Sprint và ngày hiệu lực BR chưa xác định. Reviewer/Approver tiếp tục là Tân Trần theo hội thoại; chưa gán tài khoản hoặc phê duyệt trên hệ thống.
- Không gồm video, tệp đính kèm, ghim tin, bình luận, thích, yêu thích, thống kê lượt đọc, lịch sử xem hoặc cơ chế phiên bản LIB.

## Thiết kế kỹ thuật

- [TDD-NEWS-001](../tdd/TDD-NEWS-001.md): bài viết, rich text, URL ảnh thuộc kho presign, đọc công khai và bảng liên kết.
- [TDD-NEWS-002](../tdd/TDD-NEWS-002.md): cây danh mục, thứ tự, chuyển nhánh, chống vòng lặp và khóa chung với bài.
- Quyết định đã xác nhận ngày 26/09/2026: frontend xin URL upload từ dịch vụ presign nằm ngoài backend, tự upload, nhận URL cố định rồi gửi URL cho backend. Backend không nhận bytes, không tạo presigned URL; chỉ nhận ảnh bìa và `src` của ảnh trong rich text khi là URL https thuộc `UploadedFileOption__AllowedHosts`. Môi trường chạy ST-NEWS-004, ST-NEWS-005 phải khai báo tên miền kho thử nghiệm trong cấu hình này.
- Hai TDD đã được người dùng chốt trong hội thoại; đặc tả Unit Test xem [bảng độ phủ](news-unit-test-coverage.md). Chưa triển khai hoặc chạy kiểm thử.
- Tên danh mục tối đa 200 ký tự Unicode sau chuẩn hóa: ban đầu chốt cùng TDD, từ 25/09/2026 là quy tắc nghiệp vụ tại BR-NEWS-002 khoản 1. Không giới hạn số cấp.
- Backend đã triển khai ở nhánh `feature/news` của `bmt-be` (commit `4714e68`, `bce2eb2`, `e390e2d`), chưa merge. Chưa chạy System Test nào.
