<!-- HƯỚNG DẪN CHO AI KHI ĐIỀN HOẶC CHỈNH SỬA MẪU
- Đọc toàn bộ mẫu, các comment hướng dẫn và tài liệu nguồn được cung cấp trước khi viết. Tuân thủ các quy tắc nghiệp vụ, điều kiện, ngoại lệ và giới hạn đã được xác nhận.
- Không bịa, tự chế hoặc suy đoán thành sự thật: yêu cầu, quy tắc, số liệu, API, schema, mã lỗi, nguồn tham khảo, người phụ trách và kết quả kiểm thử phải có căn cứ. Nội dung ví dụ trong mẫu không phải dữ kiện của dự án.
- Khi thiếu thông tin hoặc các nguồn mâu thuẫn, hỏi người dùng để làm rõ; giữ nguyên placeholder ở phần chưa xác định. Không tự chọn đáp án, lấp chỗ trống hay tuyên bố tài liệu đã hoàn tất.
- Chỉ thay nội dung cần điền hoặc được yêu cầu sửa. Không tự mở rộng phạm vi, sửa nghĩa quy tắc, bỏ điều kiện/ngoại lệ, xoá hay ghi đè nội dung hợp lệ đã có.
- Giữ nguyên tên, cấp và thứ tự heading, nhãn in đậm, tiền tố bullet, cấu trúc danh sách, tên/thứ tự/số cột bảng và cú pháp code fence. Không dịch, đổi tên, gộp hoặc thêm mục ngoài cấu trúc mẫu.
- Chỉ lặp khối luồng, AC hoặc API theo đúng khuôn khi có căn cứ; chỉ bỏ phần tuỳ chọn khi hướng dẫn riêng của mẫu cho phép và đã xác định không áp dụng.
- Mã tham chiếu và section phải trỏ tới tài liệu có thật đã được cung cấp hoặc kiểm chứng. Không giữ mã ví dụ như tham chiếu thật và không tự tạo tài liệu liên quan để hợp thức hoá liên kết.
- Giữ các comment hướng dẫn trong file. Xuất Markdown UTF-8, mỗi file một tài liệu; không bọc toàn bộ file trong code fence, không chèn lời dẫn hay giải thích ngoài mẫu.
- Trước khi bàn giao, đối chiếu nội dung với nguồn, kiểm tra cấu trúc, giá trị cho phép, giới hạn độ dài và tính nhất quán của mã/tham chiếu. Báo rõ phần chưa đủ thông tin; không tuyên bố đã chạy test hoặc import khi chưa thực hiện.
-->

<!-- Thay mã TDD-001 và nội dung ví dụ; giữ nguyên heading và nhãn in đậm.
Sơ đồ dùng mermaid, plantuml hoặc URL. Giữ heading Architecture, Sequence Diagram, Activity Diagram, State Diagram, Data Model.
Ví dụ API phải có heading trùng METHOD /path trong Endpoints. Xoá phần API/sơ đồ không dùng.
Version, Updated At và Change Log do lịch sử phiên bản quản lý, để trống khi nhập mới.
Author/Reviewer là tên hiển thị; gán tài khoản, phê duyệt, giả định và câu hỏi mở trên giao diện sau import.
Thiết kế phải truy vết về Story và Business Rule được cung cấp. Phân biệt thiết kế đề xuất với implementation đã kiểm chứng; không mô tả API/schema/sơ đồ ví dụ như hệ thống thật hoặc tự quyết định nghiệp vụ còn thiếu.
BẮT BUỘC KHI HOÀN THIỆN MẪU: phải có cả Reviewer và Approver, mỗi tên 1–200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Không xoá hai dòng metadata, để trống, dùng tên bịa hoặc giữ placeholder rồi coi là hoàn tất.
Nếu chưa biết người review hoặc người phê duyệt, phải hỏi người dùng và báo tài liệu chưa đủ thông tin; không tự lấy Author/Owner làm người thay thế. Tên trong file không tự gán tài khoản hoặc xác nhận đã duyệt; gán thành viên trên giao diện sau import.
Đây là yêu cầu hoàn thiện mẫu; backend hiện vẫn nhận file cũ thiếu hai trường để tương thích.

VALIDATION CHO FILE NHẬP (đối chiếu ImportSnapshotValidator, MarkdownParser và ImportService):
- Mỗi file .md UTF-8 không rỗng chỉ có một heading cấp 1 chứa mã tài liệu dài 1–100 ký tự. Mã không được trùng trong cùng lần nhập hoặc thuộc loại tài liệu khác đã tồn tại.
- Không dùng tên README.md hoặc sitemap.md vì importer bỏ qua. Giao diện nhận .md/.zip, tối đa 2.000 file, tổng file tải lên 31 MiB; API giới hạn request 32 MiB và tổng nội dung đọc/giải nén 64 MiB.
- Chỉ nhập đè tài liệu cùng loại đang Draft, chưa có phiên bản và chưa lưu trữ. Import thay toàn bộ nội dung bản nháp, vì vậy phải giữ lại nội dung hợp lệ ngoài phần được yêu cầu sửa.
- Không dùng Status trong Markdown hoặc tên Approver để tự xác nhận phê duyệt; import không cấp quyền hay gán tài khoản từ tên. Chạy Kiểm tra file và xử lý lỗi/cảnh báo trước khi nhập.
- Giới hạn độ dài bên dưới tính theo string.Length của .NET (đơn vị UTF-16); không tự cắt ngắn dữ kiện quan trọng để vượt validation, hãy viết lại có căn cứ hoặc hỏi người dùng.
- Feature tối đa 300 ký tự và được dùng làm tiêu đề tài liệu. Status được parser nhận: Draft / In Review / Approved / Deprecated; tài liệu nhập mới vẫn có trạng thái phê duyệt Draft.
- Endpoint: đường dẫn tối đa 500 ký tự; tên/mô tả endpoint được lưu trong Name tối đa 300. HTTP method: GET / POST / PUT / PATCH / DELETE / HEAD / OPTIONS.
- Error Codes: mã tối đa 100 ký tự, không trùng trong tài liệu; giữ dạng - **CODE** (400): mô tả. HTTP status trong Error Codes và Response phải là số nguyên đọc được bằng Int32; không tự đặt status khác hợp đồng API.
- URL sơ đồ tối đa 1.000 ký tự. Mục sơ đồ được sử dụng phải có khối mermaid/plantuml hoặc URL theo mẫu; không để một mục sơ đồ rỗng. Nhãn mục được tách từ bullet như tên External API Fields tối đa 300 ký tự.
- Heading Examples phải khớp METHOD /path ở Endpoints. Giữ các nhãn Request:, Response 200: (thay mã theo contract) và Error Response: để parser tách đúng dữ liệu.
- Tham chiếu dạng DOC-KEY/section: ghi chú: mã đích tối đa 100 ký tự, section tối đa 100, ghi chú tối đa 1.000. Không trùng bộ mã đích + section + loại liên kết trong cùng tài liệu.
ĐỐI CHIẾU FORM TDD (src/features/tdds/validations.ts; form có thể chặt hơn import):
- Điền Feature, Author, Reviewer, Problem và ít nhất một Goal; các Goal/Non-goal/Notes đã thêm không được rỗng. API đã khai báo phải có endpoint và mô tả/mục đích; Fields phải có tên và ý nghĩa; Error Codes phải có mã, HTTP status và điều kiện.
- Form còn kiểm tra Version, Updated At và nội dung Change Log khi khai báo; với file nhập mới, giữ các trường lịch sử này trống theo hợp đồng import, không tự tạo dữ liệu lịch sử.
-->

