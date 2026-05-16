
### 1. Rest Parameters (`...args`): "Gom" lại thành một mảng
**Mục đích:** Dùng trong **định nghĩa hàm** để gom vô số tham số truyền vào thành **một mảng** duy nhất. Hãy tưởng tượng nó như một chiếc túi hứng tất cả đồ bạn ném vào.

**Ví dụ cơ bản:**
```javascript
function sum(...args) { // args là một mảng chứa tất cả tham số
  let total = 0;
  args.forEach(arg => total += arg); // Duyệt mảng dễ dàng
  return total;
}

console.log(sum(1));        // 1
console.log(sum(1, 2));     // 3
console.log(sum(1, 2, 3));  // 6
```

**Lưu ý quan trọng:**
*   **Phải đặt ở cuối:** `function f(arg1, ...rest, arg2)` là **sai**. Rest parameters luôn phải là tham số cuối cùng.
*   **Khác với `arguments`:** Rest parameters là một **mảng thực sự**, có thể dùng `forEach`, `map`... Trong khi `arguments` là một "object giống mảng" cũ.
*   **Arrow function không có `arguments`:** Vì vậy, rest parameters là cách hiện đại và được khuyến khích dùng.

---

### 2. Spread Operator (`...arr`): "Trải" mảng ra
**Mục đích:** Dùng trong **lời gọi hàm** hoặc **tạo mảng/object mới** để trải các phần tử của một mảng thành các giá trị riêng lẻ. Giống như bạn đổ cái túi ra, lấy từng món đồ riêng biệt.

**Ví dụ cơ bản với hàm `Math.max`:**
```javascript
let arr = [3, 5, 1];

// Nếu không có spread: Math.max(arr) -> NaN (vì nhận một mảng, không phải các số)
// Với spread: trải mảng thành danh sách tham số
console.log(Math.max(...arr)); // 5, tương đương với Math.max(3, 5, 1)
```

**Ví dụ tạo mảng/object mới (rất phổ biến trong React):**
```javascript
// Tạo mảng mới bằng cách gộp
let arr1 = [3, 5];
let arr2 = [8, 9];
let merged = [0, ...arr1, 2, ...arr2]; // [0, 3, 5, 2, 8, 9]

// Tạo object mới (thường dùng trong setState của React)
let user = { name: 'Alice', age: 25 };
let updatedUser = { ...user, age: 26 }; // { name: 'Alice', age: 26 }
```

### 📝 Tóm lại, cách phân biệt siêu nhanh:
*   **Rest Parameters:** Gom lại. Xuất hiện ở **nơi khai báo** (function parameters). `function f(...tuiDo)`
*   **Spread Operator:** Trải ra. Xuất hiện ở **nơi gọi hàm** hoặc trong mảng/object. `console.log(...chiecTui)`
