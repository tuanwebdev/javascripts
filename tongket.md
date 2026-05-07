Dưới đây là danh sách các hàm và phương thức xử lý thời gian (Date) phổ biến nhất trong JavaScript, được định dạng sẵn theo chuẩn Markdown để bạn có thể sao chép trực tiếp vào tài liệu của mình.

---

## Các hàm xử lý thời gian phổ biến trong JavaScript

### 1. Khởi tạo đối tượng thời gian
Để làm việc với thời gian, trước hết chúng ta cần khởi tạo đối tượng `Date`.

```javascript
// Lấy thời gian hiện tại
const bâyGiờ = new Date();

// Khởi tạo từ một chuỗi thời gian cụ thể
const ngàyCụThể = new Date("2026-05-07T12:00:00");

// Khởi tạo bằng các tham số (Năm, Tháng-1, Ngày, Giờ, Phút, Giây)
const ngàyTùyChỉnh = new Date(2026, 4, 7, 10, 30); // Lưu ý: Tháng trong JS chạy từ 0-11
```

### 2. Các phương thức "Get" (Lấy thông tin)
Dùng để trích xuất từng phần cụ thể từ đối tượng `Date`.

| Hàm | Mô tả | Ví dụ | Kết quả (Giả sử hiện tại là 07/05/2026) |
| :--- | :--- | :--- | :--- |
| `getFullYear()` | Lấy năm (4 chữ số) | `now.getFullYear()` | `2026` |
| `getMonth()` | Lấy tháng (**0-11**) | `now.getMonth()` | `4` (tương đương tháng 5) |
| `getDate()` | Lấy ngày trong tháng (1-31) | `now.getDate()` | `7` |
| `getDay()` | Lấy thứ trong tuần (0-6) | `now.getDay()` | `4` (Thứ Năm) |
| `getHours()` | Lấy số giờ (0-23) | `now.getHours()` | `12` |
| `getMinutes()` | Lấy số phút (0-59) | `now.getMinutes()` | `15` |
| `getTime()` | Lấy Timestamp (mili giây từ 1/1/1970) | `now.getTime()` | `1770183300000` |

### 3. Định dạng thời gian thành chuỗi (Formatting)
Các hàm giúp biến đối tượng Date thành chuỗi dễ đọc.

#### a. `toLocaleDateString()` - Định dạng ngày theo khu vực
```javascript
const now = new Date();
console.log(now.toLocaleDateString('vi-VN')); 
// Kết quả: "07/05/2026"
```

#### b. `toISOString()` - Định dạng chuẩn ISO (thường dùng cho API/Database)
```javascript
console.log(now.toISOString());
// Kết quả: "2026-05-07T05:15:00.000Z"
```

### 4. Tính toán khoảng cách thời gian
Để tính toán giữa hai mốc thời gian, chúng ta thường chuyển về dạng Timestamp (mili giây).

```javascript
const start = new Date("2026-05-01");
const end = new Date("2026-05-07");

const diffInMs = end - start; // Kết quả tính bằng mili giây
const diffInDays = diffInMs / (1000 * 60 * 60 * 24);

console.log(`Khoảng cách: ${diffInDays} ngày`);
// Kết quả: "Khoảng cách: 6 ngày"
```

### 5. Sử dụng `Date.now()`
Nếu bạn chỉ cần lấy Timestamp hiện tại mà không cần tạo đối tượng Date phức tạp, hãy dùng `Date.now()`. Đây là cách tối ưu hơn về hiệu suất.

```javascript
const timestamp = Date.now();
console.log(timestamp);
// Kết quả: 1770183300000
```

---

### Một số lưu ý quan trọng:
*   **Tháng trong JS:** `0` là tháng 1, `11` là tháng 12. Đây là lỗi phổ biến nhất khi mới làm việc với Date.
*   **Thứ trong tuần:** `0` là Chủ Nhật, `1` là Thứ Hai... `6` là Thứ Bảy.
*   **Thư viện hỗ trợ:** Đối với các dự án thực tế yêu cầu xử lý thời gian phức tạp (múi giờ, cộng trừ ngày tháng khó), bạn có thể tham khảo các thư viện như **date-fns** hoặc **Day.js** để code sạch (Clean Code) và dễ bảo trì hơn.