
# 🧠 Bài tập: Shopping Cart – Function (JavaScript)

## 📌 Yêu cầu (`addItem`)

Viết hàm `createCart()` trả về một object có method `addItem`:

### 👉 `addItem(name, price, quantity)`

* Thêm sản phẩm vào giỏ hàng
* Nếu sản phẩm **chưa tồn tại** → thêm mới vào mảng
* Nếu sản phẩm **đã tồn tại (trùng name)** → cập nhật `quantity` (cộng thêm)
* Không sử dụng biến global
* Dữ liệu phải được đóng gói bằng **closure**

---

# ❌ Lời giải bạn đã làm

```js
const listItem  = [];
function  createCard() {
    return {
       function  addItem(name, price, quantity) {
            const exits_item  = listItem.filter(item => item.name === name);
            if(exits_item) {
                listItem.map(item ={
                    return item.name ===exits_item?[...listItem, item.quantity+=1]:item
                })
            }
       }
    }
}
```

---

# 🚨 Các lỗi sai

## 1. ❌ Dùng biến global

```js
const listItem = [];
```

* Vi phạm yêu cầu đề bài
* Không có closure → dữ liệu dễ bị truy cập từ ngoài

---

## 2. ❌ Sai cú pháp object

```js
function addItem(...) {}
```

* Không được viết `function` bên trong object như vậy

---

## 3. ❌ Sai cách kiểm tra tồn tại

```js
filter()
```

* `filter()` luôn trả về array → kể cả rỗng vẫn truthy
* Không phù hợp để tìm 1 phần tử

---

## 4. ❌ Sai hoàn toàn khi dùng `map`

```js
listItem.map(...)
```

* `map` dùng để tạo array mới, không phải update như bạn đang làm
* Bạn cũng không gán lại kết quả → vô nghĩa

---

## 5. ❌ Logic sai

```js
item.name === exits_item
```

* So sánh string với array → luôn sai

---

## 6. ❌ Không xử lý case thêm mới

* Bạn chưa có `push()` khi item chưa tồn tại

---

# ✅ Lời giải đúng

```js
function createCart() {
    let listItem = [];

    return {
        addItem(name, price, quantity) {
            const existingItem = listItem.find(item => item.name === name);

            if (existingItem) {
                // Cập nhật trực tiếp object (do reference)
                existingItem.quantity += quantity;
            } else {
                // Thêm mới
                listItem.push({ name, price, quantity });
            }
        },

        // (phục vụ test/debug)
        listItems() {
            return listItem;
        }
    };
}
```

---

# 🧪 Ví dụ test

```js
const cart = createCart();

cart.addItem("apple", 10, 2);
cart.addItem("apple", 10, 3);

console.log(cart.listItems());
```

👉 Kết quả:
[
  { name: "apple", price: 10, quantity: 5 }
]
```

---

# 🧠 Điểm quan trọng bạn cần nhớ

* `find()` → tìm 1 phần tử (trả về reference)
* Object trong array → **truyền theo reference**
* Không cần `map` khi chỉ update 1 phần tử
* Closure giúp bảo vệ dữ liệu

