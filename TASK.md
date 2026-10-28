# TASK.md — Danh sách công việc triển khai nhaififai

> **Nguồn tiến độ duy nhất** cho AI coding agents. `TODO.md` là đặc tả sản phẩm; `AGENT_TASKS.md` chỉ ghi phân công. Không đánh dấu hoàn thành chỉ vì code compile.

## Quy tắc thực thi

- Đọc các mục `Source Section` trong `TODO.md` trước khi làm; nếu yêu cầu mâu thuẫn hoặc đánh dấu `[?]`, báo Khoa và chỉ dừng nhánh bị chặn.
- Chỉ chọn task khi mọi `Depends On` đã hoàn thành. Mỗi lần lấy một task có thể kiểm thử được; không tự mở rộng scope.
- Trước khi sửa kiểm tra branch `master`, HEAD, file thay đổi và những công việc đang được phân công; không ghi đè công việc của agent khác.
- Mọi task cần bằng chứng: lệnh kiểm thử/kết quả thực tế, môi trường, commit hoặc log; nếu chưa test được thì `blocked`, không phải `completed`.
- Trạng thái hợp lệ: `planned`, `in_progress`, `blocked`, `completed`, `failed`; cập nhật một nơi duy nhất tại đây.
- `AGENT_TASKS.md` tham chiếu Task ID ở file này; không duy trì bản sao status/steps ở đó.
- Mốc Phase A phải hoàn thành trước các giai đoạn khai thác nâng cao. Preset an toàn/Strict là mặc định tạm thời, không thay quyết định của Khoa.
- Các task liên quan API/phiên bản phải xác minh trên JAR/source đúng mục tiêu; không đoán API. Không tự push/release nếu chưa được yêu cầu.

## Các nhiệm vụ theo thứ tự phụ thuộc

### Task 1.1 — Chốt phạm vi addon

- **Source Section:** (1.1,1.3,1.11)
- **Depends On:** none
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Xác nhận client-side Meteor/Fabric, không sửa Baritone gốc, không yêu cầu plugin server.
- **Implementation Steps:** Ghi rõ phạm vi và ranh giới can thiệp; kiểm tra mâu thuẫn giữa các tài liệu.
- **Done when:** Phạm vi nhất quán trong TODO và README.
- **Validation:** Đối chiếu tài liệu và danh sách thành phần.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 1.2 — Chốt định danh addon

- **Source Section:** (1.2)
- **Depends On:** Task 1.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Chọn tên hiển thị, mod ID, Java package, tên JAR, versioning.
- **Implementation Steps:** Đề xuất tên; hỏi Khoa nếu lựa chọn ảnh hưởng nhận diện; cập nhật metadata cùng tài liệu khi được duyệt.
- **Done when:** Một bộ định danh nhất quán.
- **Validation:** So sánh build metadata, README và tên artifact.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 1.3 — Chốt chính sách MVP còn mở

- **Source Section:** (1.4–1.9)
- **Depends On:** Task 1.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Quyết định xử lý mép, calibration, fallback, ngưỡng an toàn, preset, lệnh.
- **Implementation Steps:** Tổng hợp từng lựa chọn; nêu mặc định; yêu cầu Khoa xác nhận các quyết định có hậu quả phá/xây.
- **Done when:** Không còn quyết định bắt buộc chưa duyệt trước khi chạy mining.
- **Validation:** Bảng quyết định và kiểm tra tính nhất quán với TODO.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 2.1 — Xác minh nguồn và phiên bản

- **Source Section:** (2.1–2.6)
- **Depends On:** Task 1.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Khóa Minecraft 1.21.11, Meteor build 82, Baritone 1.17.0, Fabric/JDK 21 và dependency phù hợp.
- **Implementation Steps:** Kiểm tra branch master, JAR thật, full SHA/checksum, API; ghi lại dependency pin và sai lệch.
- **Done when:** Ma trận phiên bản đã kiểm chứng; thiếu artifact được đánh dấu blocked.
- **Validation:** Đọc metadata JAR và thử resolve dependency.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 2.2 — Khởi tạo cấu trúc build

- **Source Section:** (2.7–2.10)
- **Depends On:** Task 1.2, Task 2.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Tạo Gradle/Fabric skeleton, resource, metadata, wrapper, script build.
- **Implementation Steps:** Chọn cấu trúc; tạo file tối thiểu; cấu hình dependency compile/runtime; không bundle Baritone.
- **Done when:** Build tạo JAR chứa metadata đúng.
- **Validation:** Gradle compile, kiểm tra JAR và log lỗi.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 3.1 — Entrypoint và lifecycle

