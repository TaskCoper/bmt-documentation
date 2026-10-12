# Kế hoạch: chuyển luồng Tạo dự toán của app demo vào bmt-be (.NET)

> **Trạng thái (12/10/2026): tài liệu lịch sử.** Đây là bản kế hoạch ban đầu, lưu lại để biết lý do của các quyết định. Các quyết định đã được đưa vào STORY-PROJ-001…013, BR-PROJ-001…019 và TDD-PROJ-006/007/008. Khi file này khác các tài liệu đó (ví dụ worker chạy theo instance, soi ảnh đồng bộ, giới hạn gọi AI theo từng instance), **làm theo US/BR/TDD**.

## Cách gọi

- **Hệ thống thật:** `bmt-be` (.NET 8) và `buildx-fe-web` (Next.js, thương hiệu BuildX).
- **App demo:** `design-estimate` (Python/FastAPI). Khách hàng dựng app này để mô tả luồng mong muốn. Mốc chuẩn là commit `059e876`, prompt `2d-13` cho 2D và `dt-14` cho dự toán.

## Bối cảnh

Hệ thống thật đã có luồng Tạo dự toán (STORY-PROJ-001…007): form nhập liệu, tự lưu, giữ lượt, khóa sửa, chia sẻ, xuất tệp. Riêng phần AI vẫn chạy bằng adapter giả `mock-v1`.

App demo thể hiện luồng khách hàng muốn: 2D → dự toán → 3D → hồ sơ PDF. Ngày 11/10/2026 nhóm đã chạy thử app demo với AI thật. Kết quả cho nhà phố 2 tầng: khoảng 15 phút, khoảng $1,35, đủ bản vẽ, dự toán 84 dòng và 8 ảnh 3D.

Mục tiêu:
- Viết lại phần xử lý AI của app demo bằng .NET trong `bmt-be`, gọi OpenAI và Anthropic trực tiếp.
- Đổi giao diện thành 4 chặng, khách bấm chạy từng chặng.
- Giữ form hiện có và thêm các trường mà AI cần.
- Cập nhật bộ tài liệu PROJ theo quy trình của dự án.

## Quyết định đã chốt (11/10/2026)

**Luồng và giao diện**
- **Thanh tiến trình 5 bước:** Nhập liệu → Bản vẽ 2D → Dự toán → Phối cảnh 3D → Hồ sơ. Mỗi chặng có màn hình kết quả riêng.
  - Đây là thay đổi giao diện đã được giao. Khi bàn giao phải nêu rõ màn hình trước → sau.
- **Khách tự bấm chạy từng chặng, đúng thứ tự.** Dự toán mở sau khi xong 2D, 3D mở sau dự toán, Hồ sơ mở sau 3D. Khách có thể dừng giữa chừng.
- **Chặng đã có kết quả thì không chạy lại.** Chặng lỗi được chạy lại miễn phí, tối đa 3 lần (Admin cấu hình được). Quá 3 lần thì báo khách liên hệ hỗ trợ; Admin có nút đặt lại bộ đếm.
- **Tự làm lại phần hỏng một lần:**
  - 2D: bản vẽ hỏng được vẽ lại 1 lần. Vẫn hỏng thì 2D lỗi và hoàn lượt.
  - 3D: ảnh lỗi được dựng lại 1 lần. Vẫn lỗi thì chặng 3D lỗi; lần chạy lại chỉ dựng các ảnh còn thiếu.
- **Hạn chờ mỗi chặng** (Admin cấu hình được): 2D 20 phút, dự toán 20 phút, 3D 40 phút, hồ sơ 5 phút. Quá hạn thì chặng đó lỗi.

**Lượt và gói**
- **Một lượt cho cả bộ.** Lượt được giữ khi bấm tạo 2D và tính là đã dùng khi 2D thành công. 2D lỗi thì hoàn lượt.
- Các chặng sau không trừ thêm lượt. Gói hết hạn sau khi đã xong 2D thì khách vẫn chạy tiếp được.
- **Khóa sửa đầu vào:**
  - Form bị khóa từ lúc bấm tạo 2D.
  - 2D lỗi thì mở lại cho khách sửa.
  - 2D thành công thì khóa vĩnh viễn.
  - Tên bản dự toán vẫn đổi được như quy tắc hiện có.

