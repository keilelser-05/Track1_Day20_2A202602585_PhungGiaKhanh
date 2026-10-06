# Metrics Pack — Phùng Gia Khánh · 2A202602585

> Track 1 · Day 20 · RAV-26. Hoàn thiện thiết kế đo lường, chưa có số liệu thực tế hoặc kết quả coach chấm. Các event và thuộc tính dưới đây là yêu cầu tracking đề xuất; chưa kiểm tra hoặc triển khai trong mã RAV-26. Nguồn: brief người nộp cung cấp, quyết định trong cuộc trao đổi và bản bài đã đẩy lên repo; chưa đọc deck Day 20.

## 00 — Dự án, persona, core job

| Mục | Câu trả lời |
| --- | --- |
| Dự án | RAV-26 — hỗ trợ chọn lọc dữ liệu cho xe tự lái |
| Một persona | Reviewer — người kiểm duyệt batch dữ liệu |
| Một use case chính | Kiểm duyệt và xuất batch để chuyển sang gán nhãn |
| Core job — bằng lời người dùng | “Tôi cần chọn được bộ ảnh đáng gán nhãn, đủ chất lượng và ít trùng lặp, mà không mất quá nhiều thời gian kiểm tra.” |
| Phạm vi | Kiểm duyệt và xuất batch; không phân tích nhập dữ liệu, chạy mô hình hoặc quản lý tài khoản |

Đây là bước Reviewer kiểm tra và nhận bộ dữ liệu có thể chuyển sang gán nhãn, gần giá trị sử dụng hơn việc AI chỉ tạo batch. Định hướng là sản phẩm hoàn chỉnh; phạm vi hẹp chỉ để phân tích sâu một use case trong lab.

## 01 — Core Action

### Phân biệt bốn khái niệm

| Khái niệm | Nội dung |
| --- | --- |
| Core job | Chọn bộ ảnh phù hợp để chuyển sang gán nhãn |
| Core action | Kiểm duyệt và xuất một batch đạt yêu cầu |
| Core value | Có bộ ảnh đã kiểm tra, sẵn sàng chuyển sang gán nhãn |
| Core value event | `batch_export_completed` — đã duyệt đúng phiên bản và có xác nhận nhận gói dữ liệu đầy đủ |

### Core Action Card

| Thành phần | Câu trả lời |
| --- | --- |
| Target user | Reviewer được giao kiểm duyệt |
| Core job | Chọn bộ ảnh phù hợp để chuyển sang gán nhãn |
| Core action | Kiểm duyệt và xuất một batch đạt yêu cầu |
| Object | Batch có batch_id, batch_version và manifest cố định |
| Preconditions | Batch thật được giao hợp lệ; Reviewer có quyền; tiêu chí chất lượng được khóa theo quality_policy_version |
| Completion rule | Quyết định approve đã lưu cho đúng phiên bản; kiểm tra chất lượng đạt; gói xuất đầy đủ; ứng dụng nhận xác nhận nhận gói hợp lệ; có ít nhất một ảnh mới đủ điều kiện trong phạm vi dự án |
| Core value | Có bộ ảnh đã kiểm tra để chuyển sang gán nhãn |
| Evidence of value | Quyết định duyệt, manifest, kết quả kiểm tra và receipt xác nhận nhận gói; là bằng chứng bàn giao, chưa chứng minh ảnh được gán nhãn hoặc giúp mô hình tốt hơn |
| Candidate event | `batch_export_completed` |

**Điều kiện chất lượng vận hành đề xuất:** Reviewer xác nhận ảnh phù hợp mục tiêu đợt; ảnh đọc được; không còn lỗi PII đã phát hiện chưa xử lý; loại ảnh trùng hoàn toàn trong bộ xuất. Lưu phiên bản tiêu chí và kết quả kiểm tra. Nếu thiếu kết quả thì không tính là đạt. Đây không phải bảo đảm tuyệt đối rằng mọi PII hoặc ảnh gần trùng đã được phát hiện.

**Xác nhận nhận gói:** Đề xuất ứng dụng kiểm tra đủ byte và checksum của gói rồi nhận xác nhận “đã nhận gói” từ Reviewer, lưu receipt phía máy chủ. Tạo ZIP, HTTP 200 hoặc click tải chưa đủ. Trình duyệt thông thường không xác nhận chắc chắn file đã lưu vào ổ đĩa; nếu chưa có cơ chế receipt, chỉ báo “gói xuất đã tạo”, chưa báo core value event này.

