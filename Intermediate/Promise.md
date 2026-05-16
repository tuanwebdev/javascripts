
## Promise là gì?
**Promise (lời hứa)** là một đối tượng đại diện cho **kết quả cuối cùng** của một tác vụ bất đồng bộ (như gọi API, đọc file...) và **giá trị kết quả của nó sẽ có trong tương lai**.

Hãy tưởng tượng bạn đặt một món hàng online:
- Bạn **không biết ngay** món hàng có về không
- Nhưng bạn được **hứa** rằng: khi có kết quả (thành công hoặc thất bại), bạn sẽ được thông báo
- Trong lúc chờ, bạn vẫn làm việc khác bình thường

---

## 3 Trạng Thái Của Promise
| Trạng thái | Ý nghĩa | Ví von |
|------------|---------|--------|
| **Pending** | Đang chờ | Đơn hàng đang xử lý |
| **Fulfilled** | Thành công | Đơn hàng đã giao tới |
| **Rejected** | Thất bại | Đơn hàng bị hủy |

Một Promise chỉ chuyển trạng thái **một lần duy nhất**: từ Pending → Fulfilled (thành công) hoặc Pending → Rejected (thất bại). Sau đó nó "đóng băng" ở trạng thái đó mãi mãi.

---

## Cú Pháp Cơ Bản

### Tạo một Promise
```javascript
const myPromise = new Promise((resolve, reject) => {
  // Làm việc gì đó bất đồng bộ...
  
  const thànhCông = true; // Giả sử thành công

  if (thànhCông) {
    resolve("Dữ liệu trả về"); // Gọi resolve để báo thành công
  } else {
    reject("Lý do thất bại");  // Gọi reject để báo lỗi
  }
});
```

### Sử dụng Promise
```javascript
myPromise
  .then((result) => {
    console.log("Thành công:", result); // Chạy khi resolve được gọi
  })
  .catch((error) => {
    console.log("Thất bại:", error);    // Chạy khi reject được gọi
  })
  .finally(() => {
    console.log("Luôn chạy dù thành công hay thất bại");
  });
```

---

## Ví Dụ Thực Tế (Mô Phỏng Gọi API)

```javascript
// Hàm giả lập gọi API mất 2 giây để trả về dữ liệu user
function fetchUser(userId) {
  return new Promise((resolve, reject) => {
    console.log("⏳ Đang tải dữ liệu user...");
    
    setTimeout(() => {
      // Giả lập: 80% thành công, 20% thất bại
      if (Math.random() > 0.2) {
        resolve({ id: userId, name: "Alice", age: 25 });
      } else {
        reject("Lỗi mạng: Không thể kết nối server");
      }
    }, 2000); // Giả lập độ trễ mạng 2 giây
  });
}

// Sử dụng
fetchUser(1)
  .then((user) => {
    console.log("✅ Lấy user thành công:", user);
    return user.name; // Có thể return để .then() tiếp theo nhận
  })
  .then((name) => {
    console.log("👤 Tên user là:", name);
  })
  .catch((error) => {
    console.log("❌ Lỗi:", error);
  })
  .finally(() => {
    console.log("🔚 Hoàn tất (dù thành công hay thất bại)");
  });
```

---

## Tại Sao Cần Promise? (Thoát Khỏi "Callback Hell")

### ❌ Code Không Dùng Promise – Lồng Nhau Khó Đọc (Callback Hell)
```javascript
// Mỗi bước phải lồng vào callback của bước trước
getUser(1, (user) => {
  getOrders(user.id, (orders) => {
    getOrderDetails(orders[0].id, (details) => {
      console.log(details); // Code lồng sâu, khó đọc, khó debug
    });
  });
});
```

### ✅ Code Dùng Promise – Chuỗi Thẳng Hàng, Dễ Đọc
```javascript
getUser(1)
  .then((user) => getOrders(user.id))
  .then((orders) => getOrderDetails(orders[0].id))
  .then((details) => console.log(details))
  .catch((error) => console.log(error)); // Một chỗ bắt lỗi cho cả chuỗi
```

---

## Các Phương Thức Tĩnh Quan Trọng

```javascript
// 1. Đợi nhiều promise cùng lúc – tất cả phải thành công
Promise.all([promise1, promise2, promise3])
  .then(([kq1, kq2, kq3]) => console.log(kq1, kq2, kq3));

// 2. Đợi nhiều promise – lấy kết quả của thằng nhanh nhất
Promise.race([promise1, promise2, promise3])
  .then((kếtQuảNhanhNhất) => console.log(kếtQuảNhanhNhất));

// 3. Tạo promise đã resolved ngay lập tức
Promise.resolve("Giá trị có sẵn").then(val => console.log(val));

// 4. Tạo promise đã rejected ngay lập tức
Promise.reject("Lỗi ngay").catch(err => console.log(err));
```

---

## Async/Await – Cách Viết Promise "Sạch" Hơn
Đây chỉ là "cú pháp đường" (syntactic sugar) giúp viết code trông như đồng bộ, nhưng vẫn là bất đồng bộ:

```javascript
// Thay vì .then()
async function loadUser() {
  try {
    const user = await fetchUser(1);    // Đợi promise resolve
    const orders = await getOrders(user.id); // Rồi mới chạy tiếp
    console.log(user, orders);
  } catch (error) {
    console.log("Lỗi:", error);
  }
}
```

---

## Tóm Lại
| Khái niệm | Ý nghĩa |
|-----------|---------|
| **Promise** | Đối tượng đại diện cho kết quả trong tương lai |
| **resolve()** | Báo thành công, truyền dữ liệu |
| **reject()** | Báo thất bại, truyền lỗi |
| **.then()** | Xử lý khi thành công |
| **.catch()** | Xử lý khi thất bại |
| **.finally()** | Luôn chạy sau cùng |