# TDD-LIB-001

## Document Info

- **Feature**: Quản lý thư viện mẫu — nội dung, bản nháp, phiên bản, danh mục và tìm kiếm
- **Author**: Codex
- **Reviewer**: Tân Trần
- **Approver**: Tân Trần
- **Status**: Draft
- **Version**:
- **Updated At**:

## Context & Goals

### Problem

Người dùng đã chốt STORY-LIB-001–003 và BR-LIB-001–003; có 27 đặc tả ST-LIB-001–027. Việc tách tài liệu không thay nghiệp vụ, schema hoặc API đã đề xuất. Đây là thiết kế để chốt, chưa phải implementation hoặc kết quả kiểm thử.

Tài liệu này sở hữu năm bảng nội dung/quản trị và các API quản lý, danh sách công khai. Quyền xem, tính lượt, lịch sử và tải nội dung bảo vệ nằm ở [TDD-LIB-002](TDD-LIB-002.md).

### Goals

- Lưu nháp thiếu dữ liệu, kiểm đủ trước công bố.
- Sửa tại chỗ giữ VersionId; công bố mới giữ nguyên phiên bản cũ.
- Dùng chung catalog PROJ và phục vụ tìm/lọc công khai.

### Non-goals

- Không thiết kế lại quota hoặc LibraryAccess; tham chiếu TDD-LIB-002.
- Không yêu thích, xoay 3D, tìm kích thước, phân công mẫu, sửa bản đã thay thế hoặc xóa bản đã công bố.
- Chưa triển khai code/migration, chưa viết Unit Test.

## Architecture

**Hiện trạng đã xác minh**

Backend là .NET 8, EF Core/Npgsql 8; compose dùng PostgreSQL 15. `ApplicationDbContext` có User và các bảng nền RBAC, chưa có bảng LIB, catalog PROJ hay subscription trong DbContext được đọc. Không có tenant filter. Các port/tên lớp mới trong tài liệu là đề xuất, không khẳng định đã tồn tại.

`TransactionPipelineBehavior` commit khi handler trả về bình thường, kể cả Result.Failure. Generic `ICommand<T>` không kế thừa marker `ICommand`; command ghi mới phải đặt hậu tố Command hoặc hiện thực `ITransactionalRequest`. `IUnitOfWork` hiện đăng ký transient: phải đổi scoped để LIB và dịch vụ quota dùng cùng DbContext/transaction. Sau khi bắt đầu ghi, lỗi phải ném exception để rollback, không trả Result.Failure rồi để pipeline commit. Không mở transaction lồng. Quy ước tương ứng đã có trong TDD-PROJ-001.

**Phân chia trách nhiệm**

| Thành phần dự kiến | Trách nhiệm |
|---|---|
| LibraryApi, AdminLibraryApi | Carter routes, policy, CSRF, DTO; không tự tính quota. |
| LibraryContentPolicy | Kiểm tên, kích thước, định dạng, ảnh đại diện, phân loại và điều kiện công bố. |
| LibraryVersionService | Sửa tại chỗ, nháp riêng, công bố, ẩn/hiện, xóa nháp; kiểm version chống ghi đè. |
| ILibraryCatalogReader | Đọc cùng EstimateCatalog/CatalogBuildingType/CatalogFloor của PROJ; trả revision và lựa chọn hợp lệ. Không có bản sao danh mục LIB. |
| ILibraryObjectStore | Ghi object bất biến, xác nhận có thể đọc, mở stream; adapter nhà cung cấp còn chờ cấu hình. |

```mermaid
flowchart LR
    UI[Người quản lý và danh sách công khai] --> API[Library API]
    API --> LIB[ContentPolicy và VersionService]
    LIB --> CAT[Catalog PROJ]
    LIB --> DB[(PostgreSQL)]
    LIB --> STORE[Kho object private]
    VIEW[Tra cứu TDD-LIB-002] --> DB
```

**Quyền và biên dữ liệu**

Đề xuất mã kỹ thuật `library.manage`, RequiresAssignment=false, seed cho vai trò hệ thống Admin; các vai trò khác được cấp bằng RBAC hiện có. Bổ sung cả PermissionNames, migration seed và policy registry để PermissionCatalogGuard không từ chối khởi động. Quyền này khác quyền lợi gói `catalog.detail`: một bên là quản trị, một bên là quyền khách mở mẫu mới. Không tái dùng `assignment.manage` hoặc bắt phân công nhân viên vào mẫu.

Mutation dùng cookie phải kiểm antiforgery token và Origin theo nền tảng PROJ/RBAC, không dùng CORS thay CSRF. Endpoint công khai chỉ trả summary và ảnh đại diện của phiên bản hiện hành không ẩn. Endpoint thumbnail kiểm lại current/visibility, không để URL asset thô tồn tại công khai sau ẩn/thay ảnh. Ảnh đã tải xuống trước đó không thể thu hồi khỏi máy khách.

Preview quản trị và các route đọc nội dung bảo vệ theo TDD-LIB-002/Internal API; quyền library.manage không tạo lịch sử khách hoặc dùng lượt.

