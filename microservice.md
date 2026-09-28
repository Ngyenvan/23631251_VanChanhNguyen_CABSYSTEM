# Thiết kế Microservice CAB System

Tài liệu thiết kế kiến trúc dựa trên SRS CAB System v1.5

| Thuộc tính | Giá trị |
| --- | --- |
| Phiên bản | 1.2 |
| Baseline nghiệp vụ | `srs.md` v1.5 |
| Trạng thái | Đề xuất kiến trúc để review |
| Đối tượng đọc | Product Owner, Solution Architect, Development, QA, Operations |

## 0. Mục đích phạm vi và nguyên tắc

Tài liệu này mô tả thiết kế logic microservice của CAB System: ranh giới miền nghiệp vụ, ownership dữ liệu, giao tiếp service và event. Mục tiêu là để các tài liệu API, Test Case và quá trình triển khai cùng tạo thành một hệ thống thống nhất theo `srs.md` v1.5.

SRS là nguồn quyết định nghiệp vụ. Tên service, event, dữ liệu logic, API nội bộ và cách triển khai trong tài liệu này là đề xuất kiến trúc; chúng không thay đổi những nội dung SRS còn Open Issue.

- Mỗi service chỉ ghi dữ liệu của miền mình; không service nào truy cập database của service khác.
- API đồng bộ dùng cho command/query cần kết quả ngay. Event chỉ công bố thay đổi đã được owner ghi bền vững.
- `dispatch-service` là owner duy nhất của Session, Round, Offer và quyết định chọn Driver. Operations Staff không gán Driver thủ công.
- Consumer phải xử lý lặp an toàn theo `event_id`; mỗi event có `event_type`, `occurred_at`, `aggregate_id`, `correlation_id`, `causation_id` và payload tối thiểu.

## 1. Phân tách Use Case theo miền nghiệp vụ

| Bounded Context | Use Case thuộc miền | Năng lực nghiệp vụ | Khái niệm chính |
| --- | --- | --- | --- |
| Identity and Access | UC-01, UC-02; một phần UC-03 | Đăng ký, đăng nhập, xác thực và kiểm soát quyền. | Account, Credential, Access State |
| Customer Profile | UC-15 | Quản lý thông tin Customer. | Customer Profile |
| Driver Management | UC-03, UC-04, UC-17, UC-18 | Hồ sơ Driver, trạng thái hoạt động/sẵn sàng và vị trí. | Driver, Availability, Driver Location |
| Vehicle Management | UC-16 | Thông tin phương tiện gắn với Driver. | Vehicle, Driver Vehicle Link |
| Booking | UC-05; phần hủy của UC-07 | Tiếp nhận, sửa trước retry, hủy Booking và quản lý retry lock. | Booking, Retry Lock |
| Dispatch | UC-05, UC-06; phần phục hồi của UC-08 | Tìm Driver, Session, Round, Offer, phản hồi và chọn Driver. | Dispatch Session, Round, Offer, Candidate Snapshot |
| Trip Management | UC-07, UC-08 | Theo dõi và cập nhật các mốc Trip. | Trip, Trip Milestone |
| Fare | UC-09 | Xác định cước sau Trip hoàn thành. | Fare |
| Payment | UC-09 | Phương thức thanh toán, tiền mặt, điện tử và callback. | Payment, Payment Attempt |
| History and Rating | UC-10, UC-11 | Lịch sử Trip và đánh giá Driver. | Trip History View, Rating |
| Notification | FR-35 đến FR-40 | Gửi thông báo theo sự kiện nghiệp vụ. | Notification, Delivery Result |
| Operations | UC-12, UC-13 | Tra cứu và hỗ trợ trong phạm vi quyền. | Operational Case, Support Action |
| Reporting | UC-14 | Báo cáo từ dữ liệu đã công bố. | Reporting View, Reporting Period |
| Audit | FR-58 | Lưu vết thao tác và quyết định quan trọng. | Audit Entry, Correlation ID |

## 2. Ubiquitous Language

