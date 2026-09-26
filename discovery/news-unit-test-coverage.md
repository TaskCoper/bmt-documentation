# Đặc tả Unit Test Tin tức

Người dùng đã chốt TDD-NEWS-001 và TDD-NEWS-002, gồm giới hạn tên danh mục 200 ký tự Unicode; số cấp không giới hạn nghiệp vụ. Từ 25/09/2026, giới hạn tên còn là quy tắc nghiệp vụ tại BR-NEWS-002 khoản 1, nên UT-NEWS-025 dẫn thêm tới BR-NEWS-002 và STORY-NEWS-002/AC-008. UT-NEWS-022 kiểm thêm vai trò tùy chỉnh có `news.manage`; UT-NEWS-040 mới kiểm lọc theo danh mục không tồn tại.

**Cập nhật 26/09/2026 (ảnh):** áp dụng quyết định đã xác nhận cùng ngày, backend không có endpoint presign/complete nên hai đặc tả cổng presign cũ bị thay: UT-NEWS-019 nay kiểm quy tắc "chỉ kiểm tên miền với URL ảnh mới", UT-NEWS-020 kiểm `img` mất `src` sau khi làm sạch. UT-NEWS-015, UT-NEWS-018 viết lại theo kiểm tên miền bằng `UploadedFileOption__AllowedHosts`; UT-NEWS-013, UT-NEWS-039 sửa câu chữ liên quan.

**Cập nhật 26/09/2026 (quyết định lần 2):** người dùng xác nhận giới hạn tiêu đề 200, mô tả ngắn 500, nội dung sau làm sạch 200.000 ký tự (BR-NEWS-001 khoản 9) và cho phép `target="_blank"` kèm `rel="noopener noreferrer"`. Thêm UT-NEWS-041 đến UT-NEWS-043.

**Cập nhật 26/09/2026 (triển khai):** mã test của cả 43 đặc tả đã viết ở nhánh `feature/news` của `bmt-be` (commit `4714e68`, `bce2eb2`, `e390e2d`); bảng "Mã test" bên dưới ánh xạ từng đặc tả. Đặc tả vẫn ở trạng thái Draft; kết quả chạy không ghi vào đặc tả.

## Danh sách đặc tả

