# CAB System Final Test Cases - users

**Basis:** generated directly from `../api/users.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-USR-001 | getCurrentUser | GET /users/me | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-002 | getCurrentUser | GET /users/me | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-003 | getCurrentUser | GET /users/me | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-004 | getCurrentUser | GET /users/me | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-005 | updateCurrentUser | PATCH /users/me | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-006 | updateCurrentUser | PATCH /users/me | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-007 | updateCurrentUser | PATCH /users/me | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-008 | updateCurrentUser | PATCH /users/me | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC01; FR: FR03; BR: BR02 |
| TC-USR-009 | updateCurrentUser | PATCH /users/me | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC01; FR: FR03; BR: BR02 |
