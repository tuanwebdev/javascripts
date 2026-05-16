
## Arrow Function (Hàm Mũi Tên) Là Gì?
Đây là cách viết hàm **ngắn gọn hơn** trong JavaScript, dùng dấu `=>` thay vì từ khóa `function`.

### So sánh cú pháp
```javascript
// Cách viết thông thường
function add(a, b) {
  return a + b;
}

// Arrow function - gọn hơn nhiều
const add = (a, b) => a + b;
```

---

## Những Điều Cốt Lõi Cần Nhớ

### 1. Cú pháp linh hoạt
*   **Một tham số**: Có thể bỏ dấu ngoặc đơn
    ```javascript
    x => x * 2          // Đúng
    (x) => x * 2        // Cũng đúng
    ```
*   **Không tham số hoặc nhiều tham số**: Bắt buộc có dấu ngoặc đơn
    ```javascript
    () => console.log('Hello')
    (a, b) => a + b
    ```
*   **Thân hàm một dòng**: Tự động trả về, không cần `return`
    ```javascript
    (a, b) => a + b     // Tự động return a + b
    ```
*   **Thân hàm nhiều dòng**: Cần `{}` và `return` (nếu muốn trả về)
    ```javascript
    (a, b) => {
      const sum = a + b;
      return sum * 2;
    }
    ```

### 2. Không có `this` riêng – ĐÂY LÀ KHÁC BIỆT QUAN TRỌNG NHẤT
Arrow function **không tự tạo ra `this`**. Nó "mượn" `this` từ nơi nó được định nghĩa. Điều này giải quyết rất nhiều rắc rối với callback.

```javascript
// Với function thường: this bị "lạc"
const person = {
  name: 'Alice',
  greet: function() {
    setTimeout(function() {
      console.log('Xin chào, tôi là ' + this.name); // ❌ this.name = undefined
    }, 100);
  }
};

// Với arrow function: this được giữ đúng
const person2 = {
  name: 'Bob',
  greet: function() {
    setTimeout(() => {
      console.log('Xin chào, tôi là ' + this.name); // ✅ this.name = "Bob"
    }, 100);
  }
};
```

### 3. Những hạn chế – Khi KHÔNG NÊN dùng arrow function
*   **Làm phương thức trong object**: Không nên vì `this` sẽ không trỏ tới object đó.
    ```javascript
    const obj = {
      name: 'Sai',
      sayHi: () => console.log(this.name) // ❌ this không phải obj
    };
    ```
*   **Dùng làm constructor**: Không thể dùng `new` với arrow function.
*   **Khi cần `arguments` object**: Arrow function không có `arguments`.

---

## Liên Hệ Với React (Điều Bạn Đã Học)
Trong React, arrow function được dùng **rất nhiều** vì sự ngắn gọn và không phải lo lắng về `this`:
```javascript
// Truyền callback trong JSX
<button onClick={() => setCount(count + 1)}>

// Custom Hook với useCallback
const increment = useCallback(() => setCount(prev => prev + 1), []);
```
