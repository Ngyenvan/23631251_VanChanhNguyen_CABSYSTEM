# Test Case — Driver profile, availability and location

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-DRV-01 | Driver đã xác thực cập nhật hồ sơ. | Hồ sơ Driver được phản ánh. | FR-06 |
| TC-DRV-02 | Driver cập nhật phương tiện. | Dữ liệu phương tiện được phản ánh. | FR-07 |
| TC-DRV-03 | Driver thay đổi trạng thái hoạt động. | Trạng thái được lưu; chỉ trạng thái phù hợp mới có thể tham gia điều phối. | FR-08, FR-09 |
| TC-DRV-04 | Driver sẵn sàng, không có Trip và gửi vị trí tại thời điểm không quá 60 giây trước khi lập nhóm. | Driver đủ điều kiện để CAB System xem xét mời. | FR-09, FR-10, BRULE-12 |
| TC-DRV-05 | Driver sẵn sàng nhưng đang có Trip, hoặc vị trí cũ quá 60 giây. | Không xuất hiện trong nhóm điều phối, dù trạng thái hoạt động có vẻ sẵn sàng. | FR-09, FR-10, BRULE-12 |
| TC-DRV-06 | Customer/Operations Staff gọi endpoint chỉ dành cho Driver. | `403`; không thay đổi hồ sơ, trạng thái hay vị trí Driver. | FR-06–FR-10, FR-56 |
