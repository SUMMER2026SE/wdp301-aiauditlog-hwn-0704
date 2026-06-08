# AI Audit Log

## 1. Thông tin chung

| Thông tin | Nội dung |
|---|---|
| Môn học | SWP |
| Mã môn học | SWP391 |
| Lớp | SE20A02 |
| Học kỳ | SU26 |
| Tên bài tập / Project | wdp301-rbl-project-wdp_se18d08_group-1-1 |
| Tên sinh viên / Nhóm | Hồ Sỹ Hưng / Valo-park-group1 |
| MSSV / Danh sách MSSV | DE180321 |
| Giảng viên hướng dẫn | HanhNt |
| Ngày bắt đầu | 06/06/2026 |
| Ngày hoàn thành | 08/06/2026 |

---

## 2. Công cụ AI đã sử dụng

Đánh dấu các công cụ AI đã sử dụng trong quá trình thực hiện project.

- [x] ChatGPT / Codex
- [x] Gemini
- [ ] Claude
- [ ] GitHub Copilot
- [ ] Cursor
- [x] Antigravity
- [ ] Perplexity
- [ ] Microsoft Copilot
- [ ] Công cụ khác: ....................................

---

## 3. Mục tiêu sử dụng AI

### Mô tả mục tiêu sử dụng AI

```text
Nhóm sử dụng AI để hỗ trợ phân tích, rà soát và hoàn thiện module Notification cho hệ thống VALO Parking. Các mục tiêu chính gồm:

1. Phân tích implementation plan của module notification, xác định phần đã hoàn thành và phần còn thiếu.
2. Thiết kế và hoàn thiện luồng thông báo tự động theo sự kiện: đăng ký tài khoản, xác thực email, đổi mật khẩu, nạp tiền ví, thanh toán, cảnh báo số dư thấp, xe vào bãi, xe ra bãi, cảnh báo gần hết giờ đỗ và hết giờ đỗ.
3. Hoàn thiện backend Auto Notification Rules: model, seeder, API CRUD, test trigger, kiểm tra enabled/throttle và lưu lastTriggeredAt.
4. Hoàn thiện admin notification history: lọc theo priority, tìm kiếm theo nội dung/email, đánh dấu đã đọc, ẩn/xóa khỏi lịch sử admin.
5. Refactor frontend admin notification management để gọi API thật thay vì phụ thuộc mock store cho Live Feed và Auto Rules.
6. Thêm trang notification center cho customer và kiểm tra các API list, unread count, mark read, delete.
7. Debug vấn đề frontend gọi sai port backend do cấu hình VITE_API_BASE_URL trỏ tới 5001 trong khi backend chạy ở 5000.
8. Chạy kiểm tra build, lint cục bộ, syntax check backend và test E2E API trực tiếp với MongoDB thật.
```

---

## 4. Nhật ký sử dụng AI chi tiết

### Lần sử dụng AI số 1

| Nội dung | Thông tin |
|---|---|
| Ngày sử dụng | 06/06/2026 |
| Công cụ AI | Gemini / Antigravity |
| Mục đích sử dụng | Lập implementation plan cho module Notification, bao gồm auto rules, auto triggers, scheduler và frontend admin/customer notification |
| Phần việc liên quan | Requirement / Design / Backend / Frontend |
| Mức độ sử dụng | Hỗ trợ nhiều |

#### 4.1. Prompt đã sử dụng

```text
Yêu cầu AI phân tích module notification hiện có và lập kế hoạch triển khai để hệ thống có thể tự động gửi thông báo realtime khi xảy ra các sự kiện quan trọng như ví, phiên đỗ xe, tài khoản, đồng thời cập nhật frontend admin notification management từ mock data sang API thật.
```

#### 4.2. Kết quả AI gợi ý