| Thuật ngữ | Ý nghĩa trong CAB System | Context sở hữu |
| --- | --- | --- |
| Account | Danh tính đăng nhập và trạng thái truy cập; không thay thế hồ sơ Customer hoặc Driver. | Identity and Access |
| Driver Location | Vị trí Driver đã được CAB System tiếp nhận; chỉ dữ liệu mới không quá 60 giây được Dispatch dùng. | Driver Management |
| Booking | Yêu cầu đặt xe trước khi Driver được xác nhận. | Booking |
| Dispatch Session | Một lần điều phối tự động cho Booking: PRIMARY, RETRY hoặc RECOVERY. | Dispatch |
| Dispatch Round | Một đợt của Session, mời đồng thời tối đa 20 Driver hợp lệ trong tối đa 20 giây. | Dispatch |
| Dispatch Offer | Đề nghị nhận chuyến dành cho một Driver trong một Round. | Dispatch |
| Candidate Snapshot | Ảnh chụp ứng viên và khoảng cách tới pickup dùng cho một Round. | Dispatch |
| Trip | Chuyến đi sau khi Dispatch xác nhận Driver. | Trip Management |
| Fare | Số tiền phải trả chỉ được xác định sau `trip.completed`. | Fare |
| Payment | Hồ sơ thanh toán của Trip; không chứa dữ liệu thẻ/tài khoản nhạy cảm. | Payment |
| Recovery Session | Tối đa một Session tự động khi Driver đã xác nhận không thể phục vụ trước pickup. | Dispatch |
| Retry Lock | Khoảng khóa 10 giây sau một Session thất bại trước khi Customer gửi thử lại. | Booking |

## 3. Bounded Context và Context Map

### 3.1. Ranh giới context

| Context | Trách nhiệm | Dữ liệu chủ sở hữu | Phụ thuộc chính |
| --- | --- | --- | --- |
| Identity and Access | Xác thực và quyền truy cập. | Account, credential metadata, access state | Không sở hữu profile nghiệp vụ. |
| Customer Profile | Hồ sơ Customer. | Customer Profile | Identity and Access liên kết account. |
| Driver Management | Driver, trạng thái và vị trí. | Driver, Availability, Driver Location | Identity and Access; Vehicle tham chiếu Driver. |
| Vehicle Management | Phương tiện. | Vehicle, Driver Vehicle Link | Driver Management. |
| Booking | Booking, retry, hủy. | Booking, Retry Lock | Dispatch và Trip nhận command/event hợp lệ. |
| Dispatch | Điều phối hoàn toàn tự động. | Session, Round, Offer, Candidate Snapshot | Driver Management, Booking, Trip, Notification. |
| Trip Management | Vòng đời Trip. | Trip, Trip Milestone | Dispatch tạo/liên kết Trip sau xác nhận Driver. |
| Fare and Payment | Cước và giao dịch. | Fare, Payment, Payment Attempt | Trip Management; External Payment Provider chỉ qua Payment. |
| History and Rating | Read model lịch sử và Rating. | Trip History View, Rating | Nhận event từ Trip/Fare/Payment. |
| Notification | Yêu cầu gửi và kết quả gửi. | Notification, Delivery Result | Nhận event từ service owner. |
| Operations | Điều phối thao tác hỗ trợ theo quyền. | Operational Case, Support Action | Gọi API của owner, không ghi trực tiếp dữ liệu nguồn. |
| Reporting | Read model chỉ đọc. | Reporting View, Reporting Period | Nhận event đã chọn. |
| Audit | Audit trail. | Audit Entry | Nhận audit event từ service owner. |

### 3.2. Context Map đề xuất

Gateway là điểm vào cho Customer, Driver và Operations Staff. Gateway truyền ngữ cảnh danh tính; service xử lý vẫn tự kiểm tra quyền với tài nguyên của mình. Payment Provider chỉ giao tiếp với `payment-service` qua adapter và callback đã xác thực.

`booking-service` khởi tạo PRIMARY/RETRY qua event. `dispatch-service` truy vấn hoặc nhận projection Driver fact, điều phối và phát kết quả. `trip-service` chỉ tiếp nhận Driver đã được Dispatch xác nhận. `pricing-service` chỉ phản ứng sau Trip hoàn thành. Notification, Reporting và Audit là consumer độc lập, không làm rollback thay đổi lõi khi gửi hoặc cập nhật read model lỗi.

## 4. Aggregate và invariant nghiệp vụ