**Ảnh mới:** hash nội dung ảnh đầu ra chưa có trong một lần bàn giao hợp lệ trước đó của cùng project_id. Xuất lại, sửa version hoặc chia nhỏ cùng bộ ảnh không tạo thêm giá trị. Ảnh sửa thực chất có hash mới vẫn có thể gần trùng; theo dõi bằng kiểm tra chất lượng sau xuất.

### Tự kiểm năm tiêu chí

| Tiêu chí | Kết quả và lý do |
| --- | --- |
| Gần core value | Đạt về thiết kế: người dùng đã kiểm tra và nhận bộ dữ liệu |
| Có thể lặp lại | Đạt: có đợt dữ liệu mới cần kiểm duyệt |
| Có thể quan sát | Đạt về thiết kế: approval, manifest và receipt xác định completion; cần triển khai tracking để quan sát thực tế |
| Có ý nghĩa | Đạt có điều kiện: yêu cầu chất lượng và ảnh mới; chống tải lại/chia nhỏ để tăng số |
| Có thể tác động | Đạt: cải thiện xem ảnh, xử lý yêu cầu sửa, checklist và xuất dữ liệu |

**Kết luận:** Giữ core action; không chọn mở app, hỏi AI hay AI tạo batch. Năm tiêu chí là tự kiểm thiết kế, chưa phải xác thực với người dùng.

## 02 — Nature & cadence

### Action Nature Card

| Thành phần | Câu trả lời |
| --- | --- |
| Actor | Reviewer |
| Intent | Kiểm tra bộ ảnh để bàn giao gán nhãn |
| Trigger | Curator gửi batch mới và giao trách nhiệm kiểm duyệt |
| Effort | Xem ảnh, đối chiếu tiêu chí, duyệt hoặc yêu cầu sửa; thời gian phụ thuộc số ảnh, độ khó và chất lượng |
| Value timing | Sau duyệt và xác nhận nhận gói; trễ khi cần sửa hoặc xuất thất bại |
| State | Quyết định, lý do, phiên bản tiêu chí, manifest và lịch sử bàn giao |
| Dependency | Dữ liệu Curator chuẩn bị, quyền truy cập, tiến độ sửa và kế hoạch gán nhãn |
| Repeat condition | Có đợt mới với ảnh mới cần kiểm duyệt |

**Dạng hành vi chính:** Quy trình làm việc của nhóm (workflow).

> Đối với Reviewer, core action kiểm duyệt và xuất batch đạt yêu cầu xuất hiện theo từng đợt dữ liệu vì nhóm cần chuẩn bị ảnh cho gán nhãn. Do đó, nhịp đo phù hợp là từng đợt dữ liệu ở cấp Reviewer.

**Căn cứ:** Giả thuyết từ luồng công việc, chưa có số liệu về tần suất. Thu thời điểm giao/hoàn tất qua các đợt rồi kiểm tra; không mặc định daily/weekly/monthly.

**Quy ước đợt:** cycle_id là đợt dữ liệu, khác batch_id là bộ ảnh. Mỗi đợt có mục tiêu, thời điểm bắt đầu, hạn xử lý và hạn kiểm tra sau xuất đã thống nhất trước khi bắt đầu. Không đóng sớm hoặc kéo dài hạn sau khi xem kết quả để làm đẹp metric. Nhịp đợt là giả thuyết, cần đối chiếu kế hoạch thực tế.

**Tần suất cao hơn không luôn tốt hơn:** tải lại hoặc chia nhỏ batch có thể tăng số thao tác. Đo ảnh mới đạt yêu cầu, mức hoàn tất theo cơ hội và thời gian xử lý.

## 03 — Metric System

### Activation

| Thành phần | Định nghĩa |
| --- | --- |
| Start event | `batch_assigned` hợp lệ đầu tiên của Reviewer, có batch đủ điều kiện xem |
| First-value event | `batch_export_completed` hợp lệ đầu tiên |
| Activation rule | Tạm dùng first value: ít nhất một core value event trước hạn đợt đầu được giao |
| Time window | Từ giao batch đầu tiên đến processing_deadline_at đã chốt của đợt đó |
| A1 — First-value activation rate | Reviewer đạt first value trong window / Reviewer được giao hợp lệ ở đợt đầu và đã hết window |
| Căn cứ và kiểm chứng | Là proxy activation tạm thời, chưa khẳng định có đủ lặp lại để dự báo retention. Sau nhiều đợt, đối chiếu nhóm một lần và nhóm hoàn tất ở hai đợt trước khi chốt ngưỡng lặp lại |

