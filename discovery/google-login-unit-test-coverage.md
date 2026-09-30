# Đặc tả Unit Test cho đăng nhập Google

Người dùng đã chốt [TDD-AUTH-003](../tdd/TDD-AUTH-003.md) trong hội thoại ngày 30/09/2026. Theo thiết kế đó, bộ này có **73 đặc tả mới, UT-AUTH-038 đến UT-AUTH-110**, và cập nhật 8 đặc tả AUTH đang có. Reviewer và Approver đều là **Tân Trần** theo xác nhận trước đó.

**Chưa viết hoặc chạy mã test.** Các lớp Google và phần sửa dịch vụ hiện có trong đặc tả đều là thiết kế dự kiến. Trạng thái Draft không phải kết quả thực thi hoặc xác nhận phê duyệt trên hệ thống tài liệu. Chưa import, sửa mã ứng dụng, tạo migration hoặc chạy dịch vụ.

Bộ đặc tả giữ các quyết định đã chốt: Google dành cho Customer trên web/Android/iOS; nhận diện bằng sub; chỉ liên kết theo email sau khi chứng minh quyền sở hữu; email hồ sơ bất biến. Mật khẩu tạm không tự hết hạn, phiên dùng mật khẩu tạm phải đổi trước, còn phiên Google dùng bình thường. Đổi/reset thành công thay stamp và thu hồi phiên cũ. Hạn gửi thư 24 giờ không phải hạn của mật khẩu.

**Cách đọc và triển khai đặc tả**

- Mỗi file có một ca, đúng 13 cột; biến thể cùng một điều kiện có thể triển khai bằng test tham số hóa. Mỗi biến thể dùng fixture riêng, kết quả phải được ghi riêng khi chạy.
- A/B, S1/S2/S3, H1/H_TMP, G và E1 là ký hiệu của tài khoản, stamp UUID, hash, Google sub và emailKey trong fixture. Dùng dữ liệu thử xác định, không dùng bí mật thật. Hạn lấy từ TimeProvider; không chờ thời gian thật.
- Unit kiểm kết quả, trạng thái và các side effect cần thiết cho hợp đồng. Dùng fake ở interface do dự án sở hữu; không mock DbContext/framework để kết luận SQL đúng. Nếu handler giữ LINQ/SQL trực tiếp không có ranh giới tách được, chuyển phép kiểm đó sang integration theo cùng điều kiện, không thay database bằng EF InMemory để tuyên bố đã phủ transaction.
- Các ca JWT dùng thư viện kiểm token thật với khóa test và JWKS fixture; không mock kết quả “token hợp lệ” để chứng minh bước xác thực. Các ca thư dùng protector giả chỉ kiểm contract/đường dữ liệu; mã hóa và lưu khóa thật được kiểm riêng bên dưới.
- Các hàm ánh xạ HTTP, callback JWT và log dùng context/logger giả chỉ kiểm nhánh xử lý. Routing, serializer, middleware, response thực và log proxy vẫn cần kiểm cả pipeline.
- Các ca cũ UT-AUTH-030/031 dùng Redis thật và UT-AUTH-035–037 dùng TestServer thực chất thuộc mức integration dù mang mã UT lịch sử. Giữ mã và ý nghĩa, không tính chúng là bằng chứng unit cô lập.

**Đặc tả cũ được cập nhật**

| Mã | Thay đổi theo TDD đã chốt |
| --- | --- |
| [UT-AUTH-008](../unittest/UT-AUTH-008.md) | Kết quả kiểm mật khẩu mang ExpectedSecurityStamp của cùng bản đọc. |
| [UT-AUTH-009](../unittest/UT-AUTH-009.md) | Login mobile truyền Password và stamp đã kiểm cho issuer. |
| [UT-AUTH-013](../unittest/UT-AUTH-013.md) | Login web truyền Password/stamp; giữ phạm vi hành vi lỗi cũ. |
| [UT-AUTH-014](../unittest/UT-AUTH-014.md) | Verify-account tiêu thụ mã, lưu proof đúng email và chỉ cấp phiên sau commit. |
| [UT-AUTH-017](../unittest/UT-AUTH-017.md) | Verify-reset trả proof email/stamp; cấp phiên reset ngoài transaction SQL. |
| [UT-AUTH-019](../unittest/UT-AUTH-019.md) | Blob/JWT phiên mobile lưu đúng method/stamp, giữ thời hạn cũ. |
| [UT-AUTH-025](../unittest/UT-AUTH-025.md) | Refresh reset giữ method/stamp/email proof và thời hạn reset. |
| [UT-AUTH-032](../unittest/UT-AUTH-032.md) | Phép kiểm request nhạy cảm bao gồm DTO Google không qua MediatR và command nội bộ. |

Các ca không bị đổi hợp đồng tiếp tục dùng lại: UT-AUTH-001–007 về hạn phiên/credential; UT-AUTH-010–012, 015–016, 018 về từ chối; UT-AUTH-020–024, 026–031 về refresh/logout/định dạng cũ; UT-AUTH-033–037 về log và API hiện có. Trước khi chạy cùng implementation mới cần cập nhật fixture/chữ ký hàm tương ứng; không coi đường dẫn mã test cũ trong tài liệu là bằng chứng ca mới đã chạy.

**Đối chiếu tiêu chí nghiệm thu**

Liên kết dưới đây chỉ ra phần logic được đặc tả ở mức unit. Một ca unit có liên kết AC không chứng minh đã đạt toàn bộ AC trên web/app. Toàn bộ 15 AC vẫn phải chạy cùng [bộ System Test](google-login-system-test-coverage.md).

