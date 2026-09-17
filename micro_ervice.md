# THIẾT KẾ MICRO SERVICE

## 1. Phân tách Domain thành các Sub-domain

Hệ thống **Cab Management System (CAB SYSTEM)** được phân tách từ Domain tổng thể thành các Sub-domain dựa trên năng lực nghiệp vụ, trách nhiệm nghiệp vụ, các đối tượng nghiệp vụ và mối quan hệ giữa các Use Case.

### 1.1. Tổng quan phân tách Domain

| Domain | Sub-domain | Loại Sub-domain | Năng lực nghiệp vụ chính |
|---|---|---|---|
| Identity & Access | Authentication | Supporting | Đăng ký, đăng nhập, đăng xuất, xác thực người dùng |
| Identity & Access | Authorization | Supporting | Phân quyền và kiểm soát quyền truy cập |
| Customer Management | Customer Profile | Supporting | Quản lý thông tin khách hàng |
| Driver Management | Driver Profile | Supporting | Quản lý thông tin tài xế |
| Driver Management | Driver Availability | Supporting | Quản lý trạng thái hoạt động của tài xế |
| Vehicle Management | Vehicle Management | Supporting | Quản lý thông tin và trạng thái phương tiện |
| Booking Management | Booking | **Core** | Tạo và quản lý yêu cầu đặt xe |
| Booking Management | Booking Cancellation | **Core** | Xử lý hủy yêu cầu đặt xe |
| Trip Management | Trip | **Core** | Quản lý quá trình thực hiện chuyến đi |
| Dispatch Management | Driver Assignment | **Core** | Điều phối và phân tài xế cho chuyến |
| Pricing Management | Fare Calculation | **Core** | Tính toán giá chuyến đi |
| Payment Management | Payment Processing | Supporting | Xử lý và quản lý thanh toán |
| Rating Management | Rating | Supporting | Đánh giá chuyến đi và tài xế |
| Administration | System Administration | Generic | Quản trị và giám sát hệ thống |

---

## 2. Phân tách Use Case theo Sub-domain

Các Use Case được nhóm lại dựa trên sự tương đồng về nghiệp vụ và dữ liệu mà chúng sử dụng. Những Use Case có cùng mục tiêu nghiệp vụ và cùng phạm vi trách nhiệm được đặt trong cùng một Bounded Context.

### 2.1. Identity & Access

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Authentication | Đăng ký, đăng nhập, đăng xuất | Account, Credential, Session |
| Authorization | Xác thực và phân quyền | Role, Permission |
| Password Management | Đổi mật khẩu, quên mật khẩu | Password, OTP |

**Bounded Context:** `Identity & Access`

**Trách nhiệm chính:**
- Quản lý tài khoản người dùng.
- Xác thực thông tin đăng nhập.
- Quản lý phiên đăng nhập.
- Quản lý quyền truy cập.
- Xử lý quên và thay đổi mật khẩu.

---

### 2.2. Customer Management

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Customer Profile | Xem thông tin khách hàng | Customer, CustomerProfile |
| Customer Profile | Cập nhật thông tin khách hàng | CustomerProfile |
| Customer Management | Quản lý khách hàng | Customer |

**Bounded Context:** `Customer Management`

**Trách nhiệm chính:**
- Quản lý hồ sơ khách hàng.
- Cập nhật thông tin khách hàng.
- Tra cứu thông tin khách hàng.
- Quản lý lịch sử sử dụng dịch vụ của khách hàng.

---

### 2.3. Driver Management

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Driver Profile | Quản lý thông tin tài xế | Driver, DriverProfile |
| Driver Availability | Cập nhật trạng thái tài xế | DriverStatus |
| Driver Management | Quản lý tài xế | Driver |

**Bounded Context:** `Driver Management`

**Trách nhiệm chính:**
- Quản lý hồ sơ tài xế.
- Quản lý trạng thái hoạt động của tài xế.
- Theo dõi khả năng nhận chuyến của tài xế.
- Quản lý thông tin phục vụ điều phối.

