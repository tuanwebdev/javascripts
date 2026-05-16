
## Async Function là gì?
Đây là cách viết code bất đồng bộ **trông giống như code đồng bộ**, giúp bạn thoát khỏi "bẫy" `.then()` lồng nhau. Một hàm `async` luôn trả về một **Promise**.

## Cú pháp cơ bản
```javascript
async function tênHàm() {
  const kếtQuả = await promise; // Tạm dừng ở đây cho đến khi promise hoàn thành
  return kếtQuả;
}
```
*   **`async`**: Khai báo đây là hàm bất đồng bộ, luôn trả về Promise.
*   **`await`**: Chỉ được dùng **bên trong** hàm `async`. Nó tạm dừng hàm cho đến khi Promise phía sau nó được giải quyết (fulfilled/rejected).

---

## 3 Điều Cốt Lõi Cần Nhớ

### 1. Hàm `async` luôn trả về Promise
Dù bạn `return` giá trị gì, nó cũng được tự động bọc trong `Promise.resolve()`.
```javascript
async function chào() {
  return "Xin chào!"; // Tự động thành Promise.resolve("Xin chào!")
}
chào().then(console.log); // "Xin chào!"
```

### 2. Cách hàm chạy: Đồng bộ đến `await` đầu tiên, sau đó bất đồng bộ
Code **trước `await` đầu tiên** chạy đồng bộ. Khi gặp `await`, hàm tạm dừng, trả quyền điều khiển về cho bên ngoài. Sau khi promise hoàn thành, hàm chạy tiếp từ dòng `await` đó.

```javascript
async function víDụ() {
  console.log("1. Bắt đầu");        // Đồng bộ
  await new Promise(r => setTimeout(r, 1000)); // Tạm dừng 1 giây
  console.log("2. Sau 1 giây");     // Chạy sau khi promise resolve
}
víDụ();
console.log("3. Bên ngoài");        // Chạy ngay, không chờ víDụ
// Output: 1 -> 3 -> (1 giây sau) 2
```

### 3. Xử lý lỗi bằng `try...catch` – Đẹp hơn `.catch()`
```javascript
async function lấyDữLiệu() {
  try {
    const response = await fetch(url); // Nếu fetch lỗi, nhảy sang catch
    const data = await response.json();
    return data;
  } catch (lỗi) {
    console.log("Xử lý lỗi gọn gàng:", lỗi);
  }
}
```

---

## So sánh: Chuỗi `.then()` vs `async/await`

### ❌ Cách cũ – Promise chain
```javascript
function lấyDữLiệuNgườiDùng() {
  getUser(1)
    .then(user => getOrders(user.id))
    .then(orders => getOrderDetails(orders[0].id))
    .then(details => console.log(details))
    .catch(error => console.log(error));
}
```

### ✅ Cách mới – `async/await`
```javascript
async function lấyDữLiệuNgườiDùng() {
  try {
    const user = await getUser(1);
    const orders = await getOrders(user.id);
    const details = await getOrderDetails(orders[0].id);
    console.log(details);
  } catch (error) {
    console.log(error);
  }
}
```

---

## Lưu ý quan trọng về chạy song song
Nếu có nhiều `await` **không phụ thuộc nhau**, đừng chờ tuần tự:

```javascript
// ❌ Chậm: Chờ tuần tự (3 giây)
async function chậm() {
  await delay2s(); // Chờ 2s
  await delay1s(); // Chờ thêm 1s -> Tổng 3s
}

// ✅ Nhanh: Chạy song song (2 giây)
async function nhanh() {
  const promise2s = delay2s(); // Khởi động ngay
  const promise1s = delay1s(); // Khởi động ngay
  await Promise.all([promise2s, promise1s]); // Chờ cả hai, chỉ mất 2s
}
```

---

## Tóm tắt
| Khái niệm | Mô tả |
|-----------|-------|
| `async` | Khai báo hàm luôn trả về Promise |
| `await` | Tạm dừng hàm `async` để chờ Promise |
| `try...catch` | Cách bắt lỗi "sạch" trong `async/await` |
| **Luồng chạy** | Code trước `await` đầu tiên chạy đồng bộ, sau đó bất đồng bộ |