**Mẫu, phiên bản và lần sửa**

`LibraryTemplate` là định danh mẫu, giữ con trỏ CurrentVersionId và IsHidden. `LibraryVersion` là phiên bản tính lượt. `EditVersion` là số kiểm soát sửa đồng thời, không phải phiên bản tính lượt. Mỗi lần sửa metadata, ảnh, thứ tự, cover hoặc file tăng EditVersion của cùng VersionId. Khi V1 được sửa, quyền xem V1 giữ nguyên và nội dung đọc mới phản ánh sửa đó.

Phiên bản có State Draft hoặc Published. Phiên bản cũ là Published nhưng không còn được CurrentVersionId trỏ tới; không lưu thêm trạng thái Superseded để tránh lệch hai nguồn. Number được cấp lúc công bố bằng max Number đã công bố + 1 dưới khóa Template. Nháp có Number/PublishedAtUtc NULL. Có thể có nhiều nháp kỹ thuật; công bố phải gửi expectedCurrentVersionId nên nháp dựa trên bản cũ không tự ghi đè bản mới. Không tự thêm luồng gộp nháp. BaseVersionId là dấu nguồn sao chép, không bị so bằng current mỗi lần công bố: Admin có thể xem xét một nháp cũ rồi gửi expectedCurrentVersionId hiện hành, nhưng server không tự đổi giá trị kỳ vọng thay khách.

Tạo nháp từ current sao chép metadata và các dòng liên kết tài nguyên; object đã xác minh được dùng chung bằng AssetId, không nhân đôi bytes. Sửa asset luôn tạo object mới, không ghi đè object đang được bản khác tham chiếu. Khi công bố, dữ liệu nháp phải đủ, phân loại phải hợp lệ theo catalog hiện hành. Transaction đổi con trỏ và chuyển Draft sang Published. Bản trước không còn sửa được, kể cả API trực tiếp.

Ẩn/hiện chỉ đổi Template.IsHidden, không thay phiên bản, lượt hoặc quyền xem. Mẫu chưa công bố không xuất hiện dù IsHidden=false. Xóa nháp xóa các liên kết của riêng nháp; không xóa object hoặc template identity dùng bởi lịch sử/receipt. Công bố không tự đảo IsHidden; mẫu đang ẩn tiếp tục ẩn tới khi người quản lý chọn Hiện lại.

**Dùng chung catalog mà vẫn giữ phân loại cũ**

Version lưu CatalogRevisionId, BuildingTypeId, FloorCount, HasTum. Tên và cờ lấy từ revision đã ghim, không sao chép tên vào bảng LIB. Nullable HasTum phân biệt false=Không tum và NULL=Không áp dụng/nháp chưa nhập. Cờ từ catalog phân biệt hai nghĩa NULL này.

Sửa tên/ảnh/file không đổi revision phân loại. Nếu đổi bất kỳ trường phân loại nào, revalidate toàn bộ bộ loại/tầng/tum theo current catalog và ghim revision mới trong cùng transaction. Công bố nháp luôn revalidate và ghim current revision, kể cả khi được sao từ bản cũ. Phần tầng dùng đúng FloorCount của PROJ: 1 là trệt, 3 là tổng ba tầng; nhãn phải thống nhất với catalog, không tự cộng thêm một tầng hoặc tính tum thành tầng.

Bộ lọc dùng UNION DISTINCT của lựa chọn catalog hiện hành và phân loại trên các phiên bản current công khai. Lọc tầng giới hạn theo BuildingTypeId khi được chọn. Tầng cũ chỉ được thêm vào filter vì đang có mẫu public, không trở lại thành lựa chọn hợp lệ cho công bố mới. Mẫu ẩn, nháp và phiên bản lịch sử không làm xuất hiện giá trị filter. Tên loại trên từng mẫu dùng revision của mẫu; nhãn bộ lọc ưu tiên tên current của cùng định danh.

**Khóa và ranh giới với tra cứu**

