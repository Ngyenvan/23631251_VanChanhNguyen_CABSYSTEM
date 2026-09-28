# Test Case — Booking and Trip

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-RIDE-01 | Customer đã xác thực gửi pickup, destination và vehicle type hợp lệ. | Tạo Booking, mở PRIMARY Dispatch Session tự động và phát sinh thông báo tiếp nhận. | FR-11, FR-12, FR-35 |
| TC-RIDE-02 | Gửi Booking thiếu một trong ba đầu vào bắt buộc. | `400`; không tạo Booking/session. | FR-11 |
| TC-RIDE-03 | Booking ở `No Driver`; Customer thử retry trước thời điểm thất bại 10 giây. | `409`; không mở session mới. | FR-19, BRULE-13 |
| TC-RIDE-04 | Sau 10 giây, Customer sửa một hoặc nhiều thông tin Booking rồi retry. | Mở session `RETRY` mới nhưng vẫn thuộc Booking cũ; session cũ được giữ để audit. | FR-19, FR-20, BRULE-13, FR-58 |
| TC-RIDE-05 | Customer xem Booking khi đang điều phối, chờ retry, no-driver hoặc recovery. | Nhận đúng trạng thái hiện tại; khi có Driver, thấy Driver xác nhận và ETA nếu dữ liệu vị trí hợp lệ. | FR-20–FR-22 |
| TC-RIDE-06 | Driver được xác nhận cập nhật ARRIVED, PICKED_UP, IN_PROGRESS, COMPLETED theo thứ tự. | Customer theo dõi được từng mốc; đến điểm đón và hoàn thành kích hoạt thông báo tương ứng. | FR-23–FR-27, FR-37, FR-38 |
| TC-RIDE-07 | Driver cố cập nhật mốc không đúng thứ tự hoặc Driver khác cập nhật Trip. | `409` hoặc `403`; trạng thái Trip không đổi. | FR-24–FR-27, FR-56 |
| TC-RIDE-08 | Customer hủy khi Booking đang điều phối. | Booking hủy; các offer còn mở bị thu hồi; lưu vết hủy. | FR-60, BRULE-15, FR-58 |
| TC-RIDE-09 | Customer hủy sau Driver xác nhận nhưng trước `PICKED_UP_CUSTOMER`. | Hủy được ghi nhận và Driver được thông báo. | FR-60, BRULE-15 |
| TC-RIDE-10 | Customer hủy sau `PICKED_UP_CUSTOMER`. | `409`; ứng dụng không hủy trực tiếp Trip. | FR-60, BRULE-15 |
| TC-RIDE-11 | Driver báo không thể phục vụ trước pickup sau khi đã xác nhận. | CAB System mở một RECOVERY Session tự động, loại Driver đó; không có thao tác Operations Staff gán thay. | FR-49, FR-59, BRULE-14 |
| TC-RIDE-12 | Đã có một RECOVERY Session; Driver thay thế tiếp tục không thể phục vụ trước pickup. | Không mở RECOVERY Session thứ hai; áp dụng kết quả session hiện có/no-driver theo điều phối. | FR-59, BRULE-14 |
