# Đặc tả Unit Test cho hồ sơ công trình mở rộng

Người dùng đã chốt bộ TDD công trình sau lần bàn giao. Đợt này bổ sung **73 đặc tả UT-SITE-045–117** và cập nhật **53 đặc tả hiện có** (39 SITE, 14 PROJ), tổng cộng 126 file được rà cấu trúc và liên kết. Đây là **đặc tả chưa thực thi**; không phải 126 test đã chạy hoặc đã đạt. Không sửa mã ứng dụng, sinh/chạy migration, import tài liệu hoặc viết mã test trong tác vụ này.

Căn cứ chốt và phạm vi tài liệu được ghi tại [bàn giao TDD](construction-site-technical-design.md#ghi-nhận-chốt-tdd). Status Draft và tên Reviewer/Approver trong file không tạo phê duyệt trên Document First. Các Owner chưa được phân công vẫn giữ `[Chưa xác định]`; không tự gán người phụ trách. Nguồn làm việc là Markdown cục bộ theo hướng dẫn BMT, không dùng bản từ project từ xa để ghi đè. Không có approvalId hệ thống.

## Ghi nhận chốt đặc tả Unit Test

Người dùng đã xác nhận bộ đặc tả này bằng phản hồi “chốt” sau lần bàn giao 73 ca mới và 53 ca cập nhật. Phạm vi gồm UT-SITE-045–117, UT-SITE-003–017,020–023,025–044 và UT-PROJ-086–096,098–099,116. Đây là xác nhận nội dung đặc tả để làm căn cứ triển khai; không phải kết quả chạy test hoặc phê duyệt/import trên Document First. Giữ nguyên Status Draft và nội dung các ca đã bàn giao. UT-SITE-024 đã bỏ vẫn không thuộc phạm vi nghiệm thu.

## Phạm vi đơn vị kiểm thử

| Nhóm | Đặc tả | Kết quả cần bảo vệ |
|---|---|---|
| Hồ sơ và phân loại | UT-SITE-045–056, 065–068, 116 | Bắt buộc, biên số, Không áp dụng, revision cũ và địa chỉ ba phần. |
| Nguồn dự toán | UT-SITE-057–064, 076–081, 105 | Snapshot hoàn tất, khóa dữ liệu nguồn, chặn xóa và replay receipt. |
| Hiện trạng | UT-SITE-069–075 | Admin-only, lựa chọn active, giữ tên cũ, version và ngừng cho chọn. |
| Upload và liên kết tệp | UT-SITE-082–105, 117 | Định dạng/bytes, private final, binding đúng chủ/đích, tổng 9 và giữ tệp cũ khi lỗi. |
| Đọc tệp và MEDIA | UT-SITE-106–115 | Quyền mỗi request, thu hồi phạm vi, HEAD/Range, không public URL và không dọn nhầm final SITE. |

Mã UT-SITE bao gồm cả các ca phối hợp với handler xóa PROJ và policy MEDIA vì chúng thuộc đợt mở rộng SITE này; Unit under test ghi rõ thành phần thực tế. Mỗi file chứa một dòng dữ liệu 13 cột, dữ liệu nền cụ thể, test double, input và kỳ vọng. Các biến thể biên của cùng một quy tắc được nêu trong cùng ca và phải dùng fixture độc lập khi triển khai.

## Đối chiếu từng nhóm điều khoản

| Nguồn | Điều khoản và ngoại lệ | Unit Test | Phần cần tầng kiểm thử khác |
|---|---|---|---|
| BR-SITE-001 | 1–2: NFC/trim, tên 200, số nhà–đường 500 | SITE-001–007 đã có; SITE-004/006 cập nhật; SITE-067 | Kiểu cột địa chỉ gộp và snapshot lịch sử ở PostgreSQL. |
| BR-SITE-001 | 3,7–8: địa chỉ hành chính, tọa độ khi tạo/đổi | SITE-035–044 cập nhật, 065–067 | ST-SITE-030–037,054,056; UI loại phản hồi bản đồ đến muộn, SQL lưu đồng bộ. |
| BR-SITE-001 | 4–6: tên theo owner, không hạn mức, từ chối không lưu một phần | SITE-009–019 cập nhật, 060,081,103 | UNIQUE tên/nguồn và rollback cần PostgreSQL; ST-SITE-019,051,055,082. |
| BR-SITE-001 | 9: diện tích đất, từ chối quá 2 số lẻ | SITE-045,046,057 | API chuỗi decimal và numeric thực; ST-SITE-039. |
| BR-SITE-001 | 10–12: hiện trạng, tiền VND, khởi công | SITE-047–049,069–075 | Seed ba mục và hiển thị VND ở browser; ST-SITE-040,042,057. |
| BR-SITE-001 | 13–15 và Except: phân loại bắt buộc, nguồn/tệp tùy chọn | SITE-048,050–053,060–061,102 | ST-SITE-038,041,043–045,064. |
| BR-SITE-002 | 1–2,6 và Except: chỉ owner Customer được ghi | SITE-010,015,026 cập nhật,100 | Kiểm auth/Origin/route thật. Không quyền staff nào mở ghi hộ. |
| BR-SITE-002 | 3–5,8: gói giữ chỗ khóa hồ sơ và tệp | SITE-012,016–017,027–032 cập nhật,092,101,105 | Khóa site FOR UPDATE đối chọi assign FOR KEY SHARE và lịch sử gói thật; ST-SITE-020–021,026–029,070,078. |
| BR-SITE-002 | 7,9: nhả gói không nhả khóa nguồn; xóa giải phóng nguồn | SITE-062–064,079,104–105 | FK/UNIQUE và xóa cứng thật; ST-SITE-049–050,071,080, ST-PROJ-111–114. |
| BR-SITE-003 | 1–7 và Except: scope owner/staff, quyền rộng nhất, commerce.read không đủ | SITE-020–023,033–034 cập nhật; 100,106–107 | Query JOIN/filter thật, dữ liệu nhiều owner; không dùng InMemory chứng minh SQL. |
| BR-SITE-003 | 8–10: detail đủ dữ liệu, quyền nguồn độc lập, tải theo quyền hiện tại | SITE-068,106–110,117 | HTTP sau kết thúc/chuyển giao, link nguyên trạng; ST-SITE-072–075,079–080. |
| BR-SITE-004 | 1,12: lọc nguồn và kiểm lại lúc lưu | SITE-058–059,081 | Danh sách/pagination SQL, source-vs-delete và source-vs-source; ST-SITE-046,051,055. |
| BR-SITE-004 | 2–4,8: snapshot hoàn tất, không giả mạo hoặc đồng bộ lại | SITE-057,060–063,068 | API bind unknown members; ST-SITE-045,048–049,052. |
| BR-SITE-004 | 5: đổi/bỏ nguồn trước tạo | SITE-048,054,057,061,116 kiểm request đích của mỗi nhánh | Thao tác form và dữ liệu chưa lưu ở ST-SITE-047; không giả làm test unit backend. |
| BR-SITE-004 | 6–7: FK cùng owner, một nguồn, bất biến sau tạo | SITE-062,064,080 kiểm policy/ánh xạ lỗi | FK ghép, UNIQUE và trigger cần PostgreSQL; ST-SITE-048–051. |
| BR-SITE-004 | 9–10: chặn xóa rồi giải phóng nguồn | SITE-076–081,105; PROJ-086–096,098–099,116 cập nhật fixture | ST-PROJ-110–114; không coi fake chứng minh transaction. |
| BR-SITE-004 | 11 và Except: không sao file, không cấp quyền nguồn, không dùng quota | SITE-057,060,068,102,106 | ST-SITE-076; API kết quả PROJ vẫn kiểm owner. |
| BR-SITE-005 | 1–3,6–7: ID chung, revision ghim, lựa chọn đúng loại/nhóm | SITE-050–057,068 | FK catalog thật, đọc nhãn từ đúng revision; ST-SITE-044,052–053. |
| BR-SITE-005 | 4–5,8 và Except: Không áp dụng, thiếu loại hiện hành | SITE-050–053,116 | ST-SITE-043,083; Admin PROJ vẫn kiểm nhóm bật không rỗng. |
| BR-SITE-006 | 1: ba mục seed | Không dùng unit giả lập để chứng minh migration | ST-SITE-057 và kiểm seed DB thực. |
| BR-SITE-006 | 2–5,7 và Except: quyền, thêm/đổi/sắp, giữ mục inactive | SITE-069–075 | ST-SITE-058–060,062–063; chọn-vs-deactivate với khóa thật. |
| BR-SITE-006 | 6: không xóa mục đã dùng | Không công bố DELETE; unit policy không chứng minh FK RESTRICT | ST-SITE-061 và PostgreSQL FK; route integration kiểm không có thao tác xóa. |
| BR-SITE-007 | 1–4: nhóm, định dạng, 9 tệp, 10.000.000 byte | SITE-048,082–086,093,096–097 | Parser thật, MIME/bytes giả đuôi; ST-SITE-064–069,077,081. |
| BR-SITE-007 | 5–6,9: owner/gói, kiểm lại lúc lưu, không lấy tệp khác | SITE-087–088,092–095,099–101 | ST-SITE-070–071,078–079 và PostgreSQL tranh chấp assign/attach. |
| BR-SITE-007 | 7–8 và Except: quyền tải mới, không thu hồi bytes đã tải | SITE-106–110,112–113,117 | Private origin/preview/CDN và stream thật; ST-SITE-072–075. |
| BR-SITE-007 | 10–11: thay/tạo nguyên tử, file lỗi không lưu hồ sơ một phần | SITE-090–091,097–098,102–103 | Commit rollback/failure và object mồ côi cần DB/storage integration; ST-SITE-066,082. |
| BR-SITE-007 | 12–13: không sao nguồn, gỡ chặn đọc, không dọn mọi bản vẽ | SITE-102,104–105,114–115,117 | ST-SITE-076,080; worker/provider thật, retention final ngoài phạm vi. |
| BR-PROJ-009 | Điều kiện SITE, phân loại, replay và lỗi store | SITE-076–081; hồi quy PROJ giữ điều kiện không có SITE | ST-PROJ-093–114. |
| BR-MEDIA-001/002 | Ngoại lệ private SITE và cleanup final | SITE-089–091,112–115 | ST-MEDIA-011, bucket/policy/registry thật. |

Các mã SITE/PROJ trong cột Unit Test là UT-SITE/UT-PROJ. Tập UT không bao gồm UT-SITE-024 đã bỏ; giữ nguyên file đó làm lịch sử, không đưa lại vào nghiệm thu.

## Tái sử dụng và cập nhật đặc tả cũ

39 file SITE được cập nhật: UT-SITE-003–017,020–023,025–044. Bổ sung fixture hồ sơ đầy đủ, source NULL cho ca độc lập, địa chỉ ba phần và revision áp dụng. Các ca địa chỉ 500/501 chuyển sang AddressDetail; saved response chỉ đòi ID/version theo contract mới. Giữ ghi chú về mã test cũ nhưng đánh dấu cần cập nhật/chạy lại; không dùng kết quả cũ làm bằng chứng cho hồ sơ mới.

14 file PROJ được cập nhật: UT-PROJ-086–096,098–099,116. Chúng tiếp tục kiểm mục tiêu xóa/replay cũ với HasConstructionSite=false; receipt lịch sử v1 giữ nguyên, writer mới dùng v2. Các nhánh SITE mới nằm tại UT-SITE-076–081. Không sửa nhóm Unit Test tọa độ dự toán đang được làm riêng trong workspace.

Nền test hiện có là .NET 8, xUnit 2.5.3, NSubstitute 5.3.0 và FluentAssertions 8.9.0; không đổi framework theo ví dụ của skill. Mã có thể dùng lại khi triển khai:

- [ConstructionSiteTests.cs](../../bmt-be/test/bmt-be.application.tests/usecases/constructionSite/ConstructionSiteTests.cs): chuẩn hóa tên, quyền, gói giữ chỗ và tọa độ. Fixture hiện có phải mở rộng trước khi dùng cho contract mới.
- [MyEstimatesTests.cs](../../bmt-be/test/bmt-be.application.tests/usecases/estimate/MyEstimatesTests.cs): xóa lô, key, replay, lỗi store; thêm state SITE/receipt v2.
- [ConstructionSiteConstraintTests.cs](../../bmt-be/test/bmt-be.integration.tests/ConstructionSiteConstraintTests.cs), [MyEstimatesFlowTests.cs](../../bmt-be/test/bmt-be.integration.tests/MyEstimatesFlowTests.cs): bổ sung FK/trigger/source và tranh chấp bằng PostgreSQL thật.
- [MediaPolicyTests.cs](../../bmt-be/test/bmt-be.infrastructure.tests/files/MediaPolicyTests.cs), [MediaStorageProtocolTests.cs](../../bmt-be/test/bmt-be.infrastructure.tests/files/MediaStorageProtocolTests.cs), [MediaApiPipelineTests.cs](../../bmt-be/test/bmt-be.api.tests/security/MediaApiPipelineTests.cs): nền policy/storage/HTTP; bổ sung purpose và guard private SITE.

UT-SITE-025 là đặc tả kiểm cổng khóa bằng PostgreSQL thật đã có dù nằm trong thư mục unittest. Đợt này chỉ mở rộng fixture; không đổi nó thành test dùng mock hoặc coi nó là bằng chứng đã chạy theo schema mới.

## Phần chưa được chứng minh

- Chưa viết/chạy mã Unit Test mới. Cổng parser CAD/PDF, streaming private và binding SITE là thiết kế; test double chỉ kiểm cách service phản ứng với kết quả phụ thuộc, không chứng minh parser đọc file thật hay ACL đúng.
- Chưa chạy FK, trigger, unique, transaction, rollback hoặc concurrency trên PostgreSQL. ST liên quan đã tồn tại và được chốt; không thay chúng bằng phép đếm/mô phỏng trong bộ UT.
- Cần HTTP/browser để kiểm unknown JSON members, định dạng VND, chọn/bỏ nguồn, callback geocoding đến muộn, cookie/Origin và cache/download headers thật.
- Các policy tạm gọi bằng tên đơn vị dự kiến của TDD; khi triển khai phải ánh xạ sang hàm/service thực mà không đổi kỳ vọng nghiệp vụ. Không tự tạo tên API hoặc mã lỗi mới qua đặc tả test.
- Phạm vi đọc/đối chiếu tập trung tài liệu SITE và các phần PROJ/MEDIA/SUB liên quan; kiểm liên kết tự động không phải tuyên bố đã đọc lại mọi tài liệu bắc cầu AUTH/RBAC/PAY/CTR/LIB. Không báo kiểm toán toàn bộ workspace đã hoàn tất.

## Danh sách đặc tả mới

| Mã | Đơn vị và hành vi |
|---|---|
| [UT-SITE-045](../unittest/UT-SITE-045.md) | ConstructionSiteProfilePolicy — diện tích đất ở biên biểu diễn |
| [UT-SITE-046](../unittest/UT-SITE-046.md) | ConstructionSiteProfilePolicy — diện tích không được làm tròn để vượt kiểm tra |
| [UT-SITE-047](../unittest/UT-SITE-047.md) | ConstructionSiteProfilePolicy — ngân sách là một số nguyên VND |
| [UT-SITE-048](../unittest/UT-SITE-048.md) | CreateConstructionSiteCommandValidator — trường bắt buộc và ngoại lệ |
| [UT-SITE-049](../unittest/UT-SITE-049.md) | ConstructionSiteProfilePolicy — bốn lựa chọn khởi công không phải ngày |
| [UT-SITE-050](../unittest/UT-SITE-050.md) | ConstructionSiteProfilePolicy — tầng bắt buộc khi áp dụng |
| [UT-SITE-051](../unittest/UT-SITE-051.md) | ConstructionSiteProfilePolicy — Không tum khác Không áp dụng |
| [UT-SITE-052](../unittest/UT-SITE-052.md) | ConstructionSiteProfilePolicy — phong cách phải đúng loại và nhóm |
| [UT-SITE-053](../unittest/UT-SITE-053.md) | ConstructionSiteProfilePolicy — nhóm phong cách tắt hoặc rỗng |
| [UT-SITE-054](../unittest/UT-SITE-054.md) | CreateConstructionSiteCommandHandler — kiểm revision tại thời điểm tạo độc lập |
| [UT-SITE-055](../unittest/UT-SITE-055.md) | UpdateConstructionSiteCommandHandler — dùng revision đã ghim |
| [UT-SITE-056](../unittest/UT-SITE-056.md) | ConstructionSiteProfilePolicy — đổi loại trên công trình độc lập |
| [UT-SITE-057](../unittest/UT-SITE-057.md) | ConstructionSiteEstimateSourceReader — lấy đầu vào hoàn tất v1 và v2 |
| [UT-SITE-058](../unittest/UT-SITE-058.md) | ConstructionSiteEstimateSourceReader — nguồn không đủ điều kiện |
| [UT-SITE-059](../unittest/UT-SITE-059.md) | ConstructionSiteEstimateSourceReader — dữ liệu nguồn không được vá bằng catalog hiện hành |
| [UT-SITE-060](../unittest/UT-SITE-060.md) | CreateConstructionSiteCommandHandler — tạo từ nguồn không dùng quyền hoặc lượt AI |
| [UT-SITE-061](../unittest/UT-SITE-061.md) | CreateConstructionSiteCommandValidator — không nhận profile tự khai khi có nguồn |
| [UT-SITE-062](../unittest/UT-SITE-062.md) | UpdateConstructionSiteCommandHandler — nguồn và trường nguồn không được đổi |
| [UT-SITE-063](../unittest/UT-SITE-063.md) | UpdateConstructionSiteCommandHandler — trường nhập riêng vẫn sửa được khi nguồn bị khóa |
| [UT-SITE-064](../unittest/UT-SITE-064.md) | UpdateConstructionSiteCommandHandler — không thêm nguồn cho công trình độc lập |
| [UT-SITE-065](../unittest/UT-SITE-065.md) | ConstructionSiteProfilePolicy — địa chỉ mới kiểm tỉnh và xã |
| [UT-SITE-066](../unittest/UT-SITE-066.md) | UpdateConstructionSiteCommandHandler — địa chỉ cũ không đổi giữ snapshot |
| [UT-SITE-067](../unittest/UT-SITE-067.md) | ConstructionSiteMapping — địa chỉ gộp không bị cắt 500 |
| [UT-SITE-068](../unittest/UT-SITE-068.md) | ConstructionSiteReadModel — nhãn, khả năng sửa và quyền mở nguồn |
| [UT-SITE-069](../unittest/UT-SITE-069.md) | ConstructionSiteConditionPolicy — chọn mới chỉ nhận mục active |
| [UT-SITE-070](../unittest/UT-SITE-070.md) | UpdateConstructionSiteCommandHandler — giữ hiện trạng cũ sau đổi tên hoặc ngừng |
| [UT-SITE-071](../unittest/UT-SITE-071.md) | ConstructionSiteConditionPolicy — tên và thứ tự ở biên kỹ thuật |
| [UT-SITE-072](../unittest/UT-SITE-072.md) | ConditionAdmin authorization policy — quyền Admin không cấp quyền ghi hộ site |
| [UT-SITE-073](../unittest/UT-SITE-073.md) | Condition handlers và projection — thêm mục và sắp xếp ổn định |
| [UT-SITE-074](../unittest/UT-SITE-074.md) | DeactivateConditionCommandHandler — chuyển active và no-op inactive |
| [UT-SITE-075](../unittest/UT-SITE-075.md) | UpdateConditionCommandHandler — version hoặc mục không tồn tại |
| [UT-SITE-076](../unittest/UT-SITE-076.md) | DeleteMyEstimatesCommandHandler — trả lý do riêng cho dự toán đang được SITE dùng |
| [UT-SITE-077](../unittest/UT-SITE-077.md) | DeleteMyEstimatesCommandHandler — ưu tiên phân loại nguồn dưới khóa |
| [UT-SITE-078](../unittest/UT-SITE-078.md) | EstimateDeletionReceipt codec — tương thích hai phiên bản |
| [UT-SITE-079](../unittest/UT-SITE-079.md) | DeleteMyEstimatesCommandHandler — replay không đánh giá lại nguồn đã được giải phóng |
| [UT-SITE-080](../unittest/UT-SITE-080.md) | ConstraintViolationPipelineBehavior — unique nguồn khác unique tên |
| [UT-SITE-081](../unittest/UT-SITE-081.md) | CreateConstructionSiteCommandHandler — không dùng trạng thái nguồn đọc trước khóa |
| [UT-SITE-082](../unittest/UT-SITE-082.md) | ConstructionSiteFileValidator — các định dạng hợp lệ theo nhóm |
| [UT-SITE-083](../unittest/UT-SITE-083.md) | ConstructionSiteFileValidator — nhóm sai và giả đuôi |
| [UT-SITE-084](../unittest/UT-SITE-084.md) | ConstructionSiteFileValidator — giới hạn 10 MB theo bytes thực |
| [UT-SITE-085](../unittest/UT-SITE-085.md) | ConstructionSiteFileValidator — rỗng hoặc sai khai báo không được gắn |
| [UT-SITE-086](../unittest/UT-SITE-086.md) | ConstructionSiteUploadService — parser không hỗ trợ khác với file hỏng |
| [UT-SITE-087](../unittest/UT-SITE-087.md) | ConstructionSiteUploadService — cấp ticket đúng ngữ cảnh |
| [UT-SITE-088](../unittest/UT-SITE-088.md) | ConstructionSiteUploadService — key issue phải bao gồm nhóm và đích |
| [UT-SITE-089](../unittest/UT-SITE-089.md) | ConstructionSiteUploadService — ghi final riêng tư và không công bố origin |
| [UT-SITE-090](../unittest/UT-SITE-090.md) | ConstructionSiteUploadService — complete replay và mất lease |
| [UT-SITE-091](../unittest/UT-SITE-091.md) | ConstructionSiteUploadService — lỗi storage không làm thay tệp cũ |
| [UT-SITE-092](../unittest/UT-SITE-092.md) | ConstructionSiteUploadService — gói gán trong khi upload chặn complete/attach |
| [UT-SITE-093](../unittest/UT-SITE-093.md) | SiteAttachment policy — chỉ nhận upload đã hoàn tất đúng object |
| [UT-SITE-094](../unittest/UT-SITE-094.md) | SiteAttachment policy — không lấy ticket của khách hoặc công trình khác |
| [UT-SITE-095](../unittest/UT-SITE-095.md) | SiteAttachment policy — binding đã dùng không được tái sử dụng |
| [UT-SITE-096](../unittest/UT-SITE-096.md) | SiteAttachment slot allocation — tổng hai nhóm dùng chung 9 vị trí |
| [UT-SITE-097](../unittest/UT-SITE-097.md) | ReplaceSiteAttachmentCommandHandler — thay khi đã đủ 9 |
| [UT-SITE-098](../unittest/UT-SITE-098.md) | ReplaceSiteAttachmentCommandHandler — lỗi validation giữ liên kết cũ |
| [UT-SITE-099](../unittest/UT-SITE-099.md) | SiteAttachment handlers — version cũ không đổi file |
| [UT-SITE-100](../unittest/UT-SITE-100.md) | SiteAttachment write authorization — không ghi hộ |
| [UT-SITE-101](../unittest/UT-SITE-101.md) | SiteAttachment handlers — chỉ gói giữ chỗ khóa file |
| [UT-SITE-102](../unittest/UT-SITE-102.md) | CreateConstructionSiteCommandHandler — consume file NewSite cùng hồ sơ |
| [UT-SITE-103](../unittest/UT-SITE-103.md) | CreateConstructionSiteCommandHandler — kiểm đủ danh sách upload trước ghi |
| [UT-SITE-104](../unittest/UT-SITE-104.md) | DeleteSiteAttachmentCommandHandler — gỡ chỉ liên kết hiện hành |
| [UT-SITE-105](../unittest/UT-SITE-105.md) | DeleteConstructionSiteCommandHandler — giải phóng nguồn, bỏ tệp, giữ lịch sử |
| [UT-SITE-106](../unittest/UT-SITE-106.md) | ConstructionSiteFileReadService — đọc theo quyền hiện tại |
| [UT-SITE-107](../unittest/UT-SITE-107.md) | ConstructionSiteFileReadService — người không quyền không chạm kho |
| [UT-SITE-108](../unittest/UT-SITE-108.md) | ConstructionSiteFileReadService — quyền bị thu hồi giữa hai request |
| [UT-SITE-109](../unittest/UT-SITE-109.md) | ConstructionSiteFileReadService — HEAD, Range và điều kiện đều kiểm quyền |
| [UT-SITE-110](../unittest/UT-SITE-110.md) | File response mapping — đường bảo vệ và header không cache |
| [UT-SITE-111](../unittest/UT-SITE-111.md) | ConstructionSiteFileReadService — lỗi kho khác lỗi quyền người dùng |
| [UT-SITE-112](../unittest/UT-SITE-112.md) | Generic MediaUploadService guard — purpose SITE không đi đường public |
| [UT-SITE-113](../unittest/UT-SITE-113.md) | Media URL resolver — prefix SITE bị loại dù host được phép |
| [UT-SITE-114](../unittest/UT-SITE-114.md) | Media cleanup policy — final SITE ngoài cleanup ảnh tự động |
| [UT-SITE-115](../unittest/UT-SITE-115.md) | SITE media reference producer — nguồn là attachment hiện hành |
| [UT-SITE-116](../unittest/UT-SITE-116.md) | CreateConstructionSiteCommandHandler — catalog hiện hành rỗng không phá nguồn cũ |
| [UT-SITE-117](../unittest/UT-SITE-117.md) | ConstructionSiteUploadService — status sau gỡ tệp hoặc xóa site |

## Mốc nguồn và kết quả kiểm tra tài liệu

Mỗi đặc tả mới lưu `documentKey#SHA-256` của BR/TDD nguồn trong Rationale. Đây là hash nội dung local, không phải số phiên bản/approvalId trên hệ thống. Các bổ sung ghi nhận chốt trong TDD không đổi thiết kế đã chốt.

- Kiểm 126 file: đúng một test/file, 13 cột, mã/enum/metadata và Trace to trùng TEST_LINKS.
- Kiểm 426 tham chiếu mã/section và 156 mốc hash nguồn; không phát hiện đích thiếu, sai section hoặc hash lệch. Không phát hiện mã tài liệu trùng.
- 105 file System Test đã chốt giữ nguyên hash so với đầu tác vụ. `git diff --check` không báo lỗi khoảng trắng.
- Chưa chạy importer Document First hoặc test chức năng; các kiểm tra trên chỉ xác nhận cấu trúc và liên kết đặc tả.