| Đặc tả | Phạm vi |
| --- | --- |
| [UT-NEWS-001](../unittest/UT-NEWS-001.md) | NewsArticleService — lưu nháp |
| [UT-NEWS-002](../unittest/UT-NEWS-002.md) | NewsArticleService — kiểm đủ dữ liệu công bố |
| [UT-NEWS-003](../unittest/UT-NEWS-003.md) | NewsArticleService — công bố lần đầu |
| [UT-NEWS-004](../unittest/UT-NEWS-004.md) | NewsArticleService — công bố lại |
| [UT-NEWS-005](../unittest/UT-NEWS-005.md) | NewsArticleService — sửa Published |
| [UT-NEWS-006](../unittest/UT-NEWS-006.md) | NewsArticleService — chặn sửa Published thiếu trường |
| [UT-NEWS-007](../unittest/UT-NEWS-007.md) | NewsArticleService — danh mục đã mất |
| [UT-NEWS-008](../unittest/UT-NEWS-008.md) | NewsArticleService — xung đột phiên bản |
| [UT-NEWS-009](../unittest/UT-NEWS-009.md) | NewsArticleService — ẩn bài |
| [UT-NEWS-010](../unittest/UT-NEWS-010.md) | NewsArticleService — publish bài đang Published |
| [UT-NEWS-011](../unittest/UT-NEWS-011.md) | NewsArticleService — xóa mọi trạng thái |
| [UT-NEWS-012](../unittest/UT-NEWS-012.md) | NewsArticleService — bài không tồn tại |
| [UT-NEWS-013](../unittest/UT-NEWS-013.md) | INewsHtmlSanitizer — định dạng hợp lệ |
| [UT-NEWS-014](../unittest/UT-NEWS-014.md) | INewsHtmlSanitizer — chặn nội dung nguy hiểm |
| [UT-NEWS-015](../unittest/UT-NEWS-015.md) | Lưu bài — URL ảnh bìa và img đúng tên miền kho presign |
| [UT-NEWS-016](../unittest/UT-NEWS-016.md) | NewsArticleService — nội dung có nghĩa sau làm sạch |
| [UT-NEWS-017](../unittest/UT-NEWS-017.md) | INewsHtmlSanitizer — tính ổn định khi làm sạch lại |
| [UT-NEWS-018](../unittest/UT-NEWS-018.md) | Lưu bài — URL ảnh mới ngoài tên miền hoặc kho chưa cấu hình |
| [UT-NEWS-019](../unittest/UT-NEWS-019.md) | Lưu bài — URL ảnh đã lưu không bị kiểm lại tên miền |
| [UT-NEWS-020](../unittest/UT-NEWS-020.md) | Lưu bài — img mất src sau khi làm sạch HTML |
| [UT-NEWS-021](../unittest/UT-NEWS-021.md) | ArticleWrite validator — snapshot và trường server |
| [UT-NEWS-022](../unittest/UT-NEWS-022.md) | Policy news.manage — dùng chung quyền |
| [UT-NEWS-023](../unittest/UT-NEWS-023.md) | NewsCategoryNamePolicy — chuẩn hóa giữ dấu |
| [UT-NEWS-024](../unittest/UT-NEWS-024.md) | NewsCategoryNamePolicy — tên rỗng |
| [UT-NEWS-025](../unittest/UT-NEWS-025.md) | NewsCategoryNamePolicy — giới hạn Unicode 200 |
| [UT-NEWS-026](../unittest/UT-NEWS-026.md) | NewsCategoryService — kiểm tên cùng cha |
| [UT-NEWS-027](../unittest/UT-NEWS-027.md) | NewsCategoryService — chuyển vào chính node/hậu duệ |
| [UT-NEWS-028](../unittest/UT-NEWS-028.md) | NewsCategoryService — chuyển hợp lệ và về gốc |
| [UT-NEWS-029](../unittest/UT-NEWS-029.md) | NewsCategoryService — trùng tại cha đích |
| [UT-NEWS-030](../unittest/UT-NEWS-030.md) | NewsCategoryService — node/cha không tồn tại |
| [UT-NEWS-031](../unittest/UT-NEWS-031.md) | NewsCategoryService — xóa danh mục đang dùng |
| [UT-NEWS-032](../unittest/UT-NEWS-032.md) | NewsCategoryService — đổi vị trí |
| [UT-NEWS-033](../unittest/UT-NEWS-033.md) | NewsCategoryService — anchor hoặc version cũ |
| [UT-NEWS-034](../unittest/UT-NEWS-034.md) | Position validator — anchor không hợp lệ |
| [UT-NEWS-035](../unittest/UT-NEWS-035.md) | NewsCategoryService — thêm cấp sâu |
| [UT-NEWS-036](../unittest/UT-NEWS-036.md) | Chuẩn hóa tham số phân trang Tin tức |
| [UT-NEWS-037](../unittest/UT-NEWS-037.md) | Chuẩn bị từ khóa ILIKE |
| [UT-NEWS-038](../unittest/UT-NEWS-038.md) | Handler đọc public — điều kiện trạng thái |
| [UT-NEWS-039](../unittest/UT-NEWS-039.md) | Error mapping Tin tức — mã lỗi rõ ràng |
| [UT-NEWS-040](../unittest/UT-NEWS-040.md) | Truy vấn danh sách công khai — categoryId không tồn tại trả rỗng |
| [UT-NEWS-041](../unittest/UT-NEWS-041.md) | Validator lưu bài — độ dài tiêu đề và mô tả ngắn |
| [UT-NEWS-042](../unittest/UT-NEWS-042.md) | Lưu bài — độ dài nội dung sau khi làm sạch |
| [UT-NEWS-043](../unittest/UT-NEWS-043.md) | INewsHtmlSanitizer — liên kết mở tab mới |

## Mã test

Tên test theo quy ước `Handle_{Condition}_{ExpectedOutcome}`. Đường dẫn tính từ gốc `bmt-be/test/`.

