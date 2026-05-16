
## Hoisting là gì?
**Hoisting (nâng lên)** là hành vi của JavaScript, trong đó **phần khai báo** của biến, hàm, class... dường như được "chuyển lên đầu" phạm vi của chúng trước khi code được thực thi.

Hãy tưởng tượng JavaScript "đọc lướt" code của bạn một lần trước khi chạy, để ghi nhớ tất cả các khai báo. Nhờ đó, bạn có thể **dùng hàm trước khi viết nó**, hoặc **dùng biến `var` trước khi khai báo** (nhưng giá trị là `undefined`).

---

## 4 Kiểu Hoisting (Quan trọng nhất!)

| Kiểu | Hành vi | Áp dụng cho |
|------|---------|-------------|
| **Type 1: Value Hoisting** | Dùng được **cả giá trị** trước khi khai báo | `function`, `function*`, `async function`, `import` |
| **Type 2: Declaration Hoisting** | Dùng được nhưng giá trị là `undefined` | `var` |
| **Type 3: "Taints" Scope** | Biến "làm bẩn" scope, gây lỗi nếu dùng sớm | `let`, `const`, `class` |
| **Type 4: Side Effects** | Khai báo gây hiệu ứng phụ trước khi code chạy | `import` |

---

## Ví dụ Cụ Thể Cho Từng Loại

### ✅ Type 1: Function – Gọi được trước khi khai báo
```javascript
sayHello(); // "Xin chào!" – Hoạt động bình thường

function sayHello() {
  console.log("Xin chào!");
}
```
Toàn bộ hàm (cả tên và thân hàm) được đưa lên đầu scope.

---

### ⚠️ Type 2: `var` – Dùng được nhưng là `undefined`
```javascript
console.log(name); // undefined (KHÔNG báo lỗi!)
var name = "Alice";
console.log(name); // "Alice"
```
Chỉ phần **khai báo** `var name` được hoisting, còn phần **gán giá trị** `= "Alice"` vẫn ở nguyên chỗ.

---

### ❌ Type 3: `let`/`const`/`class` – "Vùng chết tạm thời" (TDZ)
```javascript
console.log(name); // ❌ ReferenceError!
let name = "Alice";
```
Biến `let`/`const` vẫn được hoisting, nhưng rơi vào **Temporal Dead Zone (TDZ)** – vùng chết tạm thời từ đầu scope đến dòng khai báo. Bạn **không thể truy cập** biến trong vùng này.

**Ví dụ TDZ "làm bẩn" scope cha:**
```javascript
let x = 1; // Biến global

function test() {
  console.log(x); // ❌ ReferenceError! (không phải in ra 1)
  const x = 2;    // const x "làm bẩn" toàn bộ scope của hàm test
}
test();
```
Dù `const x = 2` chưa chạy, nó vẫn "chiếm" toàn bộ scope, khiến `console.log(x)` không thể nhìn ra `x = 1` bên ngoài.

---

### ✅ Type 4: `import` – Luôn được nâng lên
```javascript
console.log(myModule); // Hoạt động nếu myModule đã được import
import myModule from './module.js';
```
`import` luôn được xử lý trước khi bất kỳ code nào trong module chạy.

---

## So Sánh Nhanh

| Code | Kết quả | Vì sao? |
|------|---------|---------|
| `fn(); function fn(){}` | ✅ Chạy được | Type 1 – value hoisting |
| `console.log(a); var a = 1;` | `undefined` | Type 2 – declaration hoisting |
| `console.log(b); let b = 2;` | ❌ ReferenceError | Type 3 – TDZ |
| `const fn = () => {}; fn();` | Phụ thuộc vị trí gọi | Arrow function gán vào biến → theo luật của biến đó |

---

## Tóm Lại
| Khái niệm | Ý nghĩa |
|-----------|---------|
| **Hoisting** | JavaScript "ghi nhớ" khai báo trước khi chạy code |
| **Function** | Dùng thoải mái trước khi khai báo |
| **`var`** | Dùng được nhưng giá trị `undefined` |
| **`let`/`const`** | Không dùng được trước khai báo (TDZ) |
| **`import`** | Luôn được ưu tiên xử lý trước |

**Mẹo thực tế:** Cứ khai báo trước, dùng sau để tránh rắc rối. Đừng dựa vào hoisting để viết code "thông minh" – nó chỉ khiến người khác (và chính bạn sau này) khó đọc hơn.