Có hoạt động trong window không có nghĩa đã activated. Reviewer bắt đầu xem nhưng chưa nhận gói chưa đạt first value. First value muộn vẫn tạo giá trị và có thể vào cohort retention, nhưng không đạt A1 đúng hạn.

### Engagement — tối đa hai góc đo

| Metric | Góc đo | Công thức và window | Event |
| --- | --- | --- | --- |
| E1 — Tỷ lệ đợt hoàn tất theo cơ hội | Frequency | Số cặp (reviewer_id, cycle_id) có ≥1 core value event đúng hạn / số cặp được giao hợp lệ đã hết hạn; tổng hợp các đợt đã đủ quan sát, mỗi cặp trọng số một | `batch_assigned`, `batch_export_completed`, `cycle_closed` |
| E2 — Ảnh mới đủ điều kiện xuất / Reviewer / đợt | Depth | Tổng ảnh mới trong các core value event của Reviewer trong đợt; báo cả giá trị 0 ở Reviewer được giao nhưng không hoàn tất | `batch_assigned`, `batch_export_completed`, `cycle_closed` |

### North Star Metric

| Thành phần | Định nghĩa |
| --- | --- |
| Unit of value | Một ảnh mới duy nhất đã được duyệt và bàn giao hợp lệ |
| Quality threshold | Thỏa tiêu chí mục 01, có quality_policy_version và quality_pass=true |
| Frequency/window | Từng đợt dữ liệu; tổng hợp ở cấp dự án, phân rã theo Reviewer |
| NSM | Số ảnh mới duy nhất đạt yêu cầu được bàn giao trong từng đợt |
| Công thức | Count distinct image_hash có lần bàn giao hợp lệ đầu tiên trong project_id nằm trong window đợt |
| Dữ liệu | `batch_export_completed`, immutable manifest, receipt, quality checks và `cycle_closed` |

So NSM phải kiểm soát số ảnh đầu vào, số batch được giao và độ khó đợt. Giá trị này là proxy dữ liệu sẵn sàng gán nhãn; không đồng nghĩa hiệu quả mô hình. E2 phân rã NSM theo người, không phải một loại giá trị khác.

### Leading indicators — hai chỉ số

| Metric | Công thức/window | Giả thuyết dự báo | Event |
| --- | --- | --- | --- |
| L1 — Tỷ lệ batch đã bắt đầu kiểm duyệt | Số batch được giao có lượt xem đầu tiên đúng hạn / tổng batch được giao hợp lệ trong đợt; batch lineage chỉ đếm một lần, không tính version sửa là việc mới | Ít việc bị bỏ chờ có thể giúp nhận giá trị và quay lại ở đợt sau | `batch_assigned`, `batch_view_started`, `cycle_closed` |
| L2 — Tỷ lệ duyệt ngay lần quyết định đầu | Batch có quyết định đầu tiên approve / batch đã có quyết định đầu tiên trong đợt | Ít phải sửa có thể giảm ma sát và hỗ trợ dùng lại; cũng có thể do duyệt qua loa, nên phải xem C1 | `batch_review_decided`, `cycle_closed` |

L2 xét quyết định đầu trên batch_id qua mọi version, không reset sau mỗi lần sửa. Hai liên hệ là giả thuyết, cần kiểm tra với retention; nguồn cung và chất lượng đầu vào có thể gây nhiễu.

### Counter-metrics

| Metric | Công thức/window | Cách đọc |
| --- | --- | --- |
| C1 — Tỷ lệ ảnh lỗi sau bàn giao | Ảnh unique trong mẫu kiểm tra sau xuất bị đánh fail / ảnh unique đã kiểm tra; tổng hợp theo đợt xuất khi hết hạn audit | Lỗi ảnh/PII/gần trùng/không phù hợp không được tăng khi NSM tăng. Báo kèm coverage = ảnh đã audit / ảnh bàn giao và cách chọn mẫu; không coi ảnh chưa kiểm tra là pass |
| C2 — Thời gian hoàn tất | Median(receipt_at − assigned_at) của batch_id hoàn tất lần đầu trong đợt, tính từ lần giao đầu tiên, không reset khi sửa version | Theo dõi thời gian có tăng khi NSM tăng không; luôn đọc cùng E1 và số việc chưa hoàn tất để tránh chỉ nhìn các việc nhanh |

**Event C1:** `batch_inspection_completed` + `batch_export_completed` + `cycle_closed`.
**Event C2:** `batch_assigned` + `batch_export_completed` + `cycle_closed`.

