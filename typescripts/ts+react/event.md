
## 📌 Tổng quan nhanh
Bài viết giới thiệu hai cách chính để xử lý sự kiện (onClick, onChange, onSubmit...) trong React khi dùng TypeScript:
1.  **Cách 1 (Phổ biến)**: Khai báo kiểu cho **hàm xử lý sự kiện**.
2.  **Cách 2 (Dùng kiểu của thẻ HTML)**: Khai báo kiểu cho **toàn bộ thẻ JSX**, từ đó TypeScript tự suy ra kiểu cho sự kiện.

## 🎯 Cách 1: Xử lý kiểu cho Hàm Xử Lý Sự Kiện
Đây là cách bạn sẽ gặp nhiều nhất. Bạn tạo một hàm handler riêng và dùng các type có sẵn của React như `React.ChangeEvent`, `React.MouseEvent`...

### Các Kiểu Event Thường Gặp

| Sự kiện | Kiểu TypeScript cho Event | Kiểu cho Element |
| :--- | :--- | :--- |
| `onClick` | `React.MouseEvent` | `HTMLButtonElement` / `HTMLDivElement`... |
| `onChange` (input) | `React.ChangeEvent` | `HTMLInputElement` |
| `onSubmit` (form) | `React.FormEvent` | `HTMLFormElement` |
| `onKeyDown` | `React.KeyboardEvent` | `HTMLInputElement` |

### Ví dụ minh họa
```tsx
const App = () => {
  // 1. Xử lý onChange cho input (phổ biến nhất)
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value); // ✅ An toàn, biết e.target là HTMLInputElement
  };

  // 2. Xử lý onClick cho button
  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    e.preventDefault();
    console.log('Clicked!');
  };

  // 3. Xử lý onSubmit cho form
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log('Form submitted');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
      <button onClick={handleClick}>Submit</button>
    </form>
  );
};
```

## 🚀 Cách 2: Khai Báo Kiểu Cho Cả Thẻ JSX
Cách này "độc đáo" hơn, bạn sử dụng kiểu của thẻ HTML gốc (như `React.InputHTMLAttributes`) và dùng `...rest` để xử lý sự kiện ngay trên thẻ. Cách này rất hữu ích khi tạo component `<Input />` tùy chỉnh thừa kế mọi thuộc tính của thẻ `<input>` gốc.

```tsx
// Định nghĩa component Input nhận mọi props của thẻ <input>
type InputProps = React.InputHTMLAttributes<HTMLInputElement>;

const Input = (props: InputProps) => {
  // Bạn có thể destructure ra các event handler từ props
  const { onChange, ...rest } = props;
  
  return <input onChange={onChange} {...rest} />;
};

// Sử dụng:
const App = () => {
  return (
    <Input
      placeholder="Nhập tên"
      onChange={(e) => console.log(e.target.value)} // e tự hiểu là React.ChangeEvent<HTMLInputElement>
    />
  );
};
```

## 💡 So sánh & Khuyên dùng

- **Cách 1** (Khai báo kiểu cho hàm): Trực quan, dễ đọc, dùng khi bạn cần viết logic xử lý phức tạp bên trong handler. **Đây là cách dùng hàng ngày.**
- **Cách 2** (Khai báo kiểu cho thẻ): Gọn gàng, mạnh mẽ, dùng khi bạn tạo component "wrapper" muốn kế thừa toàn bộ props của một thẻ HTML gốc.
