# Kiểm chứng chặng Phối cảnh 3D — 13/10/2026

Chặng 3D đã chạy thành công trên local: tạo và lưu đủ 8 ảnh 1280×720 cho dự án “Nhà phố 2 tầng 5x10 - ca đúng”. Backend và frontend nằm ở nhánh `feature/estimate-pipeline-w3`; chưa push hoặc đưa lên develop.

## Phạm vi đã làm

Theo [TDD-PROJ-010](../tdd/TDD-PROJ-010.md) và [STORY-PROJ-009](../userstory/STORY-PROJ-009.md): chọn ảnh và dựng prompt từ demo `059e876`, giữ đầu vào/kiểu mái đã chốt, dựng ngày → đêm → nội thất theo tầng, lưu từng ảnh, dùng lại ảnh đã công bố khi chạy tiếp, chỉ chốt thành công khi đủ mục trong kế hoạch. Ảnh vẫn riêng tư và truy cập qua route kiểm quyền.

Trước đây kết quả Dự toán chưa có màn dựng 3D. Nay có nút sang bước 4, nút khởi chạy, tiến độ, ảnh theo tầng, ô chờ, lưu ý và nút chạy tiếp khi lỗi. Tải lại hoặc lỗi đọc API giữ ảnh đã có. Màn Admin bổ sung nguồn artifact/lần chạy và hướng dẫn bốn cờ kiểm ảnh. Bước Hồ sơ hiển thị sẵn sàng về điều kiện dữ liệu; phần tạo Hồ sơ thuộc đợt tiếp theo.

## Kết quả tự động

| Phạm vi | Kết quả | Căn cứ chạy |
| --- | --- | --- |
| Thư viện tính toán và prompt | 1.016 đạt | `dotnet test test/bmt-be.estimatepipeline.tests --no-restore` |
| Riêng parity và giá trị biên 3D sau sửa cuối | 54 đạt | Filter `FullyQualifiedName~Render3d`; đối chiếu 19 HouseSpec, 75 prompt và dHash từ ảnh thật của demo |
| Application pipeline | 235 đạt | Filter `FullyQualifiedName~estimatePipeline` |
| Infrastructure pipeline | 305 đạt | Cùng filter; gồm 17 ca executor 3D |
| Persistence | 20 đạt | `dotnet test test/bmt-be.persistence.tests --no-restore` |
| Luồng tích hợp Fake 2D/Dự toán/3D | 4 đạt, 1 bỏ qua | Filter ba lớp EstimatePipeline*FlowTests; ca Live 2D có sẵn được bỏ qua chủ động |
| Frontend pipeline | 34 đạt | `pnpm test:estimate-pipeline`; gồm ba ca Zod 3D |
| Build và kiểm mã | Đạt | API build `--no-restore`; FE TypeScript, ESLint, Prettier, production build |

Luồng tích hợp kiểm ảnh và điểm lưu thật trong Postgres của test, quyền chủ sở hữu, không trừ thêm lượt/không tạo bản giá, giữ ảnh qua retry, không tính bước kế hoạch vào tiến độ, từ chối ghi sau hết lease và chưa đủ ảnh thì không mở Hồ sơ. Unit executor kiểm tham chiếu ngày cho ảnh đêm, dùng lại ảnh, hai lần ảnh lỗi, lỗi nhà cung cấp, QC không khả dụng và mất lease sau dựng.

## Kiểm browser

Kiểm trên Chrome với phiên local, màn 1440 px và 390 px. Dữ liệu phát lại dùng ảnh có sẵn của demo; POST retry bị chặn tại browser, không gửi tới backend/AI. Các ca đã đạt: 3 ảnh + 5 ô chờ, tải lại giữ 3/8, lỗi đọc API giữ ảnh, Failed vẫn có ảnh và nút chạy tiếp, retry gửi một yêu cầu, Succeeded đủ 8 ảnh và không có nút dựng lại, mobile không tràn ngang. Không có lỗi JavaScript.

Sau Live, mở lại trang trên production build local: đủ 8 ảnh tải được, tất cả 1280×720, chặng Succeeded và điều kiện Hồ sơ Ready. Quan sát ảnh ngày/đêm giữ cùng mặt tiền hai tầng; ảnh nội thất hiển thị trong nhóm tầng tương ứng. Đây là kiểm trực quan của ca này, không phải kết luận chất lượng cho mọi loại công trình.

Route `/en` vẫn chuyển về `/vi` theo `src/proxy.ts` hiện có. Đã bổ sung chuỗi tiếng Anh, chưa kiểm UI tiếng Anh trên browser.

## Một lần Live được người dùng cho phép

- Dự án: `4bdc9ea5-2c5c-4619-b5d6-2bec9510628b`.
- Run: `6127a540-d6c4-4145-b589-1b3227873936`.
- Model: OpenAI `gpt-image-2.5-sunburst`; quality high, 1280×720.
- Bắt đầu 02:34:33, kết thúc 02:38:08 ngày 13/10/2026 (Asia/Ho_Chi_Minh): khoảng 214,8 giây.
- 8/8 ảnh, 8 AiCallLog có Outcome=Ok; tổng DurationMs=210.774, từng tác vụ 23.410–31.605 ms. Tổng bộ đếm retry của provider là 1; không có lần bấm chạy lại thủ công.
- EstimatedCostUsd tổng 0,448800 USD theo cấu hình giá trong app; chưa đối chiếu hóa đơn nhà cung cấp. Không suy tốc độ token từ thời gian dựng ảnh.
- Cờ QC, chống ảnh trùng và tham chiếu góc trước giữ tắt theo mặc định demo; không coi ca Live này là kiểm các cờ đã bật.

## Migration và giới hạn kiểm chứng

Migration `20261012190826_EstimatePipelineRender3dStage` đã áp dụng vào `bmt_local_w1`, tạo chỉ dẫn khí hậu theo revision và bổ sung config/prompt/mái cho 3D. Luồng Fake cũng chạy với schema mới. Không rollback database local đã có ảnh; nhánh Down có guard bảo vệ revision đã dùng, nhưng chưa chạy thử vòng Down/Up hoặc tình huống dữ liệu tùy chỉnh trên database riêng.

51 đặc tả [UT-PROJ-429](../unittest/UT-PROJ-429.md)–[UT-PROJ-479](../unittest/UT-PROJ-479.md) giữ Draft, đã kiểm 13 cột, enum, mã và TEST_LINKS. Chưa import; bảng kết quả trên không đồng nghĩa đã thực thi từng đặc tả một-một. Chưa chạy tải đồng thời hoặc kiểm thủ công Admin bằng phiên quản trị trong đợt này.

Build .NET còn cảnh báo NuGet về ImageSharp 2.1.13 đã có ở nền thư viện; đợt này chưa nâng dependency hoặc chạy restore. Chưa phát hành production.
