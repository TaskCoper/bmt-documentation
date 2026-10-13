# Triển khai Admin AI — 13/10/2026

Đã triển khai ba màn Luật giá, Quy tắc AI và Prompt trên worktree W4, theo [TDD-PROJ-012](../tdd/TDD-PROJ-012.md) được người dùng chốt. Chủ đề theo miền chọn rõ chặng Plan2d, Estimate hoặc Render3d.

## Phạm vi đã triển khai

| Màn | Trước → sau |
| --- | --- |
| Luật giá | API đã có, chưa có editor → sửa hai tài liệu, xem trước dải giá từ backend, lịch sử phân trang, so sánh thêm/xóa và khôi phục thành phiên bản mới. Đổi tab giữ bản đang nhập. |
| Quy tắc AI | Chỉ đọc → sửa năm nhóm bằng form; giữ sáu nhóm trong mỗi snapshot, gồm prompt; chống ghi đè phiên bản cũ và chống gửi lặp; lịch sử và khôi phục toàn bộ. |
| Prompt | Chỉ đọc → sửa slot hiện có, kiểm biến định dạng, xem prompt/schema đã ghép, chọn mẫu/tình huống, chạy thử, theo dõi kết quả và chi phí, mở lại bằng trialId, lịch sử và khôi phục một prompt. |
| Vận hành | Giữ endpoint hiện có; bổ sung kiểm chứng quyền đặt lại bộ đếm, quyền xem log thô và vết đọc log trên PostgreSQL. |

Các quyền vẫn tách biệt: `estimate.pricing.manage`, `estimate.ai-rules.manage`, `estimate.pipeline.config.manage`, `estimate.pipeline.inspect`, `estimate.pipeline.operate`. Tên vai trò Admin không thay thế quyền endpoint.

Rules/Prompt dùng cùng `AiRuleSetRevision`; không sửa revision đã tồn tại. Lời dặn theo miền được lọc theo chặng và miền, kể cả lượt hỏi lại của Dự toán, ảnh 2D và kiểm ảnh 3D. `Render3dWeather` vẫn đi vào khối khí hậu riêng của 3D.

## Chạy thử Prompt

- `AiPromptTrial` là job riêng; không tạo dự án, giữ lượt thiết kế, tăng bộ đếm lỗi hoặc ghi artifact khách.
- Xem trước gọi bộ dựng prompt thật với dữ liệu mẫu. Hash bao gồm bản nhập, mẫu, request đã ghép và các revision nguồn; đổi nguồn phải xem trước lại.
- Bộ mẫu `townhouse-standard-central`, phiên bản `1`, đóng gói từ các tài nguyên thử sẵn có trong `bmt-be.estimatepipeline.fakes/Defaults`. Phương án mang địa chỉ mẫu “Công trình mẫu thử Prompt”. Không sao chép dự án khách local vào bộ mẫu.
- Request lưu nguyên văn prompt/schema và metadata ảnh có hash. Worker đọc lại đúng bytes đã đóng gói, dùng client AI, limiter, breaker và cách ghi chi phí hiện có.
- JSON/tool được kiểm theo schema tác vụ. Ảnh kiểm định dạng/kích thước, viền 2D, QC và chống trùng góc 3D theo điều kiện của tác vụ. Nội dung không đạt vẫn được lưu khi nhà cung cấp đã trả về.
- Hàng chờ dùng chung `MaxQueuedRuns` với job chặng và chung khóa tiếp nhận PostgreSQL. Claim, heartbeat và ghi kết quả đều kiểm lease/hạn cuối.
- Job Running mất lease kết thúc `PromptTrialInterrupted`, không tự gọi AI lại. Lỗi lưu tệp cũng không làm gọi lại AI. Kết quả muộn bị từ chối.
- Thiếu log hoặc thiếu giá thì chi phí Live là NULL kèm cảnh báo; Fake là 0. “Đạt cấu trúc” không phải phê duyệt chất lượng thiết kế.
- Tệp thử nằm dưới `media/site-private/estimate-ai-trials/{trialId}/`. Log thô vẫn cần quyền operate và ghi `AiCallLogAccess`.

## Kiểm chứng đã chạy