**Đầu vào** (giữ form hiện có, xử lý như cách "Tải ảnh lô đất + mô tả" của app demo)
- **Ảnh lô đất bắt buộc.**
  - Định dạng JPG/PNG/WebP, tối đa 10 MB, tối thiểu 640×480.
  - AI soi ảnh ngay khi tải lên; ảnh "Không đạt" chặn tạo 2D.
  - FE vẫn tải ảnh lên qua URL presign như hiện nay. **BE tự đọc nội dung ảnh từ kho** để kiểm tra, tính SHA-256 và soi.
  - Kết quả soi được nhớ lâu dài theo (tài khoản, hash ảnh, phiên bản prompt, model) và **chỉ dùng lại trong cùng tài khoản**.
  - Mỗi tài khoản soi tối đa **30 ảnh mới mỗi ngày**; Admin cấu hình được.
- **Thêm** các ô:
  - Số phòng ngủ 1–10, bắt buộc.
  - Có gara ô tô, không bắt buộc.
  - Gói hoàn thiện Cơ bản / Tiêu chuẩn / VIP, mặc định Tiêu chuẩn.
  - Bộ chọn cạnh có cửa vào Trái / Phải / Trên / Dưới quanh ảnh, **mặc định cạnh trái** như app demo.
- **Bỏ ô Diện tích**, thay bằng mặt tiền × chiều sâu, không bắt buộc. Hai ô này lấy sẵn kích thước AI đọc được trên ảnh và khách sửa được.
- Mô tả tối đa **4.000 ký tự**.
- Số tầng 1–4, cộng tum. Căn hộ luôn 1 sàn.
- **Giữ hai nhóm phong cách** kiến trúc và nội thất:
  - kiến trúc quyết định mái và ngoại thất;
  - nội thất quyết định bảng màu, ảnh phòng và vật liệu.

  Vì vậy phải sửa prompt so với app demo.
- **Vùng giá theo tỉnh khách chọn:** bảng 34 tỉnh → miền Bắc, Trung hoặc Nam; Admin sửa được. Ngày 12/10/2026 người dùng chốt Tây Nguyên thuộc miền Trung, bỏ dấu Tây Nguyên.

**Danh mục, luật giá, AI**
- **Danh mục do Admin quản lý.** Mỗi loại công trình gắn một **mã gốc** của app demo (`nha-pho`, `villa-biet-thu`, `nha-mai`, `nha-vuon-cap4`, `can-ho`). Mỗi phong cách kiến trúc gắn một mã phong cách gốc.
- **Mỗi phong cách nội thất có ô "Mô tả cho AI"** (bảng màu, vật liệu, đồ đạc), bắt buộc. Nhóm dự án viết sẵn mô tả cho các phong cách đang có.
- **Admin sửa hai tệp luật giá trên giao diện** (`khung_gia_tham_khao.md`, `quy_tac_boc_tach.md`).
  - Có lịch sử phiên bản; mỗi bản dự toán ghi lại phiên bản luật đã dùng.
  - Khi lưu §3–5, hệ thống tự tính lại các dải giá.
- **Admin đổi model AI và tham số chất lượng trên giao diện.** Prompt nằm trong mã nguồn và có số phiên bản. Mỗi kết quả ghi lại model và phiên bản prompt đã dùng.
- **Cờ "cần kỹ sư rà" và "cần nhân viên rà" chỉ lưu nội bộ**, không có bước duyệt. Ghi chú nội bộ được lọc khỏi nội dung gửi khách, như app demo đang làm.
- **Chuyển đúng hành vi app demo trước, sửa lỗi chất lượng sau** bằng phiên bản prompt hoặc luật mới. Các lỗi đã thấy: tự thêm giếng trời, `wc_count = 0`, dòng mái bị loại vì 0 đồng.