---

### 2.4. Vehicle Management

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Vehicle Management | Thêm phương tiện | Vehicle |
| Vehicle Management | Cập nhật phương tiện | Vehicle |
| Vehicle Management | Xóa phương tiện | Vehicle |
| Vehicle Status | Quản lý trạng thái phương tiện | VehicleStatus |
| Vehicle Assignment | Gán phương tiện cho tài xế | Vehicle, Driver |

**Bounded Context:** `Vehicle Management`

**Trách nhiệm chính:**
- Quản lý thông tin phương tiện.
- Quản lý loại phương tiện.
- Quản lý trạng thái phương tiện.
- Liên kết phương tiện với tài xế.

---

## 3. Booking Management

### 3.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Create Booking | Tạo yêu cầu đặt xe | Booking |
| View Booking | Xem thông tin đặt xe | Booking |
| Update Booking | Cập nhật yêu cầu đặt xe | Booking |
| Cancel Booking | Hủy yêu cầu đặt xe | Booking |
| Booking Status | Theo dõi trạng thái đặt xe | BookingStatus |

**Bounded Context:** `Booking Management`

### 3.2. Aggregate

**Aggregate:** `Booking`

**Aggregate Root:** `Booking`

### 3.3. Thành phần chính

```text
Booking
├── BookingId
├── CustomerId
├── PickupLocation
├── DropoffLocation
├── VehicleType
├── BookingTime
├── PaymentMethod
├── Fare
└── BookingStatus
```

### 3.4. Invariant nghiệp vụ

1. Booking phải thuộc về một Customer hợp lệ.
2. Booking phải có điểm đón và điểm trả.
3. Điểm đón và điểm trả phải hợp lệ trước khi xác nhận Booking.
4. Booking chỉ được chuyển sang trạng thái tiếp theo theo đúng vòng đời nghiệp vụ.
5. Booking đã hoàn thành không được hủy.

### 3.5. Trạng thái Booking

```text
PENDING
   │
   ├── CANCELLED
   │
   └── CONFIRMED
          │
          └── DRIVER_ASSIGNED
                    │
                    └── IN_PROGRESS
                              │
                              └── COMPLETED
```

---

## 4. Dispatch Management

### 4.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Driver Assignment | Phân tài xế cho Booking | Driver, Booking |
| Driver Selection | Tìm tài xế phù hợp | DriverStatus, Location |
| Assignment Management | Xác nhận tài xế nhận chuyến | Assignment |

**Bounded Context:** `Dispatch Management`

**Trách nhiệm chính:**
- Nhận thông tin Booking đã xác nhận.
- Tìm tài xế có khả năng nhận chuyến.
- Phân tài xế cho Booking.
- Cập nhật thông tin điều phối.

---

## 5. Trip Management

### 5.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Start Trip | Bắt đầu chuyến | Trip |
| Trip Tracking | Theo dõi chuyến | Trip, Location |
| Complete Trip | Kết thúc chuyến | Trip |
| Trip Cancellation | Hủy chuyến | Trip |

**Bounded Context:** `Trip Management`

### 5.2. Aggregate

**Aggregate:** `Trip`

**Aggregate Root:** `Trip`

### 5.3. Thành phần chính

```text
Trip
├── TripId
├── BookingId
├── CustomerId
├── DriverId
├── VehicleId
├── PickupLocation
├── DropoffLocation
├── StartTime
├── EndTime
├── Distance
└── TripStatus
```

### 5.4. Invariant nghiệp vụ

1. Trip phải được tạo từ một Booking hợp lệ.
2. Trip phải có Driver được phân công.
3. Trip chỉ được bắt đầu khi Booking đã được xác nhận và tài xế đã nhận chuyến.
4. Trip chỉ được hoàn thành sau khi đã bắt đầu.
5. Trip đã hoàn thành không thể quay lại trạng thái đang thực hiện.

---

