# Kế hoạch tính năng video hướng dẫn

## Trạng thái

Đang làm rõ nghiệp vụ theo yêu cầu ngày 30/09/2026. Đã có hai User Story và ba Business Rule ở dạng bản nháp; còn các điểm cần chốt bổ sung và thông tin người rà soát/phê duyệt. Các quyết định dưới đây được ghi theo câu trả lời trong hội thoại; chưa được chốt toàn văn bộ US/BR. Chưa viết đặc tả System Test, TDD hoặc Unit Test. Chưa sửa mã ứng dụng, chạy migration hay triển khai.

## Bộ tài liệu đang soạn

| Nội dung | Tài liệu |
|---|---|
| Admin quản lý, xuất bản, ẩn, xóa và sắp xếp hướng dẫn | [STORY-GUIDE-001](../userstory/STORY-GUIDE-001.md) |
| Người dùng xem danh sách và phát video công khai | [STORY-GUIDE-002](../userstory/STORY-GUIDE-002.md) |
| Quyền và vòng đời hướng dẫn | [BR-GUIDE-001](../businessrule/BR-GUIDE-001.md) |
| Nội dung và tích hợp YouTube | [BR-GUIDE-002](../businessrule/BR-GUIDE-002.md) |
| Hiển thị công khai và thứ tự | [BR-GUIDE-003](../businessrule/BR-GUIDE-003.md) |

## Đã xác nhận

1. Admin quản lý hướng dẫn; video lưu trên YouTube và được gán vào hướng dẫn bằng link.
2. Mọi người được xem hướng dẫn đã xuất bản, kể cả khách chưa đăng nhập.
3. Hướng dẫn có ba trạng thái: Nháp, Xuất bản, Ẩn. Admin tạo bản nháp trước rồi chủ động xuất bản; có thể ẩn và xuất bản lại.
4. Không chia danh mục.
5. Mỗi hướng dẫn gồm tiêu đề, mô tả ngắn và một link YouTube. Người dùng xem video ngay trong trang; chưa có bài viết hướng dẫn kèm theo.
6. Sửa hướng dẫn đang xuất bản có hiệu lực ngay sau khi lưu. Admin có thể ẩn trước nếu cần chuẩn bị nội dung.
7. Chỉ được xóa hẳn hướng dẫn đang nháp hoặc đã ẩn; hướng dẫn đang xuất bản phải được ẩn trước khi xóa.
8. Hệ thống tự lấy ảnh đại diện và thời lượng từ YouTube sau khi admin dán link.
9. Admin chủ động sắp xếp thứ tự hướng dẫn.
10. Người được cấp quyền Quản lý hướng dẫn trong hệ thống phân quyền hiện có được thực hiện các thao tác quản trị.
11. Cho lưu nháp khi chưa nhập đủ thông tin. Khi xuất bản phải có tiêu đề, mô tả ngắn và link video YouTube hợp lệ.
12. Nếu không kiểm tra được video do YouTube lỗi, video không tồn tại hoặc không cho phát trên website, từ chối xuất bản và giữ nguyên trạng thái. Với hướng dẫn đã xuất bản nhưng video phát lỗi, thông báo cho người xem; admin chủ động sửa hoặc ẩn, hệ thống không tự ẩn.
13. Quản lý một bản nội dung tiếng Việt trong đợt này.

## Đề xuất chưa chốt

- Giữ ô tìm kiếm; tìm trong tiêu đề và mô tả ngắn, kết quả vẫn theo thứ tự quản trị.
- Khi YouTube tạm thời lỗi, cho lưu link đúng định dạng vào Nháp và thử lấy thông tin lại sau. Hướng dẫn đang xuất bản chỉ lưu sửa khi còn đủ trường bắt buộc và video mới đã được kiểm tra; nếu thất bại thì giữ bản đang lưu.

## Cần làm rõ

- Phạm vi tìm kiếm và quy tắc lưu khi không kiểm tra được video: đang chờ câu trả lời.
- Giới hạn độ dài nội dung và dạng link hỗ trợ sẽ được nêu trong thiết kế, chưa tự đặt ngưỡng nghiệp vụ hoặc giới hạn thời lượng video từ dòng giới thiệu trên giao diện.
- Reviewer, Approver và các thông tin người phụ trách tài liệu chưa được cung cấp cho tính năng này.

## Hiện trạng đã kiểm tra

