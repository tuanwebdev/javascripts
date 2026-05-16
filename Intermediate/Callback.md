
### Callback Function là gì? (Định nghĩa "mì ăn liền")
Hãy tưởng tượng bạn đưa số điện thoại của mình cho một người bạn và nói: "Khi nào cậu có kết quả thì **gọi lại cho tớ** nhé". Số điện thoại của bạn chính là một callback function.

Trong lập trình, **Callback function là một hàm được truyền vào một hàm khác như một tham số, và sẽ được gọi lại bên trong hàm đó để hoàn thành một công việc.**

### 🧩 Phân biệt hai kiểu gọi lại: "Làm ngay" và "Làm sau"
Điều quan trọng nhất cần nhớ là callback có hai cách hoạt động, ảnh hưởng trực tiếp đến thứ tự code chạy.

| Loại Callback | Cách hoạt động | Ví von thực tế | Ví dụ trong JS |
| :--- | :--- | :--- | :--- |
| **Synchronous (Đồng bộ)** | **Làm ngay lập tức**, chặn các dòng code phía sau cho đến khi xong. | Bạn gọi điện đặt pizza, và cứ đứng đợi ở quầy cho đến khi nhân viên làm xong và đưa cho bạn. | `.map()`, `.forEach()` |
| **Asynchronous (Bất đồng bộ)** | **Hẹn làm sau**, code phía sau vẫn chạy tiếp. Callback chỉ được gọi lại sau khi một tác vụ nền hoàn tất. | Bạn gọi pizza, đưa số điện thoại, rồi về nhà làm việc khác. Khi nào pizza xong, nhân viên sẽ **gọi lại** cho bạn. | `setTimeout()`, `.then()` |

### 💡 Ví dụ then chốt: Tại sao phải quan tâm đến sự khác biệt này?
Trang web đưa ra một ví dụ cực kỳ quan trọng để bạn thấy rõ sự khác biệt. Hãy xem đoạn code này:

```javascript
let value = 1;

// Hàm doSomething sẽ nhận một callback và gọi nó
doSomething(() => {
  value = 2; // Dòng này được chạy trong callback
});

console.log(value); // Kết quả sẽ là 1 hay 2?
```

Kết quả phụ thuộc hoàn toàn vào cách `doSomething` thực thi callback:

*   **Nếu `doSomething` gọi callback ĐỒNG BỘ (làm ngay):**
    *   Code chạy theo thứ tự: `value = 1` → `doSomething(callback)` (bên trong gọi `value = 2`) → `console.log(value)`.
    *   **Kết quả: `2`**

*   **Nếu `doSomething` gọi callback BẤT ĐỒNG BỘ (hẹn làm sau, ví dụ như `setTimeout`):**
    *   Code chạy: `value = 1` → `doSomething(callback)` (lên lịch để gọi sau, không chạy ngay) → `console.log(value)` (in ra 1).
    *   Sau khi tất cả code đồng bộ chạy xong, callback mới được gọi: `value = 2`.
    *   **Kết quả: `1`**

### 📝 Tóm lại
Callback chỉ đơn giản là một hàm được truyền vào để "gọi lại sau", nhưng thời điểm "sau" đó là ngay lập tức hay trong tương lai sẽ quyết định cách code của bạn hoạt động. Đây là kiến thức nền tảng cốt lõi để hiểu các chủ đề phức tạp hơn như bất đồng bộ trong JavaScript.