## 6. Pricing Management

### 6.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Fare Calculation | Tính giá chuyến | Fare |
| Pricing Rule | Quản lý quy tắc tính giá | PricingRule |
| Fare Management | Quản lý giá | Fare |

**Bounded Context:** `Pricing Management`

**Trách nhiệm chính:**
- Xác định quy tắc tính giá.
- Tính giá dựa trên thông tin chuyến.
- Cung cấp giá cho Booking.
- Lưu lại thông tin giá tại thời điểm đặt chuyến.

### 6.2. Aggregate

**Aggregate:** `Fare`

**Aggregate Root:** `Fare`

---

## 7. Payment Management

### 7.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Payment Processing | Thực hiện thanh toán | Payment |
| Payment Verification | Kiểm tra trạng thái thanh toán | Transaction |
| Payment History | Tra cứu lịch sử thanh toán | Payment |
| Refund | Hoàn tiền | Refund |

**Bounded Context:** `Payment Management`

### 7.2. Aggregate

**Aggregate:** `Payment`

**Aggregate Root:** `Payment`

### 7.3. Thành phần chính

```text
Payment
├── PaymentId
├── BookingId
├── CustomerId
├── Amount
├── PaymentMethod
├── TransactionId
└── PaymentStatus
```

### 7.4. Invariant nghiệp vụ

1. Payment phải liên kết với Booking hoặc Trip hợp lệ.
2. Số tiền thanh toán phải phù hợp với số tiền cần thanh toán.
3. Một giao dịch đã thanh toán thành công không được thực hiện thanh toán lại.
4. Refund chỉ được thực hiện khi thỏa điều kiện nghiệp vụ.

---

## 8. Rating Management

### 8.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Create Rating | Đánh giá chuyến đi | Rating |
| View Rating | Xem đánh giá | Rating |
| Driver Rating | Xem đánh giá tài xế | DriverRating |

**Bounded Context:** `Rating Management`

### 8.2. Aggregate

**Aggregate:** `Rating`

**Aggregate Root:** `Rating`

### 8.3. Invariant nghiệp vụ

1. Rating phải thuộc về một Trip hợp lệ.
2. Customer chỉ được đánh giá chuyến mà mình đã thực hiện.
3. Rating phải nằm trong phạm vi giá trị được hệ thống quy định.

---

## 9. Administration

### 9.1. Năng lực nghiệp vụ

| Năng lực nghiệp vụ | Use Case liên quan | Khái niệm chính |
|---|---|---|
| Customer Administration | Quản lý khách hàng | Customer |
| Driver Administration | Quản lý tài xế | Driver |
| Vehicle Administration | Quản lý phương tiện | Vehicle |
| Booking Administration | Quản lý Booking | Booking |
| System Monitoring | Theo dõi hệ thống | System |

**Bounded Context:** `Administration`

**Trách nhiệm chính:**
- Quản trị dữ liệu hệ thống.
- Quản lý Customer.
- Quản lý Driver.
- Quản lý Vehicle.
- Theo dõi Booking và Trip.
- Cấu hình các nghiệp vụ được phân quyền.

---

## 10. Ubiquitous Language

| Thuật ngữ | Ý nghĩa | Context |
|---|---|---|
| Customer | Khách hàng sử dụng dịch vụ | Customer Management |
| Driver | Tài xế thực hiện chuyến | Driver Management |
| Vehicle | Phương tiện được sử dụng | Vehicle Management |
| Booking | Yêu cầu đặt xe của khách hàng | Booking Management |
| Trip | Chuyến đi được thực hiện từ Booking | Trip Management |
| Fare | Chi phí của chuyến đi | Pricing Management |
| Payment | Giao dịch thanh toán cho chuyến | Payment Management |
| Rating | Đánh giá của khách hàng sau chuyến | Rating Management |
| Assignment | Thông tin phân tài xế cho Booking | Dispatch Management |

---

# 11. Bounded Context và Context Map

