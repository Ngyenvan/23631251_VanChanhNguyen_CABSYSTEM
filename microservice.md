# Thiết kế kiến trúc microservice CAB System

## 1. Mục tiêu và nguyên tắc

Tài liệu này là thiết kế kỹ thuật theo `srs.md` v1.5. SRS quyết định hành vi nghiệp vụ; kiến trúc chỉ xác định ranh giới service, ownership dữ liệu, giao tiếp và event. Không tự chốt các chính sách còn là Open Issue.

- Mỗi service sở hữu dữ liệu của mình; service khác không truy cập database trực tiếp.
- API đồng bộ dùng cho command/query cần phản hồi ngay; Domain Event dùng để phát tán thay đổi đã lưu bền vững.
- `dispatch-service` là owner duy nhất của Session, Round, Offer và quyết định Driver. Operations Staff không gán Driver thủ công.
- Consumer event idempotent theo `event_id`; event chứa `event_type`, `occurred_at`, `aggregate_id`, `correlation_id`, `causation_id` và payload tối thiểu.

## 2. Phân tách Use Case theo miền nghiệp vụ

Use Case được nhóm theo năng lực nghiệp vụ để xác định ownership. Một Use Case có thể đi qua nhiều service, nhưng mỗi trạng thái và dữ liệu nghiệp vụ chỉ có một owner ghi dữ liệu.

| Miền nghiệp vụ | Use Case SRS | Service owner / tham gia | Lý do phân tách |
| --- | --- | --- | --- |
| Tài khoản Customer | UC-01, UC-02, UC-15 | `auth-service`, `customer-service` | Account/credential khác với Customer Profile. |
| Driver và phương tiện | UC-03, UC-04, UC-16, UC-17, UC-18 | `auth-service`, `driver-service`, `vehicle-service` | Identity, hồ sơ/trạng thái/vị trí và phương tiện có ownership riêng. |
| Booking và điều phối | UC-05, UC-06, UC-07 | `booking-service`, `dispatch-service`, `notification-service` | Booking giữ yêu cầu/retry/hủy; Dispatch giữ Session/Round/Offer/chọn Driver. |
| Thực hiện Trip | UC-07, UC-08 | `trip-service`, `notification-service` | Trip là owner duy nhất của các mốc arrived, picked up, in progress, completed và lịch sử. |
| Cước và thanh toán | UC-09 | `pricing-service`, `payment-service` | Fare chỉ có sau Trip hoàn thành; Payment sở hữu chọn phương thức và kết quả giao dịch. |
| Lịch sử và đánh giá | UC-10, UC-11 | `trip-service`, `rating-service` | Lịch sử lấy từ Trip owner; Rating có vòng đời và quyền riêng. |
| Vận hành và giao dịch | UC-12, UC-13 | `operations-service`, các service owner, `audit-service` | Operations chỉ gửi command/tra cứu theo quyền, không ghi dữ liệu nguồn hay gán Driver tay. |
| Báo cáo | UC-14 | `reporting-service` | Read model từ event, không thay đổi dữ liệu nguồn. |
| Thông báo và audit | FR-35 đến FR-40, FR-58 | `notification-service`, `audit-service` | Là consumer xuyên miền, tách khỏi luồng ghi trạng thái lõi. |

## 3. Bounded Context và service

| Context | Service | Dữ liệu owner | Trách nhiệm |
| --- | --- | --- | --- |
| Identity | `auth-service` | Account, credential metadata, access state | Đăng ký, đăng nhập, xác thực và quyền. |
| Customer | `customer-service` | Customer Profile | Hồ sơ Customer. |
| Driver | `driver-service` | Driver, Availability, Driver Location | Hồ sơ, hoạt động/sẵn sàng và vị trí Driver. |
| Vehicle | `vehicle-service` | Vehicle, Driver-Vehicle Link | Thông tin phương tiện. |
| Booking | `booking-service` | Booking, Retry Lock | Nhận/sửa trước retry/hủy Booking. |
| Dispatch | `dispatch-service` | Dispatch Session, Round, Offer, candidate snapshot | Điều phối tự động. |
| Trip | `trip-service` | Trip, Trip Milestone | Thực hiện Trip và lịch sử Trip theo quyền. |
| Pricing | `pricing-service` | Fare | Xác định cước sau Trip hoàn thành. |
| Payment | `payment-service` | Payment, Payment Attempt, provider reference | CASH/ELECTRONIC và callback Provider. |
| Rating | `rating-service` | Rating | Đánh giá Driver sau Trip hoàn thành. |
| Notification | `notification-service` | Notification, Delivery Result | Gửi thông báo qua adapter. |
| Operations | `operations-service` | Operational Case, Support Action | Quản trị, tra cứu, hỗ trợ Trip lỗi trong quyền. |
| Reporting | `reporting-service` | Reporting View | Read model báo cáo. |
| Audit | `audit-service` | Audit Entry | Audit trail bất biến và tra cứu theo quyền. |

## 4. Quyết định ranh giới dữ liệu

Vị trí Driver thuộc `driver-service`: Driver cập nhật vị trí tại đúng context hồ sơ/sẵn sàng, còn Dispatch nhận fact qua API hoặc projection/event `driver.location.updated`. Điều này tránh thêm network hop trong phạm vi hiện tại nhưng vẫn cho phép tách `location-service` khi cần mà không đổi hợp đồng event.

