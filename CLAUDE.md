# Hướng dẫn viết tài liệu BMT

## Bắt buộc dùng skill viết rõ ràng khi soạn tài liệu

Trước khi viết mới hoặc sửa bất kỳ tài liệu nào, agent phải đọc và áp dụng skill `vietnamese-clear-writing`. Quy định này áp dụng cả khi người dùng không gọi tên skill, khi nội dung được tạo trong một tác vụ lập trình, và khi tài liệu được trả trong hội thoại thay vì lưu thành file.

Phạm vi gồm User Story, Business Rule, TDD, đặc tả test, README, hướng dẫn, kế hoạch, báo cáo, nội dung skill và các văn bản khác. Với nội dung tiếng Việt, viết rõ ràng và tự nhiên như người Việt giải thích cho nhau: đưa ý chính lên trước, dùng từ quen thuộc và giải thích thuật ngữ khi cần. Tôn trọng ngôn ngữ người dùng yêu cầu; không tự dịch tài liệu sang tiếng Việt.

- Đọc toàn bộ `SKILL.md` trước lần viết tài liệu đầu tiên trong phiên; nếu đã đọc và nội dung không đổi thì áp dụng tiếp, không cần đọc lại mỗi lần.
- Khi dùng skill chuyên môn, kết hợp cả hai: skill chuyên môn quy định nội dung và cấu trúc, `vietnamese-clear-writing` quy định cách diễn đạt. Không bỏ qua skill viết vì đã dùng một skill khác.
- Trước khi bàn giao, đọc lại theo phần “Đọc lại trước khi bàn giao” trong skill và sửa các câu khó hiểu. Giữ nguyên dữ kiện, điều kiện, ngoại lệ, mức độ bắt buộc, mã, liên kết và cấu trúc mẫu.
- Nếu không tìm thấy skill ở đường dẫn được chỉ định, tìm bản còn lại trong dự án. Nếu vẫn không đọc được, báo rõ phần bị thiếu; không coi bước áp dụng skill đã hoàn tất.

Đường dẫn skill tính từ thư mục tài liệu này:

- Bản chuẩn: `../bmt-be/.claude/skills/vietnamese-clear-writing/SKILL.md`.
- Bản đồng bộ cho Codex: `../bmt-be/.codex/skills/vietnamese-clear-writing/SKILL.md`.

Đọc template tương ứng trong `templates/` trước khi soạn hoặc sửa tài liệu. Giữ cấu trúc của template và áp dụng skill viết cho phần diễn đạt.

## Trao đổi và xác nhận nghiệp vụ khi bàn luận tính năng

Khi người dùng muốn bàn luận, phân tích hoặc chuẩn bị một tính năng, agent phải trao đổi qua nhiều lượt để xác định đúng nghiệp vụ. Không chỉ hỏi một lần rồi tự suy diễn các quyết định còn lại.

- Trước mỗi lượt hỏi, đọc yêu cầu, câu trả lời trước đó và tài liệu liên quan. Nói ngắn gọn agent đang hiểu gì, điểm nào chưa rõ và vì sao cần người dùng quyết định.
- Mỗi lượt tập trung vào một nhóm vấn đề, thường từ 1 đến 3 câu hỏi cụ thể. Ưu tiên mục tiêu và người dùng trước, rồi đến luồng chính, quy tắc, quyền, trạng thái, ngoại lệ và tiêu chí nghiệm thu. Chỉ hỏi những phần phù hợp với tính năng đang bàn.
- Khi có nhiều cách xử lý, đưa lựa chọn dễ hiểu, giải thích ngắn tác động và đề xuất phương án nếu có căn cứ. Dùng một tình huống thực tế để kiểm tra cách hiểu; không tự chọn thay người dùng đối với quyết định nghiệp vụ còn mở.
- Sau mỗi câu trả lời, cập nhật các điểm đã chốt. Nếu câu trả lời làm phát sinh nhánh mới hoặc mâu thuẫn với tài liệu, chỉ rõ phần đó rồi hỏi tiếp. Tiếp tục cho đến khi các quyết định cần thiết của phạm vi đang bàn đã rõ.
- Giữ ba nhóm thông tin: **Đã xác nhận**, **Đề xuất chưa chốt**, **Cần làm rõ**. Chỉ chuyển một đề xuất sang đã xác nhận khi người dùng trả lời rõ; im lặng, hết thời gian chờ hoặc lựa chọn được chọn sẵn không phải là xác nhận.
- Tôn trọng câu trả lời và yêu cầu rõ ràng đã có. Không hỏi lại điều đã chốt, không yêu cầu người dùng xác nhận mọi câu hoặc quyết định kỹ thuật thông thường. Chỉ mở lại một điểm khi có thông tin mới làm thay đổi nghiệp vụ, và giải thích lý do.
- Trước khi coi phần nghiệp vụ mới đã thống nhất, đưa bản tóm tắt ngắn về phạm vi, luồng, quy tắc và ngoại lệ. Hỏi xác nhận những điểm tổng hợp hoặc suy ra chưa được người dùng chốt; nếu tất cả đã được xác nhận rõ trong các lượt trước thì chỉ cần tóm tắt, không yêu cầu duyệt lại.
- Trong lúc chờ, tiếp tục đọc tài liệu, khảo sát code hoặc chuẩn bị phần độc lập. Phần phụ thuộc vào quyết định chưa có câu trả lời phải để mở; không ghi thành quy tắc đã chốt hoặc triển khai dựa trên phỏng đoán.
- Xác nhận nghiệp vụ không phải xin phép thao tác. Nếu người dùng đã giao cả triển khai, tiếp tục phần đủ thông tin trong phạm vi được giao; nếu chỉ đang bàn luận thì bàn giao nội dung đã thống nhất, chưa tự chuyển sang viết code.

