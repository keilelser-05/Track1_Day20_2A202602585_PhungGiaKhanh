# Metrics Pack — Phùng Gia Khánh · 2A202602585

> Bản mẫu Day 20. Chưa có số liệu đo thực tế. Cadence, ngưỡng chất lượng và activation cần được lập luận, không chọn chỉ để làm đẹp dashboard.

## 00 — Dự án, persona, core job

| Mục | Câu trả lời |
| --- | --- |
| Dự án | RAV-26 — chọn lọc dữ liệu cho xe tự lái (dự kiến) |
| Một persona | Chưa chốt |
| Một use case chính | Chưa chốt |
| Core job — bằng lời người dùng | Chưa chốt |
| Phạm vi và điều không phân tích | Chưa chốt |

## 01 — Core Action

### Phân biệt bốn khái niệm

| Khái niệm | Câu trả lời |
| --- | --- |
| Core job — việc người dùng muốn hoàn thành | Chưa chốt |
| Core action — hành vi người dùng thực hiện | Chưa chốt |
| Core value — lợi ích người dùng nhận được | Chưa chốt |
| Core value event — dấu hiệu giá trị đã xảy ra | Chưa chốt |

### Core Action Card

| Thành phần | Câu trả lời |
| --- | --- |
| Target user — ai thực hiện | Chưa chốt |
| Core job | Chưa chốt |
| Core action | Chưa chốt |
| Object — đối tượng của hành vi | Chưa chốt |
| Preconditions — điều kiện trước hành vi | Chưa chốt |
| Completion rule — khi nào hoàn tất | Chưa chốt |
| Core value | Chưa chốt |
| Evidence of value — dấu hiệu và giới hạn bằng chứng | Chưa chốt |
| Candidate event — sự kiện dự kiến | Chưa chốt |

### Tự kiểm năm tiêu chí

| Tiêu chí | Đạt / chưa đạt | Lý do |
| --- | --- | --- |
| Gần giá trị cốt lõi | Chưa đánh giá | |
| Lặp lại khi nhu cầu quay lại | Chưa đánh giá | |
| Quan sát được thời điểm hoàn tất | Chưa đánh giá | |
| Có ý nghĩa; tăng không chỉ do thao tác thừa | Chưa đánh giá | |
| Nhóm có thể cải thiện khả năng xảy ra | Chưa đánh giá | |

**Kết luận:** Chưa đánh giá. Trượt từ hai tiêu chí trở lên thì chọn lại core action.

## 02 — Nature & cadence

### Action Nature Card

| Thành phần | Câu trả lời |
| --- | --- |
| Actor — user/account/team/object | Chưa chốt |
| Intent — nhu cầu khởi đầu | Chưa chốt |
| Trigger — điều gì kích hoạt nhu cầu | Chưa chốt |
| Effort — thời gian, suy nghĩ, dữ liệu | Chưa chốt |
| Value timing — khi nào nhận giá trị | Chưa chốt |
| State — dữ liệu/trạng thái được giữ lại | Chưa chốt |
| Dependency — phụ thuộc người khác, phê duyệt, nguồn cung | Chưa chốt |
| Repeat condition — vì sao cần lặp lại | Chưa chốt |

**Dạng hành vi chính:** Chưa chốt — chọn một: thói quen thường xuyên / tiến trình tích lũy / theo dự án / giao dịch / workflow của team / phản ứng theo sự kiện / theo chu kỳ.

**Kết luận cadence:**

> Đối với [persona], core action [hành vi] thường xuất hiện [nhịp] vì [lý do từ nhu cầu thật]. Do đó, nhịp đo phù hợp là [window] ở cấp [unit].

**Căn cứ và điều cần kiểm chứng:** Chưa điền. Nếu chưa biết nhịp thật, ghi rõ giả thuyết và cách thu dữ liệu để kiểm tra.

**Frequency cao hơn có luôn tốt hơn không?** Chưa điền.

## 03 — Metric System

### Activation

| Thành phần | Định nghĩa |
| --- | --- |
| Start event | Chưa chốt |
| First-value event — core action đầu tiên hoàn tất | Chưa chốt |
| Activation event/rule — đủ lần lặp để dự kiến giữ chân tốt hơn | Chưa chốt |
| Time window từ start | Chưa chốt |
| Công thức activation rate — tử số / mẫu số | Chưa chốt |
| Căn cứ chọn ngưỡng và cách kiểm chứng | Chưa chốt |

Phân biệt: có hoạt động trong window ≠ đã activated. Nếu chỉ dùng first value làm activation tạm thời, ghi rõ giới hạn; chưa khẳng định dự báo retention.

### Engagement — tối đa hai góc đo

| Metric | Frequency / depth / breadth | Công thức, unit, window | Event sử dụng |
| --- | --- | --- | --- |
| E1 — Chưa chốt | | | |
| E2 — Tùy chọn | | | |

### North Star Metric

| Thành phần | Định nghĩa |
| --- | --- |
| Unit of value — đơn vị giá trị | Chưa chốt |
| Quality threshold — điều kiện chất lượng | Chưa chốt |
| Frequency/window — khớp mục 02 | Chưa chốt |
| NSM hoàn chỉnh và công thức | Chưa chốt |
| Event và thuộc tính cần để tính | Chưa chốt |

### Leading indicators — tối đa ba

| Metric | Định nghĩa/công thức/window | Vì sao dự kiến báo trước core action lặp lại | Event |
| --- | --- | --- | --- |
| L1 — Chưa chốt | | | |
| L2 — Tùy chọn | | | |
| L3 — Tùy chọn | | | |

Đây là giả thuyết dự báo, cần đối chiếu với retention thực tế.

