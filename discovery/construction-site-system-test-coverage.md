# Độ phủ System Test cho hồ sơ công trình mở rộng

Ngày cập nhật: 01/10/2026. Người dùng đã chốt User Story và Business Rule trong hội thoại. Bộ này bổ sung **51 đặc tả System Test** và điều chỉnh **54 đặc tả hiện có** theo nghiệp vụ đã chốt. Các ca đều **chưa thực thi**; chưa có kết quả Pass/Fail.

Người dùng đã chốt bộ đặc tả System Test bằng phản hồi “Ok chốt test r” ngày 01/10/2026. Phạm vi chốt gồm 105 đặc tả đã bàn giao: ST-SITE-001–024, ST-SITE-026–083, ST-PROJ-093–114 và ST-MEDIA-011. Dùng bộ này cùng User Story và Business Rule đã chốt làm căn cứ cập nhật TDD. Xác nhận này chốt nội dung đặc tả, không phải kết quả chạy test hoặc thao tác phê duyệt trên Document First; trạng thái import trong các file vẫn giữ nguyên.

## Phạm vi và dữ liệu thử

Phạm vi gồm hồ sơ công trình đầy đủ, nguồn dự toán, danh mục hiện trạng, cấu hình phân loại dùng chung, tệp đính kèm và quyền truy cập. Phần liên quan dự toán kiểm việc chặn xóa nguồn đang được sử dụng và giải phóng nguồn khi xóa công trình hợp lệ. Chỉ lưu dữ liệu phục vụ các chức năng liên quan về sau; không thêm chức năng tìm kiếm hoặc gợi ý mới.

- Diện tích là diện tích đất, tối đa hai chữ số thập phân; ngân sách là một số nguyên VND dương. Thông tin bắt buộc có ngoại lệ cho nguồn, tệp và các trường phân loại Không áp dụng.
- Dữ liệu nền: 100.25 m², 2.000.000.000 VND, Nhà phố 3 tầng, Không tum, hiện trạng Đất trống, khởi công Trong 1–3 tháng tới. Mỗi ca ghi rõ biến thể cần kiểm; nguồn/tệp/gói trong Precondition được ưu tiên so với dữ liệu nền.
- P1/X1 là bí danh của mã tỉnh/xã hợp lệ do môi trường thử cung cấp; không gửi bí danh làm mã API. Mỗi ca tự chuẩn bị dữ liệu, không cần ca khác chạy thành công trước.
- Giới hạn tệp theo BR-SITE-007: tối đa 9 tệp tổng cộng, mỗi tệp tối đa 10.000.000 byte. Kiểm cả ngưỡng đúng và vượt một byte; không dùng nhãn dung lượng do trình duyệt báo thay kích thước thật.
- ST-SITE-025 và STORY-SITE-002/AC-005, ALT-02 đã bỏ; giữ lịch sử, không tính vào độ phủ đang áp dụng. ST-SITE-035 tiếp tục kiểm chức năng bán kính đã có, không mở rộng truy vấn mới trong đợt này.

## Tài liệu nguồn tại thời điểm soạn

Hash SHA-256 dưới đây tính từ nội dung UTF-8 của file sau khi cập nhật ghi chú trạng thái và liên kết tới bảng này. Đây là mốc nội dung cục bộ, không phải mã phiên bản hoặc xác nhận phê duyệt trên Document First. Tên Reviewer/Approver giữ theo tài liệu nguồn; Owner và phân công còn thiếu vẫn để chưa xác định.

| Nguồn | SHA-256 |
|---|---|
| [STORY-SITE-001](../userstory/STORY-SITE-001.md) | `1e63bffb9e98876d1e1a1d279b737b9151a2d7fd7164cc64346c1652d23abc68` |
| [STORY-SITE-002](../userstory/STORY-SITE-002.md) | `b0396b37ec222ac4488de5142cb194854361eb131a67667022065398a22e6ab0` |
| [STORY-SITE-003](../userstory/STORY-SITE-003.md) | `12ec2c127ab68f22d5eeb30e61f6d8141923a2e919004b708bc1eb4004f8b6b7` |
| [STORY-SITE-004](../userstory/STORY-SITE-004.md) | `d7e4fd81ffe4527537890ccb4165a714cc638fb8cdf2e8b1ab6c50b19c351bfe` |
| [BR-SITE-001](../businessrule/BR-SITE-001.md) | `ac2548d2cc9ca53bb1dffb5b8dd0094c0b6623900518e94767878e4c25527343` |
| [BR-SITE-002](../businessrule/BR-SITE-002.md) | `66e38d16f476a642c4c099adfd03b2c8a074b49a0b3a082a8c53e0bd465c52c2` |
| [BR-SITE-003](../businessrule/BR-SITE-003.md) | `b582b3be862953202222f1cff2e72904f47770674a1553ff139d291dc9868f5f` |
| [BR-SITE-004](../businessrule/BR-SITE-004.md) | `6e9c8e520bf3174926fc16a6578df6c229471fc5241b3a1212cc3b27374cb0a6` |
| [BR-SITE-005](../businessrule/BR-SITE-005.md) | `5d88cd878b68fbde774479079e5721732d2c3958c7331973a9b14f0a29846c94` |
| [BR-SITE-006](../businessrule/BR-SITE-006.md) | `7495d9d30afe669447d351acd39fd7fca7f1dc98850d1a628aeedf28dcf1a735` |
| [BR-SITE-007](../businessrule/BR-SITE-007.md) | `89eb0c8b86d3dd6ea1fa1ebe5f689ec4abcbecf2aedc476d64fe5b3243b04e1c` |
| [STORY-PROJ-007](../userstory/STORY-PROJ-007.md) | `aebf79786bded381fdcae01e52a24783a847acb0f038315a8fd205694eb5b277` |
| [BR-PROJ-009](../businessrule/BR-PROJ-009.md) | `b801c557431fc72a5a6a90d55f588950b3c551e1f33911fe0d9c50229f0e2c13` |
| [STORY-MEDIA-001](../userstory/STORY-MEDIA-001.md) | `c73dcff5bc6ffef25cc26430f3f9798e21f3a036e30ba88a11783045b44a69e6` |
| [BR-MEDIA-001](../businessrule/BR-MEDIA-001.md) | `056904a23caa5f61d78ab9a85b96a5a11f507ba449805a2be9245a2eb57244f9` |
| [BR-PROJ-004](../businessrule/BR-PROJ-004.md) | `9830fef22e3a4eb73904ce0f1a0ba7c0e69123fcc27c0ef69f70751ff9a11b3b` |

## Các đặc tả mới

Mỗi file có dữ liệu, bước thực hiện, kết quả mong đợi và liên kết về yêu cầu. Cần có implementation, hợp đồng API và môi trường thử phù hợp trước khi thực thi.

