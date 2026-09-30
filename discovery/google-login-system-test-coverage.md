# Đặc tả System Test cho đăng nhập Google

Người dùng đã chốt [STORY-AUTH-002](../userstory/STORY-AUTH-002.md) và [BR-AUTH-003](../businessrule/BR-AUTH-003.md), [BR-AUTH-004](../businessrule/BR-AUTH-004.md), [BR-AUTH-005](../businessrule/BR-AUTH-005.md), [BR-AUTH-006](../businessrule/BR-AUTH-006.md), [BR-AUTH-007](../businessrule/BR-AUTH-007.md) trong hội thoại ngày 30/09/2026. Tài liệu này đối chiếu 40 đặc tả mới, từ ST-AUTH-021 đến ST-AUTH-060, với nội dung đã chốt.

**Đây là đặc tả chưa chạy.** Trạng thái Draft trong từng file không phải kết quả kiểm thử. Đã kiểm cấu trúc 13 cột, một test/file, mã duy nhất, tham chiếu tới section có thật và sự khớp nhau giữa Trace to với TEST_LINKS. Chưa có bằng chứng tính năng Google hoạt động trên hệ thống. Tân Trần là Reviewer và Approver theo xác nhận của người dùng; chưa thao tác phê duyệt hoặc nhập tài liệu lên hệ thống.

Các ca mới có liên kết tới đủ **15/15 tiêu chí nghiệm thu**, Main Flow, **7/7 Alternative Flow**, **5/5 Exception Flow** và Non-Functional. Độ bao phủ này nói về đặc tả; API, dữ liệu Google, mô hình liên kết và fixture đã có phương án tại [TDD-AUTH-003](../tdd/TDD-AUTH-003.md), đã được người dùng chốt trong hội thoại ngày 30/09/2026. Cần triển khai trước khi có thể chạy. [Bộ Unit Test và kế hoạch integration](google-login-unit-test-coverage.md) đã cụ thể hóa các phần kỹ thuật theo TDD.

**Đối chiếu tiêu chí nghiệm thu**

