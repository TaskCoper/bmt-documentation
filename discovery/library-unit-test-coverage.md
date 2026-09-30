# Truy vết Unit Test bổ sung cho thư viện mẫu

Người dùng đã chốt TDD-LIB-001 và phần bổ sung liên quan trong TDD-LIB-002 trong hội thoại ngày 30/09/2026. Bộ này gồm 26 đặc tả UT-LIB-053–078 cho nhiều phong cách 3D, dùng chung danh mục và hai luồng tìm mẫu. Đã triển khai BE, thêm mã kiểm thử và chạy các bộ test thư viện. Kết quả thực thi ghi bên dưới. Reviewer/Approver: Tân Trần; Owner kiểm thử chưa xác định. Trạng thái Draft không phải kết quả thực thi hoặc phê duyệt trên hệ thống.

UT-LIB-001–052 giữ nguyên phạm vi trước bổ sung; các ca mới bổ sung hành vi chưa được kiểm trong bộ cũ. Các đặc tả là căn cứ kiểm thử; không đồng nghĩa mọi biến thể trong bảng đều có một test tự động riêng. Dữ liệu AR1/IN1/RA/BT1 trong đặc tả là fixture minh họa, không phải dữ liệu production.

| Hành vi | Story / Rule | Thiết kế | Unit Test | System Test / tích hợp |
| --- | --- | --- | --- | --- |
| Chọn nhiều phong cách, đúng nhóm/loại, đủ dữ liệu công bố | STORY-LIB-001/AC-008–AC-010; BR-LIB-001 khoản 9–10 | TDD-LIB-001/Architecture | [UT-LIB-053](../unittest/UT-LIB-053.md), [UT-LIB-054](../unittest/UT-LIB-054.md), [UT-LIB-055](../unittest/UT-LIB-055.md), [UT-LIB-056](../unittest/UT-LIB-056.md), [UT-LIB-057](../unittest/UT-LIB-057.md) | [ST-LIB-032](../systemtest/ST-LIB-032.md), [ST-LIB-033](../systemtest/ST-LIB-033.md), [ST-LIB-034](../systemtest/ST-LIB-034.md), [ST-LIB-049](../systemtest/ST-LIB-049.md), [ST-LIB-050](../systemtest/ST-LIB-050.md) |
| Giữ/xóa/thay mảng và replay tương thích | STORY-LIB-001/AC-009; BR-LIB-002/Then | TDD-LIB-001/Internal API | [UT-LIB-058](../unittest/UT-LIB-058.md), [UT-LIB-059](../unittest/UT-LIB-059.md), [UT-LIB-060](../unittest/UT-LIB-060.md) | [ST-LIB-033](../systemtest/ST-LIB-033.md), [ST-LIB-050](../systemtest/ST-LIB-050.md) |
| Sửa phân loại, giữ revision cũ, tranh chấp và lỗi ghi | STORY-LIB-001/AC-010–AC-011; BR-LIB-001 khoản 5,10 | TDD-LIB-001/Architecture | [UT-LIB-061](../unittest/UT-LIB-061.md), [UT-LIB-062](../unittest/UT-LIB-062.md), [UT-LIB-063](../unittest/UT-LIB-063.md), [UT-LIB-064](../unittest/UT-LIB-064.md), [UT-LIB-065](../unittest/UT-LIB-065.md) | [ST-LIB-034](../systemtest/ST-LIB-034.md), [ST-LIB-035](../systemtest/ST-LIB-035.md), [ST-LIB-050](../systemtest/ST-LIB-050.md) |
| Match 2D/3D theo cấu hình dự toán, khác revision | STORY-LIB-002/AC-005–AC-009; BR-LIB-001 khoản 11–14 | TDD-LIB-001/Architecture và Internal API | [UT-LIB-066](../unittest/UT-LIB-066.md), [UT-LIB-067](../unittest/UT-LIB-067.md), [UT-LIB-068](../unittest/UT-LIB-068.md), [UT-LIB-069](../unittest/UT-LIB-069.md), [UT-LIB-070](../unittest/UT-LIB-070.md) | [ST-LIB-036](../systemtest/ST-LIB-036.md), [ST-LIB-037](../systemtest/ST-LIB-037.md), [ST-LIB-038](../systemtest/ST-LIB-038.md), [ST-LIB-039](../systemtest/ST-LIB-039.md), [ST-LIB-040](../systemtest/ST-LIB-040.md), [ST-LIB-041](../systemtest/ST-LIB-041.md), [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| Bộ lọc tự do: OR trong nhóm, AND giữa các nhóm | STORY-LIB-002/AC-010; BR-LIB-001 khoản 15 | TDD-LIB-001/Internal API | [UT-LIB-071](../unittest/UT-LIB-071.md) | [ST-LIB-042](../systemtest/ST-LIB-042.md), [ST-LIB-043](../systemtest/ST-LIB-043.md), [ST-LIB-044](../systemtest/ST-LIB-044.md) |
| Phân trang và tính offset | STORY-LIB-002/AC-004; BR-LIB-001 khoản 8 | TDD-LIB-001/Internal API | [UT-LIB-072](../unittest/UT-LIB-072.md) | [ST-LIB-042](../systemtest/ST-LIB-042.md), [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| Danh mục lựa chọn cho người quản lý LIB | STORY-LIB-001/AC-008, AC-010; BR-LIB-001 khoản 9–10 | TDD-LIB-001/Internal API | [UT-LIB-073](../unittest/UT-LIB-073.md), [UT-LIB-078](../unittest/UT-LIB-078.md) | [ST-LIB-049](../systemtest/ST-LIB-049.md) |
| Đọc phong cách có quyền theo revision mẫu | BR-LIB-003/Then; STORY-LIB-001/AC-011 | TDD-LIB-002/Architecture và Internal API | [UT-LIB-074](../unittest/UT-LIB-074.md) | [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| Tạo/xóa nháp và công bố: link riêng mỗi phiên bản | STORY-LIB-001/AC-003, AC-009, AC-011; BR-LIB-002/Then | TDD-LIB-001/Architecture và Data Model | [UT-LIB-075](../unittest/UT-LIB-075.md), [UT-LIB-076](../unittest/UT-LIB-076.md) | [ST-LIB-033](../systemtest/ST-LIB-033.md), [ST-LIB-035](../systemtest/ST-LIB-035.md), [ST-LIB-050](../systemtest/ST-LIB-050.md), [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| Danh sách quản trị đọc đủ hai tập phong cách | STORY-LIB-001/AC-008; BR-LIB-001 khoản 9–10 | TDD-LIB-001/Internal API | [UT-LIB-077](../unittest/UT-LIB-077.md) | [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| FE tự gọi khi AI Pending, bỏ phản hồi cũ, lỗi độc lập | STORY-LIB-002/AC-011; BR-LIB-001 khoản 16 | TDD-LIB-001/Architecture | Kiểm tại FE qua System Test, không dùng unit backend để kết luận | [ST-LIB-045](../systemtest/ST-LIB-045.md), [ST-LIB-046](../systemtest/ST-LIB-046.md), [ST-LIB-047](../systemtest/ST-LIB-047.md) |

## Ranh giới kiểm chứng

- Unit Test kiểm validation, tập điều kiện được tạo, mapping, điều phối handler và exception. Mock trả sẵn danh sách không chứng minh được phép lọc SQL, khóa hoặc rollback.
- ST-LIB-042–044/048 kiểm truy vấn thật: OR/AND, hai nhóm riêng, không lặp mẫu, lọc trước count/page, dùng ID ổn định qua revision và danh mục cũ còn mẫu công khai sử dụng.
- ST-LIB-049 kiểm policy API thực tế: chỉ cần library.manage để đọc lựa chọn, không cấp quyền sửa danh mục. ST-LIB-048 kiểm binding, route công khai và phản hồi chỉ chứa summary.
- ST-LIB-050/051 cần PostgreSQL thật để kiểm FK/PK/NOT NULL, hoàn tác parent/link/receipt, vòng đời phiên bản và snapshot đọc. Điều phối bằng điểm dừng xác định, không dựa vào thời gian sleep ngẫu nhiên. Khi triển khai, bổ sung integration concurrency cho catalog đổi đồng thời lúc công bố và hai save cùng EditVersion theo chiến lược trong TDD; UT-LIB-064 chỉ kiểm nhánh điều phối bằng fake.
- ST-LIB-045–047 cần FE, API dự toán có quyền và cổng AI thử nghiệm để kiểm đầu vào đã tiếp nhận, phản hồi về muộn và lỗi thư viện độc lập. Chưa đặt chu kỳ polling thư viện hoặc ngưỡng hiệu năng.

## Tài liệu nguồn và phần chưa kiểm chứng

- [TDD-LIB-001](../tdd/TDD-LIB-001.md), [TDD-LIB-002](../tdd/TDD-LIB-002.md), [BR-LIB-001](../businessrule/BR-LIB-001.md), [STORY-LIB-001](../userstory/STORY-LIB-001.md), [STORY-LIB-002](../userstory/STORY-LIB-002.md).
- [Bảng System Test](library-system-test-coverage.md) có các ca cũ và mới; [ghi nhận phạm vi](library-planning.md) phân biệt phần đã có code với phần thiết kế bổ sung.
- Hai URL FE người dùng cung cấp chưa truy cập được trong lần đối chiếu; căn cứ luồng giao diện là mô tả đã xác nhận. Đã chạy test backend và migration trên PostgreSQL tạm; chưa import tài liệu hoặc thử trên môi trường dùng chung. Chưa rà soát hết chuỗi tham chiếu ngoài LIB và danh mục trực tiếp; không dùng bảng này để tuyên bố toàn bộ hệ thống đã được kiểm chứng.


## Kết quả triển khai BE ngày 30/09/2026

Phạm vi đã làm: schema phong cách theo phiên bản, validation khi lưu/công bố/sửa tài nguyên, tương thích payload và biên nhận cũ, sao/xóa nháp, bộ lọc tự do, match bằng đầu vào dự toán, danh mục quản trị và đọc chi tiết có quyền. Người dùng xác nhận chỉ làm BE; chưa kiểm FE, AI Pending hoặc phản hồi về muộn trên giao diện (ST-LIB-045–047).

Các đường dẫn mã dưới đây tính từ `bmt-be/`:

| Hành vi đã kiểm | Mã test / bằng chứng |
| --- | --- |
| Nhiều style, sai nhóm/nhóm tắt/2D, ID rỗng/trùng, bắt buộc khi công bố, null/[]/replace | `test/bmt-be.application.tests/usecases/library/LibraryStyleTests.cs` |
| Hash không phụ thuộc thứ tự, phân biệt giữ/xóa; biên nhận cũ dùng hash cố định trước thay đổi | `Handle_Create_IdempotencyUsesSets_AndDistinguishesPreserveFromClear`, `Handle_Create_ReplaysPreStyleReceiptFixture_WithLegacyHash` |
| Reread phát hiện phân loại hoặc EditVersion đã đổi; không lấy catalog lock sau Template | `Handle_Save_RereadChangesClassificationOrEditVersion_ConflictsBeforeWrites` |
| Giữ revision khi sửa tên; đổi phong cách/công bố chuyển toàn bộ link; bản cũ và nháp độc lập | `test/bmt-be.integration.tests/LibraryStyleFlowTests.cs`, ca `Handle_SaveAndPublish_RepinLinks_CopyDraft_DeleteOnlyOwnLinks_KeepVersionIdentity` |
| Khớp qua revision theo ID, tầng và false tum; query không ghi receipt/Access | `Handle_Matching_CrossRevision_UsesIdsAndAllApplicableFields_WithoutWrites` |
| OR trong nhóm, AND giữa nhóm; không lặp mẫu; lọc trước count/page; loại hidden/draft | `Handle_PublicFilters_OrWithinGroupAndAcrossGroups_FilterBeforePaging_NoDuplicates` |
| Hợp danh mục hiện hành/cũ còn mẫu; tên hiện hành ưu tiên; loại hidden | `Handle_FilterOptions_UnionHistoricalPublicStyles_CurrentNameWins_HiddenExcluded` |
| Khóa ngoại chặn link khác revision dù assignment hợp lệ; lỗi sau các SaveChanges trung gian hoàn tác parent/link/receipt | `Handle_Database_RejectsLinkWithDifferentPinnedRevision_EvenWhenAssignmentExists`, `Handle_Repin_FailureAfterIntermediateSaves_RollsBackParentLinksAndReceipt` |
| Hai save cùng EditVersion chỉ một thành công, không trộn tập phong cách | `Handle_ConcurrentStyleEdits_OneWinner_NoMixedStyleSets` |
| Detail đọc metadata và phong cách cùng snapshot khi có save chen vào | `Handle_Detail_ReadsMetadataAndStylesFromSameSnapshotDuringEdit` |
| Route thật, library.manage, 401/403, no-store, matches không cần đăng nhập/key, UUID hỏng, mảng query lặp | `test/bmt-be.api.tests/library/LibraryApiAuthorizationTests.cs` |

Chạy từ `bmt-be/`, dùng dependency hiện có, không restore:

```sh
dotnet test test/bmt-be.application.tests --no-restore -m:1 -nr:false --filter FullyQualifiedName~Library
dotnet test test/bmt-be.integration.tests --no-restore -m:1 -nr:false --filter FullyQualifiedName~Library
dotnet test test/bmt-be.api.tests --no-restore -m:1 -nr:false --filter FullyQualifiedName~Library
```

Kết quả: **206 unit test, 93 test PostgreSQL và 67 test HTTP đều đạt; tổng 366, không có ca bị bỏ qua**. Các số này gồm cả kiểm thử hồi quy LIB trước thay đổi, không phải số đặc tả mới. Test tích hợp dùng Testcontainers PostgreSQL 15, chạy toàn bộ migration trên database tạm rồi hủy container. Test HTTP dùng route/middleware/policy thật và MediatR giả; luồng handler/SQL được kiểm riêng trong test PostgreSQL. Vì vậy đây chưa phải kết quả end-to-end từ FE tới database.

Migration mới: `20260930101013_AddLibraryVersionStyles`. Chưa chạy migration trên database dùng chung. Đợt bàn giao này commit và đẩy phần LIB lên `develop` theo yêu cầu người dùng; kết quả CI/CD cần theo dõi riêng. Migration đứng sau migration contractor đã có sẵn trong workspace; không tách hoặc hoàn tác thay đổi đó. Trước phát hành, kiểm read-only số mẫu 3D ở môi trường đích và dung lượng bảng theo TDD-LIB-001; không tự gán phong cách cho dữ liệu phát sinh ngoài giả định ban đầu.
