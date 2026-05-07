# JavaScript Exercise — Display Current Day and Time

## 📝 Đề bài

Viết chương trình JavaScript để hiển thị ngày và thời gian hiện tại theo format:

```txt
Today is : Tuesday.
Current time is : 10 PM : 30 : 38
```

---

# 💡 Ý tưởng giải

## Bước 1: Tạo thời gian hiện tại

Sử dụng object `Date`.

```js
const date = new Date();
```

---

## Bước 2: Lấy thứ hiện tại

`getDay()` trả về số từ `0 → 6`

| Giá trị | Thứ |
|---|---|
| 0 | Sunday |
| 1 | Monday |
| 2 | Tuesday |
| 3 | Wednesday |
| 4 | Thursday |
| 5 | Friday |
| 6 | Saturday |

Ta cần dùng array để map số → tên thứ.

```js
const daylist = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday"
];
```

---

## Bước 3: Lấy giờ phút giây

```js
date.getHours();
date.getMinutes();
date.getSeconds();
```

---

## Bước 4: Xử lý AM / PM

Nếu giờ lớn hơn hoặc bằng `12`
→ `PM`

Ngược lại
→ `AM`

```js
const pmam = hour >= 12 ? "PM" : "AM";
```

---

## Bước 5: Chuyển hệ 24h → 12h

Ví dụ:

| 24h | 12h |
|---|---|
| 13 | 1 PM |
| 18 | 6 PM |
| 22 | 10 PM |

Sử dụng:

```js
hour = hour % 12;
```

---

## Bước 6: Xử lý trường hợp đặc biệt

```js
12 % 12 = 0
```

Nhưng ngoài đời không có:
```txt
0 PM
```

Nên phải đổi `0 → 12`

```js
if (hour === 0) {
    hour = 12;
}
```

---

# 🛠 Các hàm tiện ích được dùng

| Hàm | Chức năng |
|---|---|
| `new Date()` | Tạo thời gian hiện tại |
| `getDay()` | Lấy thứ trong tuần |
| `getHours()` | Lấy giờ |
| `getMinutes()` | Lấy phút |
| `getSeconds()` | Lấy giây |

---

# ✅ Code hoàn chỉnh

```js
const displayCurrentDayAndTime = () => {
    const date = new Date();

    const day = date.getDay();

    let hour = date.getHours();

    const minute = date.getMinutes();
    const second = date.getSeconds();

    const pmam = hour >= 12 ? "PM" : "AM";

    hour = hour % 12;

    if (hour === 0) {
        hour = 12;
    }

    const daylist = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
    ];

    console.log(`Today is: ${daylist[day]}`);

    console.log(
        `Current time is: ${hour} ${pmam}:${minute}:${second}`
    );
};

displayCurrentDayAndTime();
```

---

# 🎯 Kiến thức học được

- Object `Date`
- Làm việc với thời gian trong JavaScript
- Array mapping
- Template string
- Toán tử ternary `? :`
- Chuyển đổi giờ 24h → 12h
- Format output