```text
AI đề xuất implementation plan gồm các nhóm việc:
1. Tích hợp notification triggers vào wallet, session/parking và account flows.
2. Tạo parking scheduler chạy định kỳ để gửi cảnh báo còn 30/15/5 phút và hết giờ.
3. Tạo NotificationRule model, seeder default rules, API admin/rules để bật/tắt rule, chỉnh channels và throttle.
4. Mở rộng notificationTriggers để kiểm tra rule enabled trước khi tạo notification.
5. Mở rộng admin history API với filter/search và thông tin user.
6. Refactor frontend admin notification pages để dùng API thật.
7. Thêm customer notification center.
8. Đề xuất verification plan bằng API, frontend build và test luồng thực tế.
```

#### 4.3. Phần sinh viên/nhóm đã sử dụng từ AI

```text
Nhóm sử dụng cấu trúc kế hoạch triển khai của AI làm checklist để rà soát module Notification. Các thành phần được dùng gồm:
- Danh sách eventKey cho auto rules.
- Ý tưởng NotificationRule model và seeder.
- Ý tưởng parking scheduler kiểm tra active sessions.
- Ý tưởng API admin/rules và admin/history.
- Ý tưởng frontend Auto Rules gọi API thật.
- Ý tưởng customer notification center.
```

#### 4.4. Phần sinh viên/nhóm tự chỉnh sửa hoặc cải tiến

```text
Nhóm tự kiểm tra lại codebase thực tế và điều chỉnh implementation theo cấu trúc hiện có của project:
- Không thay đổi UI/layout theo yêu cầu, chỉ sửa logic và wiring API.
- Bổ sung kiểm tra throttle theo hướng per-user cho notification cá nhân để tránh user này làm chặn notification của user khác.
- Bổ sung adminReadBy/adminDeletedBy để admin history có trạng thái đọc/xóa riêng.
- Bổ sung realtime event riêng cho admin/staff qua socket notification:admin:new.
- Giữ các luồng frontend hiện có nhưng đổi nguồn dữ liệu sang API thật.
```

#### 4.5. Minh chứng

| Loại minh chứng | Nội dung |
|---|---|
| Link commit | Chưa tạo commit tại thời điểm ghi log |
| File liên quan | backend/src/models/NotificationRule.js; backend/src/services/notificationTriggers.js; backend/src/services/parkingScheduler.js; backend/src/controllers/notificationController.js; frontend/src/pages/Admin/NotificationManagement.jsx; frontend/src/pages/Customer/CustomerNotifications.jsx |
| Screenshot | Có thể chụp màn hình tab Auto Rules, Live Feed và Customer Notifications sau khi chạy frontend |
| Kết quả chạy/test | Frontend build pass; backend node --check pass; E2E API pass với MongoDB thật |
| Link video demo | Chưa có |
| Ghi chú khác | Không ghi secret trong file .env vào audit log |

#### 4.6. Nhận xét cá nhân/nhóm

```text
Nhóm học được cách dùng AI như một checklist kỹ thuật, nhưng vẫn cần tự đối chiếu implementation plan với code thực tế. AI giúp tăng tốc phần phân tích và thiết kế, còn nhóm phải tự quyết định cách tích hợp phù hợp với kiến trúc hiện có và tránh ảnh hưởng module khác.
```

---

### Lần sử dụng AI số 2

| Nội dung | Thông tin |
|---|---|
| Ngày sử dụng | 07/06/2026 - 08/06/2026 |
| Công cụ AI | ChatGPT / Codex |
| Mục đích sử dụng | Review, hoàn thiện và kiểm thử module Notification end-to-end |
| Phần việc liên quan | Backend / Frontend / Testing / Debug |
| Mức độ sử dụng | Hỗ trợ nhiều |

#### 4.1. Prompt đã sử dụng

```text
review lại bài xem implementation plan của phần module notification đã hoàn thành đầy đủ chưa, không được chỉnh sửa UI hay làm ảnh hưởng tới module khác, có thể tùy chỉnh frontend nếu cần, không được chỉnh sửa UI

vậy thì hãy hoàn thành đầy đủ đi

backend đã chạy hãy test end-to-end API
```

#### 4.2. Kết quả AI gợi ý