- **Source Section:** (3.1,3.7–3.8)
- **Depends On:** Task 2.2
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Khởi động addon trong Meteor; đăng ký module/command/event và cleanup.
- **Implementation Steps:** Tạo entrypoint; đăng ký thử một module và status command; xử lý disconnect/world switch.
- **Done when:** Client tải addon và dừng sạch.
- **Validation:** Launch dev client, xem startup log.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 4.1 — BaritoneAdapter kiểm tra khả dụng

- **Source Section:** (4.1,4.6,4.8–4.9)
- **Depends On:** Task 2.1, Task 3.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Kiểm tra API và quyền điều khiển path/goal/cancel của Baritone.
- **Implementation Steps:** Nhận biết thiếu/sai phiên bản; quy định ownership; chuyển request/status; xử lý timeout/cancel.
- **Done when:** Adapter không crash nếu Baritone thiếu hoặc đang bận.
- **Validation:** Test thiếu Baritone, xung đột goal và trạng thái path.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 4.2 — Đọc selection qua API

- **Source Section:** (4.2–4.5,4.7)
- **Depends On:** Task 4.1
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Đọc một/nhiều vùng qua manager, giữ min/max và giới hạn inclusive.
- **Implementation Steps:** Xác minh API; đọc bbox; xử lý vùng rỗng/lớn/chưa load/đổi selection.
- **Done when:** Selection được chuyển cho planner đúng tọa độ, không chỉnh vùng gốc.
- **Validation:** Unit test vùng âm, single/multi/no selection; dev-client smoke test.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 15.5 — Cổng nghiệm thu Phase A

- **Source Section:** (15.5)
- **Depends On:** Task 3.1, Task 4.1, Task 4.2
- **Priority:** P0
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Chốt skeleton, adapter, status/command và selection read trước chức năng mining.
- **Implementation Steps:** Chạy lại toàn bộ smoke test giai đoạn A; ghi lỗi/giới hạn.
- **Done when:** Build và boot thành công, đọc selection chính xác.
- **Validation:** Ghi phiên bản, lệnh test, log, commit và kết quả.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 3.2 — Task engine và state machine

- **Source Section:** (3.2–3.6,3.9–3.10)
- **Depends On:** Task 15.5
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Xây dựng task model, executor, observer, queue, ownership và checkpoint.
- **Implementation Steps:** Mô tả trạng thái; triển khai chuyển trạng thái; timeout/retry; log; cancel và pause/resume.
- **Done when:** Không có tác vụ đồng thời tranh input; trạng thái có thể quan sát.
- **Validation:** Unit test transition, cancel, disconnect, resume.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 12.1 — SafetyPolicy cơ sở

- **Source Section:** (12.1–12.7)
- **Depends On:** Task 3.2
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Định nghĩa Safe/Balanced/Fast và các điều kiện dừng bắt buộc.
- **Implementation Steps:** Áp ngưỡng health/durability/hazard; reach/cooldown; profile server; emergency stop.
- **Done when:** Mọi thao tác đi qua safety gate; có thể dừng khẩn cấp.
- **Validation:** Test các ngưỡng và cleanup input.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 5.1 — Nhận diện công cụ

- **Source Section:** (5.1–5.2)
- **Depends On:** Task 15.5
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Phân biệt cúp thường, 3×3, unknown và hư hỏng.
- **Implementation Steps:** Đọc components/lore/tool fingerprint; tạo phân loại theo server.
- **Done when:** Unknown không bị nhận nhầm là 3×3 đã xác minh.
- **Validation:** Test tool khác nhau và tình huống thiếu item.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 5.2 — Calibration profile 3×3

- **Source Section:** (5.3–5.10)
- **Depends On:** Task 5.1, Task 12.1, Task 1.3
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Học pattern đào thực tế theo face, yaw/pitch và sneak.
- **Implementation Steps:** Chọn vùng thử, snapshot trước/sau, một cú đào mỗi thử, so sánh lặp lại và lưu độ tin cậy.
- **Done when:** Chỉ profile ổn định mới được sử dụng.
- **Validation:** Test calibration bất thường, server update trễ, profile Unknown.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 6.1 — Snapshot world và hình học selection

- **Source Section:** (6.2–6.3,6.7)
- **Depends On:** Task 4.2, Task 3.2
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Xây dựng dữ liệu block đã tải/unknown và phân lớp selection.
- **Implementation Steps:** Đọc snapshot; phân loại air, solid, liquid, unbreakable; map tọa độ và layer.
- **Done when:** Không coi block chưa load là air; không tràn vùng.
- **Validation:** Unit test bbox, layer, holes, chunk unload.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 6.2 — Planner cú đào và mép selection