| Aggregate | Aggregate Root | Thành phần chính | Invariant áp dụng | Yêu cầu liên quan |
| --- | --- | --- | --- | --- |
| Account | Account | Credential metadata, access state | Xác thực/quyền theo chính sách được duyệt; không sở hữu profile. | FR-01 đến FR-06, FR-56, FR-57 |
| Driver | Driver | Profile, Availability, Driver Location | Chỉ Driver sẵn sàng, không có Trip đang thực hiện và location mới ≤60 giây là eligible. | FR-08 đến FR-10, FR-13 |
| Vehicle | Vehicle | Vehicle data, Driver Vehicle Link | Quan hệ và trường bắt buộc theo OI-18. | FR-07, FR-45 |
| Booking | Booking | Pickup, destination, vehicle type, retry lock | Customer chỉ sửa trước retry; chỉ hủy trước pickup; retry sau 10 giây. | FR-11, FR-12, FR-19, FR-60 |
| Dispatch Session | Dispatch Session | Type, invited Drivers, Round, status | PRIMARY/RETRY/RECOVERY; tối đa 3 Round; không mời lại Driver trong Session. | BRULE-10, BRULE-13, BRULE-14 |
| Dispatch Offer | Dispatch Offer | Driver, snapshot distance, decision, expiration | Tối đa 20 Offer/Round; hiệu lực 20 giây; cùng cửa sổ 1 giây chọn gần hơn rồi nhận trước. | BRULE-10, BRULE-11 |
| Trip | Trip | Booking, Driver, milestones | Chỉ actor có quyền cập nhật mốc; sau picked up không hủy trực tiếp. | FR-23 đến FR-27, FR-60 |
| Fare | Fare | Trip reference, amount | Chỉ xác định sau Trip completed. | FR-28, BRULE-05 |
| Payment | Payment | Method, result, attempts, provider reference | CASH do Driver xác nhận; ELECTRONIC chỉ có đúng một kết quả cuối dưới callback trùng/muộn. | FR-30 đến FR-34 |
| Rating | Rating | Trip, Customer, Driver, content | Chỉ tạo sau Trip completed. | FR-42 |

Aggregate dùng version để phát hiện cập nhật đồng thời. Version không thay chính sách chọn Driver: quyết định accept cạnh tranh luôn theo BRULE-11 và Candidate Snapshot của Round.

## 5. Domain Event phát sinh từ Use Case

Event chỉ phát sau khi service owner lưu thay đổi nghiệp vụ bền vững. Payload chỉ chứa định danh và dữ liệu tối thiểu cần cho consumer; không có credential hoặc dữ liệu thanh toán nhạy cảm.

| Domain Event | Use Case nguồn | Producer | Consumer chính | Mục đích |
| --- | --- | --- | --- | --- |
| `account.registered` | UC-01, UC-03 | auth-service | customer-service, driver-service, audit-service | Liên kết account với profile đúng loại. |
| `driver.availability.updated` | UC-17 | driver-service | dispatch-service, audit-service | Cập nhật Driver fact phục vụ điều phối. |
| `driver.location.updated` | UC-18 | driver-service | dispatch-service, trip-service | Cập nhật vị trí đã ghi nhận. |
| `booking.submitted` | UC-05 | booking-service | dispatch-service, notification-service, audit-service | Mở PRIMARY Session. |
| `booking.retry.requested` | UC-05 | booking-service | dispatch-service, audit-service | Mở RETRY Session sau retry lock. |
| `booking.cancelled` | UC-05, UC-08 | booking-service hoặc trip-service | dispatch-service, notification-service, audit-service | Thu hồi Offer hoặc thông báo Driver theo trạng thái. |
| `dispatch.session.started` | UC-05 | dispatch-service | booking-service, notification-service, audit-service | Công bố Session mới. |
| `dispatch.offer.issued/responded/expired` | UC-05, UC-06 | dispatch-service | notification-service, audit-service | Theo dõi Offer trong Round. |
| `dispatch.driver.confirmed` | UC-05, UC-06 | dispatch-service | booking-service, trip-service, notification-service, reporting-service, audit-service | Xác nhận Driver hợp lệ. |
| `dispatch.session.exhausted` | UC-05 | dispatch-service | booking-service, notification-service, reporting-service, audit-service | Báo no-driver và khởi tạo retry lock. |
| `trip.driver-cannot-serve` | UC-08 | trip-service | dispatch-service, notification-service, audit-service | Có thể mở đúng một RECOVERY Session. |
| `trip.arrived-at-pickup/customer-picked-up/in-progress/completed` | UC-08 | trip-service | notification-service, pricing-service, history/rating, reporting, audit | Công bố mốc Trip; completed kích hoạt Fare. |
| `fare.calculated` | UC-09 | pricing-service | payment-service, history/rating, notification-service, reporting-service, audit-service | Công bố Fare của Trip. |
| `payment.cash.confirmed/electronic.finalized` | UC-09 | payment-service | notification-service, history/rating, reporting-service, audit-service | Công bố kết quả thanh toán đã ghi nhận. |
| `rating.created` | UC-11 | rating-service | reporting-service, audit-service | Công bố Rating hợp lệ. |