```text
AI rà soát code và chỉ ra các thiếu sót:
1. Admin page vẫn dùng useNotificationStore mock ở một số phần.
2. NotificationRule có throttleMinutes nhưng trigger chưa enforce throttle.
3. Parking scheduler dùng điều kiện <= 30/15/5 nên có thể gửi cảnh báo sai mốc.
4. Backend chưa hỗ trợ priority SYSTEM trong model/validator.
5. Admin history chưa hỗ trợ read/delete riêng cho admin và chưa search email.
6. Frontend build pass nhưng cần E2E API với MongoDB thật để xác nhận.
```

#### 4.3. Phần sinh viên/nhóm đã sử dụng từ AI

```text
Nhóm sử dụng các gợi ý của AI để cập nhật code:
- Thêm enum SYSTEM vào Notification và NotificationRule.
- Thêm adminReadBy/adminDeletedBy trong Notification.
- Thêm service/controller/routes cho admin history mark read, mark all read và delete.
- Cập nhật getAdminNotifications để search title, content, createdBy/targetUsers email hoặc username.
- Cập nhật notificationTriggers để kiểm tra enabled và throttle.
- Cập nhật parkingScheduler để chỉ gửi cảnh báo đúng mốc 30/15/5 phút.
- Cập nhật socket để emit notification:admin:new cho admin/staff.
- Refactor NotificationManagement.jsx để Live Feed dùng API thật và nhận realtime event.
```

#### 4.4. Phần sinh viên/nhóm tự chỉnh sửa hoặc cải tiến

```text
Nhóm kiểm tra lại bằng chính backend đang chạy và MongoDB thật:
- Tạo JWT test từ user thật trong database để gọi API do credential mock không khớp DB.
- Test admin rules list/update/test trigger.
- Test admin history search, mark read, delete/hide.
- Test admin gửi notification cho customer.
- Test customer list, unread count, mark read, delete.
- Test phân quyền customer gọi admin rules bị chặn 403.
- Test tạo và lọc notification priority SYSTEM.
```

#### 4.5. Minh chứng

| Loại minh chứng | Nội dung |
|---|---|
| Link commit | Chưa tạo commit tại thời điểm ghi log |
| File liên quan | backend/src/models/Notification.js; backend/src/models/NotificationRule.js; backend/src/services/notificationService.js; backend/src/services/notificationTriggers.js; backend/src/services/parkingScheduler.js; backend/src/sockets/notificationSocket.js; backend/src/routes/notificationRoutes.js; frontend/src/pages/Admin/NotificationManagement.jsx; frontend/src/services/notificationService.js |
| Screenshot | Có thể chụp màn hình API response hoặc giao diện Live Feed/Auto Rules |
| Kết quả chạy/test | node --check pass cho backend notification files; npm.cmd run build pass; E2E API pass với status 200/201 cho admin rules/history/customer notification; customer gọi admin rules trả 403 đúng phân quyền |
| Link video demo | Chưa có |
| Ghi chú khác | Test E2E tạo notification test trong MongoDB thật; sau test đã gọi API delete/hide ở customer/admin history |

#### 4.6. Nhận xét cá nhân/nhóm

```text
Nhóm học được rằng sau khi AI gợi ý code, cần kiểm chứng bằng nhiều lớp: syntax check, build frontend, lint file liên quan và test API thật với database. Việc test E2E giúp phát hiện các vấn đề cấu hình và phân quyền mà chỉ đọc code khó thấy hết.
```

---

### Lần sử dụng AI số 3

| Nội dung | Thông tin |
|---|---|
| Ngày sử dụng | 08/06/2026 |
| Công cụ AI | ChatGPT / Codex |
| Mục đích sử dụng | Debug lỗi frontend gọi sai backend port và sửa các lỗi lint/build trong các file liên quan |
| Phần việc liên quan | Frontend / Environment Config / Debug / Testing |
| Mức độ sử dụng | Hỗ trợ một phần |

#### 4.1. Prompt đã sử dụng

```text
bạn có thể tùy chỉnh .env hãy xử lí vấn đề ở dưới
Backend đang chạy port 5000; port 5001 không connect được.

fix lỗi frontend
```

#### 4.2. Kết quả AI gợi ý