| Ca | Story | Nội dung |
|---|---|---|
| [ST-SITE-038](../systemtest/ST-SITE-038.md) | STORY-SITE-001 | Tạo hồ sơ đầy đủ không nguồn và không tệp |
| [ST-SITE-039](../systemtest/ST-SITE-039.md) | STORY-SITE-001 | Diện tích đất và các biên độ chính xác |
| [ST-SITE-040](../systemtest/ST-SITE-040.md) | STORY-SITE-001 | Ngân sách một số nguyên VND và định dạng |
| [ST-SITE-041](../systemtest/ST-SITE-041.md) | STORY-SITE-001 | Các trường mới bắt buộc khi áp dụng |
| [ST-SITE-042](../systemtest/ST-SITE-042.md) | STORY-SITE-001 | Bốn lựa chọn khởi công, không tự đổi theo thời gian |
| [ST-SITE-043](../systemtest/ST-SITE-043.md) | STORY-SITE-001 | Không áp dụng và Không tum là hai tình huống khác |
| [ST-SITE-044](../systemtest/ST-SITE-044.md) | STORY-SITE-001 | Phân loại bắt buộc, đúng nhóm và đúng loại |
| [ST-SITE-045](../systemtest/ST-SITE-045.md) | STORY-SITE-001 | Tạo từ dự toán hoàn tất giữ đúng nguồn |
| [ST-SITE-046](../systemtest/ST-SITE-046.md) | STORY-SITE-001 | Lọc và kiểm lại điều kiện dự toán nguồn |
| [ST-SITE-047](../systemtest/ST-SITE-047.md) | STORY-SITE-001 | Đổi và bỏ nguồn trước khi lưu |
| [ST-SITE-048](../systemtest/ST-SITE-048.md) | STORY-SITE-001 | Không giả mạo dữ liệu nguồn qua API |
| [ST-SITE-049](../systemtest/ST-SITE-049.md) | STORY-SITE-001 | Nguồn và trường nguồn luôn khóa sau khi tạo |
| [ST-SITE-050](../systemtest/ST-SITE-050.md) | STORY-SITE-001 | Không thêm nguồn sau khi tạo độc lập |
| [ST-SITE-051](../systemtest/ST-SITE-051.md) | STORY-SITE-001 | Hai phiên không chiếm cùng dự toán |
| [ST-SITE-052](../systemtest/ST-SITE-052.md) | STORY-SITE-001 | Giữ cấu hình khi Admin cập nhật danh mục |
| [ST-SITE-053](../systemtest/ST-SITE-053.md) | STORY-SITE-001 | Đổi loại chỉ trên công trình độc lập và trong cấu hình đã giữ |
| [ST-SITE-054](../systemtest/ST-SITE-054.md) | STORY-SITE-001 | Địa chỉ ba phần hợp lệ và lưu đồng bộ |
| [ST-SITE-055](../systemtest/ST-SITE-055.md) | STORY-SITE-001 | Lỗi tạo không chiếm nguồn |
| [ST-SITE-056](../systemtest/ST-SITE-056.md) | STORY-SITE-001 | Không lưu tọa độ của địa chỉ cũ do phản hồi đến muộn |
| [ST-SITE-057](../systemtest/ST-SITE-057.md) | STORY-SITE-003 | Danh mục hiện trạng ban đầu |
| [ST-SITE-058](../systemtest/ST-SITE-058.md) | STORY-SITE-003 | Thêm và sắp xếp hiện trạng |
| [ST-SITE-059](../systemtest/ST-SITE-059.md) | STORY-SITE-003 | Đổi tên hiện trạng không viết lại hồ sơ |
| [ST-SITE-060](../systemtest/ST-SITE-060.md) | STORY-SITE-003 | Ngừng cho chọn, giữ hồ sơ cũ |
| [ST-SITE-061](../systemtest/ST-SITE-061.md) | STORY-SITE-003 | Không xóa mục hiện trạng đang được dùng |
| [ST-SITE-062](../systemtest/ST-SITE-062.md) | STORY-SITE-003 | Quyền Admin cho mọi thay đổi danh mục |
| [ST-SITE-063](../systemtest/ST-SITE-063.md) | STORY-SITE-003 | Tên hiện trạng không được rỗng |
| [ST-SITE-064](../systemtest/ST-SITE-064.md) | STORY-SITE-002 | Chi tiết hồ sơ mở rộng theo phạm vi nhân viên |
| [ST-SITE-065](../systemtest/ST-SITE-065.md) | STORY-SITE-002 | Nhân viên không quản lý tệp thay khách |
| [ST-SITE-066](../systemtest/ST-SITE-066.md) | STORY-SITE-004 | Nhận đúng định dạng từng nhóm |
| [ST-SITE-067](../systemtest/ST-SITE-067.md) | STORY-SITE-004 | Từ chối sai nhóm và giả phần mở rộng |
| [ST-SITE-068](../systemtest/ST-SITE-068.md) | STORY-SITE-004 | Giới hạn 9 tệp tính chung hai nhóm |
| [ST-SITE-069](../systemtest/ST-SITE-069.md) | STORY-SITE-004 | Biên dung lượng 10 MB theo bytes thực |
| [ST-SITE-070](../systemtest/ST-SITE-070.md) | STORY-SITE-004 | Gói giữ chỗ khóa cả thêm, thay và xóa tệp |
| [ST-SITE-071](../systemtest/ST-SITE-071.md) | STORY-SITE-004 | Gỡ/hủy gói mở quản lý tệp nhưng không mở dữ liệu nguồn |
| [ST-SITE-072](../systemtest/ST-SITE-072.md) | STORY-SITE-004 | Chủ và nhân viên có quyền được đọc file riêng tư |
| [ST-SITE-073](../systemtest/ST-SITE-073.md) | STORY-SITE-004 | Có đường dẫn nhưng không có quyền thì không đọc được |
| [ST-SITE-074](../systemtest/ST-SITE-074.md) | STORY-SITE-004 | Thu hồi quyền đọc mới sau chuyển giao, gỡ hoặc hủy |
| [ST-SITE-075](../systemtest/ST-SITE-075.md) | STORY-SITE-004 | Thay file thất bại giữ bản cũ |
| [ST-SITE-076](../systemtest/ST-SITE-076.md) | STORY-SITE-004 | Nguồn không tự sao file và không chiếm hạn mức |
| [ST-SITE-077](../systemtest/ST-SITE-077.md) | STORY-SITE-004 | Hai phiên thêm file không vượt tổng 9 |
| [ST-SITE-078](../systemtest/ST-SITE-078.md) | STORY-SITE-004 | Gói gắn trong khi đang upload vẫn khóa lúc lưu |
| [ST-SITE-079](../systemtest/ST-SITE-079.md) | STORY-SITE-004 | Không lấy file công trình khác bằng cách sửa mã |
| [ST-SITE-080](../systemtest/ST-SITE-080.md) | STORY-SITE-004 | Gỡ file hoặc xóa công trình ngừng đọc mới |
| [ST-SITE-081](../systemtest/ST-SITE-081.md) | STORY-SITE-004 | Thay file khi đã đủ 9 không thành thêm file thứ mười |
| [ST-SITE-082](../systemtest/ST-SITE-082.md) | STORY-SITE-004 | Tạo hồ sơ có file lỗi không lưu một phần |
| [ST-PROJ-110](../systemtest/ST-PROJ-110.md) | STORY-PROJ-007 | Chặn xóa nguồn trong lô có nhiều kết quả |
| [ST-PROJ-111](../systemtest/ST-PROJ-111.md) | STORY-PROJ-007 | Xóa công trình hợp lệ giải phóng nguồn |
| [ST-PROJ-112](../systemtest/ST-PROJ-112.md) | STORY-PROJ-007 | Nguồn được dùng lại sau xóa công trình |
| [ST-PROJ-113](../systemtest/ST-PROJ-113.md) | STORY-PROJ-007 | Tạo công trình thắng trước thì không xóa nguồn |
| [ST-PROJ-114](../systemtest/ST-PROJ-114.md) | STORY-PROJ-007 | Xóa nguồn thắng trước thì không tạo công trình |
| [ST-SITE-083](../systemtest/ST-SITE-083.md) | STORY-SITE-001 | Cấu hình hiện hành không có loại; nguồn cũ giữ cấu hình hợp lệ |

