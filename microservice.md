# THIẾT KẾ MICROSERVICE HỆ THỐNG CAB

## 1. Kiến trúc Microservice

Hệ thống CAB được phân rã thành các Microservice dựa trên ranh giới nghiệp vụ. Mỗi Microservice chịu trách nhiệm cho một nhóm nghiệp vụ có tính liên kết cao, sở hữu dữ liệu thuộc phạm vi của mình và cung cấp các API cần thiết cho các thành phần khác.

Việc phân rã không thực hiện theo từng API riêng lẻ mà dựa trên **business capability và vòng đời của đối tượng nghiệp vụ**. Các API có cùng trách nhiệm nghiệp vụ được đặt trong cùng một Microservice.

Các Microservice được xác định từ phạm vi nghiệp vụ của hệ thống gồm:

| STT | Microservice               | Bounded Context   | Trách nhiệm chính                          |
| --: | -------------------------- | ----------------- | ------------------------------------------ |
|   1 | **Auth Service**           | Identity & Access | Đăng ký và đăng nhập người dùng            |
|   2 | **User Service**           | User Profile      | Quản lý thông tin người dùng               |
|   3 | **Driver Service**         | Driver Management | Quản lý thông tin tài xế                   |
|   4 | **Ride Service**           | Ride Management   | Quản lý yêu cầu và vòng đời chuyến         |
|   5 | **Driver Request Service** | Driver Dispatch   | Quản lý việc gửi yêu cầu chuyến đến tài xế |
|   6 | **Payment Service**        | Payment           | Quản lý thanh toán của chuyến              |
|   7 | **Rating Service**         | Rating            | Quản lý đánh giá sau chuyến                |
|   8 | **Operations Service**     | Operations        | Theo dõi chuyến và xử lý sự cố             |

Ngoài các Microservice nội bộ, hệ thống có các hệ thống bên ngoài được repo xác định gồm **Payment Gateway** và **Map/Location Service**.

---

# 2. Nguyên tắc phân tách

Mỗi Microservice phải đáp ứng các nguyên tắc sau:

### 2.1. Single Business Responsibility

Một Microservice chỉ chịu trách nhiệm cho một phạm vi nghiệp vụ rõ ràng.

Ví dụ:

* Auth Service chỉ xử lý xác thực.
* User Service chỉ xử lý hồ sơ người dùng.
* Ride Service xử lý vòng đời Ride.
* Payment Service xử lý giao dịch thanh toán.
* Rating Service xử lý đánh giá.

Auth Service không xử lý hồ sơ người dùng và Ride Service không xử lý thanh toán.

### 2.2. Data Ownership

Mỗi Microservice sở hữu dữ liệu thuộc nghiệp vụ của mình.

Microservice khác không truy cập trực tiếp Database của Service đó.

Ví dụ:

```text
Ride Service
    │
    └── ride_db

Payment Service
    │
    └── payment_db
```

Payment Service không truy cập trực tiếp `ride_db` để thay đổi trạng thái Ride.

### 2.3. API-based Communication

Các Microservice trao đổi thông tin thông qua API được cung cấp bởi Service sở hữu nghiệp vụ.

Ví dụ:

```text
Ride Service
      │
      │ request driver
      ▼
Driver Request Service
```

Ride Service không truy cập trực tiếp bảng `driver_requests`.

### 2.4. Business Boundary

Ranh giới Service được xác định theo **trách nhiệm nghiệp vụ**, không phải theo số lượng bảng hoặc số lượng endpoint.

Do đó:

```text
POST /rides
GET /rides
GET /rides/{ride_id}
PATCH /rides/{ride_id}/status
POST /rides/{ride_id}/cancel
```

được giữ trong **Ride Service** vì đều liên quan đến vòng đời của một Ride.

---

# 3. Auth Service

## 3.1. Bounded Context

**Identity & Access**

## 3.2. Business Responsibility

Auth Service chịu trách nhiệm xác thực người dùng và cấp thông tin xác thực để người dùng truy cập các chức năng của hệ thống.

## 3.3. API

| Method | Endpoint         | Chức năng         |
| ------ | ---------------- | ----------------- |
| POST   | `/auth/register` | Đăng ký tài khoản |
| POST   | `/auth/login`    | Đăng nhập         |

Các API nghiệp vụ sử dụng Bearer Token/JWT để xác thực người dùng.

## 3.4. Data Ownership

