# Test Case — Automatic dispatch

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-DISP-01 | Mở PRIMARY Session cho Booking có hơn 20 Driver hợp lệ, chưa được mời. | Round 1 mời đồng thời không quá 20 Driver; session ghi nhận ảnh chụp vị trí của round. | FR-13–FR-15, BRULE-10, BRULE-12 |
| TC-DISP-02 | Trộn Driver sẵn sàng/có Trip/vị trí cũ quá 60 giây. | Chỉ Driver sẵn sàng, không Trip và vị trí mới được mời. | FR-13, BRULE-12 |
| TC-DISP-03 | Một Driver từ chối offer còn hiệu lực; còn Driver chưa mời và session chưa đủ 3 round. | Từ chối được lưu, CAB System tự tiếp tục điều phối trên Booking hiện có. | FR-17, BRULE-04, BRULE-10 |
| TC-DISP-04 | Không Driver nào phản hồi offer trong 20 giây. | Offer hết hiệu lực; CAB System mở round kế tiếp nếu còn round. | FR-18, BRULE-10 |
| TC-DISP-05 | Driver trả lời offer sau 20 giây. | `409`; không được chọn Driver, offer vẫn hết hiệu lực. | FR-16, FR-18, BRULE-10 |
| TC-DISP-06 | Hai hoặc nhiều accept đến trong một cửa sổ 1 giây đầu tiên. | Chọn Driver gần pickup nhất theo ảnh chụp vị trí round; nếu bằng nhau chọn phản hồi CAB System nhận trước; offer khác hết hiệu lực. | FR-14, FR-16, BRULE-11 |
| TC-DISP-07 | Accept hợp lệ đến sau cửa sổ 1 giây đã đóng bởi accept đầu tiên. | Không thay đổi Driver đã được CAB System chọn. | FR-16, BRULE-11 |
| TC-DISP-08 | Hết một round mà không có Driver được xác nhận; chuyển round mới. | Không mời lại bất cứ Driver nào đã mời trong cùng session. | FR-14, FR-17, FR-18, BRULE-10 |
| TC-DISP-09 | Ba round đều kết thúc không có Driver xác nhận. | Session kết thúc no-driver; Customer được thông báo và Booking khóa retry 10 giây. | FR-19, FR-36, BRULE-10, BRULE-13 |
| TC-DISP-10 | Ops Staff truy cập session điều phối. | Chỉ xem/giám sát theo quyền; không có endpoint hay khả năng gán Driver bằng tay. | FR-47, FR-49, BRULE-14 |
| TC-DISP-11 | RECOVERY Session khởi tạo sau báo không thể phục vụ. | Có tối đa 3 round với quy tắc y hệt PRIMARY; Driver báo lỗi bị loại khỏi session. | FR-59, BRULE-10, BRULE-14 |