**Kết quả và hồ sơ**
- Chặng Hồ sơ không gọi AI và tạo ra:
  - **PDF gộp A3 ngang**, cùng thứ tự trang như app demo;
  - **PDF phối cảnh 3D**;
  - **PDF bản vẽ kiến trúc** (2D kỹ thuật + nội thất);
  - **Excel dự toán**.

  Không có bản vẽ kết cấu.
- **PDF dựng bằng PdfSharp + MigraDoc** (MIT, không cần trình duyệt): vẽ lại bố cục bám mẫu `savico/core/pdf.py` và nhúng font tiếng Việt. Excel dựng bằng ClosedXML từ `template.xlsx` của app demo.
- Thông tin trên PDF: tên khách và số điện thoại lấy từ tài khoản, tên dự án là tên bản dự toán, mã hồ sơ là mã BUILDX.
- **Chia sẻ (link/QR/email) và tải tệp chỉ mở sau chặng Hồ sơ.**
- Tệp lưu ở chế độ công khai với đường dẫn khó đoán, như ảnh đầu vào hiện nay. Khách xem và tải qua backend.

**Triển khai**
- **Giao theo 4 đợt:**
  - Đợt 1: đầu vào + 2D.
  - Đợt 2: dự toán + Admin luật giá.
  - Đợt 3: 3D.
  - Đợt 4: hồ sơ PDF.

  Mỗi đợt lên dev/staging trước. **Production chỉ bật luồng mới bằng cờ cấu hình khi người dùng quyết định**; trước đó production giữ luồng cũ.
- **Xóa dữ liệu mẫu `mock-v1`** khi chuyển luồng. Bản nháp cũ phải bổ sung ảnh lô, số phòng ngủ… thì mới tạo được 2D.

**Chốt thêm khi soạn US/BR (11/10/2026)**
- Các bản dự toán có kết quả `mock-v1` bị **xóa hẳn**, **không hoàn lượt** (BR-PROJ-018).
- Giới hạn 10 MiB chỉ dành cho ảnh lô đất; các ảnh khác giữ 5 MiB (BR-MEDIA-001).
- Mô tả **không bắt buộc** khi đã có ảnh lô.
- Danh sách “Dự toán của tôi” có năm trạng thái: Nháp, Đang xử lý, Thất bại, **Đang làm dở** (đã xong 2D, chưa xong Hồ sơ), **Hoàn tất** (đã xong Hồ sơ).
- Quyền Phối cảnh 3D trong gói đã tạm bỏ ngày 10/10/2026 (BR-SUB-008), nên chặng 3D không kiểm tra quyền 3D.
- 12 điểm suy ra khi soạn US/BR đã được người dùng xác nhận. Hai điểm đổi so với bản nháp: Admin đổi model hoặc prompt soi ảnh thì vẫn dùng kết quả Đạt cũ (BR-PROJ-015 khoản 9); ô “Mô tả cho AI” tối đa 1.000 ký tự (BR-PROJ-004 khoản 21).
- **Bộ quy tắc AI do Admin quản lý** (BR-PROJ-019, STORY-PROJ-013): đặc điểm theo mã gốc loại công trình, ma trận mái (mã gốc × phong cách kiến trúc → kiểu mái), lời dặn theo gói × chặng và theo miền, bảng lời dặn chung. Lưu theo hướng lai: phạm vi dạng quan hệ, số có kiểu, lời dặn văn bản. Thuật toán (thứ tự ưu tiên kích thước lô, đo hình học, quyết định đủ chỗ phòng/gara, lưới an toàn) vẫn trong mã nguồn.
- **Đổi quyết định về prompt:** Admin sửa phần lời dặn của prompt trên giao diện, có xem trước và chạy thử; phần cấu trúc dữ liệu AI phải trả vẫn trong mã nguồn (BR-PROJ-014).
- Bảng quy tắc và dữ liệu nạp sẵn từ app demo có ngay ở đợt dùng; màn Admin sửa quy tắc và prompt là đợt riêng sau đợt 4.