Mutation quản trị khóa User người thao tác FOR UPDATE → EstimateCatalog FOR SHARE khi revalidate → Template FOR UPDATE → Version theo Id tăng dần. Ghi content, con trỏ và receipt cùng transaction; upload ngoài transaction. Không lấy khóa tài khoản khách/quota sau Template. Quy tắc khóa phối hợp đầy đủ và xử lý mở khi nội dung đổi được định nghĩa một lần tại [TDD-LIB-002/Architecture](TDD-LIB-002.md#architecture).

**Upload và tài nguyên**

Upload từng tệp theo stream ngoài SQL transaction qua port PutImmutable; server cấp object key ngẫu nhiên, không nhận URL/key tùy ý của người gọi. Kiểm nội dung thực tương ứng JPG/PNG/WebP/PDF/DWG/DXF, không tin extension hoặc Content-Type. Với ảnh, tạo thumbnail từ ảnh đại diện bằng adapter xử lý ảnh; PDF/CAD là download attachment, không thực thi hoặc render CAD trên server. Cần chọn/thử bộ kiểm định dạng và thư viện ảnh trước triển khai production.

Chỉ tạo LibraryAsset sau khi object hoàn tất, checksum/bytes đã xác minh và đọc lại được. Upload không tự gắn vào phiên bản; attach là transaction riêng kiểm quyền, TemplateId và expectedEditVersion. Upload thất bại hoặc attach thất bại giữ nội dung cũ. Object không gắn có thể tồn tại sau lỗi; không xóa để bù một object còn được version khác dùng. Dọn object mồ côi phải có kiểm tra tham chiếu và phối hợp khóa khi triển khai; chưa bật tự dọn trong phạm vi này.

Không lưu binary trong PostgreSQL hoặc buffer cả file bằng IFormFile/MemoryStream. Không đặt số ảnh/tệp tối đa trong validator. Upload stream vẫn chịu giới hạn request, timeout và kho tệp thực tế; các giá trị vận hành phải được cấu hình và nghiệm thu, không mô tả “không giới hạn” là năng lực vật lý vô hạn. [ASP.NET Core 8 — streaming upload](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/file-uploads?view=aspnetcore-8.0).

Object/metadata bất biến là hợp đồng đầu vào cho luồng tải tài nguyên có quyền tại TDD-LIB-002. Thumbnail công khai chỉ phục vụ cover current không ẩn.

**Phạm vi thay đổi mã khi được giao triển khai**

- Thêm DTO/validator tại `contract/services/library/`, command/query handler tại `application/usecases/.../library/`, Carter routes tại `presentation/apis/library/` và entity/config tại domain/persistence.
- Thêm các port đã nêu vào application/abstractions; catalog adapter đọc schema PROJ, quota adapter ghi schema SUB, storage adapter ở infrastructure. Không cho LIB cập nhật catalog hoặc cấu hình gói trực tiếp.
- Bổ sung permission/policy registry/seed, UoW scoped và cấu hình stream/CSRF tại API. Hồi quy auth và gói/lượt sau thay đổi nền tảng.
- Thứ tự triển khai: nền RBAC/UoW → catalog và SUB cốt lõi → metadata/nháp/assets → công bố/danh sách → quyền xem/quota/history → tích hợp tải tệp và kiểm thử lỗi. Chưa giao triển khai trong tác vụ thiết kế này.

Chi tiết triển khai nội dung/quản trị thuộc tài liệu này; bước quota/history và tải có quyền thuộc TDD-LIB-002.

**Nơi thực hiện quy tắc và kiểm chứng**

| Quy tắc | Nơi thực hiện | Đặc tả hệ thống |
|---|---|---|
| BR-LIB-001: nội dung, kích thước, file | Validator, LibraryContentPolicy, asset verifier và CHECK | ST-LIB-001–005 |
| BR-LIB-001: catalog và filter | ILibraryCatalogReader, FKs ghép, query projections | ST-LIB-010, ST-LIB-012–016 |
| BR-LIB-002: sửa/công bố/ẩn/xóa | VersionService, mutex Template, receipt, state guard | ST-LIB-006–009, ST-LIB-011 |

Unit test sau khi TDD được chốt sẽ kiểm validator và policy; không dùng mock để kết luận mutex/UNIQUE/rollback đúng. Integration dùng PostgreSQL 15 thật và hai connection cho lượt cuối, cùng phiên bản, đổi kỳ, sửa/công bố chen lúc mở. Storage contract test kiểm object bất biến, lỗi trước/sau metadata, tải Range và định dạng thật. Bổ sung thực nghiệm công bố lúc xác nhận và upload lớn theo cấu hình hạ tầng; không báo đạt từ việc viết đặc tả.

**Notes**:

- Không thêm broker, outbox hoặc database khác. Quota/Access/nội dung cùng PostgreSQL; object store nằm ngoài transaction và được chuẩn bị trước. [EF Core transactions](https://learn.microsoft.com/en-us/ef/core/saving/transactions).
- Log operationId/accountId/versionId/editVersion, kết quả Granted/Reused/Denied/Conflict, không log token, signed URL hay bytes. Theo dõi lỗi storage, thời gian chờ khóa, cấp quyền thất bại và lệch Access/UsageOperation; chưa đặt ngưỡng cảnh báo khi chưa có tải thực.
- RPO/RTO, retention object chưa gắn, storage provider, timeout/giới hạn truyền tải là phần vận hành chưa có số liệu. Backup phải bao gồm DB và object cùng tham chiếu; thử restore để quyền xem không trỏ file đã mất trước mở production.

## Sequence Diagram

```mermaid
sequenceDiagram
    actor A as Người quản lý
    participant API as AdminLibraryApi
    participant S as Kho private
    participant DB as PostgreSQL
    A->>API: Upload ảnh hoặc tệp
    API->>S: Ghi bất biến, xác minh và đọc lại
    API->>DB: Lưu asset metadata bằng giao dịch ngắn
    A->>API: Tạo và chỉnh sửa nháp riêng
    API->>DB: Lưu nháp, link tài nguyên và receipt
    Note over API,DB: Current vẫn phục vụ khách
    A->>API: Publish với expected versions và key
    API->>DB: Khóa actor, catalog, template và version
    alt Dữ liệu thiếu hoặc version xung đột
        API->>DB: Rollback
        API-->>A: Lỗi, current không đổi
    else Hợp lệ
        API->>DB: Publish nháp, đổi current và ghi receipt
        API->>DB: Commit
        API-->>A: Phiên bản mới; bản trước khóa sửa
    end
```

## Activity Diagram

```mermaid
flowchart TD
    A[Mutation quản trị] --> B{Có library.manage?}
    B -->|Không| X[Từ chối]
    B -->|Có| C{Key đã xử lý?}
    C -->|Cùng hash| R[Trả kết quả đã lưu]
    C -->|Khác hash| X
    C -->|Chưa| D{Version kỳ vọng đúng?}
    D -->|Không| X
    D -->|Có| E{Thao tác}
    E -->|Lưu nháp| F[Kiểm giá trị đã nhập; cho thiếu trường]
    E -->|Sửa current| G[Giữ đủ nội dung Published]
    E -->|Công bố| H[Kiểm đủ dữ liệu và catalog hiện hành]
    E -->|Ẩn hoặc hiện| I[Đổi IsHidden]
    E -->|Xóa| J{Là Draft?}
    J -->|Không| X
    J -->|Có| K[Xóa nháp và link riêng]
    F --> L[Commit cùng receipt]
    G --> L
    H --> L
    I --> L
    K --> L
```

## State Diagram

```mermaid
stateDiagram-v2
    [*] --> Draft: Tạo mẫu hoặc nháp riêng
    Draft --> Current: Công bố hợp lệ và đổi con trỏ
    Draft --> [*]: Xóa nháp
    Current --> Current: Sửa tại chỗ tăng EditVersion
    Current --> Historical: Công bố phiên bản kế tiếp
    Historical --> Historical: Chỉ xem lại bởi người đã mở
```

Current/Historical là trạng thái suy ra từ State=Published và con trỏ Template, không phải hai giá trị State được lưu. IsHidden thuộc Template, không phải trạng thái Version. Quyền Access không hết hạn theo kỳ và không bị gỡ bởi ẩn mẫu.

## Data Model

**Quy ước và bảng dùng lại**

UUID cho định danh; timestamptz/UTC cho thời điểm; PascalCase; NN là NOT NULL. Không dùng soft-delete filter làm mất lịch sử. FK mặc định ON DELETE RESTRICT; chỉ xóa link của nháp trong transaction xóa nháp. Dữ liệu khách ngăn truy cập chéo bằng AccountId, không tự thêm tenant.

Dùng lại User/RBAC theo TDD-RBAC-001; EstimateCatalog, EstimateCatalogRevision, CatalogBuildingType và CatalogFloor theo [TDD-PROJ-001/Data Model](TDD-PROJ-001.md#data-model). Dùng DesignSubscription, DesignPeriod, PeriodQuota, UsageOperation theo [TDD-SUB-002/Data Model](TDD-SUB-002.md#data-model), gồm LifecycleState của TDD-SUB-005. Không sao chép số dư vào LIB.

LibraryAccess và phần mở rộng UsageOperation có nguồn duy nhất tại [TDD-LIB-002/Data Model](TDD-LIB-002.md#data-model); không định nghĩa lại ở đây.

**Ý nghĩa từng bảng**

| Bảng | Một dòng đại diện cho gì; ai ghi; quan hệ |
|---|---|
| LibraryTemplate | Một mẫu ổn định. Admin tạo; CurrentVersionId NULL trước công bố, IsHidden dùng chung cho mẫu. Không lưu tên/kích thước tại đây. |
| LibraryVersion | Một phiên bản nội dung tính lượt, thuộc một Template. Admin sửa khi Draft/current; Published cũ không sửa. Number NULL trước công bố. EditVersion khác Number. |
| LibraryAsset | Một object đã xác minh, thuộc một Template; uploader tạo sau upload thành công. Metadata bất biến, dùng lại được giữa các phiên bản cùng mẫu. |
| LibraryVersionAsset | Một liên kết Version–Asset với vị trí hiển thị. Cho phép cùng asset ở nhiều phiên bản; không lặp metadata file. |
| LibraryMutationReceipt | Một mutation quản trị đã commit, dùng nhận diện key gửi lại; lưu hash và kết quả ID/version, không chứa bytes tài nguyên. |

**Cột và ràng buộc**

| Bảng | Cột, khóa và CHECK |
|---|---|
| LibraryTemplate | Id uuid PK; CurrentVersionId uuid NULL; IsHidden boolean NN DEFAULT false; Version bigint NN DEFAULT 1 CHECK>0; CreatedBy uuid NN FK User; CreatedAtUtc timestamptz NN. FK(Id,CurrentVersionId) → LibraryVersion(TemplateId,Id), thêm sau khi tạo bảng; current phải Published do transaction policy. |
| LibraryVersion | Id uuid PK; TemplateId uuid NN FK Template; State varchar(16) NN CHECK Draft/Published; Number bigint NULL; EditVersion bigint NN DEFAULT 1 CHECK>0; BaseVersionId uuid NULL; Name text NULL; Description text NULL; DrawingKind varchar(2) NULL CHECK NULL/2D/3D; WidthM,LengthM,AreaM2 numeric(28,2) NULL CHECK NULL hoặc >0; CatalogRevisionId,BuildingTypeId uuid NULL; FloorCount int NULL CHECK NULL hoặc >=1; HasTum boolean NULL; CoverAssetId uuid NULL; CreatedBy uuid NN FK User; CreatedAtUtc,ModifiedAtUtc timestamptz NN; PublishedAtUtc timestamptz NULL. UNIQUE(TemplateId,Id), UNIQUE(TemplateId,Number). CHECK Draft thì Number/PublishedAtUtc NULL, Published thì Number>0 và PublishedAtUtc NN. |
| LibraryAsset | Id uuid PK; TemplateId uuid NN FK Template; Kind varchar(16) NN CHECK Image/Attachment; StorageKey text NN UNIQUE; ThumbnailKey text NULL UNIQUE; OriginalName text NN; MediaType varchar(100) NN; SizeBytes bigint NN CHECK>0; Sha256 char(64) NN; CreatedBy uuid NN FK User; CreatedAtUtc timestamptz NN; UNIQUE(TemplateId,Id). Image phải có ThumbnailKey; Attachment không có. Không có trạng thái Ready giả: chỉ insert sau xác minh object. |
| LibraryVersionAsset | TemplateId uuid NN; VersionId uuid NN; AssetId uuid NN; Position bigint NN CHECK>0; PK(VersionId,AssetId); UNIQUE(VersionId,Position); FK(TemplateId,VersionId) → Version(TemplateId,Id); FK(TemplateId,AssetId) → Asset(TemplateId,Id). |
| LibraryMutationReceipt | Id uuid PK; ActorId uuid NN FK User; Operation varchar(24) NN CHECK Create/Save/Publish/Visibility/DeleteDraft/Attach/Detach/Reorder; RequestKey varchar(100) NN; RequestHash char(64) NN; TemplateId uuid NN FK Template; ResultVersionId uuid NULL; Result jsonb NN; CreatedAtUtc timestamptz NN; UNIQUE(ActorId,Operation,RequestKey). ResultVersionId không FK vì receipt xóa nháp phải còn trả lại được ID đã xóa; không dùng nó để cấp quyền đọc nội dung. |

Ràng buộc bổ sung:

- FK Version(TemplateId,BaseVersionId) → Version(TemplateId,Id), NULL với phiên bản đầu. Base phải là bản đã công bố của cùng mẫu tại thời điểm tạo nháp; kiểm trong handler.
- CatalogRevisionId/BuildingTypeId cùng NULL hoặc cùng có giá trị. FK ghép tới CatalogBuildingType; FK ba cột (CatalogRevisionId,BuildingTypeId,FloorCount) tới CatalogFloor. Chưa có loại thì FloorCount và HasTum NULL. Policy kiểm cờ bật/tắt. Công bố kiểm đủ trường, trim Name có nội dung, cover là Image của phiên bản và có ít nhất một Image.
- FK Version(Id,CoverAssetId) → VersionAsset(VersionId,AssetId), CoverAssetId NULL khi nháp chưa có cover. Tạo version trước với cover NULL, tạo link rồi gán cover trước công bố. Thay cover tạo link mới trước, đổi pointer rồi bỏ link cũ; không cần xóa toàn bộ link bằng SaveChanges một lần tùy ý.
- CHECK Published dùng các điều kiện IS NOT NULL rõ ràng cho các trường bắt buộc, không dựa so sánh với NULL để chặn dữ liệu. CHECK Published yêu cầu Name có nội dung, DrawingKind, cả ba kích thước, catalog/type và cover NN. Chuỗi kiểm whitespace ở domain; không tự đặt max tên/mô tả nghiệp vụ. Giá trị có mặt trong nháp vẫn phải đúng định dạng/miền giá trị. Người dùng đã xác nhận cho lưu nháp thiếu thông tin; chỉ yêu cầu đủ trường trước công bố. NULL biểu diễn thông tin chưa nhập trong Draft, không dùng giá trị 0 hoặc chuỗi rỗng thay cho thiếu dữ liệu.
- Reorder dùng vị trí tạm không trùng trong transaction rồi ghi vị trí đích trước commit; không dựa thứ tự UPDATE của EF để tránh UNIQUE. Có thể đổi hai bước SaveChanges trong cùng UoW, không commit giữa chừng.
- Numeric(28,2) khớp decimal của PROJ: JSON chuỗi, tối đa 26 chữ số phần nguyên và hai chữ số lẻ, không exponent/NaN/Infinity, từ chối thay vì làm tròn; giới hạn biểu diễn không phải trần kích thước nghiệp vụ. [PostgreSQL numeric](https://www.postgresql.org/docs/15/datatype-numeric.html).

**ERD và quan hệ**

```mermaid
erDiagram
    LibraryTemplate ||--o{ LibraryVersion : versions
    LibraryTemplate ||--o{ LibraryAsset : assets
    LibraryTemplate ||--o{ LibraryMutationReceipt : mutations
    LibraryVersion ||--o{ LibraryVersionAsset : contains
    LibraryAsset ||--o{ LibraryVersionAsset : reused
    CatalogBuildingType o|--o{ LibraryVersion : classifies
```

Nháp có thể chưa có phân loại; Published bắt buộc có. FK current, cover và base được mô tả ở phần ràng buộc. RESTRICT bảo vệ tài nguyên còn tham chiếu; policy chặn xóa Published ngay cả khi chưa có người xem. Access của TDD-LIB-002 tham chiếu Version và giữ nguyên khi ẩn mẫu.

**Dữ liệu mẫu xuyên suốt**

Dữ liệu giả định, ID bí danh thay uuid, lược cột không liên quan; không phải SQL seed hoặc dữ liệu thật. T1<T2<T3 theo UTC. Catalog C1 có loại B1=Nhà phố, tầng 3, tum bật; C2 chỉ cho tầng 2. U1 là khách, A1 là quản trị. Kỳ DP1/quota Q1 của U1 có Limit=20, Used=0, Reserved=0; schema và dữ liệu kỳ xem TDD-SUB-002/Data Model.

| Bảng | Dòng dữ liệu lưu thực tế (cột chọn lọc) |
|---|---|
| LibraryTemplate | Id=M1; CurrentVersionId=V1; IsHidden=false; Version=2; CreatedBy=A1; CreatedAtUtc=T1. |
| LibraryVersion | Id=V1; TemplateId=M1; State=Published; Number=1; EditVersion=1; BaseVersionId=NULL; Name=Nhà ABC; DrawingKind=2D; WidthM=5; LengthM=20; AreaM2=80; CatalogRevisionId=C1; BuildingTypeId=B1; FloorCount=3; HasTum=true; CoverAssetId=F1; PublishedAtUtc=T1. |
| LibraryAsset | F1/M1/Kind=Image/StorageKey=lib/M1/F1/ThumbnailKey=lib/M1/F1-thumb/OriginalName=mat-bang.jpg/MediaType=image/jpeg/SizeBytes=120000/Sha256=H1; F2/M1/Kind=Attachment/StorageKey=lib/M1/F2/ThumbnailKey=NULL/OriginalName=ban-ve.pdf/MediaType=application/pdf/SizeBytes=240000/Sha256=H2. H1/H2 là bí danh hash 64 ký tự. |
| LibraryVersionAsset | M1/V1/F1/Position=1; M1/V1/F2/Position=2. |
| LibraryMutationReceipt | ActorId=A1; Operation=Publish; RequestKey=publish-demo-1; RequestHash=HP1; TemplateId=M1; ResultVersionId=V1; Result={"versionId":"V1","number":1,"templateVersion":2}; CreatedAtUtc=T1. Result minh họa JSON, ID thật là uuid. |

Sau mở đầu tiên, Q1 Used=1 và Access U1/V1 cùng tồn tại; mất mạng rồi mở lại không tạo OP2. A1 sửa tên và thay ảnh F1 bằng F3: V1.EditVersion=2, cover/link trỏ F3; M1.CurrentVersionId vẫn V1. U1 xem lịch sử thấy tên/ảnh mới nhưng Used vẫn 1. F1 không bị ghi đè.

Tạo nháp V2: TemplateId=M1, BaseVersionId=V1, State=Draft, Number/PublishedAtUtc NULL, EditVersion=1; các link sao lại dùng F3/F2. Sửa nháp không đổi V1. Khi công bố ở T3 sau khi catalog thành C2, V2 phải dùng tầng 2 và C2; V1 tiếp tục C1/tầng 3. M1.CurrentVersionId=V2, V2.Number=2, V2.State=Published; V1 không còn sửa được. U1 mở V2 đủ điều kiện tạo OP2 và Access U1/V2, Q1 Used=2; lịch sử có hai dòng. Gói hết hạn hoặc M1.IsHidden=true không xóa hai dòng này.

Lỗi trước commit OP2: quota và Access/operation đều không đổi. Xóa một nháp V3 để lại receipt Operation=DeleteDraft, ResultVersionId=V3; không có FK để receipt ngăn xóa nháp. Nếu kỳ không giới hạn, Used vẫn tăng cho OP1, Limit=NULL; quyền U1/V1 không phụ thuộc Limit về sau.
Các thay đổi lượt trong tình huống trên chỉ giải thích quan hệ; schema và bản ghi Access/UsageOperation minh họa nằm tại TDD-LIB-002/Data Model.

**Chuẩn hóa, chỉ mục và vòng đời**

| Dữ kiện | Nguồn duy nhất, khóa và đánh đổi |
|---|---|
| Nội dung | Phụ thuộc VersionId; tên/kích thước không lặp ở Template, Access hoặc receipt phục vụ đọc. |
| Phân loại | Phụ thuộc revision+type; lưu FK ghim lịch sử, không sao chép tên hiện hành. |
| File | Metadata phụ thuộc AssetId; VersionAsset chỉ lưu quan hệ/thứ tự. TemplateId lặp để FK ghép ngăn gắn file mẫu khác, được ràng buộc ở cả hai đầu. |
| Receipt | JSON là kết quả thao tác bất biến để gửi lại, không phải nguồn nội dung hoặc bảng snapshot toàn mẫu. |

Index theo truy vấn: Version(TemplateId,Number) unique; Version(PublishedAtUtc DESC,Id DESC) WHERE State='Published'; Version(DrawingKind,BuildingTypeId,FloorCount,HasTum) WHERE State='Published'; Asset(TemplateId,Id) unique; VersionAsset(VersionId,Position) unique và index AssetId cho tra tham chiếu; receipt unique key. Không tạo full-text engine; tìm tên bằng ILIKE có escape ký tự wildcard, truy vấn SQL trước phân trang. Hiệu năng contains có thể cần pg_trgm khi có số liệu, không cam kết B-tree tăng tốc contains.

Danh sách public JOIN Template.CurrentVersionId, IsHidden=false; sort PublishedAtUtc DESC, VersionId DESC để ổn định khi trùng thời gian; pageIndex/pageSize dùng chuẩn PagedResult của repo. Không lưu LastPublishedAt thứ hai trên Template. Assets cũng phân trang để số tệp không biến thành payload/RAM không giới hạn. Trang assets nhận expectedEditVersion; nội dung đổi giữa hai trang trả 409 và client tải lại từ đầu, không ghép danh sách hai lần sửa. Các khóa/hình dạng DTO này là kiểm soát kỹ thuật, không thêm phiên bản tính lượt.

Migration là công việc triển khai sau: kiểm tra schema thực tế trước; tạo bảng Template/Version/Asset/link/receipt, các FK vòng sau bảng; thêm UsageOperation.TemplateVersionId và Access sau module SUB, thêm permission. Không chạy migration ở tác vụ này. Code đọc hiện chưa có module LIB/SUB, nhưng không suy ra production trống: nếu đã có lượt tra cứu kiểu cũ thì dừng backfill tự động, cần ánh xạ phiên bản thật từ dữ liệu cũ; không gán mọi lượt cũ vào current version. Rollback sau có Access không được drop lịch sử/asset; ưu tiên tắt route mới và sửa tiếp trên schema giữ dữ liệu.
Schema quota/Access được triển khai theo TDD-LIB-002 sau bảng Version; không tạo thêm bảng do tách tài liệu.

## Internal API

### Endpoints

Tất cả route dưới đây là đề xuất. Mutation quản trị cần library.manage, Idempotency-Key và CSRF. RequestKey 1–100 ký tự; request hash chứa route, target, expected versions và body chuẩn hóa. Cùng actor/operation/key khác hash trả 409; cùng hash trả kết quả đã commit trước kiểm optimistic version, nhưng vẫn kiểm quyền hiện tại. API khách không nhận AccountId từ client.

- **GET** `/api/v1/design-templates` — Công khai. query `drawingKind,buildingTypeId,floorCount,hasTum,name,pageIndex,pageSize`; thiếu hasTum nghĩa Tất cả. Trả summary: templateId,versionId,number,name,dimensions,type label,floorCount,hasTum,thumbnailUrl,publishedAtUtc. Không có file keys hoặc manifest chi tiết.
- **GET** `/api/v1/design-templates/filters` — Công khai. query drawingKind/buildingTypeId; trả loại và số tầng hiện hành cộng giá trị cũ có mẫu public.
- **GET** `/api/v1/design-templates/{templateId}/thumbnail` — Công khai chỉ khi có current public; stream thumbnail của cover hiện hành, không chấp nhận assetId bất kỳ.
- **GET** `/api/v1/admin/library/templates` — library.manage; danh sách quản trị gồm hidden và trạng thái current; có phân trang, không qua quota.
- **GET** `/api/v1/admin/library/templates/{templateId}/versions` — library.manage; phiên bản/nháp và EditVersion để chọn sửa/preview; không cho sửa old.
- **POST** `/api/v1/admin/library/templates` — Tạo Template và nháp đầu tiên với dữ liệu; 201 `{templateId,versionId,templateVersion,editVersion}`. Cho lưu nháp thiếu trường; dữ liệu có mặt phải hợp lệ. Body rỗng tạo nháp trống, chưa công khai.
- **POST** `/api/v1/admin/library/templates/{templateId}/drafts` — `{expectedTemplateVersion,baseVersionId}`; sao current thành nháp mới, không đổi current; base phải là current lúc tiếp nhận.
- **PUT** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}` — `{expectedEditVersion,name,description,drawingKind,widthM,lengthM,areaM2,buildingTypeId,floorCount,hasTum}`; sửa metadata current/nháp. So giá trị phân loại với bản đã lưu để quyết định revalidate; không tự ghim catalog mới khi chỉ sửa tên.
- **POST** `/api/v1/admin/library/templates/{templateId}/assets` — Upload một file stream ngoài transaction dài; trả assetId sau xác minh. Upload gửi lại có thể để lại asset chưa gắn; không tự thay nội dung version. Không áp cơ chế receipt quản trị cho stream bytes trong đợt này.
- **PUT** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}/assets/{assetId}` — `{expectedEditVersion,position,setAsCover}`; attach/reposition asset cùng template, tăng EditVersion. Nếu setAsCover=false thì giữ cover hiện có. Vị trí đã chiếm trả 409, dùng reorder để đổi chỗ.
- **POST** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}/reorder` — `{expectedEditVersion,items:[{assetId,position}]}`; đổi vị trí tập con, kiểm không trùng toàn bộ phiên bản, không xóa asset không nằm trong request.
- **DELETE** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}/assets/{assetId}` — expectedEditVersion trong query; detach, không xóa bytes. Không được làm current Published thiếu ảnh/cover; phải chọn cover khác trước.
- **POST** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}/publish` — `{expectedTemplateVersion,expectedEditVersion,expectedCurrentVersionId}`; kiểm đủ nội dung/catalog và đổi con trỏ cùng receipt.
- **PUT** `/api/v1/admin/library/templates/{templateId}/visibility` — `{expectedTemplateVersion,isHidden}`; cùng version mẫu, không đổi lượt.
- **DELETE** `/api/v1/admin/library/templates/{templateId}/versions/{versionId}` — expectedEditVersion trong query; chỉ nháp, xóa link và nháp cùng receipt; published trả 409.

Các API mở, preview/chi tiết và tải tài nguyên được định nghĩa duy nhất tại TDD-LIB-002/Internal API.

### Examples

#### POST /api/v1/admin/library/templates/{templateId}/versions/{versionId}/publish

```
Request:
Header Idempotency-Key: publish-v2-demo
{"expectedTemplateVersion":4,"expectedEditVersion":3,"expectedCurrentVersionId":"10000000-0000-0000-0000-000000000001"}

Response 200:
{"versionId":"10000000-0000-0000-0000-000000000002","number":2,"templateVersion":5,"editVersion":4}

Error Response:
{"code":"InvalidLibraryContent","detail":"Cần chọn số tầng hợp lệ theo cấu hình hiện hành trước khi công bố."}
```

### Error Codes

- **Unauthorized** (401): Phiên thiếu/không hợp lệ; dùng mã xác thực hiện có khi tích hợp.
- **AccessForbidden** (403): Thiếu library.manage hoặc không có Access cho đọc nội dung; không trả tài nguyên bảo vệ.
- **LibraryNotFound** (404): Mẫu/version không tồn tại hoặc không có bản public trong route công khai.
- **LibraryVersionConflict** (409): Mutation quản trị dùng expected version cũ.
- **LibraryVersionReadOnly** (409): Sửa phiên bản đã bị thay thế hoặc xóa Published.
- **LibraryPositionConflict** (409): Vị trí liên kết tài nguyên bị trùng.
- **IdempotencyConflict** (409): Cùng mutation key nhưng khác nội dung.
- **InvalidLibraryContent** (422): Thiếu dữ liệu khi công bố, sai kích thước/phân loại/cover/membership hoặc dữ liệu có giá trị không hợp lệ.
- **UnsupportedLibraryFile** (422): Nội dung file không thuộc định dạng cho phép hoặc không qua xác minh.
- **LibraryStorageUnavailable** (503): Không chuẩn bị/upload/đọc được tài nguyên; không tính lượt nếu trước commit mở đầu.

Lỗi giới hạn truyền tải 413/timeout thuộc cấu hình hạ tầng, không tự chuyển thành hạn mức nghiệp vụ.

## External API

### Endpoints

- **Kho tệp riêng tư — nhà cung cấp chưa chọn** — port `PutImmutable`, `ProbeReadable`, `OpenRead(range)`; không chốt URL hoặc SDK giả. Object mới có key riêng; không overwrite.
- **Bộ xác minh file và tạo thumbnail — adapter nội bộ** — kiểm bytes thực và sinh thumbnail ảnh; cần đánh giá thư viện hỗ trợ DWG/DXF trước mở upload.

### Fields

- **StorageKey** — Do server sinh và lưu nội bộ, không xuất vào public DTO.
- **SizeBytes/Sha256/MediaType** — Tính/xác minh từ bytes thật, không tin trường khách gửi.
- **Range** — Khoảng byte được kiểm hợp lệ; stream có kiểm quyền, không buffer toàn bộ.

### Error Handling

Upload hoàn tất nhưng metadata lỗi để lại object chưa gắn, không báo version đã lưu. Gửi lại attach/publish dùng cùng key/hash; không upload lại trong transaction. Timeout đọc/ghi trước mở đầu làm mở đầu thất bại không trừ lượt; lỗi tải sau khi đã có quyền chỉ cần tải lại. Không tự xóa asset phiên bản cũ để bù lỗi storage.

### Quirks

- Không thể rollback kho object bằng SQL. Chính sách dọn rác phải bảo vệ mọi link current/old/nháp.
- Chưa có provider, thông số timeout, khả năng Range, giới hạn một object và request body được kiểm chứng. Đây là điều kiện vận hành trước production, không phải cam kết upload vật lý vô hạn.
- Không sao chép quy tắc ảnh 5 MB/10 MB của PROJ sang LIB.

## References

### User Stories

- STORY-LIB-001
- STORY-LIB-002
- STORY-LIB-003
- STORY-PROJ-005

### Business Rules

- BR-LIB-001/Then
- BR-LIB-002/Then
- BR-LIB-003/Then
- BR-PROJ-004/Then
- BR-RBAC-010/Then
- BR-RBAC-011/Then

### Use Cases

### Others

- Phạm vi use case: Quản lý mẫu, sửa tại chỗ, công bố phiên bản mới, ẩn/hiện và xóa nháp. Tìm/lọc, mở lần đầu, xem lại và tải tài nguyên từ lịch sử.

- [TDD-LIB-002](TDD-LIB-002.md): LibraryAccess, quota, lịch sử, tải có quyền và thứ tự khóa chung.

- [Bảng System Test LIB](../discovery/library-system-test-coverage.md) — 27 đặc tả chưa thực thi.
- [TDD-PROJ-001](TDD-PROJ-001.md) — catalog revision, kiểu số, UoW và storage đề xuất.
- [TDD-SUB-001](TDD-SUB-001.md) — BenefitDefinition và mã catalog.detail.
- [TDD-SUB-002](TDD-SUB-002.md) — kỳ/quota; phần tra cứu được cập nhật theo TDD-LIB-002.
- [TDD-SUB-005](TDD-SUB-005.md) — LifecycleState khi kiểm hiệu lực kỳ.
- [TDD-RBAC-001](TDD-RBAC-001.md) — policy/permission; LIB thêm quyền không cần Assignment.
- Hiện trạng code: `bmt-be/src/bmt-be.persistence/ApplicationDbContext.cs`, `bmt-be/src/bmt-be.application/behaviors/TransactionPipelineBehavior.cs`, `bmt-be/src/bmt-be.persistence/repositories/EFUnitOfWork.cs`, `bmt-be/src/bmt-be.persistence/dependencyInjection/extensions/ServiceCollectionExtensions.cs`, `bmt-be/src/bmt-be.contract/constants/PermissionNames.cs`.
- Chưa hoàn tất rà soát toàn bộ chuỗi phụ thuộc ngoài LIB; các UT-SUB và phần TDD-SUB lịch sử về bytes replay cần đối chiếu khi cập nhật thiết kế được chốt. Không dùng ghi chú cũ để ghi đè BR-LIB-003.

## Change Log
