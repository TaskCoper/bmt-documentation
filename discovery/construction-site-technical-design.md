# Thiết kế kỹ thuật hồ sơ công trình mở rộng

Ngày bàn giao: 01/10/2026. Căn cứ là US/BR và 105 đặc tả System Test người dùng đã chốt. **Người dùng đã chốt bộ TDD mở rộng này bằng phản hồi “chốt” sau lần bàn giao**; tại thời điểm bàn giao TDD chưa viết đặc tả Unit Test mới; chưa sửa mã ứng dụng, sinh/chạy migration, import tài liệu hoặc chạy test trong tác vụ này.

## Tài liệu để đọc và phạm vi thay thế

| TDD | Phạm vi |
|---|---|
| [TDD-SITE-003](../tdd/TDD-SITE-003.md) | Hồ sơ đầy đủ, FK nguồn, snapshot đầu vào hoàn tất, catalog đã ghim, API và tranh chấp tạo–xóa nguồn. |
| [TDD-SITE-004](../tdd/TDD-SITE-004.md) | Admin quản lý hiện trạng, snapshot tên và ngừng cho chọn. |
| [TDD-SITE-005](../tdd/TDD-SITE-005.md) | Upload riêng tư, 9 slot, 10.000.000 byte, thay tệp nguyên tử và quyền tải hiện tại. |
| [TDD-PROJ-005](../tdd/TDD-PROJ-005.md) | Thêm ConstructionSiteInUse vào kết quả từng dự toán, receipt v2, guard SQL và phối hợp khóa. |
| [TDD-SITE-001](../tdd/TDD-SITE-001.md), [TDD-SITE-002](../tdd/TDD-SITE-002.md) | Ghi rõ contract cũ được thay bởi SITE-003; giữ quyền/gói, tọa độ và chức năng bán kính đã có. |
| [TDD-PROJ-001](../tdd/TDD-PROJ-001.md), [TDD-MEDIA-001](../tdd/TDD-MEDIA-001.md) | Ghi điểm tích hợp catalog/nguồn và ngoại lệ tệp SITE private. |
| [TDD-SUB-005](../tdd/TDD-SUB-005.md), [TDD-SUB-007](../tdd/TDD-SUB-007.md) | Nới cột địa chỉ trong snapshot lịch sử gói để không cắt địa chỉ đầy đủ. |

Sáu Story SITE-001/002/003/004, PROJ-007 và MEDIA-001 đã được bổ sung liên kết TDD hai chiều. Nội dung nghiệp vụ/AC và đặc tả System Test đã chốt không được đổi trong tác vụ TDD này. Hash Story trong báo cáo ST cũ là mốc lúc soạn ST; việc thêm References/TDDs làm hash file mới khác mà không đổi AC.

## Các quyết định kỹ thuật

- **Đã xác nhận:** diện tích đất, trường bắt buộc và Không áp dụng, một số tiền VND, danh mục dùng chung, một nguồn cho một công trình, khóa dữ liệu nguồn, chỉ dự toán hoàn tất/chưa xóa, các quyền và giới hạn 9 tệp/10 MB; chỉ lưu dữ liệu cho chức năng liên quan sau này. Chi tiết ở [bộ US/BR/ST đã chốt](construction-site-system-test-coverage.md).
- **Thiết kế đã chốt:** schema/API/transaction trong bộ TDD mới. FK ghép bảo vệ owner/revision; khóa cùng hàng Estimate bảo vệ tranh chấp xóa mềm; slot 1–9 bảo vệ hạn mức tệp; private final và proxy stream bảo vệ quyền tải mới. Danh mục hiện trạng dùng ID ổn định và snapshot tên trên site.
- **Cần làm rõ nghiệp vụ:** không phát sinh quyết định nghiệp vụ mới cần hỏi trong phạm vi này. Retention/xóa vật lý final SITE chưa thuộc phạm vi; giữ file private và không đưa vào cleanup ảnh hiện hành.

Đã thấy thay đổi tài liệu tọa độ dự toán đang diễn ra trong workspace, gồm snapshot v2 ở TDD-PROJ-002. SITE source reader được thiết kế hỗ trợ cả v1 và v2; không ghi đè phần thay đổi riêng đó. Danh sách trường lấy nguồn vẫn theo BR-SITE-004. Tọa độ SITE khi tạo tiếp tục theo luồng đã chốt của SITE-002; thay đổi chính sách lấy tọa độ tự động từ PROJ nếu muốn phải là thay đổi nghiệp vụ riêng.