Audit thuộc `audit-service`, không gộp vào Operations. Operations là nơi người dùng vận hành làm việc, còn audit là bằng chứng độc lập về thao tác/quyết định. `audit-service` chỉ nhận event đã phát từ owner và không thay đổi dữ liệu nguồn.

## 5. Quy tắc điều phối và aggregate

| Aggregate | Owner | Invariant |
| --- | --- | --- |
| Booking | booking-service | Retry chỉ sau khóa 10 giây; Customer chỉ sửa dữ liệu trước retry; hủy theo BRULE-15. |
| Dispatch Session | dispatch-service | PRIMARY/RETRY/RECOVERY; tối đa 3 round, không mời lại Driver trong một session. |
| Dispatch Offer | dispatch-service | Mỗi round tối đa 20 Driver; offer tối đa 20 giây; accept trong cửa sổ 1 giây chọn theo khoảng cách snapshot rồi thời điểm nhận. |
| Trip | trip-service | Mốc arrived, picked up, in progress, completed theo quyền; sau picked up không hủy trực tiếp. |
| Fare | pricing-service | Chỉ có sau `trip.completed`; công thức còn OI-01/OI-15. |
| Payment | payment-service | CASH hoàn tất khi Driver xác nhận; ELECTRONIC chỉ một kết quả cuối dưới callback trùng/muộn. |
| Audit Entry | audit-service | Bất biến; không chứa credential hoặc dữ liệu thanh toán nhạy cảm. |

Driver hợp lệ để Dispatch xét mời phải sẵn sàng, không có Trip đang thực hiện và có vị trí mới không quá 60 giây. Sau 3 round thất bại, Booking no-driver và khóa retry 10 giây. Driver đã xác nhận nhưng báo không thể phục vụ trước pickup chỉ kích hoạt tối đa một RECOVERY session và bị loại khỏi session đó.

## 6. Context map và event

`booking-service` phát `booking.submitted` hoặc `booking.retry.requested` cho Dispatch. `dispatch-service` đọc/xác minh fact Driver, phát trạng thái Session/Offer/Driver confirmed cho Booking, Trip, Notification, Operations, Reporting và Audit. `trip-service` phát mốc Trip và `trip.completed`; Pricing tính Fare, Payment xử lý phương thức/kết quả, còn Notification/Reporting/Audit là consumer độc lập.

| Event | Producer | Consumer chính | Ý nghĩa |
| --- | --- | --- | --- |
| `account.registered` | auth | customer, driver, audit | Liên kết profile theo loại account. |
| `driver.availability.updated`, `driver.location.updated` | driver | dispatch | Đồng bộ fact Driver phục vụ điều phối. |
| `booking.submitted`, `booking.retry.requested`, `booking.cancelled` | booking | dispatch, notification, audit | Khởi tạo retry hoặc dừng điều phối. |
| `dispatch.session.started`, `dispatch.offer.issued`, `dispatch.offer.responded`, `dispatch.offer.expired` | dispatch | booking, notification, audit | Theo dõi vòng đời điều phối. |
| `dispatch.driver.confirmed`, `dispatch.session.exhausted` | dispatch | booking, trip, notification, reporting, audit | Kết quả Session. |
| `trip.driver-cannot-serve`, `trip.cancelled-before-pickup`, `trip.arrived-at-pickup`, `trip.customer-picked-up`, `trip.in-progress`, `trip.completed` | trip | dispatch/pricing và consumer phù hợp | Vòng đời Trip. |
| `fare.calculated` | pricing | payment, audit | Cước cuối. |
| `payment.method.selected`, `payment.cash-confirmed`, `payment.electronic-retry-requested`, `payment.electronic-finalized` | payment | notification, reporting, audit | Thanh toán và kết quả cuối. |
| `rating.created` | rating | reporting, audit | Rating hợp lệ. |

## 7. API, security và traceability

API public đi qua Gateway và mọi service owner kiểm tra token/quyền trên tài nguyên. Operations gửi command đến owner thay vì ghi database khác; truy vấn audit đi đến `audit-service`. Payment Provider chỉ giao tiếp với Payment qua adapter/callback đã xác thực. Không lưu trực tiếp số thẻ hoặc tài khoản thanh toán nhạy cảm.

| Nhóm SRS | Service chịu trách nhiệm |
| --- | --- |
| FR-01 đến FR-10, FR-56 | auth, customer, driver, vehicle |
| FR-11 đến FR-19, FR-59, FR-60 | booking, dispatch, trip, notification |
| FR-20 đến FR-27, FR-41 | booking, dispatch, trip, driver |
| FR-28 đến FR-34 | pricing, payment, notification |
| FR-35 đến FR-40 | notification và source owner |
| FR-42 | rating |
| FR-43 đến FR-50, FR-57 | operations và service owner |
| FR-51 đến FR-55 | reporting |
| FR-58 | audit và source owner |

Các nội dung cước, hệ quả tài chính khi hủy, điều kiện/giới hạn payment retry, rating, quyền chi tiết, retention, ETA, thông báo và chỉ số báo cáo vẫn là Open Issue của SRS.