- [Trang hướng dẫn](https://vnz-bmt-savico-abcxyz.vercel.app/vi/guide) hiển thị cho khách: ô tìm kiếm và sáu mục video kèm tiêu đề, thời lượng. Khi mở mục “Chụp ảnh lô đất đúng cách”, cửa sổ hiển thị mô tả và thông báo “Video đang được biên tập”. Đây là quan sát giao diện, chưa chứng minh nguồn dữ liệu hoặc luồng phát video thực tế.
- Workspace hiện có backend `bmt-be` và tài liệu `bmt-documentation`; chưa có mã nguồn frontend trong workspace để xác định file cần sửa.
- Tìm trong backend và tài liệu chưa thấy module quản lý video hướng dẫn. Không dùng ví dụ hoặc tên trùng trong template làm yêu cầu nghiệp vụ.
- [NewsArticleApi.cs](../../bmt-be/src/bmt-be.presentation/apis/news/NewsArticleApi.cs) có cách tách API công khai và API quản trị, kèm kiểm quyền. Đây là nguồn tham khảo kiến trúc; không sao chép quy tắc xóa tin tức vì quy tắc video hướng dẫn đã chốt khác.
- [PermissionNames.cs](../../bmt-be/src/bmt-be.contract/constants/PermissionNames.cs) khai báo các mã quyền của hệ thống; chưa có quyền quản lý hướng dẫn.
- [NewsArticle.cs](../../bmt-be/src/bmt-be.domain/entities/NewsArticle.cs) có trạng thái và trường chống ghi đè khi nhiều người sửa. Việc tái sử dụng cách xử lý cần được mô tả trong thiết kế sau khi nghiệp vụ đủ rõ.
- [ApplicationDbContext.cs](../../bmt-be/src/bmt-be.persistence/ApplicationDbContext.cs) hiện chứa dữ liệu phân quyền, tin tức và nhiều module nghiệp vụ; không chỉ có User như một số ghi chú cũ. Chưa có tập dữ liệu video hướng dẫn.
- Hướng dẫn backend có một số mô tả lịch sử khác code hiện tại. Dùng `bmt-documentation/` làm nguồn tài liệu theo hướng dẫn workspace và kiểm tra code thực tế trước khi thiết kế.

## Căn cứ tích hợp YouTube

[YouTube Data API — Videos](https://developers.google.com/youtube/v3/docs/videos) mô tả ảnh đại diện, thời lượng và trạng thái video; [videos.list](https://developers.google.com/youtube/v3/docs/videos/list) cho phép lấy thông tin bằng mã video. [IFrame Player API](https://developers.google.com/youtube/iframe_api_reference) là nguồn tham khảo cho phần phát video và nhận lỗi. Chưa gọi API bằng thông tin xác thực của dự án hoặc kiểm chứng với video thực tế.

## Thứ tự công việc

1. Hoàn tất các quyết định nghiệp vụ còn mở, soạn hai User Story cho quản trị và xem hướng dẫn, cùng các Business Rule theo template.
2. Bàn giao toàn văn US/BR để người dùng chốt, sau đó mới viết đặc tả System Test theo quy trình dự án.
3. Thiết kế lưu trữ, quyền, API, tích hợp YouTube và giao diện; giải thích lựa chọn bằng dữ liệu mẫu và các luồng thành công/thất bại. Đọc đủ phụ thuộc RBAC trước khi chốt phần quyền; việc khảo sát hiện tại chưa hoàn thành toàn bộ chuỗi tham chiếu.
4. Bàn giao TDD; sau khi được chốt mới viết đặc tả Unit Test. Bước này chỉ lập kế hoạch và tài liệu, chưa triển khai ứng dụng.

## Phần dự kiến thay đổi

| Phần | Nội dung dự kiến | Căn cứ và điều kiện |
|---|---|---|
| Admin | Danh sách mọi trạng thái, form tiêu đề/mô tả/link, ảnh và thời lượng tự lấy, các thao tác xuất bản/ẩn/xóa/sắp xếp | STORY-GUIDE-001; chưa có mã frontend trong workspace để xác định file |
| Trang hướng dẫn | Danh sách lấy từ dữ liệu đã xuất bản, trình phát YouTube, danh sách trống và thông báo video lỗi | STORY-GUIDE-002; tìm kiếm đang chờ chốt |
| Backend | Thêm module hướng dẫn theo cấu trúc contract, handler, endpoint, dữ liệu và kiểm quyền hiện có | Chưa tạo lớp, API hoặc migration; thiết kế chi tiết sau khi bộ nghiệp vụ được chốt |
| YouTube | Lấy thông tin theo video được gán; phân biệt lỗi kiểm tra trước xuất bản với lỗi phát thực tế | BR-GUIDE-002; cần cấu hình tích hợp, không cần nhận tệp video vào BMT |
| Phân quyền | Bổ sung quyền Quản lý hướng dẫn vào danh mục quyền và dùng cho các thao tác quản trị | BR-GUIDE-001; đọc đủ phụ thuộc RBAC trước khi chốt cách bổ sung |
| Kiểm thử | Đối chiếu luồng quản trị và công khai với AC, kiểm tích hợp YouTube và ràng buộc dữ liệu thực tế | Chưa viết đặc tả ST/UT hoặc chạy kiểm thử |

## Phạm vi kiểm chứng dự kiến

Kiểm chứng luồng admin tạo nháp, gán video, xuất bản, sửa, ẩn và xóa; quyền của người quản lý; danh sách công khai chỉ trả hướng dẫn đang xuất bản; phát video và xử lý lỗi YouTube; ảnh đại diện, thời lượng lấy đúng video. Bổ sung kiểm chứng tìm kiếm, sắp xếp và cập nhật đồng thời theo thiết kế được chốt. Đây là phạm vi dự kiến, chưa phải các ca kiểm thử đã soạn hoặc kết quả đã chạy.
