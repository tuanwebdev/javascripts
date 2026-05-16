
## 1. Control Flow (Luồng Điều Khiển)
Đây là cách JavaScript quyết định **dòng code nào sẽ chạy** dựa trên các điều kiện và vòng lặp.

### Cấu Trúc Rẽ Nhánh
*   **`if...else`**: Đưa ra quyết định "nếu-thì".
    ```javascript
    if (điềuKiện) {
      // Code chạy nếu điều kiện đúng
    } else {
      // Code chạy nếu điều kiện sai
    }
    ```
*   **`switch`**: Giải pháp gọn gàng hơn khi phải so sánh một giá trị với nhiều trường hợp.
    ```javascript
    switch (giáTrị) {
      case 1:
        console.log("Giá trị là 1");
        break; // Đừng quên break!
      default:
        console.log("Không phải 1");
    }
    ```

### Vòng Lặp
*   **`for`**: Lặp với số lần biết trước.
*   **`while`**: Lặp khi điều kiện còn đúng.
*   **`do...while`**: Luôn chạy ít nhất 1 lần rồi mới kiểm tra điều kiện.

---

## 2. Error Handling (Xử Lý Lỗi) – Rất Quan Trọng
Mục đích: Bắt lỗi để ứng dụng không bị "sập" đột ngột.

### Câu Lệnh Cốt Lõi: `try...catch...finally`
```javascript
try {
  // Thử chạy code có thể gây lỗi
  let kếtQuả = hàmNguyHiểm();
} catch (lỗi) {
  // Nếu có lỗi xảy ra, code ở đây sẽ chạy
  console.log("Đã bắt được lỗi:", lỗi.message);
} finally {
  // Luôn chạy dù có lỗi hay không. Rất hữu ích để "dọn dẹp".
  console.log("Kết thúc.");
}
```

### Tự Tạo Lỗi Để Kiểm Tra: `throw`
Bạn có thể chủ động ném ra một lỗi để dừng thực thi và nhảy vào `catch`.
```javascript
function chia(a, b) {
  if (b === 0) {
    throw new Error("Không thể chia cho 0!"); // Tự tạo lỗi
  }
  return a / b;
}

try {
  console.log(chia(10, 0));
} catch (e) {
  console.log(e.message); // In ra: "Không thể chia cho 0!"
}
```

---

## Tóm Lại & Liên Hệ React
| Khái niệm | Mục đích |
|:---|:---|
| **`if/switch`** | Quyết định chạy code nào |
| **Vòng lặp** | Làm một việc nhiều lần |
| **`try...catch`** | Bắt lỗi để ứng dụng không chết |
| **`throw`** | Chủ động tạo lỗi để kiểm soát |
