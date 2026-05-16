
### TypeScript là gì?
TypeScript (TS) là "phiên bản nâng cấp" của JavaScript (JS), bổ sung thêm **kiểu dữ liệu**. Code TS sẽ được biên dịch ra JS để chạy trên trình duyệt, vì trình duyệt chỉ hiểu JS. Việc khai báo kiểu giúp bạn phát hiện lỗi sớm và code rõ ràng hơn.

### Các Kiểu Dữ Liệu Cơ Bản

Dưới đây là bảng tổng hợp các kiểu dữ liệu chính.

| Kiểu | Mô tả ngắn gọn | Ví dụ khai báo |
| :--- | :--- | :--- |
| **`boolean`** | Giá trị đúng/sai (`true`/`false`) | `let isDone: boolean = false;` |
| **`number`** | Tất cả các loại số (số nguyên, thực, nhị phân, bát phân...) | `let decimal: number = 6;` |
| **`string`** | Dữ liệu văn bản, có thể dùng Template String | `let color: string = "blue";` |
| **`Array`** | Mảng chứa các giá trị, có 2 cách viết | `let list: number[] = [1, 2, 3];` |
| **`Tuple`** | Mảng với **số lượng và kiểu** của từng phần tử được cố định | `let x: [string, number];` |
| **`Enum`** | Đặt tên thân thiện cho tập các giá trị số (tăng tính rõ ràng) | `enum Color {Red, Green, Blue}` |
| **`Any`** | "Thoát" khỏi kiểm tra kiểu, dùng khi chưa biết chắc kiểu dữ liệu | `let notSure: any = 4;` |
| **`Void`** | Thường dùng cho hàm **không trả về** giá trị gì | `function warnUser(): void {}` |

---

### Giải thích chi tiết hơn qua các điểm đáng chú ý

**1. String và Template String**
Bạn có thể dùng backtick `` ` `` để tạo **Template String**, cho phép nhúng biểu thức `${...}` vào chuỗi rất tiện lợi:
```typescript
let age: number = 37;
let sentence: string = `Năm sau tôi sẽ ${age + 1} tuổi.`; // "Năm sau tôi sẽ 38 tuổi."
```

**2. Enum - "Danh sách hằng số có tên"**
Đây là cách tuyệt vời để làm việc với các nhóm giá trị cố định, giúp code dễ đọc hơn.
```typescript
enum Color {Red, Green, Blue} // Mặc định Red=0, Green=1, Blue=2
let c: Color = Color.Green; // c = 1

// Mẹo: Bạn cũng có thể lấy tên từ giá trị số
let colorName: string = Color[2]; // "Blue"
```

**3. Tuple - Mảng "khóa kiểu"**
Khác với mảng thông thường (các phần tử cùng kiểu), Tuple cho phép bạn định nghĩa một mảng có thứ tự và kiểu dữ liệu cố định cho từng vị trí.
```typescript
let employee: [number, string] = [1, "Steve"]; // Đúng: phần tử đầu là number, thứ hai là string
// employee = ["Steve", 1]; // Sai thứ tự -> Báo lỗi!
```

**4. Any và Void**
*   **`any`** là "con dao đa năng", dùng khi bạn thực sự không biết trước kiểu dữ liệu (dữ liệu từ API phức tạp, user input...). Lạm dụng `any` sẽ làm mất đi sức mạnh của TypeScript.
*   **`void`** gần như chỉ dùng để khai báo một hàm không có câu lệnh `return` giá trị.

**5. Null và Undefined**
Trong TypeScript, `null` và `undefined` cũng là các kiểu dữ liệu riêng biệt và mặc định bạn có thể gán chúng cho các biến thuộc kiểu khác (ví dụ: `let num: number = null;`).
