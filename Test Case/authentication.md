# CAB System Final Test Cases - authentication

**Basis:** generated directly from `../api/authentication.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-AUTH-001 | registerUser | POST /auth/register | Success | Payload conforms to the request schema referenced by the operation. | HTTP 201; response matches documented schema; no unconfirmed policy is inferred. | UC: UC01, UC02; FR: FR01, FR08; BR: BR02, BR03 |
| TC-AUTH-002 | registerUser | POST /auth/register | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu đăng ký không hợp lệ | UC: UC01, UC02; FR: FR01, FR08; BR: BR02, BR03 |
| TC-AUTH-003 | registerUser | POST /auth/register | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Định danh tài khoản đã tồn tại | UC: UC01, UC02; FR: FR01, FR08; BR: BR02, BR03 |
| TC-AUTH-004 | loginUser | POST /auth/login | Success | Payload conforms to the request schema referenced by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC01; FR: FR02; BR: BR02, BR16 |
| TC-AUTH-005 | loginUser | POST /auth/login | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Thiếu dữ liệu đăng nhập | UC: UC01; FR: FR02; BR: BR02, BR16 |
| TC-AUTH-006 | loginUser | POST /auth/login | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Thông tin đăng nhập không hợp lệ | UC: UC01; FR: FR02; BR: BR02, BR16 |

## Existing Excel authentication evidence

`CAB_Test_Cases(1).xlsx` contains legacy `TC-AUTH-001`-`TC-AUTH-020`. They are retained as source evidence, not silently rewritten. Cases that assume `username`, format/case/whitespace rules or lockout require customer/security-policy confirmation because the final API uses generic `login_identifier` and does not define those policies.

- Reusable conceptually after review: valid login, missing credentials, invalid credentials, sensitive-field non-disclosure.
- Requires explicit clarification before baselining: username-specific cases, username/password format rules, case-sensitivity policy, whitespace policy, account lockout threshold/behavior.