| Đặc tả | Mã test |
| --- | --- |
| UT-NEWS-001 | `bmt-be.application.tests/usecases/news/NewsArticleCommandHandlerTests.Handle_CreateWithTitleOnly_SavesDraftWithNullFields` |
| UT-NEWS-002 | `NewsArticleCommandHandlerTests.Handle_PublishMissingRequiredField_ReturnsInvalidNewsContent` |
| UT-NEWS-003 | `NewsArticleCommandHandlerTests.Handle_PublishCompleteDraft_SetsFirstPublishedAndIncrementsVersion` |
| UT-NEWS-004 | `NewsArticleCommandHandlerTests.Handle_PublishHiddenArticle_KeepsFirstPublishedDate` |
| UT-NEWS-005 | `NewsArticleCommandHandlerTests.Handle_UpdatePublishedWithValidSnapshot_UpdatesInPlaceKeepsDate`, `Handle_UpdateWithSameContent_KeepsVersion` |
| UT-NEWS-006 | `NewsArticleCommandHandlerTests.Handle_UpdatePublishedMissingRequiredField_RejectsAndKeepsSnapshot` |
| UT-NEWS-007 | `NewsArticleCommandHandlerTests.Handle_UpdateWithDeletedCategory_ReturnsInvalidNewsContent` |
| UT-NEWS-008 | `NewsArticleCommandHandlerTests.Handle_StaleExpectedVersion_ReturnsVersionConflict`; đồng thời: `bmt-be.integration.tests/NewsConcurrencyTests.Handle_TwoEditsFromSameVersion_SecondGetsVersionConflict` |
| UT-NEWS-009 | `NewsArticleCommandHandlerTests.Handle_HidePublished_BecomesHiddenKeepsDate`, `Handle_HideAlreadyHidden_IsNoOp`, `Handle_HideDraft_ReturnsStateConflict` |
| UT-NEWS-010 | `NewsArticleCommandHandlerTests.Handle_PublishAlreadyPublished_IsNoOp` |
| UT-NEWS-011 | `NewsArticleCommandHandlerTests.Handle_DeleteInAnyState_RemovesArticle`; cascade: `NewsConstraintTests.Article_Delete_CascadesLinksKeepsCategories` |
| UT-NEWS-012 | `NewsArticleCommandHandlerTests.Handle_ArticleNotFound_ReturnsNotFound` |
| UT-NEWS-013 | `bmt-be.infrastructure.tests/news/NewsHtmlSanitizerTests.Handle_AllowedFormatting_IsKept` |
| UT-NEWS-014 | `NewsHtmlSanitizerTests.Handle_DangerousMarkup_IsRemoved`, `Handle_ScriptContent_DroppedWithTag`, `Handle_ImageWithUnsafeSource_ReportedAsWithoutSource` |
| UT-NEWS-015 | `bmt-be.application.tests/usecases/news/NewsArticleImageUrlTests.Handle_ImageUrlHostOrScheme_AcceptedOnlyForConfiguredHttpsHost` |
| UT-NEWS-016 | `NewsArticleImageUrlTests.Handle_ContentMeaning_DecidesPublishability`, `NewsHtmlSanitizerTests.Handle_MeaningfulContent_DecidedAfterSanitizing` |
| UT-NEWS-017 | `NewsHtmlSanitizerTests.Handle_SanitizeTwice_IsStable` |
| UT-NEWS-018 | `NewsArticleImageUrlTests.Handle_NewImageHostNotAllowed_RejectsWithoutChangingArticle`, `Handle_NewImageWhileHostsNotConfigured_ReturnsStorageUnavailable` |
| UT-NEWS-019 | `NewsArticleImageUrlTests.Handle_StoredImageUrlKept_NotRecheckedWhileNewUrlChecked` |
| UT-NEWS-020 | `NewsArticleImageUrlTests.Handle_ContentImageWithoutUsableSource_Rejected` |
| UT-NEWS-021 | `bmt-be.application.tests/usecases/news/NewsValidatorTests.Handle_UpdateWithoutCategoryIdsOrBadVersion_FailsValidation`, `Handle_CreateCoverUrlShape_ValidatedBeforeHandler`, `Handle_StateCommandsWithoutPositiveVersion_FailValidation` |
| UT-NEWS-022 | `bmt-be.api.tests/news/NewsApiAuthorizationTests` (401, 403, đạt với `news.manage` qua route thật) |
| UT-NEWS-023 | `NewsValidatorTests.Handle_CategoryNameNormalization_KeepsDiacritics` |
| UT-NEWS-024 | `NewsValidatorTests.Handle_BlankCategoryName_FailsValidation` |
| UT-NEWS-025 | `NewsValidatorTests.Handle_CategoryNameLengthBoundary_Enforced`, `Handle_CategoryNameOutsideBmpOrCombining_CountedAsScalarsAfterNfc` |
| UT-NEWS-026 | `bmt-be.application.tests/usecases/newsCategory/NewsCategoryCommandHandlerTests.Handle_CreateDuplicateNameUnderSameParent_ReturnsNameConflict`, `Handle_RenameSelfCaseOnly_Succeeds`; unique index: `NewsConstraintTests` |
| UT-NEWS-027 | `NewsCategoryCommandHandlerTests.Handle_MoveIntoSelfOrDescendant_ReturnsCycle`; đồng thời: `NewsConcurrencyTests.Handle_CrossMovesAtSameTime_OnlyOneSucceedsAndNoCycle` |
| UT-NEWS-028 | `NewsCategoryCommandHandlerTests.Handle_MoveToOtherParent_KeepsIdAndAppends`, `Handle_MoveToRoot_BecomesLastRoot` |
| UT-NEWS-029 | `NewsCategoryCommandHandlerTests.Handle_MoveWhereTargetHasSameName_ReturnsNameConflict` |
| UT-NEWS-030 | `NewsCategoryCommandHandlerTests.Handle_CategoryOrParentMissing_ReturnsNotFound` |
| UT-NEWS-031 | `NewsCategoryCommandHandlerTests.Handle_DeleteCategoryInUse_ReturnsInUse`, `Handle_DeleteUnusedCategory_RemovesOnlyThatCategory`; FK: `NewsConstraintTests.Category_DeleteWhileInUse_IsBlockedByForeignKey` |
| UT-NEWS-032 | `NewsCategoryCommandHandlerTests.Handle_MoveBeforeAnchor_ReordersSiblings`, `Handle_MoveToEnd_ReordersAndOnlyBumpsChangedRows`; PostgreSQL: `NewsConcurrencyTests.Handle_Reorder_PersistsNewOrderAndVersions` |
| UT-NEWS-033 | `NewsCategoryCommandHandlerTests.Handle_MoveWithStaleNodeOrAnchor_ReturnsVersionConflict`, `Handle_UpdateOrDeleteWithStaleVersion_ReturnsVersionConflict` |
| UT-NEWS-034 | `NewsValidatorTests.Handle_PositionWithInvalidAnchor_FailsValidation` |
| UT-NEWS-035 | `NewsCategoryCommandHandlerTests.Handle_CreateUnderDeepChain_NotLimitedByDepth`; CTE 13 cấp: `NewsReadTests.Handle_DeepTree_FilterReachesDeepestLevel` |
| UT-NEWS-036 | `NewsValidatorTests.Handle_PagingParameters_Normalized`, `Handle_PageOffsetBeyondInt32_FailsValidation`, `NewsQueryHandlerTests.Handle_PublicListPaging_NormalizedBeforeQuery` |
| UT-NEWS-037 | `NewsValidatorTests.Handle_KeywordPattern_EscapesWildcards`; SQL: `NewsReadTests.Handle_KeywordAndCategory_MatchTitleOnly` |
| UT-NEWS-038 | `bmt-be.application.tests/usecases/news/NewsQueryHandlerTests.Handle_PublicDetailNotPublished_ReturnsNotFound`, `Handle_PublicDetailPublished_MapsPublicFieldsOnly`; SQL: `NewsReadTests.Handle_OnlyPublishedArticlesAreVisible` |
| UT-NEWS-039 | `bmt-be.application.tests/behaviors/NewsConstraintMappingTests`, `bmt-be.api.tests/middlewares/NewsErrorResponseTests` |
| UT-NEWS-040 | `NewsQueryHandlerTests.Handle_PublicListUnknownCategory_ReturnsEmptyPage`; SQL: `NewsReadTests.Handle_UnknownOrDeletedCategory_ReturnsEmptyList` |
| UT-NEWS-041 | `NewsValidatorTests.Handle_TitleSummaryLength_EnforcedAfterTrim`; cột varchar: `NewsConstraintTests.Article_TextLength_LimitedByDatabase` |
| UT-NEWS-042 | `NewsArticleImageUrlTests.Handle_SanitizedContentLength_Limited`; CHECK: `NewsConstraintTests.Article_TextLength_LimitedByDatabase` |
| UT-NEWS-043 | `NewsHtmlSanitizerTests.Handle_LinkTarget_OnlyBlankWithSafeRel`, `Handle_LinkTargetBlank_StableWhenSanitizedAgain` |

