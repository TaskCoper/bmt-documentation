# Đặc tả Unit Test Tin tức

Người dùng đã chốt TDD-NEWS-001 và TDD-NEWS-002, gồm giới hạn tên danh mục 200 ký tự Unicode; số cấp không giới hạn nghiệp vụ. Từ 25/09/2026, giới hạn tên còn là quy tắc nghiệp vụ tại BR-NEWS-002 khoản 1, nên UT-NEWS-025 dẫn thêm tới BR-NEWS-002 và STORY-NEWS-002/AC-008. UT-NEWS-022 kiểm thêm vai trò tùy chỉnh có `news.manage`; UT-NEWS-040 mới kiểm lọc theo danh mục không tồn tại. Bộ dưới đây là đặc tả cho các unit dự kiến, chưa có mã test hoặc kết quả thực thi. Tên hàm cụ thể được ánh xạ khi triển khai; không thay đổi contract đã chốt.

## Danh sách đặc tả

| Đặc tả | Phạm vi |
| --- | --- |
| [UT-NEWS-001](../unittest/UT-NEWS-001.md) | NewsArticleService — lưu nháp |
| [UT-NEWS-002](../unittest/UT-NEWS-002.md) | NewsArticleService — kiểm đủ dữ liệu công bố |
| [UT-NEWS-003](../unittest/UT-NEWS-003.md) | NewsArticleService — công bố lần đầu |
| [UT-NEWS-004](../unittest/UT-NEWS-004.md) | NewsArticleService — công bố lại |
| [UT-NEWS-005](../unittest/UT-NEWS-005.md) | NewsArticleService — sửa Published |
| [UT-NEWS-006](../unittest/UT-NEWS-006.md) | NewsArticleService — chặn sửa Published thiếu trường |
| [UT-NEWS-007](../unittest/UT-NEWS-007.md) | NewsArticleService — danh mục đã mất |
| [UT-NEWS-008](../unittest/UT-NEWS-008.md) | NewsArticleService — xung đột phiên bản |
| [UT-NEWS-009](../unittest/UT-NEWS-009.md) | NewsArticleService — ẩn bài |
| [UT-NEWS-010](../unittest/UT-NEWS-010.md) | NewsArticleService — publish bài đang Published |
| [UT-NEWS-011](../unittest/UT-NEWS-011.md) | NewsArticleService — xóa mọi trạng thái |
| [UT-NEWS-012](../unittest/UT-NEWS-012.md) | NewsArticleService — bài không tồn tại |
| [UT-NEWS-013](../unittest/UT-NEWS-013.md) | INewsHtmlSanitizer — định dạng hợp lệ |
| [UT-NEWS-014](../unittest/UT-NEWS-014.md) | INewsHtmlSanitizer — chặn nội dung nguy hiểm |
| [UT-NEWS-015](../unittest/UT-NEWS-015.md) | INewsHtmlSanitizer — URL đúng origin và scheme |
| [UT-NEWS-016](../unittest/UT-NEWS-016.md) | NewsArticleService — nội dung có nghĩa sau làm sạch |
| [UT-NEWS-017](../unittest/UT-NEWS-017.md) | INewsHtmlSanitizer — tính ổn định khi làm sạch lại |
| [UT-NEWS-018](../unittest/UT-NEWS-018.md) | NewsArticleService — lỗi kiểm URL ảnh |
| [UT-NEWS-019](../unittest/UT-NEWS-019.md) | Cổng presign dự kiến — ràng buộc ticket |
| [UT-NEWS-020](../unittest/UT-NEWS-020.md) | Cổng complete dự kiến — kết quả bền vững |
| [UT-NEWS-021](../unittest/UT-NEWS-021.md) | ArticleWrite validator — snapshot và trường server |
| [UT-NEWS-022](../unittest/UT-NEWS-022.md) | Policy news.manage dự kiến — dùng chung quyền |
| [UT-NEWS-023](../unittest/UT-NEWS-023.md) | NewsCategoryNamePolicy — chuẩn hóa giữ dấu |
| [UT-NEWS-024](../unittest/UT-NEWS-024.md) | NewsCategoryNamePolicy — tên rỗng |
| [UT-NEWS-025](../unittest/UT-NEWS-025.md) | NewsCategoryNamePolicy — giới hạn Unicode 200 |
| [UT-NEWS-026](../unittest/UT-NEWS-026.md) | NewsCategoryService — kiểm tên cùng cha |
| [UT-NEWS-027](../unittest/UT-NEWS-027.md) | NewsCategoryService — chuyển vào chính node/hậu duệ |
| [UT-NEWS-028](../unittest/UT-NEWS-028.md) | NewsCategoryService — chuyển hợp lệ và về gốc |
| [UT-NEWS-029](../unittest/UT-NEWS-029.md) | NewsCategoryService — trùng tại cha đích |
| [UT-NEWS-030](../unittest/UT-NEWS-030.md) | NewsCategoryService — node/cha không tồn tại |
| [UT-NEWS-031](../unittest/UT-NEWS-031.md) | NewsCategoryService — xóa danh mục đang dùng |
| [UT-NEWS-032](../unittest/UT-NEWS-032.md) | NewsCategoryService — đổi vị trí |
| [UT-NEWS-033](../unittest/UT-NEWS-033.md) | NewsCategoryService — anchor hoặc version cũ |
| [UT-NEWS-034](../unittest/UT-NEWS-034.md) | Position validator — anchor không hợp lệ |
| [UT-NEWS-035](../unittest/UT-NEWS-035.md) | NewsCategoryService — thêm cấp sâu |
| [UT-NEWS-036](../unittest/UT-NEWS-036.md) | Chuẩn hóa tham số phân trang Tin tức |
| [UT-NEWS-037](../unittest/UT-NEWS-037.md) | Chuẩn bị từ khóa ILIKE |
| [UT-NEWS-038](../unittest/UT-NEWS-038.md) | Handler đọc public dự kiến — điều kiện trạng thái |
| [UT-NEWS-039](../unittest/UT-NEWS-039.md) | Error mapping Tin tức — mã lỗi rõ ràng |
| [UT-NEWS-040](../unittest/UT-NEWS-040.md) | Truy vấn danh sách công khai — categoryId không tồn tại trả rỗng |

