
## Union Type là gì?
**Union Type** cho phép một biến hoặc tham số có thể **chứa một trong nhiều kiểu dữ liệu** khác nhau. Cú pháp dùng dấu `|` (pipe).

```typescript
let value: number | string;
value = 42;      // ✅ Hợp lệ
value = "Hello"; // ✅ Cũng hợp lệ
```

---

## 1. Kiểm Tra Kiểu Trước Khi Dùng (Thu hẹp kiểu)
Vì biến có thể mang nhiều kiểu, bạn cần **kiểm tra kiểu** trước khi dùng các phương thức đặc thù.

```typescript
function printId(id: number | string) {
  if (typeof id === "number") {
    console.log("ID là số:", id);          // Dùng như number
  } else {
    console.log("ID là chuỗi:", id.toUpperCase()); // Dùng như string
  }
}

printId(123);   // "ID là số: 123"
printId("abc"); // "ID là chuỗi: ABC"
```

---

## 2. Ứng Dụng Thực Tế

### a) Xử lý dữ liệu đầu vào đa dạng
```typescript
function formatInput(input: string | number) {
  if (typeof input === "string") {
    return input.trim();       // Xóa khoảng trắng
  } else {
    return input.toFixed(2);   // Làm tròn 2 chữ số thập phân
  }
}
```

### b) Xử lý phản hồi API (Pattern cực kỳ phổ biến)
Đây là ví dụ **quan trọng nhất** bạn sẽ gặp trong React:

```typescript
type SuccessResponse = {
  status: "success";
  data: any;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

// Union Type: phản hồi có thể là thành công HOẶC thất bại
type ApiResponse = SuccessResponse | ErrorResponse;

function handleResponse(response: ApiResponse) {
  if (response.status === "success") {
    console.log("Dữ liệu:", response.data);
  } else {
    console.error("Lỗi:", response.message);
  }
}
```
**Điểm mạnh:** TypeScript biết nếu `status === "success"` thì `response` chắc chắn có `data`, nếu không thì chắc chắn có `message`. Đây gọi là **Discriminated Union** (Union có phân biệt).

---

## 3. Kết Hợp Với Object – Discriminated Union
Pattern này giúp code an toàn tuyệt đối khi xử lý các hình dạng khác nhau:

```typescript
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; size: number };

type Shape = Circle | Square;

function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius ** 2; // ✅ Truy cập radius an toàn
  } else {
    return shape.size ** 2;             // ✅ Truy cập size an toàn
  }
}
```

---

## Best Practices (Nguyên tắc vàng)
1.  **Dùng có mục đích**: Đừng lạm dụng Union Type khi không cần thiết, code sẽ khó đọc.
2.  **Luôn kiểm tra kiểu**: Dùng `typeof` hoặc `property check` trước khi thao tác.
3.  **Kết hợp với `kind`/`status`**: Tạo **Discriminated Union** để TypeScript tự động hiểu đúng kiểu.

---

## Tóm Lại
| Khái niệm | Ý nghĩa |
|-----------|---------|
| **Union Type** | Biến có thể là **một trong nhiều kiểu** |
| **Cú pháp** | `string \| number` |
| **Thu hẹp kiểu** | Dùng `typeof`, `if/else` để xử lý đúng kiểu |
| **Discriminated Union** | Dùng thuộc tính chung (`kind`, `status`) để phân biệt |