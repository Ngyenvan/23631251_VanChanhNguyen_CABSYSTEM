# CAB System Final Test Cases - rides

**Basis:** generated directly from `../api/rides.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-RIDE-001 | createRide | POST /rides | Success | Payload conforms to the request schema referenced by the operation. | HTTP 201; response matches documented schema; no unconfirmed policy is inferred. | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-002 | createRide | POST /rides | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-003 | createRide | POST /rides | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-004 | createRide | POST /rides | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-005 | createRide | POST /rides | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-006 | createRide | POST /rides | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC04; FR: FR04, FR05, FR06, FR07; BR: BR01, BR06, BR11 |
| TC-RIDE-007 | listCustomerRides | GET /rides | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC12; FR: FR34, FR35; BR: BR10, BR12 |
| TC-RIDE-008 | listCustomerRides | GET /rides | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC12; FR: FR34, FR35; BR: BR10, BR12 |
| TC-RIDE-009 | listCustomerRides | GET /rides | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC12; FR: FR34, FR35; BR: BR10, BR12 |
| TC-RIDE-010 | listCustomerRides | GET /rides | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC12; FR: FR34, FR35; BR: BR10, BR12 |
| TC-RIDE-011 | getRide | GET /rides/{ride_id} | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC07; FR: FR18, FR24; BR: BR06, BR07 |
| TC-RIDE-012 | getRide | GET /rides/{ride_id} | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC07; FR: FR18, FR24; BR: BR06, BR07 |
| TC-RIDE-013 | getRide | GET /rides/{ride_id} | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC07; FR: FR18, FR24; BR: BR06, BR07 |
| TC-RIDE-014 | getRide | GET /rides/{ride_id} | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC07; FR: FR18, FR24; BR: BR06, BR07 |
| TC-RIDE-015 | getRide | GET /rides/{ride_id} | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy tài nguyên | UC: UC07; FR: FR18, FR24; BR: BR06, BR07 |
| TC-RIDE-016 | updateRideStatus | PATCH /rides/{ride_id}/status | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |
| TC-RIDE-017 | updateRideStatus | PATCH /rides/{ride_id}/status | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |
| TC-RIDE-018 | updateRideStatus | PATCH /rides/{ride_id}/status | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |
| TC-RIDE-019 | updateRideStatus | PATCH /rides/{ride_id}/status | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |
| TC-RIDE-020 | updateRideStatus | PATCH /rides/{ride_id}/status | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy tài nguyên | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |
| TC-RIDE-021 | updateRideStatus | PATCH /rides/{ride_id}/status | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC07; FR: FR20, FR21, FR22, FR23; BR: BR06, BR11 |

## Business branch coverage

| Test Case ID | operationId | Branch | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- |
| TC-RIDE-BIZ-INVALID-DATA | createRide | Invalid booking data | Omit pickup, destination, or vehicle type required by the CreateRideRequest schema. | HTTP 400; trip is not successfully created; no regex/format policy beyond the schema is assumed. | UC04; FR04-FR06; BR01 |
| TC-RIDE-BIZ-BAD-STATE | updateRideStatus | Invalid state transition | Request a lifecycle transition that conflicts with the documented trip state machine/lifecycle. | HTTP 409; current trip state is retained. Exact strict-transition policy remains based on existing SRS, not relabeled Customer Requirement. | UC07; FR20-FR23; BR06; EX05 |

## Network-loss note

[OPEN ISSUE] Network-loss behavior is not encoded as a fixed API retry/offline policy. NFR14/AC17 remain MBB design pending customer confirmation; therefore this file deliberately contains no testcase asserting a retry count, reconnect interval, RTO, or offline state-transition rule.
