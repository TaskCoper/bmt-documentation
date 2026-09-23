# Nợ nghiệp vụ: so sánh kết quả thiết kế

**Trạng thái: Hoãn — không triển khai hoặc nghiệm thu tính năng so sánh trong đợt hiện tại.**

Người dùng chọn hoãn chức năng đặt hai kết quả thiết kế cạnh nhau để đối chiếu. Đợt này khách vẫn mở xem từng kết quả riêng theo quyền truy cập đã chốt.

## Phần vẫn làm trong đợt hiện tại

- Khách mở xem từng kết quả thuộc tài khoản mà mình có quyền truy cập.
- Gói thiết kế hết hạn vẫn được xem kết quả cũ theo BR-SUB-007.
- Quyết định hoãn không thay đổi cách tạo thiết kế, tính lượt, tải tệp hoặc xuất PDF đã chốt.

## Phần hoãn

- Giao diện chọn và đặt các kết quả cạnh nhau để so sánh.
- Quyền dùng tính năng so sánh theo gói; không đưa quyền này vào danh mục đang triển khai.
- Tiêu chí so sánh, số kết quả được chọn, phạm vi giữa các dự án và cách tính lượt nếu có.

## Cần chốt khi mở lại

- So sánh những kết quả nào và theo tiêu chí nào.
- Quyền dùng chung hay chỉ dành cho một số gói.
- Chỉ đối chiếu dữ liệu đã có hay cần AI phân tích; không tự thêm tác vụ AI hoặc loại lượt mới.

## Tài liệu liên quan

- [BR-SUB-008](../businessrule/BR-SUB-008.md): danh mục quyền hiện tại.
- [BR-SUB-007](../businessrule/BR-SUB-007.md): quyền xem kết quả cũ sau khi hết hạn.
- [STORY-SUB-001](../userstory/STORY-SUB-001.md), [STORY-SUB-002](../userstory/STORY-SUB-002.md): sử dụng và cấu hình quyền lợi gói.
- [ST-SUB-013](../systemtest/ST-SUB-013.md): mở xem một kết quả cũ khi gói hết hạn, không phụ thuộc tính năng so sánh.

Chỉ mở lại khi người dùng yêu cầu. Chưa có lịch triển khai hoặc người phụ trách được giao. Mô tả so sánh dành cho PLUS/PRO trên trang tham khảo không phải quyền phải triển khai trong đợt này.
