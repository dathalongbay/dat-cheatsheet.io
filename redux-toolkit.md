Dưới đây là một cheatsheet về Redux Toolkit với những khái niệm và cách sử dụng phổ biến:

### Redux Toolkit Cheatsheet

#### Cài đặt
```
npm install @reduxjs/toolkit
```

#### Cấu trúc thư mục dự án
```
src/
  ├─ store.js
  ├─ features/
  │   ├─ slice1/
  │   │   ├─ slice1Slice.js
  │   │   └─ ...
  │   ├─ slice2/
  │   │   ├─ slice2Slice.js
  │   │   └─ ...
  │   └─ ...
  └─ components/
      ├─ Component1.js
      └─ ...
```

#### Tạo Redux Store
```javascript
// store.js
import { configureStore } from '@reduxjs/toolkit';
import rootReducer from './features/rootReducer';

const store = configureStore({
  reducer: rootReducer,
});

export default store;
```

#### Tạo Slice (Reducer + Action)
```javascript
// features/slice1/slice1Slice.js
import { createSlice } from '@reduxjs/toolkit';

const slice1Slice = createSlice({
  name: 'slice1',
  initialState: initialStateValue,
  reducers: {
    action1: (state, action) => {
      // Xử lý thay đổi trạng thái của slice1 dựa trên action
    },
    action2: (state, action) => {
      // Xử lý thay đổi trạng thái của slice1 dựa trên action
    },
  },
});

export const { action1, action2 } = slice1Slice.actions;
export default slice1Slice.reducer;
```

#### Kết hợp Reducers
```javascript
// features/rootReducer.js
import { combineReducers } from '@reduxjs/toolkit';
import slice1Reducer from './slice1/slice1Slice';
import slice2Reducer from './slice2/slice2Slice';
// Import thêm các reducers khác cần kết hợp

const rootReducer = combineReducers({
  slice1: slice1Reducer,
  slice2: slice2Reducer,
  // Thêm các slice (reducers) khác vào đây
});

export default rootReducer;
```

#### Kết nối Redux Store và Provider
```javascript
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

#### Sử dụng Selector
```javascript
import { useSelector } from 'react-redux';

const Component1 = () => {
  const data = useSelector((state) => state.slice1.data);
  // Sử dụng data từ state của slice1
};
```

#### Sử dụng Dispatch
```javascript
import { useDispatch } from 'react-redux';
import { action1, action2 } from '../features/slice1/slice1Slice';

const Component2 = () => {
  const dispatch = useDispatch();

  const handleAction1 = () => {
    dispatch(action1(payload));
  };

  const handleAction2 = () => {
    dispatch(action2(payload));
  };
};
```

#### Sử dụng Thunks (Tác vụ Bất đồng bộ)
```javascript
// features/slice1/slice1Slice.js
import { createAsyncThunk } from '@reduxjs/toolkit';
import api from '../../api'; // Import các hàm gọi API

export const fetchSomeData = createAsyncThunk('slice1/fetchSomeData', async (params) => {
  const response = await api.getData(params);
  return response.data;
});

// Thêm các thunks khác nếu cần
```

#### Sử dụng ExtraReducers
```javascript
// features/slice2/slice2Slice.js
import { createSlice } from '@reduxjs/toolkit';
import { fetchSomeData } from '../slice1/slice1Slice';

const slice2Slice = createSlice({
  name: 'slice2',
  initialState: initialStateValue,
  reducers: {
    // Các reducers của slice2
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchSomeData.fulfilled, (state, action) => {
        // Xử lý thành công khi thực hiện fetchSomeData
      })
      .addCase(fetchSomeData.rejected, (state, action) => {
        // Xử lý khi fetchSomeData bị từ chối
      });
  },
});
```

### Đó là một số cú pháp và khái niệm cơ bản khi sử dụng Redux Toolkit. Lưu ý rằng việc sử dụng Redux Toolkit giúp rút gọn và tối ưu hóa code, làm cho việc quản lý trạng thái ứng dụng trở nên dễ dàng và hiệu quả hơn.
