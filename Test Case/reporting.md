# CAB System Final Test Cases - reporting

**Basis:** generated directly from `../api/reporting.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-REP-001 | getActivityReport | GET /reports/activity | Success | Use valid path/query parameters defined by the operation. | HTTP 200; response matches documented schema; no unconfirmed policy is inferred. | UC: UC15; FR: FR43; BR: BR15 |
| TC-REP-002 | getActivityReport | GET /reports/activity | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC15; FR: FR43; BR: BR15 |
| TC-REP-003 | getActivityReport | GET /reports/activity | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC15; FR: FR43; BR: BR15 |
| TC-REP-004 | getActivityReport | GET /reports/activity | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC15; FR: FR43; BR: BR15 |
| TC-REP-005 | getActivityReport | GET /reports/activity | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC15; FR: FR43; BR: BR15 |
