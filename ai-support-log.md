# AI Support Log — Phùng Gia Khánh · 2A202602585

## Hỗ trợ đã sử dụng

| Ngày | Công cụ | Yêu cầu thực tế | Đầu ra | Người nộp cần kiểm tra |
| --- | --- | --- | --- | --- |
| 06/10/2026 | ChatGPT / Codex | Đọc brief Day 20 và đề xuất phạm vi cho RAV-26 | Gợi ý persona Reviewer, hành vi kiểm duyệt/xuất batch; chưa chốt làm quyết định cá nhân | Chọn lại hoặc xác nhận core action, bằng chứng giá trị và cadence |
| 06/10/2026 | ChatGPT / Codex | Tạo repo mẫu trên tài khoản GitHub | README, Metrics Pack trống theo mục 00–06, nhật ký này | Điền lập luận cá nhân và kiểm tra đủ năm gate |

## Giới hạn của hỗ trợ hiện tại

- AI hỗ trợ cấu trúc và nhắc yêu cầu từ brief do người nộp cung cấp.
- Chưa có số liệu người dùng, retention, benchmark hay kết quả thử loop.
- Chưa đọc deck Day 20; không khẳng định đã đối chiếu trực tiếp các slide.
- Bản hiện tại đã điền đủ 00–06; các định nghĩa là thiết kế đo lường, người nộp cần đọc và bảo vệ.
- Phần áp dụng trong README được AI hỗ trợ tổng hợp thành đề xuất, không phải lời kể trải nghiệm cá nhân hoặc kết quả đã triển khai.

## Ghi bổ sung khi tiếp tục làm bài

| Ngày | Công cụ | Prompt/yêu cầu | Phần được hỗ trợ | Điều đã tự kiểm tra/chỉnh sửa |
| --- | --- | --- | --- | --- |
| 06/10/2026 | ChatGPT / Codex | Hướng dẫn Phase 0 và cập nhật repo theo yêu cầu người nộp | Điền phạm vi Reviewer, kiểm duyệt và xuất batch; đồng bộ README | Người nộp yêu cầu cập nhật sau đề xuất; chưa có số liệu kiểm chứng core action hoặc cadence |
| 06/10/2026 | OpenCode (Muse Spark) | Điền mục 01, 02 theo nội dung người nộp chốt; hoàn thiện 03–06, README, checklist | Core action batch_export_completed, cadence theo đợt, Activation/E1-E2/NSM/L1-L3/C1, retention theo đợt có điều kiện, loop workflow, 5 events + khóa chống trùng | Người nộp đã chốt 01–02; phần 03–06 do AI dự thảo theo đúng logic đó, chưa có số liệu thực — người nộp cần tự kiểm tra, viết lại phần áp dụng bằng ý mình và bảo vệ trước coach |

## Kiểm tra và sửa bản cuối — 06/10/2026

| Công cụ | Yêu cầu | Hỗ trợ thực tế | Giới hạn |
| --- | --- | --- | --- |
| ChatGPT / Codex; kỹ năng thiết kế KPI | Kiểm tra và chỉnh sửa repo hoàn chỉnh | Đọc ba file mới từ GitHub; thống nhất batch_assigned; sửa NSM/depth sang ảnh mới unique; bổ sung completion/receipt/quality policy, định nghĩa thời gian C2 và audit C1; sửa retention/loop; chốt sáu events và tiêu chí nghiệm thu; đồng bộ README | Chưa đọc mã sản phẩm, chưa có số đo hoặc test tracking thực tế. Không tự tạo benchmark, phản hồi người dùng hay lời chấm coach. |

**Tự kiểm đã thực hiện:** Đối chiếu mục 00–06, số event 4–8, event–metric, định nghĩa retention sáu thành phần, link README, nội dung khai báo AI và quy tắc chống đếm trùng. Checklist gate chỉ xác nhận thiết kế bài đủ cấu trúc, không xác nhận coach chấm hoặc production tracking hoạt động.
