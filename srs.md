# CAB SYSTEM - Business Analysis Document

 ## 1. Phân tích bối cảnh nghiệp vụ

 ### 1.1. Business context

 **Business context** là bối cảnh mà doanh nghiệp đang hoạt động, bao gồm mục tiêu, quy trình, các bên liên quan, dữ liệu, quy định và những vấn đề cần giải quyết. Trong bài toán này, doanh nghiệp cung cấp dịch vụ kết nối nhu cầu di chuyển của khách hàng với tài xế trong khu vực.

 Bối cảnh hiện tại:

 - Khách hàng cần đặt xe nhanh, biết trước điểm đón, điểm đến và chi phí dự kiến.
 - Tài xế cần nhận được yêu cầu phù hợp với vị trí, loại xe và trạng thái sẵn sàng.
 - Doanh nghiệp cần quản lý chuyến đi, doanh thu, đánh giá và lịch sử giao dịch.
 - Việc điều phối phải hoạt động theo thời gian gần thực tế và có thể xử lý nhiều yêu cầu đồng thời.

 ### 1.2. Business customer và business customization

 - **Business customer** là đối tượng trực tiếp sử dụng hoặc trả tiền cho sản phẩm. Trong hệ thống này gồm khách hàng đi xe, tài xế và doanh nghiệp vận hành nền tảng.
 - **Business customization** là phần điều chỉnh hệ thống theo chính sách riêng của doanh nghiệp, ví dụ bán kính tìm tài xế 5 km, loại xe được hỗ trợ, phương thức thanh toán, cách tính phí, thời gian chờ xác nhận và chính sách hủy chuyến.

 ### 1.3. Vấn đề của hệ thống hiện tại

 Hệ thống hiện tại (điện thoại, tin nhắn hoặc bảng tính) không đáp ứng được các yêu cầu sau:

 1. Không cập nhật được vị trí khách hàng và tài xế theo thời gian thực.
 2. Điều phối thủ công khiến việc tìm tài xế chậm, dễ bỏ sót hoặc phân công trùng chuyến.
 3. Không có quy trình tự động khi tài xế từ chối, hết thời gian xác nhận hoặc không tìm được tài xế.
 4. Không cung cấp ngay giá dự kiến, trạng thái chuyến và lịch sử cho khách hàng.
 5. Thanh toán, đối soát doanh thu và quản lý đánh giá còn rời rạc, khó kiểm tra.
 6. Khó mở rộng khi số lượng khách hàng, tài xế và chuyến đi tăng lên.

 Vì vậy cần xây dựng ứng dụng mới có khả năng tự động hóa quy trình đặt chuyến, kết nối hai phía, quản lý dữ liệu tập trung và tích hợp thanh toán.

 ### 1.4. Các bên liên quan ban đầu

 Khách hàng, tài xế, bộ phận điều phối/chăm sóc khách hàng, quản trị viên, bộ phận tài chính, đối tác thanh toán, đội vận hành kỹ thuật và cơ quan quản lý là những bên có liên quan đến hệ thống.

 ## 2. Stakeholder analysis

 ### 2.1. Bảng xác định stakeholder

 | Mã | Stakeholder | Nhóm | Nhu cầu chính | Mức ảnh hưởng |
 |---|---|---|---|---|
 | ST01 | Khách hàng | Người dùng trực tiếp | Đặt xe nhanh, giá rõ ràng, chuyến an toàn | Cao |
 | ST02 | Tài xế | Người dùng trực tiếp | Nhận chuyến phù hợp, biết điểm đón, nhận thu nhập | Cao |
 | ST03 | Doanh nghiệp vận hành | Chủ sở hữu nghiệp vụ | Tăng số chuyến, doanh thu và chất lượng dịch vụ | Rất cao |
 | ST04 | Điều phối viên/chăm sóc khách hàng | Vận hành | Theo dõi chuyến, hỗ trợ khi có sự cố | Cao |
 | ST05 | Quản trị viên | Quản trị hệ thống | Quản lý tài khoản, cấu hình và báo cáo | Cao |
 | ST06 | Đối tác thanh toán | Đối tác bên ngoài | Xử lý giao dịch trực tuyến chính xác | Trung bình |
 | ST07 | Bộ phận tài chính | Nội bộ doanh nghiệp | Đối soát giao dịch và doanh thu | Trung bình |
 | ST08 | Đội kỹ thuật | Hỗ trợ hệ thống | Hệ thống ổn định, bảo mật, dễ bảo trì | Cao |
 | ST09 | Cơ quan quản lý | Bên quản lý | Tuân thủ quy định vận tải và dữ liệu | Trung bình |

 ### 2.2. Bảng vai trò stakeholder

 | Mã stakeholder | Vai trò trong hệ thống | Quyền hoặc trách nhiệm |
 |---|---|---|
 | ST01 | Người tạo và sử dụng chuyến | Nhập điểm đón/đến, chọn loại xe, thanh toán, đánh giá |
 | ST02 | Người cung cấp dịch vụ | Cập nhật trạng thái sẵn sàng, nhận/từ chối chuyến, thực hiện chuyến |
 | ST03 | Chủ quy trình và chính sách | Phê duyệt mục tiêu, giá, chính sách vận hành và KPI |
 | ST04 | Giám sát và xử lý sự cố | Theo dõi chuyến, hỗ trợ người dùng, xử lý khiếu nại |
 | ST05 | Quản lý dữ liệu cấu hình | Quản lý người dùng, tài xế, loại xe, quyền và báo cáo |
 | ST06 | Xác nhận thanh toán | Tạo kết quả giao dịch và gửi trạng thái về hệ thống |
 | ST07 | Kiểm tra tài chính | Đối soát giao dịch, hoàn tiền và lập báo cáo doanh thu |
 | ST08 | Xây dựng và vận hành kỹ thuật | Phát triển, giám sát hiệu năng, sao lưu và bảo mật |
 | ST09 | Kiểm tra tuân thủ | Yêu cầu báo cáo hoặc dữ liệu theo quy định pháp luật |

 ### 2.3. Stakeholder matrix

 Ma trận dưới đây phân loại theo **mức ảnh hưởng** và **mức quan tâm**.

 | Ảnh hưởng / Quan tâm | Thấp | Cao |
 |---|---|---|
 | **Cao** | ST06 Đối tác thanh toán, ST09 Cơ quan quản lý | ST01 Khách hàng, ST02 Tài xế, ST03 Doanh nghiệp, ST04 Điều phối, ST05 Quản trị viên, ST08 Đội kỹ thuật |
 | **Thấp** | Không ưu tiên ở giai đoạn đầu | ST07 Bộ phận tài chính |

 ```mermaid
 quadrantChart
	 title Stakeholder matrix
	 x-axis "Mức quan tâm thấp" --> "Mức quan tâm cao"
	 y-axis "Ảnh hưởng thấp" --> "Ảnh hưởng cao"
	 quadrant-1 "Quản lý sát"
	 quadrant-2 "Duy trì hài lòng"
	 quadrant-3 "Theo dõi"
	 quadrant-4 "Cập nhật thông tin"
	 ST01 Khách hàng: [0.88, 0.82]
	 ST02 Tài xế: [0.86, 0.80]
	 ST03 Doanh nghiệp: [0.92, 0.95]
	 ST04 Điều phối: [0.78, 0.75]
	 ST05 Quản trị viên: [0.72, 0.78]
	 ST06 Thanh toán: [0.45, 0.68]
	 ST07 Tài chính: [0.65, 0.45]
	 ST08 Kỹ thuật: [0.84, 0.76]
	 ST09 Quản lý: [0.30, 0.62]
 ```

 ## 3. Business goals

 | Mã | Business goal | Chỉ số kết quả mong đợi |
 |---|---|---|
 | BG01 | Cung cấp dịch vụ đặt chuyến nhanh và đáng tin cậy | Khách hàng tạo được chuyến, nhận trạng thái và tài xế trong thời gian ngắn; giảm chuyến bị bỏ sót |
 | BG02 | Cung cấp thanh toán linh hoạt, minh bạch | Hỗ trợ tiền mặt và trực tuyến; lưu được trạng thái giao dịch và đối soát được doanh thu |
 | BG03 | Tăng hiệu quả khai thác tài xế | Ưu tiên tài xế đang sẵn sàng, gần khách hàng và phù hợp loại xe |
 | BG04 | Nâng cao an toàn và chất lượng dịch vụ | Có thông tin tài xế/chuyến, đánh giá sau chuyến và lưu vết xử lý sự cố |

 **BG02 - phương thức thanh toán:**

 - Tiền mặt: khách hàng thanh toán trực tiếp cho tài xế sau khi chuyến kết thúc.
 - Trực tuyến: khách hàng thanh toán qua đối tác thanh toán; hệ thống chỉ xác nhận hoàn tất khi nhận được kết quả hợp lệ.

 ## 4. Phạm vi yêu cầu

 ### Trong phạm vi giai đoạn 1

 - Đăng ký, đăng nhập và quản lý hồ sơ khách hàng/tài xế.
 - Tài xế cập nhật trạng thái sẵn sàng và vị trí.
 - Khách hàng nhập điểm đón, điểm đến, loại xe và tạo chuyến.
 - Tính giá dự kiến, tìm và gửi yêu cầu cho tài xế trong bán kính 5 km.
 - Tài xế nhận hoặc từ chối chuyến; hệ thống chuyển sang tài xế khác khi cần.
 - Theo dõi trạng thái chuyến: chờ tài xế, đã nhận, đang đi, hoàn tất, hủy.
 - Thanh toán tiền mặt hoặc trực tuyến.
 - Đánh giá sau chuyến, lịch sử chuyến và báo cáo cơ bản.

 ### Ngoài phạm vi giai đoạn 1

 - Giao hàng, đặt đồ ăn và vận chuyển hàng hóa.
 - Thuật toán định giá động phức tạp theo thời tiết hoặc sự kiện.
 - Ví điện tử riêng, chương trình hội viên và khuyến mãi nâng cao.
 - Nhận diện khuôn mặt, camera hành trình và trung tâm hỗ trợ khẩn cấp tích hợp.

 ## 5. Business requirements

 Business requirement (BR) mô tả nhu cầu ở cấp độ nghiệp vụ; mã BR01, BR02... là mã định danh duy nhất để truy vết sang các functional requirements (FR).

 | Mã | Tên business requirement | Diễn giải |
 |---|---|---|
 | BR01 | Đặt chuyến | Khách hàng có thể tạo chuyến bằng cách cung cấp điểm đón, điểm đến và loại xe; hệ thống kiểm tra dữ liệu và tạo yêu cầu chuyến. |
 | BR02 | Tìm và phân công tài xế | Hệ thống tìm các tài xế đang sẵn sàng trong bán kính 5 km, gửi yêu cầu theo thứ tự phù hợp và ghi nhận tài xế chấp nhận. |
 | BR03 | Quản lý trạng thái chuyến | Hệ thống cập nhật và hiển thị các trạng thái từ tạo chuyến đến hoàn tất hoặc hủy chuyến cho các bên liên quan. |
 | BR04 | Thanh toán chuyến đi | Khách hàng được chọn tiền mặt hoặc trực tuyến; hệ thống lưu kết quả, trạng thái và thông tin đối soát. |
 | BR05 | Đánh giá và phản hồi | Khách hàng có thể đánh giá tài xế sau chuyến và gửi phản hồi để doanh nghiệp cải thiện chất lượng. |

 ## 6. Business process: đặt chuyến

 ```mermaid
 flowchart TD
	 A([Khách hàng bắt đầu]) --> B[Nhập điểm đón, điểm đến và loại xe]
	 B --> C{Dữ liệu hợp lệ?}
	 C -- Không --> D[Thông báo lỗi và yêu cầu nhập lại]
	 D --> B
	 C -- Có --> E[Tạo chuyến và tính giá dự kiến]
	 E --> F[Tìm tài xế sẵn sàng trong bán kính 5 km]
	 F --> G{Có tài xế phù hợp?}
	 G -- Chưa có --> H[Thông báo đang tìm tài xế và tiếp tục chờ]
	 H --> I{Còn trong thời gian chờ?}
	 I -- Có --> F
	 I -- Không --> J[Thông báo không tìm được tài xế]
	 J --> Z([Kết thúc])
	 G -- Có --> K[Gửi yêu cầu cho tài xế phù hợp]
	 K --> L{Tài xế chấp nhận trong thời hạn?}
	 L -- Có --> M[Xác nhận tài xế và chuyến đi]
	 L -- Không --> N[Đánh dấu hết hạn/từ chối]
	 N --> O{Còn tài xế khác?}
	 O -- Có --> K
	 O -- Không --> H
	 M --> P[Khách hàng và tài xế thực hiện chuyến]
	 P --> Q[Hoàn tất chuyến và tính phí cuối]
	 Q --> R[Thanh toán và đánh giá]
	 R --> Z
 ```

 ## 7. Phân rã thành functional requirements

 | Mã FR | Thuộc BR | Functional requirement | Tiêu chí chính |
 |---|---|---|---|
 | FR01 | BR01 | Xác định vị trí khách hàng | Cho phép nhập hoặc chọn điểm đón trên bản đồ. |
 | FR02 | BR01 | Nhập điểm đến | Bắt buộc có điểm đến hợp lệ và khác điểm đón. |
 | FR03 | BR01 | Chọn loại xe | Cho phép chọn xe 2 bánh hoặc 4 bánh. |
 | FR04 | BR01 | Hiển thị giá dự kiến | Tính và hiển thị giá trước khi khách xác nhận. |
 | FR05 | BR02 | Tìm tài xế trong bán kính 5 km | Lọc tài xế theo khoảng cách và trạng thái sẵn sàng. |
 | FR06 | BR02 | Lọc tài xế theo trạng thái | Chỉ chọn tài xế đang online và sẵn sàng nhận chuyến. |
 | FR07 | BR02 | Lọc theo loại xe | Chỉ gửi chuyến cho tài xế có loại xe khách đã chọn. |
 | FR08 | BR02 | Ưu tiên theo đánh giá | Nếu khách có yêu cầu, ưu tiên tài xế có điểm đánh giá cao hơn. |
 | FR09 | BR02 | Gửi và hết hạn yêu cầu | Gửi yêu cầu tuần tự hoặc theo nhóm; quá thời hạn thì chuyển tài xế khác. |
 | FR10 | BR03 | Quản lý trạng thái chuyến | Lưu các trạng thái và chỉ cho phép chuyển trạng thái hợp lệ. |
 | FR11 | BR03 | Thông báo trạng thái | Gửi thông báo khi tài xế nhận, từ chối, đến điểm đón hoặc hoàn tất. |
 | FR12 | BR04 | Thanh toán tiền mặt | Đánh dấu cần thu tiền mặt và hoàn tất khi tài xế xác nhận. |
 | FR13 | BR04 | Thanh toán trực tuyến | Tạo giao dịch, nhận callback và cập nhật thành công/thất bại. |
 | FR14 | BR05 | Đánh giá sau chuyến | Chỉ cho đánh giá sau khi chuyến hoàn tất; điểm từ 1 đến 5. |
 | FR15 | BR05 | Lưu phản hồi | Lưu nội dung phản hồi gắn với chuyến và tài xế. |

 ## 8. Business rules và exception

 ### 8.1. Business rules

 | Mã | Business rule |
 |---|---|
 | BRL01 | Chỉ tài xế có trạng thái `AVAILABLE` mới được nhận chuyến. |
 | BRL02 | Tài xế phải thuộc đúng loại xe khách đã chọn. |
 | BRL03 | Một tài xế không được nhận đồng thời hai chuyến đang hoạt động. |
 | BRL04 | Yêu cầu tài xế hết hạn sau 30 giây nếu không có phản hồi. |
 | BRL05 | Nếu có nhiều tài xế, ưu tiên tài xế gần hơn; khi khách yêu cầu thì ưu tiên thêm điểm đánh giá cao. |
 | BRL06 | Chỉ chuyến ở trạng thái `COMPLETED` mới được thanh toán cuối và đánh giá. |
 | BRL07 | Giao dịch trực tuyến chỉ chuyển sang `PAID` khi đối tác trả về mã thành công hợp lệ. |
 | BRL08 | Giá cuối có thể thay đổi theo quãng đường thực tế nhưng phải được lưu cùng chuyến để đối soát. |
 | BRL09 | Khách hàng có thể hủy khi chưa bắt đầu chuyến; phí hủy áp dụng theo chính sách doanh nghiệp. |

 ### 8.2. Exception và hướng xử lý

 | Mã | Tình huống ngoại lệ | Hướng xử lý | Kết thúc |
 |---|---|---|---|
 | EX01 | Không tìm thấy tài xế trong thời gian tối đa 3 phút | Thông báo, cho phép khách thử lại hoặc hủy yêu cầu | Đóng chuyến ở `NO_DRIVER` hoặc `CANCELLED` |
 | EX02 | Tài xế không phản hồi trong 30 giây | Đánh dấu yêu cầu hết hạn, gửi cho tài xế kế tiếp | Tiếp tục tìm hoặc kết thúc nếu hết danh sách |
 | EX03 | Tài xế đã nhận nhưng hủy trước khi đón khách | Giải phóng tài xế, tìm tài xế khác và thông báo khách | Kết thúc khi có tài xế mới hoặc không còn tài xế |
 | EX04 | Khách không xác nhận tài xế trong 60 giây | Hủy lượt giữ chỗ và chuyển sang tài xế khác | Kết thúc lượt xác nhận hiện tại |
 | EX05 | Thanh toán trực tuyến thất bại | Báo lỗi, cho phép thử lại hoặc chuyển sang tiền mặt | Đóng giao dịch thất bại, chuyến vẫn chờ thanh toán |
 | EX06 | Mất kết nối khi đang đặt chuyến | Lấy lại trạng thái cuối từ máy chủ khi kết nối lại | Không tạo chuyến trùng; kết thúc khi đồng bộ xong |
 | EX07 | Điểm đón/đến không hợp lệ | Hiển thị lỗi và yêu cầu nhập địa điểm khác | Không tạo chuyến |

 ## 9. Data modeling và ERD

 ### 9.1. Các thực thể chính

 - `User`: tài khoản và thông tin đăng nhập chung.
 - `Driver`: hồ sơ tài xế, trạng thái sẵn sàng, vị trí và điểm đánh giá.
 - `Vehicle`: phương tiện và loại xe.
 - `Ride`: chuyến đi, điểm đón, điểm đến, giá và trạng thái.
 - `Payment`: phương thức và kết quả thanh toán.
 - `DriverRequest`: từng lượt gửi yêu cầu đến một tài xế.
 - `Rating`: đánh giá của khách hàng sau chuyến.

 ### 9.2. ERD

 ```mermaid
 erDiagram
	 USER ||--o| DRIVER : "có hồ sơ"
	 DRIVER ||--o{ VEHICLE : "sở hữu"
	 USER ||--o{ RIDE : "tạo"
	 RIDE ||--o{ DRIVER_REQUEST : "gửi đến"
	 DRIVER ||--o{ DRIVER_REQUEST : "nhận"
	 RIDE ||--o| PAYMENT : "có"
	 RIDE ||--o| RATING : "được đánh giá"
	 USER ||--o{ RATING : "viết"
	 DRIVER ||--o{ RATING : "nhận"

	 USER {
		 int user_id PK
		 string full_name
		 string phone UK
		 string role
		 string status
	 }
	 DRIVER {
		 int driver_id PK
		 int user_id FK
		 decimal rating
		 string availability_status
		 decimal current_latitude
		 decimal current_longitude
	 }
	 VEHICLE {
		 int vehicle_id PK
		 int driver_id FK
		 string vehicle_type
		 string license_plate UK
		 string status
	 }
	 RIDE {
		 int ride_id PK
		 int customer_id FK
		 string pickup_address
		 decimal pickup_latitude
		 decimal pickup_longitude
		 string destination_address
		 decimal destination_latitude
		 decimal destination_longitude
		 string vehicle_type
		 decimal estimated_fare
		 decimal final_fare
		 string status
		 datetime created_at
	 }
	 DRIVER_REQUEST {
		 int request_id PK
		 int ride_id FK
		 int driver_id FK
		 string status
		 datetime sent_at
		 datetime responded_at
	 }
	 PAYMENT {
		 int payment_id PK
		 int ride_id FK
		 string method
		 string status
		 decimal amount
		 string provider_transaction_id
		 datetime paid_at
	 }
	 RATING {
		 int rating_id PK
		 int ride_id FK
		 int customer_id FK
		 int driver_id FK
		 int score
		 string comment
		 datetime created_at
	 }
 ```

 ### 9.3. Một số ràng buộc dữ liệu

 - `phone` và `license_plate` là duy nhất.
 - `score` chỉ nhận giá trị từ 1 đến 5.
 - `vehicle_type` chỉ nhận `BIKE` hoặc `CAR` trong giai đoạn 1.
 - Một `RIDE` chỉ có tối đa một `PAYMENT` và một `RATING`.
 - `DRIVER_REQUEST` giúp lưu lịch sử từng lần gửi, từ chối và hết hạn; nhờ đó hệ thống không gửi trùng cho cùng một tài xế.

 ## 10. Non-functional requirements (NFR)

 Non-functional requirement mô tả **chất lượng và ràng buộc vận hành** của hệ thống, không mô tả một nghiệp vụ riêng lẻ. Các NFR dưới đây được đặt theo bối cảnh ứng dụng đặt xe thời gian gần thực tế.

 | Mã | Nhóm | Non-functional requirement | Tiêu chí kiểm tra/nghiệm thu |
 |---|---|---|---|
 | NFR01 | Hiệu năng | Màn hình tạo chuyến phải phản hồi nhanh sau khi khách xác nhận điểm đón, điểm đến và loại xe. | 95% yêu cầu trả kết quả trong không quá 2 giây khi tải bình thường. |
 | NFR02 | Hiệu năng | Yêu cầu tìm tài xế phải được gửi đến nhóm tài xế phù hợp trong thời gian gần thực tế. | 95% yêu cầu được gửi trong không quá 5 giây sau khi tạo chuyến. |
 | NFR03 | Tính sẵn sàng | Khách hàng và tài xế có thể sử dụng chức năng đặt/nhận chuyến trong thời gian vận hành. | Availability tối thiểu 99,5% mỗi tháng, không tính thời gian bảo trì đã thông báo. |
 | NFR04 | Khả năng mở rộng | Hệ thống xử lý được nhiều khách hàng, tài xế và chuyến đi đồng thời. | Đáp ứng tối thiểu 1.000 yêu cầu đặt chuyến/phút mà không vượt tiêu chí NFR01. |
 | NFR05 | Bảo mật | Dữ liệu tài khoản, vị trí, giao dịch và thông tin tài xế phải được bảo vệ. | Mật khẩu được băm; truyền dữ liệu qua HTTPS; phân quyền theo vai trò; không ghi mật khẩu vào log. |
 | NFR06 | Riêng tư | Chỉ người dùng và nhân viên có quyền mới được xem vị trí/chuyến liên quan. | Kiểm thử phân quyền không cho khách xem dữ liệu của khách khác. |
 | NFR07 | Tin cậy và nhất quán | Không được tạo chuyến hoặc thanh toán trùng khi người dùng bấm lại hoặc mất mạng. | Cùng một mã yêu cầu chỉ tạo tối đa một chuyến và một giao dịch hợp lệ. |
 | NFR08 | Khả năng phục hồi | Khi một dịch vụ tạm thời lỗi, hệ thống phải lưu yêu cầu và khôi phục trạng thái. | Có retry có giới hạn, log lỗi và khôi phục dữ liệu sau sự cố dịch vụ. |
 | NFR09 | Dễ sử dụng | Khách hàng mới có thể hoàn thành việc đặt chuyến mà không cần hướng dẫn trực tiếp. | Người dùng hoàn thành luồng đặt xe trong tối đa 5 bước chính trên ứng dụng. |
 | NFR10 | Tương thích | Ứng dụng chạy trên các phiên bản Android/iOS được doanh nghiệp hỗ trợ và API bản đồ/thanh toán. | Kiểm thử trên danh sách thiết bị và phiên bản do doanh nghiệp phê duyệt. |
 | NFR11 | Quan sát và truy vết | Các sự kiện tạo chuyến, phân tài xế, thanh toán và lỗi phải có log. | Mỗi sự kiện có mã chuyến, thời gian, tác nhân và kết quả; không log dữ liệu nhạy cảm. |
 | NFR12 | Bảo trì | Mã nguồn được chia module và API có tài liệu để đội kỹ thuật vận hành. | Có test tự động cho luồng chính và tài liệu API cho các endpoint tích hợp. |

 ## 11. Use case và đặc tả use case

 ### 11.1. Actor và nhóm use case

 | Mã actor | Actor | Mô tả |
 |---|---|---|
 | A01 | Customer (Khách hàng) | Tạo chuyến, chọn loại xe, theo dõi chuyến, thanh toán và đánh giá. |
 | A02 | Driver (Tài xế) | Cập nhật sẵn sàng, nhận/từ chối chuyến, thực hiện và hoàn tất chuyến. |
 | A03 | Operations staff | Theo dõi chuyến, hỗ trợ khách/tài xế và xử lý khiếu nại. |
 | A04 | System administrator | Quản lý tài khoản, tài xế, xe, cấu hình và báo cáo. |
 | A05 | Payment gateway | Xác nhận hoặc từ chối giao dịch trực tuyến. |
 | A06 | Map/Location service | Cung cấp bản đồ, tọa độ, khoảng cách và định tuyến. |

 Trong tài liệu này, `Customer` là actor A01; mã `UC01` là use case **Đặt chuyến** mà Customer khởi tạo. Một use case là một mục tiêu hoàn chỉnh của actor, còn các bước xử lý bên trong được mô tả ở phần đặc tả.

 ### 11.2. Use case diagram

 ```mermaid
 flowchart LR
	 Customer["A01 Customer"]
	 Driver["A02 Driver"]
	 Operations["A03 Operations staff"]
	 Admin["A04 System administrator"]
	 Payment["A05 Payment gateway"]
	 Map["A06 Map/Location service"]

	 subgraph GrabRide["Ứng dụng đặt chuyến"]
		 UC01(("UC01 Đặt chuyến"))
		 UC02(("UC02 Tìm và nhận chuyến"))
		 UC03(("UC03 Theo dõi chuyến"))
		 UC04(("UC04 Thực hiện và hoàn tất chuyến"))
		 UC05(("UC05 Thanh toán chuyến"))
		 UC06(("UC06 Hủy chuyến"))
		 UC07(("UC07 Đánh giá tài xế"))
		 UC08(("UC08 Quản lý tài khoản và cấu hình"))
		 UC09(("UC09 Hỗ trợ và xử lý sự cố"))
	 end

	 Customer --> UC01
	 Customer --> UC03
	 Customer --> UC05
	 Customer --> UC06
	 Customer --> UC07
	 Driver --> UC02
	 Driver --> UC03
	 Driver --> UC04
	 Driver --> UC06
	 Payment --> UC05
	 Map --> UC01
	 Map --> UC03
	 Operations --> UC03
	 Operations --> UC09
	 Admin --> UC08
	 UC01 -. "include" .-> UC03
	 UC01 -. "include" .-> UC02
	 UC04 -. "include" .-> UC05
	 UC04 -. "extend" .-> UC07
	 UC01 -. "extend" .-> UC06
 ```

 ### 11.3. Danh sách use case

 | Mã | Tên use case | Actor chính | Kết quả thành công |
 |---|---|---|---|
 | UC01 | Đặt chuyến | Customer | Tạo chuyến ở trạng thái `SEARCHING_DRIVER`. |
 | UC02 | Tìm và nhận chuyến | Driver | Một tài xế phù hợp nhận chuyến ở trạng thái `DRIVER_ASSIGNED`. |
 | UC03 | Theo dõi chuyến | Customer, Driver | Hai bên xem được trạng thái, vị trí và thông tin chuyến. |
 | UC04 | Thực hiện và hoàn tất chuyến | Driver | Chuyến chuyển sang `COMPLETED` và có giá cuối. |
 | UC05 | Thanh toán chuyến | Customer, Payment gateway | Giao dịch ở `PAID` hoặc được ghi nhận là tiền mặt cần thu. |
 | UC06 | Hủy chuyến | Customer, Driver | Chuyến được hủy theo đúng điều kiện và chính sách phí. |
 | UC07 | Đánh giá tài xế | Customer | Lưu điểm đánh giá và phản hồi sau chuyến. |
 | UC08 | Quản lý tài khoản và cấu hình | System administrator | Dữ liệu tài khoản, loại xe và chính sách được cập nhật. |
 | UC09 | Hỗ trợ và xử lý sự cố | Operations staff | Sự cố được ghi nhận, xử lý và có lịch sử truy vết. |

 ### 11.4. Đặc tả UC01 - Đặt chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC01 - Đặt chuyến |
 | Actor chính | A01 Customer |
 | Actor phụ | A06 Map/Location service |
 | Mục tiêu | Tạo một yêu cầu di chuyển với điểm đón, điểm đến, loại xe và phương thức thanh toán. |
 | Tiền điều kiện | Customer đã đăng nhập; dịch vụ bản đồ đang hoạt động; tài khoản không bị khóa. |
 | Kích hoạt | Customer chọn chức năng đặt xe. |
 | Hậu điều kiện thành công | Tạo một `Ride`, tính giá dự kiến và chuyển sang `SEARCHING_DRIVER`. |
 | Hậu điều kiện thất bại | Không tạo chuyến hợp lệ; hiển thị nguyên nhân để Customer sửa hoặc thoát. |

 **Luồng chính:**

 1. Customer nhập hoặc chọn điểm đón.
 2. Hệ thống lấy tọa độ từ dịch vụ bản đồ và hiển thị điểm đón.
 3. Customer nhập điểm đến.
 4. Hệ thống tính khoảng cách, thời gian và giá dự kiến.
 5. Customer chọn `BIKE` hoặc `CAR`, phương thức thanh toán và xác nhận.
 6. Hệ thống kiểm tra dữ liệu, chống tạo trùng và tạo `Ride`.
 7. Hệ thống chuyển sang UC02 để tìm tài xế và hiển thị trạng thái cho Customer.

 **Luồng thay thế/ngoại lệ:**

 - A1: Điểm đón hoặc điểm đến không xác định được: thông báo lỗi, quay lại bước nhập địa điểm.
 - A2: Hai điểm không hợp lệ hoặc trùng nhau: không cho xác nhận chuyến.
 - A3: Không có loại xe trong khu vực: thông báo và cho phép chọn loại xe khác.
 - A4: Customer bấm xác nhận nhiều lần: chỉ tạo một `Ride` theo khóa yêu cầu duy nhất.
 - A5: Mất mạng: lưu trạng thái ở máy chủ; khi kết nối lại tải trạng thái cuối, không tạo chuyến trùng.

 ### 11.5. Đặc tả UC02 - Tìm và nhận chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC02 - Tìm và nhận chuyến |
 | Actor chính | Hệ thống và A02 Driver |
 | Tiền điều kiện | UC01 đã tạo chuyến; Customer chưa hủy; có dữ liệu vị trí tài xế gần nhất. |
 | Hậu điều kiện thành công | Một Driver được gán; chuyến chuyển sang `DRIVER_ASSIGNED`. |
 | Hậu điều kiện thất bại | Hết 3 phút hoặc hết danh sách; chuyến chuyển sang `NO_DRIVER`. |

 **Luồng chính:**

 1. Hệ thống lọc Driver có `availability_status = AVAILABLE` trong bán kính 5 km.
 2. Hệ thống lọc tiếp theo loại xe Customer chọn.
 3. Nếu Customer bật ưu tiên, hệ thống xếp tài xế có đánh giá cao hơn; sau đó ưu tiên khoảng cách gần hơn.
 4. Hệ thống gửi yêu cầu cho tài xế phù hợp; mỗi yêu cầu có thời hạn 30 giây.
 5. Driver chọn nhận chuyến; hệ thống khóa chuyến và cập nhật Driver sang trạng thái bận.
 6. Hệ thống thông báo thông tin Driver cho Customer và chuyển sang UC03.

 **Luồng thay thế/ngoại lệ:**

 - B1: Driver từ chối hoặc không phản hồi trong 30 giây: ghi nhận `DECLINED`/`EXPIRED`, gửi Driver tiếp theo.
 - B2: Driver mất trạng thái `AVAILABLE` trước khi nhận: bỏ qua Driver và tìm lại.
 - B3: Không còn Driver phù hợp: tiếp tục tìm đến tối đa 3 phút, sau đó báo Customer và đóng ở `NO_DRIVER`.
 - B4: Hai yêu cầu cùng tranh một chuyến: chỉ yêu cầu đầu tiên hợp lệ được chấp nhận, yêu cầu còn lại nhận lỗi đã có tài xế.

 ### 11.6. Đặc tả UC03 - Theo dõi chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC03 - Theo dõi chuyến |
 | Actor chính | A01 Customer, A02 Driver |
 | Tiền điều kiện | Có `Ride` thuộc về actor và actor đã được phân quyền xem. |
 | Luồng chính | Hiển thị tài xế/khách, biển số, điểm đón, điểm đến, trạng thái và vị trí cập nhật. |
 | Hậu điều kiện | Trạng thái hiển thị khớp dữ liệu mới nhất trên máy chủ. |

 Các trạng thái hợp lệ: `SEARCHING_DRIVER` -> `DRIVER_ASSIGNED` -> `DRIVER_ARRIVING` -> `IN_PROGRESS` -> `COMPLETED`. Từ `SEARCHING_DRIVER` hoặc `DRIVER_ASSIGNED` có thể chuyển sang `CANCELLED` theo UC06.

 ### 11.7. Đặc tả UC04 - Thực hiện và hoàn tất chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC04 - Thực hiện và hoàn tất chuyến |
 | Actor chính | A02 Driver |
 | Tiền điều kiện | Driver đã được gán cho Ride. |
 | Luồng chính | Driver báo đã đến điểm đón; bắt đầu chuyến khi đón khách; di chuyển; chọn hoàn tất khi đến điểm đến. |
 | Hậu điều kiện | Lưu quãng đường/thời gian, tính giá cuối và chuyển sang UC05. |
 | Ngoại lệ | Chỉ Driver được gán mới được đổi trạng thái; không cho hoàn tất khi chưa bắt đầu. |

 ### 11.8. Đặc tả UC05 - Thanh toán chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC05 - Thanh toán chuyến |
 | Actor chính | A01 Customer |
 | Actor phụ | A05 Payment gateway |
 | Tiền điều kiện | UC04 đã hoàn tất và hệ thống có giá cuối. |
 | Luồng tiền mặt | Customer chọn tiền mặt; hệ thống ghi `CASH_PENDING`; Driver xác nhận đã thu tiền; chuyển `PAID`. |
 | Luồng trực tuyến | Hệ thống tạo giao dịch; gateway trả kết quả; callback hợp lệ chuyển giao dịch sang `PAID`. |
 | Ngoại lệ | Giao dịch thất bại thì ghi `FAILED`, thông báo Customer và cho phép thử lại hoặc chọn tiền mặt theo chính sách. |

 ### 11.9. Đặc tả UC06 - Hủy chuyến

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC06 - Hủy chuyến |
 | Actor chính | Customer hoặc Driver |
 | Tiền điều kiện | Chuyến chưa ở `COMPLETED` và người yêu cầu có quyền hủy. |
 | Luồng chính | Actor chọn hủy, chọn lý do; hệ thống kiểm tra trạng thái, tính phí nếu có, thông báo bên còn lại và chuyển `CANCELLED`. |
 | Ngoại lệ | Không cho hủy chuyến đã hoàn tất; nếu Driver hủy thì quay lại UC02 tìm Driver khác trước khi đóng chuyến. |

 ### 11.10. Đặc tả UC07 - Đánh giá tài xế

 | Thuộc tính | Nội dung |
 |---|---|
 | Mã/tên | UC07 - Đánh giá tài xế |
 | Actor chính | A01 Customer |
 | Tiền điều kiện | Ride ở `COMPLETED`; Customer là người đặt chuyến; chưa đánh giá Ride này. |
 | Luồng chính | Customer chọn 1-5 sao, nhập phản hồi tùy chọn; hệ thống kiểm tra và lưu Rating, sau đó cập nhật điểm trung bình Driver. |
 | Ngoại lệ | Không cho đánh giá nhiều lần, điểm ngoài 1-5 hoặc đánh giá chuyến không thuộc Customer. |

 ### 11.11. Truy vết liên tục BR - FR - UC

 | Business requirement | Functional requirements liên quan | Use case thực hiện |
 |---|---|---|
 | BR01 Đặt chuyến | FR01-FR04 | UC01, UC03 |
 | BR02 Tìm và phân công tài xế | FR05-FR09 | UC02 |
 | BR03 Quản lý trạng thái chuyến | FR10-FR11 | UC03, UC04, UC06 |
 | BR04 Thanh toán chuyến | FR12-FR13 | UC04, UC05 |
 | BR05 Đánh giá và phản hồi | FR14-FR15 | UC07, UC09 |

 ## 13. Acceptance criteria (B13)

 Acceptance criteria là các điều kiện nghiệm thu cụ thể để BA, khách hàng và đội phát triển thống nhất khi nào một yêu cầu được xem là hoàn thành. Mỗi tiêu chí được mã hóa `AC`; một BR chỉ được nghiệm thu khi các AC liên quan đạt kết quả **Pass**.

 ### 13.1. Nguyên tắc nghiệm thu

 - Kiểm thử trên môi trường được doanh nghiệp phê duyệt với dữ liệu chuyến hợp lệ và dữ liệu ngoại lệ.
 - Kiểm tra cả luồng thành công và luồng thất bại, đặc biệt là hết thời gian nhận chuyến, hủy chuyến và thanh toán lỗi.
 - Kết quả phải có bằng chứng: mã chuyến, ảnh/log hoặc kết quả kiểm thử, người thực hiện và ngày kiểm thử.
 - Không được nghiệm thu nếu còn lỗi nghiêm trọng làm mất chuyến, tạo giao dịch trùng hoặc lộ dữ liệu người dùng.

 ### 13.2. Acceptance cases

 | Mã AC | Liên quan | Acceptance case | Điều kiện Given / When / Then | Kết quả nghiệm thu |
 |---|---|---|---|---|
 | AC01 | BR01, FR01-FR04, UC01 | Tạo chuyến hợp lệ | Given Customer đã đăng nhập và nhập điểm đón, điểm đến hợp lệ; When chọn `BIKE` hoặc `CAR` và xác nhận; Then hệ thống hiển thị giá dự kiến và tạo đúng một Ride ở `SEARCHING_DRIVER`. | Pass khi thông tin chuyến được lưu và không tạo bản ghi trùng. |
 | AC02 | BR01, FR01-FR02, UC01 | Từ chối địa điểm không hợp lệ | Given điểm đón và điểm đến trùng hoặc không định vị được; When Customer xác nhận; Then hệ thống hiển thị lỗi và không tạo Ride. | Pass khi không có chuyến hợp lệ được tạo. |
 | AC03 | BR02, FR05-FR07, UC02 | Tìm đúng tài xế | Given có Driver `AVAILABLE`, đúng loại xe và trong bán kính 5 km; When Ride được tạo; Then hệ thống gửi yêu cầu cho Driver phù hợp. | Pass khi tài xế ngoài bán kính, sai loại xe hoặc không sẵn sàng không nhận được yêu cầu. |
 | AC04 | BR02, FR08, UC02 | Ưu tiên tài xế theo yêu cầu Customer | Given Customer bật ưu tiên đánh giá; When có nhiều Driver phù hợp; Then hệ thống xếp Driver có điểm đánh giá cao hơn trước, sau đó xét khoảng cách. | Pass khi thứ tự phân tài xế đúng chính sách. |
 | AC05 | BR02, FR09, UC02 | Tài xế nhận chuyến | Given Driver nhận yêu cầu trong 30 giây; When hệ thống xử lý phản hồi; Then Ride chuyển sang `DRIVER_ASSIGNED` và Driver chuyển sang trạng thái bận. | Pass khi chỉ một Driver được gán cho Ride. |
 | AC06 | BR02, BRL04, UC02 | Tài xế từ chối hoặc hết hạn | Given Driver từ chối hoặc không phản hồi 30 giây; When bộ đếm hết hạn; Then hệ thống ghi `DECLINED`/`EXPIRED` và gửi Driver kế tiếp. | Pass khi không chờ vô hạn và lịch sử request được lưu. |
 | AC07 | BR03, FR10-FR11, UC03 | Cập nhật trạng thái chuyến | Given Ride đã được gán; When Driver đến điểm đón, bắt đầu và hoàn tất chuyến; Then trạng thái lần lượt là `DRIVER_ARRIVING`, `IN_PROGRESS`, `COMPLETED`. | Pass khi chỉ các chuyển trạng thái hợp lệ được chấp nhận. |
 | AC08 | BR03, UC03 | Theo dõi vị trí và thông tin | Given Customer có Ride đang hoạt động; When Driver cập nhật vị trí; Then Customer xem được trạng thái, thông tin Driver và vị trí mới nhất. | Pass khi người không liên quan không xem được dữ liệu chuyến. |
 | AC09 | BR04, FR12, UC05 | Thanh toán tiền mặt | Given Ride đã `COMPLETED` và Customer chọn tiền mặt; When Driver xác nhận đã thu đủ tiền; Then Payment chuyển từ `CASH_PENDING` sang `PAID`. | Pass khi số tiền thanh toán bằng giá cuối của chuyến. |
 | AC10 | BR04, FR13, UC05 | Thanh toán trực tuyến thành công | Given Ride đã hoàn tất; When gateway trả callback thành công đúng giao dịch; Then Payment chuyển sang `PAID` và lưu mã giao dịch đối tác. | Pass khi callback lặp lại không tạo giao dịch thứ hai. |
 | AC11 | BR04, FR13, UC05 | Thanh toán trực tuyến thất bại | Given gateway trả kết quả thất bại; When hệ thống nhận callback; Then Payment là `FAILED`, Customer được thông báo và có thể thử lại hoặc chọn tiền mặt. | Pass khi Ride không bị đánh dấu đã thanh toán. |
 | AC12 | BR03, BRL09, UC06 | Hủy chuyến đúng điều kiện | Given Ride chưa `COMPLETED`; When Customer hoặc Driver hủy và chọn lý do; Then hệ thống lưu lý do, tính phí theo chính sách và thông báo bên còn lại. | Pass khi không thể hủy Ride đã hoàn tất. |
 | AC13 | BR05, FR14-FR15, UC07 | Đánh giá sau chuyến | Given Ride `COMPLETED`; When Customer gửi điểm 1-5 và phản hồi; Then hệ thống lưu Rating và cập nhật điểm Driver. | Pass khi không thể đánh giá chuyến không thuộc Customer hoặc đánh giá lần hai. |
 | AC14 | BR01-BR05, NFR07, UC01-UC05 | Không tạo dữ liệu trùng | Given Customer bấm xác nhận hoặc callback nhiều lần; When hệ thống xử lý cùng một mã yêu cầu; Then chỉ có một Ride và một Payment hợp lệ. | Pass khi kiểm tra khóa duy nhất và tính lũy thừa thành công. |
 | AC15 | NFR01-NFR06, NFR09 | Đáp ứng chất lượng hệ thống | Given hệ thống chạy theo kịch bản tải và bảo mật được phê duyệt; When đo hiệu năng, phân quyền và khả dụng; Then kết quả đạt các ngưỡng NFR đã công bố. | Pass khi toàn bộ NFR bắt buộc đạt và có báo cáo kiểm thử. |

 ### 13.3. Ma trận ưu tiên yêu cầu

 Việc ưu tiên dùng hai tiêu chí **giá trị đối với nghiệp vụ** và **mức khẩn cấp**. BR01 và BR02 là lõi của CAB SYSTEM nên phải làm trước; BR04 là chức năng quan trọng tiếp theo vì liên quan doanh thu.

 | Nhóm | Mã | Ưu tiên | Lý do |
 |---|---|---|---|
 | Must have | BR01, BR02, BR03 | P1 | Không có các chức năng này thì không thể đặt và thực hiện chuyến. |
 | Must have | BR04 | P1 | Cần hoàn tất doanh thu và hỗ trợ tiền mặt/trực tuyến. |
 | Should have | BR05 | P2 | Cải thiện chất lượng dịch vụ nhưng không ngăn việc hoàn thành chuyến. |
 | Could have | Báo cáo nâng cao, khuyến mãi | P3 | Có thể triển khai sau giai đoạn 1. |

 ## 14. Requirements traceability matrix (B14)

 Requirements Traceability Matrix (`RTM`) là ma trận truy xuất nguồn gốc yêu cầu. RTM theo dõi một nhu cầu từ mục tiêu kinh doanh, qua BR, FR, UC, đến tiêu chí nghiệm thu AC. Nhờ đó BA có thể xác định yêu cầu bắt đầu từ đâu, được thiết kế trong chức năng nào, được thực hiện ở use case nào và kiểm thử bằng case nào.

 ### 14.1. Quy trình truy xuất yêu cầu

 1. **Khởi tạo:** ghi nhận BG từ vấn đề và mục tiêu của doanh nghiệp.
 2. **Phân tích:** chuyển BG thành BR, xác định stakeholder và phạm vi.
 3. **Thiết kế:** phân rã BR thành FR, business process, rule và UC.
 4. **Phát triển:** dùng FR và UC làm đầu vào cho thiết kế kỹ thuật, API và data model.
 5. **Kiểm thử:** dùng AC để viết test case và kiểm tra cả luồng chính/ngoại lệ.
 6. **Nghiệm thu:** chỉ đóng BR khi các AC liên quan đạt Pass và có bằng chứng kiểm thử.
 7. **Thay đổi:** khi BG, BR hoặc FR thay đổi, cập nhật các UC, AC và dòng RTM liên quan.

 ### 14.2. RTM của CAB SYSTEM

 | Business goals | BR | FR | UC | AC |
 |---|---|---|---|---|
 | BG01 Đặt chuyến nhanh, tin cậy | BR01 Đặt chuyến | FR01-FR04 | UC01 Đặt chuyến | AC01, AC02, AC14 |
 | BG01 Đặt chuyến nhanh, tin cậy | BR02 Tìm và phân công tài xế | FR05-FR09 | UC02 Tìm và nhận chuyến | AC03, AC04, AC05, AC06 |
 | BG01 Đặt chuyến nhanh, tin cậy | BR03 Quản lý trạng thái chuyến | FR10-FR11 | UC03 Theo dõi chuyến; UC04 Thực hiện và hoàn tất chuyến; UC06 Hủy chuyến | AC07, AC08, AC12 |
 | BG02 Thanh toán linh hoạt, minh bạch | BR04 Thanh toán chuyến đi | FR12-FR13 | UC05 Thanh toán chuyến | AC09, AC10, AC11, AC14 |
 | BG03 Khai thác tài xế hiệu quả | BR02 Tìm và phân công tài xế | FR05-FR09 | UC02 Tìm và nhận chuyến | AC03, AC04, AC05, AC06 |
 | BG04 An toàn và chất lượng dịch vụ | BR03 Quản lý trạng thái chuyến | FR10-FR11 | UC03 Theo dõi chuyến; UC06 Hủy chuyến; UC09 Hỗ trợ và xử lý sự cố | AC07, AC08, AC12 |
 | BG04 An toàn và chất lượng dịch vụ | BR05 Đánh giá và phản hồi | FR14-FR15 | UC07 Đánh giá tài xế; UC09 Hỗ trợ và xử lý sự cố | AC13 |
 | BG01-BG04 | BR01-BR05 | NFR01-NFR12 | UC01-UC09 | AC14, AC15 |

 ### 14.3. Quy tắc kiểm soát RTM

 - Mỗi BR phải có ít nhất một FR, một UC và một AC; không được có BR mồ côi.
 - Mỗi FR phải xuất hiện trong ít nhất một UC hoặc được đánh dấu là yêu cầu kỹ thuật hỗ trợ.
 - Mỗi AC phải chỉ rõ yêu cầu cần nghiệm thu và kết quả Pass/Fail.
 - NFR01-NFR12 áp dụng xuyên suốt các UC; khi kiểm thử phải ghi nhận NFR liên quan trong test report.
 - Khi một yêu cầu thay đổi, BA phải rà lại toàn bộ dòng RTM và cập nhật impact analysis trước khi phát triển.
