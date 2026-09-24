# CAB System Final Test Cases - drivers

**Basis:** generated directly from `../api/drivers.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-DRV-001 | updateDriverProfile | PUT /drivers/me | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC02; FR: FR09; BR: BR03 |
| TC-DRV-002 | updateDriverProfile | PUT /drivers/me | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC02; FR: FR09; BR: BR03 |
| TC-DRV-003 | updateDriverProfile | PUT /drivers/me | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC02; FR: FR09; BR: BR03 |
| TC-DRV-004 | updateDriverProfile | PUT /drivers/me | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC02; FR: FR09; BR: BR03 |
| TC-DRV-005 | updateDriverProfile | PUT /drivers/me | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC02; FR: FR09; BR: BR03 |
| TC-DRV-006 | updateDriverAvailability | PATCH /drivers/me/availability | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC03; FR: FR10; BR: BR03, BR04 |
| TC-DRV-007 | updateDriverAvailability | PATCH /drivers/me/availability | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC03; FR: FR10; BR: BR03, BR04 |
| TC-DRV-008 | updateDriverAvailability | PATCH /drivers/me/availability | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC03; FR: FR10; BR: BR03, BR04 |
| TC-DRV-009 | updateDriverAvailability | PATCH /drivers/me/availability | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC03; FR: FR10; BR: BR03, BR04 |
| TC-DRV-010 | updateDriverAvailability | PATCH /drivers/me/availability | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC03; FR: FR10; BR: BR03, BR04 |
| TC-DRV-011 | recordDriverLocation | POST /drivers/me/locations | Success | Payload conforms to the request schema referenced by the operation. | HTTP 201; response matches documented schema; no unconfirmed policy is inferred. | UC: UC08; FR: FR19; BR: BR07 |
| TC-DRV-012 | recordDriverLocation | POST /drivers/me/locations | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC08; FR: FR19; BR: BR07 |
| TC-DRV-013 | recordDriverLocation | POST /drivers/me/locations | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC08; FR: FR19; BR: BR07 |
| TC-DRV-014 | recordDriverLocation | POST /drivers/me/locations | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC08; FR: FR19; BR: BR07 |
| TC-DRV-015 | recordDriverLocation | POST /drivers/me/locations | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC08; FR: FR19; BR: BR07 |

## Network-loss note

[OPEN ISSUE] Network-loss behavior is not encoded as a fixed API retry/offline policy. NFR14/AC17 remain MBB design pending customer confirmation; therefore this file deliberately contains no testcase asserting a retry count, reconnect interval, RTO, or offline state-transition rule.
