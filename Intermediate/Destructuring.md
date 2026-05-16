
## Destructuring Là Gì?
Đây là cú pháp giúp bạn **"mở gói"** giá trị từ mảng hoặc thuộc tính từ object vào các biến riêng biệt – giống như mở vali lấy đồ ra từng món vậy.

---

## 1. Phân Rã Mảng (Array Destructuring)

### Cú pháp cơ bản
```javascript
const numbers = [1, 2, 3];
// Thay vì:
// const a = numbers[0], b = numbers[1], c = numbers[2];

// Dùng destructuring:
const [a, b, c] = numbers;
console.log(a); // 1
console.log(b); // 2
```

### Các "chiêu" hay dùng
*   **Bỏ qua phần tử**: Dùng dấu phẩy
    ```javascript
    const [first, , third] = [10, 20, 30];
    console.log(third); // 30
    ```
*   **Giá trị mặc định**: Phòng khi phần tử `undefined`
    ```javascript
    const [x = 5, y = 7] = [1];
    console.log(x); // 1
    console.log(y); // 7 (mặc định)
    ```
*   **Hoán đổi biến siêu nhanh** (không cần biến tạm):
    ```javascript
    let a = 1, b = 2;
    [a, b] = [b, a];
    console.log(a, b); // 2, 1
    ```
*   **Lấy phần còn lại (rest)**: Dùng `...`
    ```javascript
    const [head, ...tail] = [1, 2, 3, 4];
    console.log(head); // 1
    console.log(tail); // [2, 3, 4]
    ```

---

## 2. Phân Rã Object (Object Destructuring)

Đây là thứ bạn sẽ thấy **khắp nơi trong React** (props, hooks...).

### Cú pháp cơ bản
```javascript
const user = { name: 'Alice', age: 25 };

// Thay vì:
// const name = user.name, age = user.age;

// Dùng destructuring (tên biến PHẢI trùng tên thuộc tính):
const { name, age } = user;
console.log(name, age); // Alice 25
```

### Các "chiêu" quan trọng với object
*   **Đổi tên biến**: Dùng dấu `:`
    ```javascript
    const { name: userName, age: userAge } = user;
    console.log(userName); // Alice
    ```
*   **Giá trị mặc định**:
    ```javascript
    const { name, role = 'user' } = user;
    console.log(role); // 'user'
    ```
*   **Lấy phần còn lại (rest)**:
    ```javascript
    const { name, ...details } = { name: 'Bob', age: 30, city: 'HN' };
    console.log(details); // { age: 30, city: 'HN' }
    ```
*   **Destructuring lồng nhau**:
    ```javascript
    const user = { id: 1, info: { name: 'Alice', address: { city: 'HN' } } };
    const { info: { name, address: { city } } } = user;
    console.log(city); // HN
    ```

---

## Liên Hệ React: Bạn Đã Vô Tình Dùng Nó!

Trong React, bạn dùng destructuring liên tục mà có thể không để ý:

```javascript
// Dùng với props (cực kỳ phổ biến)
function Greeting({ name, age }) {
  return <p>{name} - {age}</p>;
}

// Dùng với hooks
const [count, setCount] = useState(0);        // Mảng
const { data, error } = useQuery();            // Object

// Dùng với import
import { useState, useEffect } from 'react';
```

---

## Tóm Lại
Destructuring giúp code **sạch hơn, ngắn hơn, dễ đọc hơn**. Với React, đó là kỹ năng bắt buộc vì bạn sẽ gặp nó ở mọi nơi: từ `useState`, props, cho đến import.