### 11.1. Các Bounded Context

| Bounded Context | Mục đích |
|---|---|
| Identity & Access | Quản lý danh tính, xác thực và phân quyền |
| Customer Management | Quản lý thông tin khách hàng |
| Driver Management | Quản lý thông tin và trạng thái tài xế |
| Vehicle Management | Quản lý phương tiện |
| Booking Management | Quản lý yêu cầu đặt xe |
| Dispatch Management | Điều phối và phân tài xế |
| Trip Management | Quản lý quá trình thực hiện chuyến |
| Pricing Management | Tính toán và quản lý giá |
| Payment Management | Xử lý thanh toán |
| Rating Management | Quản lý đánh giá |
| Administration | Quản trị hệ thống |

### 11.2. Context Map

```text
                         ┌──────────────────────┐
                         │ Identity & Access    │
                         │    auth-service      │
                         └──────────┬───────────┘
                                    │
                   ┌────────────────┼────────────────┐
                   │                │                │
                   ▼                ▼                ▼
          ┌────────────────┐ ┌───────────────┐ ┌────────────────┐
          │ Customer       │ │ Driver        │ │ Vehicle        │
          │ Management     │ │ Management    │ │ Management     │
          │ customer       │ │ driver        │ │ vehicle        │
          │ service        │ │ service       │ │ service        │
          └────────┬───────┘ └───────┬───────┘ └───────┬────────┘
                   │                 │                 │
                   └─────────────────┼─────────────────┘
                                     │
                                     ▼
                           ┌──────────────────┐
                           │ Booking          │
                           │ Management       │
                           │ booking-service  │
                           └────────┬─────────┘
                                    │
                   ┌────────────────┼────────────────┐
                   │                │                │
                   ▼                ▼                ▼
          ┌────────────────┐ ┌───────────────┐ ┌────────────────┐
          │ Dispatch       │ │ Pricing       │ │ Payment        │
          │ Management     │ │ Management    │ │ Management     │
          │ dispatch       │ │ pricing       │ │ payment        │
          │ service        │ │ service       │ │ service        │
          └───────┬────────┘ └───────────────┘ └────────────────┘
                  │
                  ▼
          ┌──────────────────┐
          │ Trip Management  │
          │ trip-service     │
          └────────┬─────────┘
                   │
                   ▼
          ┌──────────────────┐
          │ Rating           │
          │ Management       │
          │ rating-service   │
          └──────────────────┘
```

---

# 12. Aggregate và Invariant nghiệp vụ

| Bounded Context | Aggregate | Aggregate Root | Invariant chính |
|---|---|---|---|
| Identity & Access | Account | Account | Account phải có thông tin xác thực hợp lệ |
| Customer Management | Customer | Customer | Customer có định danh duy nhất |
| Driver Management | Driver | Driver | Driver phải có trạng thái hợp lệ |
| Vehicle Management | Vehicle | Vehicle | Vehicle phải có trạng thái hợp lệ |
| Booking Management | Booking | Booking | Booking phải có Customer, điểm đón và điểm trả hợp lệ |
| Dispatch Management | Assignment | Assignment | Chỉ phân Driver đủ điều kiện nhận chuyến |
| Trip Management | Trip | Trip | Trip phải xuất phát từ Booking hợp lệ |
| Pricing Management | Fare | Fare | Fare phải được tính theo Pricing Rule |
| Payment Management | Payment | Payment | Payment phải gắn với giao dịch hợp lệ |
| Rating Management | Rating | Rating | Rating phải thuộc Trip hợp lệ |

---

# 13. Domain Event phát sinh từ các Use Case

