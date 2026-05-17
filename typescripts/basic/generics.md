
## Generic Type là gì?
**Generic Type** cho phép bạn truyền **kiểu dữ liệu như một tham số** vào function, interface, class... Giúp code linh hoạt, tái sử dụng cao mà vẫn giữ được an toàn kiểu.

Hãy tưởng tượng nó như một "khuôn bánh": bạn có một cái khuôn, nhưng bạn có thể đổ bột sô-cô-la, bột vani... vào để tạo ra các loại bánh khác nhau.

---

## 1. Tại Sao Cần Generic? (Bài toán thực tế)

### ❌ Vấn đề khi không dùng Generic
```typescript
type NS = string | number;

function getTuple(a: NS, b: NS): [NS, NS] {
  return [a, b];
}

let result = getTuple('hello', 'world');
// result[0] chỉ có kiểu NS, KHÔNG thể gọi .toUpperCase()!
```

**Vấn đề:**
*   Không thể ràng buộc `a` và `b` **cùng kiểu** (có thể là string và number).
*   Giá trị trả về là `NS`, TypeScript không biết chính xác là `string` hay `number` → không dùng được phương thức đặc thù như `.toUpperCase()` hay `.toFixed()`.

### ✅ Giải pháp: Dùng Generic
```typescript
function getTuple<T>(a: T, b: T): [T, T] {
  return [a, b];
}

// Khi gọi, bạn "truyền kiểu" vào
let stringArray = getTuple<string>('hello', 'world'); // T = string
let numberArray = getTuple<number>(1.25, 2.56);       // T = number

// Bây giờ TypeScript hiểu chính xác kiểu!
stringArray[0].toUpperCase(); // ✅ OK
numberArray[0].toFixed();     // ✅ OK

// TypeScript cũng tự suy luận kiểu nếu bạn không truyền
getTuple('hello', 'world'); // TS tự hiểu T = string
getTuple(1.25, 'world');    // ❌ Lỗi: phải cùng kiểu
```

---

## 2. Các Loại Generic

### a) Generic Function
Truyền nhiều tham số kiểu nếu cần:
```typescript
let getTuple = <T, U>(a: T, b: U): [T, U] => [a, b];

let mixed = getTuple(1.25, 'world'); // ✅ [number, string]
```

### b) Generic Interface
```typescript
interface TupleObject<T, U> {
  a: T;
  b: U;
  getTuple(): [T, U];
}

let obj: TupleObject<string, number> = {
  a: '1',
  b: 3,
  getTuple: function() { return [this.a, this.b]; }
};
```

### c) Generic Class
```typescript
class Collection<T> {
  public items: T[];
  
  constructor(...values: T[]) {
    this.items = values;
  }

  getFirstItem(): T {
    return this.items[0];
  }
}

let letters = new Collection<string>('a', 'b', 'c');
let first = letters.getFirstItem(); // first: string
```

---

## 3. Ràng Buộc Generic (Quan trọng!)
Đôi khi bạn muốn giới hạn kiểu được phép truyền vào.

### a) `extends` – Ràng buộc kiểu cụ thể
```typescript
// Hàm merge chỉ nhận object
function merge<U extends object, V extends object>(obj1: U, obj2: V) {
  return { ...obj1, ...obj2 };
}

merge({ name: 'John' }, { age: 25 }); // ✅ OK
merge({ name: 'John' }, 25);          // ❌ Lỗi: 25 không phải object
```

### b) `extends keyof` – Ràng buộc là key của object
```typescript
function prop<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

prop({ name: 'John' }, 'name'); // ✅ OK
prop({ name: 'John' }, 'age');  // ❌ Lỗi: 'age' không phải key của object
```

---

## Tóm Lại
| Khái niệm | Ý nghĩa |
|-----------|---------|
| **Generic** | Truyền kiểu như tham số, tăng tái sử dụng |
| **`<T>`** | Tham số kiểu, đại diện cho kiểu bất kỳ |
| **`extends`** | Ràng buộc kiểu phải thỏa mãn điều kiện nào đó |
| **`extends keyof`** | Ràng buộc kiểu phải là key của một object |