Integration test trên PostgreSQL 15 thật nằm ở `bmt-be.integration.tests/NewsConstraintTests.cs`, `NewsReadTests.cs` và `NewsConcurrencyTests.cs`; chúng phủ các dòng "ràng buộc", "khóa", "CTE" của bảng dưới.

## Phần phải kiểm chứng ngoài unit test

| Phần | Cách kiểm chứng và căn cứ |
| --- | --- |
| UNIQUE tên cùng cha, NULL gốc, FK và cascade | PostgreSQL 15 thật; ST-NEWS-013, ST-NEWS-016–018 và TDD-NEWS-002/Data Model. Mock không chứng minh constraint. |
| Khóa cây và bài, rollback, cạnh tranh chuyển nhánh | Hai connection thật, kiểm cả kết quả và dữ liệu cuối; thử xóa Category đồng thời gắn Article, A→B đồng thời B→A, ghi links thất bại. Theo hai TDD/Architecture. |
| CTE toàn nhánh, EXISTS loại trùng, thứ tự và snapshot phân trang | Integration SQL và ST-NEWS-021–025, ST-NEWS-029; không dùng LINQ-to-objects thay SQL để tuyên bố đạt. |
| Quyền và CSRF ở route thực, session, AllowAnonymous | API integration và ST-NEWS-011, ST-NEWS-019–020, ST-NEWS-026; policy unit không chứng minh middleware được gắn. CSRF dùng lớp chung ở [TDD-AUTH-001](../tdd/TDD-AUTH-001.md), đã có test qua pipeline HTTP cho lớp này. |
| Upload ảnh qua dịch vụ presign ngoài backend | Giữa frontend và dịch vụ presign, backend không tham gia; kiểm ở ST-NEWS-004–005 trên môi trường thử có cấu hình `UploadedFileOption__AllowedHosts` trùng tên miền kho. |
| Rich text không thực thi mã và round-trip editor | Parser unit kiểm DOM; trình duyệt thật kiểm ST-NEWS-027, cả trang quản trị và khách. |
| Xóa/ẩn phản ánh trên trang khách và không trừ lượt | ST-NEWS-009–010, ST-NEWS-020, ST-NEWS-026; kiểm response mới, cache và dữ liệu gói trước/sau. |

## Căn cứ và phần còn mở

- [TDD bài viết](../tdd/TDD-NEWS-001.md), [TDD danh mục](../tdd/TDD-NEWS-002.md) và [30 System Test](news-system-test-coverage.md) là nguồn thiết kế/nghiệp vụ đã chốt.
- Trace to và TEST_LINKS của từng UT dẫn cùng tập mã/section; phần không kiểm được bằng unit đã nêu rõ, không tính là kết quả Pass.
- Ảnh do frontend tự upload qua dịch vụ presign ngoài backend; backend chỉ kiểm URL https thuộc tên miền kho (quyết định ngày 26/09/2026). Bộ làm sạch là thư viện HtmlSanitizer 9.2.1039 trên AngleSharp, đã kiểm allowlist bằng UT-NEWS-013, UT-NEWS-014, UT-NEWS-017.
- Owner kiểm thử chưa phân công; Reviewer/Approver là Tân Trần theo hội thoại. Không tự cập nhật trạng thái phê duyệt hoặc tạo lịch sử phát hành.
