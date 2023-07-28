Để sử dụng React Router với Redux Toolkit, chúng ta cần cấu hình Redux store và tích hợp React Router vào ứng dụng. Dưới đây là một hướng dẫn cơ bản về cách làm điều đó:

Bước 1: Cài đặt các thư viện
- Đầu tiên, cài đặt các thư viện cần thiết bằng npm hoặc yarn:
```
npm install react-router-dom
npm install @reduxjs/toolkit react-redux
```
hoặc
```
yarn add react-router-dom
yarn add @reduxjs/toolkit react-redux
```

Bước 2: Cấu hình Redux store
- Tạo Redux store và định nghĩa reducers bằng Redux Toolkit như đã thấy trong các ví dụ trước đó.

```javascript
// store.js
import { configureStore } from '@reduxjs/toolkit';
import rootReducer from './reducers';

const store = configureStore({
  reducer: rootReducer,
});

export default store;
```

Bước 3: Cấu hình Router và Tích hợp Redux Provider
- Tạo file `App.js` để cấu hình React Router và Redux Provider. Trong file này, bạn sẽ bọc toàn bộ ứng dụng bên trong Redux Provider và sử dụng Router để xác định các Route trong ứng dụng.

```javascript
// App.js
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import { Provider } from 'react-redux';
import store from './store';
import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';
import NotFoundPage from './pages/NotFoundPage';

const App = () => {
  return (
    <Provider store={store}>
      <Router>
        <Switch>
          <Route exact path="/" component={HomePage} />
          <Route path="/about" component={AboutPage} />
          <Route component={NotFoundPage} />
        </Switch>
      </Router>
    </Provider>
  );
};

export default App;
```

Bước 4: Tích hợp Redux vào các Component
- Bây giờ, bạn có thể tích hợp Redux vào các Component bên trong các Route. Để làm điều này, bạn sử dụng hooks `useSelector` và `useDispatch` từ `react-redux` để lấy dữ liệu từ state và gửi các action.

```javascript
// pages/HomePage.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from '../actions/counterActions';

const HomePage = () => {
  const count = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Home Page</h1>
      <p>Counter: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
};

export default HomePage;
```

Với cấu hình trên, bạn đã tích hợp React Router vào ứng dụng và sử dụng Redux Toolkit để quản lý trạng thái của ứng dụng. Bây giờ, bạn có thể sử dụng Router để điều hướng giữa các trang và sử dụng Redux để quản lý dữ liệu của ứng dụng.
