Dựa vào bài viết bạn cung cấp, tôi sẽ tóm tắt cách sử dụng TypeScript với React Hooks một cách dễ hiểu và có tổ chức nhất cho người mới.

### `useState`: Khai báo kiểu cho state

Đây là Hook bạn dùng hàng ngày. Để chỉ định kiểu cho state, bạn dùng cú pháp `useState<type>()`.

```typescript
// ✅ State kiểu string
const [inputValue, setInputValue] = React.useState<string>('');

// ✅ State có thể là string hoặc null
const [name, setName] = React.useState<string | null>(null);
```
Khi bạn gọi `setInputValue`, TypeScript sẽ đảm bảo bạn chỉ truyền vào đúng kiểu đã khai báo.

---

### `useRef`: Gắn vào DOM element

Hook này tạo một tham chiếu đến một phần tử DOM. Bạn cần chỉ định kiểu cho phần tử đó và khởi tạo là `null`.

```typescript
const mainRef = React.useRef<HTMLDivElement | null>(null);

// Sử dụng
return <div ref={mainRef} className="main">...</div>;
```
Bạn cần dùng `| null` vì lúc đầu component chưa render, ref có giá trị `null`.

---

### `useContext`: Dùng chung dữ liệu không cần truyền props

Với TypeScript, bạn cần chỉ định kiểu ngay khi **tạo Context**.

```typescript
// 1. Định nghĩa kiểu dữ liệu
interface User {
  name: string;
  age: number;
}

// 2. Tạo context với kiểu là mảng User (hoặc mảng rỗng)
const UserContext = React.createContext<User[] | []>([]);

// 3. Sử dụng trong component con
const ChildComponent: React.FC = () => {
  const userList = React.useContext<User[] | []>(UserContext);
  return <div>{userList.map(u => u.name)}</div>;
};
```

---

### `useReducer`: Quản lý state phức tạp (như Redux mini)

Đây là Hook cần setup kiểu cẩn thận nhất. Bạn cần định nghĩa kiểu cho **State**, **Action**, và **Reducer**.

#### Bước 1: Định nghĩa kiểu State và Action
```typescript
// Kiểu cho State
interface TypeState {
  count: number;
}

// Enum cho các loại Action (giúp gợi nhớ, tránh sai chính tả)
enum ActionTypes {
  INCREMENT = 'INCREMENT',
  DECREMENT = 'DECREMENT',
}

// Kiểu cho Action
interface TypeActions {
  type: ActionTypes;
  value: number; // Dữ liệu gửi kèm
}
```

#### Bước 2: Tạo Reducer và dùng trong Component
```typescript
// Reducer: Nhận state và action, trả về state mới
const reducer: React.Reducer<TypeState, TypeActions> = (state, action) => {
  switch (action.type) {
    case ActionTypes.INCREMENT:
      return { count: state.count + action.value };
    case ActionTypes.DECREMENT:
      return { count: state.count - action.value };
    default:
      return state;
  }
};

// Component sử dụng
const App: React.FC = () => {
  const [state, dispatch] = React.useReducer<
    React.Reducer<TypeState, TypeActions> // Kiểu cho reducer
  >(reducer, { count: 0 }); // Hàm reducer và state ban đầu

  return (
    <>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: ActionTypes.INCREMENT, value: 1 })}>
        Tăng
      </button>
    </>
  );
};
```

---

### Tổng kết nhanh

| Hook | Cú pháp chính cần nhớ |
| :--- | :--- |
| `useState` | `useState<Kiểu>(giá_trị_ban_đầu)` |
| `useRef` | `useRef<HTMLPhầnTử \| null>(null)` |
| `useContext` | `React.createContext<Kiểu>(giá_trị_mặc_định)` |
| `useReducer` | Định nghĩa `State` và `Action`, rồi `React.Reducer<State, Action>` |