**Chốt thêm sau rà soát TDD (12/10/2026)** — chi tiết ở BR/TDD được dẫn:
- Hàng đợi: DB là nguồn sự thật, RabbitMQ/outbox chỉ để đánh thức worker và báo tin; giới hạn ảnh chung qua Redis; worker chạy trong container API (TDD-PROJ-006, 008).
- Lỗi hệ thống không tính vào giới hạn lỗi; bộ đếm riêng từng chặng (BR-PROJ-011).
- Hàng chờ đầy thì từ chối ngay; dịch vụ AI lỗi liên tục thì tạm ngắt nhận việc; lần chạy tốn quá ngưỡng chỉ cảnh báo; xử lý theo thứ tự tiếp nhận (BR-PROJ-012).
- Lần chạy lại dùng lại phần đã xong dù Admin đổi cấu hình (BR-PROJ-011 khoản 8).
- Trần soi ảnh chung của cả hệ thống mỗi ngày, có cảnh báo (BR-PROJ-015 khoản 10).
- Tệp kết quả lưu riêng tư, link tải hạn 15 phút (BR-PROJ-006 khoản 12).
- Quyền vận hành riêng cho đặt lại bộ đếm và đọc nhật ký AI thô, mỗi lần đọc đều ghi vết; nhật ký AI giữ vĩnh viễn kể cả khi khách xóa tài khoản (STORY-PROJ-012).
- Quay lui trên production: tắt luồng, giữ kết quả, không quay về `mock-v1` (BR-PROJ-018 khoản 8).

## Thiết kế kỹ thuật

### Kiến trúc
- **Thư viện mới `src/bmt-be.estimatepipeline`** chứa logic thuần, không EF và không HTTP:
  - mô hình phương án, đọc mô tả tiếng Việt, luật lô đất và hình học, đo đường bao;
  - luật sửa phương án, các bộ prompt;
  - lưới an toàn dự toán, tính dải giá, kế hoạch dựng 3D, lọc nội dung gửi khách.

  Chỉ `application` tham chiếu thư viện này. Thêm vào `bmt-be.sln` và các dòng `COPY` trong `src/bmt-be.api/Dockerfile`.
- **Thư mục tính năng `estimatePipeline`** ở mỗi tầng: `contract/services`, `application/usecases`, `application/services` (các bộ chạy chặng), `infrastructure/estimatePipeline/{openai,anthropic,excel,pdf,storage,fake,worker}`, `persistence`, `presentation/apis`. Theo quy ước trong `bmt-be/.claude/skills/implement-usecase/references/project-conventions.md`.
- **Mô hình trạng thái chặng:**
  - `EstimateStageRun`: mỗi lần chạy một dòng.
    - Chỉ mục duy nhất từng phần trên (EstimateId, Stage) khi State ∈ {Queued, Running, Succeeded}. DB nhờ đó tự chặn chạy trùng và chặn chạy lại sau khi thành công.
    - Giới hạn 3 lần lỗi = số dòng Failed/TimedOut có `CountsTowardCap`. Nút đặt lại của Admin tắt cờ này.
  - `EstimateStageStep`: điểm lưu cho từng bước tốn tiền. Ví dụ `spec`, `plan:technical:f1`, `render:exterior_day`, `estimate:main`, `estimate:reask:*`.
- **Lượt chỉ dùng `UsageOperation` cho chặng 2D**, qua `IDesignUsageCoordinator` (`RequestEstimateGenerationCommandHandler.cs:108-143`). Cách này không đụng chỉ mục `UX_UsageOperation_LiveDesignGeneration`.
  - Bản chụp đầu vào dùng lại `EstimateGenerationInput` với `SchemaVersion = 3`; thêm nhánh InputV3 vào `MediaSourceReader`.
