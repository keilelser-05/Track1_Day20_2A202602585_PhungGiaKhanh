# Metrics Pack — Phùng Gia Khánh · 2A202602585

> Bản mẫu Day 20. Chưa có số liệu đo thực tế. Cadence, ngưỡng chất lượng và activation cần được lập luận, không chọn chỉ để làm đẹp dashboard.

## 00 — Dự án, persona, core job

| Mục | Câu trả lời |
| --- | --- |
| Dự án | RAV-26 — hỗ trợ chọn lọc dữ liệu cho xe tự lái |
| Một persona | Reviewer — người kiểm duyệt batch dữ liệu |
| Một use case chính | Kiểm duyệt và xuất batch để chuyển sang gán nhãn |
| Core job — bằng lời người dùng | “Tôi cần chọn được bộ ảnh đáng gán nhãn, đủ chất lượng và ít trùng lặp, mà không mất quá nhiều thời gian kiểm tra.” |
| Phạm vi và điều không phân tích | Chỉ phân tích bước kiểm duyệt và xuất batch; chưa phân tích nhập dữ liệu, chạy mô hình hoặc quản lý tài khoản. |

**Lý do chọn phạm vi:** Đây là bước người dùng quyết định bộ ảnh có phù hợp để chuyển sang gán nhãn hay không, gần giá trị sử dụng hơn việc hệ thống chỉ tạo batch. Việc xuất batch chưa chứng minh dữ liệu cải thiện mô hình; core action và bằng chứng giá trị sẽ được định nghĩa ở Phase 1.

## 01 — Core Action

### Phân biệt bốn khái niệm

| Khái niệm | Nội dung |
| --- | --- |
| Core job — việc người dùng muốn hoàn thành | Chọn bộ ảnh đáng gán nhãn, đủ chất lượng và ít trùng lặp |
| Core action — hành vi người dùng thực hiện | Kiểm duyệt và xuất một batch đạt yêu cầu |
| Core value — lợi ích người dùng nhận được | Có bộ ảnh đã kiểm tra, sẵn sàng chuyển sang gán nhãn |
| Core value event — dấu hiệu giá trị đã xảy ra | batch_export_completed — gói dữ liệu đã được tải xuống thành công |

### Core Action Card

| Thành phần | Câu trả lời |
| --- | --- |
| Target user — ai thực hiện | Reviewer |
| Core job | Chọn bộ ảnh phù hợp để chuyển sang gán nhãn |
| Core action | Kiểm duyệt và xuất một batch đạt yêu cầu |
| Object — đối tượng của hành vi | Một batch dữ liệu có phiên bản xác định |
| Preconditions — điều kiện trước hành vi | Batch đã được gửi duyệt; Reviewer có quyền kiểm duyệt; có tiêu chí chất lượng |
| Completion rule — khi nào hoàn tất | Quyết định duyệt được lưu; batch đáp ứng tiêu chí chất lượng; gói xuất được tải xuống thành công |
| Core value | Nhận được bộ ảnh đã kiểm tra để chuyển sang gán nhãn |
| Evidence of value — dấu hiệu và giới hạn bằng chứng | Có bản ghi duyệt và xác nhận tải gói dữ liệu thành công |
| Candidate event — sự kiện dự kiến | batch_export_completed |

> Lưu ý: “Đạt yêu cầu” cần có tiêu chí rõ về ảnh lỗi, thông tin cá nhân và trùng lặp. Chưa tự đặt ngưỡng số khi chưa có căn cứ. Xuất thành công cũng chưa chứng minh ảnh giúp mô hình tốt hơn.

### Tự kiểm năm tiêu chí

| Tiêu chí | Đánh giá và lý do |
| --- | --- |
| Gần giá trị | Đạt — người dùng nhận được bộ ảnh có thể chuyển sang bước tiếp theo |
| Có thể lặp lại | Đạt — mỗi đợt dữ liệu mới lại cần kiểm duyệt |
| Quan sát được | Đạt về thiết kế — có điều kiện duyệt và xuất hoàn tất; cần kiểm tra khả năng ghi nhận |
| Có ý nghĩa | Đạt có điều kiện — chỉ đếm batch đạt chất lượng, không đếm tải lại hoặc chia nhỏ batch để tăng số |
| Có thể tác động | Đạt — cải thiện cách xem ảnh, kiểm duyệt và xuất dữ liệu |

