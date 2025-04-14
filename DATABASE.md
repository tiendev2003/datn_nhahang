# Thiết kế Cơ sở dữ liệu cho Hệ thống Quản lý Nhà hàng

## Tổng quan

Tài liệu này mô tả cấu trúc cơ sở dữ liệu cho hệ thống quản lý và đặt món cho nhà hàng. Hệ thống sử dụng PostgreSQL làm hệ quản trị cơ sở dữ liệu và SQLAlchemy ORM để tương tác với cơ sở dữ liệu từ ứng dụng Python Flask.

## Sơ đồ Quan hệ Thực thể (ERD)

```
┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│      Users        │     │      Orders       │     │    OrderItems     │
├───────────────────┤     ├───────────────────┤     ├───────────────────┤
│ id (PK)           │     │ id (PK)           │     │ id (PK)           │
│ username          │◄────┤ user_id (FK)      │     │ order_id (FK)     │
│ email             │     │ table_id (FK)     │◄────┤ menu_item_id (FK) │
│ password_hash     │     │ status            │     │ quantity          │
│ role              │     │ total_amount      │     │ subtotal          │
│ first_name        │     │ created_at        │     │ notes             │
│ last_name         │     │ updated_at        │     │ status            │
│ phone             │     │ payment_method    │     └───────────────────┘
│ created_at        │     └───────────────────┘             ▲
└───────────────────┘                 ▲                     │
                                      │                     │
┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│      Tables       │     │   Reservations    │     │    MenuItems      │
├───────────────────┤     ├───────────────────┤     ├───────────────────┤
│ id (PK)           │     │ id (PK)           │     │ id (PK)           │
│ table_number      │◄────┤ table_id (FK)     │     │ name              │
│ capacity          │     │ user_id (FK)      │     │ description       │
│ status            │     │ reservation_time  │     │ price             │
│ location          │     │ duration          │     │ category_id (FK)  │◄───┐
└───────────────────┘     │ status            │     │ image_url         │    │
                          │ special_request   │     │ is_available      │    │
┌───────────────────┐     │ created_at        │     │ preparation_time  │    │
│    Employees      │     └───────────────────┘     └───────────────────┘    │
├───────────────────┤                                                        │
│ id (PK)           │     ┌───────────────────┐     ┌───────────────────┐    │
│ user_id (FK)      │     │     Inventory     │     │    Categories     │    │
│ position          │     ├───────────────────┤     ├───────────────────┤    │
│ salary            │     │ id (PK)           │     │ id (PK)           │────┘
│ hire_date         │     │ item_name         │     │ name              │
│ schedule          │     │ quantity          │     │ description       │
│ is_active         │     │ unit              │     │ image_url         │
└───────────────────┘     │ threshold         │     └───────────────────┘
                          │ last_updated       │
┌───────────────────┐     └───────────────────┘     ┌───────────────────┐
│     Payments      │                                │    Promotions     │
├───────────────────┤     ┌───────────────────┐     ├───────────────────┤
│ id (PK)           │     │    Suppliers      │     │ id (PK)           │
│ order_id (FK)     │     ├───────────────────┤     │ name              │
│ amount            │     │ id (PK)           │     │ description       │
│ payment_method    │     │ name              │     │ discount_percent  │
│ status            │     │ contact_name      │     │ start_date        │
│ transaction_id    │     │ phone             │     │ end_date          │
│ created_at        │     │ email             │     │ is_active         │
└───────────────────┘     │ address           │     │ min_order_amount  │
                          │ notes             │     └───────────────────┘
┌───────────────────┐     └───────────────────┘
│    Feedback       │                                ┌───────────────────┐
├───────────────────┤     ┌───────────────────┐     │ InventoryUsage    │
│ id (PK)           │     │  SupplyOrders     │     ├───────────────────┤
│ user_id (FK)      │     ├───────────────────┤     │ id (PK)           │
│ order_id (FK)     │     │ id (PK)           │     │ menu_item_id (FK) │
│ rating            │     │ supplier_id (FK)  │     │ inventory_id (FK) │
│ comment           │     │ item_id (FK)      │     │ quantity_used     │
│ created_at        │     │ quantity          │     └───────────────────┘
└───────────────────┘     │ order_date        │
                          │ expected_delivery │
                          │ status            │
                          │ total_cost        │
                          └───────────────────┘
```