Chưa có baseline để đặt target phần trăm hoặc thời gian. Thu baseline ở các đợt tương đương trước; thiếu audit thì C1 là “chưa đủ dữ liệu”, không phải 0% lỗi. Audit là nguồn phản hồi downstream, không đổi persona chính.

### Quy ước tính chung

- **Identity:** reviewer_id ổn định, project_id, cycle_id, batch_id và batch_version; không dùng tên/email làm khóa.
- **Ownership:** một Reviewer chịu trách nhiệm đo trên mỗi batch. Đồng nghiệp có thể cộng tác nhưng không cùng nhận một lần hoàn tất. Chuyển giao trước bắt đầu tạo assignment mới, ghi lý do; báo riêng các việc chuyển/hủy, không xóa lịch sử. Start của một lineage không reset bởi sửa phiên bản.
- **Window:** timestamp UTC; trình bày Asia/Ho_Chi_Minh. Đợt là [cycle_started_at, processing_deadline_at); đúng tại mốc cuối thuộc ngoài window. Gói hoàn tất muộn ghi nhận là late, không tính đúng hạn, vẫn có thể tạo giá trị thật.
- **Ảnh mới:** sổ bàn giao theo (project_id, image_hash); cấp credit nguyên tử cho lần bàn giao hợp lệ đầu tiên. Không tăng NSM khi đổi version, đổi batch hoặc đổi Reviewer chứa lại cùng ảnh.
- **Loại trừ:** test/demo/bot và dữ liệu demo. Nhân viên thật dùng trong vai trò Reviewer là người dùng hợp lệ, không loại chỉ vì nội bộ.
- **Chống trùng:** event_id và business transition key; quyết định khác nhau là transition khác nhau. Bản xuất lại dùng cùng logical receipt key (project_id, batch_id, batch_version), không dùng export_id mới để tạo giá trị mới.
- **Thiếu dữ liệu:** thiếu quality/receipt/manifest thì chưa hoàn tất; mẫu số 0 hoặc chưa hết hạn báo N/A. Lưu expected deadline trên assignment để biết đợt đã đủ quan sát ngay cả khi event đóng đợt bị thiếu; thiếu lifecycle event phải báo tracking incomplete.
- **Audit:** chọn mẫu trước khi biết kết quả, lưu sampling_policy_version; mỗi ảnh dùng kết quả lần kiểm tra hợp lệ đầu tiên cho C1, lần kiểm tra lại sau sửa không xóa fail ban đầu. Báo cả kích thước mẫu, coverage và giới hạn đại diện.

## 04 — Retention Definition

| Thành phần | Định nghĩa |
| --- | --- |
| Unit | Reviewer trong cùng project_id |
| Cohort entry | `batch_export_completed` hợp lệ đầu tiên; nhóm theo cycle_id first value |
| Return event | `batch_export_completed` hợp lệ với batch mới và ảnh mới ở một đợt khác |
| Window | Đợt khác được giao tiếp theo sau first value, theo thứ tự giao; từ assignment đầu của đợt đó đến processing_deadline_at đã chốt |
| Threshold | Ít nhất một core value event hợp lệ đúng hạn |
| Segment | Reviewer thật đã có first value, được giao việc đủ điều kiện ở đợt tiếp theo và đã hết hạn quan sát; loại test/demo/bot |

**R1 — Tỷ lệ quay lại theo cơ hội được giao việc:**
Reviewer của cohort hoàn tất return event trong window / Reviewer của cohort có cơ hội hợp lệ tiếp theo đã hết window.

**Event:** `batch_export_completed` (entry/return), `batch_assigned` (opportunity), `cycle_closed` (maturity).

Báo kèm số người trong cohort ban đầu, có cơ hội mới, chưa có cơ hội, đang chờ hết hạn và bị chuyển/hủy. Người chưa có việc mới không tính là churn. Conditional retention chỉ trả lời “có quay lại khi được giao việc không”, chưa chứng minh nhu cầu hoặc mức tự nguyện; số cơ hội có thể phụ thuộc người điều phối. Không tạo thêm việc giả để tăng retention.

### Đối chiếu ba mốc

| Mốc | Cách đối chiếu |
| --- | --- |
| Natural cycle | Kiểm tra lịch đợt và khoảng cách giao việc thật; không lấy D7/D30 khi chưa có căn cứ |
| Cohort đúng segment | So Reviewer cùng vai trò, nguồn dữ liệu, khối lượng và độ khó; tách người mới/người quen |
| Benchmark category | Chưa có nguồn với định nghĩa tương thích; dùng baseline nội bộ trước, không tự tạo chuẩn “tốt” |

