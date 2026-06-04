# BA_outsource-_skill
Hướng dẫn sử dụng hệ thống Skill viết tài liệu SRS
Tổng quan
Hệ thống gồm 4 skill chuyên biệt, chạy tuần tự theo luồng:

BRD_analyst → SRS_writer → SRS_test_review → Mapping_API_DB
Mỗi skill đóng một vai trò khác nhau. Output của skill trước là input của skill sau.

Skill 1: BRD_analyst
Vai trò: Business Analyst

Dùng khi: Bạn có tài liệu yêu cầu thô (BRD, CR, meeting note, danh sách chức năng) và cần làm rõ trước khi viết SRS.

Skill này sẽ làm:

Đọc và phân tích tài liệu đầu vào
Hỏi từng phần để làm rõ yêu cầu — không hỏi dồn một lúc
Tổng hợp output gồm 4 phần: Tổng quan module, Danh sách chức năng, Chi tiết từng chức năng, Điểm cần xác nhận
Nhắc bạn chuyển sang SRS_writer khi hoàn thành
Cách gọi: Nhắn /brd-analyst hoặc nói "Phân tích tài liệu này giúp tôi"

Input bạn cần cung cấp:

File BRD, CR, meeting note, hoặc mô tả trực tiếp yêu cầu
Output nhận được:

Tài liệu yêu cầu đã được làm rõ, sẵn sàng để viết SRS
Skill 2: SRS_writer
Vai trò: Business Analyst

Dùng khi: Đã có output từ BRD_analyst và muốn bắt đầu viết tài liệu SRS.

Skill này sẽ làm:

Hỏi bạn có tài liệu bổ sung nào không (mockup, API doc...)
Đọc SRS template do bạn cung cấp, liệt kê các section
Lập bảng mapping từ BRD vào đúng section SRS trước khi viết
Viết từng phần, chờ bạn xác nhận xong mới viết tiếp
Xuất file SRS dạng Markdown
Nhắc bạn chuyển sang SRS_test_review khi hoàn thành
Cách gọi: Nhắn /srs-writer hoặc nói "Viết SRS dựa trên tài liệu này"

Input bạn cần cung cấp:

Output từ BRD_analyst
SRS template (file .docx, Google Docs link, hoặc định dạng khác)
Output nhận được:

Tài liệu SRS hoàn chỉnh dạng file Markdown
Skill 3: SRS_test_review
Vai trò: Tester

Dùng khi: Đã có tài liệu SRS từ SRS_writer và muốn kiểm tra tính đầy đủ trước khi bàn giao cho Dev.

Skill này sẽ làm:

Đọc từng phần SRS, kiểm tra validate và edge case từng trường
Phân loại vấn đề theo 3 mức:
🔴 Thiếu — phải bổ sung ngay
🟡 Cần làm rõ — mô tả mơ hồ, Dev có thể hiểu sai
🟢 Gợi ý — cải thiện UX, không bắt buộc
Tự động đề xuất nội dung bổ sung cụ thể cho từng vấn đề
Lặp lại review cho đến khi không còn vấn đề 🔴
Nhắc bạn chuyển sang Mapping_API_DB khi tài liệu đạt chuẩn
Cách gọi: Nhắn /srs-test-review hoặc nói "Review tài liệu SRS này giúp tôi"

Input bạn cần cung cấp:

Tài liệu SRS từ SRS_writer
Output nhận được:

Báo cáo review phân loại theo mức độ + đề xuất bổ sung cụ thể
Tài liệu SRS đã được xác nhận đầy đủ
Skill 4: Mapping_API_DB
Vai trò: Developer

Dùng khi: Đã có SRS đạt chuẩn và muốn mapping các trường UI với tài liệu API và Database.

Skill này sẽ làm:

Đọc toàn bộ SRS, list tất cả các trường theo từng chức năng
Xác nhận danh sách với bạn trước khi mapping
Tìm đúng tên API và tên trường trong section Response Success
Tìm đúng tên bảng và tên cột trong tài liệu Database
Ghi chú rõ field tính toán hoặc field chưa xác định — không tự suy đoán
Xuất bảng mapping vào mục riêng, không sửa SRS gốc
Nhắc bạn bàn giao tài liệu cho Dev/BA khi hoàn thành
Cách gọi: Nhắn /mapping-api-db hoặc nói "Mapping API và Database cho tài liệu SRS này"

Input bạn cần cung cấp:

Tài liệu SRS đã review
Tài liệu API (file .docx hoặc .md)
Tài liệu Database (file .docx hoặc .md)
Output nhận được:

Bảng mapping theo format: Tên trường (UI) | Tên API | Tên trường API | Tên bảng | Tên cột
Luồng làm việc đầy đủ
1. Bạn có tài liệu yêu cầu
        ↓
2. Gọi BRD_analyst → làm rõ yêu cầu
        ↓
3. Gọi SRS_writer + cung cấp SRS template → viết tài liệu SRS
        ↓
4. Gọi SRS_test_review → kiểm tra và bổ sung cho đến khi đạt chuẩn
        ↓
5. Gọi Mapping_API_DB + cung cấp tài liệu API & DB → mapping các trường
        ↓
6. Bàn giao tài liệu hoàn chỉnh cho Dev/BA
Lưu ý quan trọng
Không bỏ qua bước nào — mỗi skill phụ thuộc vào output của skill trước
Xác nhận từng phần trước khi cho skill viết tiếp — tránh phải làm lại từ đầu
Không tự suy đoán — nếu skill hỏi lại, hãy trả lời đầy đủ thay vì bỏ qua
Model khuyến nghị:
BRD_analyst: claude-opus-4-8 (phân tích sâu)
SRS_writer, SRS_test_review, Mapping_API_DB: claude-sonnet-4-6 (viết nhanh, hiệu quả)