```text
AI xác định frontend/.env đang cấu hình VITE_API_BASE_URL=http://localhost:5001/api trong khi backend health check pass ở port 5000. Ngoài ra một số file frontend hardcode trực tiếp http://localhost:5001/api nên chỉ sửa .env là chưa đủ.
```

#### 4.3. Phần sinh viên/nhóm đã sử dụng từ AI

```text
Nhóm sử dụng gợi ý để:
- Đổi frontend/.env sang VITE_API_BASE_URL=http://localhost:5000/api.
- Đổi fallback trong frontend/src/services/api.js sang http://localhost:5000/api.
- Thay các fetch hardcode 5001 trong KioskFlow, KioskStep1, KioskOutWelcome, KioskOutInvoice và Staff/SessionManagement sang API_BASE.
- Rà lại bằng rg để đảm bảo không còn localhost:5001 hoặc 5001/api.
```

#### 4.4. Phần sinh viên/nhóm tự chỉnh sửa hoặc cải tiến

```text
Nhóm tiếp tục chạy lint cục bộ trên các file vừa sửa và sửa thêm:
- Bỏ import không dùng.
- Sửa một số useEffect gọi setState đồng bộ theo rule React compiler bằng setTimeout cleanup.
- Di chuyển hoặc bọc function để tránh lỗi dùng function trước khi khai báo theo lint rule.
- Giữ nguyên UI, chỉ thay đổi logic gọi API và cleanup code.
```

#### 4.5. Minh chứng

| Loại minh chứng | Nội dung |
|---|---|
| Link commit | Chưa tạo commit tại thời điểm ghi log |
| File liên quan | frontend/.env; frontend/src/services/api.js; frontend/src/pages/Kiosk/KioskFlow.jsx; frontend/src/pages/Kiosk/KioskStep1.jsx; frontend/src/pages/KioskOut/KioskOutWelcome.jsx; frontend/src/pages/KioskOut/KioskOutInvoice.jsx; frontend/src/pages/Staff/SessionManagement.jsx |
| Screenshot | Có thể chụp terminal hiển thị build/lint pass |
| Kết quả chạy/test | rg không còn localhost:5001 hoặc 5001/api; npm.cmd run build pass; eslint riêng các file liên quan pass |
| Link video demo | Chưa có |
| Ghi chú khác | Cần restart Vite dev server sau khi đổi frontend/.env để load lại biến môi trường |

#### 4.6. Nhận xét cá nhân/nhóm

```text
Nhóm học được rằng lỗi cấu hình môi trường không chỉ nằm ở file .env mà có thể còn nằm ở các đường dẫn hardcode trong code. Khi sửa cấu hình, cần rà toàn bộ project và chạy build/lint để đảm bảo không tạo lỗi mới.
```

---

## 5. Bảng tổng hợp mức độ sử dụng AI

| Hạng mục | Không dùng AI | AI hỗ trợ ít | AI hỗ trợ nhiều | AI sinh chính | Ghi chú |
|---|:---:|:---:|:---:|:---:|---|
| Phân tích yêu cầu |  |  | x |  | Rà implementation plan module Notification |
| Viết user story/use case | x |  |  |  |  |
| Thiết kế database |  | x |  |  | Bổ sung NotificationRule, adminReadBy/adminDeletedBy dựa trên nhu cầu module |
| Thiết kế kiến trúc hệ thống |  | x |  |  | Xác định trigger, scheduler, socket, API admin/customer |
| Thiết kế giao diện | x |  |  |  | Không chỉnh UI theo yêu cầu |
| Code frontend |  | x |  |  | Refactor API wiring, env port, lint các file liên quan |
| Code backend |  |  | x |  | Notification rules, triggers, scheduler, admin history API |
| Debug lỗi |  |  | x |  | Debug port 5001/5000, mock store, priority SYSTEM, scheduler warning |
| Viết test case |  | x |  |  | Tạo script E2E API bằng Node/fetch |
| Kiểm thử sản phẩm |  |  | x |  | Build, syntax check, E2E API với MongoDB thật |
| Tối ưu code |  | x |  |  | Throttle per-user, dùng API_BASE thay hardcode URL |
| Viết báo cáo |  | x |  |  | Điền AI audit log |
| Làm slide thuyết trình | x |  |  |  |  |