## Phần phải kiểm chứng ngoài unit test

| Phần | Cách kiểm chứng và căn cứ |
| --- | --- |
| UNIQUE tên cùng cha, NULL gốc, FK và cascade | PostgreSQL 15 thật; ST-NEWS-013, ST-NEWS-016–018 và TDD-NEWS-002/Data Model. Mock không chứng minh constraint. |
| Khóa cây và bài, rollback, cạnh tranh chuyển nhánh | Hai connection thật, kiểm cả kết quả và dữ liệu cuối; thử xóa Category đồng thời gắn Article, A→B đồng thời B→A, ghi links thất bại. Theo hai TDD/Architecture. |
| CTE toàn nhánh, EXISTS loại trùng, thứ tự và snapshot phân trang | Integration SQL và ST-NEWS-021–025, ST-NEWS-029; không dùng LINQ-to-objects thay SQL để tuyên bố đạt. |
| Quyền và CSRF ở route thực, session, AllowAnonymous | API integration và ST-NEWS-011, ST-NEWS-019–020, ST-NEWS-026; policy unit không chứng minh middleware được gắn. |
| Presign, CORS, signed headers, URL hết hạn, ảnh final bất biến | Storage contract test trên môi trường thử của dự án và ST-NEWS-004–005; thử finalize đồng thời và staging bị đổi giữa xác minh/copy. |
| Rich text không thực thi mã và round-trip editor | Parser unit kiểm DOM; trình duyệt thật kiểm ST-NEWS-027, cả trang quản trị và khách. |
| Xóa/ẩn phản ánh trên trang khách và không trừ lượt | ST-NEWS-009–010, ST-NEWS-020, ST-NEWS-026; kiểm response mới, cache và dữ liệu gói trước/sau. |

## Căn cứ và phần còn mở

- [TDD bài viết](../tdd/TDD-NEWS-001.md), [TDD danh mục](../tdd/TDD-NEWS-002.md) và [29 System Test](news-system-test-coverage.md) là nguồn thiết kế/nghiệp vụ đã chốt.
- Trace to và TEST_LINKS của từng UT dẫn cùng tập mã/section; phần không kiểm được bằng unit đã nêu rõ, không tính là kết quả Pass.
- Presign dùng cơ chế của dự án; route/provider thực và source gateway chưa có trong checkout đã khảo sát. Chưa chốt package sanitizer; cần kiểm khả năng .NET 8 và allowlist trước triển khai.
- Owner kiểm thử chưa phân công; Reviewer/Approver là Tân Trần theo hội thoại. Không tự cập nhật trạng thái phê duyệt hoặc tạo lịch sử phát hành.
