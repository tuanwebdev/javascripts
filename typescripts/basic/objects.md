
## Object Types là gì?
Trong TypeScript, object types dùng để **mô tả hình dạng (shape)** của một object: nó có những thuộc tính gì, mỗi thuộc tính có kiểu dữ liệu gì.

Có 3 cách để định nghĩa object type:

```typescript
// Cách 1: Ẩn danh (anonymous) - dùng trực tiếp
function greet(person: { name: string; age: number }) {
  return "Hello " + person.name;
}

// Cách 2: Interface (phổ biến nhất)
interface Person {
  name: string;
  age: number;
}

// Cách 3: Type alias
type Person = {
  name: string;
  age: number;
};
```

---

## Các "Công Cụ" Quan Trọng Trên Object Type

### 1. Thuộc Tính Tùy Chọn (`?`)
Đánh dấu thuộc tính có thể có hoặc không. Khi truy cập, giá trị có thể là `undefined`.
```typescript
interface PaintOptions {
  shape: Shape;
  xPos?: number; // Có thể có hoặc không
  yPos?: number;
}
```

### 2. Thuộc Tính Chỉ Đọc (`readonly`)
Ngăn không cho gán lại thuộc tính sau khi khởi tạo. **Chú ý:** Chỉ ngăn gán lại thuộc tính, không ngăn thay đổi nội dung bên trong nếu đó là object.
```typescript
interface Home {
  readonly resident: { name: string; age: number };
}

let home: Home = { resident: { name: "Alice", age: 30 } };
home.resident.age++; // ✅ Được phép
home.resident = { name: "Bob", age: 25 }; // ❌ Lỗi
```

### 3. Chữ Ký Chỉ Mục (Index Signature)
Dùng khi bạn **không biết trước tên thuộc tính**, nhưng biết kiểu của giá trị.
```typescript
interface StringArray {
  [index: number]: string; // Mọi key là number đều trả về string
}
```

---

## Mở Rộng và Kết Hợp Type

### Extending Types (`extends`)
Cho phép interface kế thừa từ interface khác, tránh lặp code.
```typescript
interface BasicAddress {
  street: string;
  city: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string; // Chỉ cần thêm thuộc tính mới
}
```

### Intersection Types (`&`)
Kết hợp nhiều type thành một type mới có tất cả thuộc tính.
```typescript
interface Colorful { color: string }
interface Circle { radius: number }

type ColorfulCircle = Colorful & Circle;
// { color: string; radius: number }
```

**Phân biệt `extends` và `&`:** Khi có thuộc tính trùng tên:
*   `extends`: Báo lỗi nếu kiểu không tương thích.
*   `&`: Tự động gộp kiểu, có thể tạo ra kiểu `never` (nếu không thể vừa là `string` vừa là `number`).

---

## Generic Object Types (Quan Trọng)
Cho phép tạo type "tổng quát", có thể tái sử dụng với nhiều kiểu dữ liệu khác nhau.

```typescript
// Định nghĩa một "khuôn" Box tổng quát
interface Box<Type> {
  contents: Type;
}

// Sử dụng
let stringBox: Box<string> = { contents: "hello" };
let numberBox: Box<number> = { contents: 42 };
```
Các kiểu quen thuộc như `Array<string>`, `Promise<number>` đều là generic types.

---

## Tuple Types (Kiểu Bộ)
Là mảng có **số lượng phần tử cố định** và **kiểu dữ liệu cụ thể cho từng vị trí**.

```typescript
type StringNumberPair = [string, number];
let pair: StringNumberPair = ["hello", 42]; // ✅ OK
// let pair2: StringNumberPair = [42, "hello"]; // ❌ Sai thứ tự kiểu
```

Tuple cũng hỗ trợ:
*   **Optional elements**: `[number, number, number?]`
*   **Rest elements**: `[string, number, ...boolean[]]`
*   **Readonly tuple**: `readonly [string, number]`

---

## Kiểm Tra Thuộc Tính Thừa (Excess Property Checks)
TypeScript **báo lỗi nếu bạn truyền object literal có thuộc tính không có trong type**. Đây là cơ chế bắt lỗi chính tả hữu ích.
```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

// ❌ Lỗi: 'colour' không tồn tại trong SquareConfig
createSquare({ colour: "red", width: 100 });
```

---

## Tóm Lại
| Khái niệm | Mục đích chính |
|-----------|----------------|
| **Optional (`?`)** | Thuộc tính có thể không có |
| **`readonly`** | Thuộc tính không thể gán lại |
| **Index Signature** | Mô tả object có key động |
| **`extends`** | Kế thừa interface |
| **`&`** | Kết hợp nhiều type |
| **Generic Types** | Tạo type tổng quát, tái sử dụng |
| **Tuple** | Mảng có số lượng và kiểu cố định |
| **Excess Property Check** | Bắt lỗi thừa thuộc tính trong object literal |
