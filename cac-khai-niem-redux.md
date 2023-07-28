Redux là một thư viện quản lý trạng thái (state management) cho ứng dụng JavaScript. Nó giúp quản lý trạng thái của ứng dụng một cách dễ dàng và dự đoán được. Dưới đây là một số khái niệm cơ bản trong Redux:

1. **Store (Kho chứa)**:
   - Store là nơi lưu trữ toàn bộ trạng thái của ứng dụng.
   - Trạng thái của ứng dụng được duy trì dưới dạng một đối tượng (state object).
   - Trạng thái chỉ có thể được thay đổi bằng cách gửi một action đến reducer.

2. **Action (Hành động)**:
   - Action là một đối tượng JavaScript chứa thông tin về sự kiện đã xảy ra trong ứng dụng.
   - Nó phải có một thuộc tính bắt buộc là "type", mô tả loại hành động mà action đó thực hiện.
   - Action có thể chứa thêm các dữ liệu cần thiết để thay đổi trạng thái của ứng dụng.

3. **Reducer (Bộ giảm thiểu)**:
   - Reducer là một hàm JavaScript xử lý các action và trả về trạng thái mới cho store.
   - Nó nhận vào hai đối số: trạng thái hiện tại và action.
   - Reducer không thể thay đổi trực tiếp trạng thái hiện tại, mà thay vào đó, nó sẽ tạo ra một bản sao mới của trạng thái và trả về.

4. **Dispatch (Phát hành)**:
   - Dispatch là một phương thức của store, dùng để gửi action đến reducer để cập nhật trạng thái.
   - Khi bạn muốn thay đổi trạng thái của ứng dụng, bạn gửi một action thông qua dispatch.

5. **Middleware (Phần mềm trung gian)**:
   - Middleware là một lớp trung gian cho phép bạn thực hiện các hành động trước khi action đến reducer.
   - Nó cho phép bạn thực hiện các tác vụ như ghi log, xử lý bất đồng bộ, hoặc thay đổi action trước khi nó đến reducer.

6. **Selector (Bộ chọn)**:
   - Selector là một hàm dùng để trích xuất một phần của trạng thái từ store.
   - Nó giúp bạn tránh việc truy cập trực tiếp vào store và giúp tách biệt logic truy xuất dữ liệu từ trạng thái.

7. **Action Creator (Trình tạo hành động)**:
   - Action Creator là một hàm trả về một action.
   - Nó giúp bạn tạo action một cách dễ dàng và tránh việc viết trực tiếp các đối tượng action trong code.

Các khái niệm trên là những thành phần chính của Redux. Khi bạn kết hợp chúng lại với nhau, bạn sẽ có một quy trình quản lý trạng thái rõ ràng và dễ dàng trong ứng dụng của mình.

Để hiểu rõ hơn về Redux và các khái niệm tương ứng, hãy xem qua một ví dụ đơn giản về ứng dụng Todo List:

**1. Action (Hành động):**
Action là một đối tượng JavaScript, chứa thông tin về sự kiện đã xảy ra trong ứng dụng. Trong ví dụ này, chúng ta sẽ có hai loại action: "ADD_TODO" (Thêm công việc mới) và "TOGGLE_TODO" (Đánh dấu công việc đã hoàn thành).

```javascript
// Action Creator (Trình tạo hành động)
const addTodo = (text) => {
  return {
    type: "ADD_TODO",
    payload: {
      id: Date.now(),
      text,
      completed: false,
    },
  };
};

const toggleTodo = (id) => {
  return {
    type: "TOGGLE_TODO",
    payload: id,
  };
};
```

**2. Reducer (Bộ giảm thiểu):**
Reducer là một hàm JavaScript xử lý các action và trả về trạng thái mới cho store. Trong ví dụ này, chúng ta có một reducer để quản lý danh sách công việc.

```javascript
const initialState = [];

const todosReducer = (state = initialState, action) => {
  switch (action.type) {
    case "ADD_TODO":
      return [...state, action.payload];
    case "TOGGLE_TODO":
      return state.map((todo) =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
};
```

**3. Store (Kho chứa):**
Store là nơi lưu trữ toàn bộ trạng thái của ứng dụng. Trong Redux, bạn chỉ có một store duy nhất cho ứng dụng.

```javascript
const { createStore } = Redux;
const store = createStore(todosReducer);
```

**4. Dispatch (Phát hành):**
Dispatch là một phương thức của store, dùng để gửi action đến reducer để cập nhật trạng thái.

```javascript
// Gửi action để thêm công việc mới vào store
store.dispatch(addTodo("Mua sữa"));
store.dispatch(addTodo("Đi chợ"));

// Gửi action để đánh dấu công việc đã hoàn thành
store.dispatch(toggleTodo(1));
```

**5. Middleware (Phần mềm trung gian):**
Middleware cho phép bạn thực hiện các hành động trước khi action đến reducer. Trong ví dụ này, chúng ta sẽ sử dụng middleware để ghi log mỗi khi có action được gửi đến reducer.

```javascript
const loggerMiddleware = (store) => (next) => (action) => {
  console.log("Action:", action);
  next(action);
};

const { applyMiddleware } = Redux;
const createStoreWithMiddleware = applyMiddleware(loggerMiddleware)(createStore);
const store = createStoreWithMiddleware(todosReducer);
```

**6. Selector (Bộ chọn):**
Selector là một hàm dùng để trích xuất một phần của trạng thái từ store. Trong ví dụ này, chúng ta có một selector để lấy danh sách các công việc chưa hoàn thành.

```javascript
const getIncompleteTodos = (state) => {
  return state.filter((todo) => !todo.completed);
};

console.log(getIncompleteTodos(store.getState())); // In ra danh sách công việc chưa hoàn thành
```

Trên đây là một ví dụ cơ bản về Redux và các khái niệm liên quan. Lưu ý rằng việc sử dụng Redux trong ứng dụng thực tế có thể phức tạp hơn và yêu cầu cấu hình thêm như sử dụng thư viện React-Redux để kết nối Redux với React components.