---

## 6. Các lỗi hoặc hạn chế từ AI

| STT | Lỗi/hạn chế từ AI | Cách phát hiện | Cách xử lý/cải tiến |
|---:|---|---|---|
| 1 | Implementation plan ban đầu mô tả nhiều phần nhưng code thực tế chưa hoàn tất, ví dụ frontend admin vẫn dùng mock store và trigger chưa enforce throttle. | Review source code và đối chiếu từng file với implementation plan. | Bổ sung API wiring thật, throttle rule, admin history read/delete và E2E test. |
| 2 | Nếu enforce throttle toàn cục theo lastTriggeredAt, notification của user này có thể chặn notification của user khác. | Phân tích logic event user-scoped như top-up/payment/low balance. | Chỉnh throttle theo hướng per-user cho notification cá nhân; broadcast/system mới dùng throttle toàn cục. |
| 3 | AI hoặc code trước đó giả định frontend gọi backend port 5001, trong khi backend thực tế chạy port 5000. | Health check localhost:5001 fail, localhost:5000 pass; rg phát hiện nhiều hardcode 5001. | Sửa frontend/.env, API_BASE fallback và các fetch hardcode sang API_BASE. |
| 4 | Một số thay đổi frontend có thể pass build nhưng vẫn bị React compiler lint cảnh báo/lỗi. | Chạy eslint riêng trên các file frontend liên quan. | Bỏ import thừa, điều chỉnh useEffect/useCallback và cleanup timer để lint pass. |

---

## 7. Kiểm chứng kết quả AI

### Nội dung kiểm chứng

```text
1. Kiểm tra backend health:
   - GET http://localhost:5000/api/health trả success true.
   - http://localhost:5001 không connect được nên frontend được chuyển về 5000.

2. Kiểm tra syntax backend:
   - node --check cho các file notification controller, routes, models, services, scheduler, socket và validator đều pass.

3. Kiểm tra frontend:
   - npm.cmd run build pass.
   - npx.cmd eslint riêng các file liên quan đến Notification/Kiosk/Staff/API config pass.

4. Kiểm tra E2E API với MongoDB thật:
   - GET /api/notifications/admin/rules trả 22 rules.
   - PUT /api/notifications/admin/rules/wallet.low_balance pass.
   - POST /api/notifications/admin/rules/wallet.low_balance/test tạo notification test thành công.
   - GET /api/notifications/admin/history?search=wallet.low_balance tìm thấy notification test.
   - PUT /api/notifications/admin/history/:id/read cập nhật isRead true.
   - DELETE /api/notifications/admin/history/:id ẩn notification khỏi admin history.
   - POST /api/notifications gửi notification từ admin tới customer trả 201.
   - Customer GET /api/notifications thấy notification mới.
   - Customer unread count tăng đúng.
   - Customer mark read và delete pass.
   - Customer gọi /api/notifications/admin/rules bị chặn 403 đúng phân quyền.
   - Tạo notification priority SYSTEM pass và filter admin history priority=SYSTEM pass.

5. Kiểm tra không còn port sai:
   - rg localhost:5001 và 5001/api không còn kết quả trong frontend/backend.
```

---

## 8. Đóng góp cá nhân hoặc đóng góp nhóm

### 8.1. Đối với bài cá nhân

```text
- Phần sinh viên tự làm: Cung cấp yêu cầu nghiệp vụ, kiểm tra backend/frontend đang chạy, xác nhận MongoDB thật, kiểm tra kết quả trên môi trường local và quyết định phạm vi không chỉnh UI.
- Phần AI hỗ trợ: Rà soát implementation plan, chỉ ra phần thiếu, đề xuất và hỗ trợ chỉnh code backend/frontend, tạo lệnh kiểm thử build/lint/E2E API.
- Phần tự cải tiến: Điều chỉnh theo project thực tế, không đưa secret vào báo cáo, test lại với dữ liệu thật và chỉ giữ các thay đổi có thể giải thích được.
```

