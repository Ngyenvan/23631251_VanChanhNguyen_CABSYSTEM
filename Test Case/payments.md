# CAB System Final Test Cases - payments

**Basis:** generated directly from `../api/payments.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-PAY-001 | createPayment | POST /rides/{ride_id}/payments | Success | Payload conforms to the request schema referenced by the operation. | HTTP 201; response matches documented schema; no unconfirmed policy is inferred. | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-002 | createPayment | POST /rides/{ride_id}/payments | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-003 | createPayment | POST /rides/{ride_id}/payments | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-004 | createPayment | POST /rides/{ride_id}/payments | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-005 | createPayment | POST /rides/{ride_id}/payments | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy tài nguyên | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-006 | createPayment | POST /rides/{ride_id}/payments | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-007 | createPayment | POST /rides/{ride_id}/payments | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC10; FR: FR26, FR27, FR28, FR29; BR: BR09, BR10 |
| TC-PAY-008 | paymentCallback | POST /payments/callback | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC10; FR: FR27, FR28, FR29, FR30; BR: BR09, BR10 |
| TC-PAY-009 | paymentCallback | POST /payments/callback | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Callback payload không hợp lệ | UC: UC10; FR: FR27, FR28, FR29, FR30; BR: BR09, BR10 |
| TC-PAY-010 | paymentCallback | POST /payments/callback | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Không xác thực được callback provider | UC: UC10; FR: FR27, FR28, FR29, FR30; BR: BR09, BR10 |
| TC-PAY-011 | paymentCallback | POST /payments/callback | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy payment | UC: UC10; FR: FR27, FR28, FR29, FR30; BR: BR09, BR10 |
| TC-PAY-012 | paymentCallback | POST /payments/callback | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Callback xung đột/không khớp amount | UC: UC10; FR: FR27, FR28, FR29, FR30; BR: BR09, BR10 |
| TC-PAY-013 | listPayments | GET /payments | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC13; FR: FR39; BR: BR13 |
| TC-PAY-014 | listPayments | GET /payments | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC13; FR: FR39; BR: BR13 |
| TC-PAY-015 | listPayments | GET /payments | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC13; FR: FR39; BR: BR13 |
| TC-PAY-016 | listPayments | GET /payments | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC13; FR: FR39; BR: BR13 |

## Business branch coverage

| Test Case ID | operationId | Branch | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- |
| TC-PAY-BIZ-FAILED | paymentCallback | Electronic payment failed | Send an authenticated provider callback with a valid FAILED result for an existing electronic payment. | Payment result is recorded as failed and customer notification is requested; retry timing/count is not asserted. | UC10; FR29, FR30; BR10; OI-P1-001/006 |
