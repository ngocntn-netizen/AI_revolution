# GHN AI Platform · AI Experimentation OS (MVP)

Bản MVP cho Checkpoint 1, track SPARK. Prototype một file HTML, dữ liệu hard code, không có backend.

- Bản chạy: mở `index.html` bằng trình duyệt, hoặc qua GitHub Pages của repo này.
- Tài khoản demo: bấm "Vào demo" ở màn đăng nhập, không cần mật khẩu.

## Sản phẩm

GHN AI Platform là nền tảng nội bộ để nhân viên giao việc cho AI agent qua khung chat. Agent làm việc trên tài nguyên dùng chung của công ty, và mọi hành động ghi vào hệ thống thật đều cần người duyệt.

- **Workspace:** mỗi người có workspace riêng, tham gia được workspace chung của team.
- **Skill:** cách làm một loại việc. Có skill riêng và library public.
- **Knowledge base:** KB riêng và KB public dùng chung.
- **MCP connector:** riêng hoặc dùng chung cho mọi workspace. Mỗi người vẫn phải kết nối bằng tài khoản của mình.
- **Workflow:** mini app có input, các bước xử lý và output. Dùng riêng hoặc publish cho cả công ty.
- **Eval:** skill eval_setup hướng dẫn từng bước: dataset ground truth, đưa trace hội thoại vào dataset, cấu hình tiêu chí và rule, báo cáo, đề xuất chỉnh workflow.
- **Policy guardrail agent:** super admin ban hành policy và quy định loại workflow nào cần policy nào. Guardrail agent kiểm tra khi publish và khi chạy.

Bài demo: cài bảng giá LTL. Agent đọc bảng giá và hợp đồng của khách, hỏi hệ thống cài giá B2B cần cấu hình gì, sinh nháp, hỏi người chỗ không chắc, rồi tạo bảng giá DRAFT sau khi người duyệt. Khách đổi giá thì chỉ cần nhắn một câu.

## Kịch bản xem demo

1. Đăng nhập bằng tài khoản demo, tham gia workspace GHN Freight.
2. Ở màn chat, chọn "Cài bảng giá LTL cho Con Cưng". Kết nối tài khoản B2B khi được hỏi.
3. Trả lời 2 câu agent hỏi, duyệt và tạo DRAFT. Output hiện ở khung bên phải.
4. Thử "Cập nhật giá bằng chat".
5. Vào Workflows, mở "Tóm tắt điều khoản giá", bấm Publish. Guardrail chặn vì chưa có eval. Bấm "Set up eval" và đi hết 5 bước, rồi publish lại.
6. Vào Policy guardrail, đổi vai trò sang Super admin để ban hành policy.

## Hình thức triển khai thực tế

- Web app nội bộ, đăng nhập bằng SSO nhân viên. GHN tự build, dùng lại chuẩn mở: Claude Agent SDK, SKILL.md, MCP.
- Tháng 10/2026: demo end-to-end bài cài giá LTL trên dữ liệu đã che, hệ thống B2B dùng API giả lập theo FT-4031, FT-4032.
- Sau đó thay bằng API thật của hệ thống cài giá B2B, chạy eval trên bộ golden 7 bảng giá, 619 tuyến, rồi chạy shadow mode song song với BD trước khi cho agent tạo nháp.
- Mở cho các team khác đưa workflow lên, bắt đầu từ workflow lên đơn B2B từ file booking.