- **Cổng mới** thay cho `IEstimateAiGateway` (cổng cũ chỉ giữ để đọc dữ liệu cũ):
  - `IEstimateStageExecutor` với 4 bản cài đặt cho 4 chặng.
  - Các port: `ILotVerifier`, `IStructuredTextModel`, `IImageModel`, `IToolCallModel`, `IEstimateWorkbookWriter`, `IDossierPdfRenderer`, `IStageArtifactStore`, `IAiCallRecorder`.
- **Worker `EstimateStageWorker`:**
  - Nhận việc bằng `FOR UPDATE SKIP LOCKED`, theo mẫu `EstimateGenerationRunner.cs`.
  - Lease 120 s, cứ 30 s gia hạn một lần (heartbeat). Mất lease thì hủy việc đang chạy.
  - Quét hạn chờ và chốt kết quả bằng `FinalizeStageRunCommand`.
  - Mỗi bước tốn tiền: tải tệp lên kho, ghi bước vào DB, rồi mới sang bước sau. Khi chạy tiếp sau sự cố, các bước đã xong được bỏ qua, nên mất tối đa một lời gọi đang chạy dở.
  - Mỗi lời gọi AI chỉ được dùng phần thời gian còn lại của chặng.
- **Chế độ** `EstimatePipelineOption:Mode = Off | Fake | Live`, kèm cờ `EnabledStages`.
  - Fake chỉ thay nhà cung cấp AI: phát lại phản hồi đã ghi từ app demo và dùng ảnh giả PNG. Mọi luật, lưới an toàn, Excel và PDF vẫn chạy thật.

### Gọi AI
- **Anthropic:** NuGet chính thức `Anthropic`, tắt retry của SDK, luôn dùng streaming.
  - Bật prompt cache cho hai tệp luật giá.
  - Xử lý các trường hợp: lỗi giữa stream, stream bị cắt, `max_tokens`, `refusal`.
  - Mỗi model có cờ khả năng: `ForcedToolChoice`, `Temperature`. Các model mới từ chối `tool_choice` ép buộc và `temperature`, nên khi đó chuyển sang `tool_choice:auto` kèm hỏi lại.
- **OpenAI:** `HttpClient` có kiểu gọi `/v1/responses` (json_schema strict) và `/v1/images/generations|edits`.
  - Gặp lỗi 400 "không hỗ trợ tham số X" thì bỏ X, gọi lại, và nhớ điều này theo model (trường hợp `input_fidelity`).
- **Retry chung** (`AiRetryPolicy`), theo app demo:
  - Chờ 2 → 4 → 8 s, cộng ngẫu nhiên; tôn trọng `Retry-After`.
  - Thử lại khi gặp 429, 5xx hoặc lỗi mạng; dừng ngay khi hết tiền và báo Admin qua `IAlertNotifier`.
- **Ghi nhận mỗi lời gọi** vào `AiCallLog`: token, chi phí theo bảng giá Admin, thời gian, mã lỗi. Prompt và phản hồi thô lưu ở `media/site-private/estimate-ai/…`.
- Thêm khóa vào `.docker/.env.sample.secrets`, `.docker/env-classification.json`, `compose.yaml` và `.github/workflows/deploy-application.yaml`.

### Bản đồ chuyển mã (khoảng 32.000 dòng C#)

