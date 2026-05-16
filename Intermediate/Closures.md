
## Closure là gì?
**Closure (bao đóng)** là một hàm có thể "nhớ" và truy cập các biến từ phạm vi bên ngoài nó, **ngay cả khi hàm đó được thực thi ở một nơi khác**, sau khi phạm vi bên ngoài đã kết thúc.

Nói đơn giản: **Closure = Hàm + Môi trường nơi hàm được tạo ra**.

---

## Ví dụ "Thần Thánh" Để Hiểu Ngay

```javascript
function tạoMáyĐếm() {
  let count = 0; // Biến này là "riêng tư"

  function đếm() {
    count++; // Truy cập biến từ bên ngoài
    console.log(count);
  }

  return đếm; // Trả về hàm, KHÔNG trả về giá trị
}

const máyĐếm1 = tạoMáyĐếm(); // tạoMáyĐếm() chạy xong rồi
máyĐếm1(); // 1  <-- Nhưng hàm vẫn "nhớ" biến count!
máyĐếm1(); // 2  <-- Vẫn nhớ tiếp!
máyĐếm1(); // 3

const máyĐếm2 = tạoMáyĐếm(); // Tạo một closure MỚI, độc lập
máyĐếm2(); // 1  <-- Môi trường riêng, count bắt đầu từ 0
```

**Điều gì đã xảy ra?**
1. Hàm `tạoMáyĐếm()` chạy, tạo biến `count = 0`.
2. Nó trả về hàm `đếm` bên trong.
3. Thông thường, khi hàm chạy xong, biến `count` sẽ bị dọn rác. Nhưng vì hàm `đếm` vẫn "tham chiếu" đến nó, **JavaScript giữ lại môi trường đó**.
4. Mỗi lần gọi `máyĐếm1()`, nó vẫn tăng và in `count` – đó chính là closure!

---

## Vì Sao Closure Quan Trọng?

### 1. Tạo biến "riêng tư" (Private Variables)
Trước khi có class, closure là cách duy nhất để tạo dữ liệu không bị truy cập từ bên ngoài:
```javascript
function tạoVí() {
  let tiền = 1000;
  return {
    xemTiền: () => tiền,
    thêmTiền: (số) => { tiền += số },
  };
}

const víTôi = tạoVí();
console.log(víTôi.tiền); // undefined – không thể truy cập trực tiếp!
console.log(víTôi.xemTiền()); // 1000
víTôi.thêmTiền(500);
console.log(víTôi.xemTiền()); // 1500
```

### 2. Giải thích cách Hooks hoạt động trong React
Closure là cách `useState`, `useEffect`... giữ trạng thái qua các lần render. Mỗi lần component render, nó tạo ra một "bản chụp" của các giá trị, và closure giữ chúng lại.

---

## Một Cạm Bẫy Cần Tránh

```javascript
// ❌ Code gây nhầm lẫn
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // In ra 3, 3, 3 (không phải 0, 1, 2)
}

// ✅ Sửa bằng let (tạo block scope)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // In ra 0, 1, 2
}
```
Với `var`, tất cả closure chia sẻ cùng một biến `i` (và `i` thành 3 sau vòng lặp). Với `let`, mỗi lần lặp có một `i` riêng.

---

## Tóm Tắt
| Đặc điểm | Ý nghĩa |
|----------|---------|
| **Closure là gì** | Hàm + Môi trường nơi nó được tạo |
| **Tại sao dùng** | Tạo biến riêng tư, giữ trạng thái, quản lý callback |
| **Cần nhớ** | Biến được "nhớ" là tham chiếu, không phải bản sao giá trị |

