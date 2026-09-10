## **TC08 - users**

| **Test Case ID** | **Test Scenario** | **Test Case** | **Preconditions** | **Test Steps** | **Test Data** | **Expected Result** | **Priority** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-USR-001 | View profile | User views their own profile | Valid access token exists | 1. GET `/users/me`.<br>2. Check the response. | Valid JWT | HTTP `200`.<br>Returns `user_id`, `full_name`, `phone`, `role`, and `status`. | High |
| TC-USR-002 | View profile | No token | User is not logged in | 1. GET `/users/me`. | No Authorization header | HTTP `401`. | High |
| TC-USR-003 | View profile | Invalid token | Fake token is provided | 1. GET the API. | `Authorization: Bearer invalid-token` | HTTP `401`.<br>Error `UNAUTHORIZED` or equivalent. | High |
| TC-USR-004 | Update profile | Update full name | User is logged in | 1. PATCH `/users/me`.<br>2. Send full_name.<br>3. GET the profile again. | `{ "full_name":"Nguyen Van B" }` | HTTP `200`.<br>`full_name` is updated. | High |
| TC-USR-005 | Update profile | Update to an unused phone number | User is logged in; new phone is unused | 1. PATCH the phone.<br>2. Check the response. | `{ "phone":"0987654321" }` | HTTP `200`.<br>Phone number is updated. | High |
| TC-USR-006 | Update profile | Update both full name and phone | User is logged in | 1. PATCH with both fields.<br>2. Check the response. | `{ "full_name":"Nguyen Van C", "phone":"0987654321" }` | HTTP `200`.<br>Both details are updated. | High |
| TC-USR-007 | Validation | Empty PATCH body | User is logged in | 1. PATCH `/users/me` with `{}`.<br>2. Check the response. | `{}` | HTTP `400` because `minProperties=1`. | High |
| TC-USR-008 | Validation | Send an undefined field | User is logged in | 1. PATCH the profile. | `{ "role":"ADMIN" }` | API does not allow users to change their own role; request must be rejected or the field must not be applied according to the contract. | High |
| TC-USR-009 | Business Rule | New phone belongs to another user | User 101 exists; new phone belongs to User 102 | 1. PATCH the phone.<br>2. Check the response. | `{ "phone":"0912345678" }` | HTTP `409 Conflict`.<br>Phone is not changed. | High |
| TC-USR-010 | Authentication | Expired token | Expired JWT exists | 1. PATCH the profile. | Expired JWT | HTTP `401`. | High |
| TC-USR-011 | Data integrity | Response after update contains all User fields | Update succeeds | 1. PATCH the profile.<br>2. Check the response. | Valid update | HTTP `200`.<br>Response contains `user_id`, `full_name`, `phone`, `role`, and `status`. | Medium |
| TC-USR-012 | Security | User cannot view another user's data through `/users/me` | User 101 token exists | 1. GET `/users/me`.<br>2. Check the user_id. | User 101 token | Only User 101's data is returned; another user_id cannot be supplied to access another user through this endpoint. | High |