## Chi tiết Bảng

### 1. Users
Lưu thông tin về tất cả người dùng trong hệ thống, bao gồm khách hàng, nhân viên và quản trị viên.

| Tên cột      | Kiểu dữ liệu | Mô tả                                |
|--------------|--------------|--------------------------------------|
| id           | SERIAL       | Khóa chính, ID tự động tăng          |
| username     | VARCHAR(50)  | Tên đăng nhập, duy nhất              |
| email        | VARCHAR(100) | Địa chỉ email, duy nhất              |
| password_hash| VARCHAR(255) | Mật khẩu đã được băm                 |
| role         | VARCHAR(20)  | Vai trò (admin, staff, customer)     |
| first_name   | VARCHAR(50)  | Tên                                  |
| last_name    | VARCHAR(50)  | Họ                                   |
| phone        | VARCHAR(20)  | Số điện thoại liên hệ                |
| created_at   | TIMESTAMP    | Thời điểm tài khoản được tạo         |

### 2. Employees
Lưu thông tin chi tiết về nhân viên nhà hàng.

| Tên cột      | Kiểu dữ liệu | Mô tả                                |
|--------------|--------------|--------------------------------------|
| id           | SERIAL       | Khóa chính, ID tự động tăng          |
| user_id      | INTEGER      | Khóa ngoại tới bảng Users            |
| position     | VARCHAR(50)  | Vị trí công việc                     |
| salary       | DECIMAL      | Mức lương                            |
| hire_date    | DATE         | Ngày tuyển dụng                      |
| schedule     | JSONB        | Lịch làm việc (dạng JSON)            |
| is_active    | BOOLEAN      | Trạng thái làm việc                  |

### 3. Tables
Lưu thông tin về các bàn trong nhà hàng.

| Tên cột      | Kiểu dữ liệu | Mô tả                                |
|--------------|--------------|--------------------------------------|
| id           | SERIAL       | Khóa chính, ID tự động tăng          |
| table_number | INTEGER      | Số bàn                               |
| capacity     | INTEGER      | Số lượng người tối đa                |
| status       | VARCHAR(20)  | Trạng thái (available, occupied)     |
| location     | VARCHAR(50)  | Vị trí trong nhà hàng                |

### 4. Reservations
Lưu thông tin đặt bàn của khách hàng.

| Tên cột         | Kiểu dữ liệu | Mô tả                                |
|-----------------|--------------|--------------------------------------|
| id              | SERIAL       | Khóa chính, ID tự động tăng          |
| table_id        | INTEGER      | Khóa ngoại tới bảng Tables           |
| user_id         | INTEGER      | Khóa ngoại tới bảng Users            |
| reservation_time| TIMESTAMP    | Thời gian đặt bàn                    |
| duration        | INTEGER      | Thời lượng (phút)                    |
| status          | VARCHAR(20)  | Trạng thái (pending, confirmed)      |
| special_request | TEXT         | Yêu cầu đặc biệt                     |
| created_at      | TIMESTAMP    | Thời điểm tạo đặt bàn                |

### 5. Categories
Phân loại các món ăn.

| Tên cột     | Kiểu dữ liệu | Mô tả                                |
|-------------|--------------|--------------------------------------|
| id          | SERIAL       | Khóa chính, ID tự động tăng          |
| name        | VARCHAR(50)  | Tên danh mục                         |
| description | TEXT         | Mô tả                                |
| image_url   | VARCHAR(255) | Đường dẫn hình ảnh                   |

### 6. MenuItems
Lưu thông tin về các món ăn trong thực đơn.

| Tên cột        | Kiểu dữ liệu | Mô tả                                |
|----------------|--------------|--------------------------------------|
| id             | SERIAL       | Khóa chính, ID tự động tăng          |
| name           | VARCHAR(100) | Tên món ăn                           |
| description    | TEXT         | Mô tả                                |
| price          | DECIMAL      | Giá                                  |
| category_id    | INTEGER      | Khóa ngoại tới bảng Categories       |
| image_url      | VARCHAR(255) | Đường dẫn hình ảnh                   |
| is_available   | BOOLEAN      | Tình trạng còn món hay không         |
| preparation_time| INTEGER      | Thời gian chuẩn bị (phút)           |

### 7. Orders
Lưu thông tin đơn hàng.

