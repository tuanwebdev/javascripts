
## Scope là gì?
**Scope là phạm vi mà một biến hoặc biểu thức có thể được "nhìn thấy" và sử dụng.** Nếu bạn cố dùng một biến ngoài phạm vi của nó, JavaScript sẽ báo lỗi.

Hãy tưởng tượng scope như những chiếc hộp lồng vào nhau. Hộp con có thể nhìn thấy đồ trong hộp cha, nhưng hộp cha **không thể** nhìn thấy đồ trong hộp con.

---

## 4 Loại Scope trong JavaScript

| Loại Scope | Được tạo bởi | Ví dụ |
|------------|--------------|-------|
| **Global** | Mặc định cho code chạy ở script mode | Biến khai báo ngoài mọi hàm |
| **Module** | Code chạy trong module mode | File `.js` được import/export |
| **Function** | Mỗi khi một hàm được tạo | Biến khai báo bên trong `function` |
| **Block** | Cặp dấu `{}` (chỉ với `let`, `const`, `class`) | Biến trong `if`, `for`, `{}` trần |

---

## Ví dụ Trực Quan

```javascript
// GLOBAL SCOPE - ai cũng thấy
let globalVar = "Tôi là global";

function myFunction() {
  // FUNCTION SCOPE - chỉ bên trong hàm này thấy
  let functionVar = "Tôi trong hàm";

  if (true) {
    // BLOCK SCOPE - chỉ trong cặp {} này thấy
    let blockVar = "Tôi trong block";
    const alsoBlock = "Tôi cũng block";
    var notBlock = "Tôi không bị block"; // var không tôn trọng block scope!

    console.log(globalVar);   // ✅ Thấy được (global)
    console.log(functionVar); // ✅ Thấy được (function cha)
    console.log(blockVar);    // ✅ Thấy được (cùng block)
  }

  console.log(globalVar);     // ✅ Thấy được
  console.log(functionVar);   // ✅ Thấy được (cùng function)
  console.log(blockVar);      // ❌ ReferenceError! (blockVar nằm trong block scope)
  console.log(notBlock);      // ✅ Thấy được! (var không bị giới hạn bởi block)
}

console.log(globalVar);       // ✅ Thấy được
console.log(functionVar);     // ❌ ReferenceError! (functionVar nằm trong function scope)
```

---

## Quy Tắc Vàng: Thứ Tự Tìm Biến (Scope Chain)
Khi bạn dùng một biến, JavaScript tìm theo thứ tự:
1. **Block scope** hiện tại
2. **Function scope** cha (nếu có)
3. **Global scope**

```javascript
let x = "global";

function outer() {
  let x = "outer function";
  
  function inner() {
    let x = "inner function";
    console.log(x); // "inner function" - tìm thấy ở block/function gần nhất
  }
  
  inner();
  console.log(x); // "outer function"
}

outer();
console.log(x); // "global"
```

---

## Tại Sao Phải Quan Tâm Scope?
1. **Tránh xung đột tên biến**: Biến trong scope khác nhau có thể trùng tên mà không ảnh hưởng nhau.
2. **Bảo vệ dữ liệu**: Biến trong function không bị truy cập từ bên ngoài.
3. **Hiểu closure**: Closure hoạt động dựa trên scope (hàm nhớ scope nơi nó được tạo).
4. **Tránh lỗi var**: `var` không tôn trọng block scope, dễ gây bug → dùng `let` và `const`.

---

## Tóm Lại
| Khái niệm | Ý nghĩa |
|-----------|---------|
| **Scope** | Phạm vi "nhìn thấy" của biến |
| **Scope Chain** | Chuỗi tìm kiếm từ trong ra ngoài |
| **Global Scope** | Toàn cục, ai cũng thấy |
| **Function Scope** | Chỉ trong hàm đó |
| **Block Scope** | Chỉ trong `{}` (với `let`/`const`) |
