# CAB System Final Test Cases - pricing

**Basis:** generated directly from `../api/pricing.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-PRICE-001 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-002 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-003 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-004 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-005 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Not found | Use a syntactically valid identifier that does not resolve to an accessible resource. | HTTP 404 with ErrorResponse. Không tìm thấy tài nguyên | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-006 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Business/state conflict | Create the conflict described by the endpoint contract (for example invalid lifecycle state, expired request, or idempotency conflict). | HTTP 409 with ErrorResponse; state is not silently advanced. Xung đột trạng thái nghiệp vụ hoặc idempotency | UC: UC09; FR: FR25; BR: BR08 |
| TC-PRICE-007 | calculateFinalFare | POST /pricing/rides/{ride_id}/final-fare | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC09; FR: FR25; BR: BR08 |
