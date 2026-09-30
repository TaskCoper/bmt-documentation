# Kế hoạch tính năng video hướng dẫn

Đợt triển khai hiện tại chỉ làm backend, theo câu trả lời “chỉ backend” của người dùng. Đã có module, migration và kiểm thử trên nhánh `feature/video-guides`; xem [bàn giao backend](guide-backend-implementation.md). Các mục “hiện trạng” và “dự kiến” bên dưới ghi lại thời điểm lập kế hoạch ban đầu, không phải trạng thái triển khai mới nhất.


## Trạng thái

Đã cập nhật hai User Story và ba Business Rule theo các câu trả lời trong hội thoại chuẩn bị tính năng ngày 30/09/2026. Người dùng đã xác nhận các quyết định nghiệp vụ bên dưới; Reviewer và Approver đều là Tân Trần. Người dùng đã xác nhận “chốt” bộ US/BR sau cập nhật; được chuyển sang viết System Test và thiết kế TDD. Creator, Assignee, Owner, Sprint và ngày hiệu lực chưa được cung cấp; không tự gán. Đã soạn 43 đặc tả System Test và 44 đặc tả Unit Test. Người dùng đã chốt TDD-GUIDE-001 tại commit f9d659d bằng câu “ok chốt”; xác nhận này là căn cứ viết Unit Test. Phần rà soát toàn bộ chuỗi phụ thuộc liên module chưa hoàn tất và không được coi là đã hoàn tất chỉ vì TDD được chốt. Chưa sửa mã ứng dụng, chạy migration hay triển khai.

## Bộ tài liệu đang soạn

| Nội dung | Tài liệu |
|---|---|
| Admin quản lý, xuất bản, ẩn, xóa và sắp xếp hướng dẫn | [STORY-GUIDE-001](../userstory/STORY-GUIDE-001.md) |
| Người dùng xem danh sách và phát video công khai | [STORY-GUIDE-002](../userstory/STORY-GUIDE-002.md) |
| Quyền và vòng đời hướng dẫn | [BR-GUIDE-001](../businessrule/BR-GUIDE-001.md) |
| Nội dung và tích hợp YouTube | [BR-GUIDE-002](../businessrule/BR-GUIDE-002.md) |
| Hiển thị công khai và thứ tự | [BR-GUIDE-003](../businessrule/BR-GUIDE-003.md) |
| Thiết kế kỹ thuật đã chốt trong hội thoại | [TDD-GUIDE-001](../tdd/TDD-GUIDE-001.md) |
| 43 đặc tả System Test và bảng phủ AC | [Bảng đối chiếu](guide-system-test-coverage.md) |
| 44 đặc tả Unit Test | [Bảng phủ Unit Test](guide-unit-test-coverage.md) |

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
14. Giữ ô tìm kiếm, tìm trong cả tiêu đề và mô tả ngắn. Kết quả chỉ gồm hướng dẫn đang xuất bản, theo thứ tự admin đã chọn.
15. Khi YouTube tạm thời lỗi, cho lưu link đúng định dạng vào bản Nháp và thử lấy thông tin lại sau. Hướng dẫn đang xuất bản chỉ lưu sửa khi còn đủ trường bắt buộc và, nếu thay video, video mới đã được kiểm tra thành công. Nếu không đạt, từ chối lưu, báo lỗi và giữ nguyên toàn bộ bản đang hiển thị.
16. Reviewer và Approver: Tân Trần. Việc điền tên không tự xác nhận tài liệu đã được phê duyệt.

17. Tiêu đề tối đa 200 ký tự, mô tả ngắn tối đa 2.000 ký tự; vượt giới hạn báo lỗi, không tự cắt.
18. Chỉ nhận video đã đăng, gồm Shorts; chưa nhận livestream đang diễn ra hoặc sắp phát.

## Đề xuất chưa chốt

Không còn đề xuất kỹ thuật GUIDE chờ xác nhận trong bản TDD đã bàn giao. Hai bảng Guide/GuideOrderState, quyền guide.manage, chống ghi đè bằng phiên bản, API quản trị/công khai và cache YouTube 24 giờ thuộc phạm vi TDD người dùng đã chốt. Khi không lấy lại được metadata, vẫn trả nội dung, dùng ảnh thay thế và không hiển thị thời lượng giả theo thiết kế đã chốt.

