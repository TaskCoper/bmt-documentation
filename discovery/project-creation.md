# Khảo sát tính năng tạo dự toán

Người dùng đã xác nhận “Ok tôi đã chốt US và BR” cho bộ tài liệu Tạo dự toán hiện tại. Bộ nghiệp vụ gồm năm User Story và bảy BR-PROJ, dùng lại quy tắc quyền/gói/lượt liên quan. Đã bổ sung 70 đặc tả System Test, gồm ba ca theo các quyết định khi thiết kế TDD ngày 21/09/2026 và mười ca theo quyết định ngày 25/09/2026; xem [bảng truy vết](estimate-system-test-coverage.md). Đã soạn ba bản nháp TDD tại [bảng thiết kế](estimate-technical-design.md); chưa sửa mã ứng dụng hoặc thực thi test. Hợp đồng AI và phần rà soát phụ thuộc chưa hoàn tất được nêu rõ bên dưới.

## Cập nhật ngày 25/09/2026

- Chủ sở hữu được đổi tên bản dự toán bất cứ lúc nào, kể cả khi gói hết hạn, hết lượt, toàn bộ lượt còn lại đang bị giữ hoặc AI đang xử lý. Chỉ cần quyền sở hữu và tên hợp lệ; tên không phải đầu vào gửi AI. Các thông tin đầu vào khác giữ nguyên điều kiện gói, lượt và khóa sửa (BR-SUB-007 khoản 11, BR-PROJ-003, BR-PROJ-005).
- Sau khi đổi tên, màn hình chủ sở hữu và trang xem qua link hiện tên mới; tệp PDF/Excel được xuất lại với tên mới, không tính lượt và không gọi AI (BR-PROJ-007 khoản 7, STORY-PROJ-003/AC-006).
- Quản trị danh mục loại công trình và phong cách cần quyền riêng theo STORY-RBAC-001. Vai trò Admin có quyền này, nhưng hệ thống kiểm theo quyền chứ không theo tên vai trò (STORY-PROJ-005).
- Gói giám sát gắn với công trình, một thực thể riêng khác bản dự toán (BR-SUB-007/Notes). Mục “Đã xác nhận” bên dưới ghi “dự án” trong tài liệu quyền/gói/lượt tương ứng với bản dự toán; cách đọc này chỉ đúng với gói thiết kế. Với gói giám sát, “dự án” đọc là công trình.
- Các quyết định đã cập nhật vào US/BR, ST-PROJ-061 đến ST-PROJ-070 và UT-PROJ-049 đến UT-PROJ-061. Thiết kế kỹ thuật xem [bảng thiết kế](estimate-technical-design.md#cập-nhật-ngày-25092026).

## Bổ sung trong bước thiết kế TDD ngày 21/09/2026

- Tên loại công trình và phong cách bắt buộc có nội dung sau trim, tối đa 200 ký tự; cho phép trùng tên.
- Link hết hiệu lực từ 00:00 ngày kế tiếp theo giờ Việt Nam, sau ngày khách chọn; không chọn ngày đã qua.
- Mỗi bản dự toán tối đa một link đang hiệu lực, dùng chung sao chép, QR và email. Dùng lại khi còn hiệu lực; chỉ tạo link khác sau khi hết hạn hoặc thu hồi.
- Ba quyết định đã cập nhật vào US/BR và ST-PROJ-058–060. Các đoạn khảo sát và kế hoạch bên dưới giữ bối cảnh ban đầu; schema, API và phụ thuộc kỹ thuật hiện tại xem [bản nháp TDD](estimate-technical-design.md).

## Đã xác nhận

- Admin tự nhập tên, ảnh minh họa và gán danh mục phong cách kiến trúc/nội thất theo loại công trình trước khi khách sử dụng. Không chuẩn bị sẵn bộ phong cách từ trang mẫu và không chờ người dùng cung cấp danh sách/ảnh khởi tạo. Điều kiện chặn lưu cấu hình có nhóm bật nhưng thiếu lựa chọn vẫn áp dụng.

- Chỉ giữ một trường Mô tả chi tiết tối đa 500 ký tự dùng để gửi AI; không thêm ghi chú riêng. Điều kiện đầu vào vẫn là có ít nhất ảnh hoặc mô tả, không bắt buộc có mô tả nếu đã có ảnh hợp lệ. Đề xuất giữ ghi chú riêng trước đó không áp dụng.

- Mỗi phong cách kiến trúc/nội thất do Admin quản lý bắt buộc có một ảnh minh họa JPG/PNG/WebP, tối đa 5 MB. Đây là quy tắc riêng cho ảnh danh mục, không thay giới hạn ảnh đầu vào bản dự toán.

- Tên bản dự toán bắt buộc, tối đa 200 ký tự sau khi bỏ khoảng trắng đầu/cuối; tên chỉ có khoảng trắng không hợp lệ. Không tự cắt ngắn tên vượt giới hạn.

- Bản dự toán cũ chỉ dùng danh mục loại công trình đã áp dụng khi tạo. Không được chuyển bản nháp sang loại do Admin thêm sau đó; muốn dùng loại mới thêm phải tạo bản dự toán mới, đáp ứng điều kiện tạo hiện hành.

- Cấu hình Admin bật chọn tầng hoặc nhóm phong cách phải có ít nhất một lựa chọn phù hợp cho từng danh sách đang bật. Nếu thiếu, chặn lưu và yêu cầu bổ sung; không thay cấu hình đã lưu bằng cấu hình không hợp lệ.

- Tự lưu lỗi do mạng/máy chủ: giữ nội dung khi trang còn mở, báo chưa lưu, tự thử lưu lại khi kết nối phục hồi và có nút Thử lại. Mỗi lần thử lại vẫn kiểm tra quyền/gói/lượt và khóa sửa. Không tự thử lại yêu cầu bị từ chối do quyền/gói/lượt; không tự chạy lại AI. Sau khi đóng hoặc tải lại trang, chưa hỗ trợ khôi phục phần chưa lưu.
- Khi thay ảnh đầu vào, giữ ảnh cũ đến khi ảnh mới hợp lệ, tải lên và lưu thay thế thành công. Nếu thay thất bại, ảnh cũ vẫn là ảnh đầu vào; bản dự toán chỉ có một ảnh đầu vào sau khi thay thành công.

- Khi khách đổi loại công trình trong bản nháp, giữ lựa chọn phong cách/tầng/tum còn hợp lệ theo cấu hình áp dụng cho bản dự toán, xóa phần không phù hợp và yêu cầu chọn lại phần bắt buộc trước khi gửi AI. Giữ nguyên diện tích, địa chỉ, ảnh và mô tả.
- Khi khách đổi tỉnh/thành, xóa xã/phường đã chọn và yêu cầu chọn lại; giữ địa chỉ chi tiết để khách tự sửa. Bản nháp vẫn được lưu khi chưa chọn lại đủ thông tin.

- Tên nhóm tính năng là **Tạo dự toán** theo yêu cầu đổi tên của người dùng. Đối tượng được tạo, tự lưu, gửi AI và chia sẻ trong nhóm này gọi là **bản dự toán**; người có quyền sở hữu gọi là **chủ sở hữu bản dự toán**. Tên mới vẫn bao gồm thiết kế và hồ sơ thi công, không thu hẹp thành chỉ bảng chi phí.
- Giữ nguyên mã STORY-PROJ-***, BR-PROJ-*** và đường dẫn tài liệu để giữ tham chiếu. Các nhãn và dữ liệu thử trong phần khảo sát website giữ nguyên theo hiện trạng đã quan sát. Thuật ngữ “dự án” trong các tài liệu quyền/gói/lượt cũ khi được nhóm Story này viện dẫn tương ứng với bản dự toán của luồng này; việc đổi tên không thiết kế thêm tính năng quản lý dự án tương lai hoặc quan hệ giữa hai tính năng.

- Lập kế hoạch cho backend và tài liệu nghiệp vụ.
- Phạm vi gồm cả ba bước: nhập thông tin bản dự toán, nhận dự toán và hồ sơ thi công.
- Hỗ trợ năm loại ban đầu: Nhà phố, Villa/Biệt thự, Nhà mái, Nhà vườn/Nhà cấp 4 và Căn hộ; Admin được thêm loại mới. Danh mục không còn cố định ở năm loại.
- Đầu vào phải có ít nhất ảnh hoặc mô tả; không bắt buộc đồng thời cả hai.
- Giữ tối đa một ảnh JPG/PNG/HEIC, dung lượng tối đa 10 MB, và mô tả thiết kế tối đa 500 ký tự. Giới hạn này đã được người dùng xác nhận, không còn chỉ là thông tin quan sát trên trang.
- Cả năm loại công trình dùng một trường chung có nhãn Diện tích, đơn vị m², bắt buộc nhập. Người dùng chọn rõ “Đổi cả năm loại sang một trường diện tích chung”. Không yêu cầu riêng diện tích đất, diện tích xây dựng hoặc chiều ngang/dài. Quyết định mới thay thế phương án hai trường đã trao đổi trước đó.
- Backend chuyển diện tích khách nhập sang AI service theo hợp đồng tích hợp sau này; không tự nhân số tầng để thay giá trị đầu vào. Việc tính toán kết quả thuộc AI service.
- Diện tích lớn hơn 0, cho phép tối đa hai chữ số thập phân, chưa đặt giới hạn tối đa theo nghiệp vụ.
- Với Căn hộ, người dùng yêu cầu bám theo những gì trang web có. Màn hình đã khảo sát chia dự toán thành ba nhóm phần thô, hoàn thiện và nội thất; biểu mẫu không có bước chọn riêng phạm vi sửa chữa. Giữ cấu trúc này trong kế hoạch, không tự giới hạn Căn hộ thành chỉ hoàn thiện/nội thất. Yêu cầu này không xác nhận số tiền mẫu, công thức hoặc việc mọi hạng mục mẫu đều áp dụng cho mọi căn hộ.
- AI service nhận thông tin đầu vào và trả các kết quả thiết kế/dự toán. Người dùng xác nhận: “Phần dự toán AI service sẽ trả cho chúng ta, việc của chúng ta là điền thông tin, còn lại mọi thứ sẽ là do AI trả”. Backend BMT không xây bảng đơn giá Admin quản lý, không tự bóc khối lượng hoặc tính lại dự toán. Đề xuất bảng giá ở lượt trước không còn áp dụng.
- Trách nhiệm tạo nội dung kết quả thuộc AI service. Cách trả dữ liệu, ảnh/bản vẽ và tệp hồ sơ sẽ xác định theo hợp đồng tích hợp; chưa mặc định AI service trả sẵn PDF/Excel hay chỉ dữ liệu để xuất tệp.
- AI service do bên khác phụ trách. Người dùng yêu cầu trước mắt ghi rõ phần chờ tích hợp; chưa giao backend BMT tự xây AI service hoặc tự chốt hợp đồng thay bên cung cấp. Việc chưa có hợp đồng không ngăn tiếp tục làm rõ nghiệp vụ quản lý bản dự toán và thông tin đầu vào.
- Tạo dự toán khi khách nhập tên và thực hiện thao tác tạo; sau đó tự lưu thông tin đang nhập để khách quay lại làm tiếp. Cho phép bản nháp chưa đủ diện tích, ảnh hoặc mô tả. Kiểm tra đủ đầu vào trước khi gửi AI; việc tạo/tự lưu không tự khởi chạy AI.
- Giữ địa chỉ tỉnh/thành–xã/phường–chi tiết và gói hoàn thiện/nội thất Cơ bản/Tiêu chuẩn/VIP. Theo bổ sung qua hai ảnh ngày 2026-09-20, Admin cấu hình việc chọn tầng/tum theo loại công trình. Hiện trạng bốn loại có tầng/tum, Căn hộ không có chỉ là căn cứ ban đầu, không phải logic cố định theo tên loại.
- Admin bật/tắt riêng chọn tầng và chọn tum theo từng loại; được cấu hình cả danh sách số tầng cho loại đó.
- Admin được thêm/sửa cả danh mục phong cách kiến trúc và danh mục phong cách nội thất, gồm tên, ảnh minh họa và gán phong cách được chọn theo loại công trình.
- Khi Admin sửa cấu hình loại công trình hoặc phong cách, bản dự toán đã tạo giữ cấu hình đã áp dụng, kể cả bản nháp. Thay đổi chỉ dùng cho bản dự toán mới. Quyết định này không giữ nguyên quyền/gói/lượt cũ hoặc bỏ qua khóa sửa khi AI chạy.
- Mỗi nhóm phong cách được bật phải chọn đúng một phong cách hợp lệ trước khi gửi AI. Nếu bật cả hai nhóm, khách chọn một phong cách kiến trúc và một phong cách nội thất; nhóm bị tắt không yêu cầu chọn. Bản nháp vẫn có thể chưa chọn đủ.
- Tách Phong cách kiến trúc và Phong cách nội thất. Người dùng xác nhận phần gộp trên website chưa đúng; bảng phong cách khảo sát bên dưới chỉ ghi hiện trạng, không dùng làm danh mục cuối cùng.
- Admin cấu hình theo loại công trình việc cho khách chọn phong cách kiến trúc và phong cách nội thất. Người dùng đã làm rõ: cấu hình chỉ điều khiển lựa chọn phong cách, AI vẫn trả đủ kết quả; không dùng việc tắt lựa chọn để bỏ phần thiết kế, dự toán hoặc hồ sơ. Quyền quản lý cả hai danh mục phong cách đã được xác nhận trong BR-PROJ-004.
- Khóa sửa đầu vào khi AI đang xử lý, kể cả tự lưu đến muộn hoặc cập nhật trực tiếp. Khi thất bại, khách được sửa hoặc chủ động thử lại khi còn đủ điều kiện; bản dự toán đã thành công muốn phương án khác phải tạo mới.
- Giữ đủ các thao tác hồ sơ như trang mẫu: PDF, Excel dự toán, link chia sẻ, QR và email.
- Ai có link hoặc QR còn hiệu lực đều được xem và tải hồ sơ không cần đăng nhập. Chủ sở hữu bản dự toán chọn ngày hết hạn và có thể thu hồi sớm.
- Chủ sở hữu bản dự toán nhập một email nhận mỗi lần gửi; có thể gửi tới người khác, không chỉ email tài khoản của mình.
- Email gửi link hồ sơ, dùng cùng ngày hết hạn và quyền thu hồi. QR và email không tạo đường truy cập độc lập để vượt qua thời hạn hoặc việc thu hồi link.
- Khi gói thiết kế hết hạn, chủ sở hữu bản dự toán vẫn được thao tác đầy đủ trên hồ sơ đã có: xuất PDF/Excel, tạo link/QR, gửi email và thu hồi link; không tạo thiết kế AI mới hoặc tính lượt. Đã bổ sung BR-SUB-007 để ghi rõ phần Excel và chia sẻ.
- Reviewer và Approver của bộ tài liệu tính năng mới đều là Tân Trần. Đây là thông tin người review/phê duyệt, không phải xác nhận bộ tài liệu đã được phê duyệt.

## Hiện trạng đã khảo sát

### Trang tham khảo

- Nguồn: https://vnz-bmt-savico-abcxyz.vercel.app/vi/design/SVC-2026-0006/input.
- Trang yêu cầu đăng nhập. Thử email và mật khẩu ngẫu nhiên theo yêu cầu của người dùng đã vào được giao diện với tên Dev User. Kết quả này chỉ mô tả trang tham khảo, không xác minh cơ chế xác thực của backend BMT.
- Danh sách dự án ban đầu của tài khoản thử trống. Đã tạo dự án khảo sát có tên `TEST BMT - Khảo sát tạo dự án 73918`, mã `SVC-2026-0001`, qua giao diện.
- Đã kiểm tra lại hộp thoại qua nút Tạo dự án mới ở thanh trên cùng: bên dưới Tên dự án có trường “Mô tả (không bắt buộc)”; nút trợ giúp ghi “Ghi chú riêng về dự án, chỉ bạn nhìn thấy.” Đây là vị trí được gọi là ghi chú riêng khi trao đổi, khác ô Mô tả chi tiết trong biểu mẫu nhập liệu. Sau khi làm rõ vị trí, người dùng chọn không đưa trường này vào tính năng Tạo dự toán; chỉ giữ Mô tả chi tiết trong biểu mẫu nhập liệu để gửi AI. Lần kiểm tra lại chỉ mở hộp thoại, không tạo thêm bản ghi.
- Trang nhập liệu có ảnh JPG/PNG/HEIC với hướng dẫn tối đa 10 MB, mô tả với bộ đếm 500, tỉnh/thành phố, xã/phường, địa chỉ chi tiết, loại công trình, gói hoàn thiện/nội thất và phong cách. Người dùng đã xác nhận giữ một ảnh tối đa 10 MB và mô tả tối đa 500 ký tự.
- Các loại ngoài Căn hộ hiển thị lựa chọn từ trệt đến trệt + 4 lầu và có/không tum. Căn hộ ẩn số tầng và tum. Không suy ra tất cả lựa chọn này đều hợp lệ cho mọi loại công trình.
- Gói hoàn thiện/nội thất gồm Cơ bản, Tiêu chuẩn và VIP. Đây là lựa chọn vật liệu/nội thất trên biểu mẫu; cần phân biệt với gói đăng ký cấp quyền sử dụng hệ thống.

Bảng dưới ghi lại giao diện cũ gộp hai nhóm phong cách. Yêu cầu mới đã thay cấu trúc này; chưa phân loại các mục sang kiến trúc hoặc nội thất.

| Loại công trình | Phong cách quan sát được |
|---|---|
| Nhà phố | Hiện đại, Wabi-sabi, Tân cổ điển, Tối giản, Indochine |
| Villa/Biệt thự | Hiện đại, Tân cổ điển |
| Nhà mái | Mái Thái hiện đại, mái Nhật hiện đại |
| Nhà vườn/Nhà cấp 4 | Nhà vườn mái Thái, nhà vườn mái Nhật, biệt thự/villa sân vườn, nhà cấp 4 hiện đại |
| Căn hộ | Hiện đại, Wabi-sabi, Tối giản |

- Màn hình dự toán có tổng tiền, ba nhóm phần thô/hoàn thiện/nội thất, hạng mục và thành tiền, tỷ trọng chi phí, nội dung tư vấn và nút tải Excel.

Các hạng mục quan sát trực tiếp ở dự án Căn hộ thử nghiệm:

| Nhóm | Hạng mục trên trang mẫu |
|---|---|
| Phần thô | Móng, cọc; khung, sàn, mái; xây tô, chống thấm |
| Hoàn thiện | Lát nền, ốp tường, sơn nước; cửa, lan can, trần; điện, nước, thiết bị vệ sinh |
| Nội thất | Nội thất gỗ cố định; nội thất rời; chiếu sáng, rèm, trang trí |

Đây là nội dung mẫu đã quan sát, chưa chứng minh có quy tắc lựa chọn hạng mục theo hiện trạng căn hộ. Không lấy các dòng tiền hiển thị làm bảng đơn giá. Trang mô tả dự toán là tham khảo theo khu vực và thời điểm, nhưng chưa xác minh được nguồn giá hoặc cách tính khối lượng.

- Màn hình hồ sơ có thông tin khách/dự án, trang bìa, mặt bằng 2D, phối cảnh ngoại thất, bảng dự toán, nút render, tải PDF, tạo liên kết chia sẻ, gửi email và QR. Chưa thao tác gửi email hoặc tạo liên kết chia sẻ.
- Hai màn hình kết quả được đọc tại `/vi/design/SVC-2026-0001/estimate` và `/vi/design/SVC-2026-0001/dossier`. Việc mở được màn hình không chứng minh AI thật đã xử lý, đầu ra đúng chuyên môn hoặc tệp xuất hoạt động.
- Với dữ liệu thử Căn hộ và mô tả 70 m², màn hình kết quả hiển thị 240 m² do AI ước tính và có hạng mục móng/cọc. Không dùng các số liệu hoặc công thức suy ra từ ví dụ này làm yêu cầu backend.
- Dấu bắt buộc ở ảnh mâu thuẫn với hướng dẫn cho phép chỉ có mô tả. Quyết định người dùng trong hội thoại đã làm rõ: có ít nhất một trong hai.
- Thông báo “3 lượt thiết kế miễn phí” trên trang chưa phải chính sách của tính năng.

### Mã nguồn workspace

- Workspace có `bmt-be/` và `bmt-documentation/`; chưa tìm thấy repository frontend của trang tham khảo.
- `bmt-be/src/bmt-be.persistence/ApplicationDbContext.cs` lúc khảo sát chỉ khai báo `DbSet<User>`. Kiểm lại ngày 25/09/2026: đã có thêm RBAC, danh mục gói, kỳ thiết kế, lượt và gói giám sát; vẫn chưa có module bản dự toán.
- Thư mục entity hiện có `User.cs`; thư mục API nghiệp vụ hiện có `apis/user/UserApi.cs`.
- README backend mô tả nền tảng tài khoản/xác thực. Chưa có module dự án, xử lý AI, dự toán hoặc hồ sơ trong phần mã đã khảo sát.
- Một số nội dung trong AGENTS.md backend mô tả kiến trúc và đường dẫn tài liệu cũ. Dùng `bmt-documentation/` theo chỉ dẫn workspace hiện tại; không coi thành phần lịch sử là thành phần đã triển khai.

### Tài liệu liên quan đã đọc trực tiếp

- [BR-SUB-007](../businessrule/BR-SUB-007.md): điều kiện tạo/lưu dự án và truy cập kết quả sau khi gói hết hạn.
- [BR-SUB-003](../businessrule/BR-SUB-003.md): giữ lượt và chỉ tính đã dùng khi đủ kết quả đã lưu, có thể mở xem.
- [BR-SUB-016](../businessrule/BR-SUB-016.md): quá thời gian xử lý, giải phóng lượt và không công bố kết quả đến muộn.
- [BR-SUB-017](../businessrule/BR-SUB-017.md): hai loại lượt; không sửa thiết kế sau thành công; thử lại trên dự án thất bại.
- [BR-SUB-008](../businessrule/BR-SUB-008.md): phạm vi kiểm soát quyền hiện tại, mức chi tiết chung của dự toán nội thất/bố trí công năng và các tính năng đã hoãn.
- [BR-RBAC-005](../businessrule/BR-RBAC-005.md): tài khoản khách hàng tách khỏi nhân viên; tài khoản nhân viên không tạo dự án.

Đã tìm thấy Story, TDD và test liên quan đến subscription, thanh toán và phân quyền. Quét mã tham chiếu và tham chiếu ngược ban đầu tìm được 487 tài liệu liên kết với phần tạo dự án (trước khi bổ sung Story 003/004 và BR 007); không phát hiện mã đích thiếu trong lần quét này. Đây chỉ là kiểm tra tồn tại mã, chưa xác minh từng section hoặc đọc hết nội dung. Chưa hoàn tất việc đọc toàn bộ chuỗi tham chiếu và đối chiếu giữa các tài liệu. Chưa kết luận các phụ thuộc này đã triển khai hoặc bộ tài liệu nhất quán hoàn toàn.

## Bộ nghiệp vụ đã được người dùng chốt

Đã soạn [STORY-PROJ-001](../userstory/STORY-PROJ-001.md) cho tạo dự toán, nhập thông tin và tự lưu; [STORY-PROJ-002](../userstory/STORY-PROJ-002.md) cho gửi AI, theo dõi kết quả và xử lý thất bại. Các Business Rule nháp gồm:

- [BR-PROJ-001](../businessrule/BR-PROJ-001.md): danh mục loại công trình có thể mở rộng và một trường diện tích chung.
- [BR-PROJ-002](../businessrule/BR-PROJ-002.md): có ít nhất ảnh hoặc mô tả.
- [BR-PROJ-003](../businessrule/BR-PROJ-003.md): tạo dự toán bằng tên và tự lưu tiến độ.
- [BR-PROJ-004](../businessrule/BR-PROJ-004.md): loại công trình do Admin quản lý, cấu hình tầng/tum và hai nhóm phong cách riêng; địa chỉ và gói hoàn thiện giữ nguyên.
- [BR-PROJ-005](../businessrule/BR-PROJ-005.md): khóa sửa khi AI chạy, mở lại sau thất bại và kiểm tra lại khi thử lại.
- [BR-PROJ-006](../businessrule/BR-PROJ-006.md): link, QR và email dùng cùng quyền xem/tải, ngày hết hạn và cơ chế thu hồi.
- [BR-PROJ-007](../businessrule/BR-PROJ-007.md): dự toán và hồ sơ dùng kết quả AI, hỗ trợ PDF/Excel, không tự tính nội dung chuyên môn tại backend.

Đã làm rõ lưu tiến độ được phép thiếu dữ liệu, còn gửi AI phải đủ dữ liệu. Story dùng lại BR-SUB-007 về điều kiện tạo/lưu và BR-RBAC-005 về tài khoản khách hàng/nhân viên. Đã chốt giới hạn ảnh, mô tả và diện tích; một số luồng lỗi và thông tin phân công còn thiếu. Reviewer/Approver đã ghi Tân Trần. Chưa hoàn tất đối chiếu toàn bộ chuỗi tham chiếu của tài liệu liên quan; người dùng đã chốt nội dung US/BR trong hội thoại; 70 đặc tả ST đã được bổ sung, nhưng chưa coi toàn bộ phụ thuộc kỹ thuật hoặc chuỗi tài liệu ngoài phạm vi PROJ đã hoàn tất.

Phân chia Story của bản nháp: tạo/lưu đầu vào; gửi và theo dõi AI; xem dự toán, hồ sơ và tải tệp; quản lý chia sẻ và gửi email. Đã bổ sung [STORY-PROJ-003](../userstory/STORY-PROJ-003.md) cho xem dự toán, hồ sơ và PDF/Excel; [STORY-PROJ-004](../userstory/STORY-PROJ-004.md) cho link, QR, email và thu hồi. Đã bổ sung [STORY-PROJ-005](../userstory/STORY-PROJ-005.md) cho Admin quản lý danh mục, tầng/tum và hai nhóm phong cách; thay đổi chỉ áp dụng cho bản dự toán mới. Chưa tự định nghĩa API của bên AI.

Chia công việc chuẩn bị theo thứ tự sau:

1. Chốt dữ liệu đầu vào và phạm vi công việc cho từng loại công trình, gồm kích thước/diện tích, số tầng, tum, phong cách và gói hoàn thiện.
2. Chuẩn bị danh sách thông tin cần nhận từ bên AI và ghi rõ phụ thuộc chờ tích hợp. Khi nhận tài liệu, đối chiếu trường đầu vào, cấu trúc kết quả, ảnh/bản vẽ, tệp hồ sơ và dấu hiệu xử lý thành công/thất bại. AI service chịu trách nhiệm nội dung và số liệu dự toán; backend không thiết kế công thức chuyên môn hoặc tự chốt hợp đồng thay bên cung cấp.
3. Chốt vòng đời bản dự toán: tạo, lưu thông tin, gửi AI, xử lý lỗi, nhận kết quả, xuất và chia sẻ hồ sơ; đối chiếu các quy tắc quyền/hạn mức đã có.
4. Soạn User Story và Business Rule theo template trong `bmt-documentation/`, dùng lại quy tắc hiện có khi phù hợp; trình bản nháp đầy đủ để chốt.
5. Sau khi chốt cả US và BR, soạn System Test. Nếu tiếp tục đến thiết kế kỹ thuật, áp dụng các skill thiết kế phù hợp trước khi xác định schema, API, tích hợp AI và lưu tệp.

Thứ tự triển khai đề xuất sau khi tài liệu đủ điều kiện:

| Đợt | Kết quả cần có | Phần backend dự kiến ảnh hưởng | Cách kiểm chứng tổng quát |
|---|---|---|---|
| 0. Quản trị danh mục | Admin thêm loại công trình; bật/tắt tầng và tum riêng; cấu hình số tầng; thêm/sửa tên, ảnh và gán hai danh mục phong cách theo loại. | Chức năng quản trị danh mục và kiểm tra quyền Admin; cung cấp danh mục/cấu hình cho luồng khách hàng. | Quyền quản trị, lựa chọn đúng loại/nhóm, giữ cấu hình bản dự toán cũ và chỉ áp dụng thay đổi cho bản dự toán mới. |
| 1. Tạo và lưu đầu vào | Tạo bằng tên, danh mục loại do Admin quản lý, hai phần phong cách, kiểm tra diện tích/ảnh/mô tả, tự lưu và mở lại. | Domain bản dự toán; contract/validator; handler tạo/đọc/cập nhật; persistence; API; lưu ảnh. | Dữ liệu hợp lệ/không hợp lệ, bản nháp thiếu trường, quyền sở hữu, quyền/gói/lượt, lỗi lưu và yêu cầu đến sai thứ tự. |
| 2. Gửi và theo dõi AI | Tiếp nhận đúng đầu vào, giữ lượt, khóa sửa, nhận kết quả hoặc giải phóng lượt khi lỗi. | Handler xử lý bản dự toán; tích hợp quyền/lượt; thành phần giao tiếp AI và tiếp nhận kết quả. | Yêu cầu trùng/đồng thời, sửa khi đang chạy, thất bại/quá thời gian, thử lại, kết quả muộn, lưu thiếu bộ kết quả. Chỉ kiểm chứng thật khi có AI service. |
| 3. Dự toán và hồ sơ | Xem dữ liệu đã lưu; cung cấp PDF/Excel từ đúng kết quả nguồn. | Truy vấn kết quả; tích hợp tệp; API xem/xuất/tải. | Quyền truy cập, đối chiếu dữ liệu màn hình/tệp, lỗi xuất/tải, quyền dùng hồ sơ cũ sau hết hạn gói. |
| 4. Chia sẻ và email | Link/QR có ngày hết hạn, thu hồi sớm; email một người nhận chứa cùng link. | Quản lý chia sẻ; API xem/tải qua link; tích hợp email. | Xem/tải không đăng nhập khi link hợp lệ, chặn sau hết hạn/thu hồi, tách quyền chủ sở hữu bản dự toán/người nhận, lỗi gửi thư. |

Bổ sung đợt quản trị danh mục trước bốn đợt của luồng khách hàng. Phần nhập liệu có thể được chuẩn bị độc lập trong lúc chờ AI. Quyền/gói/lượt hiện mới có tài liệu, nên phải xác định phụ thuộc triển khai trước khi cam kết đợt 1 chạy đầy đủ. Bảng này là kế hoạch và chiến lược kiểm chứng, không phải schema/API hoặc đặc tả System Test đã chốt.

Phạm vi code dự kiến cần bổ sung gồm domain, contract/validator, application handler, persistence và presentation API; tích hợp AI và lưu tệp thuộc infrastructure theo thiết kế sau. Việc xuất hồ sơ sẽ bám cách AI service trả kết quả, chưa mặc định backend phải tự render PDF/Excel. Chưa đặt tên bảng, endpoint hoặc lựa chọn nhà cung cấp.

Phân chia trách nhiệm đề xuất trên cơ sở quyết định dùng AI service:

| Thành phần | Trách nhiệm |
|---|---|
| Backend BMT | Tiếp nhận và kiểm tra thông tin; kiểm tra quyền sở hữu, gói/lượt; gửi yêu cầu AI; theo dõi xử lý; nhận, kiểm tra cấu trúc và lưu kết quả; cung cấp kết quả cho frontend. |
| AI service | Xử lý đầu vào và tạo nội dung thiết kế/dự toán, gồm khối lượng, đơn giá và số tiền do dịch vụ trả. Danh sách đầu ra và định dạng cụ thể cần được mô tả trong hợp đồng tích hợp. |

Kiểm tra kết quả ở backend nhằm bảo đảm phản hồi đúng cấu trúc và đủ phần bắt buộc theo hợp đồng, không thay thế tính toán hoặc tự sửa số liệu của AI. Cách xử lý phản hồi thiếu/sai cấu trúc sẽ được chốt cùng hợp đồng tích hợp.

Kiểm chứng dự kiến tập trung vào dữ liệu hợp lệ theo từng loại công trình, quyền sở hữu, điều kiện gói/lượt, yêu cầu gửi lặp, AI thất bại hoặc quá thời gian, tính nhất quán của dự toán và tệp kết quả. Đây là phạm vi kiểm chứng tổng quát, chưa phải đặc tả test hoặc kết quả đã chạy.

### Phần tiếp tục chuẩn bị và phần chờ tích hợp

| Phần việc | Có thể chuẩn bị lúc này | Phụ thuộc còn thiếu |
|---|---|---|
| Tạo dự toán và lưu đầu vào | Tạo bằng tên và tự lưu đã chốt; tiếp tục soạn US/BR về đầu vào, mở lại bản dự toán và lưu tiến độ. | Một số quy tắc đầu vào và luồng lỗi còn mở; quyền/gói/lượt cần đối chiếu tài liệu hiện có. |
| Gửi thông tin sang AI | Mô tả mục tiêu nghiệp vụ, dữ liệu cần bàn giao và các quy tắc lượt đã có. | API, xác thực dịch vụ, định dạng yêu cầu, cách gửi ảnh, giới hạn và cơ chế chống xử lý lặp của bên AI. |
| Theo dõi AI và nhận kết quả | Làm rõ hành vi khách quan sát được khi đang xử lý, thành công, thất bại và thử lại theo quy tắc hiện có. | Cách nhận trạng thái/kết quả, mã lỗi, thời gian xử lý và danh sách đầu ra bắt buộc. Chưa chọn webhook, polling hoặc cơ chế khác. |
| Dự toán và hồ sơ | Ghi nhận cấu trúc kết quả trên trang mẫu và quyền truy cập kết quả. | Cấu trúc phản hồi thật, cách bàn giao ảnh/bản vẽ, tệp PDF/Excel hoặc dữ liệu để xuất tệp, thời hạn truy cập tệp. |
| Kiểm chứng tích hợp thật | Xác định phạm vi kiểm chứng tổng quát. | Môi trường thử, quyền truy cập và dữ liệu mẫu từ bên AI. Chưa thể xác nhận luồng xuyên suốt đã chạy thành công. |

Danh sách phụ thuộc trên phục vụ lập kế hoạch, chưa phải yêu cầu đã gửi cho bên AI hoặc hợp đồng được hai bên thống nhất. Chưa liên hệ bên thứ ba.

## Cần làm rõ

Yêu cầu mới về danh mục và phong cách đang được trao đổi:

- Bộ kết quả AI không bị cắt theo cấu hình lựa chọn. Cách gửi nhóm không có lựa chọn sang AI còn chờ hợp đồng; không tự chọn phong cách mặc định.
- Danh mục phong cách ban đầu đã chốt do Admin tự nhập và gán theo loại trước khi khách sử dụng; ảnh bắt buộc JPG/PNG/WebP tối đa 5 MB. Xóa/ngừng dùng loại công trình hoặc phong cách chưa thuộc phạm vi được giao, không coi là quyết định bắt buộc để hoàn tất phần thêm/sửa hiện tại.
- Đã bổ sung STORY-PROJ-005 cho quản trị danh mục theo phần đã xác nhận. Người dùng đã chốt bộ US/BR gồm phần mở rộng quản trị này. Các ràng buộc kỹ thuật chưa có giá trị cụ thể vẫn được giữ rõ, không tự điền thành nghiệp vụ mới.

Các điểm đầu vào còn mở:

- Đã chốt tên bản dự toán bắt buộc, tối đa 200 ký tự sau khi bỏ khoảng trắng đầu/cuối. Đã chốt không thêm ghi chú riêng, chỉ giữ Mô tả chi tiết tối đa 500 ký tự để gửi AI. Ảnh minh họa phong cách đã chốt bắt buộc một ảnh JPG/PNG/WebP tối đa 5 MB.


- Các thông tin phân công ngoài Reviewer/Approver vẫn để chưa xác định khi chưa có nguồn; giới hạn tên bản dự toán và việc không thêm ghi chú riêng đã chốt.
- Định dạng bàn giao tệp từ AI vẫn chờ hợp đồng tích hợp; cách nhập người nhận email đã chốt là một địa chỉ mỗi lần gửi.

AI service đã được xác nhận là phần do bên khác phụ trách, chờ tích hợp. Tiếp tục làm rõ cách lưu và các quy tắc quản lý bản dự toán độc lập với API của bên AI; không hỏi lại các quyết định đã chốt.

Phụ thuộc và phần thiết kế cần tiếp tục xử lý:

- Nội dung bắt buộc của bản vẽ, phối cảnh, dự toán và hồ sơ cần bên AI xác nhận trong hợp đồng. PDF, Excel, link, QR và email đã thuộc phạm vi.
- Hợp đồng AI service: cách biểu diễn diện tích, loại công trình, vật liệu/nội thất và địa chỉ; cấu trúc dự toán, đơn vị tiền, ảnh/bản vẽ và tệp đầu ra. Đơn giá, khối lượng và số tiền do AI service chịu trách nhiệm, không yêu cầu người dùng chốt công thức cho backend BMT.
- Phạm vi Story đã chốt không có bước nhân viên duyệt chuyên môn trước khi giao kết quả. Danh sách đầu ra và giới hạn sử dụng cần bên AI xác nhận; không tự bổ sung luồng phê duyệt mới.
- Đã chốt tự lưu bản nháp thiếu dữ liệu, chủ động gửi AI và cách xử lý khi đổi loại công trình/tỉnh thành. Đã chốt cách thử lại sau lỗi tự lưu do mạng/máy chủ và giữ ảnh cũ đến khi thay thành công; các quyết định được ghi tại BR-PROJ-002, BR-PROJ-003 và STORY-PROJ-001.
- Không thêm ghi chú riêng; không hỏi lại giới hạn ảnh đầu vào, mô tả thiết kế hoặc cấu hình tầng/tum đã chốt. Đã chốt bản nháp cũ không được chọn loại mới thêm sau thời điểm tạo.
- Các phụ thuộc vào module quyền/gói/lượt chưa có trong backend hiện tại.

Chưa ghi các câu trả lời còn thiếu thành Business Rule hoặc tiêu chí nghiệm thu đã chốt.

## Kiểm tra bản nháp và bàn giao

- Lúc bàn giao bộ US/BR, năm Story có 49 tiêu chí nghiệm thu; sau các lần bổ sung ngày 21/09 và 25/09/2026, hiện có 53. Các Story dẫn tới bảy BR mới và dùng lại quy tắc quyền/gói/lượt hiện có.
- Đã kiểm tra cấu trúc heading, mã trùng trong từng Story, giới hạn độ dài AC, metadata Reviewer/Approver, mã đích tham chiếu trực tiếp và đường dẫn Markdown của bộ PROJ. Lần kiểm tra không phát hiện lỗi ở các mục này; chưa chạy importer hoặc test ứng dụng.
- BR-SUB-007 được bổ sung quyền xuất Excel và quản lý chia sẻ sau khi gói hết hạn. ST-PROJ-035 và ST-PROJ-046 đã đặc tả phần mở rộng; TDD và các tham chiếu ngoài PROJ vẫn cần rà soát tiếp. Không coi kiểm tra mã tồn tại là đã xác minh nội dung toàn bộ chuỗi.
- Người dùng đã chốt US/BR trong hội thoại; đã tạo ST-PROJ-001 đến ST-PROJ-060, rồi bổ sung ST-PROJ-061 đến ST-PROJ-070 ngày 25/09/2026. Các mục chờ hợp đồng AI, thiết kế tích hợp và phân công vẫn được giữ rõ. Tất cả ST là đặc tả Draft chưa chạy, không phải kết quả Pass.
- Điều kiện viết ST của skill prepare-feature đã được đáp ứng bằng xác nhận rõ của người dùng. Chưa import, publish, gán tài khoản hoặc thay trạng thái phê duyệt trên hệ thống quản lý tài liệu.

Bảng [truy vết System Test](estimate-system-test-coverage.md) liệt kê từng ca, liên kết AC/luồng và các điều kiện chưa thể thực thi. Chốt US/BR không tự chốt những giá trị kỹ thuật còn trống hoặc đồng nghĩa cho phép triển khai code.
