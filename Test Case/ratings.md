## **TC06 - ratings**

| **Test Case ID** | **Test Scenario** | **Test Case** | **Preconditions** | **Test Steps** | **Test Data** | **Expected Result** | **Priority** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-RAT-001 | Rating | Customer rates a completed Ride with 5 stars | Ride `COMPLETED`; Customer is the rider; no existing rating | 1. POST `/rides/3001/rating`.<br>2. Send score/comment. | `{ "score":5, "comment":"The driver was friendly and punctual" }` | HTTP `201`.<br>Rating is saved.<br>Rating/Driver information is returned. | High |
| TC-RAT-002 | Rating | Rate with 1 star | Ride is completed | 1. POST rating. | `{ "score":1 }` | HTTP `201`. | High |
| TC-RAT-003 | Rating | Rate with score = 3 | Ride is completed | 1. POST rating. | `{ "score":3 }` | HTTP `201`. | Medium |
| TC-RAT-004 | Validation | Score = 0 | Ride is completed | 1. POST rating. | `{ "score":0 }` | HTTP `400`.<br>Error `INVALID_SCORE`. | High |
| TC-RAT-005 | Validation | Score = 6 | Ride is completed | 1. POST rating. | `{ "score":6 }` | HTTP `400`. | High |
| TC-RAT-006 | Validation | Omit score | Ride is completed | 1. POST rating. | `{ "comment":"Good" }` | HTTP `400`. | High |
| TC-RAT-007 | Authorization | Another User rates the Ride | Ride belongs to Customer 101; Customer 102 is logged in | 1. POST rating. | `score=5` | HTTP `403 Forbidden`. | High |
| TC-RAT-008 | Business Rule | Rate a Ride that is not completed | Ride = `IN_PROGRESS` | 1. POST rating. | `score=5` | HTTP `409 Conflict`.<br>Rating is not saved. | High |
| TC-RAT-009 | Business Rule | Rate a cancelled Ride | Ride = `CANCELLED` | 1. POST rating. | `score=5` | HTTP `409`. | High |
| TC-RAT-010 | Business Rule | Rate the same Ride a second time | A rating already exists | 1. POST rating a second time. | `score=4` | HTTP `409 Conflict`.<br>A second Rating is not created. | High |
| TC-RAT-011 | Feedback | Send an optional comment | Ride is completed | 1. POST rating with a comment.<br>2. Check the response. | `score=5`, valid comment | Rating is saved with the comment. | Medium |
