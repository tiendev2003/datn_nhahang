# HỆ THỐNG HỖ TRỢ QUẢN LÝ VÀ ĐẶT MÓN CHO NHÀ HÀNG

![Restaurant Management System](https://via.placeholder.com/800x400?text=Restaurant+Management+System)

## Giới thiệu

Đây là hệ thống hỗ trợ quản lý và đặt món cho nhà hàng, được phát triển nhằm cải thiện quy trình làm việc tại nhà hàng, mang lại trải nghiệm tốt hơn cho khách hàng và nâng cao hiệu quả quản lý.

Hệ thống được thiết kế với giao diện thân thiện, dễ sử dụng và tích hợp đầy đủ các chức năng cần thiết cho việc quản lý nhà hàng từ A-Z.

## Công nghệ sử dụng (Tech Stack)

### Front-end
- ReactJS
- HTML5/CSS3
- JavaScript/ES6+
- Redux (quản lý state)
- Axios (gọi API)
- Shadcn ui
- WebSocket (cập nhật dữ liệu thời gian thực)
- Progressive Web App (PWA)

### Back-end
- Python
- Flask Framework
- RESTful API
- JWT Authentication
- Bcrypt (mã hóa mật khẩu)
- WebSocket (giao tiếp thời gian thực)
 
### Database
- PostgreSQL
- SQLAlchemy ORM

### DevOps & Tools
- Git/GitHub (quản lý phiên bản)
- Docker (container hóa)
- Nginx (web server)
- AWS/Google Cloud (hosting)

## Tính năng

### 1. Chức năng phía khách hàng

#### Trải nghiệm Progressive Web App (PWA)
- Sử dụng ứng dụng khi không có kết nối internet (offline mode)
- Cài đặt ứng dụng trên màn hình chính của thiết bị di động
- Nhận thông báo đẩy (push notifications) về trạng thái đơn hàng, khuyến mãi
- Trải nghiệm gần giống với ứng dụng native trên mọi thiết bị
- Tối ưu tốc độ tải trang và hiệu suất sử dụng

#### Quản lý tài khoản
- Đăng ký tài khoản mới
- Đăng nhập/Đăng xuất
- Xem và chỉnh sửa thông tin cá nhân

#### Thông tin nhà hàng
- Xem thông tin giới thiệu
- Xem lịch sử phát triển
- Xem menu, giá cả
- Xem các khuyến mãi hiện tại

#### Đặt bàn và gọi món
- Đặt bàn trực tuyến
- Đặt món trước khi đến
- Theo dõi trạng thái đơn hàng
- Gọi món bổ sung trong quá trình ăn

#### Tương tác trong nhà hàng
- Gọi nhân viên phục vụ
- Yêu cầu thanh toán
- Gửi đánh giá và phản hồi
- Gửi góp ý về chất lượng dịch vụ

### 2. Chức năng phía quản lý và nhân viên nhà hàng

#### Chủ/Quản lý nhà hàng

##### Quản lý nhân viên
- Thêm/xóa/sửa thông tin nhân viên
- Upload ảnh nhân viên
- Phân quyền tài khoản theo loại nhân viên
- Theo dõi lịch làm việc và hiệu suất

##### Quản lý thực đơn (Menu)
- Thêm/xóa/sửa món ăn
- Upload ảnh món ăn
- Phân loại món ăn
- Cập nhật giá cả

##### Quản lý khuyến mãi
- Tạo chương trình khuyến mãi mới
- Thiết lập thời gian và điều kiện áp dụng
- Theo dõi hiệu quả chương trình

##### Quản lý bàn ăn
- Thêm/xóa/sửa thông tin bàn
- Theo dõi trạng thái các bàn
- Sắp xếp và bố trí bàn

##### Quản lý kho nguyên liệu
- Thêm/xóa/sửa thông tin nguyên liệu
- Theo dõi số lượng tồn kho
- Cảnh báo khi nguyên liệu sắp hết
- Lập kế hoạch mua sắm

##### Quản lý doanh thu và báo cáo
- Thống kê doanh thu theo ngày/tháng/năm
- Báo cáo chi tiết về các món bán chạy
- Phân tích xu hướng kinh doanh
- Xuất báo cáo tài chính

##### Quản lý nhà cung cấp nguyên liệu
- Thêm/xóa/sửa thông tin nhà cung cấp
- Theo dõi lịch sử mua hàng
- Đánh giá chất lượng nhà cung cấp

#### Nhân viên phục vụ

##### Quản lý đặt bàn
- Xác nhận đặt bàn
- Hủy/thay đổi thông tin đặt bàn
- Sắp xếp bàn theo yêu cầu

##### Quản lý đơn hàng
- Gửi yêu cầu chế biến cho bếp trưởng
- Theo dõi tiến trình chế biến
- Cập nhật trạng thái đơn hàng

##### Quản lý bàn
- Chuyển bàn/Gộp bàn
- Kiểm tra trạng thái bàn
- Phản hồi yêu cầu của khách hàng

##### Thanh toán
- Gửi hóa đơn tạm tính cho thu ngân
- Thông báo khi khách yêu cầu thanh toán

#### Bếp trưởng

##### Quản lý chế biến
- Nhận danh sách món cần nấu
- Phân công nhiệm vụ cho nhân viên bếp
- Cập nhật tiến trình chế biến

##### Quản lý nguyên liệu
- Theo dõi nguyên liệu tồn kho
- Yêu cầu bổ sung nguyên liệu
- Cảnh báo khi nguyên liệu sắp hết

#### Thu ngân

##### Quản lý thanh toán
- Xác nhận thanh toán
- Xử lý các hình thức thanh toán khác nhau
- Xuất hóa đơn
- Xử lý hoàn tiền

## Cài đặt và Chạy ứng dụng

### Yêu cầu hệ thống
- Node.js (phiên bản 16.x hoặc cao hơn)
- Python (phiên bản 3.8 hoặc cao hơn)
- PostgreSQL (phiên bản 13 hoặc cao hơn)
- Docker (tùy chọn)

### Cài đặt

1. Clone repository từ GitHub:
```bash
git clone https://github.com/yourusername/datn_nhahang.git
cd datn_nhahang
```

2. Cài đặt dependencies cho Front-end:
```bash
cd frontend
npm install
# Cài đặt thêm các thư viện PWA
npm install workbox-webpack-plugin workbox-core workbox-routing workbox-strategies
```

3. Cài đặt dependencies cho Back-end:
```bash
cd ../backend
python -m venv venv
source venv/bin/activate  # Trên Windows: venv\Scripts\activate
pip install -r requirements.txt
```

4. Cấu hình database:
```bash
# Tạo database trong PostgreSQL
# Cập nhật thông tin kết nối trong file config
```

5. Khởi tạo database:
```bash
cd backend
flask db init
flask db migrate
flask db upgrade
```

### Chạy ứng dụng

1. Chạy Back-end:
```bash
cd backend
flask run
```

2. Chạy Front-end:
```bash
cd frontend
npm start
```

3. Truy cập ứng dụng tại địa chỉ: `http://localhost:3000`

### Cấu hình PWA

1. Tạo và cấu hình file manifest.json:
```json
{
  "name": "Hệ thống Quản lý Nhà hàng",
  "short_name": "NhaHang",
  "description": "Hệ thống hỗ trợ quản lý và đặt món cho nhà hàng",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#4CAF50",
  "icons": [
    {
      "src": "icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

2. Đăng ký Service Worker trong index.js:
```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then(registration => {
        console.log('Service Worker đã đăng ký thành công:', registration.scope);
      })
      .catch(error => {
        console.log('Đăng ký Service Worker thất bại:', error);
      });
  });
}
```

3. Cấu hình Workbox trong webpack.config.js:
```javascript
const { GenerateSW } = require('workbox-webpack-plugin');

