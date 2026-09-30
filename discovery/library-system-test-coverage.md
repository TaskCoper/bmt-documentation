# Truy vết System Test thư viện mẫu

**Cập nhật triển khai BE ngày 30/09/2026:** đã có kiểm thử unit, HTTP và PostgreSQL cho phần nhiều phong cách/hai luồng tìm. Xem [kết quả và giới hạn kiểm chứng](library-unit-test-coverage.md#kết-quả-triển-khai-be-ngày-30092026). Các dòng “chưa thực thi” bên dưới ghi thời điểm soạn đặc tả; không dùng để thay kết quả thực chạy. FE ST-LIB-045–047 nằm ngoài phạm vi người dùng giao lần này.

US và BR đã được người dùng chốt trong hội thoại. 51 ca dưới đây là đặc tả, chưa thực thi; không phải kết quả Pass. Reviewer/Approver: Tân Trần. Owner kiểm thử chưa xác định.

**Cập nhật 25/09/2026:** quyền quản lý thư viện mẫu theo STORY-RBAC-001 và BR-LIB-002 có mã kỹ thuật `library.manage` trong TDD-RBAC-001, không gắn phân công; ST-LIB-011 kiểm thêm nhân viên thuộc vai trò tùy chỉnh được cấp mã này. Đã thêm ST-LIB-028 kiểm việc mở mẫu lần đầu và thay đổi gói của cùng khách chạy đồng thời. Bổ sung sau đó: ST-LIB-029 kiểm sửa tại chỗ cùng phiên bản không tính lượt mới (STORY-LIB-003/ALT-01), ST-LIB-030 kiểm mẫu bị ẩn không nhận lượt mở mới nhưng người đã có quyền vẫn xem lại được (STORY-LIB-003/EXC-01); các ca đang phủ luồng ALT/EXC được ghi thêm mã luồng trong TEST_LINKS.

**Cập nhật 30/09/2026:** người dùng đã chốt phần US/BR bổ sung. Thêm ST-LIB-032–041 cho nhiều phong cách 3D, danh mục dùng chung và tìm mẫu khớp mọi điều kiện giữa các phiên bản danh mục. Các ca mới chưa có code hoặc kết quả chạy; không thay trạng thái các ca cũ.

**Bổ sung sau khi chốt TDD ngày 30/09/2026:** ST-LIB-042–051 kiểm hai luồng tìm, API, lựa chọn danh mục, khóa ngoại/rollback và đọc theo phiên bản. Tất cả là đặc tả Draft, chưa thực thi. Các ca Integration boundary yêu cầu PostgreSQL thật; không dùng mock để kết luận ràng buộc dữ liệu đã đúng.

| Tiêu chí | System Test |
| --- | --- |
| STORY-LIB-002/AC-010 | [ST-LIB-042](../systemtest/ST-LIB-042.md), [ST-LIB-043](../systemtest/ST-LIB-043.md), [ST-LIB-044](../systemtest/ST-LIB-044.md) |
| STORY-LIB-002/AC-011 | [ST-LIB-045](../systemtest/ST-LIB-045.md), [ST-LIB-046](../systemtest/ST-LIB-046.md), [ST-LIB-047](../systemtest/ST-LIB-047.md) |
| TDD-LIB-001/Internal API: match công khai, dữ liệu áp dụng | [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| TDD-LIB-001/Internal API: classification-options và quyền | [ST-LIB-049](../systemtest/ST-LIB-049.md) |
| TDD-LIB-001/Data Model: FK và rollback đổi revision | [ST-LIB-050](../systemtest/ST-LIB-050.md) |
| TDD-LIB-001/Data Model, TDD-LIB-002/Architecture: link theo phiên bản, quyền và snapshot đọc | [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| STORY-LIB-002/AC-009 | [ST-LIB-041](../systemtest/ST-LIB-041.md), [ST-LIB-047](../systemtest/ST-LIB-047.md) |
| STORY-LIB-002/AC-008 | [ST-LIB-040](../systemtest/ST-LIB-040.md) |
| STORY-LIB-002/ALT-02 | [ST-LIB-039](../systemtest/ST-LIB-039.md), [ST-LIB-040](../systemtest/ST-LIB-040.md), [ST-LIB-044](../systemtest/ST-LIB-044.md) |
| STORY-LIB-002/AC-007 | [ST-LIB-038](../systemtest/ST-LIB-038.md), [ST-LIB-039](../systemtest/ST-LIB-039.md), [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| STORY-LIB-002/AC-006 | [ST-LIB-037](../systemtest/ST-LIB-037.md), [ST-LIB-038](../systemtest/ST-LIB-038.md), [ST-LIB-045](../systemtest/ST-LIB-045.md) |
| STORY-LIB-002/AC-005 | [ST-LIB-036](../systemtest/ST-LIB-036.md), [ST-LIB-038](../systemtest/ST-LIB-038.md), [ST-LIB-045](../systemtest/ST-LIB-045.md), [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| STORY-LIB-001/AC-011 | [ST-LIB-035](../systemtest/ST-LIB-035.md), [ST-LIB-050](../systemtest/ST-LIB-050.md), [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| STORY-LIB-001/AC-010 | [ST-LIB-034](../systemtest/ST-LIB-034.md), [ST-LIB-049](../systemtest/ST-LIB-049.md) |
| STORY-LIB-001/AC-009 | [ST-LIB-033](../systemtest/ST-LIB-033.md) |
| STORY-LIB-001/AC-008 | [ST-LIB-032](../systemtest/ST-LIB-032.md), [ST-LIB-034](../systemtest/ST-LIB-034.md), [ST-LIB-049](../systemtest/ST-LIB-049.md), [ST-LIB-050](../systemtest/ST-LIB-050.md) |
| STORY-LIB-001/AC-001 | [ST-LIB-001](../systemtest/ST-LIB-001.md), [ST-LIB-002](../systemtest/ST-LIB-002.md), [ST-LIB-011](../systemtest/ST-LIB-011.md) |
| STORY-LIB-001/AC-002 | [ST-LIB-006](../systemtest/ST-LIB-006.md), [ST-LIB-010](../systemtest/ST-LIB-010.md) |
| STORY-LIB-001/AC-003 | [ST-LIB-007](../systemtest/ST-LIB-007.md), [ST-LIB-051](../systemtest/ST-LIB-051.md) |
| STORY-LIB-001/AC-004 | [ST-LIB-008](../systemtest/ST-LIB-008.md) |
| STORY-LIB-001/AC-005 | [ST-LIB-009](../systemtest/ST-LIB-009.md) |
| STORY-LIB-001/AC-006 | [ST-LIB-005](../systemtest/ST-LIB-005.md) |
| STORY-LIB-001/AC-007 | [ST-LIB-003](../systemtest/ST-LIB-003.md), [ST-LIB-004](../systemtest/ST-LIB-004.md) |
| STORY-LIB-002/AC-001 | [ST-LIB-012](../systemtest/ST-LIB-012.md), [ST-LIB-016](../systemtest/ST-LIB-016.md), [ST-LIB-048](../systemtest/ST-LIB-048.md) |
| STORY-LIB-002/AC-002 | [ST-LIB-013](../systemtest/ST-LIB-013.md) |
| STORY-LIB-002/AC-003 | [ST-LIB-014](../systemtest/ST-LIB-014.md) |
| STORY-LIB-002/AC-004 | [ST-LIB-015](../systemtest/ST-LIB-015.md) |
| STORY-LIB-003/AC-001 | [ST-LIB-017](../systemtest/ST-LIB-017.md), [ST-LIB-027](../systemtest/ST-LIB-027.md) |
| STORY-LIB-003/AC-002 | [ST-LIB-018](../systemtest/ST-LIB-018.md) |
| STORY-LIB-003/AC-003 | [ST-LIB-019](../systemtest/ST-LIB-019.md), [ST-LIB-028](../systemtest/ST-LIB-028.md) |
| STORY-LIB-003/AC-004 | [ST-LIB-020](../systemtest/ST-LIB-020.md), [ST-LIB-021](../systemtest/ST-LIB-021.md) |
| STORY-LIB-003/AC-005 | [ST-LIB-022](../systemtest/ST-LIB-022.md), [ST-LIB-023](../systemtest/ST-LIB-023.md), [ST-LIB-025](../systemtest/ST-LIB-025.md) |
| STORY-LIB-003/AC-006 | [ST-LIB-024](../systemtest/ST-LIB-024.md) |
| STORY-LIB-003/AC-007 | [ST-LIB-026](../systemtest/ST-LIB-026.md) |
| STORY-LIB-001/ALT-01 | [ST-LIB-010](../systemtest/ST-LIB-010.md), [ST-LIB-035](../systemtest/ST-LIB-035.md) |
| STORY-LIB-001/EXC-01 | [ST-LIB-011](../systemtest/ST-LIB-011.md), [ST-LIB-033](../systemtest/ST-LIB-033.md), [ST-LIB-034](../systemtest/ST-LIB-034.md) |
| STORY-LIB-001/Main Flow | [ST-LIB-031](../systemtest/ST-LIB-031.md) |
| STORY-LIB-002/ALT-01 | [ST-LIB-014](../systemtest/ST-LIB-014.md) |
| STORY-LIB-002/EXC-01 | [ST-LIB-016](../systemtest/ST-LIB-016.md), [ST-LIB-041](../systemtest/ST-LIB-041.md) |
| STORY-LIB-003/ALT-01 | [ST-LIB-018](../systemtest/ST-LIB-018.md), [ST-LIB-029](../systemtest/ST-LIB-029.md) |
| STORY-LIB-003/EXC-01 | [ST-LIB-021](../systemtest/ST-LIB-021.md), [ST-LIB-022](../systemtest/ST-LIB-022.md), [ST-LIB-027](../systemtest/ST-LIB-027.md), [ST-LIB-030](../systemtest/ST-LIB-030.md) |

Đã kiểm tra cấu trúc mỗi ca một file, 13 cột, mã/section tham chiếu tồn tại và Trace to khớp TEST_LINKS. Bao phủ tham chiếu không đồng nghĩa đã chứng minh hành vi đúng.

**Cập nhật 26/09/2026 (triển khai TDD-LIB-001):** backend của các ca ST-LIB-001 đến ST-LIB-016 đã có ở nhánh `feature/library-admin` của `bmt-be` (commit `66e4671`, chưa merge). Các ca System Test vẫn chưa chạy trên môi trường thử; bảng dưới chỉ ghi test tự động phía backend đang kiểm cùng hành vi, không phải kết quả System Test. Các ca ST-LIB-017 đến ST-LIB-030 phụ thuộc TDD-LIB-002, chưa có backend (riêng phần sửa tại chỗ, ẩn mẫu và xóa nháp của ST-LIB-006, ST-LIB-008, ST-LIB-009 đã có).

| System Test | Test tự động phía backend (test class của `bmt-be`) |
| --- | --- |
| ST-LIB-001 | `LibraryFlowTests.Handle_CreateAttachSaveAndPublish_AppearsPublicly` |
| ST-LIB-002 | `LibraryContentCommandHandlerTests.Handle_PublishMissingRequiredContent_ReturnsInvalidContentAndKeepsDraft`, `LibraryConstraintTests.Handle_VersionOutsideCheck_RejectedByDatabase` |
| ST-LIB-003 | `LibraryValidatorTests.Handle_DimensionOutOfDomain_ReturnsInvalidContent`, `LibraryContentCommandHandlerTests.Handle_SaveDraftWithValidDimensions_StoresExactValues` |
| ST-LIB-004 | Chỉ phần backend: `LibraryContentCommandHandlerTests.Handle_AddAssetWithAllowedUrl_StoresRequestValues`, `Handle_AddAssetWithUnsupportedUrlOrKind_ReturnsUnsupportedFile`. Định dạng và dung lượng do frontend kiểm, cần chạy qua giao diện. |
| ST-LIB-005 | `LibraryContentCommandHandlerTests.Handle_PublishWithClassification_AcceptsOnlyValidCombination`, `LibraryConstraintTests.Handle_ClassificationAgainstPinnedRevision_EnforcedByForeignKeys` |
| ST-LIB-006 | `LibraryFlowTests.Handle_EditCurrentInPlace_ReplacesCoverAndKeepsVersion` (phần xem lại từ lịch sử thuộc TDD-LIB-002) |
| ST-LIB-007 | `LibraryFlowTests.Handle_CreateDraftFromCurrentWithCover_CopiesLinksAndCover`, `LibraryFlowTests.Handle_DeleteOrEditSupersededVersion_ReturnsReadOnly`, `LibraryConcurrencyTests.Handle_TwoDraftsPublishedAtSameTime_OnlyOneBecomesCurrent` |
| ST-LIB-008 | `LibraryVersionCommandHandlerTests.Handle_HideThenShow_ChangesOnlyVisibility`, `LibraryReadTests.Handle_PublicListPaging_ExcludesHiddenAndDrafts` (phần mở mới và xem lại thuộc TDD-LIB-002) |
| ST-LIB-009 | `LibraryFlowTests.Handle_DeleteDraftWithCover_RemovesDraftKeepsAssetsAndReplays`, `LibraryFlowTests.Handle_DeleteOrEditSupersededVersion_ReturnsReadOnly` |
| ST-LIB-010 | `LibraryFlowTests.Handle_CatalogChanged_KeepsPinnedOnRenameAndRejectsStaleClassification`, `LibraryContentCommandHandlerTests.Handle_PublishDraftAfterCatalogChange_RequiresCurrentOptionsAndPinsRevision` |
| ST-LIB-011 | `LibraryApiAuthorizationTests` (401 khi chưa đăng nhập, 403 khi thiếu `library.manage` kể cả mang vai trò tên Admin, đạt khi chỉ có `library.manage`) |
| ST-LIB-012 | `LibraryReadTests.Handle_PublicListPaging_ExcludesHiddenAndDrafts`, `LibraryApiAuthorizationTests.Handle_PublicRouteAnonymous_Returns200` |
| ST-LIB-013 | `LibraryReadTests.Handle_PublicListTumFilter_DistinguishesNotApplicable` |
| ST-LIB-014 | `LibraryReadTests.Handle_Filters_UnionCurrentCatalogWithPublicLegacyFloors` |
| ST-LIB-015 | `LibraryReadTests.Handle_PublicList_OrdersByLatestPublishAndIgnoresInPlaceEdits`, `LibraryReadTests.Handle_PublicListNameSearch_TreatsWildcardsLiterally` |
| ST-LIB-016 | `LibraryReadTests.Handle_PublicListNameSearch_TreatsWildcardsLiterally` |
| ST-LIB-031 | `LibraryReadTests.Handle_AdminVersionAssets_PagesByPositionWithOriginalUrl`, `LibraryApiAuthorizationTests` (401, 403, đạt với `library.manage` ở route `.../versions/{versionId}/assets`), commit `9e02c4d` |

**Cập nhật 26/09/2026 (triển khai TDD-LIB-002):** backend của các ca ST-LIB-017 đến ST-LIB-030 đã có ở nhánh `feature/library-access` của `bmt-be` (commit `0263297`, chưa merge), gồm cả phần xem lại từ lịch sử của ST-LIB-006 và phần mở mới, xem lại của ST-LIB-008. Các ca System Test vẫn chưa chạy trên môi trường thử; bảng dưới chỉ ghi test tự động phía backend đang kiểm cùng hành vi. Tranh chấp dùng hai kết nối PostgreSQL thật; kho presign là bản giả không gọi mạng, nên phần HEAD/Range của kho thật và mất phản hồi mạng thật (ST-LIB-022) vẫn cần kiểm trên môi trường thử.

| System Test | Test tự động phía backend (test class của `bmt-be`) |
| --- | --- |
| ST-LIB-017 | `LibraryAccessFlowTests.Handle_FirstOpenThenReopen_ChargesExactlyOnce` |
| ST-LIB-018 | `LibraryAccessFlowTests.Handle_HistoryAfterNewVersion_TwoRowsNewestFirstWithCurrentContent` |
| ST-LIB-019 | `LibraryAccessFlowTests.Handle_ReopenAfterPlanLapsed_FreeButNewVersionDenied` (gói hết hạn, bị hủy, hết lượt) |
| ST-LIB-020 | `LibraryAccessFlowTests.Handle_NotConfirmed_ReturnsConfirmationRequiredWithoutProbing` |
| ST-LIB-021 | `LibraryAccessFlowTests.Handle_StorageFailsBeforeCommit_NoChargeThenSucceedsLater` |
| ST-LIB-022 | Chỉ phần backend: `LibraryAccessFlowTests.Handle_FirstOpenThenReopen_ChargesExactlyOnce` (mở lại bằng yêu cầu mới không tính thêm), `LibraryAccessConcurrencyTests.Handle_SameVersionOpenedConcurrently_ChargedOnce` |
| ST-LIB-023 | `LibraryAccessConcurrencyTests.Handle_SameVersionOpenedConcurrently_ChargedOnce`, `LibraryAccessConcurrencyTests.Handle_TwoVersionsCompeteForLastUnit_OnlyOneCharged` |
| ST-LIB-024 | `LibraryAccessFlowTests.Handle_DownloadContent_StreamsThroughBackendWithRangeAndChecks`, `LibraryAccessApiTests.Handle_Content_StreamsBytesWithProtectiveHeaders`, `LibraryAccessApiTests.Handle_ContentWithRange_Returns206AndForwardsRange` |
| ST-LIB-025 | `LibraryAccessApiTests.Handle_RouteWithoutSession_Returns401`, `LibraryViewerTests.Handle_OtherAccount_CannotReadVersionOrSeeHistory`, `LibraryAccessFlowTests.Handle_DownloadContent_StreamsThroughBackendWithRangeAndChecks` (tài khoản chưa mở bị 403) |
| ST-LIB-026 | `LibraryAccessFlowTests.Handle_ReopenAfterPlanLapsed_FreeButNewVersionDenied` (biến thể không giới hạn rồi hết hạn) |
| ST-LIB-027 | `LibraryAccessApiTests.Handle_RouteWithoutSession_Returns401`, `LibraryAccessFlowTests.Handle_PlanWithoutCatalogDetail_ReturnsEntitlementMissingAndKeepsGenerateQuota`, `LibraryAccessFlowTests.Handle_QuotaExhausted_ReturnsQuotaUnavailableWithoutWrites`, `LibraryOpenTests.Handle_PlanConditionMissing_ReturnsSubscriptionCodeNotQuotaUnavailable` |
| ST-LIB-028 | `LibraryAccessConcurrencyTests.Handle_PlanChangedWhileOpenWaits_ChecksCurrentPeriodAfterLock` |
| ST-LIB-029 | `LibraryAccessFlowTests.Handle_DownloadContent_StreamsThroughBackendWithRangeAndChecks` (tệp đã gỡ không tải được), `LibraryAccessFlowTests.Handle_HistoryAfterNewVersion_TwoRowsNewestFirstWithCurrentContent` (sửa tại chỗ không tính lượt) |
| ST-LIB-030 | `LibraryAccessFlowTests.Handle_HiddenTemplate_NewOpenDeniedButExistingAccessServed`, `LibraryAccessConcurrencyTests.Handle_AdminMutationDuringOpen_OpenWaitsThenRejectsWithoutCharge` (ẩn mẫu chen vào lúc mở) |

Các ca 004 và phần truyền tải chưa có ngưỡng hạ tầng để nghiệm thu tải lớn; không thể chứng minh “không giới hạn” bằng một bộ dữ liệu hữu hạn. API, fixture, điểm gây lỗi và cách điều phối đồng thời phải được cụ thể hóa trong TDD trước khi chạy. Trong bước thiết kế TDD, người dùng đã xác nhận cho lưu nháp thiếu dữ liệu và kiểm đủ khi công bố; cần bổ sung đặc tả riêng cho việc lưu nháp khi cập nhật bộ kiểm thử.

Đã cập nhật ST-SUB-050–052 theo lượt từng phiên bản. TDD-SUB-002 đã được cập nhật phần tra cứu cùng TDD-LIB-002 để chốt; quản trị nội dung và danh sách theo TDD-LIB-001; các UT về tra cứu cần cập nhật sau khi chốt TDD; chưa dùng thiết kế cũ để triển khai quyền xem lại.