| App demo | .NET | Rủi ro |
|---|---|---|
| `plan2d/verify.py` | `LotVerification` | thấp |
| `plan2d/inputs.py`, `brief.py` | `BriefReader`, `JobNormalizer` | cao: regex tiếng Việt, chuẩn hóa NFC |
| `lot_rules.py`, `core/geometry.py`, `frame.py` | `LotRules`, `Geometry`, `FrameRenderer` (ImageSharp 2.x) | vừa |
| `outline.py`, `gate.py` | `OutlineMeasurer`, `PlanGate` | cao: xử lý điểm ảnh |
| `plan2d/service.py` (`enforce_spec_rules`), `prompts.py` | `SpecRuleEngine`, `Plan2dPrompts` (2d-13) | cao |
| `estimate/dutoan.py`, `quantity.py`, `service.py`, `prompts.py` | `EstimateNets`, `QuantityBase`, `EstimatePrompts` (dt-14) | cao: cách làm tròn số kiểu JS |
| `estimate/fill.py`, `resources/excel/fill_template.py` | `CustomerContentFilter`, `ClosedXmlEstimateWorkbookWriter` | vừa |
| `tools/derive_price_bands.py` | `PriceBandDeriver` | vừa |
| `render3d/*` | `RenderPlanBuilder`, `Render3dPrompts` (3d-1, có hai nhóm phong cách) | vừa |
| `core/pdf.py` | `MigraDocDossierRenderer` (vẽ lại bố cục) | vừa |

**Đối chiếu với app demo:** viết một script chỉ dùng khi phát triển, đặt ở `bmt-be/test/parity/`. Script chạy các hàm thuần của app demo ở commit `059e876` trên các mẫu `tests/fixtures` và xuất ra các bộ dữ liệu vào → ra (JSON). Test xUnit chạy lại các bộ này trên mã .NET; riêng phần đo đường bao dùng sai số dưới 1%.

### Dữ liệu (mỗi đợt một migration)
- **Đợt 1:**
  - `Estimate`:
    - `AreaM2` chuyển sang cho phép null.
    - Thêm `LotFrontageM`, `LotDepthM`, `EntranceSide`, `DesiredBedrooms`, `HasGarage`, `LotVerificationId`.
    - Description dài tối đa 4000; số tầng ≤ 4 (sửa các check ở `EstimateConfigurations.cs:27-80`, `EstimateInputPolicy.cs:266-345`).
  - Bảng mới:
    - `EstimateLotVerification` (chỉ mục account + sha256 + prompt + model).
    - `PricingRegionProvince` (nạp sẵn 34 tỉnh).
    - `EstimatePipelineConfigRevision` (model, cờ khả năng, hạn chờ, giới hạn lần lỗi và soi ảnh, bảng giá token; chuỗi phiên bản giống `CatalogWriteFlow.cs`).
    - `EstimateStageRun`, `EstimateStageStep`, `EstimateStageArtifact`, `AiCallLog`, `EstimatePlanResult`.
  - Danh mục: `CatalogBuildingType.BaseCode`, `CatalogTypeStyle.BaseStyleCode`, `CatalogStyle.PromptDescriptor`. Mục chưa gắn mã gốc thì báo `CatalogNotMapped`.
  - Quyền mới: `EstimatePipelineConfigManage`, `EstimatePipelineInspect` (cần migration nạp quyền).
  - Xóa dữ liệu mẫu `mock-v1`.
- **Đợt 2:** `PriceRuleDocument` và `PriceRuleDocumentVersion` (nạp sẵn từ tệp của app demo), `EstimateCostResult` (bản gửi khách và bản nội bộ dạng jsonb, cùng các tổng tiền). Quyền `EstimatePricingRuleManage`.
- **Đợt 4:**
  - Ghi `EstimateGenerationResult` (contract `pipeline-v1`) và `EstimateResultFile`.
  - Thêm `PipelineV1ResultContract`.
  - Nhờ vậy phần chia sẻ, xuất tệp và BR-SUB-007 chạy tiếp mà không phải sửa (`EstimateSharingStore.cs`, `EstimateSharingWorkers.cs`).
- **Lưu tệp:**
  - `IMediaObjectStore.PutAsync` với khóa `estimates/{estimateId}/{stage}/{runId}/{itemKey}-{sha8}.{ext}`.
  - Thêm nhánh `EstimateStageArtifact` vào `MediaSourceReader` để tiến trình dọn kho không xóa các tệp này.
  - Thêm việc dọn PDF mồ côi.