## 6. Ánh xạ Context sang Microservice

| Bounded Context | Service đề xuất | Kho dữ liệu logic | Giao tiếp chính |
| --- | --- | --- | --- |
| Identity and Access | `auth-service` | auth store | API đồng bộ qua Gateway; phát `account.registered`. |
| Customer Profile | `customer-service` | customer store | API profile; nhận liên kết account. |
| Driver Management | `driver-service` | driver store | API Driver, status và location; phát Driver facts. |
| Vehicle Management | `vehicle-service` | vehicle store | API Vehicle theo quyền. |
| Booking | `booking-service` | booking store | API Customer; phát submitted/retry/cancelled. |
| Dispatch | `dispatch-service` | dispatch store | Nhận event Booking, Driver facts; API Offer cho Driver; phát kết quả. |
| Trip Management | `trip-service` | trip store | Nhận Driver confirmed; API mốc Trip. |
| Fare | `pricing-service` | pricing store | Nhận `trip.completed`; phát Fare. |
| Payment | `payment-service` | payment store | Nhận Fare; adapter tới Payment Provider. |
| History and Rating | `rating-service` | rating store | API Rating; nhận event tạo read model nếu cần. |
| Notification | `notification-service` | notification store | Nhận event, gọi adapter kênh. |
| Operations | `operations-service` | operations store | API Operations; gọi API/command của service owner. |
| Reporting | `reporting-service` | reporting store | Nhận event; read API báo cáo. |
| Audit | `audit-service` | audit store | Nhận audit event; read API theo quyền. |

## 7. Mô tả Service

### 7.1. auth-service

| Nội dung | Thiết kế |
| --- | --- |
| Trách nhiệm | Đăng ký, đăng nhập, xác thực và chính sách access được duyệt. |
| Dữ liệu sở hữu | Account, credential metadata, access state. |
| API ngoài | Đăng ký, đăng nhập, xác thực phiên. |
| Event phát | `account.registered`, audit event khi áp dụng. |
| Không sở hữu | Customer Profile, Driver Profile, Vehicle. |

### 7.2. customer-service, driver-service và vehicle-service

| Service | Trách nhiệm | Dữ liệu sở hữu | Event/API chính |
| --- | --- | --- | --- |
| customer-service | Hồ sơ Customer. | Customer Profile, account link. | Xem/cập nhật profile; audit event. |
| driver-service | Driver, availability và location. | Driver, Availability, Driver Location. | Cập nhật hồ sơ/trạng thái/vị trí; phát Driver facts. |
| vehicle-service | Thông tin phương tiện. | Vehicle, Driver Vehicle Link. | Cập nhật/truy vấn Vehicle theo quyền. |

### 7.3. booking-service

| Nội dung | Thiết kế |
| --- | --- |
| Trách nhiệm | Nhận Booking, sửa thông tin trước retry, hủy trong thời điểm cho phép và quản lý retry lock. |
| Dữ liệu sở hữu | Booking, Retry Lock. |
| API ngoài | Tạo/xem/sửa Booking; retry; hủy theo trạng thái. |
| Event phát | `booking.submitted`, `booking.retry.requested`, `booking.cancelled`. |
| Không sở hữu | Session, Round, Offer và quyết định Driver. |

### 7.4. dispatch-service

| Nội dung | Thiết kế |
| --- | --- |
| Trách nhiệm | Tự động tạo Session/Round/Offer, chọn Driver và xử lý Session thất bại/phục hồi. |
| Dữ liệu sở hữu | Dispatch Session, Round, Offer, Candidate Snapshot. |
| API ngoài | Driver phản hồi Offer còn hiệu lực; Operations chỉ xem trạng thái. |
| Event nhận | Booking submitted/retry/cancelled, Driver facts, `trip.driver-cannot-serve`. |
| Event phát | Session/Offer lifecycle, Driver confirmed, Session exhausted. |
| Invariant then chốt | 3 Round × 20 Driver × 20 giây; tie-break 1 giây; không manual dispatch; RECOVERY tối đa một lần. |

### 7.5. trip-service

| Nội dung | Thiết kế |
| --- | --- |
| Trách nhiệm | Tạo/liên kết Trip sau Driver confirmed, cập nhật mốc và quản lý việc không thể phục vụ trước pickup. |
| Dữ liệu sở hữu | Trip, Trip Milestone. |
| API ngoài | Xem Trip, Driver cập nhật arrived/picked up/in progress/completed; hủy khi hợp lệ. |
| Event nhận | `dispatch.driver.confirmed`. |
| Event phát | Mốc Trip, `trip.driver-cannot-serve`, `trip.completed`. |