### Counter-metric — ít nhất một

| Metric | Điều không được xấu đi khi NSM tăng | Công thức/window/ngưỡng | Event |
| --- | --- | --- | --- |
| C1 — Chưa chốt | | | |

### Quy ước tính chung

- Identity và unit đếm: Chưa chốt.
- Đối tượng và phiên bản được tính: Chưa chốt.
- Timezone, mốc đầu/cuối window: Chưa chốt.
- Loại trừ tài khoản nội bộ/test/bot: Chưa chốt.
- Quy tắc chống đếm trùng: Chưa chốt.
- Mẫu số bằng 0 hoặc chưa đủ thời gian quan sát: Chưa chốt.

## 04 — Retention Definition

| Thành phần bắt buộc | Định nghĩa |
| --- | --- |
| Unit | Chưa chốt |
| Cohort entry — sự kiện vào nhóm | Chưa chốt |
| Return event — core action/value phải lặp lại | Chưa chốt |
| Window — thời gian quay lại | Chưa chốt |
| Threshold — số lần tối thiểu trong window | Chưa chốt |
| Segment — áp dụng cho ai | Chưa chốt |

**Công thức:** [Số unit của cohort đạt return event và threshold trong window] / [số unit hợp lệ của cohort đã đủ thời gian quan sát].

**Phân biệt không có nhu cầu mới với không quay lại dù có nhu cầu:** Chưa chốt cách xử lý; nếu dùng retention có điều kiện theo cơ hội, đặt tên riêng và nêu rõ mẫu số.

### Đối chiếu ba mốc

| Mốc | Cách đối chiếu |
| --- | --- |
| Natural cycle — chu kỳ nhu cầu thật | Chưa điền |
| Cohort đúng segment | Chưa điền |
| Benchmark cùng loại sản phẩm và định nghĩa tương thích | Chưa có; không tự tạo số liệu |

## 05 — Product Loop

**Loại loop chính:** Chưa chốt.

| Bước | Chu kỳ 1 | Chu kỳ 2 |
| --- | --- | --- |
| Natural trigger — nhu cầu tự nhiên | Chưa điền | Chưa điền |
| Core action | Chưa điền | Chưa điền |
| Immediate/repeat value | Chưa điền | Chưa điền |
| Saved state/investment — điều giữ lại cho lượt sau | Chưa điền | Chưa điền |

**Reason to return khi bỏ notification:** Chưa điền.

**Metric hypothesis:**

> Nếu loop này hoạt động, metric [tên đã định nghĩa ở mục 03/04] sẽ [hướng thay đổi] trong [khung thời gian], vì [cơ chế tạo giá trị và lý do quay lại].

**Cách thử và yếu tố có thể gây nhiễu:** Chưa điền. Không suy ra loop hiệu quả chỉ từ tương quan hoặc số lượt mở app tăng.

## 06 — Tracking nhanh

Điền 4–8 core events. Tên theo dạng object_action; chỉ giữ event tính được metric.

| Tên event | Ý nghĩa — điều đã xảy ra | Thời điểm ghi nhận chính xác | Metric sử dụng ở mục 03/04 |
| --- | --- | --- | --- |
| Event 1 — Chưa chốt | | | |
| Event 2 — Chưa chốt | | | |
| Event 3 — Chưa chốt | | | |
| Event 4 — Chưa chốt | | | |

Có thể thêm tối đa bốn hàng. Với metric cần thời gian/chất lượng, bổ sung thuộc tính hoặc event đủ để tính; không chỉ ghi tên metric.

### Tiêu chí nghiệm thu

1. **Hoàn tất thật:** Event xác nhận core action chỉ được ghi khi completion rule ở mục 01 được thỏa mãn và trạng thái đã lưu thành công. Bấm nút, tác vụ thất bại hoặc đang xử lý không được tính hoàn tất.
2. **Không ghi trùng:** Với cùng actor, object, version và lần chuyển trạng thái, reload/retry/autosave không tạo thêm một hành vi. Chốt khóa chống trùng cụ thể sau khi chọn event.
3. **Đủ dữ liệu tính:** Mỗi metric phải có event/thuộc tính để tính tử số, mẫu số, chất lượng và window. Thiếu dữ liệu phải báo thiếu, không tự coi là đạt.
4. **Lần lặp hợp lệ:** Phân biệt một hành vi mới với tải lại hoặc xuất lại cùng kết quả; nêu rõ quy tắc trước khi tính engagement/retention.

### Tự soi lỗi và năm gate

- [ ] Gate 1: Core action có actor/object/completion rule, qua năm tiêu chí.
- [ ] Gate 2: Cadence theo template và có lý do từ nature.
- [ ] Gate 3: Retention đủ sáu thành phần; NSM có value + quality + frequency; có counter-metric.
- [ ] Gate 4: Loop đủ hai chu kỳ, có reason to return và metric hypothesis.
- [ ] Gate 5: Có 4–8 events map về metric và ít nhất hai tiêu chí nghiệm thu cụ thể.
- [ ] Mọi event map về metric; mọi metric có event/thuộc tính đủ để tính.
- [ ] README có link Metrics Pack xem được.
- [ ] AI Support Log phản ánh đúng việc đã dùng AI.
- [ ] Phần áp dụng cho dự án thật đã được người nộp điền.

### Revision — lý do thay đổi lớn

| Ngày | Thay đổi | Lý do |
| --- | --- | --- |
| 06/10/2026 | Tạo khung 00–06; chưa chốt core action/cadence/metric | Chuẩn bị repo mẫu để làm bài cá nhân, chưa có bằng chứng đo thực tế |
