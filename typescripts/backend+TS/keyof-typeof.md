
## `keyof typeof` là gì?
Đây là sự kết hợp của hai từ khóa trong TypeScript để **lấy ra tất cả các key của một object hoặc enum**, tạo thành một **Union Type** chứa các key đó.

---

## Giải thích từng phần

### 1. `typeof` – Lấy kiểu của một giá trị
Trong TypeScript, `typeof` không chỉ hoạt động như JavaScript, mà còn có thể **lấy ra kiểu (type) của một biến hoặc object**.

```typescript
const book = {
  slug: 'retrofit',
  title: 'Retrofit Book',
  price: 28
};

// typeof book → lấy ra kiểu của object book
type Book = typeof book;
// Book = { slug: string; title: string; price: number; }
```

### 2. `keyof` – Lấy tất cả key của một kiểu
`keyof` nhận vào một **kiểu object** và trả về **Union Type** chứa tất cả các thuộc tính (key) của kiểu đó.

```typescript
type BookProps = keyof Book;
// BookProps = "slug" | "title" | "price"
```

---

## Kết hợp `keyof typeof` – "Làm một lần cả hai bước"

Thay vì tạo type trung gian `Book`, bạn có thể kết hợp trực tiếp `keyof typeof` trên object gốc:

```typescript
const book = {
  slug: 'retrofit',
  title: 'Retrofit Book',
  price: 28
};

// keyof typeof book: lấy kiểu của book, rồi lấy tất cả key
type BookProps = keyof typeof book;
// BookProps = "slug" | "title" | "price"
```

**Ý nghĩa:** Bạn không cần phải định nghĩa interface/type thủ công. Chỉ cần có object thực tế, TypeScript sẽ tự suy ra các key hợp lệ.

---

## Ứng dụng với Enum (Pattern cực kỳ phổ biến)

Bài viết nhấn mạnh rằng `keyof typeof` hoạt động giống hệt với enum:

```typescript
export enum FutureStudioBooks {
  retrofit = 'Retrofit Book',
  picasso = 'Picasso Book',
  glide = 'Glide Book',
}

// Lấy ra union type của tất cả key trong enum
type FutureStudioBooksKey = keyof typeof FutureStudioBooks;
// "retrofit" | "picasso" | "glide"
```

---

## Tóm lại

| Cú pháp | Ý nghĩa | Kết quả |
|---------|---------|---------|
| `typeof obj` | Lấy kiểu của object | `{ slug: string; title: string; ... }` |
| `keyof Type` | Lấy tất cả key của Type | `"slug" \| "title" \| "price"` |
| `keyof typeof obj` | Làm cả hai trong một bước | `"slug" \| "title" \| "price"` |

**Khi nào dùng?** Khi bạn cần tạo một Union Type từ chính các key của một object hoặc enum có sẵn, giúp code linh hoạt và an toàn kiểu hơn.
