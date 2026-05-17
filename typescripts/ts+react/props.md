
Khi làm việc với React và TypeScript, `props` chính là "xương sống" để truyền dữ liệu giữa các component. Việc khai báo kiểu cho `props` đảm bảo component của bạn hoạt động đúng đắn và an toàn hơn.

### 1. Interface vs. Type Aliases
Đây là hai cách phổ biến nhất để định nghĩa hình dạng của `props`.

```typescript
// Sử dụng Interface (Cách phổ biến, dễ mở rộng)
interface UserProps {
  name: string;
  age: number;
  isActive?: boolean; // Prop tùy chọn (optional)
}

// Sử dụng Type Alias
type UserPropsType = {
  name: string;
  age: number;
  isActive?: boolean;
};

// Sử dụng trong component
const UserCard = ({ name, age, isActive }: UserProps) => {
  return <div>{name} - {age} tuổi</div>;
};
```

### 2. Các Kiểu Dữ Liệu `props` Phổ Biến và Hữu Ích
TypeScript cho phép bạn kiểm soát rất chi tiết các giá trị mà component có thể nhận.

```typescript
interface AdvancedProps {
  // Các kiểu cơ bản
  title: string;
  count: number;

  // Kiểu Union: Chỉ chấp nhận một vài giá trị cụ thể
  status: 'loading' | 'success' | 'error';

  // Kiểu Function: Truyền callback
  onClick: (id: number) => void;
  onSave: (data: object) => Promise<void>;

  // Kiểu React Node: Để nhận JSX, string, null, v.v.
  children: React.ReactNode;

  // Kiểu CSS: Truyền style trực tiếp
  customStyle?: React.CSSProperties;
}

// Component ví dụ
const MyComponent = ({ status, onClick, children, customStyle }: AdvancedProps) => {
  // ... logic component
  return <div style={customStyle}>{children}</div>;
};
```

### 3. Generic Props: "Vũ khí" cho Component Siêu Linh Hoạt
Đây là cách bạn tạo ra các component có thể làm việc với nhiều kiểu dữ liệu khác nhau, ví dụ như một component `List`.

```typescript
// Định nghĩa component List dùng Generic
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

// Khi dùng, bạn chỉ định kiểu cụ thể cho T
interface User {
  id: number;
  name: string;
}

const users: User[] = [{ id: 1, name: 'Alice' }];

<List<User>
  items={users}
  renderItem={(user) => <div>{user.name}</div>} // TypeScript hiểu user là { id: number, name: string }
/>
```

### 4. Component Kế Thừa Props của Thẻ HTML Gốc
Đôi khi bạn muốn component `Input` của mình có tất cả thuộc tính của thẻ `<input>` gốc mà không cần khai báo lại.

```typescript
import { InputHTMLAttributes } from 'react';

// Kế thừa mọi prop của thẻ <input> và thêm prop 'label'
interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
}

const CustomInput = ({ label, ...restProps }: InputProps) => {
  return (
    <div>
      <label>{label}</label>
      <input {...restProps} /> {/* ...restProps bao gồm placeholder, type, value, onChange,... */}
    </div>
  );
};
```

### 5. Xử Lý Sự Kiện (Event Handlers) với Form
Đây là trường hợp cực kỳ thường gặp khi bạn làm việc với form.

```typescript
const FormComponent = () => {
  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    console.log(event.target.value); // An toàn, TypeScript biết event.target là HTMLInputElement
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault(); // Ngăn form reload trang
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
    </form>
  );
};
```

### Tóm tắt & Lời khuyên
| Chủ đề | Lợi ích |
| :--- | :--- |
| **Định nghĩa Props** | Code tự tin, an toàn, dễ tái sử dụng. |
| **Children, CSS, Style** | Kiểm soát giao diện và nội dung được truyền vào. |
| **Generic Props** | Tạo component siêu linh hoạt (List, Table, Dropdown...). |
| **Kế thừa HTML** | Không cần gõ lại toàn bộ props của thẻ gốc. |
| **Xử lý sự kiện** | Truy cập `event.target.value` chính xác, không sợ sai kiểu. |