- **Source Section:** (6.4–6.8,6.16)
- **Depends On:** Task 6.1, Task 5.2, Task 12.1, Task 1.3
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Tính ứng viên 3×3, Strict và fallback cúp thường.
- **Implementation Steps:** Tính pattern; lọc ứng viên ngoài vùng; chọn đường/điểm đứng; xử lý mép.
- **Done when:** Strict không dự kiến phá ra ngoài vùng, phần dư có lý do rõ.
- **Validation:** Unit test vùng 1/2/3/4/10 và tool thiếu.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 6.3 — Preview và xác nhận

- **Source Section:** (6.9,6.15.4–6.15.6,11)
- **Depends On:** Task 6.2
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Cho xem kế hoạch trước bất kỳ hành động phá block nào.
- **Implementation Steps:** Hiển thị block/tâm/vùng thừa; refresh khi selection thay đổi; confirm/hủy.
- **Done when:** Không đào trước confirm; preview cũ không thể được dùng lại.
- **Validation:** Test thay đổi selection và confirm stale.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 6.4 — Execution đào và observer

- **Source Section:** (6.10–6.14,6.15.7–6.15.12)
- **Depends On:** Task 6.3, Task 8.1, Task 3.2, Task 12.1
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Đi tới điểm đứng, kiểm tra lại điều kiện, đào và xác nhận phản hồi server.
- **Implementation Steps:** Mỗi cú đào: goto → check reach/tool → click → wait update → compare → replan/stop.
- **Done when:** Số block hoàn thành dựa phản hồi quan sát, có checkpoint.
- **Validation:** Integration test partial 3×3, lag, tool break, protected.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 8.1 — SmartGoto goal và progress

- **Source Section:** (8.1–8.2,8.9)
- **Depends On:** Task 4.1, Task 3.2, Task 12.1
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Nhận A, kiểm tra goal, giao Baritone, xác nhận đến đích.
- **Implementation Steps:** Tạo request; theo dõi distance/progress; phân biệt path success/actual arrival.
- **Done when:** Không báo đến đích chỉ từ path status.
- **Validation:** Test goal hợp lệ/không hợp lệ/unreachable.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 8.2 — StuckDetector và RecoveryController

- **Source Section:** (8.3–8.8,8.10)
- **Depends On:** Task 8.1
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Phân loại stuck và phục hồi có ngân sách retry.
- **Implementation Steps:** Theo dõi vị trí; revalidate/repath; waypoint; policy phá/đặt; timeout và dừng.
- **Done when:** Không retry vô hạn; phục hồi chỉ thành công nếu có tiến bộ.
- **Validation:** Test lag, rubberband, lá/sculk, path fail.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 14.1 — Cổng nghiệm thu Smart Miner MVP

- **Source Section:** (14.1–14.8,15.6–15.7)
- **Depends On:** Task 6.4, Task 8.2
- **Priority:** P1
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Kiểm thử end-to-end Smart Selection Miner trên world thử nghiệm.
- **Implementation Steps:** Đối chiếu preview với block thực; test pause/resume/cancel/hazard.
- **Done when:** JAR chạy, Strict được tuân thủ trong case kiểm thử, báo partial completion.
- **Validation:** Bảng ca kiểm thử, log và tái hiện lỗi.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 7.1 — Đào top-down zigzag

- **Source Section:** (7.1–7.4)
- **Depends On:** Task 14.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Lập layer/hàng và di chuyển zigzag giữ đường thoát.
- **Implementation Steps:** Chia layer, chọn X/Z, quét đổi hướng; kiểm tra cát/sỏi, sàn đứng.
- **Done when:** Có plan top-down an toàn và cập nhật sau thay đổi world.
- **Validation:** World test nhiều tầng/địa hình.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 7.2 — Tối ưu thứ tự block

- **Source Section:** (7.5–7.7)
- **Depends On:** Task 7.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** So sánh chi phí block, movement, reach, hardness và hazard.
- **Implementation Steps:** Sinh ứng viên đường tiếp cận; chấm điểm; kiểm tra lại sau đào.
- **Done when:** Giảm thao tác thừa mà không vi phạm safety.
- **Validation:** Benchmark nhiều cấu hình.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 9.1 — HazardNavigator tiếp cận mục tiêu thấp

- **Source Section:** (9.1–9.6)
- **Depends On:** Task 8.2, Task 12.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Tạo đường an toàn tới item/quặng dưới hố/vách.
- **Implementation Steps:** Tính staircase/cầu/đường vòng; vật liệu; hạ cánh; abort khi target mất.
- **Done when:** Không đào dưới chân thiếu kiểm chứng; dừng khi hết vật liệu.
- **Validation:** Test rơi, lava, nước, đích biến mất.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 10.1 — Nhặt item whitelist

