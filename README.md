# Track 1 · Day 20 — Metrics Pack

- **Họ tên:** Phùng Gia Khánh
- **Mã học viên:** 2A202602585
- **Dự án:** RAV-26 — chọn lọc dữ liệu cho xe tự lái
- **Persona:** Reviewer — người kiểm duyệt batch dữ liệu
- **Use case:** Kiểm duyệt và xuất batch để chuyển sang gán nhãn
- **Trạng thái:** Đủ mục 00–06 và tự kiểm năm gate về nội dung; chưa có số liệu thực tế, kết quả coach chấm hoặc xác nhận triển khai tracking.

## Tệp bài làm

[**Mở Metrics Pack hoàn chỉnh**](metrics-pack.md)

Tệp nằm trong repo công khai, người có đường dẫn có thể xem mà không cần cấp quyền riêng.

## Các quyết định chính

| Mục | Quyết định |
| --- | --- |
| Core action | Reviewer kiểm duyệt và nhận gói batch đạt yêu cầu để bàn giao |
| Cadence | Theo từng đợt dữ liệu, không ép sử dụng mỗi ngày |
| Activation | First value trong đợt đầu là proxy tạm thời; chưa khẳng định dự báo ở lại |
| North Star | Ảnh mới duy nhất đạt yêu cầu được bàn giao / đợt dữ liệu |
| Retention | Có hoàn tất khi được giao đợt mới tiếp theo không; tách người chưa có cơ hội |
| Loop | Công việc mới + lịch sử giúp giảm công sức; thử bằng R1, C2 và guardrails |
| Tracking | Sáu events, có quy tắc hoàn tất, identity, window và chống đếm trùng |

## Điều mang về áp dụng cho dự án thật

**Đề xuất áp dụng — AI hỗ trợ tổng hợp từ bài làm, chưa phải các thay đổi đã triển khai:**

1. Đo bộ ảnh Reviewer thực sự nhận được, không chỉ batch AI đã tạo. Gắn approval đúng phiên bản, manifest và receipt; không coi click tải là bàn giao thành công.
2. Đếm ảnh mới theo hash ở cấp dự án. Tải lại, chia nhỏ batch hoặc đổi Reviewer không làm chỉ số giá trị tăng.
3. Ghi cycle_id, hạn xử lý và cơ hội được giao việc để đo retention đúng nhịp; không coi người chưa có batch mới là bỏ sản phẩm.
4. Theo dõi thời gian hoàn tất cùng tỷ lệ ảnh lỗi sau bàn giao và độ phủ kiểm tra. Thu baseline qua các đợt tương đương trước khi đặt target.
5. Kiểm tra khả năng triển khai các sự kiện trong mã RAV-26 trước khi báo cáo số đo; đọc tracking thiếu như dữ liệu thiếu, không biến thành 0% lỗi hoặc 100% thành công.

Người nộp cần đọc và bảo vệ các quyết định trên bằng hiểu biết của mình; phần này không mô tả trải nghiệm hoặc kết quả đo chưa có.

## Giới hạn

- Nguồn là brief, nội dung trao đổi và bản repo hiện có; chưa đọc trực tiếp deck Day 20.
- Chưa có benchmark tương thích hoặc dữ liệu người dùng để xác thực cadence và activation.
- Receipt là cơ chế đề xuất để quan sát nhận gói; chưa xác nhận sản phẩm có sẵn.
- NSM phản ánh dữ liệu sẵn sàng bàn giao, chưa chứng minh cải thiện mô hình hoặc nhu cầu thị trường.

[Nhật ký hỗ trợ AI](ai-support-log.md).