## 05 — Product Loop

**Loop chính:** Workflow của nhóm.

| Bước | Chu kỳ 1 | Chu kỳ 2 |
| --- | --- | --- |
| Natural trigger | Có đợt ảnh mới cần chuyển sang gán nhãn | Có đợt ảnh khác phục vụ nhu cầu gán nhãn tiếp theo |
| Core action | Reviewer kiểm duyệt, yêu cầu sửa nếu cần, rồi xuất batch đạt yêu cầu | Reviewer làm hành vi đó với batch và ảnh mới |
| Value | Nhận bộ ảnh đã kiểm tra để bàn giao | Nhận bộ ảnh mới đủ điều kiện |
| Saved state/investment | Lưu tiêu chí, lý do duyệt/sửa, manifest, receipt và phản hồi audit | Tái sử dụng tiêu chí/lý do và học từ lỗi trước để xử lý nhất quán hơn |

**Reason to return:** Có công việc thật mới; lịch sử chỉ giảm công sức thực hiện, không tạo nhu cầu. Nếu không có dữ liệu mới thì không ép quay lại bằng notification.

**Metric hypothesis:**
> Nếu lịch sử quyết định và phản hồi chất lượng giúp workflow tốt hơn, R1 sẽ tăng và C2 sẽ giảm qua ba đợt có cơ hội tiếp theo đã kết thúc, vì Reviewer ít phải xử lý lại vấn đề cũ; C1 không được tăng và E1 không được giảm.

Ba đợt là khung thử đề xuất, không phải chu kỳ nhu cầu đã xác thực. So với baseline từ các đợt trước tương đương; nếu chưa có baseline, thu baseline trước, chưa kết luận tăng/giảm.

**Cách thử:** Cho một nhóm dùng lịch sử/checklist rõ hơn; giữ nguồn cung, số ảnh, độ khó và cách giao việc tương đương nhóm đối chiếu nếu có thể. So R1, C2, C1, E1; phân tích người mới/người quen và dữ liệu thiếu. Nếu chỉ so trước/sau thì kết quả mang tính gợi ý, chưa chứng minh quan hệ nhân quả. Chưa khẳng định các tính năng này đã triển khai.

## 06 — Tracking nhanh

Sáu core events, mỗi event dùng được để tính metric. Đây là contract đề xuất, không phải báo cáo tracking đang hoạt động.

| Tên event | Điều đã xảy ra | Thời điểm ghi nhận chính xác | Metric sử dụng |
| --- | --- | --- | --- |
| `batch_assigned` | Batch đủ điều kiện đã được giao cho Reviewer | Sau transaction assignment lưu thành công; kèm assignment_id, owner, cycle_id, batch/version, deadline, eligibility và lý do chuyển/hủy nếu có | A1 start/mẫu số, E1/E2 population, L1 mẫu số, C2 start, R1 opportunity |
| `batch_view_started` | Reviewer bắt đầu xem batch đã được giao | Khi dữ liệu và gallery đúng batch/version đã tải, hiển thị thành công trong tab đang hiện; ghi lần đầu của assignment, không khi click hoặc chỉ tải khung rỗng | L1 tử số |
| `batch_review_decided` | Quyết định approve/revise/reject đã lưu | Sau transaction quyết định thành công, kèm decision_id, sequence, decision, version và quality_policy_version; không bắn khi mới bấm | L2 |
| `batch_export_completed` | Bàn giao hợp lệ thỏa completion rule mục 01 | Sau kiểm tra approval/quality/manifest và receipt được lưu; kèm manifest_ref, receipt_at, new_image_count, new_image_hashes và first-value credit; không khi ZIP vừa tạo | A1, E1/E2, NSM, C1 population, C2 end, R1 entry/return |
| `batch_inspection_completed` | Kết quả kiểm tra mẫu ảnh sau bàn giao đã lưu | Sau audit commit; kèm export/receipt_ref, image_hash, result pass/fail, defect_type, sampling_policy_version, inspected_at và audit_deadline_at | C1 tử số/mẫu số/coverage |
| `cycle_closed` | Đợt đến hạn xử lý hoặc hoàn tất thời hạn audit | Khi trạng thái đóng đã lưu; stage=processing/audit, kỳ hạn đã chốt và mốc đóng thực tế. Hai stage có transition key khác nhau | A1/E1/E2/NSM/L1/L2/C2/R1 window; C1 audit maturity |