### API (`/api/v1`, mọi lệnh ghi đều có Idempotency-Key)
- **Khách:**
  - `POST /estimates/{id}/lot-verifications`
  - `PUT /estimates/{id}/input` (mở rộng thêm trường)
  - `POST /estimates/{id}/stages/{stage}/runs`
  - `GET /estimates/{id}/stages` (trạng thái, tiến độ, số lần chạy lại còn lại, lý do bị chặn)
  - `GET /estimates/{id}/stages/{stage}/result`
  - `GET /estimates/{id}/files/{fileId}`
- **Admin:**
  - `/admin/estimate-pricing/rule-documents/*` (sửa, xem lịch sử, xem trước dải giá, khôi phục)
  - `/admin/estimate-pricing/regions`
  - `/admin/estimate-pipeline/config`
  - `GET /admin/estimates/{id}/pipeline`
  - `POST …/stages/{stage}/reset-failures`
  - Lệnh danh mục nhận thêm `baseCode`, `baseStyleCode`, `promptDescriptor`.
- **Mã lỗi:** `LotImageRequired`, `LotImageInvalid`, `LotVerificationRequired`, `LotVerificationFailed`, `LotVerificationQuotaExceeded`, `StagePrerequisiteNotMet`, `StageAlreadySucceeded`, `StageRunInProgress`, `StageRetryLimitReached`, `PipelineUnavailable`, `CatalogNotMapped`, `PriceRuleInvalid`, `VersionConflict`.
- Endpoint cũ `POST …/generations` trả 409 khi luồng mới đang bật. Xóa bản dự toán bị chặn khi có chặng đang chạy (BR-PROJ-009).

### Frontend (`buildx-fe-web/src/features/design`, theo skill `savico-implement-feature` và `savico-integrate-api`)
- **Đợt 1:**
  - `DESIGN_STEPS` thành 5 bước; thêm route `design/[projectId]/plan`.
  - Sửa `step-input-api-form.tsx`, `land-photo-field.tsx`, `estimate-input.logic.ts`, `estimate-api.schema.ts`:
    - ảnh lô bắt buộc, có thẻ kết quả soi và bộ chọn cạnh cửa vào;
    - thêm số phòng ngủ, gara, gói;
    - thêm mặt tiền × chiều sâu, bỏ Diện tích;
    - mô tả tối đa 4.000 ký tự.
  - Viết hook `use-stage-run(stage)` thay cho cách hỏi trạng thái hiện tại.
- **Đợt 2:**
  - Màn dự toán dùng lại `estimate-table`, `cost-donut`, với bộ đọc dữ liệu `pipeline-v1` trong `estimate-result.logic.ts`.
  - Thêm các màn Admin: luật giá (có so sánh phiên bản), vùng giá, cấu hình AI.
- **Đợt 3:** route `design/[projectId]/render`, có thư viện ảnh và thanh tiến độ.
- **Đợt 4:**
  - `dossier-ready`, `dossier-overview` liệt kê 4 tệp.
  - `shared-dossier-view` hiển thị `pipeline-v1`; cập nhật ánh xạ role trong `design.bmt.ts`.

## Trình tự làm mỗi đợt

1. **Tài liệu nghiệp vụ** theo skill `prepare-feature`: soạn US/BR, người dùng chốt, rồi soạn ST.
   - **Cập nhật 11/10/2026:** người dùng chốt soạn US/BR/ST cho cả 4 đợt một lần; TDD vẫn soạn theo đợt. Bản nháp US/BR đã soạn:
     - Story: STORY-PROJ-002 viết lại thành chặng 2D; thêm STORY-PROJ-008 (Dự toán), 009 (3D), 010 (Hồ sơ), 011 (Admin luật giá và miền giá), 012 (Admin cấu hình AI, xem lần chạy, đặt lại bộ đếm lỗi); sửa STORY-PROJ-001, 003–007.
     - BR: viết lại BR-PROJ-001, 002, 005, 007; sửa BR-PROJ-003, 004, 006, 008, 009; thêm BR-PROJ-010 (thứ tự chặng), 011 (chạy lại, tự làm lại), 012 (hạn chờ), 013 (miền giá), 014 (ghi phiên bản), 015 (soi ảnh lô), 016 (luật giá), 017 (nội dung nội bộ), 018 (chuyển luồng).
     - Sửa BR-SUB-003, BR-SUB-016 (một lượt cho cả bộ, hạn chờ theo chặng) và BR-MEDIA-001 (ảnh lô 10 MiB).
     - Thêm BR-PROJ-019 (bộ quy tắc AI) và STORY-PROJ-013 (Admin quản lý quy tắc AI và prompt, đợt sau đợt 4); sửa BR-PROJ-014 (prompt).
