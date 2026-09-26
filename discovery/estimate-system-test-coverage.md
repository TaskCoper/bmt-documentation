# System Test cho tính năng Tạo dự toán

Người dùng đã xác nhận “Ok tôi đã chốt US và BR” trong hội thoại. Phạm vi xác nhận gồm STORY-PROJ-001 đến STORY-PROJ-005, BR-PROJ-001 đến BR-PROJ-007 và các quy tắc quyền/gói/lượt được áp dụng trong các Story. Xác nhận này cho phép viết ST; không phải bằng chứng import, publish hay phê duyệt trên hệ thống quản lý tài liệu.

Đã soạn 71 ca ST-PROJ-001 đến ST-PROJ-071, một ca mỗi file theo template System Test. Các ca có liên kết tới đủ 53 AC, 5 Main Flow và 31 nhánh ALT/EXC hiện tại. Đây là mức bao phủ của đặc tả, không phải kết quả chạy hay bằng chứng không còn lỗi.

**Cập nhật 25/09/2026:** người dùng xác nhận chủ sở hữu được đổi tên bản dự toán bất cứ lúc nào (BR-SUB-007 khoản 11), kể cả khi gói hết hạn, hết lượt, toàn bộ lượt còn lại đang bị giữ hoặc AI đang xử lý; tên không phải đầu vào gửi AI. Hồ sơ, link và tệp xuất sau đó dùng tên hiện tại (BR-PROJ-007 khoản 7, STORY-PROJ-003/AC-006). Khi dùng lại link còn hiệu lực mà khách chọn ngày khác, hệ thống giữ ngày cũ và báo rõ theo TDD-PROJ-003. ST-PROJ-061 đến ST-PROJ-070 kiểm các quyết định này. Quyền quản trị danh mục theo STORY-RBAC-001 có mã kỹ thuật `estimate.catalog.manage` trong TDD-RBAC-001.

Reviewer và Approver: Tân Trần theo phân công đã xác nhận. Owner kiểm thử chưa xác định; tất cả ca giữ trạng thái Draft và chưa thực thi. Không sửa mã ứng dụng, chạy migration, gọi AI thật hoặc gửi email cho người nhận thật trong tác vụ này.

## Điều kiện thực thi và giới hạn

- Hiện trạng đã khảo sát: backend mới có nền tảng tài khoản/xác thực, chưa có module Tạo dự toán, AI hoặc quota tương ứng. Các test chỉ thực thi sau khi phần ứng dụng cần thiết tồn tại.
- Dùng tài khoản khách riêng C1/C2 và Admin/nhân viên riêng; dữ liệu tên, địa chỉ, ảnh và quota trong ca chỉ là fixture. Không áp dụng số quota hoặc kiểu cấu hình fixture thành mặc định sản phẩm.
- Kiểm tra backend bằng phản hồi và đọc lại dữ liệu đã lưu, sổ lượt, tác vụ, đầu vào/kết quả và lời gọi dịch vụ. Không chỉ dựa vào toast, nút bị khóa hoặc HTTP thành công. API và cách quan sát dữ liệu được đề xuất trong TDD-PROJ-001 đến TDD-PROJ-003; cần đối chiếu tiếp khi chốt TDD và triển khai.
- Các bước giao diện cần frontend tích hợp hoặc client thử. Client thử chỉ chứng minh phần backend; không được báo đã kiểm chứng hành vi trình duyệt như giữ nội dung chưa lưu, tự thử lại hoặc quét QR nếu chưa chạy qua giao diện tương ứng.
- AI: chờ API, cách nhận kết quả, cấu trúc và danh sách phần bắt buộc, dữ liệu phản hồi mẫu, xác thực và môi trường thử. Có thể kiểm tra lỗi bằng giả lập sau khi hợp đồng thống nhất; kết quả với giả lập phải phân biệt với kiểm chứng AI thật.
- PDF/Excel: chờ quyết định AI trả tệp hay backend xuất từ dữ liệu, cấu trúc tệp và phương án lưu/truy cập. Không cam kết số trang, số bản vẽ hoặc chất lượng chuyên môn chưa có nguồn.
- Email: dùng hộp thư giả lập hoặc môi trường thử; tiếp nhận gửi không đồng nghĩa đã phát/đã đọc. Chính sách chống gửi trùng/thử lại cụ thể còn thuộc TDD.
- TDD đã đề xuất quy đổi 5/10 MB thành byte, cách đếm Unicode, kiểu số diện tích, mã lỗi/API và xử lý đồng thời. Timeout AI chờ tích hợp. Mốc hết hạn cuối ngày Việt Nam đã được người dùng chốt. Test dùng ngưỡng nghiệp vụ đã chốt, không tự đặt giá trị triển khai.
- Chưa đặt test cho xóa/ngừng dùng danh mục, gia hạn link, duyệt chuyên môn hoặc tính năng dự án tương lai vì không thuộc nghiệp vụ đã giao. ST-PROJ-058–060 bổ sung ba quyết định đã xác nhận: tên danh mục tối đa 200 ký tự và cho trùng, hết hạn cuối ngày Việt Nam, tối đa một link đang hiệu lực kể cả khi yêu cầu đồng thời.

## Danh sách ca kiểm thử

