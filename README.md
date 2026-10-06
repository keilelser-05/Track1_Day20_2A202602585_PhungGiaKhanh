# Track 1 · Day 20 — Metrics Pack

- **Họ tên:** Phùng Gia Khánh
- **Mã học viên:** 2A202602585
- **Dự án:** RAV-26 — chọn lọc dữ liệu cho xe tự lái
- **Trạng thái:** Đã hoàn thiện 00–06; chưa có số liệu đo thực tế.

- **Persona:** Reviewer — người kiểm duyệt batch dữ liệu.
- **Use case:** Kiểm duyệt và xuất batch để chuyển sang gán nhãn.

## Tệp bài làm

[**Mở Metrics Pack**](metrics-pack.md) — mẫu đủ mục 00–06, có bảng tracking và checklist năm gate.

Repo công khai nên người có đường dẫn có thể xem tệp. Không đưa mật khẩu, dữ liệu riêng hoặc ảnh có thông tin cá nhân vào bài.

## Cách dùng

1. Chốt một persona và một use case ở mục 00.
2. Điền lần lượt: core action → nhịp tự nhiên → metric và retention → loop → event.
3. Kiểm tra mọi metric có dữ liệu để tính và mọi event phục vụ một metric.
4. Hoàn thiện phần áp dụng bên dưới và [nhật ký hỗ trợ AI](ai-support-log.md).

## Điều tôi mang về áp dụng cho dự án thật

> Bản nháp do AI gợi ý theo logic bài làm — người nộp cần viết lại bằng ý của mình trước khi nộp:
> 1. Áp dụng quy tắc “mỗi (batch_id, version) chỉ đếm một lần, tải lại không tính” khi báo cáo số batch hoàn tất, để không thổi phồng NSM/E1.
> 2. Ghi thêm data_batch_id, decision và lý do trả về (batch_returned_by_labeling) cho mọi batch xuất, vì hiện chưa đo được chất lượng sau xuất và tỉ lệ duyệt ngay lần đầu.
> 3. Đo tiếp thời gian từ giao đến bắt đầu duyệt (L3) và tỉ lệ bị gán nhãn trả về (C1) qua 2–3 đợt dữ liệu để kiểm chứng cadence theo đợt và ngưỡng chất lượng trước khi chốt số.

## Các file

- [metrics-pack.md](metrics-pack.md): bài làm chính.
- [ai-support-log.md](ai-support-log.md): khai báo hỗ trợ AI thực tế.

Các ô “Chưa chốt” là nội dung cần làm, không phải bằng chứng đã hoàn thành hay coach đã đánh giá đạt.