## Cần làm rõ

- US/BR đã được chốt; hai xác nhận bổ sung về giới hạn nội dung và video đã được cập nhật vào BR-GUIDE-002, STORY-GUIDE-001 và ST-GUIDE-031 đến ST-GUIDE-034.
- TDD đã nêu dạng link, cách đếm ký tự, so khớp từ khóa và vị trí khi xuất bản. Chưa kiểm chứng toàn bộ chuỗi tài liệu phụ thuộc, credentials YouTube hoặc mã frontend; xem giới hạn trong bảng phủ System Test.
- Creator, Assignee, Owner, Sprint và ngày hiệu lực chưa được cung cấp; tài liệu chưa đủ các thông tin quản trị này.

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

1. Đã cập nhật hai User Story và ba Business Rule theo các quyết định được xác nhận; bổ sung luồng và tiêu chí nghiệm thu cho tìm kiếm, lưu nháp khi YouTube lỗi và giữ bản đang hiển thị khi sửa không hợp lệ.
2. Người dùng đã chốt US/BR và TDD; đã viết 43 đặc tả System Test, phủ 26 AC bằng tham chiếu và bổ sung các ràng buộc kỹ thuật. Chưa chạy kiểm thử.
3. Đã ghi nhận TDD được người dùng chốt, giữ nguyên phương án dữ liệu, API và sơ đồ. Cần hoàn tất rà soát chuỗi phụ thuộc trước khi coi phần quyền và tác động liên module đã được kiểm chứng đầy đủ.
4. Đã soạn 44 đặc tả Unit Test theo TDD đã chốt và bảng phủ. Bước này chỉ lập kế hoạch và tài liệu, chưa triển khai ứng dụng.

## Phần dự kiến thay đổi

| Phần | Nội dung dự kiến | Căn cứ và điều kiện |
|---|---|---|
| Admin | Danh sách mọi trạng thái, form tiêu đề/mô tả/link, ảnh và thời lượng tự lấy, các thao tác xuất bản/ẩn/xóa/sắp xếp | STORY-GUIDE-001; chưa có mã frontend trong workspace để xác định file |
| Trang hướng dẫn | Danh sách lấy từ dữ liệu đã xuất bản, tìm trong tiêu đề/mô tả ngắn, trình phát YouTube, danh sách trống và thông báo video lỗi | STORY-GUIDE-002 |
| Backend | Thêm module hướng dẫn theo cấu trúc contract, handler, endpoint, dữ liệu và kiểm quyền hiện có | Thiết kế dự kiến trong TDD-GUIDE-001; chưa tạo lớp, API hoặc migration |
| YouTube | Lấy thông tin theo video được gán; phân biệt lỗi kiểm tra trước xuất bản với lỗi phát thực tế | BR-GUIDE-002; cần cấu hình tích hợp, không cần nhận tệp video vào BMT |
| Phân quyền | Bổ sung quyền Quản lý hướng dẫn vào danh mục quyền và dùng cho các thao tác quản trị | BR-GUIDE-001; đọc đủ phụ thuộc RBAC trước khi chốt cách bổ sung |
| Kiểm thử | Đối chiếu luồng quản trị và công khai với AC, kiểm tích hợp YouTube và ràng buộc dữ liệu thực tế | Có 43 đặc tả ST và 44 đặc tả UT; chưa viết mã hoặc chạy kiểm thử |

## Phạm vi kiểm chứng dự kiến

Kiểm chứng luồng admin tạo nháp, gán video, xuất bản, sửa, ẩn và xóa; quyền của người quản lý; danh sách công khai chỉ trả hướng dẫn đang xuất bản; phát video và xử lý lỗi YouTube; ảnh đại diện, thời lượng lấy đúng video. Bổ sung kiểm chứng tìm kiếm, sắp xếp và cập nhật đồng thời theo thiết kế được chốt. 43 đặc tả System Test, 44 đặc tả Unit Test và giới hạn kiểm chứng nằm trong các bảng phủ tương ứng. Chưa có kết quả chạy.