// Thêm vào plugins
plugins: [
  // ...existing plugins
  new GenerateSW({
    clientsClaim: true,
    skipWaiting: true,
    runtimeCaching: [
      {
        urlPattern: new RegExp('https://api.nhahang.com/'),
        handler: 'StaleWhileRevalidate',
        options: {
          cacheName: 'api-cache',
          expiration: {
            maxEntries: 50,
            maxAgeSeconds: 60 * 60
          }
        }
      },
      {
        urlPattern: /\.(?:png|jpg|jpeg|svg|gif)$/,
        handler: 'CacheFirst',
        options: {
          cacheName: 'images',
          expiration: {
            maxEntries: 60,
            maxAgeSeconds: 30 * 24 * 60 * 60 // 30 ngày
          }
        }
      }
    ]
  })
]
```

### Lợi ích của PWA trong hệ thống nhà hàng

1. **Trải nghiệm Offline**: Khách hàng có thể xem menu, lịch sử đặt hàng, và thông tin khuyến mãi ngay cả khi mất kết nối internet.

2. **Tăng tốc độ truy cập**: Người dùng không phải tải lại toàn bộ ứng dụng mỗi lần truy cập, giúp tiết kiệm dữ liệu và thời gian chờ đợi.

3. **Thông báo đẩy**: Gửi thông báo thời gian thực khi đơn hàng được xác nhận, đang chuẩn bị, hoặc sẵn sàng để phục vụ.

4. **Tiết kiệm chi phí**: Không cần phát triển ứng dụng native riêng cho iOS và Android, tiết kiệm thời gian và chi phí phát triển.

5. **Hiển thị như ứng dụng native**: Người dùng có thể thêm ứng dụng vào màn hình chính của thiết bị, mở ứng dụng với giao diện toàn màn hình không có thanh URL, cải thiện trải nghiệm người dùng.

6. **Tương thích đa nền tảng**: Hoạt động trên tất cả các thiết bị có trình duyệt web hiện đại.

## Người đóng góp

- Trần Công Tiến - Lead Developer

## Giấy phép

© 2025 Hệ thống quản lý nhà hàng. All Rights Reserved.