```text
auth_db
└── User authentication information
```

Auth Service sở hữu thông tin phục vụ xác thực.

## 3.5. Không thuộc trách nhiệm

Auth Service không xử lý:

* Tạo Ride.
* Quản lý trạng thái Ride.
* Thông tin chuyến.
* Thanh toán.
* Đánh giá.

---

# 4. User Service

## 4.1. Bounded Context

**User Profile**

## 4.2. Business Responsibility

User Service quản lý thông tin hồ sơ của người dùng đã đăng nhập.

## 4.3. API

| Method | Endpoint    | Chức năng                              |
| ------ | ----------- | -------------------------------------- |
| GET    | `/users/me` | Xem thông tin người dùng hiện tại      |
| PATCH  | `/users/me` | Cập nhật thông tin người dùng hiện tại |

## 4.4. Data Ownership

```text
user_db
└── User Profile
```

## 4.5. Ranh giới với Auth Service

Hai Service có trách nhiệm khác nhau:

```text
Auth Service
"Người dùng có xác thực hợp lệ không?"

User Service
"Thông tin hồ sơ của người dùng là gì?"
```

Auth Service không cập nhật trực tiếp Profile Database.

---

# 5. Driver Service

## 5.1. Bounded Context

**Driver Management**

## 5.2. Business Responsibility

Driver Service quản lý thông tin nghiệp vụ của tài xế và phương tiện thuộc hồ sơ tài xế.

## 5.3. API

| Method | Endpoint      | Chức năng                          |
| ------ | ------------- | ---------------------------------- |
| PATCH  | `/drivers/me` | Tài xế cập nhật thông tin của mình |

## 5.4. Data Ownership

```text
driver_db
├── Driver
└── Vehicle
```

## 5.5. Ranh giới

Driver Service trả lời:

> Tài xế là ai và thông tin tài xế/phương tiện là gì?

Driver Service không chịu trách nhiệm:

> Tài xế hiện có sẵn sàng nhận chuyến hay không?

Thông tin trạng thái sẵn sàng được sử dụng trong quá trình Driver Request/Dispatch.

---

# 6. Ride Service

## 6.1. Bounded Context

**Ride Management**

## 6.2. Business Responsibility

Ride Service là Service quản lý đối tượng nghiệp vụ trung tâm `Ride`.

Service chịu trách nhiệm:

* Tạo Ride.
* Xem Ride.
* Liệt kê Ride.
* Cập nhật trạng thái Ride.
* Hủy Ride.
* Lưu thông tin điểm đón và điểm đến.
* Lưu loại xe.
* Lưu giá dự kiến và giá cuối.

## 6.3. API

| Method | Endpoint                  | Chức năng           |
| ------ | ------------------------- | ------------------- |
| POST   | `/rides`                  | Tạo Ride            |
| GET    | `/rides`                  | Danh sách Ride      |
| GET    | `/rides/{ride_id}`        | Chi tiết Ride       |
| PATCH  | `/rides/{ride_id}/status` | Cập nhật trạng thái |
| POST   | `/rides/{ride_id}/cancel` | Hủy Ride            |

## 6.4. Ride Lifecycle

```text
SEARCHING_DRIVER
       │
       ▼
DRIVER_ASSIGNED
       │
       ▼
DRIVER_ARRIVING
       │
       ▼
IN_PROGRESS
       │
       ▼
COMPLETED
```

Các trạng thái kết thúc:

```text
CANCELLED
NO_DRIVER
```

Các trạng thái trên thuộc cùng vòng đời của `Ride`, do đó không tách thành các Microservice riêng.

## 6.5. Data Ownership

```text
ride_db
└── Ride
```

Ride Service sở hữu dữ liệu Ride.

## 6.6. Ranh giới

Ride Service quyết định:

> Ride đang ở trạng thái nào?

Ride Service không quyết định:

> Tài xế nào được gửi yêu cầu?

Việc này thuộc Driver Request Service.

---

# 7. Fare Estimation

## 7.1. Phạm vi

Repo có API:

```text
POST /rides/estimate
```

API nhận:

* Điểm đón.
* Điểm đến.
* Loại xe.

và trả về:

* Khoảng cách dự kiến.
* Thời gian dự kiến.
* Giá dự kiến.
* Đơn vị tiền tệ.

## 7.2. Vị trí trong kiến trúc

