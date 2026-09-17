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
