# 🧠 Bài tập: Xử lý chuỗi với Function Pipeline (Slugify)

---

## 📌 Yêu cầu đề bài

### 1. Viết function `pipe(...functions)`

* Nhận vào nhiều function
* Trả về một function mới
* Khi gọi → chạy lần lượt từ trái → phải

👉 Ví dụ:

```js
const process = pipe(f1, f2, f3);
process("input"); // = f3(f2(f1("input")))
```

---

### 2. Tạo các function nhỏ

* `trim(str)` → xóa khoảng trắng đầu cuối
* `toLowerCase(str)` → viết thường
* `removeSpecialChars(str)` → xóa ký tự đặc biệt
* `slugify(str)` → thay khoảng trắng thành `-`

---

### 3. Kết hợp lại

```js
const format = pipe(
  trim,
  toLowerCase,
  removeSpecialChars,
  slugify
);
```

---

### 4. Test

```js
format("   Hello World!!!   "); 
// 👉 expected: "hello-world"
```

---

## 📌 Cách làm của bạn

```js
const removeSpace = (str) => str.trim();
const chuthuong = (str) => str.toLowerCase();
const reomoveSpecialChars = (str) =>{
    const regex = /^[a-zA-Z]+$/;
    return str.filter(char => regex.test(char))
}
const slugify = (str) =>str.replace(' ','-')

const format = slugify(reomoveSpecialChars(chuthuong(removeSpace(str))))

format("  Hello world!!!  ");
```

---

## ❌ Lỗi sai trong cách làm của bạn

### 1. Dùng sai method với string

```js
str.filter(...)
```

* `filter()` chỉ dùng cho **array**, không dùng cho string
* 👉 Phải chuyển sang array trước (`split`)

---

### 2. Regex sai mục đích

```js
/^[a-zA-Z]+$/
```

* Dùng để kiểm tra **toàn bộ chuỗi**, không phù hợp khi lọc từng ký tự
* 👉 Không cần `^` và `$`

---

### 3. `replace()` chưa đúng

```js
str.replace(' ', '-')
```

* Chỉ thay **1 khoảng trắng đầu tiên**
* 👉 Cần dùng regex global `/\s+/g`

---

### 4. Sai biến `str`

```js
slugify(...(str))
```

* `str` chưa được định nghĩa → lỗi runtime

---

### 5. Không dùng `pipe` như yêu cầu

* Bạn đang viết:

```js
f(g(h(x)))
```

* 👉 Đề bài yêu cầu:

```js
pipe(f, g, h)(x)
```

---

## ✅ Cách làm đúng (theo yêu cầu đề bài)

### ✔️ Bước 1: Viết các function nhỏ

```js
// 1. Xóa khoảng trắng đầu cuối
const trim = (str) => str.trim();

// 2. Chuyển thành chữ thường
const toLowerCase = (str) => str.toLowerCase();

// 3. Xóa ký tự đặc biệt (chỉ giữ lại chữ cái và khoảng trắng)
const removeSpecialChars = (str) => str.replace(/[^a-zA-Z0-9 ]/g, "");

// 4. Thay khoảng trắng thành dấu gạch ngang (-)
const slugify = (str) => str.replace(/\s+/g, "-");
```

---

### ✔️ Bước 2: Viết `pipe`

```js
const pipe = (...functions) => (input) =>
  functions.reduce((result, fn) => fn(result), input);
```

---

### ✔️ Bước 3: Kết hợp pipeline

```js
const format = pipe(
  trim,
  toLowerCase,
  removeSpecialChars,
  slugify
);
```

---

### ✔️ Bước 4: Test

```js
console.log(format("   Hello World!!!   "));
// 👉 "hello-world"
```

---

## 🔥 Kết luận

* Bạn đã hiểu đúng hướng: ✔️ chia nhỏ function
* Nhưng sai ở kỹ thuật: ❌ string, regex, replace
* Và chưa áp dụng đúng yêu cầu quan trọng nhất: ❌ `pipe`
---