Fare Estimation **không được tự động tách thành một Microservice độc lập chỉ vì nó có một endpoint riêng**.

Nếu logic tính giá vẫn thuộc cùng phạm vi xử lý của Ride và repo không mô tả một vòng đời hoặc boundary độc lập cho Pricing, chức năng này được xem là **một capability bên trong Ride Service**.

```text
Ride Service
├── Ride Management
└── Fare Estimation
```

Cách này tránh tạo Microservice chỉ để "chia nhỏ cho nhiều".

---

# 8. Driver Request Service

## 8.1. Bounded Context

**Driver Dispatch**

## 8.2. Business Responsibility

Driver Request Service chịu trách nhiệm quản lý quá trình gửi yêu cầu Ride đến tài xế và ghi nhận phản hồi của tài xế.

## 8.3. API

| Method | Endpoint                                | Chức năng                  |
| ------ | --------------------------------------- | -------------------------- |
| POST   | `/rides/{ride_id}/driver-requests`      | Tạo yêu cầu gửi đến tài xế |
| POST   | `/driver-requests/{request_id}/respond` | Tài xế phản hồi yêu cầu    |

## 8.4. Quy tắc nghiệp vụ

Theo repo:

* Chỉ tài xế `AVAILABLE` được xem xét.
* Tài xế phải phù hợp loại xe.
* Phạm vi tìm kiếm là bán kính 5 km.
* Có thể ưu tiên tài xế có đánh giá cao khi Customer yêu cầu.
* Khoảng cách được sử dụng trong thứ tự lựa chọn.
* Một yêu cầu tài xế có thời gian phản hồi 30 giây.
* Nếu tài xế từ chối hoặc hết hạn, hệ thống chuyển sang tài xế tiếp theo.
* Khi tài xế nhận chuyến, Ride được khóa và tài xế chuyển sang trạng thái bận.

## 8.5. Data Ownership

```text
driver_request_db
└── DriverRequest
```

`DriverRequest` lưu lại từng lần hệ thống gửi yêu cầu cho tài xế.

---

# 9. Payment Service

## 9.1. Bounded Context

**Payment**

## 9.2. Business Responsibility

Payment Service chịu trách nhiệm xử lý và lưu trạng thái thanh toán của Ride.

## 9.3. API

| Method | Endpoint                                   | Chức năng                       |
| ------ | ------------------------------------------ | ------------------------------- |
| POST   | `/rides/{ride_id}/payments`                | Tạo thanh toán                  |
| POST   | `/payments/{payment_id}/cash-confirmation` | Xác nhận thanh toán tiền mặt    |
| POST   | `/payments/callback`                       | Nhận kết quả từ Payment Gateway |

## 9.4. Payment Method

Repo hỗ trợ:

```text
CASH
ONLINE
```

## 9.5. Payment Status

```text
CASH_PENDING
PENDING
PAID
FAILED
```

## 9.6. Data Ownership

```text
payment_db
└── Payment
```

Payment Service sở hữu Payment và Transaction Information.

Payment Service không thay đổi trực tiếp dữ liệu Ride.

---

# 10. Rating Service

## 10.1. Bounded Context

**Rating**

## 10.2. Business Responsibility

Rating Service chịu trách nhiệm ghi nhận đánh giá của Customer đối với Driver sau khi Ride hoàn thành.

## 10.3. API

| Method | Endpoint                  | Chức năng    |
| ------ | ------------------------- | ------------ |
| POST   | `/rides/{ride_id}/rating` | Tạo đánh giá |

## 10.4. Business Rules

* Ride phải ở trạng thái `COMPLETED`.
* Customer phải là người của Ride.
* Điểm đánh giá từ 1 đến 5.
* Một Ride không được đánh giá nhiều lần.

## 10.5. Data Ownership

```text
rating_db
└── Rating
```

Rating Service không quản lý vòng đời Ride.

---

# 11. Operations Service

## 11.1. Bounded Context

**Operations**

## 11.2. Business Responsibility

Operations Service phục vụ nhân viên vận hành trong việc theo dõi các chuyến đang hoạt động và ghi nhận sự cố.

## 11.3. API

| Method | Endpoint                   | Chức năng                   |
| ------ | -------------------------- | --------------------------- |
| GET    | `/operations/rides/active` | Xem các Ride đang hoạt động |
| POST   | `/operations/incidents`    | Ghi nhận sự cố              |

