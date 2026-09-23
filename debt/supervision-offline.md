# Nợ nghiệp vụ: lịch và lượt kiểm tra thực tế

**Trạng thái: Hoãn — không triển khai hoặc nghiệm thu trong đợt hiện tại.**

Lịch hẹn, số lượt và việc trừ lượt kiểm tra thực tế đang vận hành offline. Nền tảng chưa quản lý các phần này và không yêu cầu nhập số lượt còn lại để hoàn thành gói.

## Phần làm trong đợt hiện tại

**Cập nhật 19/09/2026:** Danh sách cũ dưới đây phải đọc theo [nghiệp vụ thanh toán mới](../discovery/payment-packages.md): được cấp gói chưa gán, hạn gán một năm; nhân viên có quyền riêng được sửa dự án, hủy và khôi phục. Không quản lý trạng thái khảo sát/giám sát để kiểm tra quyền sửa. Nút hoàn thành/mở lại cũ không tự trở thành yêu cầu mới của thanh toán.

- Ghi nhận gói giám sát của khách hàng, gắn cố định với một dự án.
- Mỗi dự án có tối đa một gói giám sát đang thực hiện. Tài khoản có nhiều dự án có thể có nhiều gói riêng.
- Không cấu hình chu kỳ tháng/năm, ngày hết hạn hay tự làm mới lượt cho gói giám sát.
- Nhân viên phụ trách được xác định theo phân công dự án, không có phân công riêng theo gói. Đây là phân quyền quản lý dự án, không phải điều phối kỹ sư cho lịch kiểm tra offline.
- Khi dự án xong, Admin hoặc nhân viên phụ trách bấm **Hoàn thành gói giám sát** cho đúng khách hàng, dự án và gói. Không tự hoàn thành các gói khác của khách hàng.
- Khách hàng và nhân viên không phụ trách không được hoàn thành gói. Nếu bấm nhầm, Admin hoặc nhân viên phụ trách được mở lại, bắt buộc ghi lý do và không làm phát sinh hai gói đang thực hiện trên cùng dự án. Không tự bổ sung luồng tự động hoàn thành từ trạng thái dự án.

## Phần nợ cần quay lại sau

- Đặt, xác nhận, đổi và hủy lịch; phân công kỹ sư; thông báo và bằng chứng hoàn thành buổi kiểm tra.
- Số lượt được cấp, số lượt còn lại, giữ/trừ/hoàn lượt và lịch sử sử dụng trên nền tảng.
- Nhập hoặc đối chiếu dữ liệu đã vận hành offline khi chuyển lên nền tảng.
- Chính sách hủy sát giờ, vắng mặt, buổi kiểm tra dở dang, lịch còn mở khi hoàn thành gói và quyền thao tác của các bên.

## Quyết định đã trao đổi, giữ để tham khảo khi mở lại

- Một buổi kỹ sư đến kiểm tra thực tế tại công trình tương ứng một lượt.
- Đã chọn giữ lượt khi lịch được xác nhận, tính lượt khi buổi kiểm tra hoàn thành. Quy tắc này hiện hoãn, không phải yêu cầu triển khai đợt này.
- Không dùng lại giả định chu kỳ, hết hạn hoặc gia hạn cho giám sát: gói hiện được xác định theo vòng đời dự án.

## Tài liệu liên quan và giới hạn nghiệm thu

- [STORY-SUB-003](../userstory/STORY-SUB-003.md): phần theo dự án; AC-004 và AC-005 được hoãn, không thuộc nghiệm thu hiện tại.
- [BR-SUB-010](../businessrule/BR-SUB-010.md): giữ/trừ lượt, đã đánh dấu hoãn.
- [ST-SUB-024](../systemtest/ST-SUB-024.md), [ST-SUB-025](../systemtest/ST-SUB-025.md): đặc tả hoãn, không chạy hoặc tính vào điều kiện bàn giao hiện tại.
- [BR-SUB-011](../businessrule/BR-SUB-011.md): vòng đời gói và nút hoàn thành thuộc đợt hiện tại.

Chỉ mở lại phần nợ khi người dùng yêu cầu. Khi đó đọc lại các quyết định này, chốt phần còn thiếu và cập nhật Story, BR, test trước khi triển khai. Chưa có thời hạn hay người phụ trách được giao.
