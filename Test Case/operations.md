## **TC04 - operations**

| **Test Case ID** | **Test Scenario** | **Test Case** | **Preconditions** | **Test Steps** | **Test Data** | **Expected Result** | **Priority** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-OPS-001 | Ride monitoring | Operations views all active Rides | Logged in as Operations | 1. GET `/operations/rides/active`.<br>2. Check the response. | No filter | HTTP `200`.<br>Returns `page`, `limit`, `total`, and `items`. | High |
| TC-OPS-002 | Ride monitoring | Filter by `SEARCHING_DRIVER` | A Ride is searching for a Driver | 1. GET the API with a status.<br>2. Check the items. | `status=SEARCHING_DRIVER` | Only Rides with status `SEARCHING_DRIVER` are returned. | High |
| TC-OPS-003 | Ride monitoring | Filter by `IN_PROGRESS` | A Ride is in progress | 1. GET the API.<br>2. Check the items. | `status=IN_PROGRESS` | Only Rides with status `IN_PROGRESS` are returned. | High |
| TC-OPS-004 | Pagination | Valid page | Multiple active Rides exist | 1. GET the API.<br>2. Provide page and limit. | `page=1&limit=50` | HTTP `200`; pagination data is correct. | Medium |
| TC-OPS-005 | Pagination | Page = 0 | API is operational | 1. GET the API. | `page=0` | Request is rejected by validation because `minimum=1`. | Medium |
| TC-OPS-006 | Pagination | Limit > 100 | API is operational | 1. GET the API. | `limit=101` | Request is rejected because `maximum=100`. | Medium |
| TC-OPS-007 | Authorization | Customer views the Operations list | Logged in as a Customer | 1. GET the API.<br>2. Check the response. | Valid Customer token | HTTP `403 Forbidden`. | High |
| TC-OPS-008 | Incident reporting | Create a valid incident | Logged in as Operations; Ride exists | 1. POST `/operations/incidents`.<br>2. Send the payload.<br>3. Check the response. | `ride_id=3001`, `incident_type=TECHNICAL`, `description="Connection lost"`, `priority=HIGH` | HTTP `201`.<br>Incident is created with `status=OPEN`, an `incident_id`, and `created_at`. | High |
| TC-OPS-009 | Incident reporting | Incident with each valid type | Logged in as Operations | 1. Send a request for each type.<br>2. Check the response. | `SAFETY`, `PAYMENT`, `DRIVER`, `CUSTOMER`, `TECHNICAL`, `OTHER` | All valid enum values are accepted. | High |
| TC-OPS-010 | Incident reporting | Omit `incident_type` | Logged in as Operations | 1. POST an incident.<br>2. Remove the field. | Missing `incident_type` | HTTP `400`.<br>Incident is not created. | High |
| TC-OPS-011 | Incident reporting | Invalid incident type | Logged in as Operations | 1. POST an incident. | `incident_type="NETWORK"` | HTTP `400`. | High |
| TC-OPS-012 | Incident reporting | Ride does not exist | Logged in as Operations | 1. POST an incident. | `ride_id=999999` | HTTP `404 Not Found`. | High |
| TC-OPS-013 | Incident reporting | Customer attempts to create an incident | Logged in as a Customer | 1. POST an incident. | Valid incident | HTTP `403 Forbidden`. | High |
