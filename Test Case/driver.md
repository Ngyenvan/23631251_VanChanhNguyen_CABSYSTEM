# Test Case - Driver

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-DRV-01 | Driver cập nhật hồ sơ và trạng thái sẵn sàng. | Fact Driver được lưu. | FR-06, FR-08 |
| TC-DRV-02 | Driver sẵn sàng, không Trip, vị trí không quá 60 giây. | Có thể là candidate Dispatch. | FR-09, FR-10, BRULE-12 |
| TC-DRV-03 | Driver có Trip hoặc vị trí cũ quá 60 giây. | Không được Dispatch mời. | FR-09, FR-10, BRULE-12 |
