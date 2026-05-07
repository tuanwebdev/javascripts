# Đề bài

Viết một chương trình JavaScript để kiểm tra xem ngày `1 tháng 1`
của các năm trong khoảng từ `2014` đến `2050`
có rơi vào `Chủ nhật` hay không.

Nếu có, hãy in năm đó ra màn hình.

---

# Giải pháp

## Ý tưởng
- Duyệt qua từng năm từ `2014` đến `2050`
- Tạo ngày `1/1` cho từng năm
- Dùng `getDay()` để kiểm tra thứ
- Nếu kết quả bằng `0` thì đó là `Chủ nhật`

---

# Các bước thực hiện

## 1. Duyệt qua các năm

```javascript
for (let year = 2014; year <= 2050; year++)
```

- Chạy lần lượt từ năm `2014` đến `2050`

---

## 2. Tạo ngày 1 tháng 1 của từng năm

```javascript
const d = new Date(year, 0, 1);
```

Trong đó:
- `year` → năm hiện tại
- `0` → tháng 1
- `1` → ngày mùng 1

Lưu ý:
JavaScript đánh số tháng từ `0 → 11`

| Giá trị | Tháng |
|---|---|
| 0 | Tháng 1 |
| 1 | Tháng 2 |
| ... | ... |
| 11 | Tháng 12 |

---

## 3. Kiểm tra có phải Chủ nhật không

```javascript
d.getDay() === 0
```

`getDay()` trả về:

| Giá trị | Thứ |
|---|---|
| 0 | Chủ nhật |
| 1 | Thứ hai |
| 2 | Thứ ba |
| 3 | Thứ tư |
| 4 | Thứ năm |
| 5 | Thứ sáu |
| 6 | Thứ bảy |

Nếu bằng `0` thì ngày đó là `Chủ nhật`.

---

# Code hoàn chỉnh

```javascript
const findYearsWhereJanFirstIsSunday = () => {
    for (let year = 2014; year <= 2050; year++) {
        const d = new Date(year, 0, 1);

        if (d.getDay() === 0) {
            console.log(`1 January ${year} is a Sunday`);
        }
    }
};

findYearsWhereJanFirstIsSunday();
```