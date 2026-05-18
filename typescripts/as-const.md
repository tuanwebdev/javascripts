
## `as const` là gì?
Đây là một **const assertion** (khẳng định hằng số). Nó nói với TypeScript: **"Hãy coi giá trị này là bất biến (không thể thay đổi) và suy ra kiểu chính xác nhất, hẹp nhất có thể."**
---

## Vấn đề `as const` giải quyết

### ❌ Không có `as const`
```typescript
// TypeScript suy ra kiểu rộng hơn
const roles = ['admin', 'user'];           // type: string[]
const config = { theme: 'dark', version: 1 }; // type: { theme: string; version: number }
```
*   `roles` chỉ là `string[]`, bạn có thể push thêm, sửa phần tử.
*   `config.theme` chỉ là `string`, không phải giá trị cụ thể `'dark'`.

### ✅ Có `as const`
```typescript
const roles = ['admin', 'user'] as const;           // type: readonly ["admin", "user"]
const config = { theme: 'dark', version: 1 } as const; // type: { readonly theme: "dark"; readonly version: 1 }
```
*   `roles` trở thành **readonly tuple** với 2 giá trị chính xác.
*   `config.theme` có kiểu là `"dark"` (literal type), không phải `string` chung chung.

---

## 3 Ứng dụng thực tế quan trọng nhất

### 1. Tạo mảng hoặc object thực sự bất biến
```typescript
const COLORS = ['red', 'green', 'blue'] as const;
// COLORS.push('yellow'); // ❌ Lỗi: Property 'push' does not exist on type 'readonly ["red", "green", "blue"]'
// COLORS[0] = 'yellow';  // ❌ Lỗi: Cannot assign to '0' because it is a read-only property
```

### 2. Tạo Union Type từ mảng (Pattern rất hay dùng)
```typescript
const ROLES = ['admin', 'editor', 'viewer'] as const;
type Role = (typeof ROLES)[number]; // "admin" | "editor" | "viewer"

function checkRole(role: Role) {
  // Chỉ chấp nhận đúng 3 giá trị
}
checkRole('admin');  // ✅ OK
checkRole('guest');  // ❌ Lỗi
```
## Tóm lại bằng 1 câu
**`as const` biến giá trị thành "hằng số bất biến" với kiểu chính xác nhất, giúp code an toàn và TypeScript hiểu rõ ý định của bạn hơn.**