## 11.4. Incident

Các loại sự cố được repo xác định:

```text
SAFETY
PAYMENT
DRIVER
CUSTOMER
TECHNICAL
OTHER
```

Trạng thái:

```text
OPEN
IN_PROGRESS
RESOLVED
CLOSED
```

## 11.5. Data Ownership

```text
operations_db
├── Operation
└── Incident
```

Operations Service sử dụng thông tin từ các nghiệp vụ khác để phục vụ vận hành nhưng không trở thành nơi sở hữu dữ liệu Ride, Payment hoặc Driver.

---

# 12. Tổng hợp Microservice và Database

| Microservice           | Database            | Aggregate/Entity chính |
| ---------------------- | ------------------- | ---------------------- |
| Auth Service           | `auth_db`           | User Authentication    |
| User Service           | `user_db`           | User Profile           |
| Driver Service         | `driver_db`         | Driver, Vehicle        |
| Ride Service           | `ride_db`           | Ride                   |
| Driver Request Service | `driver_request_db` | DriverRequest          |
| Payment Service        | `payment_db`        | Payment                |
| Rating Service         | `rating_db`         | Rating                 |
| Operations Service     | `operations_db`     | Incident, Operation    |

Nguyên tắc:

```text
1 Service
     │
     └── 1 phạm vi nghiệp vụ
              │
              └── 1 Database sở hữu
```

Không sử dụng Database chung giữa các Microservice.

---

# 13. Giao tiếp giữa các Microservice

Kiến trúc giao tiếp được tổ chức theo hướng Service gọi đến Service sở hữu nghiệp vụ cần thiết.

```text
                         ┌───────────────┐
                         │  Auth Service │
                         └───────┬───────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │ User Service  │
                         └───────────────┘


Customer
   │
   ▼
┌───────────────┐
│  Ride Service │
└───────┬───────┘
        │
        │ Driver Request
        ▼
┌────────────────────────┐
│ Driver Request Service │
└───────┬────────────────┘
        │
        ├──────────────► Driver Service
        │
        └──────────────► Driver availability/location


        Ride completed
              │
       ┌──────┴──────┐
       ▼             ▼
┌─────────────┐ ┌─────────────┐
│Payment      │ │Rating       │
│Service      │ │Service      │
└─────────────┘ └─────────────┘

Operations
     │
     ├──────► xem Ride đang hoạt động
     │
     └──────► ghi nhận Incident
```

---

# 14. API Gateway và Authentication

Các API nghiệp vụ yêu cầu xác thực được bảo vệ bằng Bearer Token/JWT.

Luồng tổng quát:

```text
Client
   │
   │ Login
   ▼
Auth Service
   │
   │ Token
   ▼
Client
   │
   │ Bearer Token
   ▼
API
   │
   ├── User Service
   ├── Driver Service
   ├── Ride Service
   ├── Driver Request Service
   ├── Payment Service
   ├── Rating Service
   └── Operations Service
```

Auth Service chịu trách nhiệm xác thực token; Service nghiệp vụ chịu trách nhiệm kiểm tra quyền truy cập phù hợp với nghiệp vụ mà nó cung cấp.

---

# 15. Luồng nghiệp vụ xuyên Microservice

## 15.1. Đặt và thực hiện chuyến

```text
Customer
   │
   │ 1. Login
   ▼
Auth Service
   │
   │ 2. Authentication
   ▼
Customer
   │
   │ 3. Create Ride
   ▼
Ride Service
   │
   │ 4. Find/Request Driver
   ▼
Driver Request Service
   │
   │ 5. Check Driver
   ▼
Driver Service
   │
   │ 6. Driver accepts
   ▼
Driver Request Service
   │
   │ 7. Update Ride assignment/status
   ▼
Ride Service
   │
   │ 8. Driver performs Ride
   ▼
Ride Service
   │
   │ 9. COMPLETED
   ├─────────────────┐
   ▼                 ▼
Payment Service   Rating Service
```

Điểm quan trọng là **mỗi bước không có nghĩa là một Microservice mới**. Microservice được xác định bởi trách nhiệm nghiệp vụ mà nó sở hữu.

---

# 16. Ranh giới trách nhiệm giữa các Microservice

