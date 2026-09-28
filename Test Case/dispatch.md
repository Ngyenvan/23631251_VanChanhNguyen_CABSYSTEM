# Test Case - Automatic Dispatch

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-DSP-01 | Có hơn 20 Driver hợp lệ. | Round chỉ mời đồng thời tối đa 20 Driver. | FR-13 đến FR-15, BRULE-10 |
| TC-DSP-02 | Offer không phản hồi sau 20 giây. | Offer hết hiệu lực và round tiếp theo mở nếu còn. | FR-18, BRULE-10 |
| TC-DSP-03 | Nhiều accept trong cửa sổ 1 giây. | Chọn khoảng cách snapshot gần nhất; hòa chọn phản hồi nhận trước. | FR-14, FR-16, BRULE-11 |
| TC-DSP-04 | Ba round không xác nhận Driver. | No-driver, thông báo Customer và retry lock 10 giây. | FR-19, BRULE-10, BRULE-13 |
| TC-DSP-05 | Operations xem session. | Chỉ giám sát, không có gán Driver thủ công. | FR-47, FR-49 |
