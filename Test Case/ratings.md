# CAB System Final Test Cases - ratings

**Basis:** generated directly from `../api/ratings.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-RATE-001 | rateDriver | POST /rides/{ride_id}/rating | Success | Payload conforms to the request schema referenced by the operation. | HTTP 201; response matches documented schema; no unconfirmed policy is inferred. | UC: UC12; FR: FR36; BR: BR12 |
| TC-RATE-002 | rateDriver | POST /rides/{ride_id}/rating | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC12; FR: FR36; BR: BR12 |
| TC-RATE-003 | rateDriver | POST /rides/{ride_id}/rating | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC12; FR: FR36; BR: BR12 |
| TC-RATE-004 | rateDriver | POST /rides/{ride_id}/rating | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC12; FR: FR36; BR: BR12 |
| TC-RATE-005 | rateDriver | POST /rides/{ride_id}/rating | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy tài nguyên | UC: UC12; FR: FR36; BR: BR12 |
| TC-RATE-006 | rateDriver | POST /rides/{ride_id}/rating | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC12; FR: FR36; BR: BR12 |
