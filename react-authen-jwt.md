Viết một ứng dụng React với xác thực sử dụng JWT (JSON Web Token) đòi hỏi một số bước phức tạp, nhưng dưới đây tôi sẽ cung cấp cho bạn một hướng dẫn cơ bản về cách bắt đầu. Để giữ cho hướng dẫn này ngắn gọn, tôi sẽ giả sử bạn đã có một back-end đã cài đặt để xác thực và phát hành JWT. Nếu bạn cần hỗ trợ viết back-end hoặc có bất kỳ câu hỏi nào về điều này, hãy để lại một yêu cầu cụ thể hơn.

Bước 1: Chuẩn bị môi trường
Đảm bảo bạn đã cài đặt Node.js và npm trên máy tính của mình. Sau đó, tạo một dự án React mới bằng cách chạy lệnh sau trong terminal:

```bash
npx create-react-app react-auth-jwt-app
cd react-auth-jwt-app
```

Bước 2: Cài đặt các thư viện cần thiết
Trong thư mục dự án, cài đặt các thư viện sau:

```bash
npm install axios react-router-dom jwt-decode
```

- `axios`: Dùng để thực hiện các yêu cầu HTTP cho việc xác thực và gọi API.
- `react-router-dom`: Sử dụng để quản lý định tuyến trong ứng dụng.
- `jwt-decode`: Dùng để giải mã JWT để lấy thông tin người dùng từ token.

Bước 3: Tạo các thành phần
Tạo các thành phần cơ bản cho ứng dụng, chẳng hạn như `Login`, `Register`, và `Dashboard`. Đảm bảo rằng bạn đã cấu trúc lại ứng dụng của mình như sau:

```
src
|-- components
|   |-- Login.js
|   |-- Register.js
|   |-- Dashboard.js
|-- App.js
|-- index.js
```

Bước 4: Cấu hình React Router
Trong `App.js`, cấu hình React Router để quản lý định tuyến giữa các thành phần:

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Login from './components/Login';
import Register from './components/Register';
import Dashboard from './components/Dashboard';

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/login" component={Login} />
        <Route path="/register" component={Register} />
        <Route path="/dashboard" component={Dashboard} />
      </Switch>
    </Router>
  );
}

export default App;
```

Bước 5: Xử lý xác thực và JWT trong thành phần `Login`
Trong `Login.js`, bạn cần tạo một form đăng nhập và xử lý sự kiện gửi thông tin đăng nhập:

```jsx
import React, { useState } from 'react';
import axios from 'axios';
import jwt_decode from 'jwt-decode';
import { useHistory } from 'react-router-dom';

const Login = () => {
  const history = useHistory();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post('/api/auth/login', { email, password });
      const { token } = response.data;
      localStorage.setItem('jwtToken', token);
      const decoded = jwt_decode(token);
      // Lưu thông tin người dùng vào localStorage hoặc Redux nếu cần thiết
      // localStorage.setItem('userData', JSON.stringify(decoded));
      history.push('/dashboard');
    } catch (error) {
      console.error('Đăng nhập không thành công:', error);
    }
  };

  return (
    <div>
      <h2>Đăng nhập</h2>
      <form onSubmit={handleSubmit}>
        <input type="email" placeholder="Email" onChange={(e) => setEmail(e.target.value)} />
        <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
        <button type="submit">Đăng nhập</button>
      </form>
    </div>
  );
};

export default Login;
```

Bước 6: Xử lý xác thực và JWT trong thành phần `Dashboard`
Trong `Dashboard.js`, bạn cần kiểm tra xem người dùng đã đăng nhập chưa (nếu có JWT trong localStorage) và đảm bảo chỉ cho phép truy cập vào trang này nếu người dùng đã đăng nhập:

```jsx
import React, { useEffect } from 'react';
import jwt_decode from 'jwt-decode';
import { useHistory } from 'react-router-dom';

const Dashboard = () => {
  const history = useHistory();

  useEffect(() => {
    const token = localStorage.getItem('jwtToken');
    if (!token) {
      history.push('/login');
    } else {
      const decoded = jwt_decode(token);
      // Kiểm tra hết hạn của token nếu cần thiết và thực hiện các tác vụ khác
      // Nếu token hết hạn, xóa token khỏi localStorage và đưa người dùng về trang đăng nhập
      // if (decoded.exp < Date.now() / 1000) {
      //   localStorage.removeItem('jwtToken');
      //   history.push('/login');
      // }
    }
  }, [history]);

  return (
    <div>
      <h2>Dashboard</h2>
      <p>Chào mừng đến với trang Dashboard!</p>
    </div>
  );
};

export default Dashboard;
```

Bước 7: Chạy ứng dụng
Cuối cùng, bạn có thể chạy ứng dụng bằng lệnh:

```bash
npm start
```

Ứng dụng React của bạn sẽ chạy trên http://localhost:3000 và bạn có thể thử nghiệm tính năng xác thực sử dụng JWT thông qua trang đăng nhập và trang Dashboard.