**Kết luận:** Đạt — chọn core action này vì người dùng đã kiểm tra và nhận bộ dữ liệu; việc AI tạo batch mới chỉ là đầu ra của hệ thống.

## 02 — Nature & cadence

### Action Nature Card

| Thành phần | Câu trả lời |
| --- | --- |
| Actor — user/account/team/object | Reviewer |
| Intent — nhu cầu khởi đầu | Kiểm tra bộ ảnh trước khi chuyển sang gán nhãn |
| Trigger — điều gì kích hoạt nhu cầu | Curator gửi batch mới cần kiểm duyệt |
| Effort — thời gian, suy nghĩ, dữ liệu | Xem ảnh, kiểm tra tiêu chí, quyết định duyệt hoặc yêu cầu sửa; thời gian phụ thuộc số ảnh và chất lượng |
| Value timing — khi nào nhận giá trị | Khi batch đạt yêu cầu và gói dữ liệu được tải xuống thành công; có thể chậm nếu cần sửa |
| State — dữ liệu/trạng thái được giữ lại | Phiên bản batch, quyết định duyệt, lý do và lịch sử xuất |
| Dependency — phụ thuộc người khác, phê duyệt, nguồn cung | Curator chuẩn bị batch, chất lượng dữ liệu và tiến độ gán nhãn của nhóm |
| Repeat condition — vì sao cần lặp lại | Có batch mới thuộc đợt dữ liệu tiếp theo cần kiểm duyệt |

**Dạng hành vi chính:** Quy trình làm việc của nhóm (workflow).

**Kết luận cadence:**

> Đối với Reviewer, hành vi kiểm duyệt và xuất batch đạt yêu cầu xuất hiện theo từng đợt dữ liệu vì nhóm cần chuẩn bị ảnh cho bước gán nhãn. Do đó, nhịp đo phù hợp là từng đợt dữ liệu ở cấp Reviewer.

**Căn cứ và điều cần kiểm chứng:** Đây là giả thuyết dựa trên luồng công việc của RAV-26. Cần ghi nhận thời điểm gửi batch và hoàn tất kiểm duyệt qua nhiều đợt để biết nhịp thực tế. Chưa có căn cứ chọn hằng ngày, hằng tuần hoặc hằng tháng.

**Frequency cao hơn có luôn tốt hơn không?** Không. Nhiều lượt xuất có thể do tải lại, chia nhỏ batch hoặc sửa lỗi. Điều tốt hơn là hoàn tất dữ liệu đạt chất lượng trong thời gian hợp lý.

## 03 — Metric System

### Activation

| Thành phần | Định nghĩa |
| --- | --- |
| Start event | reviewer_first_assigned — Reviewer được giao batch đầu tiên cần kiểm duyệt |
| First-value event — core action đầu tiên hoàn tất | batch_export_completed đầu tiên thỏa completion rule ở mục 01 |
| Activation event/rule — đủ lần lặp để dự kiến giữ chân tốt hơn | Tạm dùng first value làm activation tạm thời: ≥1 batch_export_completed hợp lệ trong đợt dữ liệu đầu tiên được giao |
| Time window từ start | Trong đợt dữ liệu đầu tiên được giao (chưa có căn cứ chọn số ngày/tuần cố định) |
| Công thức activation rate — tử số / mẫu số | Số Reviewer đạt activation / Số Reviewer được giao ≥1 batch và đã hết đợt quan sát |
| Căn cứ chọn ngưỡng và cách kiểm chứng | Chưa có dữ liệu retention để chọn ngưỡng lặp lại; tạm dùng first value và ghi rõ giới hạn chưa dự báo giữ chân. Kiểm chứng bằng cách đối chiếu nhóm activated với tỉ lệ quay lại ở đợt sau khi có dữ liệu. |

Phân biệt: có hoạt động trong window ≠ đã activated. Nếu chỉ dùng first value làm activation tạm thời, ghi rõ giới hạn; chưa khẳng định dự báo retention.

### Engagement — tối đa hai góc đo

| Metric | Frequency / depth / breadth | Công thức, unit, window | Event sử dụng |
| --- | --- | --- | --- |
| E1 — Số batch đạt yêu cầu xuất thành công | Frequency | Count distinct (batch_id, batch_version) có batch_export_completed hợp lệ / Reviewer / đợt dữ liệu | batch_export_completed |
| E2 — Tỉ lệ duyệt ngay lần đầu (first-pass approval rate) | Depth / chất lượng duyệt | Số batch duyệt ngay ở quyết định đầu / Tổng số batch đã có quyết định / đợt dữ liệu | batch_review_decided (decision, batch_version), batch_assigned để xác định mẫu số |

