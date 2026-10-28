# AGENT_TASKS.md — Phân công và phối hợp công việc

> **Chỉ quản lý ai làm việc gì.** Danh sách task, phụ thuộc, trạng thái nghiệm thu và bằng chứng thuộc `TASK.md`; yêu cầu thuộc `TODO.md`. Không sao chép toàn bộ task vào đây.
>
> **Chưa kích hoạt multi-agent.** Khoa và ChatGPT sẽ cùng bàn mô hình đội ngũ, số agent, trách nhiệm và ngân sách token trước khi phân công. File này là mẫu quản lý, không tự yêu cầu spawn agent.

## (1) Trách nhiệm của các tài liệu

- [ ] (1.1) `TODO.md`: nguồn đặc tả về chức năng/hành vi.
- [ ] (1.2) `TASK.md`: nguồn duy nhất cho task ID, trạng thái, phụ thuộc và bằng chứng hoàn thành.
- [ ] (1.3) `AGENT_TASKS.md`: ai nhận task nào, thời điểm bàn giao, xung đột phạm vi, tài nguyên/tokens dự kiến.
- [ ] (1.4) `AGENTS.md` (nếu tạo sau khi bàn bạc): quy tắc phối hợp và thẩm quyền của đội; không tự tạo ở lượt này.

## (2) Danh sách phân công hiện tại

| Task ID | Agent/Người phụ trách | Phạm vi file | Trạng thái phân công | Bàn giao | Ghi chú |
|---|---|---|---|---|---|
| Chưa giao | — | — | Unassigned | — | Đợi Khoa chốt cơ chế multi-agent |

**Phân biệt:** Trạng thái phân công (`Unassigned / Reserved / Assigned / Handoff / Released`) ở đây **khác** trạng thái thực thi (`planned / in_progress / blocked / completed / failed`) trong `TASK.md`. Không sao chép status thực thi sang đây.

## (3) Quy trình phân công dự kiến — chưa áp dụng tự động

- [ ] (3.1) Chọn một Task ID từ `TASK.md` và xác minh dependency đã đạt; không tự đoán bước tiếp theo.
- [ ] (3.2) Ước lượng phạm vi và chi phí công việc. Nếu một agent đủ xử lý và kiểm thử, ưu tiên một agent để tránh tốn token phối hợp.
- [ ] (3.3) Chỉ giao việc cho nhiều agent khi Khoa đã đồng ý cách tổ chức; mô tả đầu ra, ranh giới file, mức quyền và điểm bàn giao của từng người.
- [ ] (3.4) Trước khi giao, kiểm tra có agent khác giữ cùng file/task không; không giao hai agent sửa cùng một file đồng thời.
- [ ] (3.5) Ghi `Assigned` và tên agent khi **thực sự** đã giao; không tạo agent giả trong bảng.
- [ ] (3.6) Agent báo kết quả, lỗi và bằng chứng về người điều phối; người chịu trách nhiệm nghiệm thu cập nhật `TASK.md`.
- [ ] (3.7) Khi task được nghiệm thu hoặc thu hồi, cập nhật `Released`, không tự đánh dấu `completed` chỉ vì agent báo đã code xong.

## (4) Thông tin phải có trong một lượt giao việc

- **Task ID:** ID đúng ở `TASK.md`.
- **Mục tiêu:** Một kết quả có thể kiểm chứng, tham chiếu TODO khi cần.
- **Phạm vi được phép:** File/thư mục, hành động, quyền commit/push (nếu được duyệt).
- **Không được làm:** Những phần ngoài scope hoặc có owner khác.
- **Phụ thuộc/đầu vào:** Task, branch, artifact và thông tin còn thiếu.
- **Đầu ra và bằng chứng:** File thay đổi, kiểm thử, log, commit/ref nếu có.
- **Bàn giao:** Ai review, điều kiện nghiệm thu, trường hợp phải hỏi Khoa.
- **Dự toán chi phí:** Ước lượng số lượt agent/độ khó; không coi là số token chính xác.

## (5) Các quyết định sẽ bàn với Khoa trước khi dùng multi-agent

- [ ] (5.1) Có cần multi-agent cho giai đoạn A hay một agent là đủ?
- [ ] (5.2) Giới hạn số agent hoạt động cùng lúc và ngân sách token/chi phí.
- [ ] (5.3) Chọn vai trò điều phối, thực hiện, kiểm thử, review — có cần tách vai trò hay một người kiêm nhiệm?
- [ ] (5.4) Khi nào được phân việc song song, khi nào phải tuần tự theo dependency?
- [ ] (5.5) Quy ước nhánh Git/worktree, tránh hai người ghi cùng file và cách tích hợp thay đổi.
- [ ] (5.6) Ai có quyền commit, push, tạo PR, merge; có cần Khoa xác nhận từng bước không?
- [ ] (5.7) Khi agent hết ngân sách, cần thêm quyền hoặc gặp yêu cầu chưa rõ thì bàn giao thế nào?
- [ ] (5.8) Cách đo hiệu quả multi-agent so với làm tuần tự: thời gian, lỗi, token và số task thực sự nghiệm thu.

## (6) Chú ý

- Không đặt danh sách task triển khai lặp lại vào file này; chỉ tham chiếu Task ID.
- Không tạo `AGENTS.md` cho đến khi Khoa duyệt quy tắc đội ngũ.
- Không tự kích hoạt agent hoặc tiêu token chỉ để hoàn thành bảng phân công.
- Khi công việc làm thay đổi chức năng, cập nhật `TODO.md` trước, rồi cập nhật kế hoạch `TASK.md`; sau cùng mới phân công ở đây.
