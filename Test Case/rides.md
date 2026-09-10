## **TC07 - rides**

| **Test Case ID** | **Test Scenario** | **Test Case** | **Preconditions** | **Test Steps** | **Test Data** | **Expected Result** | **Priority** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-RIDE-001 | Fare estimation | Estimate fare with valid pickup/destination | Customer is logged in; Map service is operational | 1. POST `/rides/estimate`.<br>2. Send valid locations.<br>3. Check the response. | Pickup: `10.7743,106.704`; Destination: `10.8188,106.6519`; `CAR` | HTTP `200`.<br>Returns `distance_km`, `estimated_duration_minutes`, `estimated_fare`, and `currency=VND`. | High |
| TC-RIDE-002 | Fare estimation | Estimate fare for BIKE | Logged in as a Customer | 1. POST estimate. | `vehicle_type=BIKE` | HTTP `200`. | High |
| TC-RIDE-003 | Validation | Missing pickup | API is operational | 1. POST estimate.<br>2. Remove pickup. | Valid destination; `vehicle_type=CAR` | HTTP `400`. | High |
| TC-RIDE-004 | Validation | Missing destination | API is operational | 1. POST estimate. | Valid pickup; destination omitted | HTTP `400`. | High |
| TC-RIDE-005 | Validation | Pickup and destination are identical | Logged in as a Customer | 1. POST estimate. | Same coordinates for both points | HTTP `400`.<br>No valid estimate is created. | High |
| TC-RIDE-006 | Validation | Invalid vehicle type | Logged in as a Customer | 1. POST estimate. | `vehicle_type=TRUCK` | HTTP `400`. | High |
| TC-RIDE-007 | Location | Coordinates are out of range | Logged in as a Customer | 1. POST estimate. | Latitude `100` | HTTP `400`. | High |
| TC-RIDE-008 | Create Ride | Create a valid Ride with CASH | Customer is logged in; no active Ride exists | 1. POST `/rides`.<br>2. Send the payload and Idempotency-Key.<br>3. Check the response. | `request_id=req-001`; `CAR`; `payment_method=CASH` | HTTP `201`.<br>Ride is created in `SEARCHING_DRIVER`. | High |
| TC-RIDE-009 | Create Ride | Create a Ride with ONLINE payment | Customer has no active Ride | 1. POST `/rides`. | `payment_method=ONLINE` | HTTP `201`.<br>Ride = `SEARCHING_DRIVER`. | High |
| TC-RIDE-010 | Create Ride | Prefer a highly rated Driver | Drivers have different ratings | 1. POST `/rides`.<br>2. Set the preference flag. | `prefer_high_rating_driver=true` | Ride is created successfully; the system uses rating preference when finding a Driver. | Medium |
| TC-RIDE-011 | Validation | Missing request_id | Logged in as a Customer | 1. POST create Ride. | No `request_id` | HTTP `400`. | High |
| TC-RIDE-012 | Validation | Missing payment_method | Logged in as a Customer | 1. POST create Ride. | No `payment_method` | HTTP `400`. | High |
| TC-RIDE-013 | Validation | Invalid payment method | Logged in as a Customer | 1. POST create Ride. | `payment_method=TRANSFER` | HTTP `400`. | High |
| TC-RIDE-014 | Duplicate | Send the same Idempotency-Key multiple times | First request has already created a Ride | 1. Send the request once.<br>2. Send it again with the same key. | Same `Idempotency-Key=req-001` | A second Ride is not created.<br>API returns a duplicate error according to the contract or preserves idempotent behavior. | High |
| TC-RIDE-015 | Business Rule | Customer already has an active Ride | Customer has a Ride `IN_PROGRESS` | 1. POST a new Ride. | Valid new Ride payload | HTTP `409 Conflict`.<br>New Ride is not created. | High |
| TC-RIDE-016 | Business Rule | Pickup and destination are identical | Customer is logged in | 1. POST create Ride. | Same location | HTTP `400`. | High |
| TC-RIDE-017 | History | Customer views Ride history | Ride history exists | 1. GET `/rides`.<br>2. Check the response. | No filter | HTTP `200`.<br>Only the current Customer's Rides are returned. | High |
| TC-RIDE-018 | History | Filter history by status | A `COMPLETED` Ride exists | 1. GET `/rides?status=COMPLETED`.<br>2. Check the response. | `status=COMPLETED` | Only completed Rides are returned. | Medium |
| TC-RIDE-019 | History | Valid page | More than 20 Rides exist | 1. GET `/rides?page=2&limit=20`.<br>2. Check the response. | `page=2`, `limit=20` | HTTP `200`; pagination is correct. | Medium |
| TC-RIDE-020 | History | Page = 0 | API is operational | 1. GET `/rides?page=0`. | `page=0` | Request is rejected by validation. | Medium |
| TC-RIDE-021 | History | Limit > 100 | API is operational | 1. GET the API. | `limit=101` | Request is rejected. | Medium |
| TC-RIDE-022 | History | No token | User is not logged in | 1. GET `/rides`. | No Authorization header | HTTP `401 Unauthorized`. | High |
| TC-RIDE-023 | Tracking | Customer views their own Ride | Ride belongs to the Customer | 1. GET `/rides/3001`. | `ride_id=3001` | HTTP `200`.<br>Returns the latest Ride information. | High |
| TC-RIDE-024 | Tracking | Customer views another Customer's Ride | Ride belongs to Customer 101; Customer 102 is logged in | 1. GET `/rides/3001`. | `ride_id=3001` | HTTP `403 Forbidden`. | High |
| TC-RIDE-025 | Tracking | Ride does not exist | Valid User | 1. GET a non-existent Ride. | `ride_id=999999` | HTTP `404 Not Found`. | High |
| TC-RIDE-026 | Tracking | Assigned Driver views the Ride | Driver is assigned | 1. GET the Ride. | Valid Driver token | HTTP `200`.<br>Assigned Driver can view the related Ride information. | High |
| TC-RIDE-027 | Status | Assigned Driver changes status to `DRIVER_ARRIVING` | Ride `DRIVER_ASSIGNED`; correct Driver | 1. PATCH status.<br>2. Check the response. | `{ "status":"DRIVER_ARRIVING" }` | HTTP `200`.<br>Status is updated. | High |
| TC-RIDE-028 | Status | Driver changes `DRIVER_ARRIVING -> IN_PROGRESS` | Ride is `DRIVER_ARRIVING` | 1. PATCH status. | `{ "status":"IN_PROGRESS" }` | HTTP `200`. | High |
| TC-RIDE-029 | Status | Complete Ride with final fare and distance | Ride `IN_PROGRESS` | 1. PATCH status.<br>2. Send final fare/distance. | `status=COMPLETED`, `final_fare=102000`, `actual_distance_km=7.8` | HTTP `200`.<br>Ride becomes `COMPLETED`; `final_fare` is saved. | High |
| TC-RIDE-030 | Status | Complete without final_fare | Ride `IN_PROGRESS` | 1. PATCH status. | `status=COMPLETED`, `final_fare` omitted | HTTP `400`. | High |
| TC-RIDE-031 | Status | Complete without actual_distance_km | Ride `IN_PROGRESS` | 1. PATCH status. | `status=COMPLETED`, distance omitted | HTTP `400`. | High |
| TC-RIDE-032 | Status | Unassigned Driver attempts to update status | Ride belongs to Driver 201; Driver 202 is logged in | 1. PATCH status. | `status=IN_PROGRESS` | HTTP `403`. | High |
| TC-RIDE-033 | Status | Invalid status transition | Ride `IN_PROGRESS` | 1. PATCH status to `DRIVER_ARRIVING`. | `status=DRIVER_ARRIVING` | HTTP `409 Conflict`.<br>Error `INVALID_STATUS_TRANSITION`. | High |
| TC-RIDE-034 | Status | Attempt to complete before starting | Ride `DRIVER_ARRIVING` | 1. PATCH `COMPLETED`. | `final_fare=102000` | HTTP `409` or `400` according to the implementation; Ride must not become `COMPLETED` outside the business flow. | High |
| TC-RIDE-035 | Ride cancellation | Customer cancels a Ride while searching for a Driver | Ride `SEARCHING_DRIVER` | 1. POST cancel.<br>2. Enter a reason. | `{ "reason":"Change of plans" }` | HTTP `200`.<br>Ride changes to `CANCELLED`. | High |
| TC-RIDE-036 | Ride cancellation | Customer cancels a Ride with an assigned Driver | Ride `DRIVER_ASSIGNED` | 1. POST cancel. | `{ "reason":"No longer needed" }` | HTTP `200`.<br>Ride changes to `CANCELLED` according to policy. | High |
| TC-RIDE-037 | Ride cancellation | Customer cancels a `COMPLETED` Ride | Ride is completed | 1. POST cancel. | `{ "reason":"Test" }` | HTTP `409`.<br>Completed Ride cannot be cancelled. | High |
| TC-RIDE-038 | Ride cancellation | User does not have permission to cancel | Customer 102 calls a Ride belonging to Customer 101 | 1. POST cancel. | Valid reason | HTTP `403`. | High |
| TC-RIDE-039 | Ride cancellation | Missing reason | Ride can be cancelled | 1. POST cancel.<br>2. Remove reason. | `{}` | Request is rejected because reason is required. | High |
| TC-RIDE-040 | Ride cancellation | Driver cancels before picking up the Customer | Ride `DRIVER_ASSIGNED` | 1. Driver calls cancel.<br>2. Check the Ride. | Valid reason | According to the SRS, the system releases the Driver and returns to driver search instead of closing the Ride immediately, if required by policy. | High |