## Các đặc tả hiện có được điều chỉnh

- SITE: bổ sung dữ liệu nền đầy đủ để các lỗi thiếu thông tin mới không che mất mục tiêu test cũ; làm rõ tọa độ, cấu hình lưu theo hồ sơ và khóa các trường mới khi đã gán/hoàn tất gói.
- PROJ: các ca xóa cũ dùng dự toán chưa bị công trình tham chiếu; các ca mới kiểm riêng điều kiện nguồn để không làm sai kỳ vọng trước đây.
- MEDIA: ST-MEDIA-011 kiểm tệp công khai có purpose Image; không áp dụng quyền đọc công khai đó cho tệp công trình.

| Nhóm | Các file đã cập nhật |
|---|---|
| SITE | [ST-SITE-001](../systemtest/ST-SITE-001.md), [ST-SITE-002](../systemtest/ST-SITE-002.md), [ST-SITE-003](../systemtest/ST-SITE-003.md), [ST-SITE-004](../systemtest/ST-SITE-004.md), [ST-SITE-005](../systemtest/ST-SITE-005.md), [ST-SITE-006](../systemtest/ST-SITE-006.md), [ST-SITE-007](../systemtest/ST-SITE-007.md), [ST-SITE-008](../systemtest/ST-SITE-008.md), [ST-SITE-009](../systemtest/ST-SITE-009.md), [ST-SITE-010](../systemtest/ST-SITE-010.md), [ST-SITE-011](../systemtest/ST-SITE-011.md), [ST-SITE-012](../systemtest/ST-SITE-012.md), [ST-SITE-013](../systemtest/ST-SITE-013.md), [ST-SITE-014](../systemtest/ST-SITE-014.md), [ST-SITE-015](../systemtest/ST-SITE-015.md), [ST-SITE-016](../systemtest/ST-SITE-016.md), [ST-SITE-017](../systemtest/ST-SITE-017.md), [ST-SITE-018](../systemtest/ST-SITE-018.md), [ST-SITE-019](../systemtest/ST-SITE-019.md), [ST-SITE-020](../systemtest/ST-SITE-020.md), [ST-SITE-021](../systemtest/ST-SITE-021.md), [ST-SITE-022](../systemtest/ST-SITE-022.md), [ST-SITE-023](../systemtest/ST-SITE-023.md), [ST-SITE-024](../systemtest/ST-SITE-024.md), [ST-SITE-026](../systemtest/ST-SITE-026.md), [ST-SITE-027](../systemtest/ST-SITE-027.md), [ST-SITE-028](../systemtest/ST-SITE-028.md), [ST-SITE-029](../systemtest/ST-SITE-029.md), [ST-SITE-030](../systemtest/ST-SITE-030.md), [ST-SITE-031](../systemtest/ST-SITE-031.md), [ST-SITE-032](../systemtest/ST-SITE-032.md), [ST-SITE-033](../systemtest/ST-SITE-033.md), [ST-SITE-034](../systemtest/ST-SITE-034.md), [ST-SITE-035](../systemtest/ST-SITE-035.md), [ST-SITE-036](../systemtest/ST-SITE-036.md), [ST-SITE-037](../systemtest/ST-SITE-037.md) |
| PROJ | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md), [ST-PROJ-100](../systemtest/ST-PROJ-100.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-108](../systemtest/ST-PROJ-108.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| MEDIA | [ST-MEDIA-011](../systemtest/ST-MEDIA-011.md) |

## Đối chiếu tiêu chí nghiệm thu

Có **93/93 AC đang áp dụng** trong sáu Story dưới đây có ít nhất một đặc tả liên kết, gồm 72 AC của bốn Story SITE và 21 AC kế thừa của PROJ/MEDIA. Đây là độ phủ ở mức đặc tả và truy vết; không phải tỷ lệ test chạy đạt hoặc độ phủ code. Với PROJ/MEDIA, bảng giữ cả các ca hiện có để đối chiếu phần liên quan, không khẳng định đã rà soát lại toàn bộ hai module.

| Yêu cầu | Đặc tả liên quan |
|---|---|
| [STORY-SITE-001/AC-001](../userstory/STORY-SITE-001.md#ac-001) | [ST-SITE-001](../systemtest/ST-SITE-001.md) |
| [STORY-SITE-001/AC-002](../userstory/STORY-SITE-001.md#ac-002) | [ST-SITE-002](../systemtest/ST-SITE-002.md) |
| [STORY-SITE-001/AC-003](../userstory/STORY-SITE-001.md#ac-003) | [ST-SITE-003](../systemtest/ST-SITE-003.md) |
| [STORY-SITE-001/AC-004](../userstory/STORY-SITE-001.md#ac-004) | [ST-SITE-004](../systemtest/ST-SITE-004.md) |
| [STORY-SITE-001/AC-005](../userstory/STORY-SITE-001.md#ac-005) | [ST-SITE-005](../systemtest/ST-SITE-005.md) |
| [STORY-SITE-001/AC-006](../userstory/STORY-SITE-001.md#ac-006) | [ST-SITE-006](../systemtest/ST-SITE-006.md) |
| [STORY-SITE-001/AC-007](../userstory/STORY-SITE-001.md#ac-007) | [ST-SITE-007](../systemtest/ST-SITE-007.md) |
| [STORY-SITE-001/AC-008](../userstory/STORY-SITE-001.md#ac-008) | [ST-SITE-008](../systemtest/ST-SITE-008.md) |
| [STORY-SITE-001/AC-009](../userstory/STORY-SITE-001.md#ac-009) | [ST-SITE-009](../systemtest/ST-SITE-009.md) |
| [STORY-SITE-001/AC-010](../userstory/STORY-SITE-001.md#ac-010) | [ST-SITE-010](../systemtest/ST-SITE-010.md) |
| [STORY-SITE-001/AC-011](../userstory/STORY-SITE-001.md#ac-011) | [ST-SITE-011](../systemtest/ST-SITE-011.md), [ST-SITE-037](../systemtest/ST-SITE-037.md) |
| [STORY-SITE-001/AC-012](../userstory/STORY-SITE-001.md#ac-012) | [ST-SITE-012](../systemtest/ST-SITE-012.md) |
| [STORY-SITE-001/AC-013](../userstory/STORY-SITE-001.md#ac-013) | [ST-SITE-013](../systemtest/ST-SITE-013.md) |
| [STORY-SITE-001/AC-014](../userstory/STORY-SITE-001.md#ac-014) | [ST-SITE-014](../systemtest/ST-SITE-014.md) |
| [STORY-SITE-001/AC-015](../userstory/STORY-SITE-001.md#ac-015) | [ST-SITE-015](../systemtest/ST-SITE-015.md) |
| [STORY-SITE-001/AC-016](../userstory/STORY-SITE-001.md#ac-016) | [ST-SITE-016](../systemtest/ST-SITE-016.md), [ST-SITE-037](../systemtest/ST-SITE-037.md) |
| [STORY-SITE-001/AC-017](../userstory/STORY-SITE-001.md#ac-017) | [ST-SITE-030](../systemtest/ST-SITE-030.md) |
| [STORY-SITE-001/AC-018](../userstory/STORY-SITE-001.md#ac-018) | [ST-SITE-033](../systemtest/ST-SITE-033.md), [ST-SITE-056](../systemtest/ST-SITE-056.md) |
| [STORY-SITE-001/AC-019](../userstory/STORY-SITE-001.md#ac-019) | [ST-SITE-034](../systemtest/ST-SITE-034.md) |
| [STORY-SITE-001/AC-020](../userstory/STORY-SITE-001.md#ac-020) | [ST-SITE-035](../systemtest/ST-SITE-035.md), [ST-SITE-056](../systemtest/ST-SITE-056.md) |
| [STORY-SITE-001/AC-021](../userstory/STORY-SITE-001.md#ac-021) | [ST-SITE-036](../systemtest/ST-SITE-036.md) |
| [STORY-SITE-001/AC-022](../userstory/STORY-SITE-001.md#ac-022) | [ST-SITE-038](../systemtest/ST-SITE-038.md) |
| [STORY-SITE-001/AC-023](../userstory/STORY-SITE-001.md#ac-023) | [ST-SITE-039](../systemtest/ST-SITE-039.md) |
| [STORY-SITE-001/AC-024](../userstory/STORY-SITE-001.md#ac-024) | [ST-SITE-040](../systemtest/ST-SITE-040.md) |
| [STORY-SITE-001/AC-025](../userstory/STORY-SITE-001.md#ac-025) | [ST-SITE-041](../systemtest/ST-SITE-041.md), [ST-SITE-042](../systemtest/ST-SITE-042.md) |
| [STORY-SITE-001/AC-026](../userstory/STORY-SITE-001.md#ac-026) | [ST-SITE-043](../systemtest/ST-SITE-043.md) |
| [STORY-SITE-001/AC-027](../userstory/STORY-SITE-001.md#ac-027) | [ST-SITE-043](../systemtest/ST-SITE-043.md), [ST-SITE-044](../systemtest/ST-SITE-044.md), [ST-SITE-053](../systemtest/ST-SITE-053.md), [ST-SITE-083](../systemtest/ST-SITE-083.md) |
| [STORY-SITE-001/AC-028](../userstory/STORY-SITE-001.md#ac-028) | [ST-SITE-045](../systemtest/ST-SITE-045.md), [ST-SITE-048](../systemtest/ST-SITE-048.md), [ST-SITE-055](../systemtest/ST-SITE-055.md) |
| [STORY-SITE-001/AC-029](../userstory/STORY-SITE-001.md#ac-029) | [ST-SITE-046](../systemtest/ST-SITE-046.md) |
| [STORY-SITE-001/AC-030](../userstory/STORY-SITE-001.md#ac-030) | [ST-SITE-047](../systemtest/ST-SITE-047.md) |
| [STORY-SITE-001/AC-031](../userstory/STORY-SITE-001.md#ac-031) | [ST-SITE-048](../systemtest/ST-SITE-048.md), [ST-SITE-049](../systemtest/ST-SITE-049.md) |
| [STORY-SITE-001/AC-032](../userstory/STORY-SITE-001.md#ac-032) | [ST-SITE-050](../systemtest/ST-SITE-050.md) |
| [STORY-SITE-001/AC-033](../userstory/STORY-SITE-001.md#ac-033) | [ST-SITE-051](../systemtest/ST-SITE-051.md) |
| [STORY-SITE-001/AC-034](../userstory/STORY-SITE-001.md#ac-034) | [ST-PROJ-110](../systemtest/ST-PROJ-110.md), [ST-PROJ-111](../systemtest/ST-PROJ-111.md), [ST-PROJ-112](../systemtest/ST-PROJ-112.md), [ST-SITE-055](../systemtest/ST-SITE-055.md) |
| [STORY-SITE-001/AC-035](../userstory/STORY-SITE-001.md#ac-035) | [ST-SITE-052](../systemtest/ST-SITE-052.md), [ST-SITE-053](../systemtest/ST-SITE-053.md), [ST-SITE-083](../systemtest/ST-SITE-083.md) |
| [STORY-SITE-001/AC-036](../userstory/STORY-SITE-001.md#ac-036) | [ST-SITE-060](../systemtest/ST-SITE-060.md) |
| [STORY-SITE-001/AC-037](../userstory/STORY-SITE-001.md#ac-037) | [ST-SITE-054](../systemtest/ST-SITE-054.md) |
| [STORY-SITE-001/AC-038](../userstory/STORY-SITE-001.md#ac-038) | [ST-SITE-042](../systemtest/ST-SITE-042.md) |
| [STORY-SITE-002/AC-001](../userstory/STORY-SITE-002.md#ac-001) | [ST-SITE-021](../systemtest/ST-SITE-021.md) |
| [STORY-SITE-002/AC-002](../userstory/STORY-SITE-002.md#ac-002) | [ST-SITE-022](../systemtest/ST-SITE-022.md) |
| [STORY-SITE-002/AC-003](../userstory/STORY-SITE-002.md#ac-003) | [ST-SITE-023](../systemtest/ST-SITE-023.md) |
| [STORY-SITE-002/AC-004](../userstory/STORY-SITE-002.md#ac-004) | [ST-SITE-024](../systemtest/ST-SITE-024.md) |
| [STORY-SITE-002/AC-006](../userstory/STORY-SITE-002.md#ac-006) | [ST-SITE-026](../systemtest/ST-SITE-026.md) |
| [STORY-SITE-002/AC-007](../userstory/STORY-SITE-002.md#ac-007) | [ST-SITE-027](../systemtest/ST-SITE-027.md) |
| [STORY-SITE-002/AC-008](../userstory/STORY-SITE-002.md#ac-008) | [ST-SITE-028](../systemtest/ST-SITE-028.md) |
| [STORY-SITE-002/AC-009](../userstory/STORY-SITE-002.md#ac-009) | [ST-SITE-029](../systemtest/ST-SITE-029.md) |
| [STORY-SITE-002/AC-010](../userstory/STORY-SITE-002.md#ac-010) | [ST-SITE-031](../systemtest/ST-SITE-031.md) |
| [STORY-SITE-002/AC-011](../userstory/STORY-SITE-002.md#ac-011) | [ST-SITE-064](../systemtest/ST-SITE-064.md) |
| [STORY-SITE-002/AC-012](../userstory/STORY-SITE-002.md#ac-012) | [ST-SITE-074](../systemtest/ST-SITE-074.md) |
| [STORY-SITE-002/AC-013](../userstory/STORY-SITE-002.md#ac-013) | [ST-SITE-065](../systemtest/ST-SITE-065.md) |
| [STORY-SITE-003/AC-001](../userstory/STORY-SITE-003.md#ac-001) | [ST-SITE-057](../systemtest/ST-SITE-057.md) |
| [STORY-SITE-003/AC-002](../userstory/STORY-SITE-003.md#ac-002) | [ST-SITE-058](../systemtest/ST-SITE-058.md) |
| [STORY-SITE-003/AC-003](../userstory/STORY-SITE-003.md#ac-003) | [ST-SITE-059](../systemtest/ST-SITE-059.md) |
| [STORY-SITE-003/AC-004](../userstory/STORY-SITE-003.md#ac-004) | [ST-SITE-060](../systemtest/ST-SITE-060.md) |
| [STORY-SITE-003/AC-005](../userstory/STORY-SITE-003.md#ac-005) | [ST-SITE-061](../systemtest/ST-SITE-061.md) |
| [STORY-SITE-003/AC-006](../userstory/STORY-SITE-003.md#ac-006) | [ST-SITE-062](../systemtest/ST-SITE-062.md) |
| [STORY-SITE-003/AC-007](../userstory/STORY-SITE-003.md#ac-007) | [ST-SITE-063](../systemtest/ST-SITE-063.md) |
| [STORY-SITE-004/AC-001](../userstory/STORY-SITE-004.md#ac-001) | [ST-SITE-038](../systemtest/ST-SITE-038.md), [ST-SITE-082](../systemtest/ST-SITE-082.md) |
| [STORY-SITE-004/AC-002](../userstory/STORY-SITE-004.md#ac-002) | [ST-SITE-066](../systemtest/ST-SITE-066.md), [ST-SITE-067](../systemtest/ST-SITE-067.md) |
| [STORY-SITE-004/AC-003](../userstory/STORY-SITE-004.md#ac-003) | [ST-SITE-068](../systemtest/ST-SITE-068.md), [ST-SITE-081](../systemtest/ST-SITE-081.md) |
| [STORY-SITE-004/AC-004](../userstory/STORY-SITE-004.md#ac-004) | [ST-SITE-069](../systemtest/ST-SITE-069.md) |
| [STORY-SITE-004/AC-005](../userstory/STORY-SITE-004.md#ac-005) | [ST-SITE-070](../systemtest/ST-SITE-070.md) |
| [STORY-SITE-004/AC-006](../userstory/STORY-SITE-004.md#ac-006) | [ST-SITE-071](../systemtest/ST-SITE-071.md) |
| [STORY-SITE-004/AC-007](../userstory/STORY-SITE-004.md#ac-007) | [ST-SITE-072](../systemtest/ST-SITE-072.md) |
| [STORY-SITE-004/AC-008](../userstory/STORY-SITE-004.md#ac-008) | [ST-SITE-073](../systemtest/ST-SITE-073.md) |
| [STORY-SITE-004/AC-009](../userstory/STORY-SITE-004.md#ac-009) | [ST-SITE-074](../systemtest/ST-SITE-074.md) |
| [STORY-SITE-004/AC-010](../userstory/STORY-SITE-004.md#ac-010) | [ST-SITE-075](../systemtest/ST-SITE-075.md), [ST-SITE-081](../systemtest/ST-SITE-081.md), [ST-SITE-082](../systemtest/ST-SITE-082.md) |
| [STORY-SITE-004/AC-011](../userstory/STORY-SITE-004.md#ac-011) | [ST-SITE-045](../systemtest/ST-SITE-045.md), [ST-SITE-076](../systemtest/ST-SITE-076.md) |
| [STORY-SITE-004/AC-012](../userstory/STORY-SITE-004.md#ac-012) | [ST-SITE-077](../systemtest/ST-SITE-077.md) |
| [STORY-SITE-004/AC-013](../userstory/STORY-SITE-004.md#ac-013) | [ST-SITE-078](../systemtest/ST-SITE-078.md) |
| [STORY-SITE-004/AC-014](../userstory/STORY-SITE-004.md#ac-014) | [ST-SITE-079](../systemtest/ST-SITE-079.md) |
| [STORY-SITE-004/AC-015](../userstory/STORY-SITE-004.md#ac-015) | [ST-SITE-080](../systemtest/ST-SITE-080.md) |
| [STORY-PROJ-007/AC-001](../userstory/STORY-PROJ-007.md#ac-001) | [ST-PROJ-094](../systemtest/ST-PROJ-094.md) |
| [STORY-PROJ-007/AC-002](../userstory/STORY-PROJ-007.md#ac-002) | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md) |
| [STORY-PROJ-007/AC-003](../userstory/STORY-PROJ-007.md#ac-003) | [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| [STORY-PROJ-007/AC-004](../userstory/STORY-PROJ-007.md#ac-004) | [ST-PROJ-096](../systemtest/ST-PROJ-096.md) |
| [STORY-PROJ-007/AC-005](../userstory/STORY-PROJ-007.md#ac-005) | [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| [STORY-PROJ-007/AC-006](../userstory/STORY-PROJ-007.md#ac-006) | [ST-PROJ-100](../systemtest/ST-PROJ-100.md) |
| [STORY-PROJ-007/AC-007](../userstory/STORY-PROJ-007.md#ac-007) | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| [STORY-PROJ-007/AC-008](../userstory/STORY-PROJ-007.md#ac-008) | [ST-PROJ-102](../systemtest/ST-PROJ-102.md) |
| [STORY-PROJ-007/AC-009](../userstory/STORY-PROJ-007.md#ac-009) | [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| [STORY-PROJ-007/AC-010](../userstory/STORY-PROJ-007.md#ac-010) | [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md) |
| [STORY-PROJ-007/AC-011](../userstory/STORY-PROJ-007.md#ac-011) | [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md) |
| [STORY-PROJ-007/AC-012](../userstory/STORY-PROJ-007.md#ac-012) | [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |
| [STORY-PROJ-007/AC-013](../userstory/STORY-PROJ-007.md#ac-013) | [ST-PROJ-110](../systemtest/ST-PROJ-110.md) |
| [STORY-PROJ-007/AC-014](../userstory/STORY-PROJ-007.md#ac-014) | [ST-PROJ-111](../systemtest/ST-PROJ-111.md), [ST-PROJ-112](../systemtest/ST-PROJ-112.md) |
| [STORY-PROJ-007/AC-015](../userstory/STORY-PROJ-007.md#ac-015) | [ST-PROJ-113](../systemtest/ST-PROJ-113.md), [ST-PROJ-114](../systemtest/ST-PROJ-114.md) |
| [STORY-MEDIA-001/AC-001](../userstory/STORY-MEDIA-001.md#ac-001) | [ST-MEDIA-001](../systemtest/ST-MEDIA-001.md), [ST-MEDIA-002](../systemtest/ST-MEDIA-002.md), [ST-MEDIA-003](../systemtest/ST-MEDIA-003.md) |
| [STORY-MEDIA-001/AC-002](../userstory/STORY-MEDIA-001.md#ac-002) | [ST-MEDIA-009](../systemtest/ST-MEDIA-009.md), [ST-MEDIA-010](../systemtest/ST-MEDIA-010.md) |
| [STORY-MEDIA-001/AC-003](../userstory/STORY-MEDIA-001.md#ac-003) | [ST-MEDIA-001](../systemtest/ST-MEDIA-001.md), [ST-MEDIA-002](../systemtest/ST-MEDIA-002.md), [ST-MEDIA-003](../systemtest/ST-MEDIA-003.md), [ST-MEDIA-004](../systemtest/ST-MEDIA-004.md), [ST-MEDIA-005](../systemtest/ST-MEDIA-005.md), [ST-MEDIA-006](../systemtest/ST-MEDIA-006.md), [ST-MEDIA-007](../systemtest/ST-MEDIA-007.md), [ST-MEDIA-008](../systemtest/ST-MEDIA-008.md) |
| [STORY-MEDIA-001/AC-004](../userstory/STORY-MEDIA-001.md#ac-004) | [ST-MEDIA-011](../systemtest/ST-MEDIA-011.md) |
| [STORY-MEDIA-001/AC-005](../userstory/STORY-MEDIA-001.md#ac-005) | [ST-MEDIA-012](../systemtest/ST-MEDIA-012.md) |
| [STORY-MEDIA-001/AC-006](../userstory/STORY-MEDIA-001.md#ac-006) | [ST-MEDIA-013](../systemtest/ST-MEDIA-013.md), [ST-MEDIA-014](../systemtest/ST-MEDIA-014.md) |

## Đối chiếu luồng và ràng buộc ngoài chức năng

Các luồng đang áp dụng của bốn Story SITE và phần xóa dự toán đều có liên kết trực tiếp. Quyền được kiểm bằng API và dữ liệu sau thao tác, không chỉ dựa vào việc ẩn nút. Các ca đồng thời phải kiểm cả hai phản hồi và trạng thái cuối trong database; TDD cần cung cấp cách điều phối để tái hiện tranh chấp.

| Luồng hoặc ràng buộc | Đặc tả liên quan |
|---|---|
| STORY-SITE-001/Main Flow | [ST-SITE-001](../systemtest/ST-SITE-001.md), [ST-SITE-038](../systemtest/ST-SITE-038.md), [ST-SITE-043](../systemtest/ST-SITE-043.md) |
| STORY-SITE-001/ALT-01 | [ST-SITE-009](../systemtest/ST-SITE-009.md), [ST-SITE-010](../systemtest/ST-SITE-010.md) |
| STORY-SITE-001/ALT-02 | [ST-SITE-013](../systemtest/ST-SITE-013.md), [ST-SITE-018](../systemtest/ST-SITE-018.md), [ST-SITE-030](../systemtest/ST-SITE-030.md), [ST-SITE-035](../systemtest/ST-SITE-035.md), [ST-SITE-049](../systemtest/ST-SITE-049.md), [ST-SITE-053](../systemtest/ST-SITE-053.md), [ST-SITE-054](../systemtest/ST-SITE-054.md) |
| STORY-SITE-001/ALT-03 | [ST-SITE-014](../systemtest/ST-SITE-014.md), [ST-SITE-015](../systemtest/ST-SITE-015.md), [ST-SITE-017](../systemtest/ST-SITE-017.md) |
| STORY-SITE-001/ALT-04 | [ST-SITE-045](../systemtest/ST-SITE-045.md), [ST-SITE-047](../systemtest/ST-SITE-047.md) |
| STORY-SITE-001/ALT-05 | [ST-SITE-052](../systemtest/ST-SITE-052.md), [ST-SITE-083](../systemtest/ST-SITE-083.md) |
| STORY-SITE-001/EXC-01 | [ST-SITE-003](../systemtest/ST-SITE-003.md), [ST-SITE-005](../systemtest/ST-SITE-005.md), [ST-SITE-018](../systemtest/ST-SITE-018.md) |
| STORY-SITE-001/EXC-02 | [ST-SITE-006](../systemtest/ST-SITE-006.md), [ST-SITE-012](../systemtest/ST-SITE-012.md), [ST-SITE-055](../systemtest/ST-SITE-055.md) |
| STORY-SITE-001/EXC-03 | [ST-SITE-011](../systemtest/ST-SITE-011.md), [ST-SITE-015](../systemtest/ST-SITE-015.md) |
| STORY-SITE-001/EXC-04 | [ST-SITE-016](../systemtest/ST-SITE-016.md) |
| STORY-SITE-001/EXC-05 | [ST-SITE-034](../systemtest/ST-SITE-034.md), [ST-SITE-036](../systemtest/ST-SITE-036.md) |
| STORY-SITE-001/EXC-06 | [ST-SITE-039](../systemtest/ST-SITE-039.md), [ST-SITE-040](../systemtest/ST-SITE-040.md), [ST-SITE-041](../systemtest/ST-SITE-041.md), [ST-SITE-044](../systemtest/ST-SITE-044.md), [ST-SITE-053](../systemtest/ST-SITE-053.md), [ST-SITE-054](../systemtest/ST-SITE-054.md), [ST-SITE-055](../systemtest/ST-SITE-055.md) |
| STORY-SITE-001/EXC-07 | [ST-PROJ-114](../systemtest/ST-PROJ-114.md), [ST-SITE-046](../systemtest/ST-SITE-046.md), [ST-SITE-048](../systemtest/ST-SITE-048.md), [ST-SITE-049](../systemtest/ST-SITE-049.md), [ST-SITE-050](../systemtest/ST-SITE-050.md) |
| STORY-SITE-001/Non-Functional | [ST-SITE-019](../systemtest/ST-SITE-019.md), [ST-SITE-020](../systemtest/ST-SITE-020.md), [ST-SITE-032](../systemtest/ST-SITE-032.md), [ST-SITE-051](../systemtest/ST-SITE-051.md), [ST-SITE-056](../systemtest/ST-SITE-056.md) |
| STORY-SITE-002/Main Flow | [ST-SITE-021](../systemtest/ST-SITE-021.md), [ST-SITE-022](../systemtest/ST-SITE-022.md) |
| STORY-SITE-002/ALT-01 | [ST-SITE-023](../systemtest/ST-SITE-023.md), [ST-SITE-026](../systemtest/ST-SITE-026.md) |
| STORY-SITE-002/ALT-03 | [ST-SITE-031](../systemtest/ST-SITE-031.md), [ST-SITE-074](../systemtest/ST-SITE-074.md) |
| STORY-SITE-002/ALT-04 | [ST-SITE-064](../systemtest/ST-SITE-064.md), [ST-SITE-065](../systemtest/ST-SITE-065.md), [ST-SITE-074](../systemtest/ST-SITE-074.md) |
| STORY-SITE-002/EXC-01 | [ST-SITE-024](../systemtest/ST-SITE-024.md) |
| STORY-SITE-002/EXC-02 | [ST-SITE-027](../systemtest/ST-SITE-027.md) |
| STORY-SITE-002/EXC-03 | [ST-SITE-029](../systemtest/ST-SITE-029.md), [ST-SITE-065](../systemtest/ST-SITE-065.md) |
| STORY-SITE-002/Non-Functional | [ST-SITE-026](../systemtest/ST-SITE-026.md) |
| STORY-SITE-003/Main Flow | [ST-SITE-057](../systemtest/ST-SITE-057.md), [ST-SITE-058](../systemtest/ST-SITE-058.md) |
| STORY-SITE-003/ALT-01 | [ST-SITE-060](../systemtest/ST-SITE-060.md) |
| STORY-SITE-003/ALT-02 | [ST-SITE-059](../systemtest/ST-SITE-059.md) |
| STORY-SITE-003/EXC-01 | [ST-SITE-061](../systemtest/ST-SITE-061.md), [ST-SITE-062](../systemtest/ST-SITE-062.md) |
| STORY-SITE-003/EXC-02 | [ST-SITE-063](../systemtest/ST-SITE-063.md) |
| STORY-SITE-003/Non-Functional | [ST-SITE-059](../systemtest/ST-SITE-059.md), [ST-SITE-062](../systemtest/ST-SITE-062.md) |
| STORY-SITE-004/Main Flow | [ST-SITE-066](../systemtest/ST-SITE-066.md), [ST-SITE-072](../systemtest/ST-SITE-072.md) |
| STORY-SITE-004/ALT-01 | [ST-SITE-038](../systemtest/ST-SITE-038.md) |
| STORY-SITE-004/ALT-02 | [ST-SITE-071](../systemtest/ST-SITE-071.md), [ST-SITE-075](../systemtest/ST-SITE-075.md), [ST-SITE-080](../systemtest/ST-SITE-080.md), [ST-SITE-081](../systemtest/ST-SITE-081.md) |
| STORY-SITE-004/ALT-03 | [ST-SITE-076](../systemtest/ST-SITE-076.md) |
| STORY-SITE-004/EXC-01 | [ST-SITE-067](../systemtest/ST-SITE-067.md), [ST-SITE-068](../systemtest/ST-SITE-068.md), [ST-SITE-069](../systemtest/ST-SITE-069.md) |
| STORY-SITE-004/EXC-02 | [ST-SITE-070](../systemtest/ST-SITE-070.md), [ST-SITE-078](../systemtest/ST-SITE-078.md) |
| STORY-SITE-004/EXC-03 | [ST-SITE-073](../systemtest/ST-SITE-073.md), [ST-SITE-074](../systemtest/ST-SITE-074.md), [ST-SITE-079](../systemtest/ST-SITE-079.md) |
| STORY-SITE-004/EXC-04 | [ST-SITE-075](../systemtest/ST-SITE-075.md), [ST-SITE-082](../systemtest/ST-SITE-082.md) |
| STORY-SITE-004/Non-Functional | [ST-SITE-077](../systemtest/ST-SITE-077.md) |
| STORY-PROJ-007/Main Flow | [ST-PROJ-093](../systemtest/ST-PROJ-093.md), [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md) |
| STORY-PROJ-007/ALT-01 | [ST-PROJ-095](../systemtest/ST-PROJ-095.md) |
| STORY-PROJ-007/ALT-02 | [ST-PROJ-097](../systemtest/ST-PROJ-097.md), [ST-PROJ-098](../systemtest/ST-PROJ-098.md) |
| STORY-PROJ-007/ALT-03 | [ST-PROJ-096](../systemtest/ST-PROJ-096.md), [ST-PROJ-099](../systemtest/ST-PROJ-099.md) |
| STORY-PROJ-007/ALT-04 | [ST-PROJ-110](../systemtest/ST-PROJ-110.md), [ST-PROJ-111](../systemtest/ST-PROJ-111.md), [ST-PROJ-112](../systemtest/ST-PROJ-112.md), [ST-PROJ-113](../systemtest/ST-PROJ-113.md), [ST-PROJ-114](../systemtest/ST-PROJ-114.md) |
| STORY-PROJ-007/EXC-01 | [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md) |
| STORY-PROJ-007/EXC-02 | [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md) |
| STORY-PROJ-007/EXC-03 | [ST-PROJ-106](../systemtest/ST-PROJ-106.md) |
| STORY-PROJ-007/EXC-04 | [ST-PROJ-108](../systemtest/ST-PROJ-108.md) |
| STORY-PROJ-007/Non-Functional | [ST-PROJ-094](../systemtest/ST-PROJ-094.md), [ST-PROJ-095](../systemtest/ST-PROJ-095.md), [ST-PROJ-100](../systemtest/ST-PROJ-100.md), [ST-PROJ-101](../systemtest/ST-PROJ-101.md), [ST-PROJ-102](../systemtest/ST-PROJ-102.md), [ST-PROJ-103](../systemtest/ST-PROJ-103.md), [ST-PROJ-104](../systemtest/ST-PROJ-104.md), [ST-PROJ-105](../systemtest/ST-PROJ-105.md), [ST-PROJ-106](../systemtest/ST-PROJ-106.md), [ST-PROJ-107](../systemtest/ST-PROJ-107.md), [ST-PROJ-108](../systemtest/ST-PROJ-108.md), [ST-PROJ-109](../systemtest/ST-PROJ-109.md), [ST-PROJ-113](../systemtest/ST-PROJ-113.md), [ST-PROJ-114](../systemtest/ST-PROJ-114.md) |

MEDIA trong đợt này chỉ điều chỉnh phạm vi đọc tệp công khai ở AC-004 và kiểm tệp công trình bằng STORY-SITE-004. Các liên kết trực tiếp tới Main Flow và ALT-01 của STORY-MEDIA-001 còn thiếu trong bộ ST hiện có; không tuyên bố đã hoàn tất truy vết mọi luồng MEDIA.

## Đối chiếu các nhóm quy tắc

| Quy tắc | Điều cần chứng minh | Các ca tiêu biểu |
|---|---|---|
| [BR-SITE-001](../businessrule/BR-SITE-001.md) | Hồ sơ đầy đủ; diện tích đất, VND, khởi công, địa chỉ và thông tin bắt buộc | [ST-SITE-038](../systemtest/ST-SITE-038.md), [ST-SITE-039](../systemtest/ST-SITE-039.md), [ST-SITE-040](../systemtest/ST-SITE-040.md), [ST-SITE-041](../systemtest/ST-SITE-041.md), [ST-SITE-042](../systemtest/ST-SITE-042.md), [ST-SITE-054](../systemtest/ST-SITE-054.md), [ST-SITE-056](../systemtest/ST-SITE-056.md) |
| [BR-SITE-002](../businessrule/BR-SITE-002.md) | Chủ sở hữu sửa; gói giữ chỗ khóa; gỡ/hủy gói không mở khóa trường nguồn | [ST-SITE-011](../systemtest/ST-SITE-011.md), [ST-SITE-016](../systemtest/ST-SITE-016.md), [ST-SITE-030](../systemtest/ST-SITE-030.md), [ST-SITE-049](../systemtest/ST-SITE-049.md), [ST-SITE-050](../systemtest/ST-SITE-050.md), [ST-SITE-065](../systemtest/ST-SITE-065.md), [ST-SITE-070](../systemtest/ST-SITE-070.md), [ST-SITE-071](../systemtest/ST-SITE-071.md) |
| [BR-SITE-003](../businessrule/BR-SITE-003.md) | Phạm vi đọc của khách/nhân viên; truy cập tệp theo quyền hiện tại | [ST-SITE-021](../systemtest/ST-SITE-021.md), [ST-SITE-022](../systemtest/ST-SITE-022.md), [ST-SITE-023](../systemtest/ST-SITE-023.md), [ST-SITE-024](../systemtest/ST-SITE-024.md), [ST-SITE-026](../systemtest/ST-SITE-026.md), [ST-SITE-027](../systemtest/ST-SITE-027.md), [ST-SITE-028](../systemtest/ST-SITE-028.md), [ST-SITE-064](../systemtest/ST-SITE-064.md), [ST-SITE-072](../systemtest/ST-SITE-072.md), [ST-SITE-073](../systemtest/ST-SITE-073.md), [ST-SITE-074](../systemtest/ST-SITE-074.md) |
| [BR-SITE-004](../businessrule/BR-SITE-004.md) | Nguồn hợp lệ và duy nhất; điền/khóa dữ liệu, không chiếm nguồn sau lỗi | [ST-SITE-045](../systemtest/ST-SITE-045.md), [ST-SITE-046](../systemtest/ST-SITE-046.md), [ST-SITE-047](../systemtest/ST-SITE-047.md), [ST-SITE-048](../systemtest/ST-SITE-048.md), [ST-SITE-049](../systemtest/ST-SITE-049.md), [ST-SITE-050](../systemtest/ST-SITE-050.md), [ST-SITE-051](../systemtest/ST-SITE-051.md), [ST-SITE-055](../systemtest/ST-SITE-055.md), [ST-SITE-076](../systemtest/ST-SITE-076.md) |
| [BR-SITE-005](../businessrule/BR-SITE-005.md) | Cấu hình dùng chung; Không áp dụng; kiểm giá trị theo loại; giữ phiên bản; cấu hình hiện hành rỗng | [ST-SITE-043](../systemtest/ST-SITE-043.md), [ST-SITE-044](../systemtest/ST-SITE-044.md), [ST-SITE-052](../systemtest/ST-SITE-052.md), [ST-SITE-053](../systemtest/ST-SITE-053.md), [ST-SITE-083](../systemtest/ST-SITE-083.md) |
| [BR-SITE-006](../businessrule/BR-SITE-006.md) | Khởi tạo, thêm/sắp xếp/đổi tên/ngừng; bảo toàn hồ sơ; Admin; tên bắt buộc | [ST-SITE-057](../systemtest/ST-SITE-057.md), [ST-SITE-058](../systemtest/ST-SITE-058.md), [ST-SITE-059](../systemtest/ST-SITE-059.md), [ST-SITE-060](../systemtest/ST-SITE-060.md), [ST-SITE-061](../systemtest/ST-SITE-061.md), [ST-SITE-062](../systemtest/ST-SITE-062.md), [ST-SITE-063](../systemtest/ST-SITE-063.md) |
| [BR-SITE-007](../businessrule/BR-SITE-007.md) | Định dạng và dung lượng thật, tổng 9 tệp, quyền đọc/ghi, khóa, lỗi thay tệp, đồng thời và xóa | [ST-SITE-065](../systemtest/ST-SITE-065.md), [ST-SITE-066](../systemtest/ST-SITE-066.md), [ST-SITE-067](../systemtest/ST-SITE-067.md), [ST-SITE-068](../systemtest/ST-SITE-068.md), [ST-SITE-069](../systemtest/ST-SITE-069.md), [ST-SITE-070](../systemtest/ST-SITE-070.md), [ST-SITE-071](../systemtest/ST-SITE-071.md), [ST-SITE-072](../systemtest/ST-SITE-072.md), [ST-SITE-073](../systemtest/ST-SITE-073.md), [ST-SITE-074](../systemtest/ST-SITE-074.md), [ST-SITE-075](../systemtest/ST-SITE-075.md), [ST-SITE-076](../systemtest/ST-SITE-076.md), [ST-SITE-077](../systemtest/ST-SITE-077.md), [ST-SITE-078](../systemtest/ST-SITE-078.md), [ST-SITE-079](../systemtest/ST-SITE-079.md), [ST-SITE-080](../systemtest/ST-SITE-080.md), [ST-SITE-081](../systemtest/ST-SITE-081.md), [ST-SITE-082](../systemtest/ST-SITE-082.md) |
| [BR-PROJ-009](../businessrule/BR-PROJ-009.md) | Không xóa dự toán đang là nguồn; xóa công trình hợp lệ giải phóng nguồn; tranh chấp tạo/xóa | [ST-PROJ-110](../systemtest/ST-PROJ-110.md), [ST-PROJ-111](../systemtest/ST-PROJ-111.md), [ST-PROJ-112](../systemtest/ST-PROJ-112.md), [ST-PROJ-113](../systemtest/ST-PROJ-113.md), [ST-PROJ-114](../systemtest/ST-PROJ-114.md) |

## Phần cần hoàn thiện trước khi chạy

1. Cập nhật thiết kế liên quan: [TDD-SITE-001](../tdd/TDD-SITE-001.md), [TDD-SITE-002](../tdd/TDD-SITE-002.md), [TDD-PROJ-005](../tdd/TDD-PROJ-005.md) và [TDD-MEDIA-001](../tdd/TDD-MEDIA-001.md); bổ sung thiết kế cho danh mục hiện trạng và tệp riêng của công trình. Các TDD hiện có chưa mô tả đầy đủ phần mở rộng vừa chốt.
2. Chốt contract kỹ thuật: request/response, HTTP status/mã lỗi, ánh xạ trường, cách kiểm tệp, lưu trữ riêng, transaction và điểm điều phối các ca đồng thời. ST-SITE-048 chỉ yêu cầu dữ liệu nguồn giả mạo không được lưu; cách từ chối hay lấy lại dữ liệu nguồn cần theo contract cuối cùng.
3. Chuẩn bị implementation, giao diện, database và kho tệp thử; bổ sung đặc tả Unit Test sau bước thiết kế. Chưa chạy migration, sửa mã ứng dụng hoặc thực thi các ca trong lượt soạn tài liệu này.
4. Phân công Owner/người chạy và bổ sung bằng chứng HTTP, giao diện, dữ liệu lưu thực tế khi thực thi. Chưa nhập tài liệu hoặc gán phê duyệt trên Document First.
5. Không đặt kỳ vọng thu hồi bản đã tải xuống, ngắt luồng tải đã bắt đầu, hoặc tự xóa tệp ở nhà cung cấp ngoài khi chưa có quy tắc tương ứng. Kiểm quyền đối với yêu cầu truy cập mới theo nghiệp vụ đã chốt.

## Kiểm tra tài liệu

Đã kiểm cấu trúc 13 cột, mỗi file đúng một dòng dữ liệu, mã tài liệu không trùng, comment hướng dẫn còn nguyên, Trace to khớp TEST_LINKS, mã và section đích tồn tại, liên kết file trong báo cáo, độ phủ AC và các luồng SITE/PROJ liệt kê ở trên. Đây là kiểm tra tài liệu cục bộ; không thay thế chạy ứng dụng hoặc bước kiểm tra import trên hệ thống tài liệu.