| AC của STORY-AUTH-002 | Unit Test mới | Phần kiểm ngoài unit |
| --- | --- | --- |
| [AC-001](../userstory/STORY-AUTH-002.md#ac-001) | [UT-AUTH-051](../unittest/UT-AUTH-051.md), [UT-AUTH-062](../unittest/UT-AUTH-062.md), [UT-AUTH-070](../unittest/UT-AUTH-070.md), [UT-AUTH-072](../unittest/UT-AUTH-072.md), [UT-AUTH-074](../unittest/UT-AUTH-074.md), [UT-AUTH-100](../unittest/UT-AUTH-100.md), [UT-AUTH-101](../unittest/UT-AUTH-101.md) | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-024](../systemtest/ST-AUTH-024.md), [ST-AUTH-029](../systemtest/ST-AUTH-029.md) |
| [AC-002](../userstory/STORY-AUTH-002.md#ac-002) | [UT-AUTH-054](../unittest/UT-AUTH-054.md), [UT-AUTH-065](../unittest/UT-AUTH-065.md), [UT-AUTH-071](../unittest/UT-AUTH-071.md), [UT-AUTH-073](../unittest/UT-AUTH-073.md), [UT-AUTH-106](../unittest/UT-AUTH-106.md) | [ST-AUTH-027](../systemtest/ST-AUTH-027.md), [ST-AUTH-028](../systemtest/ST-AUTH-028.md), [ST-AUTH-050](../systemtest/ST-AUTH-050.md), [ST-AUTH-051](../systemtest/ST-AUTH-051.md) |
| [AC-003](../userstory/STORY-AUTH-002.md#ac-003) | [UT-AUTH-063](../unittest/UT-AUTH-063.md) | [ST-AUTH-025](../systemtest/ST-AUTH-025.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-052](../systemtest/ST-AUTH-052.md) |
| [AC-004](../userstory/STORY-AUTH-002.md#ac-004) | [UT-AUTH-064](../unittest/UT-AUTH-064.md), [UT-AUTH-069](../unittest/UT-AUTH-069.md), [UT-AUTH-085](../unittest/UT-AUTH-085.md), [UT-AUTH-097](../unittest/UT-AUTH-097.md), [UT-AUTH-098](../unittest/UT-AUTH-098.md) | [ST-AUTH-026](../systemtest/ST-AUTH-026.md), [ST-AUTH-059](../systemtest/ST-AUTH-059.md) |
| [AC-005](../userstory/STORY-AUTH-002.md#ac-005) | [UT-AUTH-041](../unittest/UT-AUTH-041.md), [UT-AUTH-050](../unittest/UT-AUTH-050.md), [UT-AUTH-051](../unittest/UT-AUTH-051.md), [UT-AUTH-055](../unittest/UT-AUTH-055.md), [UT-AUTH-056](../unittest/UT-AUTH-056.md), [UT-AUTH-057](../unittest/UT-AUTH-057.md), [UT-AUTH-058](../unittest/UT-AUTH-058.md), [UT-AUTH-059](../unittest/UT-AUTH-059.md), [UT-AUTH-060](../unittest/UT-AUTH-060.md), [UT-AUTH-061](../unittest/UT-AUTH-061.md), [UT-AUTH-067](../unittest/UT-AUTH-067.md), [UT-AUTH-105](../unittest/UT-AUTH-105.md), [UT-AUTH-109](../unittest/UT-AUTH-109.md) | [ST-AUTH-029](../systemtest/ST-AUTH-029.md), [ST-AUTH-030](../systemtest/ST-AUTH-030.md), [ST-AUTH-031](../systemtest/ST-AUTH-031.md), [ST-AUTH-058](../systemtest/ST-AUTH-058.md) |
| [AC-006](../userstory/STORY-AUTH-002.md#ac-006) | [UT-AUTH-080](../unittest/UT-AUTH-080.md), [UT-AUTH-081](../unittest/UT-AUTH-081.md), [UT-AUTH-082](../unittest/UT-AUTH-082.md), [UT-AUTH-083](../unittest/UT-AUTH-083.md), [UT-AUTH-084](../unittest/UT-AUTH-084.md), [UT-AUTH-088](../unittest/UT-AUTH-088.md), [UT-AUTH-089](../unittest/UT-AUTH-089.md), [UT-AUTH-091](../unittest/UT-AUTH-091.md), [UT-AUTH-094](../unittest/UT-AUTH-094.md), [UT-AUTH-100](../unittest/UT-AUTH-100.md), [UT-AUTH-104](../unittest/UT-AUTH-104.md) | [ST-AUTH-032](../systemtest/ST-AUTH-032.md), [ST-AUTH-033](../systemtest/ST-AUTH-033.md), [ST-AUTH-036](../systemtest/ST-AUTH-036.md) |
| [AC-007](../userstory/STORY-AUTH-002.md#ac-007) | [UT-AUTH-065](../unittest/UT-AUTH-065.md), [UT-AUTH-079](../unittest/UT-AUTH-079.md), [UT-AUTH-080](../unittest/UT-AUTH-080.md), [UT-AUTH-082](../unittest/UT-AUTH-082.md), [UT-AUTH-088](../unittest/UT-AUTH-088.md) | [ST-AUTH-034](../systemtest/ST-AUTH-034.md), [ST-AUTH-056](../systemtest/ST-AUTH-056.md) |
| [AC-008](../userstory/STORY-AUTH-002.md#ac-008) | [UT-AUTH-069](../unittest/UT-AUTH-069.md), [UT-AUTH-085](../unittest/UT-AUTH-085.md), [UT-AUTH-086](../unittest/UT-AUTH-086.md), [UT-AUTH-087](../unittest/UT-AUTH-087.md), [UT-AUTH-090](../unittest/UT-AUTH-090.md), [UT-AUTH-091](../unittest/UT-AUTH-091.md), [UT-AUTH-092](../unittest/UT-AUTH-092.md), [UT-AUTH-093](../unittest/UT-AUTH-093.md), [UT-AUTH-094](../unittest/UT-AUTH-094.md), [UT-AUTH-095](../unittest/UT-AUTH-095.md), [UT-AUTH-096](../unittest/UT-AUTH-096.md), [UT-AUTH-097](../unittest/UT-AUTH-097.md), [UT-AUTH-099](../unittest/UT-AUTH-099.md), [UT-AUTH-104](../unittest/UT-AUTH-104.md) | [ST-AUTH-035](../systemtest/ST-AUTH-035.md), [ST-AUTH-037](../systemtest/ST-AUTH-037.md) |
| [AC-009](../userstory/STORY-AUTH-002.md#ac-009) | [UT-AUTH-093](../unittest/UT-AUTH-093.md), [UT-AUTH-096](../unittest/UT-AUTH-096.md), [UT-AUTH-098](../unittest/UT-AUTH-098.md), [UT-AUTH-106](../unittest/UT-AUTH-106.md), [UT-AUTH-107](../unittest/UT-AUTH-107.md) | [ST-AUTH-037](../systemtest/ST-AUTH-037.md), [ST-AUTH-038](../systemtest/ST-AUTH-038.md) |
| [AC-010](../userstory/STORY-AUTH-002.md#ac-010) | [UT-AUTH-062](../unittest/UT-AUTH-062.md), [UT-AUTH-063](../unittest/UT-AUTH-063.md), [UT-AUTH-065](../unittest/UT-AUTH-065.md), [UT-AUTH-074](../unittest/UT-AUTH-074.md), [UT-AUTH-075](../unittest/UT-AUTH-075.md), [UT-AUTH-078](../unittest/UT-AUTH-078.md), [UT-AUTH-107](../unittest/UT-AUTH-107.md) | [ST-AUTH-039](../systemtest/ST-AUTH-039.md), [ST-AUTH-040](../systemtest/ST-AUTH-040.md) |
| [AC-011](../userstory/STORY-AUTH-002.md#ac-011) | [UT-AUTH-066](../unittest/UT-AUTH-066.md), [UT-AUTH-081](../unittest/UT-AUTH-081.md) | [ST-AUTH-044](../systemtest/ST-AUTH-044.md), [ST-AUTH-045](../systemtest/ST-AUTH-045.md), [ST-AUTH-046](../systemtest/ST-AUTH-046.md) |
| [AC-012](../userstory/STORY-AUTH-002.md#ac-012) | [UT-AUTH-039](../unittest/UT-AUTH-039.md), [UT-AUTH-040](../unittest/UT-AUTH-040.md), [UT-AUTH-044](../unittest/UT-AUTH-044.md), [UT-AUTH-045](../unittest/UT-AUTH-045.md), [UT-AUTH-046](../unittest/UT-AUTH-046.md), [UT-AUTH-047](../unittest/UT-AUTH-047.md), [UT-AUTH-048](../unittest/UT-AUTH-048.md), [UT-AUTH-049](../unittest/UT-AUTH-049.md), [UT-AUTH-050](../unittest/UT-AUTH-050.md), [UT-AUTH-052](../unittest/UT-AUTH-052.md), [UT-AUTH-053](../unittest/UT-AUTH-053.md), [UT-AUTH-054](../unittest/UT-AUTH-054.md) | [ST-AUTH-041](../systemtest/ST-AUTH-041.md), [ST-AUTH-042](../systemtest/ST-AUTH-042.md), [ST-AUTH-043](../systemtest/ST-AUTH-043.md), [ST-AUTH-060](../systemtest/ST-AUTH-060.md) |
| [AC-013](../userstory/STORY-AUTH-002.md#ac-013) | [UT-AUTH-038](../unittest/UT-AUTH-038.md), [UT-AUTH-053](../unittest/UT-AUTH-053.md), [UT-AUTH-073](../unittest/UT-AUTH-073.md), [UT-AUTH-082](../unittest/UT-AUTH-082.md), [UT-AUTH-083](../unittest/UT-AUTH-083.md), [UT-AUTH-084](../unittest/UT-AUTH-084.md), [UT-AUTH-109](../unittest/UT-AUTH-109.md) | [ST-AUTH-021](../systemtest/ST-AUTH-021.md), [ST-AUTH-022](../systemtest/ST-AUTH-022.md), [ST-AUTH-023](../systemtest/ST-AUTH-023.md), [ST-AUTH-055](../systemtest/ST-AUTH-055.md), [ST-AUTH-056](../systemtest/ST-AUTH-056.md), [ST-AUTH-057](../systemtest/ST-AUTH-057.md) |
| [AC-014](../userstory/STORY-AUTH-002.md#ac-014) | [UT-AUTH-068](../unittest/UT-AUTH-068.md), [UT-AUTH-072](../unittest/UT-AUTH-072.md) | [ST-AUTH-047](../systemtest/ST-AUTH-047.md) |
| [AC-015](../userstory/STORY-AUTH-002.md#ac-015) | [UT-AUTH-076](../unittest/UT-AUTH-076.md), [UT-AUTH-077](../unittest/UT-AUTH-077.md), [UT-AUTH-078](../unittest/UT-AUTH-078.md), [UT-AUTH-088](../unittest/UT-AUTH-088.md) | [ST-AUTH-048](../systemtest/ST-AUTH-048.md), [ST-AUTH-049](../systemtest/ST-AUTH-049.md) |

**Đối chiếu Business Rule và ranh giới kỹ thuật**

| Quy tắc | Khoản và trách nhiệm trong TDD-AUTH-003 | Unit Test chính | Kiểm bổ sung |
| --- | --- | --- | --- |
| [BR-AUTH-003/Then](../businessrule/BR-AUTH-003.md#then) | 1–2: verifier, client binding và email; 3–5: command tạo/link; 6: kiểm trạng thái; 7–8: trả phiên/hủy. Architecture, Internal API, Fields. | UT-AUTH-038–073, 109 | Google thử nghiệm, pipeline HTTP, PostgreSQL/Redis và UI thật. |
| [BR-AUTH-004/Then](../businessrule/BR-AUTH-004.md#then) | 1–3: sub và tài khoản đã xác minh; 4–5: thay credential/stamp/mã; 6: conflict; 7–8: lặp và giữ hồ sơ. Architecture, Data Model. | UT-AUTH-063–073, 085, 092, 097–098 | Khóa, unique, outbox rollback, race với reset/đăng ký thường/Staff. |
| [BR-AUTH-005/Then](../businessrule/BR-AUTH-005.md#then) | 1–2: tạo mật khẩu và không tự hết hạn; 3–4: quyền theo phương thức; 5–6: change/reset/stamp; 7–8: phục hồi/mail/bí mật. Architecture, State Diagram, Error Codes. | UT-AUTH-079–110; dùng lại 008–009, 013–014, 017, 019, 025, 032 | Named policy, thu hồi DB thật, SMTP/broker, bảo vệ và phục hồi keyring. |
| [BR-AUTH-006/Then](../businessrule/BR-AUTH-006.md#then) | 1–3: chỉ lấy hồ sơ lúc tạo, tên/ảnh tùy chọn; 4–5: không ghi đè. Data Model. | UT-AUTH-062–065, 074–075, 078, 107 | Lưu NULL, serialization và màn hình thiếu tên; CSP/ảnh. |
| [BR-AUTH-007/Then](../businessrule/BR-AUTH-007.md#then) | 1–4: email bất biến, cập nhật toàn bộ hoặc từ chối; 5: không suy ra bằng chứng từ cờ cũ. Data Model, Internal API. | UT-AUTH-064, 076–078, 088, 092 | Chuẩn hóa PostgreSQL, endpoint web/app, migration cờ cũ. |

**Các phép kiểm bắt buộc ngoài unit**

Đây là kế hoạch kiểm tích hợp và đầu cuối gắn với TDD; chưa phải ca đã chạy. Các số ở cột đầu chỉ để dẫn mục trong báo cáo này, không tạo thêm mã tài liệu test.

| Mục | Môi trường và tình huống | Assertion cần quan sát trực tiếp | Nguồn liên quan |
| --- | --- | --- | --- |
| 1 | PostgreSQL 15, migration trên bản sao dữ liệu thử: email hoa/thường/khoảng trắng, trùng kể cả đã xóa, email dài, tên NULL, sub khác hoa/thường. | Preflight dừng khi trùng/không hợp lệ, không tự xóa/gộp. Generated EmailKey và lookup cùng lower(btrim); không gộp dấu chấm/+alias Gmail. Unique email/sub, FK RESTRICT, check proof và xmin đúng; UserId/dữ liệu giữ nguyên. Không backfill từ IsEmailVerified đơn lẻ; mã cũ về 0. | TDD-AUTH-003/Data Model; ST-AUTH-025, 026, 039, 059. |
| 2 | PostgreSQL thật, hai login cùng sub/email; cạnh tranh register và CreateStaff; liên kết G→B trong lúc attempt chờ A. | Đúng một User cho email và một đích/sub; không nhân đôi thư mật khẩu. Conflict 409 không thay dữ liệu A/B. Khóa theo thứ tự đã thiết kế; timeout 5 giây không retry cùng DbContext. | ST-AUTH-047, 050, 051, 052. |
| 3 | SQL + MassTransit outbox: gây lỗi sau từng bước User, role, link, publish, trước commit; dùng một DbContext/UoW thật. | Tất cả rollback, không có event được phát ra từ giao dịch thất bại. Commit thành công nhưng Redis cấp phiên lỗi: User/link/event còn, API 503; lượt mới không sinh lại mật khẩu. ON CONFLICT không để tiếp tục transaction đã lỗi. | TDD-AUTH-003/Architecture; ST-AUTH-050–052. |
| 4 | Redis thật: callback/complete/verify-email đồng thời; mã được lấy đúng/sai; giới hạn 5 lượt/3 mail/30 start; restart và lỗi kho. | Lua reserve/consume/CAS chỉ có một finalizer; lần thứ năm đúng còn hợp lệ, sai thì đóng. Lease cũ không ghi đè version mới. Mã dùng rồi không chạy lại; counter không mất khi API restart; lỗi kho không bỏ qua bảo vệ. | TDD-AUTH-003/State Diagram; ST-AUTH-031, 050, 058, 060. |
| 5 | PostgreSQL + Redis + middleware thật: verify-account/reset/change/Google takeover tranh chấp trên cùng User; cache giữ stamp cũ. | Lệnh dùng proof cũ không sửa credential hoặc cấp phiên bằng stamp mới; xmin bắt writer cũ. Sau commit, access/refresh của mọi phương thức bị chặn. Lỗi DB trả 503 AuthStateUnavailable, không 401/xóa cookie/cho vào bằng cache. Không tuyên bố hủy ngược request đã được xác thực trước commit. | ST-AUTH-026, 035, 037, 059. |
| 6 | Redis thật: tác vụ dọn S2 đến sau khi đã có S3, đồng thời refresh và tạo phiên mới. | Lua xóa có điều kiện chỉ loại thế hệ cũ, giữ token/tập/TTL của S3; không blanket revoke. Cleanup lỗi không làm access cũ hợp lệ. Phiên thiếu/mâu thuẫn method/stamp bị từ chối; blob cũ suy ra đúng phương thức. | TDD-AUTH-003/Architecture; ST-AUTH-035, 056, 057. |
| 7 | API host thật với Carter, FluentValidation, serializer, middleware, cookie và policies. | DTO Google lạ (email/role/userId/clientKind/profile) trả 422; public DTO không vào transaction MediatR. Web start/complete/verify bắt Origin kể cả có Authorization; mobile không nhận cookie làm proof. 200 web bỏ hẳn trường token, mobile không Set-Cookie; 202/409 không cấp phiên mới. Named policy chặn temp/reset trước và sau refresh; me theo phiên và không ghi timezone khi hạn chế. | ST-AUTH-033, 034, 043, 055, 056, 060. |
| 8 | API/provider adapter thật với HTTP server kiểm thử và giới hạn body/query; đồng hồ/cancellation điều khiển. | Biên body 4 KiB, callback query 8 KiB, response Google 64 KiB, ID token 16 KiB; quá biên bị chặn trước log/deserialize. Token timeout 10 giây, callback budget 15 giây; POST token không retry, GET tối đa một retry trong budget. Callback invalid state không redirect; no-store/no-referrer đúng chỗ. | TDD-AUTH-003/Internal API, Error Handling; ST-AUTH-042, 053, 060. |
| 9 | Broker, outbox/error queue và Data Protection thật với certificate/keyring dành riêng cho môi trường thử; restart/rotation/khôi phục backup. | Dữ liệu lưu và event chỉ có ciphertext, không To/password/code/Body rõ. Khóa cũ giải mã được mail đang chờ và link chia sẻ; không fallback thư mục tạm khi bật Google. Envelope bị sửa/khóa thiếu không gửi. Queue lỗi event mới có retention 7 ngày; backup có ciphertext không được coi là đã xóa bí mật vĩnh viễn. | TDD-AUTH-003/Architecture; bổ sung kiểm kỹ thuật cho ST-AUTH-053/054, không coi hai ST đó tự đủ. |
| 10 | SMTP thử nghiệm timeout/crash sau gửi; reset commit giữa kiểm stamp và SMTP; thư quá cửa sổ gửi 24 giờ. | Retry dùng cùng D1/credential, không tạo mật khẩu mới; có thể nhận thư trùng. Thư đến muộn mang mật khẩu đã vô hiệu không khôi phục hash. Hết hạn gửi không hết hạn mật khẩu; Google login không đợi SMTP. Khi mail hoạt động lại, Quên mật khẩu vẫn khôi phục được. | ST-AUTH-032, 036, 037, 038, 051. |
| 11 | Web, Android và iOS thật; Google test project; verified HTTPS App/Universal Links; quay lại app hoặc app bị đóng giữa luồng. | Dùng trình duyệt ngoài, nhận đúng attempt/client; verifier không ở URL/log. Fragment được xóa, trang nhận mã không tải analytics. Tráo state/nonce/code/verifier giữa máy/phiên không tạo User/phiên; UI không dùng cookie cũ để coi 202 là thành công. | ST-AUTH-021–024, 029–031, 041–043, 058, 060. |
| 12 | Profile API/UI, logger API/proxy/Cloudflare và trace thực. | Email không sửa được trên web/app; gọi trực tiếp bị từ chối toàn bộ kể cả Staff. Tên NULL tương thích, tên render như text, ảnh tùy chọn. Không có bí mật trong query log/Location/body/token HTTP/structured exception/metric labels. | ST-AUTH-039, 040, 048, 049, 053. |

Các assertion bổ sung ở trên làm rõ fixture theo TDD đã chốt; khi viết automation phải triển khai chúng cùng ST liên quan hoặc tách ca integration có mã phù hợp, không báo đã phủ chỉ vì báo cáo có một dòng kế hoạch. Kiểm môi trường Google thật phải dùng tài khoản/hộp thư do nhóm thử nghiệm sở hữu.

**Danh mục ca mới**

| Mã | Nội dung | Unit dự kiến | Ưu tiên |
| --- | --- | --- | --- |
| [UT-AUTH-038](../unittest/UT-AUTH-038.md) | URL Google lấy đích trả về từ cấu hình | GoogleLoginCoordinator — bắt đầu luồng | P0 |
| [UT-AUTH-039](../unittest/UT-AUTH-039.md) | Biên định dạng challenge và nền tảng | Validator StartGoogleLoginCommand / StartMobileGoogleLoginCommand | P0 |
| [UT-AUTH-040](../unittest/UT-AUTH-040.md) | Biên proof hoàn tất | Validator CompleteGoogleLoginCommand | P0 |
| [UT-AUTH-041](../unittest/UT-AUTH-041.md) | Mã email giữ số 0 ở đầu | Validator VerifyGoogleEmailCommand | P1 |
| [UT-AUTH-042](../unittest/UT-AUTH-042.md) | Bật Google với cấu hình không hợp lệ | Bộ kiểm GoogleSignInOption / AuthMailProtectionOption | P1 |
| [UT-AUTH-043](../unittest/UT-AUTH-043.md) | Tắt Google không khởi tạo luồng mới | GoogleLoginCoordinator — kiểm feature flag | P1 |
| [UT-AUTH-044](../unittest/UT-AUTH-044.md) | State sai không cho redirect | GoogleLoginCoordinator — callback | P0 |
| [UT-AUTH-045](../unittest/UT-AUTH-045.md) | Hủy Google kết thúc đúng attempt | GoogleLoginCoordinator — callback hủy | P0 |
| [UT-AUTH-046](../unittest/UT-AUTH-046.md) | Callback hợp lệ chỉ trả mã hoàn tất | GoogleLoginCoordinator — callback thành công | P0 |
| [UT-AUTH-047](../unittest/UT-AUTH-047.md) | Lỗi đổi code không tự gửi lại POST | GoogleIdentityVerifier — trao đổi authorization code | P0 |
| [UT-AUTH-048](../unittest/UT-AUTH-048.md) | Kiểm chữ ký, issuer và thuật toán thật | GoogleIdentityVerifier — xác thực ID token | P0 |
| [UT-AUTH-049](../unittest/UT-AUTH-049.md) | Audience, azp và nonce phải đúng attempt | GoogleIdentityVerifier — ràng buộc claim | P0 |
| [UT-AUTH-050](../unittest/UT-AUTH-050.md) | Hạn token và kiểu claim | GoogleIdentityVerifier — thời gian và cấu trúc | P0 |
| [UT-AUTH-051](../unittest/UT-AUTH-051.md) | Phân loại bằng chứng sở hữu email Google | GoogleLoginCoordinator — phân loại email đã xác thực | P0 |
| [UT-AUTH-052](../unittest/UT-AUTH-052.md) | Khóa không tin cậy không có đường bỏ qua | GoogleIdentityVerifier — discovery/JWKS | P0 |
| [UT-AUTH-053](../unittest/UT-AUTH-053.md) | Mã hoàn tất phải đi kèm đúng client proof | GoogleLoginCoordinator — complete | P0 |
| [UT-AUTH-054](../unittest/UT-AUTH-054.md) | Attempt hết hạn hoặc đã dùng không hoàn tất lại | GoogleLoginCoordinator — kiểm attempt và quyền xử lý | P0 |
| [UT-AUTH-055](../unittest/UT-AUTH-055.md) | Hạn mã không vượt bằng chứng gốc | GoogleLoginCoordinator — tính thời điểm hết hạn | P1 |
| [UT-AUTH-056](../unittest/UT-AUTH-056.md) | Chờ email chưa được sửa tài khoản | GoogleLoginCoordinator — complete cần email bổ sung | P0 |
| [UT-AUTH-057](../unittest/UT-AUTH-057.md) | Mã email chỉ dùng trong đúng ngữ cảnh | Bộ kiểm HMAC mã email trong GoogleLoginCoordinator | P0 |
| [UT-AUTH-058](../unittest/UT-AUTH-058.md) | Lần nhập mã thứ năm đúng vẫn thành công | GoogleLoginCoordinator — verify-email | P0 |
| [UT-AUTH-059](../unittest/UT-AUTH-059.md) | Lượt cuối sai đóng bước email | GoogleLoginCoordinator — giới hạn nhập mã | P0 |
| [UT-AUTH-060](../unittest/UT-AUTH-060.md) | Hết hạn mức không gửi thư hoặc mở attempt mới | GoogleLoginCoordinator — hạn mức và lỗi kho bảo vệ | P1 |
| [UT-AUTH-061](../unittest/UT-AUTH-061.md) | Xếp thư xác minh thất bại không để attempt tiếp tục | GoogleLoginCoordinator — QueueGoogleVerificationEmailCommand | P0 |
| [UT-AUTH-062](../unittest/UT-AUTH-062.md) | Tạo Customer và credential tạm đúng một lần | CompleteGoogleSignInCommandHandler — nhánh tạo mới | P0 |
| [UT-AUTH-063](../unittest/UT-AUTH-063.md) | Liên kết giữ mật khẩu và dữ liệu đã xác minh | CompleteGoogleSignInCommandHandler — Customer đã xác minh | P0 |
| [UT-AUTH-064](../unittest/UT-AUTH-064.md) | Tiếp nhận email chưa có bằng chứng hợp lệ | CompleteGoogleSignInCommandHandler — Customer chưa xác minh | P0 |
| [UT-AUTH-065](../unittest/UT-AUTH-065.md) | Liên kết sub có sẵn thắng email Google mới | CompleteGoogleSignInCommandHandler — đã liên kết | P0 |
| [UT-AUTH-066](../unittest/UT-AUTH-066.md) | Tài khoản không đủ điều kiện bị chặn trước ghi | CompleteGoogleSignInCommandHandler — kiểm User đích | P0 |
| [UT-AUTH-067](../unittest/UT-AUTH-067.md) | Command SQL không tin phân nhánh trước transaction | CompleteGoogleSignInCommandHandler — điều kiện proof | P0 |
| [UT-AUTH-068](../unittest/UT-AUTH-068.md) | Đích đã quan sát khác liên kết hiện tại | CompleteGoogleSignInCommandHandler — xung đột liên kết | P0 |
| [UT-AUTH-069](../unittest/UT-AUTH-069.md) | Reset trong lúc chờ email làm proof cũ mất hiệu lực | CompleteGoogleSignInCommandHandler — kiểm stamp đã quan sát | P0 |
| [UT-AUTH-070](../unittest/UT-AUTH-070.md) | Thiếu role customer không tạo kết quả đăng nhập | CompleteGoogleSignInCommandHandler — thiếu dữ liệu cấu hình | P0 |
| [UT-AUTH-071](../unittest/UT-AUTH-071.md) | Lượt mới sau mất phản hồi dùng lại tài khoản | CompleteGoogleSignInCommandHandler — xử lý kết quả đã tồn tại | P0 |
| [UT-AUTH-072](../unittest/UT-AUTH-072.md) | Ghi tài khoản thất bại không được cấp phiên | GoogleLoginCoordinator — ranh giới command/issuer | P0 |
| [UT-AUTH-073](../unittest/UT-AUTH-073.md) | Lỗi cấp phiên không hoàn tác tài khoản đã commit | GoogleLoginCoordinator — issuer sau commit | P0 |
| [UT-AUTH-074](../unittest/UT-AUTH-074.md) | Tên thiếu hoặc một phần không chặn tạo khách hàng | Ánh xạ hồ sơ trong CompleteGoogleSignInCommandHandler | P1 |
| [UT-AUTH-075](../unittest/UT-AUTH-075.md) | Ảnh không hợp lệ được bỏ qua | Ánh xạ Avatar trong CompleteGoogleSignInCommandHandler | P1 |
| [UT-AUTH-076](../unittest/UT-AUTH-076.md) | Đổi email bị từ chối toàn bộ hồ sơ | UpdateUserProfileCommandHandler — nhánh email bất biến | P0 |
| [UT-AUTH-077](../unittest/UT-AUTH-077.md) | Email cùng khóa chuẩn hóa không bị ghi lại | UpdateUserProfileCommandHandler — giữ email hiện tại | P1 |
| [UT-AUTH-078](../unittest/UT-AUTH-078.md) | Sửa ảnh khi tên đang thiếu vẫn được phép | UpdateUserProfileCommandHandler và ánh xạ GetMe — tên tùy chọn | P1 |
| [UT-AUTH-079](../unittest/UT-AUTH-079.md) | Phiên Google không mang giới hạn mật khẩu tạm | AccessClaimsBuilder.BuildAsync — AuthenticationMethod.Google | P0 |
| [UT-AUTH-080](../unittest/UT-AUTH-080.md) | Password và EmailVerification giữ giới hạn đổi mật khẩu | AccessClaimsBuilder.BuildAsync — credential tạm | P0 |
| [UT-AUTH-081](../unittest/UT-AUTH-081.md) | Reset vẫn hạn chế và Staff không có phiên Google | AccessClaimsBuilder / AuthSessionIssuer — phương thức đặc biệt | P0 |
| [UT-AUTH-082](../unittest/UT-AUTH-082.md) | Refresh giữ phương thức và stamp, dựng lại quyền | AuthSessionIssuer.RefreshAsync — phiên mới có phương thức | P0 |
| [UT-AUTH-083](../unittest/UT-AUTH-083.md) | Blob phiên mới không được mâu thuẫn JWT | AuthSessionIssuer.RefreshAsync — kiểm auth context | P0 |
| [UT-AUTH-084](../unittest/UT-AUTH-084.md) | Phiên cũ chỉ suy ra Password hoặc PasswordReset | AuthSessionIssuer.RefreshAsync — tương thích blob cũ | P0 |
| [UT-AUTH-085](../unittest/UT-AUTH-085.md) | Proof trước đổi mật khẩu không cấp phiên bằng stamp mới | AuthSessionIssuer.IssueAsync — ExpectedSecurityStamp | P0 |
| [UT-AUTH-086](../unittest/UT-AUTH-086.md) | Stamp ở DB thắng giá trị cache cũ | SecurityStampValidator / ISecurityStampService.GetCurrentAsync | P0 |
| [UT-AUTH-087](../unittest/UT-AUTH-087.md) | DB lỗi phải khác credential không hợp lệ | SecurityStampValidator và callback JWT — lỗi dependency | P0 |
| [UT-AUTH-088](../unittest/UT-AUTH-088.md) | Me trả cờ theo phiên và không ghi timezone cho phiên hạn chế | GetMeQueryHandler — thông tin xác thực phiên | P1 |
| [UT-AUTH-089](../unittest/UT-AUTH-089.md) | Mật khẩu tạm không hết hạn theo tuổi | UserCredentialChecker.CheckAsync — credential tạm | P1 |
| [UT-AUTH-090](../unittest/UT-AUTH-090.md) | Mật khẩu mới rỗng bị chặn trước hash | ChangePassword validator — đổi và reset | P0 |
| [UT-AUTH-091](../unittest/UT-AUTH-091.md) | Không đổi mật khẩu tạm thành thường bằng chính bí mật cũ | ChangePasswordCommandHandler — so mật khẩu mới với hash hiện tại | P0 |
| [UT-AUTH-092](../unittest/UT-AUTH-092.md) | Đổi mật khẩu thay credential nhưng không xác minh email | ChangePasswordCommandHandler — nhánh mật khẩu hiện tại | P0 |
| [UT-AUTH-093](../unittest/UT-AUTH-093.md) | Reset thành công dùng proof đúng email hiện tại | ChangePasswordCommandHandler — nhánh PasswordReset | P0 |
| [UT-AUTH-094](../unittest/UT-AUTH-094.md) | Đổi mật khẩu sai không làm mất credential đang có | ChangePasswordCommandHandler — lỗi kiểm credential | P0 |
| [UT-AUTH-095](../unittest/UT-AUTH-095.md) | Lệnh đổi hoặc reset dùng stamp cũ bị từ chối | ChangePasswordCommandHandler — kiểm proof dưới khóa User | P0 |
| [UT-AUTH-096](../unittest/UT-AUTH-096.md) | Phiên reset thiếu binding email phải xác minh lại | ChangePasswordCommandHandler — kiểm bằng chứng reset | P0 |
| [UT-AUTH-097](../unittest/UT-AUTH-097.md) | Mã xác minh và reset bằng 0 không hợp lệ | Các handler VerifyAccount / VerifyForgotPasswordCode — kiểm mã | P0 |
| [UT-AUTH-098](../unittest/UT-AUTH-098.md) | Xác minh hợp lệ tạo proof gắn email và stamp | Các handler VerifyAccount / VerifyForgotPasswordCode — tiêu thụ mã | P0 |
| [UT-AUTH-099](../unittest/UT-AUTH-099.md) | Dọn phiên theo stamp hiện tại khi tác vụ đến muộn | SessionCutter — RevokeOlderGenerationsAsync | P0 |
| [UT-AUTH-100](../unittest/UT-AUTH-100.md) | Credential tạm được băm và không đưa ra kết quả | Nhánh sinh credential trong CompleteGoogleSignInCommandHandler | P0 |
| [UT-AUTH-101](../unittest/UT-AUTH-101.md) | Thư được bảo vệ trước khi publish | Bộ tạo SendProtectedAuthEmailV1 trong command | P0 |
| [UT-AUTH-102](../unittest/UT-AUTH-102.md) | Không publish khi bảo vệ payload thất bại | Bộ tạo SendProtectedAuthEmailV1 — lỗi protector | P0 |
| [UT-AUTH-103](../unittest/UT-AUTH-103.md) | Envelope bị tráo không gửi SMTP | SendProtectedAuthEmailV1 consumer — xác thực payload | P0 |
| [UT-AUTH-104](../unittest/UT-AUTH-104.md) | Bỏ thư mật khẩu thuộc credential cũ hoặc hết hạn gửi | SendProtectedAuthEmailV1 consumer — kiểm thư mật khẩu | P0 |
| [UT-AUTH-105](../unittest/UT-AUTH-105.md) | Mã bổ sung chỉ được gửi khi attempt còn chờ đúng mã | SendProtectedAuthEmailV1 consumer — kiểm thư xác minh | P0 |
| [UT-AUTH-106](../unittest/UT-AUTH-106.md) | Giao lại event không sinh mật khẩu mới | SendProtectedAuthEmailV1 consumer — tái giao | P1 |
| [UT-AUTH-107](../unittest/UT-AUTH-107.md) | Dữ liệu hồ sơ được escape trong thư | Template của SendProtectedAuthEmailV1 consumer | P1 |
| [UT-AUTH-108](../unittest/UT-AUTH-108.md) | Log của luồng Google không chứa bí mật | GoogleLoginCoordinator / verifier / consumer — bản ghi lỗi và tracing | P0 |
| [UT-AUTH-109](../unittest/UT-AUTH-109.md) | Kết quả Google tách đúng cookie web và JSON mobile | GoogleAuthApi — hàm ánh xạ kết quả hoàn tất | P0 |
| [UT-AUTH-110](../unittest/UT-AUTH-110.md) | Mã lỗi mới giữ đúng HTTP status | Ánh xạ lỗi Google/Auth trong ExceptionHandlingMiddleware | P1 |

**Kiểm tra tài liệu đã thực hiện**

Đã kiểm 73 file mới và 8 file cập nhật theo template: một dòng test/file, 13 cột, mã duy nhất, Reviewer/Approver, giá trị phân loại và giới hạn trường. Trace to khớp TEST_LINKS; mã và section đích tồn tại; liên kết file trong báo cáo và TDD hợp lệ. Các ca mới có tham chiếu tới 15/15 AC. Không phát hiện lỗi trong các kiểm tra này; chưa chạy chức năng kiểm tra import của ứng dụng tài liệu.

Đây là kiểm cấu trúc và đối chiếu đặc tả, không phải kết quả thực thi của 73 ca mới, tám ca được sửa hay 12 nhóm integration.

**Thứ tự triển khai và phần chưa kiểm chứng**

1. Đổi dữ liệu và đồng bộ các writer User/credential, giữ Google tắt; kiểm migration trên bản sao dữ liệu thử.
2. Làm proof/stamp/phương thức phiên, email bất biến và đổi/reset; cập nhật 8 ca cũ, chạy hồi quy AUTH/RBAC chịu ảnh hưởng.
3. Làm OIDC, attempt và command liên kết; chạy unit rồi PostgreSQL/Redis integration cho các nhánh tranh chấp.
4. Làm thư mã hóa và xác minh bổ sung; kiểm broker, SMTP, khóa và lỗi sau commit.
5. Tích hợp web/app, chạy 40 System Test với contract đã chốt và các phép kiểm bổ sung ở trên trước khi bật Google.

Còn cần phân công Owner của test/BR; Story còn Priority, Sprint, Assignee; BR còn Effective Date. Các trường này giữ placeholder, không tự lấy tên Reviewer làm người triển khai. Cấu hình môi trường cần Google client/callback, domain association Android/iOS, nơi lưu và sao lưu khóa/certificate; không ghi secret thật vào tài liệu. Chưa có mã nguồn mobile để xác định framework hoặc thư viện giao diện.

Phạm vi đối chiếu gồm AUTH trực tiếp và các phần RBAC/phiên được TDD dẫn tới; đây không phải xác nhận đã rà nội dung toàn bộ đồ thị tài liệu BMT. Kiểm liên kết tồn tại không thay việc đọc nguồn khi triển khai. Trước khi viết code phải đọc toàn bộ TDD-AUTH và các tài liệu được dẫn tới theo hướng dẫn workspace.