### 8.2. Đối với bài nhóm

| Thành viên | MSSV | Nhiệm vụ chính | Có sử dụng AI không? | Minh chứng đóng góp |
|---|---|---|---|---|
| Hồ Sỹ Hưng | DE180321 | Module Notification, cấu hình env, kiểm thử API, audit log | Có | Các file notification backend/frontend, frontend env/API config, kết quả build/lint/E2E API |
| Thành viên khác |  |  | Có / Không |  |
| Thành viên khác |  |  | Có / Không |  |
| Thành viên khác |  |  | Có / Không |  |

---

## 9. Reflection cuối bài

### 9.1. AI đã hỗ trợ em/nhóm ở điểm nào?

```text
AI hỗ trợ nhóm phân tích nhanh module Notification, phát hiện thiếu sót trong implementation plan, đề xuất hướng sửa phù hợp và tạo checklist kiểm thử. AI cũng giúp tăng tốc debug lỗi cấu hình frontend gọi sai port backend và hỗ trợ viết script test API end-to-end.
```

### 9.2. Phần nào em/nhóm không sử dụng theo gợi ý của AI? Vì sao?

```text
Nhóm không chỉnh sửa UI theo các đề xuất redesign vì yêu cầu là không làm ảnh hưởng giao diện và module khác. Nhóm cũng không đưa secret trong file .env vào audit log để tránh lộ thông tin nhạy cảm. Một số đề xuất được điều chỉnh lại, ví dụ throttle không áp dụng toàn cục cho mọi user mà xử lý per-user cho notification cá nhân.
```

### 9.3. Em/nhóm đã kiểm tra tính đúng đắn của kết quả AI như thế nào?

```text
Nhóm kiểm tra bằng build frontend, eslint riêng các file liên quan, node --check backend, health check backend, truy vấn MongoDB thật và gọi API end-to-end bằng token test hợp lệ. Kết quả được đối chiếu qua status code, response body và trạng thái dữ liệu sau khi mark read/delete.
```

### 9.4. Nếu không có AI, phần nào sẽ khó khăn nhất?

```text
Phần khó nhất là rà soát đầy đủ implementation plan lớn của module Notification, vì module liên quan nhiều lớp: model, controller, service, scheduler, socket, frontend admin và customer. Ngoài ra việc tìm hết hardcode port 5001 và tách lỗi build/lint cũng sẽ mất nhiều thời gian hơn.
```

### 9.5. Sau project này, em/nhóm học được gì về môn học?

```text
Nhóm hiểu rõ hơn cách thiết kế module backend theo hướng event-driven, cách quản lý notification realtime bằng Socket.IO, cách tách service/controller/routes/model trong Node.js, cách dùng biến môi trường cho frontend/backend và cách kiểm thử API với dữ liệu thật.
```

### 9.6. Sau project này, em/nhóm học được gì về cách sử dụng AI có trách nhiệm?

```text
Nhóm học được rằng AI chỉ nên dùng như công cụ hỗ trợ phân tích và tăng tốc, không nên copy kết quả mà không kiểm tra. Mọi thay đổi cần được review bằng source code thực tế, test build/lint/API và tránh đưa thông tin nhạy cảm như Mongo URI, JWT secret, API key hoặc email password vào báo cáo công khai.
```

---

## 10. Cam kết học thuật

Sinh viên/nhóm cam kết rằng:

- Nội dung AI hỗ trợ đã được ghi nhận trung thực.
- Không nộp nguyên văn kết quả AI mà không kiểm tra.
- Có khả năng giải thích các phần đã nộp.
- Chịu trách nhiệm về tính đúng đắn của sản phẩm cuối cùng.
- Hiểu rằng việc sử dụng AI không khai báo có thể ảnh hưởng đến kết quả đánh giá.

| Đại diện sinh viên/nhóm | Ngày xác nhận |
|---|---|
| Hồ Sỹ Hưng / Valo-park-group1 | 08/06/2026 |
