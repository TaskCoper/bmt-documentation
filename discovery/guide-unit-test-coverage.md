# Phạm vi Unit Test cho video hướng dẫn

Đã soạn 44 đặc tả Unit Test theo TDD-GUIDE-001 tại commit f9d659d, sau khi người dùng trả lời “ok chốt”. Tên unit là vai trò dự kiến, chưa phải lớp/hàm GUIDE đã tồn tại. Các đặc tả có Status Draft theo mẫu; chưa viết mã hoặc chạy test.

## Bảng đối chiếu

| Đặc tả | Unit dự kiến | Căn cứ |
|---|---|---|
| [UT-GUIDE-001](../unittest/UT-GUIDE-001.md) | Chuẩn hóa nội dung (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-002/Then |
| [UT-GUIDE-002](../unittest/UT-GUIDE-002.md) | Validator độ dài (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-002/Then |
| [UT-GUIDE-003](../unittest/UT-GUIDE-003.md) | Validator lưu nháp và xuất bản (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-004](../unittest/UT-GUIDE-004.md) | Bộ phân tích URL YouTube (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-005](../unittest/UT-GUIDE-005.md) | Bộ phân tích URL YouTube (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-006](../unittest/UT-GUIDE-006.md) | Validator phiên bản (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-007](../unittest/UT-GUIDE-007.md) | Chuẩn hóa phân trang (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-008](../unittest/UT-GUIDE-008.md) | Tạo mẫu tìm kiếm literal (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-009](../unittest/UT-GUIDE-009.md) | YoutubeVideoClient ánh xạ metadata (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-010](../unittest/UT-GUIDE-010.md) | YoutubeVideoClient chọn ảnh (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-011](../unittest/UT-GUIDE-011.md) | YoutubeVideoClient kiểm loại video (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-012](../unittest/UT-GUIDE-012.md) | YoutubeVideoClient kiểm video khả dụng (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-013](../unittest/UT-GUIDE-013.md) | YoutubeVideoClient lỗi dịch vụ (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-014](../unittest/UT-GUIDE-014.md) | YoutubeVideoClient tạo HTTP request (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-015](../unittest/UT-GUIDE-015.md) | GuideVideoMetadataReader cache hit (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-002/Then |
| [UT-GUIDE-016](../unittest/UT-GUIDE-016.md) | GuideVideoMetadataReader cache miss (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-002/Then |
| [UT-GUIDE-017](../unittest/UT-GUIDE-017.md) | GuideVideoMetadataReader đổi video (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-018](../unittest/UT-GUIDE-018.md) | GuideVideoMetadataReader cache lỗi (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-019](../unittest/UT-GUIDE-019.md) | GuideVideoMetadataReader chia batch (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-020](../unittest/UT-GUIDE-020.md) | Handler tạo Guide (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-001/Then |
| [UT-GUIDE-021](../unittest/UT-GUIDE-021.md) | Handler lưu Draft/Hidden khi YouTube lỗi (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-022](../unittest/UT-GUIDE-022.md) | Handler lưu video chắc chắn không phù hợp (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-023](../unittest/UT-GUIDE-023.md) | Handler xuất bản (dự kiến theo TDD) | TDD-GUIDE-001/State Diagram<br>BR-GUIDE-001/Then |
| [UT-GUIDE-024](../unittest/UT-GUIDE-024.md) | Handler xuất bản bỏ qua kết quả preview (dự kiến theo TDD) | TDD-GUIDE-001/Sequence Diagram<br>BR-GUIDE-002/Then |
| [UT-GUIDE-025](../unittest/UT-GUIDE-025.md) | Handler xuất bản khi dịch vụ lỗi (dự kiến theo TDD) | TDD-GUIDE-001/Sequence Diagram<br>BR-GUIDE-002/Then |
| [UT-GUIDE-026](../unittest/UT-GUIDE-026.md) | Handler xuất bản gặp sửa đồng thời (dự kiến theo TDD) | TDD-GUIDE-001/Sequence Diagram<br>BR-GUIDE-001/Then |
| [UT-GUIDE-027](../unittest/UT-GUIDE-027.md) | Handler sửa Published không thay video (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-028](../unittest/UT-GUIDE-028.md) | Handler sửa Published thay video đạt (dự kiến theo TDD) | TDD-GUIDE-001/Architecture<br>BR-GUIDE-002/Then |
| [UT-GUIDE-029](../unittest/UT-GUIDE-029.md) | Handler sửa Published thay video lỗi (dự kiến theo TDD) | TDD-GUIDE-001/Architecture<br>BR-GUIDE-002/Then |
| [UT-GUIDE-030](../unittest/UT-GUIDE-030.md) | Handler PUT thay toàn bộ ba trường (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-031](../unittest/UT-GUIDE-031.md) | Handler ẩn hướng dẫn (dự kiến theo TDD) | TDD-GUIDE-001/State Diagram<br>BR-GUIDE-001/Then |
| [UT-GUIDE-032](../unittest/UT-GUIDE-032.md) | Handler lệnh ghi với phiên bản cũ (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-033](../unittest/UT-GUIDE-033.md) | Handler xóa hướng dẫn (dự kiến theo TDD) | TDD-GUIDE-001/State Diagram<br>BR-GUIDE-001/Then |
| [UT-GUIDE-034](../unittest/UT-GUIDE-034.md) | Handler đọc hoặc ghi Guide không tồn tại (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-035](../unittest/UT-GUIDE-035.md) | Handler di chuyển kiểm đầu vào (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-036](../unittest/UT-GUIDE-036.md) | Thuật toán di chuyển (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-003/Then |
| [UT-GUIDE-037](../unittest/UT-GUIDE-037.md) | Thuật toán đưa xuống cuối và no-op (dự kiến theo TDD) | TDD-GUIDE-001/Data Model<br>BR-GUIDE-003/Then |
| [UT-GUIDE-038](../unittest/UT-GUIDE-038.md) | Handler xuất bản đã Published (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-001/Then |
| [UT-GUIDE-039](../unittest/UT-GUIDE-039.md) | Handler đọc công khai không thấy Guide (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-040](../unittest/UT-GUIDE-040.md) | Ánh xạ DTO công khai (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-041](../unittest/UT-GUIDE-041.md) | Handler danh sách công khai thiếu metadata (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-003/Then |
| [UT-GUIDE-042](../unittest/UT-GUIDE-042.md) | Preview video không ghi Guide (dự kiến theo TDD) | TDD-GUIDE-001/Internal API<br>BR-GUIDE-002/Then |
| [UT-GUIDE-043](../unittest/UT-GUIDE-043.md) | Điều phối preview ở frontend (dự kiến theo TDD) | TDD-GUIDE-001/Architecture<br>BR-GUIDE-002/Then |
| [UT-GUIDE-044](../unittest/UT-GUIDE-044.md) | Điều khiển player ở frontend (dự kiến theo TDD) | TDD-GUIDE-001/External API<br>BR-GUIDE-002/Then |

## Ranh giới kiểm chứng

- UT-GUIDE-001 đến UT-GUIDE-008: chuẩn hóa, giới hạn, URL, phiên bản và đầu vào truy vấn.
- UT-GUIDE-009 đến UT-GUIDE-019: ánh xạ YouTube, loại video, lỗi provider, TTL và batch metadata.
- UT-GUIDE-020 đến UT-GUIDE-038: tạo/sửa/xuất bản/ẩn/xóa, concurrency guard và thuật toán thứ tự bằng cổng dữ liệu giả.
- UT-GUIDE-039 đến UT-GUIDE-044: kết quả công khai, preview và điều phối giao diện dự kiến.

PostgreSQL CHECK/FK, lọc SQL, snapshot, khóa và rollback cần ST-GUIDE-035 đến ST-GUIDE-041; mock không chứng minh các tính chất đó. Cache/API/giao diện khi metadata hết hạn nằm ở ST-GUIDE-042; policy và CSRF nằm ở ST-GUIDE-043, bên cạnh ST-GUIDE-002/003. Video/Shorts thật vẫn cần ST-GUIDE-024/033. Xem [bảng System Test](guide-system-test-coverage.md).

## Kiểm tra tài liệu và phần còn thiếu

Đối chiếu cấu trúc template, 13 cột, một dòng dữ liệu, mã duy nhất, Reviewer/Approver, Trace to và TEST_LINKS cùng section thật. Đây là kiểm tra đặc tả, không phải kết quả Pass của ứng dụng.

Giữ các giới hạn khảo sát đã công bố trong bảng System Test: chưa đọc hết chuỗi tài liệu phụ thuộc liên module, chưa có repository frontend và chưa kiểm YouTube credentials/quota. Author/Owner và các thông tin phân công chưa được cung cấp; không tự gán. Việc chốt TDD đã được ghi nhận, không cần xin chốt lại cùng bản thiết kế.