2. **Thiết kế kỹ thuật** theo skill `design-feature-technical`: soạn TDD, người dùng chốt, rồi soạn UT.
   - Viết lại TDD-PROJ-002 (kiến trúc luồng).
   - Thêm TDD-PROJ-006 (2D), 007 (dự toán), 008 (3D), 009 (hồ sơ), 010 (AI và cấu hình).
   - Cập nhật TDD-PROJ-001/003/005.
3. **Code** theo skill `implement-usecase` (backend) và `savico-implement-feature` (frontend).
   - Báo thay đổi giao diện trước khi sửa.
   - Commit tài liệu theo pathspec từng nhóm, không dùng `git add -A`.

## Kiểm chứng
- **Unit test:**
  - chạy lại các bộ dữ liệu mẫu từ app demo;
  - các handler: chặn theo thứ tự chặng, giới hạn lần lỗi, khóa sửa, giữ/hoàn lượt, chạy lại chỉ phần thiếu;
  - validator;
  - retry và đọc stream qua stub `HttpMessageHandler`: 429 + Retry-After, 5xx, lỗi 400 tham số, stream bị cắt, lỗi tool ép buộc;
  - Excel ClosedXML so với Excel do openpyxl tạo;
  - `PriceBandDeriver` cho ra đúng 36 dải giá của app demo.
- **Integration (Testcontainers Postgres):** migration và các check, chỉ mục duy nhất từng phần, `SKIP LOCKED`, mất lease, tranh chấp giữa quét hạn chờ và `ExpireStaleUsageJob`, tham chiếu tệp trong kho.
- **Chạy đầu-cuối ở chế độ Fake** bằng skill `run-local-api`: đủ 4 chặng, tắt worker giữa chừng rồi chạy tiếp, giới hạn lần lỗi, và luồng FE.
- **Chạy thật có trả phí trên staging** với cùng đầu vào đã dùng ngày 11/10 (hồ sơ app demo `BUILDX-20261011-YQH93J`). So sánh:
  - phương án (phòng, diện tích);
  - ảnh;
  - tổng dự toán trong biên độ ± đã thống nhất;
  - cấu trúc Excel, thứ tự trang PDF;
  - thời gian và chi phí so với số đo của app demo.
- **Cổng phát hành:** xuất lần chạy .NET theo đúng cấu trúc thư mục của app demo, rồi chạy `tools/eval_plan2d_geometry.py` và `eval_estimate.py` của app demo.

## Giả định và rủi ro còn lại
- Khi gặp sự cố, mất tối đa một lời gọi AI đang chạy dở (giả định chấp nhận được).
- Danh sách model Admin được chọn phải có cờ khả năng để tránh lỗi gọi API.
- Giới hạn gọi đồng thời tới OpenAI/Anthropic hiện tính theo từng instance, chưa phải giới hạn toàn hệ thống.
- Phần dễ lệch nhất khi chuyển mã: regex tiếng Việt, cách làm tròn số kiểu JS, đo đường bao bằng ImageSharp so với Pillow.
- PDF vẽ lại bằng MigraDoc sẽ gần giống, không khớp từng điểm ảnh với app demo.
- Thay đổi của app demo sau commit `059e876` không được tự động chuyển sang; cần người theo dõi và chuyển tiếp.
- Cần mua hoặc cấp khóa OpenAI và Anthropic cho dev, staging và production.