### North Star Metric

| Thành phần | Định nghĩa |
| --- | --- |
| Unit of value — đơn vị giá trị | Một batch đạt yêu cầu đã xuất thành công |
| Quality threshold — điều kiện chất lượng | Có quyết định duyệt đã lưu và đáp ứng tiêu chí ảnh lỗi, thông tin cá nhân, trùng lặp; chưa đặt ngưỡng số khi chưa có căn cứ |
| Frequency/window — khớp mục 02 | Theo đợt dữ liệu, ở cấp toàn nhóm (tổng hợp từ Reviewer), khớp cadence mục 02 |
| NSM hoàn chỉnh và công thức | Số batch đạt yêu cầu được xuất thành công trong một đợt dữ liệu |
| Event và thuộc tính cần để tính | batch_export_completed + thuộc tính batch_id, batch_version, reviewer_id, data_batch_id (đợt), decision_ref, exported_at |

### Leading indicators — tối đa ba

| Metric | Định nghĩa/công thức/window | Vì sao dự kiến báo trước core action lặp lại | Event |
| --- | --- | --- | --- |
| L1 — Số batch được mở xem | Count batch_view_started / Reviewer / đợt dữ liệu | Mở xem là bước bắt buộc trước duyệt và xuất; không xem thì không thể hoàn tất | batch_view_started |
| L2 — Tỉ lệ quyết định duyệt ở lần đầu | Số batch có decision=approve ở lần quyết định đầu / Tổng số batch được quyết định / đợt | Duyệt sớm báo trước xuất thành công trong cùng đợt; phải sửa nhiều thì xuất chậm | batch_review_decided |
| L3 — Thời gian trung vị từ giao đến bắt đầu duyệt | Median(batch_view_started_at − batch_assigned_at) / đợt | Bắt đầu sớm tăng khả năng hoàn tất trong đợt; bắt đầu muộn cảnh báo tắc nghẽn | batch_assigned, batch_view_started |

Đây là giả thuyết dự báo, cần đối chiếu với retention thực tế.

### Counter-metric — ít nhất một

| Metric | Điều không được xấu đi khi NSM tăng | Công thức/window/ngưỡng | Event |
| --- | --- | --- | --- |
| C1 — Tỉ lệ batch bị nhóm gán nhãn trả về | Chất lượng sau xuất không được xấu đi khi tăng số batch xuất | Số batch_id đã xuất bị trả về / Tổng số batch_export_completed hợp lệ / cùng đợt; ngưỡng chưa đặt khi chưa có căn cứ, theo dõi xu hướng | batch_export_completed, batch_returned_by_labeling |

### Quy ước tính chung

- Identity và unit đếm: reviewer_id cho Activation/Engagement/Retention; batch (cặp batch_id + batch_version) cho NSM. Mỗi (batch_id, batch_version) chỉ tính một lần.
- Đối tượng và phiên bản được tính: chỉ batch có phiên bản xác định, có quyết định duyệt đã lưu và thỏa completion rule mục 01. Tải lại cùng version không tính thêm.
- Timezone, mốc đầu/cuối window: window = một đợt dữ liệu (data_batch_id). Mốc đầu = thời điểm giao batch đầu tiên của đợt; mốc cuối = thời điểm đóng đợt. Timestamp lưu ISO 8601, múi giờ UTC+7.
- Loại trừ tài khoản nội bộ/test/bot: loại trừ tài khoản test, demo, bot và batch demo khỏi mọi tử số/mẫu số.
- Quy tắc chống đếm trùng: khóa chống trùng (reviewer_id, batch_id, batch_version, event_name) + export_id; reload/retry/autosave/tải lại không tạo thêm một hành vi.
- Mẫu số bằng 0 hoặc chưa đủ thời gian quan sát: nếu không có unit đủ điều kiện hoặc đợt chưa kết thúc thì báo “chưa đủ dữ liệu”, không tính tỉ lệ bằng 0% hay 100%.

## 04 — Retention Definition