## Hiện trạng mã nguồn đã đối chiếu

Các đường dẫn tính từ workspace BMT; tên thành phần mới trong TDD vẫn là đề xuất.

| Mã nguồn hiện có | Kết luận dùng cho thiết kế |
|---|---|
| `bmt-be/src/bmt-be.domain/entities/ConstructionSite.cs`, `bmt-be.persistence/configurations/ConstructionSiteConfiguration.cs` | Có owner/name/address/tọa độ/version, tên unique; chưa có profile mở rộng. |
| `bmt-be/src/bmt-be.application/usecases/commands/constructionSite/`, `usecases/queries/constructionSite/` | Có quyền Customer/staff, khóa site trước sửa/xóa và kiểm gói; giữ nền này cho hồ sơ và tệp. |
| `bmt-be/src/bmt-be.application/services/EstimateGenerationInputFactory.cs`, `bmt-be.domain/entities/EstimateGeneration.cs` | Có snapshot đầu vào v1 bất biến; phù hợp lấy dữ liệu hoàn tất, không lấy dữ liệu AI suy ra. |
| `bmt-be/src/bmt-be.persistence/configurations/EstimateConfigurations.cs`, `EstimateGenerationConfigurations.cs` | Có FK owner/revision; cần thêm key snapshot và FK SITE để bảo vệ quan hệ nguồn. |
| `bmt-be/src/bmt-be.application/usecases/commands/estimate/DeleteMyEstimatesCommandHandler.cs`, `bmt-be.persistence/repositories/EstimateStore.MyEstimates.cs` | Đã khóa account/Estimate và có receipt xóa; bổ sung guard SITE và status, không thiết kế lại toàn luồng. |
| `bmt-be/src/bmt-be.persistence/configurations/EstimateDeletionReceiptConfiguration.cs` | CHECK đang chỉ nhận ResponseVersion=1; phải nới 1/2 trước writer mới. |
| `bmt-be/src/bmt-be.persistence/configurations/PackageLifecycleEventConfiguration.cs` | ConstructionSiteAddress đang giới hạn 500; phải nới text khi Address gộp chứa ba phần. |
| Các service MEDIA/Library upload, MediaConfigurations và BizflyMediaObjectStore trong `bmt-be/src` | Có ticket/lease/binding pattern; PutAsync hiện public, cần nhánh private và ngăn generic API bypass. UNIQUE object theo SourceUploadId/Id đã có, dùng lại cho FK attachment. |

## Thứ tự triển khai và kiểm chứng dự kiến

1. Schema điều kiện hiện trạng, profile SITE, FK nguồn/snapshot, receipt v2 và cột địa chỉ lịch sử; kiểm bảng SITE trống trước migration, dừng nếu có dữ liệu ngoài giả định. Không xóa dữ liệu để làm cho giả định đúng.
2. Policy/profile/API và source reader; kiểm FK, khóa nguồn, catalog cũ và các tranh chấp bằng PostgreSQL thật.
3. Danh mục Admin và giữ snapshot hiện trạng, gồm chọn đồng thời với ngừng mục.
4. MEDIA private purpose/binding, kiểm tệp, attachment, source registry và streaming theo quyền; kiểm bucket trước phát hành.
5. Đồng bộ form FE: địa chỉ/tọa độ, định dạng VND, nhóm Không áp dụng, chọn/bỏ nguồn trước tạo, khóa trường, lỗi giữ form và upload trước commit tạo.
6. Chạy bộ ST đã chốt và kiểm hồi quy PROJ/MEDIA/SUB; chỉ ghi Pass khi có bằng chứng thực thi.

Unit/policy phù hợp kiểm quy tắc và ánh xạ; integration PostgreSQL cần cho FK, khóa, rollback và concurrency; system/browser cần cho form, quyền sau chuyển giao và bytes tải thật. Sau phản hồi chốt, đặc tả Unit Test được cập nhật theo [bảng đối chiếu Unit Test](construction-site-unit-test-coverage.md).

## Đối chiếu quy tắc và System Test