**Properties chung:** event_id, schema_version, occurred_at_utc, project_id, cycle_id, actor_id/actor_role, environment và is_test; event batch có batch_id/batch_version/assignment_id nếu phù hợp. Chỉ lưu ID/hash/tham chiếu, không đưa ảnh, thông tin cá nhân hay nội dung ghi chú vào telemetry.

Manifest và quality record là nguồn dữ liệu bất biến được event tham chiếu, không cần bắn thêm click events. new_image_count phải khớp danh sách hash được cấp credit; số lượng lớn có thể dùng manifest_ref thay vì nhét toàn bộ hash vào event.

### Tiêu chí nghiệm thu

1. **Hoàn tất thật:** Nếu approval sai version, còn lỗi chặn, thiếu receipt hoặc checksum sai thì không có `batch_export_completed`. Tạo ZIP/HTTP 200/click tải không được tính first value.
2. **Retry không tăng số:** Với cùng logical receipt (project_id, batch_id, batch_version), retry, reload và export_id mới không tạo thêm value event. Event transport gửi lại cùng event_id được deduplicate.
3. **Chia batch không tăng NSM:** Xuất 20 hash mới rồi xuất lại chúng trong hai batch khác/version khác/Reviewer khác thì NSM vẫn chỉ 20. Test fixture này là ví dụ kiểm thử, không phải kết quả đo.
4. **Quyết định đầu không bị reset:** revise ở version 1 rồi approve ở version 2 không tính first-pass approval; hai quyết định vẫn được lưu bằng hai decision_id.
5. **Quay lại đúng nhu cầu:** Cùng đợt hoặc tải lại không tính return. Có ảnh mới ở đợt kế tiếp đúng window mới được tính. Không có assignment mới thì không vào mẫu số R1; chưa hết hạn thì pending.
6. **Thiếu audit không phải tốt:** Không có audit thì C1=N/A; không chuyển thành 0% lỗi. Fail rồi sửa/pass lại không xóa lỗi của audit đầu.
7. **Lifecycle đúng hạn:** Hạn đã khóa trước khi giao; đóng sớm hoặc thiếu event lifecycle được gắn tracking incomplete, không tự loại người chưa hoàn tất khỏi mẫu số.

### Tự soi lỗi và năm gate

- [x] Gate 1 — Có actor/object/completion rule, tự kiểm năm tiêu chí.
- [x] Gate 2 — Có Nature Card và kết luận cadence từ nhu cầu công việc.
- [x] Gate 3 — Có activation, hai góc engagement, NSM value + quality + cadence, leading/counter; retention đủ sáu thành phần.
- [x] Gate 4 — Hai chu kỳ workflow và hypothesis nối R1/C2/C1/E1.
- [x] Gate 5 — Sáu events map metric, có tiêu chí hoàn tất và chống trùng.
- [x] Mọi metric có event/thuộc tính; mọi event map về metric.
- [x] README liên kết Metrics Pack trong repo công khai.
- [x] AI Support Log khai báo hỗ trợ thực tế và giới hạn.
- [x] Có đề xuất áp dụng cho dự án thật trong README, ghi rõ AI hỗ trợ soạn.

Checklist là tự kiểm nội dung bài, không phải coach chấm pass, test tự động đã chạy hoặc tracking đã triển khai.

### Revision — lý do thay đổi

| Ngày | Thay đổi | Lý do |
| --- | --- | --- |
| 06/10/2026 | Tạo khung 00–06 rồi chốt Reviewer, kiểm duyệt và xuất batch | Giữ một persona/use case gần giá trị |
| 06/10/2026 | Người nộp đẩy bản điền 01–06 có hỗ trợ OpenCode | Hoàn thiện bản nháp theo cadence đợt |
| 06/10/2026 | Thống nhất start thành batch_assigned; giữ first-value activation là proxy tạm | Xóa tên event không có trong tracking; chưa có bằng chứng cho ngưỡng lặp |
| 06/10/2026 | Đổi NSM/depth sang ảnh mới unique, gắn receipt và policy chất lượng | Chống tăng số bằng tải lại, đổi version hoặc chia batch; tránh coi tạo ZIP là nhận giá trị |
| 06/10/2026 | Định nghĩa C2, audit C1 và vòng đời đợt; sửa loop và đủ sáu events | Hypothesis dùng metric tính được; mẫu số chất lượng chỉ gồm ảnh thực sự kiểm tra |
