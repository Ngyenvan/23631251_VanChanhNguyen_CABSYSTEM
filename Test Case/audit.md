# Test Case - Audit

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-AUD-01 | Mở/kết thúc session, offer/response, confirm, retry, recovery, cancellation hoặc payment result. | Audit Entry chứa fact tương ứng và correlation để tra cứu. | FR-58 |
| TC-AUD-02 | User không đủ quyền tra audit. | Không lộ Audit Entry. | FR-57, FR-58 |
