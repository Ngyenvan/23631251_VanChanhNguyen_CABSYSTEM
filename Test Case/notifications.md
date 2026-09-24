# CAB System Final Test Cases - notifications

**Basis:** generated directly from `../api/notifications.yaml` after Phase 6 validation. Test cases do not introduce business rules not present in the YAML/requirement trace.

## API coverage

| Test Case ID | operationId | Method / Path | Coverage | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- | --- |
| TC-NOTIF-001 | createNotification | POST /notifications | Success | Payload conforms to the request schema referenced by the operation. | HTTP 202; response matches documented schema; no unconfirmed policy is inferred. | UC: UC11; FR: FR31, FR32, FR33; BR: BR10, BR11 |
| TC-NOTIF-002 | createNotification | POST /notifications | Validation / reject | Send a request that violates an explicit OpenAPI required/type/enum/path/query constraint. | HTTP 400 with ErrorResponse; no state-changing success result is produced. Dữ liệu yêu cầu không hợp lệ | UC: UC11; FR: FR31, FR32, FR33; BR: BR10, BR11 |
| TC-NOTIF-003 | createNotification | POST /notifications | Authentication | Omit or invalidate the credential/signature required by the operation. | HTTP 401 with ErrorResponse. Chưa xác thực hoặc thông tin xác thực không hợp lệ | UC: UC11; FR: FR31, FR32, FR33; BR: BR10, BR11 |
| TC-NOTIF-004 | createNotification | POST /notifications | Authorization | Authenticate as a caller outside the documented actor/authorization boundary. | HTTP 403 with ErrorResponse. Không có quyền thực hiện thao tác | UC: UC11; FR: FR31, FR32, FR33; BR: BR10, BR11 |
| TC-NOTIF-005 | createNotification | POST /notifications | Dependency/service failure | Make a required downstream dependency unavailable without changing the business input. | HTTP 503 with ErrorResponse; infrastructure failure is not converted into a business outcome. Dependency/service tạm thời không khả dụng; không được chuyển thành kết quả nghiệp vụ giả | UC: UC11; FR: FR31, FR32, FR33; BR: BR10, BR11 |

## Business branch coverage

| Test Case ID | operationId | Branch | Test basis / steps | Expected result | Requirement trace |
| --- | --- | --- | --- | --- | --- |
| TC-NOTIF-BIZ-FAIL | createNotification | Notification failure isolation | Cause the notification provider/dependency to fail after a valid notification request. | Notification failure is observable/recorded and does not roll back the already valid Trip/Payment business state. No retry count/provider fallback is asserted. | UC11; FR31-FR33; BR11; NFR03 |
