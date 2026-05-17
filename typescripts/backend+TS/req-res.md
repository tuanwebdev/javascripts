
## 1. Import đúng Type từ Express
Luôn import sẵn các type cần dùng:
```typescript
import { Request, Response } from 'express';
```

---

## 2. `Request` (req) – 4 điều cần nhớ

### a) Truy cập dữ liệu phổ biến
```typescript
app.post('/user', (req: Request, res: Response) => {
  const body = req.body;        // Dữ liệu từ form/JSON (cần express.json() middleware)
  const params = req.params;    // /user/:id → req.params.id
  const query = req.query;      // /user?name=Alice → req.query.name
  const headers = req.headers;  // req.headers.authorization
});
```

### b) `req.body` mặc định là `any` – Nên tạo kiểu riêng
```typescript
interface CreateUserBody {
  name: string;
  age: number;
}

app.post('/user', (req: Request<{}, {}, CreateUserBody>, res: Response) => {
  // Bây giờ req.body.name và req.body.age đã có kiểu rõ ràng
});
```
Cú pháp `Request<Params, ResBody, Body>` hơi dài, nhưng đây là cách chính thống để gõ chặt `req.body`.

### c) Mở rộng `req` (thêm `currentUser`...) – Dùng `declare global`
Đây là pattern quan trọng khi middleware gắn thêm dữ liệu:
```typescript
declare global {
  namespace Express {
    interface Request {
      currentUser?: { id: number; name: string };
    }
  }
}
// Sau đó dùng thoải mái: req.currentUser
```

### d) `req.params` cần khai báo kiểu
```typescript
app.get('/user/:id', (req: Request<{ id: string }>, res: Response) => {
  console.log(req.params.id); // id là string
});
```

---

## 3. `Response` (res) – 3 điều cần nhớ

### a) Các phương thức trả về phổ biến
```typescript
app.get('/data', (req: Request, res: Response) => {
  res.json({ ok: true });           // Trả JSON
  res.status(404).json({ msg: 'Not found' }); // Kèm status code
  res.send('Hello');                // Trả text/HTML
  res.redirect('/login');           // Chuyển hướng
});
```

### b) Khai báo kiểu cho body trả về (ít dùng hơn)
```typescript
interface UserResponse {
  id: number;
  name: string;
}

app.get('/user/:id', (req: Request<{ id: string }>, res: Response<UserResponse>) => {
  res.json({ id: 1, name: 'Alice' }); // TypeScript kiểm tra đúng kiểu
});
```

### c) Luôn dùng `res.json()` hoặc `res.send()` – Tránh quên `return`
Khi đã gọi `res.json()`, không cần `return` nhưng nên đảm bảo code không chạy tiếp nếu không mong muốn.

---

## Tóm tắt "bỏ túi"

| Thứ cần nhớ | Cú pháp |
|-------------|---------|
| Import type | `import { Request, Response } from 'express'` |
| Truy cập body | `req.body` (nên tạo interface riêng) |
| Truy cập params | `Request<{ id: string }>` |
| Truy cập query | `req.query.name` |
| Mở rộng req | `declare global { namespace Express { interface Request { ... } } }` |
| Trả JSON | `res.json(data)` |
| Trả lỗi | `res.status(404).json({ error: '...' })` |
