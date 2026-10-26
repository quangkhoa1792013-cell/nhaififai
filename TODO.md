# TODO — nhaififai: Smart Baritone Addon

> **Trạng thái:** Đặc tả công việc, chưa phải mã nguồn. Nguồn: báo cáo trong `chat.md` ngày 08/10/2026. Ô `[ ]` là chưa hoàn thành; `[?]` là phải khảo sát hoặc Khoa quyết định. Không coi các giả thuyết về server là sự thật đã xác minh.
>
> **Mục tiêu:** Addon Java cho Meteor Client, lập kế hoạch và điều phối Baritone; người dùng xem trước, xác nhận, tạm dừng, tiếp tục hoặc hủy tác vụ. Ưu tiên module Smart Selection Miner trước; các module khác triển khai từng giai đoạn.

## (0) Quy ước, thuật ngữ và nguyên tắc

- [ ] (0.1) Dùng thứ tự `(1)`, `(1.1)`, `(1.1.1)` cho hạng mục, tính năng con và việc cụ thể.
- [ ] (0.2) Mỗi việc khi thực hiện phải có: trạng thái, phụ thuộc, điều kiện hoàn thành, bằng chứng kiểm thử.
- [ ] (0.3) Phân biệt **đã kiểm chứng**, **giả thuyết**, **cần người dùng chọn** và **đề xuất mặc định**.
- [ ] (0.4) Giải thích thuật ngữ: **selection** = vùng hộp được Baritone đánh dấu; **bounding box** = hộp giới hạn theo min/max X,Y,Z; **planner** = bộ chọn chuỗi hành động; **executor** = bộ thực thi; **observer** = bộ đọc trạng thái game và phản hồi server; **adapter** = lớp tích hợp API khác; **calibration** = đào thử để đo cơ chế cúp; **state machine** = máy trạng thái; **repath** = tính lại đường đi; **tick** = nhịp cập nhật game; **reach** = tầm tương tác hợp lệ; **rollback** = trở về trạng thái an toàn khi thất bại, không phải phục hồi block đã phá.
- [ ] (0.5) **Lưu ý:** Không tự suy diễn cơ chế máy chủ chỉ từ tooltip; xác nhận bằng phép thử, log và block update thực tế.
- [ ] (0.6) **Chú ý:** Kế hoạch chỉ được phép chọn hành động có giới hạn, có thể hủy và không tranh quyền điều khiển với module khác.
- [ ] (0.7) Liên kết tham khảo: [Fabric documentation](https://docs.fabricmc.net/), [Meteor source](https://github.com/MeteorDevelopment/meteor-client), [Meteor addon template](https://github.com/MeteorDevelopment/meteor-addon-template), [Baritone source](https://github.com/cabaletta/baritone), [Minecraft Wiki](https://minecraft.wiki/).

## (1) Chốt yêu cầu và phạm vi

- [ ] (1.1) Xác nhận mục tiêu là addon Meteor client-side, không chỉnh mã Baritone gốc và không yêu cầu plugin server.
- [ ] (1.2) Chốt tên hiển thị addon, mod ID, package Java, tên file JAR và cách đặt phiên bản.
- [ ] (1.3) Chốt MVP: Smart Selection Miner + Baritone adapter + calibration + preview/confirm + pause/resume + log; để các module khác vào roadmap.
- [ ] (1.4) [?] Chọn cách xử lý mép vùng 3×3: cúp thường, bỏ mép, hoặc cho phép vượt vùng sau xác nhận. Mặc định đề xuất: **không phá ngoài selection**; dùng cúp thường nếu có, không thì để lại và báo.
- [ ] (1.5) [?] Chấp nhận calibration 3–6 phép đào thử ở khu an toàn? Mặc định: yêu cầu xác nhận rõ trước mỗi lần có thể phá block.
- [ ] (1.6) [?] Xác định khi thiếu cúp thường: dừng phần mép, yêu cầu đổi công cụ, hay xử lý bằng một cách đã đo an toàn.
- [ ] (1.7) [?] Chốt ngưỡng durability, máu, đói, độ rơi, nước/lava, phá lá/sculk, đặt block ngoài selection, đào xuống; mặc định chọn hành vi bảo thủ.
- [ ] (1.8) [?] Chốt preset mặc định Safe hay Balanced; ưu tiên **Safe** khi chưa đo môi trường.
- [ ] (1.9) [?] Chốt cú pháp lệnh `.smart` và các alias; giữ prefix Meteor có thể cấu hình.
- [ ] (1.10) [?] Thu thập URL và tính năng của từng video TikTok sau 11 video đầu, ghi thành yêu cầu độc lập; profile từng trả HTTP 403, **không giả vờ đã xem video**.
- [ ] (1.11) Xác định giới hạn quyền thao tác ở server, vùng bảo vệ, chế độ singleplayer/multiplayer và tình huống phải nhường điều khiển cho người chơi.

## (2) Repo, môi trường và tương thích

- [ ] (2.1) Mở `quangkhoa1792013-cell/nhaififai` nhánh `main`; kiểm tra `git status`, remotes, commit hiện tại và thay đổi chưa commit trước khi ghi đè bất kỳ thứ gì.
- [ ] (2.2) Xác minh lại mốc khảo sát `8d92d31` (first commit) còn đúng với repo hiện tại hay không; không giả định repo vẫn chỉ có README.
- [ ] (2.3) Khóa target Minecraft `1.21.11`, Meteor `1.21.11-82`, Fabric Loader `0.19.5`, Baritone `v1.17.0`, JDK 21; kiểm tra lại JAR thực tế trước build.
- [ ] (2.4) Đối chiếu Meteor build 82 với commit `7fc97a87…` đã ghi ở báo cáo; lưu full SHA, tag/build metadata, checksum JAR.
- [ ] (2.5) Đối chiếu Baritone tag `v1.17.0` commit `23723891da460ef15797b02fe5b385b0c5b163cc`, chữ ký API với JAR người dùng đang cài.
- [ ] (2.6) Chọn phiên bản Loom/Yarn/Fabric API tương thích, pin dependency; không chép template mới nhất nếu đã chuyển target khác.
- [ ] (2.7) Tạo cấu trúc Gradle wrapper, `settings.gradle`, build config, Java sources, resources, `fabric.mod.json`, metadata license/README; **không code trước khi các lựa chọn MVP tối thiểu được chốt**.
- [ ] (2.8) Phân biệt dependency compile, runtime và optional; không đóng gói Baritone vào JAR addon; báo thiếu hoặc sai phiên bản rõ ràng.
- [ ] (2.9) Lưu hướng dẫn build, chạy client dev, cài JAR, môi trường test, quy trình tái tạo lỗi và checksum phát hành.
- [ ] (2.10) Thiết lập kiểm tra compile, format, metadata, dependency resolution và artifact JAR; ghi nguyên nhân khi Maven SNAPSHOT không truy cập được.

## (3) Kiến trúc tổng thể và vòng đời

- [ ] (3.1) Xác định entrypoint đăng ký Meteor modules, command, event listeners, settings, renderer và lifecycle.
- [ ] (3.2) Phân ranh `BaritoneAdapter`, `WorldObserver`, `TaskPlanner`, `ExecutionEngine`, `SafetyPolicy`, `ToolProfiler`, `SelectionPlanner`, `StuckDetector`, `RecoveryController`, `TaskStore`, `PreviewUI`.
- [ ] (3.3) Định nghĩa dữ liệu task: ID, nguồn yêu cầu, mục tiêu, vùng, preset, yêu cầu item, giới hạn, trạng thái, kết quả và lý do thất bại.
- [ ] (3.4) Thiết kế máy trạng thái: Idle → Observing → Planning → Preview → AwaitConfirm → Executing → Paused/Recovering → Completed/Failed/Cancelled.
- [ ] (3.5) Quy định quyền sở hữu duy nhất cho một tác vụ movement/mining tại mỗi thời điểm; không để hai module đồng thời điều khiển Baritone hoặc input.
- [ ] (3.6) Mọi hành động có precondition, timeout, quan sát kết quả, số lần retry tối đa và hành động khi thất bại.
- [ ] (3.7) Đồng bộ trạng thái theo client tick, world change, disconnect, death, dimension change, GUI open và người dùng tự lái.
- [ ] (3.8) Hỗ trợ dừng khẩn cấp giải phóng phím giữ, path, thao tác đào/đặt và các subscription còn hoạt động.
- [ ] (3.9) Chọn chính sách lưu task và checkpoint, gắn server/world/dimension; không tự resume vào thế giới khác.
- [ ] (3.10) Tạo log có thời điểm, task ID, trạng thái, target, path failure, block update, nguyên nhân stop; chế độ debug có thể bật/tắt.

## (4) Baritone adapter và selection

- [ ] (4.1) Kiểm tra runtime Baritone hiện diện, phiên bản và API tương thích; thông báo dễ hiểu nếu thiếu.
- [ ] (4.2) Đọc selection bằng API manager, không phụ thuộc phân tích văn bản chat; xác minh `getSelections`, `getOnlySelection`, `getLastSelection`, `min/max`, `pos1/pos2`, `size/aabb` với đúng JAR.
- [ ] (4.3) Xử lý không có selection, một vùng, nhiều vùng, vùng quá lớn, tọa độ âm, vượt chunk chưa tải, khác dimension.
- [ ] (4.4) Bảo toàn tọa độ người dùng tạo; xác định phạm vi gồm cả hai đầu, thể tích và giới hạn bộ nhớ/khối lượng công việc.
- [ ] (4.5) Xác minh cú pháp `#sel 1/2` so với `#sel pos1/pos2` trong phiên bản thực; ghi hướng dẫn sử dụng chính xác.
- [ ] (4.6) Adapter tạo/cancel/pause goal, đọc trạng thái path và kết quả; xác minh event/callback nào API cung cấp thay vì đoán.
- [ ] (4.7) Không xóa/chỉnh selection gốc khi planner chạy; phát hiện người dùng đổi vùng và yêu cầu lập kế hoạch lại.
- [ ] (4.8) Nếu Baritone đã được người dùng giao tác vụ khác: chọn từ chối, hàng đợi hoặc yêu cầu xác nhận quyền điều khiển; mặc định không giành quyền âm thầm.

### (4.9) Luồng gọi Baritone từ một task

- [ ] (4.9.1) Khi planner tạo yêu cầu đi/đào, adapter nhận goal, giới hạn và mã task; trước tiên xác minh client đang trong world đúng và Baritone sẵn sàng.
- [ ] (4.9.2) Kiểm tra goal hợp lệ, dimension khớp, chunk biết/không biết và quyền điều khiển; nếu không hợp lệ, trả lỗi cụ thể cho planner, **không gửi lệnh**.
- [ ] (4.9.3) Chụp trạng thái Baritone đang chạy; nếu có task do người chơi khởi động thì dừng chờ quyết định của người chơi.
- [ ] (4.9.4) Khi được phép, adapter chuyển goal sang đối tượng/path request phù hợp API đã xác minh; đánh dấu task là đang yêu cầu path.
- [ ] (4.9.5) Observer theo dõi path được chấp nhận, đang tính, đang di chuyển, đã tới, không có đường và bị hủy; phân biệt sự kiện API với suy luận từ vị trí.
- [ ] (4.9.6) Khi tới đích, xác minh điều kiện hoàn thành của task (đúng vị trí trong sai số, đứng an toàn, mục tiêu còn tồn tại); chỉ khi đó mới báo `Completed`.
- [ ] (4.9.7) Khi thất bại, lưu nguyên nhân/path cuối, gửi sang RecoveryController; khi cancel, hủy đúng request của addon và giải phóng quyền điều khiển.

## (5) Nhận diện và calibration cúp 3×3 KingMC

- [ ] (5.1) Thu thập tooltip, display name, lore, enchant, NBT/components và slot của cúp; thiết kế rule phát hiện có profile theo server, tránh chỉ dựa vào tên màu.
- [ ] (5.2) Phân biệt cúp thường, cúp 3×3 đã xác minh, công cụ chưa biết, công cụ gần hỏng hoặc không có trong inventory.
- [ ] (5.3) Ghi lại thao tác đào thử: tọa độ block tâm, mặt block click, yaw/pitch, sneak, vị trí đứng, tool, kết quả block update và latency.
- [ ] (5.4) Thử riêng hướng nhìn theo Y, nhìn ngang, cúi/ngửa kết hợp sneak; kiểm tra các mặt phẳng XY, XZ, YZ, trường hợp 3×3/1×3/khác.
- [ ] (5.5) Với mỗi phép thử ghi tập block thực sự biến mất, block không bị phá, block bị server khôi phục, và sai lệch so với dự đoán.
- [ ] (5.6) Nếu pattern không nhất quán, vùng thử bị bảo vệ hoặc không đủ dữ liệu: đánh dấu **Unknown**, không chạy đào hàng loạt.
- [ ] (5.7) Lưu profile có server ID, dimension/rule context, dấu vân tay công cụ, pattern, ngày thử và mức tin cậy; cho phép xóa/hiệu chỉnh lại.
- [ ] (5.8) Hiển thị trước block sẽ bị ảnh hưởng và cảnh báo khi có thể vượt selection hoặc va phải block cấm phá.
- [ ] (5.9) Phát hiện server đổi plugin/cúp hoặc bản cập nhật làm pattern lệch; tự dừng và yêu cầu calibration lại.

### (5.10) Quy trình tạo profile cúp

- [ ] (5.10.1) Người dùng chọn công cụ và vùng thử; hệ thống kiểm tra block có thể phá, không có vật quan trọng phía sau và không nằm ngoài khu được phép.
- [ ] (5.10.2) Tạo danh sách phép thử độc lập theo hướng và trạng thái sneak, giới hạn số block có thể bị mất cho từng phép thử.
- [ ] (5.10.3) Preview từng phép thử, chờ confirm rồi ghi snapshot trước đào.
- [ ] (5.10.4) Thực hiện đúng một lần đào, chờ block update ổn định hoặc timeout, ghi tập block thay đổi và cả trường hợp không thay đổi.
- [ ] (5.10.5) So sánh các phép thử lặp lại; chỉ kết luận pattern khi kết quả đủ nhất quán, nếu không ghi `Unknown` và yêu cầu kiểm tra thủ công.
- [ ] (5.10.6) Sau khi lưu profile, chạy một bài test nhỏ để đối chiếu preview với thực tế trước khi cho phép đào selection hàng loạt.

## (6) Smart Selection Miner — MVP

- [ ] (6.1) Nhận lựa chọn vùng, tool, hướng quét, preset, tiêu chí dừng và giới hạn phá ngoài vùng.
- [ ] (6.2) Chụp snapshot trạng thái block trong các chunk đã tải, đánh dấu unknown/chưa tải, air, liquid, unbreakable, protected/đào thất bại.
- [ ] (6.3) Chia selection thành layer và ứng viên cú đánh 3×3 theo đúng pattern đã calibration, không giả định luôn là mặt phẳng ngang.
- [ ] (6.4) Tính phần giao giữa vùng phá dự đoán và selection; phạt block thừa, đường di chuyển, durability, hardness, thao tác đổi tool và nguy cơ rơi/lava.
- [ ] (6.5) Chọn vị trí đứng, mặt click, yaw/pitch và thứ tự các cú đánh hợp lệ trong reach; bảo toàn đường thoát.
- [ ] (6.6) Tối ưu độ phủ trước, sau đó số cú đánh, số bước di chuyển và thời gian; cho phép chiến lược zigzag.
- [ ] (6.7) Xử lý mép 1–2 block, lỗ rỗng, tầng không đầy, cột lẻ, block không thể phá, chunk chưa load và tool thiếu.
- [ ] (6.8) Chế độ Strict mặc định cấm bất kỳ block dự đoán nào ngoài vùng; chế độ ngoại lệ phải preview và confirm riêng.
- [ ] (6.9) Hiển thị preview gồm selection, ô 3×3, tâm click, vùng thừa, block bỏ lại, hướng di chuyển, số cú đánh và chi phí dự tính.
- [ ] (6.10) Chỉ chạy sau confirm; mỗi lần click/đào chờ phản hồi server, không tính block là đã hoàn thành chỉ vì client đã gửi lệnh.
- [ ] (6.11) So sánh expected/observed, cập nhật mặt nạ block còn lại và replan nếu server chỉ phá một phần hoặc phản hồi khác.
- [ ] (6.12) Pause/resume/cancel, checkpoint theo vùng/layer; đọc lại block thực tế trước khi resume.
- [ ] (6.13) Xử lý block rơi (cát/sỏi), nước/lava, gravel, tool break, inventory đầy, vùng bảo vệ và disconnect.
- [ ] (6.14) Kết quả cuối báo block đã đào, chưa đào, bỏ qua, ngoài vùng (nếu được cho phép), số lỗi/retry và nguyên nhân.

### (6.15) Luồng từ chọn vùng đến hoàn thành đào

- [ ] (6.15.1) Khi người dùng bật Smart Selection Miner, đọc selection và công cụ; nếu thiếu một trong hai, hiển thị chính xác thứ còn thiếu và dừng ở bước chuẩn bị.
- [ ] (6.15.2) Nếu công cụ thuộc loại 3×3 nhưng chưa có profile đáng tin cậy, đề nghị calibration; không tự chạy mining.
- [ ] (6.15.3) Observer đọc các block có thể thấy trong selection; đánh dấu phần chưa tải là chưa biết thay vì coi như air.
- [ ] (6.15.4) Planner tạo tập ứng viên cú đào, mỗi ứng viên gồm tâm, mặt click, hướng đứng, vùng tác động, yêu cầu đường đến và dự kiến số block hợp lệ.
- [ ] (6.15.5) Loại ứng viên vi phạm Strict/safety; gom ứng viên còn lại thành thứ tự tối ưu có đường di chuyển và fallback cho mép.
- [ ] (6.15.6) Render preview và tổng kết chi phí/rủi ro; nếu người dùng chỉnh config hoặc selection, hủy preview cũ và tính lại.
- [ ] (6.15.7) Sau confirm, chọn cú đào đầu tiên, nhờ SmartGoto/Baritone tới điểm đứng đã tính.
- [ ] (6.15.8) Khi Baritone báo đến điểm đứng, **kiểm tra lại** reach, mặt click, vị trí, tool, block mục tiêu và safety; nếu sai, replan thay vì đào theo dữ liệu cũ.
- [ ] (6.15.9) Khi hợp lệ, thực hiện một cú đào, chờ kết quả world observer, đối chiếu block được phá thực tế rồi cập nhật vùng còn lại.
- [ ] (6.15.10) Nếu đủ block như dự đoán thì chuyển cú tiếp theo; nếu thiếu block, block bảo vệ hoặc pattern lệch thì tạm dừng, phân loại nguyên nhân và chỉ replan khi an toàn.
- [ ] (6.15.11) Nếu cúp gần hỏng/đồ đầy/địa hình nguy hiểm thì chuyển `Paused` cùng lý do và lựa chọn tiếp tục, đổi công cụ hoặc kết thúc.
- [ ] (6.15.12) Khi không còn ứng viên hợp lệ, báo hoàn tất hoặc hoàn tất một phần; liệt kê block chưa xử lý kèm lý do, không tuyên bố thành công 100%.

### (6.16) Nhánh xử lý mép vùng

- [ ] (6.16.1) Xác định phần dư theo từng chiều và những block không được bất kỳ cú 3×3 Strict nào bao phủ.
- [ ] (6.16.2) Nếu có cúp thường và chính sách cho phép thì xếp các block dư vào bước đào đơn lẻ sau phần 3×3.
- [ ] (6.16.3) Nếu không có cúp thường, dừng phần dư và báo rõ danh sách/khối lượng còn lại; không lặng lẽ phá ra ngoài vùng.
- [ ] (6.16.4) Nếu người dùng chủ động chọn chế độ cho phép vượt vùng, tạo preview riêng cho từng khu bị ảnh hưởng và kiểm tra block cấm/protected trước confirm.

## (7) Đào base từ trên xuống và tối ưu thứ tự đào

- [ ] (7.1) Định nghĩa chế độ top-down: layer bắt đầu, thứ tự Y giảm, chiều quét X/Z, điểm vào/ra, giới hạn chiều cao.
- [ ] (7.2) Quét zigzag đổi hướng mỗi hàng, tránh quay đầu vô ích nhưng không hy sinh an toàn.
- [ ] (7.3) Kết hợp profile cúp 3×3 và cúp thường theo từng layer; preview khu vực dự định phá.
- [ ] (7.4) Giữ lối thoát, sàn đứng, block đỡ; phát hiện nguy cơ tự chôn hoặc đứng trên block sẽ bị phá.
- [ ] (7.5) Với quặng/block ưu tiên, cân nhắc reach từ vị trí hiện tại, số block cản, hardness, công cụ, movement, hazard và lợi ích mỗi cú đánh.
- [ ] (7.6) Thử nhiều thứ tự hợp lệ (đào trực tiếp, đổi góc, đi vòng, mở lối) và chọn kế hoạch có tổng chi phí nhỏ; không chỉ chọn block gần nhất.
- [ ] (7.7) Sau mỗi thay đổi địa hình cần kiểm tra lại đường đi và khả năng tiếp cận mục tiêu.

## (8) Goto/pathfinding và chống mắc kẹt

- [ ] (8.1) Module `SmartGoto` nhận tọa độ, dimension hiện tại, độ lệch đích chấp nhận, giới hạn thời gian/chi phí và policy hazard.
- [ ] (8.2) Đọc path/progress Baritone; theo dõi khoảng cách đến goal, dịch chuyển theo cửa sổ tick, số lần đứng yên và path recalculation.
- [ ] (8.3) Phân loại stuck: collision/shape, sculk, lá/cây, nước, block chưa tải, goal không tới được, packet/server rubberband, tác vụ bị tranh quyền.
- [ ] (8.4) Recovery theo cấp: chờ → revalidate → repath → lùi/nhảy an toàn → waypoint khác → phá/đặt block nếu được phép → dừng và báo.
- [ ] (8.5) Ghi số lần recovery theo vị trí/nguyên nhân, cooldown và penalty vùng thất bại để tránh vòng lặp vô hạn.
- [ ] (8.6) Có whitelist/blacklist block được phép phá, đặt, bước lên; dùng collision shape thực tế, không suy từ block có đầy hay không.
- [ ] (8.7) Với sculk sensor, cây và lá: tùy chọn đi vòng, phá, xây qua hoặc hỏi; default không phá trong vùng chưa được phép.
- [ ] (8.8) Khi người chơi can thiệp tay, dừng tác vụ điều khiển đầu vào và hỏi tiếp tục nếu thích hợp.

### (8.9) Luồng SmartGoto khi nhận tọa độ A

- [ ] (8.9.1) Nhận A `(x,y,z)` và nguồn yêu cầu (command, mining planner hay task queue); kiểm tra tọa độ có thuộc dimension hiện tại và giới hạn world không.
- [ ] (8.9.2) Kiểm tra điều kiện đến đích: sai số chấp nhận, cần đứng trên block nào, có bắt buộc nhìn về phía mục tiêu không và có thể đứng an toàn không.
- [ ] (8.9.3) Nếu A không đứng được, chọn waypoint đứng gần A được phép hoặc trả yêu cầu planner tạo phương án tiếp cận; không báo đến đích giả.
- [ ] (8.9.4) Giao goal cho Baritone adapter và bắt đầu bộ theo dõi tiến độ gồm vị trí hiện tại, khoảng cách còn lại và thời gian từ lần tiến bộ cuối.
- [ ] (8.9.5) Khi Baritone đang di chuyển, cập nhật tiến độ theo từng cửa sổ thời gian; bỏ qua dao động nhỏ để tránh đánh dấu stuck sai.
- [ ] (8.9.6) Khi đến A, kiểm tra lại sai số và an toàn, trả kết quả cho task đã gọi; task đó mới quyết định đào/nhặt/đặt block tiếp theo.
- [ ] (8.9.7) Nếu Baritone không thể tìm path, chuyển sang phân loại nguyên nhân; nếu server đưa nhân vật lùi lại, xác định rubberband trước khi retry.

### (8.10) Cây quyết định khi không tiến gần A

- [ ] (8.10.1) Nếu path vẫn đang tính hoặc chunk chưa tải: chờ có timeout, không gửi liên tiếp nhiều goal trùng lặp.
- [ ] (8.10.2) Nếu có collision/sculk/lá trước mặt: xác định block cụ thể và policy cho phép đi vòng, phá hay đặt block; mặc định thử đi vòng trước.
- [ ] (8.10.3) Nếu đường bị hố/rơi/lava: gửi yêu cầu cho HazardNavigator lập đường an toàn; nếu không có đường hợp lệ thì dừng.
- [ ] (8.10.4) Nếu path không hợp lệ do địa hình thay đổi: yêu cầu Baritone revalidate/repath từ vị trí thực tế.
- [ ] (8.10.5) Nếu cùng một vị trí thất bại nhiều lần: ghi penalty/cooldown, thử waypoint khác; vượt ngân sách retry thì báo lỗi và kết thúc.
- [ ] (8.10.6) Sau mỗi recovery, kiểm tra lại khoảng cách đến A; chỉ công nhận recovery thành công khi thật sự có tiến bộ.

## (9) Tiếp cận item/quặng nguy hiểm và xây đường

- [ ] (9.1) Phát hiện mục tiêu ở thấp hơn, vách cao, hố, lava, nước, thiếu điểm đứng hoặc ngoài reach.
- [ ] (9.2) Tạo ứng viên đường tiếp cận: staircase, cầu, bậc xuống, scaffold, thang, đường vòng; water-drop chỉ khi được bật và đã kiểm tra.
- [ ] (9.3) Kiểm tra block đỡ, khoảng rơi, chỗ hạ cánh, vật liệu trong inventory, khả năng lấy lại đường và giới hạn xây ngoài vùng.
- [ ] (9.4) Không đào thẳng dưới chân khi không nhìn thấy điểm đỡ an toàn; khảo sát trước khi xuống.
- [ ] (9.5) Tính chi phí xây/phá/di chuyển so với giá trị mục tiêu; abort nếu item biến mất hoặc plan không còn hợp lệ.
- [ ] (9.6) Sau khi tới mục tiêu, xác nhận thu được item/quặng rồi quay lại task chính; đánh dấu phần đường đã xây để không phá nhầm.

## (10) Nhặt item theo whitelist

- [ ] (10.1) Config danh sách item ID/tag được nhặt, ưu tiên, blacklist và phạm vi theo dõi.
- [ ] (10.2) Phát hiện item entity trong chunk tải, tính khoảng cách/độ nguy hiểm/khả năng despawn; tránh suy đoán thời gian despawn nếu server không cung cấp.
- [ ] (10.3) Lập lộ trình nhặt nhiều item có giới hạn số mục tiêu; không đổi target liên tục khi item mới xuất hiện.
- [ ] (10.4) Kiểm tra inventory còn slot và stackability; khi đầy chọn dừng, bỏ qua hoặc xử lý theo chính sách đã chốt.
- [ ] (10.5) Không tự vứt item giá trị hoặc dùng chest/shulker khi chưa được người dùng cho phép.
- [ ] (10.6) Tạm ngừng mining khi nhặt, giữ checkpoint, quay lại đúng tác vụ và đánh giá địa hình sau khi trở lại.

### (10.7) Luồng nhặt một item

- [ ] (10.7.1) Observer thấy item entity → đối chiếu whitelist, inventory và giới hạn bán kính.
- [ ] (10.7.2) Tính điểm ưu tiên có cân nhắc khoảng cách và nguy hiểm; nếu item không phù hợp thì bỏ qua, không tạo task.
- [ ] (10.7.3) Khi được chọn, lưu task hiện tại vào trạng thái tạm dừng rồi gọi SmartGoto đến điểm nhặt an toàn.
- [ ] (10.7.4) Trong lúc di chuyển, kiểm tra item còn tồn tại; nếu biến mất, hủy goal nhặt, không đi tiếp đến vị trí trống.
- [ ] (10.7.5) Khi tới nơi, đợi xác nhận item biến mất và inventory thay đổi nếu quan sát được; phân biệt được nhặt với despawn/được người khác nhặt.
- [ ] (10.7.6) Quay lại checkpoint nhiệm vụ cũ, kiểm tra điều kiện tiếp tục và chỉ resume nếu vẫn hợp lệ.

## (11) Task queue và planner/AI

- [ ] (11.1) Thiết kế hàng đợi tác vụ tuần tự với điều kiện trước/sau, ưu tiên, hủy và retry; ví dụ goto → mine → pickup → resume.
- [ ] (11.2) Planner local phân tích mục tiêu thành thao tác hữu hạn; mỗi bước có mô tả cho UI và chi phí/rủi ro dự kiến.
- [ ] (11.3) Chọn kế hoạch từ nhiều ứng viên theo trọng số an toàn, di chuyển, số block, durability và thời gian.
- [ ] (11.4) Cho phép người dùng chỉnh một bước, bỏ bước, đổi thứ tự rồi tạo preview mới; không thi hành kế hoạch cũ sau khi sửa.
- [ ] (11.5) Khi bước fail, chuyển sang recovery hoặc yêu cầu quyết định; không tự sinh task vô hạn.
- [ ] (11.6) AI/LLM ngoài là phần **tùy chọn về sau**: chỉ chuyển mô tả tự nhiên thành kế hoạch cấu trúc cần kiểm định và confirm; không sinh/biên dịch/thực thi Java tùy ý, không điều khiển từng tick.
- [ ] (11.7) Bảo đảm mọi task có thể giải thích vì sao đã chọn mục tiêu và vì sao đã dừng.

### (11.8) Chu trình xử lý task nối tiếp

- [ ] (11.8.1) Nhận yêu cầu tổng, phân tách thành các task có phụ thuộc rõ (ví dụ `Goto` hoàn thành mới `Mine`, `Pickup` là nhiệm vụ ngắt quãng).
- [ ] (11.8.2) Kiểm tra đầu vào, quyền điều khiển, tài nguyên, độ nguy hiểm trước khi xếp task vào hàng.
- [ ] (11.8.3) Chọn task sẵn sàng có ưu tiên cao nhất, lập plan, hiển thị confirm khi cần.
- [ ] (11.8.4) Executor chạy từng hành động; mỗi kết quả được observer xác nhận rồi state machine mới chuyển bước.
- [ ] (11.8.5) Nếu task con fail, áp dụng recovery có giới hạn; nếu phụ thuộc không thể hoàn thành, đánh dấu các task sau là blocked và giải thích lý do.
- [ ] (11.8.6) Nếu người dùng pause/cancel hoặc world đổi, khóa chuyển task, giải phóng điều khiển và lưu checkpoint an toàn.
- [ ] (11.8.7) Khi task cuối hoàn tất, xuất báo cáo cho toàn bộ chuỗi gồm thành công, bỏ qua, lỗi, vật liệu và thời gian.

## (12) Safety policy, anti-cheat và vận hành

- [ ] (12.1) Preset Safe/Balanced/Fast có giới hạn riêng cho rotation, delay, hành động/tick, đào/đặt block, retry và movement.
- [ ] (12.2) Giới hạn tầm reach, góc nhìn, vị trí, cooldown và packet hợp lệ theo trạng thái client/server; không hứa tránh được mọi anti-cheat.
- [ ] (12.3) Dừng trước các điều kiện: HP/đói thấp, độ rơi vượt ngưỡng, tool gần hỏng, lava nguy hiểm, item cần thiết hết, inventory đầy, quá nhiều failure, world đổi.
- [ ] (12.4) Ghi chính sách theo profile server, mặc định từ chối thao tác gây phá ngoài selection hoặc vùng bảo vệ.
- [ ] (12.5) Không thiết kế tính năng lách kiểm tra, giả mạo trạng thái hay packet bất hợp lệ; tập trung tương thích và giới hạn thao tác hợp lệ.
- [ ] (12.6) Kiểm tra hành vi khi lag, rubberband, server restart, TPS thấp và block update đến trễ.
- [ ] (12.7) Có nút dừng khẩn cấp hoạt động bất kể GUI hiện tại và đảm bảo không giữ input sau khi dừng.

## (13) GUI, command và cấu hình

- [ ] (13.1) Đăng ký các module Meteor theo nhóm rõ ràng: SmartSelectionMiner, SmartGoto, SmartPickup, TopDownExcavator, TaskCoordinator; phần chưa làm không bật giả.
- [ ] (13.2) Settings: bật/tắt, preset, selection policy, tool, whitelist, thông số path, safety, logging, calibration profile.
- [ ] (13.3) Command dự kiến `.smart help`, `goto`, `mine`, `pickup`, `plan`, `confirm`, `cancel`, `pause`, `resume`, `calibrate`, `status`, `profile`; xác minh cơ chế đăng ký lệnh trong Meteor target.
- [ ] (13.4) Màn hình preview có tổng quan, danh sách bước, vị trí/ô đào, màu phân biệt an toàn/nguy cơ/không biết, xác nhận và chỉnh sửa.
- [ ] (13.5) HUD/trạng thái hiển thị task hiện tại, phần trăm tiến độ **theo block server xác nhận**, tốc độ, tool, lý do chờ, path và số lần retry.
- [ ] (13.6) Cấu hình lưu theo server/world, có giá trị mặc định và kiểm tra dữ liệu không hợp lệ; hỗ trợ import/export mà không lộ thông tin nhạy cảm.
- [ ] (13.7) Mô tả trong GUI cho các thuật ngữ khó, tooltip cảnh báo thao tác phá/xây vượt vùng và liên kết tài liệu hướng dẫn.
- [ ] (13.8) Không để nút Confirm hoạt động nếu thiếu selection, chưa calibration, không có path hợp lệ hoặc policy không cho phép.

## (14) Kiểm thử và tiêu chí chấp nhận

- [ ] (14.1) Unit test geometry: chọn min/max, vùng âm, biên inclusive, kích thước 1/2/3/4/10, selection chồng nhau, mép không chia hết 3.
- [ ] (14.2) Unit test planner: tối ưu phủ, không phá ngoài vùng ở Strict, mục tiêu unreachable, block cấm, tool thiếu, fallback cúp thường.
- [ ] (14.3) Test giả lập observer với block update tới muộn, bị rollback, chỉ phá một phần 3×3, block đổi bất ngờ, chunk unload.
- [ ] (14.4) Test state machine và task queue: hoàn tất, pause/resume, cancel giữa path/mining, disconnect, death, retry vượt giới hạn, người chơi can thiệp.
- [ ] (14.5) Test integration Meteor/Baritone với đúng JAR trên Minecraft 1.21.11; kiểm tra không xung đột selection hay module khác.
- [ ] (14.6) Test trong singleplayer/world thử nghiệm trước: calibration an toàn, 3×3 ngang/dọc, nhiều layer, zigzag, chọn mép, khôi phục checkpoint.
- [ ] (14.7) Thử multiplayer trên KingMC chỉ ở nơi được phép, dùng preset Safe, diện tích nhỏ, xem log phản hồi server và dừng khi có hành vi bất thường.
- [ ] (14.8) Tiêu chí MVP: người dùng chọn vùng, thấy preview đúng, confirm mới đào; Strict không phá ngoài vùng theo kết quả xác nhận; dừng an toàn; báo phần chưa hoàn thành; JAR build chạy được.
- [ ] (14.9) Đo chỉ số: block mục tiêu/đã đào, block ngoài vùng, số cú click, path length, thời gian, retry, tỷ lệ hoàn thành và sai lệch dự đoán.
- [ ] (14.10) Ghi ma trận thiết bị/server/tool đã thử và giới hạn chưa hỗ trợ; không tuyên bố tương thích tuyệt đối từ một lần test.

## (15) Phát hành, tài liệu và roadmap

- [ ] (15.1) Viết README: mục tiêu, yêu cầu version, cài đặt, command, selection, calibration, các preset và hạn chế.
- [ ] (15.2) Viết troubleshooting: thiếu Baritone, mismatch Meteor, Gradle SNAPSHOT, không đọc selection, cúp không nhận, stuck, lag, anti-cheat, inventory đầy.
- [ ] (15.3) Build artifact, kiểm tra metadata/version, checksum và cài chạy thử bằng một profile Minecraft sạch.
- [ ] (15.4) Ghi changelog, tag release, commit/push chỉ khi repo đúng và người dùng đồng ý bước phát hành.
- [ ] (15.5) Giai đoạn A: skeleton + adapter + command/status + selection read.
- [ ] (15.6) Giai đoạn B: detector/cúp calibration + geometry planner + preview/confirm.
- [ ] (15.7) Giai đoạn C: execution/observer + resume/recovery + Smart Selection Miner MVP.
- [ ] (15.8) Giai đoạn D: top-down zigzag + tối ưu block + pickup whitelist.
- [ ] (15.9) Giai đoạn E: SmartGoto + hazard navigation + task queue + GUI hoàn chỉnh.
- [ ] (15.10) Giai đoạn F: tính năng từ TikTok đã xác minh, mở rộng planner hoặc AI tùy chọn, tối ưu hóa hiệu năng.

> **Điểm bắt đầu khi tiếp tục coding:** xử lý (1.4)–(1.9), xác nhận repo/version ở (2), rồi thực hiện giai đoạn A của (15). Không tự ý làm toàn bộ module cùng lúc.