| Domain Event | Producer Context | Consumer Context | Mục đích |
|---|---|---|---|
| `customer.registered` | Customer Management | Identity & Access | Tạo/xác lập tài khoản khách hàng |
| `driver.registered` | Driver Management | Identity & Access | Tạo tài khoản cho tài xế |
| `booking.created` | Booking Management | Dispatch Management | Thông báo có Booking cần điều phối |
| `booking.confirmed` | Booking Management | Dispatch Management | Booking sẵn sàng để phân tài xế |
| `driver.assigned` | Dispatch Management | Booking Management | Thông báo Booking đã được phân tài xế |
| `trip.started` | Trip Management | Payment Management | Thông báo chuyến bắt đầu |
| `trip.completed` | Trip Management | Payment Management | Thông báo chuyến hoàn thành |
| `payment.completed` | Payment Management | Booking Management | Thông báo thanh toán hoàn tất |
| `trip.rated` | Rating Management | Driver Management | Cập nhật thông tin đánh giá tài xế |

---

# 14. Ánh xạ Context sang Microservice

| Bounded Context | Service triển khai | Dữ liệu sở hữu | Giao tiếp chính |
|---|---|---|---|
| Identity & Access | `auth-service` | `auth_db` | REST qua API Gateway |
| Customer Management | `customer-service` | `customer_db` | REST / Event |
| Driver Management | `driver-service` | `driver_db` | REST / Event |
| Vehicle Management | `vehicle-service` | `vehicle_db` | REST / Event |
| Booking Management | `booking-service` | `booking_db` | REST / Event |
| Dispatch Management | `dispatch-service` | `dispatch_db` | REST / Event |
| Trip Management | `trip-service` | `trip_db` | REST / Event |
| Pricing Management | `pricing-service` | `pricing_db` | REST / Event |
| Payment Management | `payment-service` | `payment_db` | REST / Event |
| Rating Management | `rating-service` | `rating_db` | REST / Event |
| Administration | `admin-service` | `admin_db` | REST qua API Gateway |

---

# 15. Mô tả Microservice

| Microservice | Trách nhiệm chính | Dữ liệu sở hữu |
|---|---|---|
| `auth-service` | Đăng ký, đăng nhập, xác thực, phân quyền, quản lý phiên | `auth_db` |
| `customer-service` | Quản lý hồ sơ và thông tin khách hàng | `customer_db` |
| `driver-service` | Quản lý hồ sơ và trạng thái tài xế | `driver_db` |
| `vehicle-service` | Quản lý phương tiện và trạng thái phương tiện | `vehicle_db` |
| `booking-service` | Tạo, cập nhật, xác nhận và hủy Booking | `booking_db` |
| `dispatch-service` | Tìm và phân tài xế cho Booking | `dispatch_db` |
| `trip-service` | Quản lý quá trình thực hiện Trip | `trip_db` |
| `pricing-service` | Tính toán và quản lý giá chuyến | `pricing_db` |
| `payment-service` | Xử lý và quản lý giao dịch thanh toán | `payment_db` |
| `rating-service` | Quản lý đánh giá chuyến đi và tài xế | `rating_db` |
| `admin-service` | Quản trị và giám sát hệ thống | `admin_db` |

---

# 16. Kiến trúc Microservice tổng thể

```text
                           CLIENT
                              │
                              ▼
                       ┌──────────────┐
                       │ API GATEWAY  │
                       └──────┬───────┘
                              │
          ┌───────────────────┼────────────────────┐
          │                   │                    │
          ▼                   ▼                    ▼
   ┌────────────┐      ┌──────────────┐     ┌────────────┐
   │   Auth     │      │   Customer   │     │   Driver   │
   │  Service   │      │   Service    │     │  Service   │
   └─────┬──────┘      └──────┬───────┘     └─────┬──────┘
         │                    │                    │
      auth_db             customer_db          driver_db

          ┌────────────────────────────────────────────┐
          │                                            │
          ▼                                            ▼
   ┌────────────┐                              ┌────────────┐
   │  Vehicle   │                              │  Booking   │
   │  Service   │                              │  Service   │
   └─────┬──────┘                              └─────┬──────┘
         │                                           │
    vehicle_db                                       │
                                                     │
                              ┌──────────────────────┼─────────────────────┐
                              │                      │                     │
                              ▼                      ▼                     ▼
                       ┌────────────┐        ┌────────────┐        ┌────────────┐
                       │  Dispatch  │        │  Pricing   │        │  Payment   │
                       │  Service   │        │  Service   │        │  Service   │
                       └─────┬──────┘        └────────────┘        └────────────┘
                             │
                        dispatch_db
                             │
                             ▼
                       ┌────────────┐
                       │    Trip    │
                       │  Service   │
                       └─────┬──────┘
                             │
                          trip_db
                             │
                             ▼
                       ┌────────────┐
                       │   Rating   │
                       │  Service   │
                       └────────────┘
                             │
                         rating_db
```

