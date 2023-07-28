Để sử dụng nhiều slice trong Redux Toolkit, chúng ta cần tạo và kết hợp nhiều slice lại với nhau trong Redux store. Mỗi slice sẽ quản lý một phần của trạng thái ứng dụng và có riêng các reducer, action và selector.

Dưới đây là một ví dụ về cách tạo và kết hợp nhiều slice trong Redux Toolkit:

1. Tạo các Slice:
- Tạo các slice bằng cách sử dụng hàm `createSlice` từ Redux Toolkit.
- Mỗi slice sẽ có các trường initialState, reducers, và tên của slice (name).

```javascript
// features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

```javascript
// features/user/userSlice.js
import { createSlice } from '@reduxjs/toolkit';

const userSlice = createSlice({
  name: 'user',
  initialState: null,
  reducers: {
    setUser: (state, action) => action.payload,
    clearUser: () => null,
  },
});

export const { setUser, clearUser } = userSlice.actions;
export default userSlice.reducer;
```

2. Kết hợp các Slice:
- Trong file `store.js`, kết hợp các reducer từ các slice lại với nhau bằng cách sử dụng hàm `combineReducers` từ Redux Toolkit.
- Đưa các reducer vào trường reducer khi khởi tạo Redux store.

```javascript
// store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './features/counter/counterSlice';
import userReducer from './features/user/userSlice';

const rootReducer = {
  counter: counterReducer,
  user: userReducer,
};

const store = configureStore({
  reducer: rootReducer,
});

export default store;
```

3. Sử dụng các Slice trong Components:
- Cuối cùng, sử dụng các slice trong các components bằng cách sử dụng hooks `useSelector` và `useDispatch` từ `react-redux`.
- Bạn có thể sử dụng các action và selector đã được xuất ra từ các slice để gửi action và lấy dữ liệu từ state.

```javascript
// components/Counter.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from '../features/counter/counterSlice';

const Counter = () => {
  const count = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Counter: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
};
```

```javascript
// components/UserInfo.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { setUser, clearUser } from '../features/user/userSlice';

const UserInfo = () => {
  const user = useSelector((state) => state.user);
  const dispatch = useDispatch();

  return (
    <div>
      {user ? (
        <div>
          <p>Name: {user.name}</p>
          <p>Email: {user.email}</p>
        </div>
      ) : (
        <p>No user data available.</p>
      )}
      <button onClick={() => dispatch(setUser({ name: 'John', email: 'john@example.com' }))}>Set User</button>
      <button onClick={() => dispatch(clearUser())}>Clear User</button>
    </div>
  );
};
```

Với việc kết hợp nhiều slice, bạn có thể quản lý nhiều phần của trạng thái ứng dụng một cách dễ dàng và tổ chức code của bạn trở nên rõ ràng hơn.
