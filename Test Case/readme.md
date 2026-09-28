# CAB System — Test Case index

Các test trong thư mục này lấy `srs.md` đã chốt làm nguồn logic. `srs.md` trong repository và `microservice.md` không phải nguồn yêu cầu của bộ test này.

## Nguyên tắc

- Test theo hành vi đã nêu trong FR/BRULE; không suy diễn định dạng tài khoản, công thức cước, phí hủy, thang điểm, giới hạn retry điện tử hoặc quyền chi tiết đang là OI.
- Mỗi test gồm tiền điều kiện, thao tác và kết quả quan sát được. Mã HTTP chỉ được kiểm tra khi hợp đồng API quy định.
- Bộ test tích hợp trọng tâm là `driver-requests.md`, `rides.md`, `payments.md`, `notifications.md`, `operations.md` và `reporting.md`; các tài liệu còn lại kiểm tra ranh giới của từng chức năng.

## Chuỗi nghiệp vụ liên thông

1. Customer tạo Booking → CAB System mở PRIMARY Dispatch Session và gửi thông báo.
2. Session chọn Driver theo 3 round × tối đa 20 Driver × 20 giây, sau đó xác nhận hoặc no-driver/retry.
3. Driver xác nhận thực hiện Trip, cập nhật mốc hành trình; CAB System tính cước và kích hoạt thanh toán sau hoàn thành.
4. Customer thanh toán, xem lịch sử/đánh giá; Operations Staff chỉ giám sát, hỗ trợ và tra cứu theo quyền.