| Thành phần bắt buộc | Định nghĩa |
| --- | --- |
| Unit | Reviewer (khớp cấp đo ở mục 02) |
| Cohort entry — sự kiện vào nhóm | batch_export_completed đầu tiên thỏa completion rule mục 01 (first value) |
| Return event — core action/value phải lặp lại | batch_export_completed hợp lệ ở đợt dữ liệu tiếp theo |
| Window — thời gian quay lại | Đợt dữ liệu kế tiếp mà Reviewer được giao ≥1 batch (không dùng số ngày/tuần cố định khi chưa có căn cứ) |
| Threshold — số lần tối thiểu trong window | ≥1 lần hợp lệ trong window |
| Segment — áp dụng cho ai | Reviewer được giao ≥1 batch ở cả đợt gốc và đợt kế tiếp (có cơ hội quay lại) |

**Công thức:** [Số Reviewer của cohort đạt return event và threshold trong window] / [số Reviewer hợp lệ của cohort đã đủ thời gian quan sát (đợt kế tiếp đã kết thúc)].

**Phân biệt không có nhu cầu mới với không quay lại dù có nhu cầu:** dùng retention có điều kiện theo cơ hội (conditional retention given opportunity). Reviewer không được giao batch mới trong đợt kế tiếp thì loại khỏi mẫu số, không tính là rời bỏ; đặt tên riêng khi báo cáo.

### Đối chiếu ba mốc

| Mốc | Cách đối chiếu |
| --- | --- |
| Natural cycle — chu kỳ nhu cầu thật | Đợt dữ liệu của RAV-26 (mục 02); window retention = đợt kế tiếp, không quy ra ngày/tuần khi chưa đo thực tế |
| Cohort đúng segment | Chỉ tính Reviewer có batch được giao ở cả hai đợt; ghi rõ số bị loại vì không có cơ hội |
| Benchmark cùng loại sản phẩm và định nghĩa tương thích | Chưa có; không tự tạo số liệu |

## 05 — Product Loop

**Loại loop chính:** Vòng lặp workflow của nhóm (team workflow loop) — Reviewer quay lại vì có batch mới cần xử lý để unblock gán nhãn, không phải vì viral hay notification.

| Bước | Chu kỳ 1 | Chu kỳ 2 |
| --- | --- | --- |
| Natural trigger — nhu cầu tự nhiên | Curator gửi batch mới thuộc đợt hiện tại cần kiểm duyệt | Curator gửi batch mới thuộc đợt tiếp theo cần kiểm duyệt |
| Core action | Kiểm duyệt và xuất batch đạt yêu cầu (batch_export_completed) | Kiểm duyệt và xuất batch đạt yêu cầu của đợt tiếp theo |
| Immediate/repeat value | Có bộ ảnh đã kiểm tra, sẵn sàng chuyển sang gán nhãn | Có thêm bộ ảnh đã kiểm tra cho đợt tiếp theo |
| Saved state/investment — điều giữ lại cho lượt sau | Phiên bản batch, quyết định duyệt, lý do và lịch sử xuất được lưu lại | Lịch sử duyệt/xuất tích lũy giúp lần sau duyệt nhanh và nhất quán hơn |

**Reason to return khi bỏ notification:** vẫn có batch mới được giao cần xử lý để không chặn bước gán nhãn; nhu cầu công việc kéo Reviewer quay lại mà không cần nhắc mở app.

**Metric hypothesis:**

> Nếu loop này hoạt động, metric E1 (số batch hoàn tất / Reviewer / đợt) sẽ ổn định và thời gian trung vị từ giao đến xuất sẽ giảm qua các đợt, vì trạng thái lưu (quyết định, lý do, lịch sử xuất) giúp duyệt nhanh hơn và batch mới tạo nhu cầu quay lại.

**Cách thử và yếu tố có thể gây nhiễu:** so sánh E1, L3 và NSM theo từng đợt dữ liệu; kiểm soát số batch được giao, chất lượng ảnh đầu vào và thay đổi nhân sự Reviewer/Curator. Không suy ra loop hiệu quả chỉ từ tương quan hoặc số lượt mở app tăng.

## 06 — Tracking nhanh

Điền 4–8 core events. Tên theo dạng object_action; chỉ giữ event tính được metric.