| Nghiệp vụ              | Service sở hữu         | Service không sở hữu |
| ---------------------- | ---------------------- | -------------------- |
| Đăng nhập              | Auth Service           | User, Ride           |
| Hồ sơ User             | User Service           | Auth                 |
| Hồ sơ Driver           | Driver Service         | Driver Request       |
| Tạo Ride               | Ride Service           | Driver Request       |
| Trạng thái Ride        | Ride Service           | Payment              |
| Tìm/Gửi yêu cầu Driver | Driver Request Service | Ride Service         |
| Thông tin Driver       | Driver Service         | Ride Service         |
| Thanh toán             | Payment Service        | Ride Service         |
| Đánh giá               | Rating Service         | Ride Service         |
| Sự cố vận hành         | Operations Service     | Ride Service         |

---

# 17. Nguyên tắc không chồng lấn

Các Microservice được thiết kế sao cho một nghiệp vụ chỉ có **một nơi sở hữu chính**.

Ví dụ:

### Không đúng

```text
Ride Service ── quản lý Payment
Payment Service ── quản lý Payment
```

### Đúng

```text
Ride Service
    │
    └── quản lý Ride

Payment Service
    │
    └── quản lý Payment
```

Ride Service chỉ cần biết thông tin thanh toán cần thiết cho Ride.

Tương tự:

### Không đúng

```text
Driver Service
    ├── Driver
    ├── Driver Availability
    ├── Driver Request
    └── Assignment
```

### Đúng

```text
Driver Service
    └── Driver information

Driver Request Service
    └── DriverRequest / Assignment process
```

Như vậy, việc thay đổi thuật toán tìm tài xế không buộc phải thay đổi logic quản lý hồ sơ tài xế.

---

# 18. Kiến trúc Microservice tổng thể

```text
                         ┌────────────────────┐
                         │       Client       │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │    API Gateway     │
                         └─────────┬──────────┘
                                   │
             ┌─────────────────────┼─────────────────────┐
             │                     │                     │
             ▼                     ▼                     ▼
      ┌─────────────┐       ┌─────────────┐      ┌──────────────┐
      │Auth Service │       │User Service │      │Driver Service│
      └──────┬──────┘       └──────┬──────┘      └──────┬───────┘
             │                     │                    │
             ▼                     ▼                    ▼
          auth_db              user_db              driver_db


                         ┌─────────────────┐
                         │   Ride Service  │
                         └────────┬────────┘
                                  │
                 ┌────────────────┼────────────────┐
                 │                │                │
                 ▼                ▼                ▼
       ┌────────────────┐  ┌──────────────┐  ┌───────────────┐
       │Driver Request  │  │Payment       │  │Rating         │
       │Service         │  │Service       │  │Service        │
       └───────┬────────┘  └──────┬───────┘  └───────┬───────┘
               │                  │                  │
               ▼                  ▼                  ▼
       driver_request_db      payment_db          rating_db


                         ┌──────────────────┐
                         │Operations Service│
                         └────────┬─────────┘
                                  │
                                  ▼
                            operations_db


External Systems:

       ┌─────────────────────┐
       │   Payment Gateway   │
       └──────────┬──────────┘
                  │
                  ▼
           Payment Service


       ┌─────────────────────┐
       │ Map/Location Service│
       └──────────┬──────────┘
                  │
                  ▼
              Ride/Driver
```

# 19. Kết luận thiết kế

Hệ thống CAB được phân tách thành các Microservice dựa trên **business boundary** thay vì chia nhỏ theo số lượng API hoặc bảng dữ liệu.

Các trách nhiệm cốt lõi được tách thành:

**Authentication → User → Driver → Ride → Driver Request → Payment → Rating → Operations**

Trong đó:

* **Auth Service** quản lý xác thực.
* **User Service** quản lý hồ sơ người dùng.
* **Driver Service** quản lý thông tin tài xế và phương tiện.
* **Ride Service** quản lý toàn bộ vòng đời Ride.
* **Driver Request Service** quản lý quá trình tìm và nhận chuyến.
* **Payment Service** quản lý thanh toán.
* **Rating Service** quản lý đánh giá.
* **Operations Service** quản lý hoạt động vận hành và sự cố.

Các Service có **ranh giới trách nhiệm riêng**, **sở hữu dữ liệu riêng** và chỉ giao tiếp thông qua interface được cung cấp. Khi ghép các Service lại, chúng tạo thành một hệ thống CAB hoàn chỉnh nhưng mỗi Service vẫn có thể được phát triển và thay đổi tương đối độc lập.
