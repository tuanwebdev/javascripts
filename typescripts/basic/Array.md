
### Mảng trong TypeScript là gì?
Về cơ bản, mảng trong TypeScript hoạt động giống JavaScript, nhưng có thêm **chú thích kiểu dữ liệu (Type Annotation)** để đảm bảo tất cả phần tử trong mảng đều có cùng một kiểu.

---

### 1. Khai Báo Mảng với Kiểu Dữ Liệu
Có 2 cách chính để khai báo một mảng:

**Cách 1: Dùng cặp dấu ngoặc vuông `[]` (Phổ biến và ngắn gọn nhất)**
```typescript
let numbers: number[] = [1, 2, 3];
let names: string[] = ["Alice", "Bob"];
```

**Cách 2: Dùng Generic `Array<type>`**
```typescript
let numbers: Array<number> = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];
```

---

### 2. Lợi Ích của Việc Khai Báo Kiểu
Khi bạn đã khai báo kiểu cho mảng, TypeScript sẽ **kiểm tra và báo lỗi** nếu bạn cố gắng thêm phần tử sai kiểu.

```typescript
let names: string[] = ["Alice"];
names.push("Bob");   // ✅ Hợp lệ
// names.push(10);   // ❌ Lỗi: Argument of type 'number' is not assignable to parameter of type 'string'
```

---

### 3. Mảng Đa Kiểu (Multi-Type Array)
Đôi khi bạn cần một mảng chứa nhiều kiểu dữ liệu khác nhau. TypeScript hỗ trợ 2 cách:

**Cách 1: Union Type (`|`)**
```typescript
// Mảng có thể chứa string hoặc number
let mixed: (string | number)[] = ["Alice", 25, "Bob", 30];
```

**Cách 2: Dùng `any` (Không khuyến khích)**
```typescript
// Mất hoàn toàn việc kiểm tra kiểu, giống như JavaScript thuần
let anything: any[] = [1, "hello", true];
```

---

### 4. Mảng Chỉ Đọc (`readonly`)
Ngăn chặn mọi thao tác thay đổi mảng sau khi khởi tạo (như `push`, `pop`, gán lại phần tử...).

```typescript
let colors: readonly string[] = ["red", "green"];
// colors.push("blue");  // ❌ Lỗi: Property 'push' does not exist on type 'readonly string[]'
// colors[0] = "yellow"; // ❌ Lỗi: Index signature in type 'readonly string[]' only permits reading
```

---

### Tóm Tắt Nhanh
| Mục đích | Cú pháp |
|----------|---------|
| Mảng kiểu cố định | `let arr: string[] = [];` |
| Generic | `let arr: Array<string> = [];` |
| Mảng đa kiểu | `let arr: (string | number)[] = [];` |
| Mảng chỉ đọc | `let arr: readonly string[] = [];` |