### 7.6. pricing-service và payment-service

| Service | Trách nhiệm | Dữ liệu sở hữu | Event/API chính |
| --- | --- | --- | --- |
| pricing-service | Xác định Fare sau Trip hoàn thành. | Fare. | Nhận `trip.completed`; phát `fare.calculated`. |
| payment-service | Chọn phương thức, CASH/ELECTRONIC, retry và callback Provider. | Payment, Payment Attempt, provider reference. | Nhận Fare; callback xác thực; phát kết quả cuối. |

### 7.7. notification-service, rating-service, operations-service, reporting-service và audit-service

| Service | Trách nhiệm | Dữ liệu sở hữu | Nguyên tắc |
| --- | --- | --- | --- |
| notification-service | Gửi thông báo và lưu delivery result. | Notification, Delivery Result. | Lỗi gửi không làm mất thay đổi nghiệp vụ nguồn. |
| rating-service | Tạo Rating sau Trip completed. | Rating. | Không sở hữu Trip. |
| operations-service | Điểm vào cho Operations Staff. | Operational Case, Support Action. | Gọi owner; không sửa trực tiếp DB khác, không gán Driver tay. |
| reporting-service | Xây read model báo cáo. | Reporting View, Reporting Period. | Không thay đổi dữ liệu nguồn. |
| audit-service | Lưu audit trail độc lập. | Audit Entry. | Bất biến; không lưu credential hay payment sensitive data. |

## 8. Mô hình dữ liệu cho Service

Đây là mô hình logic, không phải schema vật lý. Trường bắt buộc/duy nhất, chính sách retention, danh mục Vehicle/service type và công thức Fare chỉ được chốt khi Open Issue liên quan đã được phê duyệt.

| Service | Entity logic | Quan hệ chính |
| --- | --- | --- |
| auth-service | Account, Access State | Account liên kết một Customer hoặc Driver profile theo loại account. |
| driver-service | Driver, Availability, Driver Location | Nhiều location tham chiếu một Driver; chỉ location mới ≤60 giây là Dispatch fact hợp lệ. |
| vehicle-service | Vehicle, Driver Vehicle Link | Vehicle liên kết Driver theo quy tắc account/input còn mở. |
| booking-service | Booking, Retry Lock | Một Booking có thể sinh PRIMARY, RETRY và tối đa một RECOVERY Session qua Dispatch. |
| dispatch-service | Dispatch Session, Round, Offer, Candidate Snapshot | Một Session tối đa 3 Round; mỗi Round tối đa 20 Offer; một Offer tham chiếu đúng một Driver. |
| trip-service | Trip, Trip Milestone | Trip tham chiếu Booking và Driver được xác nhận. |
| pricing-service | Fare | Fare tham chiếu Trip completed. |
| payment-service | Payment, Payment Attempt | Payment tham chiếu Fare/Trip; nhiều attempt điện tử là mô hình kỹ thuật cho retry. |
| rating-service | Rating | Rating tham chiếu Trip, Customer và Driver. |
| reporting-service | Reporting View, Reporting Period | Read model từ selected events. |
| audit-service | Audit Entry | Entry tham chiếu actor, action, target, correlation ID. |

## 9. Hợp đồng giao tiếp và nhất quán dữ liệu

### 9.1. API đồng bộ

API đồng bộ dùng cho command/query cần phản hồi tức thời: Customer tạo/sửa/retry/hủy Booking, Driver trả lời Offer hoặc cập nhật Trip, Customer chọn/thử lại Payment, và Operations Staff tra cứu/hỗ trợ theo quyền. API xác thực, kiểm tra object-level authorization, nhận `Idempotency-Key` với command có thể lặp và trả `correlation_id` để truy vết.

### 9.2. Giao tiếp bất đồng bộ

Event phân phối thay đổi đã lưu tới Notification, Reporting, Audit và các service nghiệp vụ phụ thuộc. Service consumer chỉ cập nhật dữ liệu thuộc ownership của mình. Triển khai cần bảo đảm phát event bền vững, khử trùng lặp theo `event_id`, xử lý callback muộn/trùng và theo dõi lỗi consumer.

### 9.3. Luồng nghiệp vụ liên service