| Tên cột       | Kiểu dữ liệu | Mô tả                                |
|---------------|--------------|--------------------------------------|
| id            | SERIAL       | Khóa chính, ID tự động tăng          |
| user_id       | INTEGER      | Khóa ngoại tới bảng Users            |
| table_id      | INTEGER      | Khóa ngoại tới bảng Tables           |
| status        | VARCHAR(20)  | Trạng thái đơn hàng                  |
| total_amount  | DECIMAL      | Tổng tiền                            |
| created_at    | TIMESTAMP    | Thời điểm tạo đơn                    |
| updated_at    | TIMESTAMP    | Thời điểm cập nhật đơn               |
| payment_method| VARCHAR(50)  | Phương thức thanh toán               |

### 8. OrderItems
Chi tiết các món trong đơn hàng.

| Tên cột     | Kiểu dữ liệu | Mô tả                                |
|-------------|--------------|--------------------------------------|
| id          | SERIAL       | Khóa chính, ID tự động tăng          |
| order_id    | INTEGER      | Khóa ngoại tới bảng Orders           |
| menu_item_id| INTEGER      | Khóa ngoại tới bảng MenuItems        |
| quantity    | INTEGER      | Số lượng                             |
| subtotal    | DECIMAL      | Thành tiền                           |
| notes       | TEXT         | Ghi chú đặc biệt                     |
| status      | VARCHAR(20)  | Trạng thái (preparing, served)       |

### 9. Payments
Lưu thông tin thanh toán.

| Tên cột       | Kiểu dữ liệu | Mô tả                                |
|---------------|--------------|--------------------------------------|
| id            | SERIAL       | Khóa chính, ID tự động tăng          |
| order_id      | INTEGER      | Khóa ngoại tới bảng Orders           |
| amount        | DECIMAL      | Số tiền thanh toán                   |
| payment_method| VARCHAR(50)  | Phương thức thanh toán               |
| status        | VARCHAR(20)  | Trạng thái thanh toán                |
| transaction_id| VARCHAR(100) | ID giao dịch                         |
| created_at    | TIMESTAMP    | Thời điểm thanh toán                 |

### 10. Feedback
Lưu thông tin phản hồi từ khách hàng.

| Tên cột   | Kiểu dữ liệu | Mô tả                                |
|-----------|--------------|--------------------------------------|
| id        | SERIAL       | Khóa chính, ID tự động tăng          |
| user_id   | INTEGER      | Khóa ngoại tới bảng Users            |
| order_id  | INTEGER      | Khóa ngoại tới bảng Orders           |
| rating    | INTEGER      | Đánh giá (1-5 sao)                   |
| comment   | TEXT         | Nhận xét                             |
| created_at| TIMESTAMP    | Thời điểm gửi đánh giá               |

### 11. Inventory
Lưu thông tin về kho nguyên liệu.

| Tên cột     | Kiểu dữ liệu | Mô tả                                |
|-------------|--------------|--------------------------------------|
| id          | SERIAL       | Khóa chính, ID tự động tăng          |
| item_name   | VARCHAR(100) | Tên nguyên liệu                      |
| quantity    | DECIMAL      | Số lượng tồn kho                     |
| unit        | VARCHAR(20)  | Đơn vị (kg, g, lít, ...)             |
| threshold   | DECIMAL      | Ngưỡng cảnh báo hết hàng             |
| last_updated| TIMESTAMP    | Thời điểm cập nhật cuối              |

### 12. Suppliers
Lưu thông tin về nhà cung cấp nguyên liệu.

| Tên cột     | Kiểu dữ liệu | Mô tả                                |
|-------------|--------------|--------------------------------------|
| id          | SERIAL       | Khóa chính, ID tự động tăng          |
| name        | VARCHAR(100) | Tên nhà cung cấp                     |
| contact_name| VARCHAR(100) | Tên người liên hệ                    |
| phone       | VARCHAR(20)  | Số điện thoại                        |
| email       | VARCHAR(100) | Email                                |
| address     | TEXT         | Địa chỉ                              |
| notes       | TEXT         | Ghi chú                              |

### 13. SupplyOrders
Lưu thông tin đặt hàng nguyên liệu từ nhà cung cấp.

