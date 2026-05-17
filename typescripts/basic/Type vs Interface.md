
## Type và Interface là gì?
Cả hai đều thuộc nhóm **User Defined Types** – tức là kiểu do người dùng tự định nghĩa. Về cơ bản, chúng có cú pháp rất giống nhau:

```typescript
// Type
type Shape = {
  name: string;
  color: string;
};

// Interface
interface Shape {
  name: string;
  color: string;
}
```

Tuy nhiên, có những **khác biệt quan trọng** sau:

---

## 5 Điểm Khác Biệt Chính

### 1. Interface có thể "Merge" – Type thì không
Nếu khai báo **cùng tên 2 lần**, Interface sẽ tự động gộp lại, còn Type sẽ báo lỗi trùng tên.

```typescript
// ✅ Interface: tự động merge
interface Shape { name: string; }
interface Shape { color: string; }
// Kết quả: Shape = { name: string; color: string }

// ❌ Type: báo lỗi "Duplicate identifier"
type Shape = { name: string; }
type Shape = { color: string; } // Lỗi!
```

**Khi nào hữu ích?** Khi bạn viết thư viện và muốn người dùng có thể mở rộng thêm thuộc tính.

---

### 2. Type dùng được "Computed Properties" – Interface thì không
Type có thể dùng **mapped types** với `[key in ...]`, Interface không hỗ trợ cú pháp này.

```typescript
type Keys = 'color' | 'name';

// ✅ Type: dùng được
type ShapeType = {
  [key in Keys]: string;
};

// ❌ Interface: lỗi
interface ShapeInterface {
  [key in Keys]: string; // Lỗi!
}
```

---

### 3. Tuple Type: Cùng khai báo nhưng kiểm tra khác nhau
```typescript
type Tuple = [number, number];
interface ITuple {
  0: number;
  1: number;
}

// Type: báo lỗi nếu thừa phần tử
[1, 2, 3] as Tuple; // ⚠️ Cảnh báo

// Interface: không báo lỗi
[1, 2, 3] as ITuple; // ✅ OK
```

---

### 4. Type có Union Types – Interface thì không
Đây là **ưu thế lớn nhất của Type**. Bạn có thể định nghĩa kiểu là tập hợp của nhiều giá trị/kiểu.

```typescript
// ✅ Type: Union
type Colors = 'blue' | 'green' | 'red';

// ❌ Interface: không làm được điều này
```

---

### 5. Khi nào nên dùng gì? (Theo kinh nghiệm từ bài viết)

| Trường hợp | Nên dùng |
|------------|----------|
| Viết thư viện, cung cấp public API | **Interface** (để người khác có thể merge, mở rộng) |
| Cần Union types, Computed properties | **Type** |
| Cần Tuple chặt chẽ | **Type** |
| Còn lại không rõ ràng | Cả hai đều được, tùy thói quen cá nhân |

---

## Tóm Lại
| Đặc điểm | Interface | Type |
|----------|-----------|------|
| Merge (gộp) được | ✅ Có | ❌ Không |
| Computed Properties | ❌ Không | ✅ Có |
| Union Types | ❌ Không | ✅ Có |
| Tuple chặt chẽ | ⚠️ Lỏng hơn | ✅ Chặt hơn |

**Lời khuyên thực tế:** Hầu hết các dự án React/TypeScript hiện nay dùng cả hai. Nếu không cần merge hay union, chọn cái nào cũng được – nhưng nên thống nhất một phong cách trong dự án.