| Tiêu chí | System Test |
| --- | --- |
| [STORY-AUTH-002/AC-001](../userstory/STORY-AUTH-002.md#ac-001) | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-024](../systemtest/ST-AUTH-024.md), [ST-AUTH-029](../systemtest/ST-AUTH-029.md), [ST-AUTH-039](../systemtest/ST-AUTH-039.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md), [ST-AUTH-050](../systemtest/ST-AUTH-050.md) |
| [STORY-AUTH-002/AC-002](../userstory/STORY-AUTH-002.md#ac-002) | [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-028](../systemtest/ST-AUTH-028.md), [ST-AUTH-040](../systemtest/ST-AUTH-040.md), [ST-AUTH-050](../systemtest/ST-AUTH-050.md), [ST-AUTH-051](../systemtest/ST-AUTH-051.md) |
| [STORY-AUTH-002/AC-003](../userstory/STORY-AUTH-002.md#ac-003) | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-052](../systemtest/ST-AUTH-052.md) |
| [STORY-AUTH-002/AC-004](../userstory/STORY-AUTH-002.md#ac-004) | [ST-AUTH-026](../systemtest/ST-AUTH-026.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md) |
| [STORY-AUTH-002/AC-005](../userstory/STORY-AUTH-002.md#ac-005) | [ST-AUTH-029](../systemtest/ST-AUTH-029.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-031](../systemtest/ST-AUTH-031.md), [ST-AUTH-058](../systemtest/ST-AUTH-058.md) |
| [STORY-AUTH-002/AC-006](../userstory/STORY-AUTH-002.md#ac-006) | [ST-AUTH-032](../systemtest/ST-AUTH-032.md), [ST-AUTH-033](../systemtest/ST-AUTH-033.md), [ST-AUTH-036](../systemtest/ST-AUTH-036.md), [ST-AUTH-054](../systemtest/ST-AUTH-054.md) |
| [STORY-AUTH-002/AC-007](../userstory/STORY-AUTH-002.md#ac-007) | [ST-AUTH-034](../systemtest/ST-AUTH-034.md), [ST-AUTH-056](../systemtest/ST-AUTH-056.md) |
| [STORY-AUTH-002/AC-008](../userstory/STORY-AUTH-002.md#ac-008) | [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-037](../systemtest/ST-AUTH-037.md) |
| [STORY-AUTH-002/AC-009](../userstory/STORY-AUTH-002.md#ac-009) | [ST-AUTH-037](../systemtest/ST-AUTH-037.md), [ST-AUTH-038](../systemtest/ST-AUTH-038.md) |
| [STORY-AUTH-002/AC-010](../userstory/STORY-AUTH-002.md#ac-010) | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-039](../systemtest/ST-AUTH-039.md), [ST-AUTH-040](../systemtest/ST-AUTH-040.md) |
| [STORY-AUTH-002/AC-011](../userstory/STORY-AUTH-002.md#ac-011) | [ST-AUTH-044](../systemtest/ST-AUTH-044.md), [ST-AUTH-045](../systemtest/ST-AUTH-045.md), [ST-AUTH-046](../systemtest/ST-AUTH-046.md) |
| [STORY-AUTH-002/AC-012](../userstory/STORY-AUTH-002.md#ac-012) | [ST-AUTH-041](../systemtest/ST-AUTH-041.md), [ST-AUTH-042](../systemtest/ST-AUTH-042.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md), [ST-AUTH-060](../systemtest/ST-AUTH-060.md) |
| [STORY-AUTH-002/AC-013](../userstory/STORY-AUTH-002.md#ac-013) | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-033](../systemtest/ST-AUTH-033.md), [ST-AUTH-055](../systemtest/ST-AUTH-055.md), [ST-AUTH-056](../systemtest/ST-AUTH-056.md), [ST-AUTH-057](../systemtest/ST-AUTH-057.md) |
| [STORY-AUTH-002/AC-014](../userstory/STORY-AUTH-002.md#ac-014) | [ST-AUTH-028](../systemtest/ST-AUTH-028.md), [ST-AUTH-047](../systemtest/ST-AUTH-047.md) |
| [STORY-AUTH-002/AC-015](../userstory/STORY-AUTH-002.md#ac-015) | [ST-AUTH-040](../systemtest/ST-AUTH-040.md), [ST-AUTH-048](../systemtest/ST-AUTH-048.md), [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |

**Đối chiếu luồng và yêu cầu ngoài chức năng**

| Phần của Story | System Test |
| --- | --- |
| ALT-01 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md) |
| ALT-02 | [ST-AUTH-026](../systemtest/ST-AUTH-026.md) |
| ALT-03 | [ST-AUTH-029](../systemtest/ST-AUTH-029.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-031](../systemtest/ST-AUTH-031.md) |
| ALT-04 | [ST-AUTH-032](../systemtest/ST-AUTH-032.md), [ST-AUTH-033](../systemtest/ST-AUTH-033.md), [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-036](../systemtest/ST-AUTH-036.md) |
| ALT-05 | [ST-AUTH-037](../systemtest/ST-AUTH-037.md), [ST-AUTH-038](../systemtest/ST-AUTH-038.md) |
| ALT-06 | [ST-AUTH-034](../systemtest/ST-AUTH-034.md) |
| ALT-07 | [ST-AUTH-040](../systemtest/ST-AUTH-040.md), [ST-AUTH-048](../systemtest/ST-AUTH-048.md), [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |
| EXC-01 | [ST-AUTH-041](../systemtest/ST-AUTH-041.md), [ST-AUTH-042](../systemtest/ST-AUTH-042.md) |
| EXC-02 | [ST-AUTH-044](../systemtest/ST-AUTH-044.md), [ST-AUTH-045](../systemtest/ST-AUTH-045.md), [ST-AUTH-046](../systemtest/ST-AUTH-046.md) |
| EXC-03 | [ST-AUTH-031](../systemtest/ST-AUTH-031.md), [ST-AUTH-058](../systemtest/ST-AUTH-058.md) |
| EXC-04 | [ST-AUTH-047](../systemtest/ST-AUTH-047.md) |
| EXC-05 | [ST-AUTH-038](../systemtest/ST-AUTH-038.md) |
| Main Flow | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-024](../systemtest/ST-AUTH-024.md), [ST-AUTH-027](../systemtest/ST-AUTH-027.md) |
| Non-Functional | [ST-AUTH-042](../systemtest/ST-AUTH-042.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md), [ST-AUTH-050](../systemtest/ST-AUTH-050.md), [ST-AUTH-051](../systemtest/ST-AUTH-051.md), [ST-AUTH-052](../systemtest/ST-AUTH-052.md), [ST-AUTH-053](../systemtest/ST-AUTH-053.md), [ST-AUTH-054](../systemtest/ST-AUTH-054.md), [ST-AUTH-055](../systemtest/ST-AUTH-055.md), [ST-AUTH-058](../systemtest/ST-AUTH-058.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md), [ST-AUTH-060](../systemtest/ST-AUTH-060.md) |

**Đối chiếu từng khoản của Business Rule**

Các khoản được đối chiếu theo mục Then của bản đã chốt. BR-AUTH-007 khoản 5 được kiểm qua fixture có bằng chứng xác minh đúng email và ca ngăn đổi mật khẩu tự biến thành xác minh email. Dữ liệu thử từng có email thay đổi phải được kiểm tra trước khi dùng làm fixture; không coi việc chặn đổi email từ nay là bằng chứng cho dữ liệu cũ.

| Business Rule | Khoản Then | System Test |
| --- | --- | --- |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 1 | [ST-AUTH-042](../systemtest/ST-AUTH-042.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 2 | [ST-AUTH-024](../systemtest/ST-AUTH-024.md), [ST-AUTH-029](../systemtest/ST-AUTH-029.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-031](../systemtest/ST-AUTH-031.md), [ST-AUTH-058](../systemtest/ST-AUTH-058.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 3 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-024](../systemtest/ST-AUTH-024.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 4 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-032](../systemtest/ST-AUTH-032.md), [ST-AUTH-039](../systemtest/ST-AUTH-039.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 5 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-026](../systemtest/ST-AUTH-026.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 6 | [ST-AUTH-044](../systemtest/ST-AUTH-044.md), [ST-AUTH-045](../systemtest/ST-AUTH-045.md), [ST-AUTH-046](../systemtest/ST-AUTH-046.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 7 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-056](../systemtest/ST-AUTH-056.md), [ST-AUTH-057](../systemtest/ST-AUTH-057.md) |
| [BR-AUTH-003](../businessrule/BR-AUTH-003.md#then) | 8 | [ST-AUTH-041](../systemtest/ST-AUTH-041.md), [ST-AUTH-042](../systemtest/ST-AUTH-042.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 1 | [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-028](../systemtest/ST-AUTH-028.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 2 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-026](../systemtest/ST-AUTH-026.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 3 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 4 | [ST-AUTH-026](../systemtest/ST-AUTH-026.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 5 | [ST-AUTH-026](../systemtest/ST-AUTH-026.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 6 | [ST-AUTH-047](../systemtest/ST-AUTH-047.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 7 | [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-051](../systemtest/ST-AUTH-051.md) |
| [BR-AUTH-004](../businessrule/BR-AUTH-004.md#then) | 8 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-040](../systemtest/ST-AUTH-040.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 1 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-026](../systemtest/ST-AUTH-026.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 2 | [ST-AUTH-032](../systemtest/ST-AUTH-032.md), [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-036](../systemtest/ST-AUTH-036.md), [ST-AUTH-037](../systemtest/ST-AUTH-037.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 3 | [ST-AUTH-033](../systemtest/ST-AUTH-033.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 4 | [ST-AUTH-034](../systemtest/ST-AUTH-034.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 5 | [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-036](../systemtest/ST-AUTH-036.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 6 | [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-037](../systemtest/ST-AUTH-037.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 7 | [ST-AUTH-037](../systemtest/ST-AUTH-037.md), [ST-AUTH-038](../systemtest/ST-AUTH-038.md) |
| [BR-AUTH-005](../businessrule/BR-AUTH-005.md#then) | 8 | [ST-AUTH-053](../systemtest/ST-AUTH-053.md), [ST-AUTH-054](../systemtest/ST-AUTH-054.md) |
| [BR-AUTH-006](../businessrule/BR-AUTH-006.md#then) | 1 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-039](../systemtest/ST-AUTH-039.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md) |
| [BR-AUTH-006](../businessrule/BR-AUTH-006.md#then) | 2 | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-039](../systemtest/ST-AUTH-039.md) |
| [BR-AUTH-006](../businessrule/BR-AUTH-006.md#then) | 3 | [ST-AUTH-039](../systemtest/ST-AUTH-039.md) |
| [BR-AUTH-006](../businessrule/BR-AUTH-006.md#then) | 4 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md) |
| [BR-AUTH-006](../businessrule/BR-AUTH-006.md#then) | 5 | [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-040](../systemtest/ST-AUTH-040.md) |
| [BR-AUTH-007](../businessrule/BR-AUTH-007.md#then) | 1 | [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |
| [BR-AUTH-007](../businessrule/BR-AUTH-007.md#then) | 2 | [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |
| [BR-AUTH-007](../businessrule/BR-AUTH-007.md#then) | 3 | [ST-AUTH-048](../systemtest/ST-AUTH-048.md), [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |
| [BR-AUTH-007](../businessrule/BR-AUTH-007.md#then) | 4 | [ST-AUTH-048](../systemtest/ST-AUTH-048.md) |
| [BR-AUTH-007](../businessrule/BR-AUTH-007.md#then) | 5 | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md) |

**Danh mục ca kiểm thử**

| Mã | Nội dung | Nhóm chạy | Ưu tiên kiểm thử |
| --- | --- | --- | --- |
| [ST-AUTH-021](../systemtest/ST-AUTH-021.md) | Tạo khách hàng mới bằng Google trên web | SMOKE | P0 |
| [ST-AUTH-022](../systemtest/ST-AUTH-022.md) | Tạo khách hàng mới bằng Google trên Android | SMOKE | P0 |
| [ST-AUTH-023](../systemtest/ST-AUTH-023.md) | Tạo khách hàng mới bằng Google trên iOS | SMOKE | P0 |
| [ST-AUTH-024](../systemtest/ST-AUTH-024.md) | Google Workspace đủ căn cứ xác minh email | REGRESSION | P1 |
| [ST-AUTH-025](../systemtest/ST-AUTH-025.md) | Tự liên kết tài khoản đã xác minh, giữ mật khẩu và dữ liệu | SMOKE | P0 |
| [ST-AUTH-026](../systemtest/ST-AUTH-026.md) | Tiếp nhận tài khoản chưa xác minh và vô hiệu các đường truy cập cũ | SMOKE | P0 |
| [ST-AUTH-027](../systemtest/ST-AUTH-027.md) | Đăng nhập lại cùng Google trên các nền tảng | REGRESSION | P1 |
| [ST-AUTH-028](../systemtest/ST-AUTH-028.md) | Nhận diện Google đã liên kết khi thông tin Google thay đổi | REGRESSION | P0 |
| [ST-AUTH-029](../systemtest/ST-AUTH-029.md) | Xác minh bổ sung email ngoài Google trước khi tạo khách hàng | REGRESSION | P0 |
| [ST-AUTH-030](../systemtest/ST-AUTH-030.md) | Xác minh bổ sung trước khi liên kết email ngoài Google với tài khoản cũ | REGRESSION | P0 |
| [ST-AUTH-031](../systemtest/ST-AUTH-031.md) | Không hoàn tất xác minh bổ sung thì không thay đổi tài khoản | REGRESSION | P0 |
| [ST-AUTH-032](../systemtest/ST-AUTH-032.md) | Mật khẩu tạm không tự hết hạn theo thời gian | REGRESSION | P1 |
| [ST-AUTH-033](../systemtest/ST-AUTH-033.md) | Phiên mật khẩu tạm bị giới hạn cả trước và sau khi làm mới | SMOKE | P0 |
| [ST-AUTH-034](../systemtest/ST-AUTH-034.md) | Phiên Google không bị giới hạn vì mật khẩu tạm chưa đổi | SMOKE | P0 |
| [ST-AUTH-035](../systemtest/ST-AUTH-035.md) | Đổi mật khẩu tạm thành công hủy mọi phiên BMT cũ | SMOKE | P0 |
| [ST-AUTH-036](../systemtest/ST-AUTH-036.md) | Đổi mật khẩu thất bại không làm mất mật khẩu tạm | REGRESSION | P1 |
| [ST-AUTH-037](../systemtest/ST-AUTH-037.md) | Quên mật khẩu thay mật khẩu tạm và giữ liên kết Google | SMOKE | P0 |
| [ST-AUTH-038](../systemtest/ST-AUTH-038.md) | Không nhận thư mật khẩu tạm thì dùng Quên mật khẩu | REGRESSION | P1 |
| [ST-AUTH-039](../systemtest/ST-AUTH-039.md) | Tạo hồ sơ khi Google thiếu tên hoặc ảnh | REGRESSION | P2 |
| [ST-AUTH-040](../systemtest/ST-AUTH-040.md) | Không ghi đè hồ sơ BMT sau khi khách đã sửa | REGRESSION | P1 |
| [ST-AUTH-041](../systemtest/ST-AUTH-041.md) | Khách hủy bước đăng nhập Google | REGRESSION | P1 |
| [ST-AUTH-042](../systemtest/ST-AUTH-042.md) | Từ chối bằng chứng Google không hợp lệ | SMOKE | P0 |
| [ST-AUTH-043](../systemtest/ST-AUTH-043.md) | Không tin email hoặc hồ sơ do client tự khai | REGRESSION | P0 |
| [ST-AUTH-044](../systemtest/ST-AUTH-044.md) | Nhân viên không được đăng nhập Google | SMOKE | P0 |
| [ST-AUTH-045](../systemtest/ST-AUTH-045.md) | Tài khoản khách hàng bị khóa không được vào bằng Google | SMOKE | P0 |
| [ST-AUTH-046](../systemtest/ST-AUTH-046.md) | Tài khoản đã xóa không được tự khôi phục bằng Google | REGRESSION | P0 |
| [ST-AUTH-047](../systemtest/ST-AUTH-047.md) | Liên kết mâu thuẫn không bị ghi đè hoặc tự gộp | REGRESSION | P0 |
| [ST-AUTH-048](../systemtest/ST-AUTH-048.md) | Sửa hồ sơ với email giữ nguyên vẫn thành công | REGRESSION | P1 |
| [ST-AUTH-049](../systemtest/ST-AUTH-049.md) | Chặn đổi email ở giao diện và API, không ghi một phần hồ sơ | SMOKE | P0 |
| [ST-AUTH-050](../systemtest/ST-AUTH-050.md) | Đăng nhập Google đồng thời không tạo tài khoản hoặc mật khẩu trùng | REGRESSION | P0 |
| [ST-AUTH-051](../systemtest/ST-AUTH-051.md) | Thử đăng nhập lại sau khi mất phản hồi không cấp lại mật khẩu | REGRESSION | P1 |
| [ST-AUTH-052](../systemtest/ST-AUTH-052.md) | Liên kết đồng thời vào tài khoản cũ không làm mất dữ liệu | REGRESSION | P0 |
| [ST-AUTH-053](../systemtest/ST-AUTH-053.md) | Không ghi mật khẩu, token hoặc mã xác minh vào log | REGRESSION | P0 |
| [ST-AUTH-054](../systemtest/ST-AUTH-054.md) | Lưu dữ liệu kiểm mật khẩu dưới dạng bản băm | REGRESSION | P0 |
| [ST-AUTH-055](../systemtest/ST-AUTH-055.md) | Phiên Google trên web vẫn được bảo vệ CSRF | REGRESSION | P0 |
| [ST-AUTH-056](../systemtest/ST-AUTH-056.md) | Phiên Google làm mới theo đúng nền tảng và thời hạn | REGRESSION | P0 |
| [ST-AUTH-057](../systemtest/ST-AUTH-057.md) | Đăng xuất một phiên Google không cắt các phiên khác | REGRESSION | P1 |
| [ST-AUTH-058](../systemtest/ST-AUTH-058.md) | Bằng chứng xác minh bổ sung phải đúng email đang liên kết | REGRESSION | P0 |
| [ST-AUTH-059](../systemtest/ST-AUTH-059.md) | Đổi mật khẩu thông thường không thay cho xác minh email | REGRESSION | P0 |
| [ST-AUTH-060](../systemtest/ST-AUTH-060.md) | Không chấp nhận đăng nhập Google bị khởi tạo hoặc tráo từ trang khác | REGRESSION | P0 |

**Điều kiện để thực thi**

- Môi trường biệt lập có backend đã triển khai Google, web, app Android/iOS, database và kho phiên thật. Không dùng EF InMemory hoặc mock kho phiên để kết luận về tranh chấp, thu hồi hoặc tính toàn vẹn dữ liệu.
- Google test project và tài khoản Gmail/Workspace/email ngoài Google do nhóm kiểm thử sở hữu. Các bí danh như G_WEB chỉ tên fixture; email, mật khẩu và token thật không nằm trong tài liệu hoặc báo cáo công khai. Không gửi thư tới địa chỉ minh họa hoặc người không tham gia thử nghiệm.
- Hộp thư và mailer kiểm thử cho phép kiểm đúng người nhận, mô phỏng thư thất bại và nhận mã Quên mật khẩu. Không gọi là khôi phục thành công nếu dịch vụ email vẫn hỏng và khách chưa nhận được mã.
- Có đồng hồ điều khiển được khi kiểm thời hạn. Mốc 24 giờ và 365 ngày trong ST-AUTH-032 là dữ liệu thử, không phải chính sách hết hạn mật khẩu.
- Mỗi biến thể có dữ liệu và kết quả riêng. Ghi trạng thái trước/sau bằng truy vấn chỉ đọc; dùng hành vi API thật để tạo/liên kết/đổi mật khẩu. Chỉ dùng fixture trực tiếp cho trạng thái không tạo được bằng luồng công khai, và phải ghi rõ cách tạo. Không gọi helper kiểm tra mật khẩu hoặc callback nội bộ thay cho luồng hệ thống rồi coi đó là kiểm thử đầu cuối.
- Theo dõi cùng mã lần chạy hoặc correlation ID qua request, database, kho phiên và gửi thư. Bằng chứng phải che bí mật. Chỉ dọn dữ liệu gắn với lần chạy hoặc hủy môi trường dùng riêng; xác nhận không còn dữ liệu thử. Chưa có thao tác chạy hay dọn dữ liệu nào trong công việc soạn đặc tả này.
- Chờ xử lý nền bằng cơ chế quan sát có giới hạn lấy từ cấu hình kiểm thử được ghi nhận. Chưa tự đặt SLA gửi thư, số lần thử lại hoặc ngưỡng hiệu năng nghiệp vụ.

**Các phụ thuộc kỹ thuật trong TDD đã chốt**

Các mục dưới đây ghi nhu cầu được xác định khi soạn ST. TDD-AUTH-003 đã chốt cách xử lý: Internal API cho contract; Architecture cho xác minh bổ sung, binding, xung đột và thư mã hóa; Data Model cho tên tùy chọn và phiên. Fixture/assertion kỹ thuật được bổ sung trong bảng “Các phép kiểm bắt buộc ngoài unit” của bộ Unit Test. Khi viết automation cần triển khai cả các kiểm tra này; chưa có kết quả chạy hoặc phê duyệt trên ứng dụng tài liệu.

- TDD-AUTH-003/Internal API đã chốt API, mã lỗi và cách trao đổi Google. ST hiện mô tả chức năng; dùng contract đó cùng phần bổ sung integration khi triển khai từng request/assertion.
- ST-AUTH-029, 030, 031 và 058 đối chiếu contract mã email và cách gắn proof với đúng attempt/email/Google trong TDD-AUTH-003/Architecture. ST-AUTH-060 đối chiếu state, nonce và verifier gắn client tại cùng TDD; kiểm chữ ký token hợp lệ chưa đủ cho ca này.
- ST-AUTH-039 đối chiếu ánh xạ tên NULL/tên một từ và ảnh tùy chọn trong TDD-AUTH-003/Data Model. Đặc tả không tự đặt tên giả hoặc thêm bước bắt khách bổ sung hồ sơ.
- ST-AUTH-047 dùng fixture attempt chờ A trong khi sub đã liên kết B tại TDD-AUTH-003/Architecture đã chốt. Không tự quyết định giới hạn số tài khoản Google hoặc dựng dữ liệu phá ràng buộc database chỉ để ép nhận lỗi. Các ca ST-AUTH-050 đến 052 cần ranh giới ghi dữ liệu và cách xử lý tranh chấp thực.
- ST-AUTH-053 và 054 kiểm log, phản hồi và bản băm của User. TDD-AUTH-003/Architecture đã chốt event thư mã hóa và vòng đời khóa; phần bổ sung integration nêu kiểm tra outbox, hàng đợi, hàng đợi lỗi và phục hồi khóa cần chạy khi triển khai. Không suy ra rằng toàn hệ thống chỉ lưu bản băm vì một cột User đã dùng bản băm.
- Phần trả thông tin phiên cho web/app cần cho phép giao diện phân biệt phiên Google với phiên dùng mật khẩu tạm; nếu chỉ đọc MustChangePassword ở tài khoản thì có thể bắt đổi mật khẩu sai cho phiên Google. ST-AUTH-033 và 034 sẽ kiểm hành vi này qua cả lần làm mới.
- BMT hiện chỉ có tài khoản thử nghiệm theo xác nhận của người dùng. Chưa cần quy trình chuyển đổi tài khoản khách hàng thật; việc này không cấp quyền xóa hoặc đặt lại database đang có.

**Hiện trạng liên quan và thứ tự dự kiến**

| Nhóm việc | Code/tài liệu cần xem khi thiết kế | Mục đích và cách kiểm chứng |
| --- | --- | --- |
| Xác thực Google và liên kết | Contract user, các handler đăng nhập, User và cấu hình dữ liệu; BR-AUTH-003/004 | Chốt nhận diện Google, bằng chứng email, dữ liệu liên kết và ghi dữ liệu đồng thời; kiểm ST-AUTH-021–031, 041–047, 050–052, 058–060. Chưa chọn schema mới trong tài liệu này. |
| Phiên theo cách đăng nhập | AuthSessionIssuer, AccessClaimsBuilder, các policy và chức năng me | Giữ giới hạn khi refresh; phiên Google không bị bắt đổi mật khẩu tạm; kiểm ST-AUTH-032–037, 055–057. |
| Mật khẩu và gửi thư | ChangePasswordCommandHandler, các handler Quên mật khẩu, SendEmailConsumer và đường gửi thư | Chỉ vô hiệu mật khẩu sau thành công; hủy phiên đúng; bảo vệ dữ liệu thư; kiểm ST-AUTH-035–038, 053–054. |
| Tính đúng của email và hồ sơ | UpdateUserProfileCommandHandler, ChangePasswordCommandHandler; BR-AUTH-006/007 | Chặn đổi email ở backend; không tự xác minh email bằng đổi mật khẩu thông thường; kiểm ST-AUTH-039–040, 048–049, 059. |
| Web và app | Luồng Google trên web, Android, iOS và giao diện đổi mật khẩu | Kiểm các ca đầu cuối trên nền tảng thật, sau khi contract đã chốt; không dùng kết quả API riêng lẻ để kết luận UI đã hoàn tất. |

Nguồn đã đối chiếu trực tiếp gồm STORY-AUTH-002, BR-AUTH-003–007, STORY-AUTH-001, BR-AUTH-001/002, BR-RBAC-005/006/009, TDD-AUTH-001/002 và ST-AUTH-001–020, cùng mã nguồn xác thực đã khảo sát. Chuỗi tham chiếu từ AUTH còn dẫn tới các module khác; công việc này chưa phải cuộc rà soát nội dung của toàn bộ tài liệu dự án. Khi soạn TDD phải tiếp tục đọc các phần phụ thuộc cần thiết, không xem kiểm tra liên kết tồn tại là đã đọc nội dung của tài liệu đích.

Đã sửa tham chiếu cũ trong ST-AUTH-002 và ST-AUTH-020 từ `STORY-AUTH-001/Main` thành `STORY-AUTH-001/Main Flow` ở cả Trace to và TEST_LINKS; hành vi của hai ca cũ không đổi.

Các trường quản trị còn thiếu được giữ rõ: Owner của test và BR; Priority, Sprint, Assignee của Story; Effective Date của BR. Chưa bịa người phụ trách, ngày hiệu lực hoặc lịch sử phiên bản để coi tài liệu đã đủ điều kiện nhập. Nội dung US/BR đã chốt trong hội thoại; phần quản trị và phê duyệt trên ứng dụng tài liệu vẫn riêng biệt.