**Booking và điều phối.** Customer tạo Booking tại `booking-service`; service phát `booking.submitted`. `dispatch-service` dùng Driver fact hợp lệ, mở tối đa ba Round và phát Offer tới tối đa 20 Driver trong từng Round. Nếu accept hợp lệ cùng cửa sổ một giây, Dispatch chọn theo Candidate Snapshot distance rồi thời điểm nhận. Khi xác nhận Driver, Dispatch phát `dispatch.driver.confirmed`; `trip-service` tạo/liên kết Trip. Sau Session thất bại, Booking áp dụng retry lock 10 giây; retry dùng chính Booking. Nếu Driver đã xác nhận không thể phục vụ trước pickup, Trip phát event cho Dispatch mở đúng một RECOVERY Session, loại Driver đó.

**Hoàn thành Trip và thanh toán.** `trip-service` lưu completed trước khi phát `trip.completed`. `pricing-service` xác định Fare rồi phát `fare.calculated`. Customer chọn CASH hoặc ELECTRONIC tại `payment-service`; CASH hoàn thành khi Driver xác nhận. Callback ELECTRONIC trùng hoặc muộn không thể tạo kết quả cuối thứ hai. Lỗi Payment không đảo ngược Trip đã completed.

**Thông báo và read model.** Notification, Reporting, Rating/History và Audit nhận event sau thay đổi nghiệp vụ. Lỗi notification/read model không làm mất thay đổi Booking, Dispatch, Trip hoặc Payment đã được ghi nhận.

## 10. Bảo mật vận hành và khả năng phục hồi

| Chủ đề | Thiết kế |
| --- | --- |
| Phân quyền | Gateway và service owner cùng kiểm tra. Ma trận permission và thao tác nhạy cảm tuân theo OI-08/OI-20. |
| Thanh toán | Không lưu số thẻ/tài khoản nhạy cảm. Callback Provider phải được xác thực; kết quả được idempotent theo provider reference. |
| Tính nhất quán | Owner ghi dữ liệu trước khi công bố event; correlation ID, causation ID và aggregate version phục vụ truy vết/cập nhật cạnh tranh. |
| Dispatch | Candidate Snapshot bất biến trong Round; cơ chế version/lock phải bảo đảm chỉ một Driver được confirmed theo BRULE-11. |
| Khả năng phục hồi | Theo dõi Round quá hạn, event backlog, callback Payment chưa rõ kết quả và notification lỗi. Mục tiêu tải/sẵn sàng/phục hồi định lượng chờ OI-13. |
| Lưu trữ và audit | Retention vị trí, lịch sử, Payment và Audit theo OI-06; Audit không ghi dữ liệu nhạy cảm. |

## 11. Quyết định cần có trước khi triển khai

| Nhóm quyết định | Open Issue | Ảnh hưởng thiết kế |
| --- | --- | --- |
| Fare, loại xe và Payment | OI-01, OI-07, OI-15, OI-17 | Pricing, Payment adapter, payment contract. |
| Hủy và Trip lỗi | OI-04, OI-14, OI-20 | Booking/Trip command, Operations support. |
| Vị trí, ETA và retention | OI-05, OI-06, OI-19 | Driver location ingestion, ETA, storage. |
| Quyền và đầu vào account | OI-08, OI-18 | Auth, profile/vehicle API và authorization. |
| Notification, báo cáo, chất lượng | OI-09, OI-12, OI-13 | Channel adapter, Reporting model, resilience target. |
| Rating và bảo vệ/audit | OI-10, OI-16 | Rating policy, audit content/query. |
| Phạm vi release đầu tiên | OI-11 | Phân kỳ triển khai, không thay ownership miền. |

## 12. Truy vết thiết kế về SRS

| Nhóm yêu cầu SRS | Context/Service thực hiện |
| --- | --- |
| FR-01 đến FR-06, FR-56 đến FR-57 | auth-service, customer-service, driver-service, vehicle-service |
| FR-07 đến FR-10 | driver-service, vehicle-service |
| FR-11 đến FR-19, FR-59 đến FR-60 | booking-service, dispatch-service, trip-service, notification-service |
| FR-20 đến FR-27, FR-41 | booking-service, dispatch-service, trip-service, driver-service |
| FR-28 đến FR-34 | pricing-service, payment-service, notification-service |
| FR-35 đến FR-40 | notification-service cùng source owner |
| FR-42 | rating-service |
| FR-43 đến FR-50 | operations-service cùng service owner |
| FR-51 đến FR-55 | reporting-service |
| FR-58 | audit-service cùng source owner |