---

# 17. Nguyên tắc thiết kế Microservice

Việc phân tách Microservice được thực hiện dựa trên ranh giới nghiệp vụ của từng Bounded Context.

### 17.1. Business Capability

Mỗi Microservice chịu trách nhiệm cho một hoặc một nhóm Business Capability có liên quan chặt chẽ với nhau.

### 17.2. Data Ownership

Mỗi Microservice sở hữu dữ liệu thuộc phạm vi nghiệp vụ của mình và không cho Microservice khác truy cập trực tiếp vào Database.

### 17.3. Loose Coupling

Các Microservice hạn chế phụ thuộc trực tiếp vào nhau. Khi cần trao đổi thông tin, Service sử dụng API hoặc Domain Event.

### 17.4. Business Rule Ownership

Business Rule thuộc Context nào thì Context đó chịu trách nhiệm thực thi và đảm bảo tính nhất quán.

### 17.5. Independent Deployment

Mỗi Microservice có thể được phát triển, kiểm thử, triển khai và mở rộng tương đối độc lập.

### 17.6. Domain Event

Các sự kiện nghiệp vụ được sử dụng để thông báo sự thay đổi trạng thái giữa các Context mà không tạo ra sự phụ thuộc chặt chẽ.

---

# 18. Kết quả phân tách

Domain tổng thể:

```text
Cab Management System
```

được phân tách thành các Bounded Context:

```text
Cab Management System
│
├── Identity & Access
│   └── auth-service
│
├── Customer Management
│   └── customer-service
│
├── Driver Management
│   └── driver-service
│
├── Vehicle Management
│   └── vehicle-service
│
├── Booking Management
│   └── booking-service
│
├── Dispatch Management
│   └── dispatch-service
│
├── Trip Management
│   └── trip-service
│
├── Pricing Management
│   └── pricing-service
│
├── Payment Management
│   └── payment-service
│
├── Rating Management
│   └── rating-service
│
└── Administration
    └── admin-service
```

Kết quả phân tách giúp xác định rõ phạm vi trách nhiệm của từng miền nghiệp vụ, dữ liệu mà từng Service sở hữu, các quy tắc nghiệp vụ được quản lý trong từng Context và cách các Microservice giao tiếp với nhau thông qua API hoặc Domain Event.

# 19. Kết luận

Thiết kế Microservice cho **Cab Management System** được xây dựng dựa trên nguyên tắc phân tách Domain theo **Business Capability và Bounded Context**. Mỗi Bounded Context có trách nhiệm nghiệp vụ riêng, sở hữu dữ liệu riêng và được ánh xạ thành một Microservice tương ứng.

Luồng nghiệp vụ chính của hệ thống có thể được mô tả như sau:

```text
Customer
   │
   ▼
Booking
   │
   ▼
Pricing
   │
   ▼
Dispatch
   │
   ▼
Driver
   │
   ▼
Trip
   │
   ├──────────► Payment
   │
   └──────────► Rating
```

Trong đó, **Booking, Dispatch, Trip và Pricing** là các miền có vai trò trực tiếp trong hoạt động cung cấp dịch vụ vận chuyển; các miền như **Identity & Access, Customer, Driver, Vehicle, Payment và Rating** hỗ trợ cho quá trình vận hành của hệ thống.