| Nhóm | Kết quả | Bằng chứng |
| --- | --- | --- |
| Ứng dụng: Rules, luật giá, cấu hình và audit | 93 đạt | `AiRuleAdminTests`, `PriceRuleAdminTests`, `PipelineAdminTests`; `/private/tmp/bmt-admin-tests.log` |
| Phân quyền qua HTTP | 45 đạt | 44 test `AiRuleAdminAuthorizationTests` và một test bổ sung reset; `/private/tmp/bmt-admin-http-tests.log`, `bmt-admin-reset-http-tests.log` |
| PostgreSQL thật | 12 đạt, không skip | `AiRuleAdministrationFlowTests`, `AiPromptTrialFlowTests`; `/private/tmp/bmt-admin-pg-tests.log` |
| Thực thi trial và ghi log | 12 đạt | `PromptTrialExecutorTests`, `AiCallRecorderTests`; `/private/tmp/bmt-admin-executor-tests.log` |
| Đọc bảng giá có thụt đầu dòng | 3 đạt; giữ đúng 41 dải demo, vẫn chặn dòng bắt buộc bị xóa | `PriceRuleCalculatorTests`; `/private/tmp/bmt-admin-price-indent-tests.log` |
| Model EF | 8 đạt | `AccessModelTests`; `/private/tmp/bmt-admin-model-tests.log` |
| Frontend: quyền route và so sánh | 33 đạt | `scripts/route-access.test.mjs`, `scripts/ai-rule-diff.test.mjs`; `/private/tmp/bmt-admin-fe-tests.log` |
| Build | API publish Release, frontend production Webpack đạt; TypeScript và lint đạt | Các log `bmt-admin-publish`, `bmt-admin-fe-build`, `bmt-admin-typecheck`, `bmt-admin-lint` trong `/private/tmp` |
| Browser, kết quả văn bản | Fake thành công qua nút Chạy thử, hỏi trạng thái tới khi đạt | Trial `f9d13cdc-e435-47fb-af73-7b1514c222a7`; `/private/tmp/bmt-admin-browser-trial-result.json` |
| Browser, luật giá | Xem trước đủ 41 dải giá trên nội dung local; giữ riêng bản nháp khi chuyển qua lại hai tab | `/private/tmp/bmt-admin-browser-price-result.json` |
| Browser, lưu/khôi phục Rules | Thêm chủ đề cho Plan2d/Miền Trung: phiên bản 3 → 4 → 5; nội dung khôi phục khớp hoàn toàn; không lỗi JavaScript | `/private/tmp/bmt-admin-browser-rules-result.json` |
| Browser, kết quả ảnh | Fake thành công; ảnh 1280×720 tải được từ kho riêng tư | Trial `e2a20a7e-282d-4dcb-b3de-587c864067b3`; `/private/tmp/bmt-admin-browser-render-result.json` |

PostgreSQL kiểm lưu/sao chép/khôi phục snapshot, replay/conflict, mọi tình huống xem trước trong registry, nhận việc một lần, mất lease, quá hạn, từ chối kết quả muộn, ràng buộc metadata, hai yêu cầu đồng thời không vượt hàng chờ, audit đọc log và reset bộ đếm không đổi kết quả/lượt sử dụng.

Kiểm migration gồm Up/Down/Up trên database tạm và chạy script SQL do EF sinh ra. Lượt áp script local đầu tiên gặp thiếu dấu chấm phẩy trong câu backfill; transaction đã rollback. Đã sửa nguồn migration, thêm kiểm script SQL trên PostgreSQL rồi áp lại thành công.

Kiểm browser phát hiện tài liệu luật giá đã nạp từ trước có khoảng trắng đầu dòng do định dạng SQL seed, khiến xem trước báo thiếu bảng. Đã sửa `LibraryPriceRuleCalculator` bỏ khoảng trắng trước tiêu đề/bảng khi tính; không sửa nội dung lưu hoặc prompt. Ba regression test kiểm khoảng trắng/tab, xuống dòng LF/CRLF và dòng bị thiếu đều đạt.

Không chạy AI Live trong đợt kiểm Admin. Hai lần thử browser dùng Fake. Các kiểm tra schema và Fake không thay cho đánh giá chất lượng model thật. Cảnh báo ImageSharp NU1902/NU1903 và các cảnh báo nullable có sẵn vẫn còn; đợt này không nâng dependency ngoài phạm vi.

## Local và dữ liệu

- Đã trả API về `EstimatePipelineOption__Mode=Live` sau hai lượt thử Fake. Không còn job thử đang chạy.
- Frontend: `http://localhost:3001`; API: `http://localhost:8080`. Cổng 3000 của phiên khác giữ nguyên.
- Đã áp `20261013014453_AiRuleAdministration` và `20261013015529_AiPromptTrials` vào `bmt_local_w1`.
- Backup trước migration: `/private/tmp/bmt-before-admin-20261013.dump`.
- Image trước Admin: `localhost/bmt/bmt-api:local-w4-before-admin-20261013`. Image hiện hành: `localhost/bmt/bmt-api:local-w4`.
- Đối chiếu hash trước/sau migration: 8 dự án, 1 kết quả generation cũ, 3 kết quả 2D, 3 kết quả Dự toán, 17 tệp kết quả, 24 artifact, 8 lần chạy và 234 dòng prompt cũ đều giữ nguyên. Bằng chứng: `bmt-admin-before.txt` và `bmt-admin-after.txt` trong `/private/tmp`.
- Sau kiểm browser, đối chiếu lại 7 nhóm dữ liệu dự án/kết quả: hash giữ nguyên. Lịch sử Rules có thêm phiên bản 4/5 để ghi việc thử và khôi phục; bản hiện hành có nội dung như phiên bản 3.
- Down từ chối khi đã có dữ liệu Admin/trial mới; không xóa lịch sử để ép hạ schema.
- Tài khoản test giữ tại `bmt-wt/local-test/local-accounts.txt`; không chép mật khẩu vào tài liệu.
- Không push/merge hoặc áp migration production.

Đặc tả UT-PROJ-510–548 vẫn là Draft, không tự đổi thành Approved hay coi từng đặc tả đã chạy chỉ vì có file. Đã kiểm đủ 39 file, mỗi bảng có 13 cột và các mã tài liệu tham chiếu tồn tại. Kết quả thực thi là các suite nêu ở bảng trên; chưa import hoặc phê duyệt trên Document First.