- **Source Section:** (10.1–10.7)
- **Depends On:** Task 8.1, Task 9.1, Task 3.2
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Theo dõi item, ưu tiên, goto, xác nhận nhặt và resume.
- **Implementation Steps:** Lọc whitelist; kiểm tra inventory; tạm task; theo dõi despawn; resume.
- **Done when:** Không đuổi item không hợp lệ, không tự vứt item.
- **Validation:** Test inventory đầy, item mất, nhiều item.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 11.1 — Task queue và planner local

- **Source Section:** (11.1–11.8)
- **Depends On:** Task 3.2, Task 10.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Điều phối goto/mine/pickup theo chuỗi có phụ thuộc.
- **Implementation Steps:** Queue, priority, precondition, completion, blocked, cancel; giải thích lựa chọn.
- **Done when:** Không chạy bước sau khi bước trước thất bại phụ thuộc.
- **Validation:** Test task chain, interruption và restore.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 13.1 — GUI/command/HUD/config hoàn chỉnh

- **Source Section:** (13.1–13.8)
- **Depends On:** Task 11.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Hoàn thiện module Meteor, commands, preview, HUD, profile config.
- **Implementation Steps:** Đăng ký module; tooltip; lưu cấu hình theo server; validate dữ liệu.
- **Done when:** Các thao tác quan trọng có phản hồi và confirm.
- **Validation:** Dev client UI test và config roundtrip.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 14.2 — Kiểm thử tổng thể và hiệu năng

- **Source Section:** (14.1–14.10)
- **Depends On:** Task 13.1
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Test singleplayer trước, multiplayer được phép sau; đo chi phí/thất bại.
- **Implementation Steps:** Đo path, clicks, latency, block ngoài vùng, tốc độ và lỗi.
- **Done when:** Ma trận test và giới hạn được công bố.
- **Validation:** Bằng chứng test trên version cố định.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 15.1 — Tài liệu và phát hành

- **Source Section:** (15.1–15.4)
- **Depends On:** Task 14.2
- **Priority:** P2
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** README, troubleshooting, version, checksum, changelog và JAR release.
- **Implementation Steps:** Ghi cách build/cài/rollback; kiểm tra artifact; đề nghị người dùng duyệt phát hành.
- **Done when:** Release có JAR xác minh và hướng dẫn.
- **Validation:** Clean install, checksum và changelog.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

### Task 15.2 — Roadmap mở rộng và AI tùy chọn

- **Source Section:** (11.6,15.8–15.10,1.10)
- **Depends On:** Task 14.2
- **Priority:** P3
- **Status:** `planned`
- **Owner:** chưa phân công (xem `AGENT_TASKS.md`)
- **Goal:** Lập backlog tính năng TikTok đã xác minh và planner AI tùy chọn.
- **Implementation Steps:** Thu thập link từng video; đặc tả riêng; không sinh/thực thi Java động; đặt cổng confirm.
- **Done when:** Không cam kết tính năng chưa quan sát được.
- **Validation:** Review yêu cầu và test riêng từng module.
- **Evidence:** chưa có
- **Blocked By:** chưa xác định; chỉ ghi khi thực sự bị chặn

## Cổng giai đoạn và quy tắc cập nhật

- **Phase A:** Task 1.1 → 1.2/2.1 → 2.2 → 3.1 → 4.1 → 4.2 → **15.5**.
- **Phase B:** Task 3.2, 12.1, 5.1, 5.2, 6.1, 6.2, 6.3.
- **Phase C:** Task 8.1, 8.2, 6.4, **14.1**. Không tự động triển khai C nếu lựa chọn calibration/safety chưa được chốt.
- **Phase D:** Task 7.1, 7.2, 9.1, 10.1, 11.1.
- **Phase E:** Task 13.1, 14.2, 15.1.
- **Phase F:** Task 15.2, sau khi có dữ liệu/video và được duyệt.

### Khi hoàn thành một task

1. Ghi lại file thực sự tạo/sửa và kết quả kiểm thử.
2. Đổi `Status` thành `completed` **chỉ** khi đạt `Done when`.
3. Nếu còn lỗi, đổi `blocked` hoặc `failed` với nguyên nhân và bước tái hiện; không bỏ qua.
4. Thông báo ID đã xong cho người quản lý phân công ở `AGENT_TASKS.md`; tránh sao chép toàn bộ checklist.
5. Xác định task tiếp theo từ dependency, không dựa đơn thuần vào số thứ tự.