| Test | Story | Mục tiêu | Loại | Ưu tiên |
|---|---|---|---|---|
| [ST-PROJ-001](../systemtest/ST-PROJ-001.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Tạo bằng tên, lưu bản nháp thiếu dữ liệu và mở lại | Main | P1 |
| [ST-PROJ-002](../systemtest/ST-PROJ-002.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Biên tên 200 ký tự và bỏ khoảng trắng đầu/cuối | Main / EXC | P1 |
| [ST-PROJ-003](../systemtest/ST-PROJ-003.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Một diện tích chung cho năm loại và loại Admin thêm | Main / ALT | P1 |
| [ST-PROJ-004](../systemtest/ST-PROJ-004.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Kiểm tra diện tích dương và tối đa hai chữ số thập phân | Main / EXC | P1 |
| [ST-PROJ-005](../systemtest/ST-PROJ-005.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Ảnh hoặc mô tả và không có ghi chú riêng | Main / ALT / EXC | P1 |
| [ST-PROJ-006](../systemtest/ST-PROJ-006.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Giới hạn ảnh đầu vào theo định dạng, số lượng và dung lượng | Main / EXC | P1 |
| [ST-PROJ-007](../systemtest/ST-PROJ-007.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Giới hạn 500 ký tự của Mô tả chi tiết | Main / EXC | P1 |
| [ST-PROJ-008](../systemtest/ST-PROJ-008.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Địa chỉ, gói hoàn thiện và hai nhóm phong cách theo cấu hình | Main / ALT / EXC | P1 |
| [ST-PROJ-009](../systemtest/ST-PROJ-009.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Đổi loại giữ lựa chọn hợp lệ và xóa phần không phù hợp | ALT | P1 |
| [ST-PROJ-010](../systemtest/ST-PROJ-010.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Đổi tỉnh xóa xã/phường nhưng giữ địa chỉ chi tiết | ALT | P1 |
| [ST-PROJ-011](../systemtest/ST-PROJ-011.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Thay ảnh lỗi giữ ảnh cũ; thay thành công chỉ còn một ảnh | EXC / ALT | P1 |
| [ST-PROJ-012](../systemtest/ST-PROJ-012.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Mất mạng giữ dữ liệu trên trang và tự lưu lại khi phục hồi | EXC / ALT | P1 |
| [ST-PROJ-013](../systemtest/ST-PROJ-013.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Lỗi máy chủ, thử lại thủ công và tải lại trang | EXC / ALT | P1 |
| [ST-PROJ-014](../systemtest/ST-PROJ-014.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Thử lưu lại kiểm tra quyền và khóa tại thời điểm yêu cầu | EXC | P0 |
| [ST-PROJ-015](../systemtest/ST-PROJ-015.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Chặn tạo/tự lưu khi không có quyền lợi hoặc hết quota | EXC | P0 |
| [ST-PROJ-016](../systemtest/ST-PROJ-016.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Lượt bị giữ hết chặn lưu; AI lỗi trả lượt mới cho yêu cầu tiếp theo | EXC / ALT | P0 |
| [ST-PROJ-017](../systemtest/ST-PROJ-017.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Nhân viên và khách chưa đăng nhập không tạo bản dự toán | EXC | P0 |
| [ST-PROJ-018](../systemtest/ST-PROJ-018.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Không đọc hoặc cập nhật bản dự toán của người khác | EXC / NFR | P0 |
| [ST-PROJ-019](../systemtest/ST-PROJ-019.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Cấu hình cũ áp dụng khi mở lại và thử lại AI | ALT / Integration boundary | P1 |
| [ST-PROJ-020](../systemtest/ST-PROJ-020.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Bản nháp cũ không được chọn loại công trình mới thêm | ALT / EXC | P1 |
| [ST-PROJ-021](../systemtest/ST-PROJ-021.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Gửi hợp lệ giữ một lượt và khóa đúng đầu vào | Main / EXC | P0 |
| [ST-PROJ-022](../systemtest/ST-PROJ-022.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Đủ kết quả đã lưu và mở được mới ghi thành công | Main / Integration boundary | P0 |
| [ST-PROJ-023](../systemtest/ST-PROJ-023.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Thiếu kết quả hoặc lưu lỗi không được tính thành công | EXC / Integration boundary | P0 |
| [ST-PROJ-024](../systemtest/ST-PROJ-024.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | AI thất bại mở sửa và khách chủ động thử lại | ALT / EXC | P0 |
| [ST-PROJ-025](../systemtest/ST-PROJ-025.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Thử lại sau lỗi phải kiểm tra gói và lượt hiện tại | EXC | P0 |
| [ST-PROJ-026](../systemtest/ST-PROJ-026.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Quá thời gian, kết quả muộn và lần thử lại không bị lẫn | EXC / Integration boundary | P0 |
| [ST-PROJ-027](../systemtest/ST-PROJ-027.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Thành công trước timeout không bị hoàn lượt bởi xử lý muộn | EXC / Integration boundary | P0 |
| [ST-PROJ-028](../systemtest/ST-PROJ-028.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Đã thành công phải tạo bản mới để nhận phương án khác | EXC / ALT | P0 |
| [ST-PROJ-029](../systemtest/ST-PROJ-029.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Gói hết hạn hoặc đổi kỳ trong lúc AI đang chạy | ALT / Integration boundary | P0 |
| [ST-PROJ-030](../systemtest/ST-PROJ-030.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Không giới hạn lượt vẫn kiểm tra hiệu lực và vòng đời AI | ALT / EXC | P0 |
| [ST-PROJ-031](../systemtest/ST-PROJ-031.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Yêu cầu không hợp lệ bị chặn trước khi giữ lượt hoặc gửi AI | EXC | P0 |
| [ST-PROJ-032](../systemtest/ST-PROJ-032.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Hai bản cùng tài khoản tranh lượt cuối | NFR / Integration boundary | P0 |
| [ST-PROJ-033](../systemtest/ST-PROJ-033.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Xem đúng kết quả AI đã lưu, không tính lại hoặc dùng dữ liệu mẫu | Main / Integration boundary | P1 |
| [ST-PROJ-034](../systemtest/ST-PROJ-034.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | PDF và Excel khớp cùng kết quả nguồn | Main / Integration boundary | P1 |
| [ST-PROJ-035](../systemtest/ST-PROJ-035.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Hết hạn gói vẫn xuất mới và tải lại PDF/Excel cũ | ALT / Integration boundary | P0 |
| [ST-PROJ-036](../systemtest/ST-PROJ-036.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Từ chối xem và tải kết quả qua mã bản của người khác | EXC / NFR | P0 |
| [ST-PROJ-037](../systemtest/ST-PROJ-037.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Không cung cấp hồ sơ giả khi AI chưa xong hoặc thất bại | EXC | P1 |
| [ST-PROJ-038](../systemtest/ST-PROJ-038.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Lỗi xuất hoặc tải tệp không làm mất kết quả, không trừ lượt | EXC / ALT / Integration boundary | P1 |
| [ST-PROJ-039](../systemtest/ST-PROJ-039.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Link và QR cho người không đăng nhập xem/tải đúng hồ sơ | Main | P0 |
| [ST-PROJ-040](../systemtest/ST-PROJ-040.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Thu hồi chặn cả link, QR, email và yêu cầu tải mới | ALT / EXC / NFR | P0 |
| [ST-PROJ-041](../systemtest/ST-PROJ-041.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Hết hạn chặn mọi cách truy cập và tải mới | EXC / NFR | P0 |
| [ST-PROJ-042](../systemtest/ST-PROJ-042.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Link không hợp lệ không làm lộ nội dung | EXC / NFR | P0 |
| [ST-PROJ-043](../systemtest/ST-PROJ-043.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Email một người nhận chứa link, không đính kèm tệp | ALT / Integration boundary | P1 |
| [ST-PROJ-044](../systemtest/ST-PROJ-044.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Từ chối email không hợp lệ hoặc nhiều người nhận | EXC / Integration boundary | P1 |
| [ST-PROJ-045](../systemtest/ST-PROJ-045.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Email bị từ chối hoặc chưa xác nhận không báo đã gửi thành công | EXC / Integration boundary | P1 |
| [ST-PROJ-046](../systemtest/ST-PROJ-046.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Hết hạn gói vẫn tạo, gửi và thu hồi link hồ sơ cũ | ALT / Integration boundary | P0 |
| [ST-PROJ-047](../systemtest/ST-PROJ-047.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Link công khai không cấp quyền sửa, gửi AI hoặc quản lý chia sẻ | EXC / NFR | P0 |
| [ST-PROJ-048](../systemtest/ST-PROJ-048.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Admin tự nhập hai danh mục và thêm loại thứ sáu | Main | P1 |
| [ST-PROJ-049](../systemtest/ST-PROJ-049.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Tầng và tum bật tắt độc lập, số tầng theo từng loại | Main / ALT / EXC | P1 |
| [ST-PROJ-050](../systemtest/ST-PROJ-050.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Sửa tên ảnh và gán phong cách không làm lẫn hai nhóm | Main / ALT / EXC | P1 |
| [ST-PROJ-051](../systemtest/ST-PROJ-051.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Mỗi nhóm bật cần một phong cách; AI vẫn trả đủ kết quả | Main / EXC / Integration boundary | P1 |
| [ST-PROJ-052](../systemtest/ST-PROJ-052.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Giữ cấu hình đã áp dụng ở mọi trạng thái bản cũ | ALT / Integration boundary | P0 |
| [ST-PROJ-053](../systemtest/ST-PROJ-053.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Danh sách đang bật không được rỗng khi tạo hoặc sửa | EXC / ALT | P0 |
| [ST-PROJ-054](../systemtest/ST-PROJ-054.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Ảnh phong cách bắt buộc JPG PNG WebP tối đa 5 MB | Main / EXC | P1 |
| [ST-PROJ-055](../systemtest/ST-PROJ-055.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Khách hoặc nhân viên không có quyền không sửa danh mục | EXC / NFR | P0 |
| [ST-PROJ-056](../systemtest/ST-PROJ-056.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Lỗi lưu danh mục hoặc ảnh không báo thành công sai | EXC / Integration boundary | P1 |
| [ST-PROJ-057](../systemtest/ST-PROJ-057.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Luồng đầy đủ từ Admin cấu hình đến người nhận tải hồ sơ | Main / Integration boundary | P0 |
| [ST-PROJ-058](../systemtest/ST-PROJ-058.md) | [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | Tên danh mục bắt buộc, tối đa 200 ký tự sau trim và được trùng | Main / EXC | P1 |
| [ST-PROJ-059](../systemtest/ST-PROJ-059.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Ngày hết hạn tính hết ngày Việt Nam và từ chối ngày đã qua | Main / EXC / NFR | P0 |
| [ST-PROJ-060](../systemtest/ST-PROJ-060.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Một link đang hiệu lực dùng chung, kể cả khi yêu cầu đồng thời | Main / ALT / Integration boundary | P0 |
| [ST-PROJ-061](../systemtest/ST-PROJ-061.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Gói hết hạn vẫn đổi tên, lưu đầu vào vẫn bị chặn | EXC | P1 |
| [ST-PROJ-062](../systemtest/ST-PROJ-062.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Hết lượt hoặc không có quyền tạo thiết kế vẫn đổi tên | EXC | P1 |
| [ST-PROJ-063](../systemtest/ST-PROJ-063.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Lượt còn lại đang bị giữ hết không chặn đổi tên | EXC | P1 |
| [ST-PROJ-064](../systemtest/ST-PROJ-064.md) | [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | Đổi tên khi AI đang xử lý không đổi đầu vào đã gửi | EXC / Integration boundary | P0 |
| [ST-PROJ-065](../systemtest/ST-PROJ-065.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Bản đã thành công vẫn đổi tên, đầu vào vẫn khóa | EXC | P1 |
| [ST-PROJ-066](../systemtest/ST-PROJ-066.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Chỉ chủ sở hữu được đổi tên, link chia sẻ không có quyền đổi | EXC / NFR | P0 |
| [ST-PROJ-067](../systemtest/ST-PROJ-067.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Đổi tên vẫn kiểm tên bắt buộc và tối đa 200 ký tự | EXC | P1 |
| [ST-PROJ-068](../systemtest/ST-PROJ-068.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Đổi tên từ hai tab và gửi lại không ghi đè nhau | EXC / Integration boundary | P1 |
| [ST-PROJ-069](../systemtest/ST-PROJ-069.md) | [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | Hồ sơ, link và tệp xuất lại theo tên hiện tại | Main / Integration boundary | P1 |
| [ST-PROJ-070](../systemtest/ST-PROJ-070.md) | [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | Dùng lại link còn hiệu lực khi khách chọn ngày khác | ALT | P1 |
| [ST-PROJ-071](../systemtest/ST-PROJ-071.md) | [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | Nguồn địa chỉ lỗi dùng bản lưu, phiên bản dữ liệu đổi và xã cũ trong bản nháp | EXC / ALT / Integration boundary | P1 |

## Truy vết tiêu chí nghiệm thu

| Story / AC | System Test |
|---|---|
| [STORY-PROJ-001/AC-001](../userstory/STORY-PROJ-001.md#ac-001) | [ST-PROJ-001](../systemtest/ST-PROJ-001.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-001/AC-002](../userstory/STORY-PROJ-001.md#ac-002) | [ST-PROJ-001](../systemtest/ST-PROJ-001.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-001/AC-003](../userstory/STORY-PROJ-001.md#ac-003) | [ST-PROJ-003](../systemtest/ST-PROJ-003.md) |
| [STORY-PROJ-001/AC-004](../userstory/STORY-PROJ-001.md#ac-004) | [ST-PROJ-005](../systemtest/ST-PROJ-005.md) |
| [STORY-PROJ-001/AC-005](../userstory/STORY-PROJ-001.md#ac-005) | [ST-PROJ-008](../systemtest/ST-PROJ-008.md), [ST-PROJ-071](../systemtest/ST-PROJ-071.md) |
| [STORY-PROJ-001/AC-006](../userstory/STORY-PROJ-001.md#ac-006) | [ST-PROJ-014](../systemtest/ST-PROJ-014.md), [ST-PROJ-015](../systemtest/ST-PROJ-015.md), [ST-PROJ-016](../systemtest/ST-PROJ-016.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md), [ST-PROJ-061](../systemtest/ST-PROJ-061.md), [ST-PROJ-062](../systemtest/ST-PROJ-062.md), [ST-PROJ-063](../systemtest/ST-PROJ-063.md) |
| [STORY-PROJ-001/AC-007](../userstory/STORY-PROJ-001.md#ac-007) | [ST-PROJ-017](../systemtest/ST-PROJ-017.md), [ST-PROJ-018](../systemtest/ST-PROJ-018.md), [ST-PROJ-066](../systemtest/ST-PROJ-066.md) |
| [STORY-PROJ-001/AC-008](../userstory/STORY-PROJ-001.md#ac-008) | [ST-PROJ-012](../systemtest/ST-PROJ-012.md), [ST-PROJ-013](../systemtest/ST-PROJ-013.md) |
| [STORY-PROJ-001/AC-009](../userstory/STORY-PROJ-001.md#ac-009) | [ST-PROJ-006](../systemtest/ST-PROJ-006.md), [ST-PROJ-007](../systemtest/ST-PROJ-007.md) |
| [STORY-PROJ-001/AC-010](../userstory/STORY-PROJ-001.md#ac-010) | [ST-PROJ-004](../systemtest/ST-PROJ-004.md) |
| [STORY-PROJ-001/AC-011](../userstory/STORY-PROJ-001.md#ac-011) | [ST-PROJ-014](../systemtest/ST-PROJ-014.md), [ST-PROJ-021](../systemtest/ST-PROJ-021.md), [ST-PROJ-024](../systemtest/ST-PROJ-024.md), [ST-PROJ-065](../systemtest/ST-PROJ-065.md) |
| [STORY-PROJ-001/AC-012](../userstory/STORY-PROJ-001.md#ac-012) | [ST-PROJ-008](../systemtest/ST-PROJ-008.md), [ST-PROJ-051](../systemtest/ST-PROJ-051.md) |
| [STORY-PROJ-001/AC-013](../userstory/STORY-PROJ-001.md#ac-013) | [ST-PROJ-019](../systemtest/ST-PROJ-019.md) |
| [STORY-PROJ-001/AC-014](../userstory/STORY-PROJ-001.md#ac-014) | [ST-PROJ-009](../systemtest/ST-PROJ-009.md) |
| [STORY-PROJ-001/AC-015](../userstory/STORY-PROJ-001.md#ac-015) | [ST-PROJ-010](../systemtest/ST-PROJ-010.md), [ST-PROJ-071](../systemtest/ST-PROJ-071.md) |
| [STORY-PROJ-001/AC-016](../userstory/STORY-PROJ-001.md#ac-016) | [ST-PROJ-011](../systemtest/ST-PROJ-011.md) |
| [STORY-PROJ-001/AC-017](../userstory/STORY-PROJ-001.md#ac-017) | [ST-PROJ-012](../systemtest/ST-PROJ-012.md), [ST-PROJ-013](../systemtest/ST-PROJ-013.md), [ST-PROJ-014](../systemtest/ST-PROJ-014.md) |
| [STORY-PROJ-001/AC-018](../userstory/STORY-PROJ-001.md#ac-018) | [ST-PROJ-020](../systemtest/ST-PROJ-020.md) |
| [STORY-PROJ-001/AC-019](../userstory/STORY-PROJ-001.md#ac-019) | [ST-PROJ-002](../systemtest/ST-PROJ-002.md), [ST-PROJ-061](../systemtest/ST-PROJ-061.md), [ST-PROJ-062](../systemtest/ST-PROJ-062.md), [ST-PROJ-063](../systemtest/ST-PROJ-063.md), [ST-PROJ-064](../systemtest/ST-PROJ-064.md), [ST-PROJ-065](../systemtest/ST-PROJ-065.md), [ST-PROJ-067](../systemtest/ST-PROJ-067.md), [ST-PROJ-068](../systemtest/ST-PROJ-068.md) |
| [STORY-PROJ-001/AC-020](../userstory/STORY-PROJ-001.md#ac-020) | [ST-PROJ-005](../systemtest/ST-PROJ-005.md), [ST-PROJ-007](../systemtest/ST-PROJ-007.md) |
| [STORY-PROJ-002/AC-001](../userstory/STORY-PROJ-002.md#ac-001) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md), [ST-PROJ-031](../systemtest/ST-PROJ-031.md), [ST-PROJ-032](../systemtest/ST-PROJ-032.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-064](../systemtest/ST-PROJ-064.md) |
| [STORY-PROJ-002/AC-002](../userstory/STORY-PROJ-002.md#ac-002) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md), [ST-PROJ-064](../systemtest/ST-PROJ-064.md) |
| [STORY-PROJ-002/AC-003](../userstory/STORY-PROJ-002.md#ac-003) | [ST-PROJ-022](../systemtest/ST-PROJ-022.md), [ST-PROJ-023](../systemtest/ST-PROJ-023.md), [ST-PROJ-027](../systemtest/ST-PROJ-027.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-002/AC-004](../userstory/STORY-PROJ-002.md#ac-004) | [ST-PROJ-023](../systemtest/ST-PROJ-023.md), [ST-PROJ-024](../systemtest/ST-PROJ-024.md), [ST-PROJ-025](../systemtest/ST-PROJ-025.md), [ST-PROJ-026](../systemtest/ST-PROJ-026.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md) |
| [STORY-PROJ-002/AC-005](../userstory/STORY-PROJ-002.md#ac-005) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md), [ST-PROJ-027](../systemtest/ST-PROJ-027.md) |
| [STORY-PROJ-002/AC-006](../userstory/STORY-PROJ-002.md#ac-006) | [ST-PROJ-028](../systemtest/ST-PROJ-028.md) |
| [STORY-PROJ-002/AC-007](../userstory/STORY-PROJ-002.md#ac-007) | [ST-PROJ-029](../systemtest/ST-PROJ-029.md) |
| [STORY-PROJ-003/AC-001](../userstory/STORY-PROJ-003.md#ac-001) | [ST-PROJ-033](../systemtest/ST-PROJ-033.md), [ST-PROJ-037](../systemtest/ST-PROJ-037.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-003/AC-002](../userstory/STORY-PROJ-003.md#ac-002) | [ST-PROJ-034](../systemtest/ST-PROJ-034.md), [ST-PROJ-037](../systemtest/ST-PROJ-037.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [STORY-PROJ-003/AC-003](../userstory/STORY-PROJ-003.md#ac-003) | [ST-PROJ-035](../systemtest/ST-PROJ-035.md) |
| [STORY-PROJ-003/AC-004](../userstory/STORY-PROJ-003.md#ac-004) | [ST-PROJ-036](../systemtest/ST-PROJ-036.md) |
| [STORY-PROJ-003/AC-005](../userstory/STORY-PROJ-003.md#ac-005) | [ST-PROJ-038](../systemtest/ST-PROJ-038.md) |
| [STORY-PROJ-003/AC-006](../userstory/STORY-PROJ-003.md#ac-006) | [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [STORY-PROJ-004/AC-001](../userstory/STORY-PROJ-004.md#ac-001) | [ST-PROJ-039](../systemtest/ST-PROJ-039.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-060](../systemtest/ST-PROJ-060.md) |
| [STORY-PROJ-004/AC-002](../userstory/STORY-PROJ-004.md#ac-002) | [ST-PROJ-039](../systemtest/ST-PROJ-039.md), [ST-PROJ-042](../systemtest/ST-PROJ-042.md), [ST-PROJ-047](../systemtest/ST-PROJ-047.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-066](../systemtest/ST-PROJ-066.md), [ST-PROJ-069](../systemtest/ST-PROJ-069.md) |
| [STORY-PROJ-004/AC-003](../userstory/STORY-PROJ-004.md#ac-003) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md), [ST-PROJ-041](../systemtest/ST-PROJ-041.md), [ST-PROJ-042](../systemtest/ST-PROJ-042.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-059](../systemtest/ST-PROJ-059.md) |
| [STORY-PROJ-004/AC-004](../userstory/STORY-PROJ-004.md#ac-004) | [ST-PROJ-043](../systemtest/ST-PROJ-043.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md), [ST-PROJ-060](../systemtest/ST-PROJ-060.md) |
| [STORY-PROJ-004/AC-005](../userstory/STORY-PROJ-004.md#ac-005) | [ST-PROJ-046](../systemtest/ST-PROJ-046.md) |
| [STORY-PROJ-004/AC-006](../userstory/STORY-PROJ-004.md#ac-006) | [ST-PROJ-047](../systemtest/ST-PROJ-047.md) |
| [STORY-PROJ-004/AC-007](../userstory/STORY-PROJ-004.md#ac-007) | [ST-PROJ-045](../systemtest/ST-PROJ-045.md) |
| [STORY-PROJ-004/AC-008](../userstory/STORY-PROJ-004.md#ac-008) | [ST-PROJ-043](../systemtest/ST-PROJ-043.md), [ST-PROJ-044](../systemtest/ST-PROJ-044.md) |
| [STORY-PROJ-004/AC-009](../userstory/STORY-PROJ-004.md#ac-009) | [ST-PROJ-059](../systemtest/ST-PROJ-059.md), [ST-PROJ-070](../systemtest/ST-PROJ-070.md) |
| [STORY-PROJ-004/AC-010](../userstory/STORY-PROJ-004.md#ac-010) | [ST-PROJ-060](../systemtest/ST-PROJ-060.md), [ST-PROJ-070](../systemtest/ST-PROJ-070.md) |
| [STORY-PROJ-005/AC-001](../userstory/STORY-PROJ-005.md#ac-001) | [ST-PROJ-048](../systemtest/ST-PROJ-048.md) |
| [STORY-PROJ-005/AC-002](../userstory/STORY-PROJ-005.md#ac-002) | [ST-PROJ-049](../systemtest/ST-PROJ-049.md) |
| [STORY-PROJ-005/AC-003](../userstory/STORY-PROJ-005.md#ac-003) | [ST-PROJ-048](../systemtest/ST-PROJ-048.md), [ST-PROJ-050](../systemtest/ST-PROJ-050.md) |
| [STORY-PROJ-005/AC-004](../userstory/STORY-PROJ-005.md#ac-004) | [ST-PROJ-051](../systemtest/ST-PROJ-051.md) |
| [STORY-PROJ-005/AC-005](../userstory/STORY-PROJ-005.md#ac-005) | [ST-PROJ-050](../systemtest/ST-PROJ-050.md), [ST-PROJ-052](../systemtest/ST-PROJ-052.md) |
| [STORY-PROJ-005/AC-006](../userstory/STORY-PROJ-005.md#ac-006) | [ST-PROJ-055](../systemtest/ST-PROJ-055.md), [ST-PROJ-056](../systemtest/ST-PROJ-056.md) |
| [STORY-PROJ-005/AC-007](../userstory/STORY-PROJ-005.md#ac-007) | [ST-PROJ-053](../systemtest/ST-PROJ-053.md) |
| [STORY-PROJ-005/AC-008](../userstory/STORY-PROJ-005.md#ac-008) | [ST-PROJ-054](../systemtest/ST-PROJ-054.md) |
| [STORY-PROJ-005/AC-009](../userstory/STORY-PROJ-005.md#ac-009) | [ST-PROJ-048](../systemtest/ST-PROJ-048.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-005/AC-010](../userstory/STORY-PROJ-005.md#ac-010) | [ST-PROJ-058](../systemtest/ST-PROJ-058.md) |

## Truy vết các luồng

| Story / luồng | System Test |
|---|---|
| [STORY-PROJ-001/Main Flow](../userstory/STORY-PROJ-001.md#main-flow) | [ST-PROJ-001](../systemtest/ST-PROJ-001.md), [ST-PROJ-002](../systemtest/ST-PROJ-002.md), [ST-PROJ-003](../systemtest/ST-PROJ-003.md), [ST-PROJ-008](../systemtest/ST-PROJ-008.md), [ST-PROJ-019](../systemtest/ST-PROJ-019.md) |
| [STORY-PROJ-001/ALT-01](../userstory/STORY-PROJ-001.md#alt-01) | [ST-PROJ-001](../systemtest/ST-PROJ-001.md) |
| [STORY-PROJ-001/ALT-02](../userstory/STORY-PROJ-001.md#alt-02) | [ST-PROJ-005](../systemtest/ST-PROJ-005.md) |
| [STORY-PROJ-001/ALT-03](../userstory/STORY-PROJ-001.md#alt-03) | [ST-PROJ-003](../systemtest/ST-PROJ-003.md), [ST-PROJ-008](../systemtest/ST-PROJ-008.md) |
| [STORY-PROJ-001/ALT-04](../userstory/STORY-PROJ-001.md#alt-04) | [ST-PROJ-009](../systemtest/ST-PROJ-009.md), [ST-PROJ-020](../systemtest/ST-PROJ-020.md) |
| [STORY-PROJ-001/ALT-05](../userstory/STORY-PROJ-001.md#alt-05) | [ST-PROJ-010](../systemtest/ST-PROJ-010.md), [ST-PROJ-071](../systemtest/ST-PROJ-071.md) |
| [STORY-PROJ-001/EXC-01](../userstory/STORY-PROJ-001.md#exc-01) | [ST-PROJ-014](../systemtest/ST-PROJ-014.md), [ST-PROJ-015](../systemtest/ST-PROJ-015.md), [ST-PROJ-016](../systemtest/ST-PROJ-016.md), [ST-PROJ-017](../systemtest/ST-PROJ-017.md), [ST-PROJ-061](../systemtest/ST-PROJ-061.md), [ST-PROJ-062](../systemtest/ST-PROJ-062.md), [ST-PROJ-063](../systemtest/ST-PROJ-063.md), [ST-PROJ-066](../systemtest/ST-PROJ-066.md), [ST-PROJ-067](../systemtest/ST-PROJ-067.md) |
| [STORY-PROJ-001/EXC-02](../userstory/STORY-PROJ-001.md#exc-02) | [ST-PROJ-018](../systemtest/ST-PROJ-018.md), [ST-PROJ-066](../systemtest/ST-PROJ-066.md) |
| [STORY-PROJ-001/EXC-03](../userstory/STORY-PROJ-001.md#exc-03) | [ST-PROJ-012](../systemtest/ST-PROJ-012.md), [ST-PROJ-013](../systemtest/ST-PROJ-013.md), [ST-PROJ-014](../systemtest/ST-PROJ-014.md) |
| [STORY-PROJ-001/EXC-04](../userstory/STORY-PROJ-001.md#exc-04) | [ST-PROJ-011](../systemtest/ST-PROJ-011.md) |
| [STORY-PROJ-002/Main Flow](../userstory/STORY-PROJ-002.md#main-flow) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md), [ST-PROJ-022](../systemtest/ST-PROJ-022.md), [ST-PROJ-027](../systemtest/ST-PROJ-027.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md), [ST-PROJ-032](../systemtest/ST-PROJ-032.md), [ST-PROJ-057](../systemtest/ST-PROJ-057.md) |
| [STORY-PROJ-002/ALT-01](../userstory/STORY-PROJ-002.md#alt-01) | [ST-PROJ-024](../systemtest/ST-PROJ-024.md), [ST-PROJ-025](../systemtest/ST-PROJ-025.md) |
| [STORY-PROJ-002/ALT-02](../userstory/STORY-PROJ-002.md#alt-02) | [ST-PROJ-029](../systemtest/ST-PROJ-029.md) |
| [STORY-PROJ-002/EXC-01](../userstory/STORY-PROJ-002.md#exc-01) | [ST-PROJ-025](../systemtest/ST-PROJ-025.md), [ST-PROJ-028](../systemtest/ST-PROJ-028.md), [ST-PROJ-031](../systemtest/ST-PROJ-031.md) |
| [STORY-PROJ-002/EXC-02](../userstory/STORY-PROJ-002.md#exc-02) | [ST-PROJ-021](../systemtest/ST-PROJ-021.md), [ST-PROJ-064](../systemtest/ST-PROJ-064.md) |
| [STORY-PROJ-002/EXC-03](../userstory/STORY-PROJ-002.md#exc-03) | [ST-PROJ-023](../systemtest/ST-PROJ-023.md), [ST-PROJ-024](../systemtest/ST-PROJ-024.md), [ST-PROJ-026](../systemtest/ST-PROJ-026.md), [ST-PROJ-030](../systemtest/ST-PROJ-030.md) |
| [STORY-PROJ-002/EXC-04](../userstory/STORY-PROJ-002.md#exc-04) | [ST-PROJ-026](../systemtest/ST-PROJ-026.md) |
| [STORY-PROJ-003/Main Flow](../userstory/STORY-PROJ-003.md#main-flow) | [ST-PROJ-033](../systemtest/ST-PROJ-033.md), [ST-PROJ-034](../systemtest/ST-PROJ-034.md) |
| [STORY-PROJ-003/ALT-01](../userstory/STORY-PROJ-003.md#alt-01) | [ST-PROJ-035](../systemtest/ST-PROJ-035.md) |
| [STORY-PROJ-003/EXC-01](../userstory/STORY-PROJ-003.md#exc-01) | [ST-PROJ-036](../systemtest/ST-PROJ-036.md) |
| [STORY-PROJ-003/EXC-02](../userstory/STORY-PROJ-003.md#exc-02) | [ST-PROJ-037](../systemtest/ST-PROJ-037.md) |
| [STORY-PROJ-003/EXC-03](../userstory/STORY-PROJ-003.md#exc-03) | [ST-PROJ-038](../systemtest/ST-PROJ-038.md) |
| [STORY-PROJ-004/Main Flow](../userstory/STORY-PROJ-004.md#main-flow) | [ST-PROJ-039](../systemtest/ST-PROJ-039.md), [ST-PROJ-060](../systemtest/ST-PROJ-060.md), [ST-PROJ-070](../systemtest/ST-PROJ-070.md) |
| [STORY-PROJ-004/ALT-01](../userstory/STORY-PROJ-004.md#alt-01) | [ST-PROJ-043](../systemtest/ST-PROJ-043.md) |
| [STORY-PROJ-004/ALT-02](../userstory/STORY-PROJ-004.md#alt-02) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md) |
| [STORY-PROJ-004/ALT-03](../userstory/STORY-PROJ-004.md#alt-03) | [ST-PROJ-046](../systemtest/ST-PROJ-046.md) |
| [STORY-PROJ-004/EXC-01](../userstory/STORY-PROJ-004.md#exc-01) | [ST-PROJ-040](../systemtest/ST-PROJ-040.md), [ST-PROJ-041](../systemtest/ST-PROJ-041.md), [ST-PROJ-042](../systemtest/ST-PROJ-042.md) |
| [STORY-PROJ-004/EXC-02](../userstory/STORY-PROJ-004.md#exc-02) | [ST-PROJ-047](../systemtest/ST-PROJ-047.md) |
| [STORY-PROJ-004/EXC-03](../userstory/STORY-PROJ-004.md#exc-03) | [ST-PROJ-045](../systemtest/ST-PROJ-045.md) |
| [STORY-PROJ-004/EXC-04](../userstory/STORY-PROJ-004.md#exc-04) | [ST-PROJ-044](../systemtest/ST-PROJ-044.md) |
| [STORY-PROJ-004/EXC-05](../userstory/STORY-PROJ-004.md#exc-05) | [ST-PROJ-059](../systemtest/ST-PROJ-059.md) |
| [STORY-PROJ-005/Main Flow](../userstory/STORY-PROJ-005.md#main-flow) | [ST-PROJ-048](../systemtest/ST-PROJ-048.md), [ST-PROJ-049](../systemtest/ST-PROJ-049.md), [ST-PROJ-050](../systemtest/ST-PROJ-050.md), [ST-PROJ-051](../systemtest/ST-PROJ-051.md), [ST-PROJ-054](../systemtest/ST-PROJ-054.md), [ST-PROJ-058](../systemtest/ST-PROJ-058.md) |
| [STORY-PROJ-005/ALT-01](../userstory/STORY-PROJ-005.md#alt-01) | [ST-PROJ-050](../systemtest/ST-PROJ-050.md), [ST-PROJ-052](../systemtest/ST-PROJ-052.md) |
| [STORY-PROJ-005/EXC-01](../userstory/STORY-PROJ-005.md#exc-01) | [ST-PROJ-055](../systemtest/ST-PROJ-055.md) |
| [STORY-PROJ-005/EXC-02](../userstory/STORY-PROJ-005.md#exc-02) | [ST-PROJ-054](../systemtest/ST-PROJ-054.md), [ST-PROJ-056](../systemtest/ST-PROJ-056.md) |
| [STORY-PROJ-005/EXC-03](../userstory/STORY-PROJ-005.md#exc-03) | [ST-PROJ-053](../systemtest/ST-PROJ-053.md) |

## Phạm vi rà soát nguồn

- Đã đối chiếu bộ Story/BR PROJ hiện tại và các quy tắc trực tiếp về tạo/lưu, giữ/tính/giải phóng lượt, timeout, thử lại, không giới hạn và tài khoản nhân viên. Nội dung và comment template US/BR/ST đã được đọc trong phiên; không lấy ví dụ template làm chính sách.
- BR-SUB-003 và một số tài liệu cũ còn giữ mô tả lịch sử về quyền 3D. Bộ ST mới theo phạm vi hiện hành: không dùng cờ phong cách hoặc cờ hiển thị quyền 3D để tự cắt kết quả; bộ đầu ra cụ thể chờ hợp đồng AI.
- ST-SUB-056 và ST-SUB-076 đã được đọc để đối chiếu PDF sau hết hạn và lượt đang giữ hết. Bộ ST-PROJ kiểm tra các hành vi này trong luồng Tạo dự toán và bổ sung Excel/chia sẻ; không coi test SUB đã tồn tại là đã chạy.
- Chưa hoàn tất đọc và rà soát ngữ nghĩa toàn bộ chuỗi tham chiếu qua subscription, thanh toán, RBAC và các TDD/UT/ST cũ. Kiểm tra tự động dưới đây chỉ xác minh cấu trúc/liên kết, không thay cho rà soát nội dung phụ thuộc.

## Kiểm tra tài liệu

- Mỗi file ST có một heading mã, một dòng dữ liệu đúng 13 cột; mã bảng khớp heading.
- Reviewer/Approver đúng tên đã xác nhận; Status Draft, chưa thực thi.
- Trace to khớp TEST_LINKS; mã tài liệu và section đích tồn tại, không trùng mã ST.
- Mỗi AC và mỗi nhánh có ít nhất một ca được liên kết. Các bước và expected result được soạn theo hành vi; vẫn cần chạy sau triển khai để đánh giá implementation.

## Bản tài liệu dùng làm căn cứ

Các mã SHA-256 dưới đây giữ nguyên mốc US/BR lúc soạn 57 ST ban đầu. Sau đó đã bổ sung ba quyết định và liên kết TDD; bảng này không đại diện nội dung file hiện tại, không phải số phiên bản hoặc lịch sử phê duyệt tự tạo. Các mốc sau được ghi riêng bên dưới; mốc mới nhất là ngày 25/09/2026.

| File | SHA-256 |
|---|---|
| [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | `13f9073ad95928633e679b93a8a5aa86ef60e7c5a79e8dbd937107256953d45d` |
| [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | `6ca4826aefb12ff042fde27d2dc8ecb25c27b8cb80e94c01343f90a6ab425944` |
| [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | `4efdb6dd803268f8b4477e4fc27ec6ed00349d7cfb31d5563794e6961c863b8e` |
| [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | `7247a0685df5c7c7b064426f64641adebc596b24edbec2692e893ee8ab8dcf4b` |
| [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | `a133321e31b23f44d9f4513223494fb3ac382913db8b16522501c89e2800bd1b` |
| [BR-PROJ-001](../businessrule/BR-PROJ-001.md) | `49bdb37302aae0b36376791c52fcc4d45107976589752588ea08ff803fb72b32` |
| [BR-PROJ-002](../businessrule/BR-PROJ-002.md) | `2aed709a7a4c5e8dbb586a43ec1eeead9a751d108c18e662e497d76238d17dab` |
| [BR-PROJ-003](../businessrule/BR-PROJ-003.md) | `be6cad9732decec9be6890ec690688dc0244cbf4f1e08a8d29a874fbf072e759` |
| [BR-PROJ-004](../businessrule/BR-PROJ-004.md) | `e1395af9c9456d0ef4d3fb7ee786b2fb1832a47bf95ae55657c7e59bed3bcc08` |
| [BR-PROJ-005](../businessrule/BR-PROJ-005.md) | `c298e1cce65e60e53695d853f79eab1dfed9cf246995fcaf5dae41198180e344` |
| [BR-PROJ-006](../businessrule/BR-PROJ-006.md) | `8696eb229d9267fddf692c5ce499e8a66c1a9964d9410d7e17487ee767a83cc2` |
| [BR-PROJ-007](../businessrule/BR-PROJ-007.md) | `b9378df057426b79c7399527a5e90d2ad611694d1496609141e304014903815b` |

## Mốc bổ sung khi thiết kế TDD ngày 21/09/2026

Đã cập nhật các quyết định người dùng xác nhận và tham chiếu TDD. Các hash sau chỉ dùng đối chiếu nội dung, không xác nhận phê duyệt.

| File | SHA-256 |
|---|---|
| [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | `ee875cf73a12765ff203becaee4a1e127bbbaa869cad4afb0e0e7fadee1cb3e4` |
| [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | `ed1b99f7b5d43255c7faceb912ddf623ac002e49de7562260e23b22d1fefa6ac` |
| [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | `c456471fbafae321158accdb72e42b3838196e47dfa1719a8f0f76f840ab3132` |
| [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | `f3aa5c4c9db5aabf050cee579d2469a34f3f094994fece08c1bb2c629acb33a6` |
| [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | `62fa83378d25ca1246b0e9332c0cddb7bbb70e2ed90c14f8446723f10f73a2b1` |
| [BR-PROJ-001](../businessrule/BR-PROJ-001.md) | `49bdb37302aae0b36376791c52fcc4d45107976589752588ea08ff803fb72b32` |
| [BR-PROJ-002](../businessrule/BR-PROJ-002.md) | `12752059af15c7776058fc9da9a5ed513d26a3b8391198a2e2fcfedb663fe741` |
| [BR-PROJ-003](../businessrule/BR-PROJ-003.md) | `be6cad9732decec9be6890ec690688dc0244cbf4f1e08a8d29a874fbf072e759` |
| [BR-PROJ-004](../businessrule/BR-PROJ-004.md) | `bc104af242a7fc45ff91a76af63a9176bc8c55d9d4be4f3f6a6bbf0e38ab8680` |
| [BR-PROJ-005](../businessrule/BR-PROJ-005.md) | `c298e1cce65e60e53695d853f79eab1dfed9cf246995fcaf5dae41198180e344` |
| [BR-PROJ-006](../businessrule/BR-PROJ-006.md) | `7e91ac36df5072c150a542dd6f2d8ffafb5be68f665504407f9326eb87509cd4` |
| [BR-PROJ-007](../businessrule/BR-PROJ-007.md) | `b9378df057426b79c7399527a5e90d2ad611694d1496609141e304014903815b` |

## Mốc cập nhật ngày 25/09/2026

Đã cập nhật theo các quyết định người dùng xác nhận ngày 25/09/2026: đổi tên bản dự toán, tên hiện tại trên hồ sơ và tệp xuất, dùng lại link khi chọn ngày khác và mã quyền quản trị danh mục. BR-SUB-007 được thêm vào bảng vì khoản 11 là căn cứ của việc đổi tên. Các hash chỉ dùng đối chiếu nội dung, không xác nhận phê duyệt.

| File | SHA-256 |
|---|---|
| [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) | `a14136a360144b433fad42c77ec2e618b843799b55d047dbddcb9597c060f358` |
| [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) | `ae726535a94271215efe5e491c8b257f22bc03cdda67ed1282772734962a2bef` |
| [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) | `3ac04a8eeb8606051d623956b2c2d45962fc13a08bab84e7f65a59061747b680` |
| [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) | `f3aa5c4c9db5aabf050cee579d2469a34f3f094994fece08c1bb2c629acb33a6` |
| [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) | `cd568fdb18f572303f67e51199950e57fe34fff4be31dece37c299579554fad3` |
| [BR-PROJ-001](../businessrule/BR-PROJ-001.md) | `49bdb37302aae0b36376791c52fcc4d45107976589752588ea08ff803fb72b32` |
| [BR-PROJ-002](../businessrule/BR-PROJ-002.md) | `12752059af15c7776058fc9da9a5ed513d26a3b8391198a2e2fcfedb663fe741` |
| [BR-PROJ-003](../businessrule/BR-PROJ-003.md) | `da4647919a4d239f9b0561b3be8dc382d7f9290844db0ba131ff8807a69d21c1` |
| [BR-PROJ-004](../businessrule/BR-PROJ-004.md) | `bc104af242a7fc45ff91a76af63a9176bc8c55d9d4be4f3f6a6bbf0e38ab8680` |
| [BR-PROJ-005](../businessrule/BR-PROJ-005.md) | `21eaa06d91d8a46300fcf80f549d4531a32e4c74fa139436bb652dbeef52c774` |
| [BR-PROJ-006](../businessrule/BR-PROJ-006.md) | `7e91ac36df5072c150a542dd6f2d8ffafb5be68f665504407f9326eb87509cd4` |
| [BR-PROJ-007](../businessrule/BR-PROJ-007.md) | `49c986448c5a6bd262f81f52828f1517e0d3752b129e665ce5888540cbbd6786` |
| [BR-SUB-007](../businessrule/BR-SUB-007.md) | `304a9933681189a355bd860b7cf6feb04e02c4fd305f5401454468c99d97df39` |

## Mốc cập nhật ngày 26/09/2026 (lần 2)

Người dùng xác nhận bốn quyết định: URL ảnh mới phải thuộc tên miền kho presign; nhóm lựa chọn tắt vẫn giữ danh sách nhưng khách không chọn được; người vận hành bật cổng tạo bản dự toán; nguồn địa chỉ là provinces.open-api.vn theo địa giới mới, dùng bản lưu khi nguồn lỗi, bản nháp giữ xã cũ nhưng phải chọn lại trước khi gửi AI. ST-PROJ-006, ST-PROJ-053 và ST-PROJ-054 được sửa; ST-PROJ-071 được thêm. US không đổi; BR-PROJ-002 và BR-PROJ-004 chỉ bổ sung Notes, không đổi nghĩa quy tắc. Kiểm chặn gửi AI khi xã không còn trong dữ liệu mới thuộc TDD-PROJ-002, chưa có ca riêng.