| Tên cột          | Kiểu dữ liệu | Mô tả                                |
|------------------|--------------|--------------------------------------|
| id               | SERIAL       | Khóa chính, ID tự động tăng          |
| supplier_id      | INTEGER      | Khóa ngoại tới bảng Suppliers        |
| item_id          | INTEGER      | Khóa ngoại tới bảng Inventory        |
| quantity         | DECIMAL      | Số lượng đặt                         |
| order_date       | DATE         | Ngày đặt hàng                        |
| expected_delivery| DATE         | Ngày dự kiến giao hàng               |
| status           | VARCHAR(20)  | Trạng thái                           |
| total_cost       | DECIMAL      | Chi phí                              |

### 14. Promotions
Lưu thông tin về các chương trình khuyến mãi.

| Tên cột         | Kiểu dữ liệu | Mô tả                                |
|-----------------|--------------|--------------------------------------|
| id              | SERIAL       | Khóa chính, ID tự động tăng          |
| name            | VARCHAR(100) | Tên chương trình                     |
| description     | TEXT         | Mô tả                                |
| discount_percent| INTEGER      | Phần trăm giảm giá                   |
| start_date      | DATE         | Ngày bắt đầu                         |
| end_date        | DATE         | Ngày kết thúc                        |
| is_active       | BOOLEAN      | Trạng thái kích hoạt                 |
| min_order_amount| DECIMAL      | Số tiền tối thiểu để áp dụng         |

### 15. InventoryUsage
Liên kết giữa món ăn và nguyên liệu sử dụng.

| Tên cột      | Kiểu dữ liệu | Mô tả                                |
|--------------|--------------|--------------------------------------|
| id           | SERIAL       | Khóa chính, ID tự động tăng          |
| menu_item_id | INTEGER      | Khóa ngoại tới bảng MenuItems        |
| inventory_id | INTEGER      | Khóa ngoại tới bảng Inventory        |
| quantity_used| DECIMAL      | Số lượng nguyên liệu sử dụng         |

## Các Ràng buộc và Chỉ mục

### Chỉ mục (Indexes)
1. Chỉ mục trên các khóa ngoại để tối ưu hiệu suất truy vấn
2. Chỉ mục tìm kiếm trên các trường thường được sử dụng để tìm kiếm như tên món ăn, username, email
3. Chỉ mục thời gian trên các trường ngày tháng để tối ưu các truy vấn theo khoảng thời gian

### Ràng buộc (Constraints)
1. Khóa chính trên mỗi bảng
2. Khóa ngoại với tính toàn vẹn referential
3. Ràng buộc CHECK trên các trường như rating (1-5), discount_percent (0-100)
4. Unique constraints trên các trường như username, email, table_number

## Migrations và Seeding

### Database Migrations
Sử dụng Alembic (tích hợp với Flask-Migrate) để quản lý phiên bản cơ sở dữ liệu:
```bash
# Khởi tạo migration repository
flask db init

# Tạo migration mới
flask db migrate -m "Khởi tạo database"

# Áp dụng migration
flask db upgrade
```

### Seeding (Dữ liệu mẫu)
Tạo script Python để thêm dữ liệu mẫu vào cơ sở dữ liệu:
```python
from app import db
from app.models import User, Category, MenuItem, Table
import datetime

# Thêm dữ liệu mẫu cho người dùng
admin = User(username='admin', email='admin@example.com', role='admin')
admin.set_password('admin123')
db.session.add(admin)

# Thêm dữ liệu mẫu cho danh mục
categories = [
    Category(name='Khai vị', description='Các món khai vị'),
    Category(name='Món chính', description='Các món chính'),
    # ...
]
db.session.add_all(categories)

# Thêm dữ liệu mẫu cho bàn ăn
tables = [
    Table(table_number=1, capacity=2, status='available'),
    Table(table_number=2, capacity=4, status='available'),
    # ...
]
db.session.add_all(tables)

db.session.commit()
```

## Tối ưu hóa hiệu suất

1. Sử dụng chỉ mục hợp lý trên các trường thường xuyên truy vấn
2. Áp dụng eager loading khi cần thiết để tránh vấn đề N+1 query
3. Sử dụng các transaction khi thực hiện nhiều thay đổi liên quan
4. Cân nhắc sử dụng Redis để cache các truy vấn phổ biến
5. Thiết kế query tối ưu, tránh join nhiều bảng không cần thiết