| Quy tắc / phạm vi | Nơi thiết kế | Bộ ST làm căn cứ |
|---|---|---|
| BR-SITE-001: hồ sơ bắt buộc, tên, diện tích, tiền, địa chỉ | SITE-003 Data Model/Internal API; SITE-002 tọa độ | ST-SITE-001–024, 026–056 theo bảng ST gốc |
| BR-SITE-002/003: owner, staff, gói khóa, scope hiện tại | SITE-003 Architecture; SITE-005 đọc/ghi tệp | ST-SITE-001–037 (trừ 025 đã bỏ), 070–075, 078–080 |
| BR-SITE-004 và BR-PROJ-009: nguồn, bất biến, giải phóng và xóa | SITE-003; PROJ-005 Notes/SQL/receipt | ST-SITE-045–051, 055, 071, 076, 080, 082; ST-PROJ-093–114 |
| BR-SITE-005, BR-PROJ-004: revision, applicability, ID dùng chung | SITE-003 profile policy/FK | ST-SITE-043–044, 052–053, 083 |
| BR-SITE-006: hiện trạng, Admin, giữ hồ sơ cũ | SITE-004 toàn bộ | ST-SITE-057–063 |
| BR-SITE-007, BR-MEDIA-001/002: bytes, tệp riêng tư, vòng đời | SITE-005; MEDIA-001 ngoại lệ SITE | ST-SITE-064–082 theo trace của từng ca; ST-MEDIA-011 |

Các nhóm ST trên có phần giao nhau vì một ca kiểm nhiều quy tắc. Danh sách chi tiết và 93 AC đang áp dụng nằm ở báo cáo ST; không cộng các khoảng để suy ra số ca hoặc kết quả chạy.

## Giới hạn kiểm chứng và nguồn

Tác vụ này kiểm tài liệu và khảo sát code, không chứng minh implementation đáp ứng thiết kế. Chưa gọi API thật, chưa chạy DB/storage/renderer Mermaid hay importer Document First. Phạm vi đọc sâu tập trung SITE, các BR/ST đã chốt và phần PROJ/MEDIA/SUB liên quan. Các liên kết bắc cầu AUTH/RBAC/SUB/PAY/CTR/LIB tiếp tục được dẫn như phụ thuộc hiện có; chưa báo đây là cuộc kiểm toán toàn bộ đồ thị tài liệu của workspace. Liên kết file được kiểm tự động không có nghĩa đã đọc lại toàn bộ mọi tài liệu được dẫn gián tiếp.

Các điều kiện cần kiểm khi triển khai: parser ACadSharp/PdfPig phải qua fixture và ghim phiên bản, kho thật phải chặn mọi đường đọc ẩn danh prefix SITE, các writer cũ phải dừng trước mở contract mới, migration phải kiểm dữ liệu thực tế. Các điều kiện này không được ghi thành kết quả đã đạt.

## Kết quả kiểm tra tài liệu

- Ba TDD mới đúng thứ tự đề mục của template; metadata, mã lỗi và ba ví dụ API khớp endpoint đã khai báo. Mục External API của danh mục hiện trạng được bỏ vì không gọi dịch vụ ngoài, theo phần tùy chọn của mẫu.
- Đã kiểm 232 liên kết file/anchor trực tiếp trong 10 TDD thuộc phạm vi và 30 tham chiếu mã/section trong ba TDD mới; không phát hiện đích thiếu. Kiểm này không thay việc đọc toàn bộ đồ thị tham chiếu gián tiếp.
- Các liên kết Story–TDD đã kiểm hai chiều. Có 15 khối Mermaid trong ba TDD mới; đã kiểm hàng rào Markdown, chưa render sơ đồ.
- 105 file đặc tả System Test đã chốt giữ nguyên hash so với đầu tác vụ TDD. `git diff --check` không báo lỗi khoảng trắng. Không ghi kết quả chạy kiểm thử chức năng từ các kiểm tra tài liệu này.

## Ghi nhận chốt TDD

Phản hồi “chốt” của người dùng xác nhận bản SITE-003/004/005 đã bàn giao và phần tích hợp SITE trong SITE-001/002, PROJ-001/005, MEDIA-001, SUB-005/007. Đây là căn cứ tiếp tục đặc tả Unit Test; không tự đổi Status thành Approved, không tạo approvalId hoặc coi là đã import. Thay đổi tọa độ dự toán riêng đang có trong workspace không được mặc nhiên phê duyệt bởi ghi nhận này.

## Triển khai sau khi chốt thiết kế

Phần backend đã được triển khai trên nhánh `feature/construction-site-profile` trong worktree riêng. Xem [báo cáo triển khai và kiểm thử](construction-site-implementation.md) để phân biệt kết quả thực thi với các kiểm tra tài liệu ở trên.