| Tên event | Ý nghĩa — điều đã xảy ra | Thời điểm ghi nhận chính xác | Metric sử dụng ở mục 03/04 |
| --- | --- | --- | --- |
| batch_assigned | Curator giao một batch (có version) cho Reviewer | Khi bản ghi giao việc đã lưu thành công, kèm batch_id, batch_version, reviewer_id, data_batch_id, assigned_at | Activation (mẫu số), E2 (mẫu số), L3, Retention (cơ hội quay lại) |
| batch_view_started | Reviewer mở xem batch để kiểm duyệt | Khi màn hình duyệt mở thành công với đúng batch_id + version, kèm reviewer_id, data_batch_id, viewed_at | L1, L3 |
| batch_review_decided | Quyết định duyệt hoặc yêu cầu sửa đã được lưu | Khi quyết định + lý do + version đã lưu thành công (không tính bấm nút chưa lưu/thất bại), kèm decision, decided_at | E2, L2 |
| batch_export_completed | Gói dữ liệu của batch đạt yêu cầu đã được tải xuống thành công | Khi file xuất đã tạo xong và xác nhận tải thành công cho đúng (batch_id, version) đã duyệt, kèm export_id, exported_at | Activation, E1, NSM, Retention (entry/return) |
| batch_returned_by_labeling | Nhóm gán nhãn trả về batch đã xuất để làm lại | Khi bản ghi trả về đã lưu thành công, kèm batch_id, batch_version, reason, returned_at | C1 (counter-metric) |

Có thể thêm tối đa bốn hàng. Với metric cần thời gian/chất lượng, bổ sung thuộc tính hoặc event đủ để tính; không chỉ ghi tên metric.

### Tiêu chí nghiệm thu

1. **Hoàn tất thật:** Event xác nhận core action chỉ được ghi khi completion rule ở mục 01 được thỏa mãn và trạng thái đã lưu thành công. Bấm nút, tác vụ thất bại hoặc đang xử lý không được tính hoàn tất. Cụ thể: batch_export_completed chỉ ghi khi đã có batch_review_decided=approve cho đúng version và xác nhận tải gói thành công.
2. **Không ghi trùng:** Với cùng actor, object, version và lần chuyển trạng thái, reload/retry/autosave không tạo thêm một hành vi. Chốt khóa chống trùng cụ thể sau khi chọn event. Cụ thể: khóa (reviewer_id, batch_id, batch_version, event_name) + export_id cho xuất; tải lại cùng version không tạo batch_export_completed mới.
3. **Đủ dữ liệu tính:** Mỗi metric phải có event/thuộc tính để tính tử số, mẫu số, chất lượng và window. Thiếu dữ liệu phải báo thiếu, không tự coi là đạt. Cụ thể: mọi event mang data_batch_id để tính theo đợt; batch_export_completed mang decision_ref để đối chiếu chất lượng.
4. **Lần lặp hợp lệ:** Phân biệt một hành vi mới với tải lại hoặc xuất lại cùng kết quả; nêu rõ quy tắc trước khi tính engagement/retention. Cụ thể: chỉ đếm distinct (batch_id, batch_version) có duyệt hợp lệ; xuất lại/tải lại cùng version chỉ tính một lần cho E1/NSM/Retention.

### Tự soi lỗi và năm gate

- [x] Gate 1: Core action có actor/object/completion rule, qua năm tiêu chí.
- [x] Gate 2: Cadence theo template và có lý do từ nature.
- [x] Gate 3: Retention đủ sáu thành phần; NSM có value + quality + frequency; có counter-metric.
- [x] Gate 4: Loop đủ hai chu kỳ, có reason to return và metric hypothesis.
- [x] Gate 5: Có 4–8 events map về metric và ít nhất hai tiêu chí nghiệm thu cụ thể.
- [x] Mọi event map về metric; mọi metric có event/thuộc tính đủ để tính.
- [x] README có link Metrics Pack xem được.
- [x] AI Support Log phản ánh đúng việc đã dùng AI.
- [ ] Phần áp dụng cho dự án thật đã được người nộp điền.

### Revision — lý do thay đổi lớn

| Ngày | Thay đổi | Lý do |
| --- | --- | --- |
| 06/10/2026 | Tạo khung 00–06; chưa chốt core action/cadence/metric | Chuẩn bị repo mẫu để làm bài cá nhân, chưa có bằng chứng đo thực tế |
| 06/10/2026 | Chốt Phase 0: Reviewer, kiểm duyệt và xuất batch | Giữ một persona và một use case gần giá trị sử dụng; các Phase 1–5 chưa hoàn thiện |
| 06/10/2026 | Hoàn thiện 01–06: core action batch_export_completed, cadence theo đợt, metric/retention/loop/5 events, quy ước chống trùng | Khớp logic workflow Reviewer; không đặt ngưỡng số khi chưa có căn cứ; mọi metric có event để tính |
