
## 1. Điểm Khác Biệt Chính So Với JavaScript
TypeScript cho phép bạn **khai báo kiểu dữ liệu** cho tham số và giá trị trả về của hàm, giúp phát hiện lỗi sớm.

```typescript
// Cú pháp tổng quát
function tênHàm(thamSố: kiểuDữLiệu): kiểuTrảVề {
    // code
}
```

## 2. Các Ví Dụ Cốt Lõi

### Hàm Cơ Bản (Có Kiểu)
```typescript
// Hàm nhận 2 số, trả về số
function add(a: number, b: number): number {
    return a + b;
}

add(10, 20); // ✅ OK
add('10', '20'); // ❌ Lỗi: Argument of type 'string' is not assignable to parameter of type 'number'
```
Trình biên dịch sẽ kiểm tra từng đối số bạn truyền vào, đảm bảo chúng là số.

### Hàm Không Trả Về Giá Trị (`void`)
```typescript
function echo(message: string): void {
    console.log(message.toUpperCase());
}
// Hàm này chỉ in ra, không có return
```

### TypeScript Tự Đoán Kiểu Trả Về (Type Inference)
Nếu bạn không ghi kiểu trả về, TypeScript sẽ cố gắng tự suy luận:
```typescript
function add(a: number, b: number) {
    return a + b; // TS tự hiểu kiểu trả về là 'number'
}
```
Tuy nhiên, nên khai báo rõ ràng để code dễ đọc và tránh sai sót.

## 3. Các Cách Khai Báo Hàm
TypeScript hỗ trợ nhiều cách viết hàm, bao gồm cả arrow function:

```typescript
// Cách 1: Hàm khai báo thông thường
function add(x: number, y: number): number {
    return x + y;
}

// Cách 2: Hàm biểu thức (function expression)
let add2 = function(x: number, y: number): number {
    return x + y;
};

// Cách 3: Arrow function - đầy đủ
let add3 = (x: number, y: number): number => { return x + y; };

// Cách 4: Arrow function - gọn hơn (tự suy luận kiểu trả về)
let add4 = (x: number, y: number) => { return x + y; };

// Cách 5: Arrow function - gọn nhất (bỏ {} và return)
let add5 = (x: number, y: number) => x + y;

// Cách 6: Khai báo kiểu cho biến, gán hàm sau
let add6: (a: number, b: number) => number = 
    function(x, y) {
        return x + y;
    };
```

## Tóm Lại
| Đặc điểm | Mô tả |
|----------|-------|
| **Tham số có kiểu** | Bắt buộc đúng kiểu khi gọi hàm |
| **Kiểu trả về** | `number`, `string`, `void`... hoặc để TS tự suy |
| **Arrow function** | Dùng `=>` giữa tham số và kiểu trả về |
