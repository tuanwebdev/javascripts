

## Middleware trong Express là gì?
Middleware là các hàm được chạy trong quá trình xử lý request. Chúng có thể:
- Thực thi bất kỳ code nào.
- Thay đổi object `request` (`req`) và `response` (`res`).
- Kết thúc vòng đời request-response.
- Gọi middleware tiếp theo trong chuỗi.

## Các cách thêm TypeScript cho Middleware

Có 3 cách chính để định nghĩa kiểu cho một middleware trong Express:

### Cách 1: Sử dụng `RequestHandler` (Phổ biến nhất cho người mới)
Đây là cách đơn giản nhất, dùng type có sẵn `RequestHandler` từ Express.

```typescript
import express, { RequestHandler } from 'express';

const app = express();

// Định nghĩa middleware với type RequestHandler
const myLogger: RequestHandler = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next(); // Gọi next() để chuyển sang middleware tiếp theo
};

// Sử dụng middleware
app.use(myLogger);

app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

### Cách 2: Khai báo riêng kiểu cho Request, Response, NextFunction
Cách này rõ ràng và đầy đủ nhất, giúp bạn kiểm soát tốt từng tham số.

```typescript
import express, { Request, Response, NextFunction } from 'express';

const app = express();

// Middleware xác thực giả lập
const authMiddleware = (req: Request, res: Response, next: NextFunction) => {
  const token = req.headers.authorization;

  if (token === 'secret-token') {
    console.log('Xác thực thành công!');
    next(); // Hợp lệ -> đi tiếp
  } else {
    res.status(401).json({ message: 'Không có quyền truy cập!' });
    // Không gọi next() -> dừng chuỗi middleware
  }
};

app.use(authMiddleware);
```

### Cách 3: Mở rộng (Extend) Request để thêm thuộc tính mới
Đây là trường hợp rất thực tế khi middleware của bạn cần "gắn" thêm dữ liệu vào object `req` (ví dụ: thông tin user sau khi xác thực).

```typescript
import express, { Request, Response, NextFunction } from 'express';

// Bước 1: Mở rộng interface Request của Express
// Đây là cách TypeScript "khai báo thêm" thuộc tính cho Request
declare global {
  namespace Express {
    interface Request {
      currentUser?: { id: number; name: string };
    }
  }
}

// Bước 2: Tạo middleware để gán giá trị cho currentUser
const attachUserMiddleware = (req: Request, res: Response, next: NextFunction) => {
  // Giả lập lấy user từ database
  req.currentUser = { id: 1, name: 'Alice' };
  next();
};

// Bước 3: Sử dụng middleware
app.use(attachUserMiddleware);

app.get('/profile', (req, res) => {
  // TypeScript hiểu req.currentUser có thể là { id: number; name: string } hoặc undefined
  res.json({ user: req.currentUser });
});
```

### Cách 4: Middleware xử lý lỗi (Error Handling Middleware)
Middleware xử lý lỗi có 4 tham số, trong đó tham số đầu tiên là `err`.

```typescript
import express, { Request, Response, NextFunction } from 'express';

const errorHandler = (
  err: Error, // Lưu ý: err là Error
  req: Request,
  res: Response,
  next: NextFunction
) => {
  console.error('Lỗi:', err.message);
  res.status(500).json({ error: 'Đã có lỗi xảy ra từ phía máy chủ!' });
};

// Đặt middleware xử lý lỗi ở cuối cùng sau tất cả các routes
app.use(errorHandler);
```

## Tóm tắt các type cần nhớ

| Type của Express | Dùng cho |
| :--- | :--- |
| `Request` | Tham số `req` của middleware |
| `Response` | Tham số `res` của middleware |
| `NextFunction` | Tham số `next` của middleware |
| `RequestHandler` | Type tổng hợp cho cả một hàm middleware `(req, res, next) => void` |