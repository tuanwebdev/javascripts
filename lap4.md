# Bài 3: Lấy ngày tháng hiện tại ở nhiều định dạng khác nhau

## Yêu cầu đề bài

Viết chương trình JavaScript để lấy ngày tháng hiện tại.

Định dạng đầu ra mong muốn:

- `mm-dd-yyyy`
- `mm/dd/yyyy`
- `dd-mm-yyyy`
- `dd/mm/yyyy`

Ví dụ:

```javascript
05-07-2026
05/07/2026
07-05-2026
07/05/2026
```

---

# Những kiến thức cần dùng

## 1. Tạo ngày hiện tại

```javascript
const date = new Date();
```

---

## 2. Lấy năm hiện tại

```javascript
date.getFullYear()
```

Ví dụ:

```javascript
2026
```

---

## 3. Lấy tháng hiện tại

```javascript
date.getMonth()
```

⚠️ Lưu ý:

JavaScript đánh số tháng từ `0 → 11`

| Tháng thật | Giá trị JS |
|---|---|
| January | 0 |
| February | 1 |
| March | 2 |
| ... | ... |
| December | 11 |

Vì vậy phải cộng thêm `1`:

```javascript
date.getMonth() + 1
```

---

## 4. Lấy ngày trong tháng

```javascript
date.getDate()
```

Ví dụ:

```javascript
7
```

---

# Lỗi sai ban đầu

Bạn từng viết:

```javascript
if(currentMonth < 10 || currentDate < 10){
    return `0${currentDate}/0${currentMonth}/${currentYears}`;
}
```

## Vấn đề

Bạn thêm `0` cho cả ngày và tháng cùng lúc.

Ví dụ:

```javascript
currentDate = 15
currentMonth = 5
```

Kết quả sẽ là:

```javascript
015/05/2026
```

❌ Sai vì ngày `15` không cần thêm `0`.

---

# Cách xử lý đúng

Cần kiểm tra riêng:

- Nếu tháng < 10 → thêm `0`
- Nếu ngày < 10 → thêm `0`

---

# Lời giải hoàn chỉnh

```javascript
const getCurrentDateandMonth = () => {
    const date = new Date();

    const currentYear = date.getFullYear();

    let currentMonth = date.getMonth() + 1;
    let currentDate = date.getDate();

    // Thêm số 0 phía trước tháng
    if (currentMonth < 10) {
        currentMonth = '0' + currentMonth;
    }

    // Thêm số 0 phía trước ngày
    if (currentDate < 10) {
        currentDate = '0' + currentDate;
    }

    return `${currentDate}/${currentMonth}/${currentYear}`;
};

console.log(getCurrentDateandMonth());
```

---

# Output ví dụ

```javascript
07/05/2026
```

---

# Phiên bản ngắn gọn hơn (Ternary Operator)

```javascript
const getCurrentDateandMonth = () => {
    const date = new Date();

    const currentYear = date.getFullYear();

    let currentMonth = date.getMonth() + 1;
    let currentDate = date.getDate();

    currentMonth =
        currentMonth < 10
            ? '0' + currentMonth
            : currentMonth;

    currentDate =
        currentDate < 10
            ? '0' + currentDate
            : currentDate;

    return `${currentDate}/${currentMonth}/${currentYear}`;
};
```

---

# Tư duy quan trọng

Khi format dữ liệu:

- Không xử lý nhiều giá trị cùng lúc nếu logic khác nhau
- Mỗi giá trị nên được kiểm tra riêng
- Sau đó mới ghép thành chuỗi kết quả

Đây là cách viết code sạch và dễ bảo trì hơn.