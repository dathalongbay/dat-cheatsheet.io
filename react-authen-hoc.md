Để bảo vệ các component sau khi đã đăng nhập bằng JWT trong React, bạn có thể sử dụng một Higher-Order Component (HOC). HOC là một cơ chế trong React giúp tái sử dụng logic component một cách dễ dàng. Trong trường hợp này, HOC sẽ kiểm tra xem người dùng đã đăng nhập hay chưa bằng cách kiểm tra sự tồn tại và hợp lệ của JWT.

Dưới đây là một ví dụ về cách viết một HOC bảo vệ các component sau khi đã đăng nhập bằng JWT trong React:

1. Tạo file `withAuth.js` để viết HOC:

```jsx
import React, { Component } from 'react';

const withAuth = (WrappedComponent) => {
  return class extends Component {
    constructor(props) {
      super(props);
      this.state = {
        isAuthenticated: false,
      };
    }

    componentDidMount() {
      // Thực hiện kiểm tra đăng nhập ở đây
      // Đọc JWT từ local storage hoặc cookie và kiểm tra tính hợp lệ của JWT
      // Nếu JWT hợp lệ, đánh dấu isAuthenticated là true
      const isAuthenticated = this.checkAuthStatus();
      this.setState({ isAuthenticated });
    }

    checkAuthStatus = () => {
      // Kiểm tra xem JWT có tồn tại và hợp lệ hay không
      // Đây chỉ là một ví dụ cơ bản, bạn có thể sử dụng thư viện như jsonwebtoken để kiểm tra
      const token = localStorage.getItem('jwt'); // Hoặc đọc JWT từ nơi lưu trữ khác (cookies, redux store, ...)
      if (token) {
        // Thực hiện kiểm tra tính hợp lệ của JWT ở đây (có thể sử dụng thư viện jsonwebtoken)
        return true; // Trả về true nếu JWT hợp lệ
      }
      return false; // Trả về false nếu không tìm thấy JWT
    };

    render() {
      const { isAuthenticated } = this.state;
      if (isAuthenticated) {
        // Nếu người dùng đã đăng nhập, trả về WrappedComponent bên trong HOC
        return <WrappedComponent {...this.props} />;
      } else {
        // Nếu người dùng chưa đăng nhập, có thể chuyển hướng đến trang đăng nhập hoặc hiển thị thông báo
        return <div>Bạn cần đăng nhập để truy cập vào trang này!</div>;
      }
    }
  };
};

export default withAuth;
```

2. Sử dụng HOC để bảo vệ các component:

```jsx
import React from 'react';
import withAuth from './withAuth';

// Component cần bảo vệ bằng HOC
const ProtectedComponent = () => {
  return <div>Đây là trang cần được bảo vệ sau khi đăng nhập!</div>;
};

// Áp dụng HOC để bảo vệ component
const ProtectedComponentWithAuth = withAuth(ProtectedComponent);

// Sử dụng ProtectedComponentWithAuth thay vì ProtectedComponent
// Nếu người dùng đã đăng nhập, nội dung của ProtectedComponent sẽ được hiển thị
// Nếu người dùng chưa đăng nhập, thông báo sẽ được hiển thị
```

Khi sử dụng `ProtectedComponentWithAuth`, HOC `withAuth` sẽ thực hiện kiểm tra xem người dùng đã đăng nhập hay chưa và hiển thị nội dung tương ứng. Nếu người dùng đã đăng nhập, nội dung của `ProtectedComponent` sẽ được hiển thị; nếu người dùng chưa đăng nhập, HOC sẽ hiển thị thông báo hoặc chuyển hướng đến trang đăng nhập (tùy thuộc vào cách bạn triển khai logic trong HOC).

## Ví dụ sử dụng functional component 
Tất nhiên! Thay vì sử dụng class component, chúng ta có thể viết HOC bảo vệ các component sau khi đã đăng nhập bằng JWT sử dụng function component và React Hooks. Dưới đây là phiên bản HOC sử dụng function component:

1. Tạo file `withAuth.js`:

```jsx
import React, { useEffect, useState } from 'react';

const withAuth = (WrappedComponent) => {
  const WithAuth = (props) => {
    const [isAuthenticated, setIsAuthenticated] = useState(false);

    useEffect(() => {
      // Thực hiện kiểm tra đăng nhập ở đây
      // Đọc JWT từ local storage hoặc cookie và kiểm tra tính hợp lệ của JWT
      // Nếu JWT hợp lệ, đánh dấu isAuthenticated là true
      const isAuthenticated = checkAuthStatus();
      setIsAuthenticated(isAuthenticated);
    }, []);

    const checkAuthStatus = () => {
      // Kiểm tra xem JWT có tồn tại và hợp lệ hay không
      // Đây chỉ là một ví dụ cơ bản, bạn có thể sử dụng thư viện như jsonwebtoken để kiểm tra
      const token = localStorage.getItem('jwt'); // Hoặc đọc JWT từ nơi lưu trữ khác (cookies, redux store, ...)
      if (token) {
        // Thực hiện kiểm tra tính hợp lệ của JWT ở đây (có thể sử dụng thư viện jsonwebtoken)
        return true; // Trả về true nếu JWT hợp lệ
      }
      return false; // Trả về false nếu không tìm thấy JWT
    };

    if (isAuthenticated) {
      // Nếu người dùng đã đăng nhập, trả về WrappedComponent bên trong HOC
      return <WrappedComponent {...props} />;
    } else {
      // Nếu người dùng chưa đăng nhập, có thể chuyển hướng đến trang đăng nhập hoặc hiển thị thông báo
      return <div>Bạn cần đăng nhập để truy cập vào trang này!</div>;
    }
  };

  return WithAuth;
};

export default withAuth;
```

2. Sử dụng HOC để bảo vệ các component:

```jsx
import React from 'react';
import withAuth from './withAuth';

// Component cần bảo vệ bằng HOC
const ProtectedComponent = () => {
  return <div>Đây là trang cần được bảo vệ sau khi đăng nhập!</div>;
};

// Áp dụng HOC để bảo vệ component
const ProtectedComponentWithAuth = withAuth(ProtectedComponent);

// Sử dụng ProtectedComponentWithAuth thay vì ProtectedComponent
// Nếu người dùng đã đăng nhập, nội dung của ProtectedComponent sẽ được hiển thị
// Nếu người dùng chưa đăng nhập, thông báo sẽ được hiển thị
```

Phiên bản này của HOC sử dụng React Hooks như `useEffect` và `useState` để thực hiện các tác vụ của class component trong function component.

## Ví dụ sử dụng react router 
Tất nhiên! Để sử dụng React Router trong ví dụ HOC bảo vệ các component sau khi đã đăng nhập bằng JWT, chúng ta cần cài đặt và cấu hình React Router và sau đó sử dụng nó để xác định xem trang bảo vệ nào sẽ được hiển thị sau khi người dùng đã đăng nhập.

Dưới đây là phiên bản cải tiến của ví dụ sử dụng React Router:

1. Cài đặt React Router:

Trước tiên, bạn cần cài đặt React Router vào dự án của mình bằng lệnh sau:

```
npm install react-router-dom
```

2. Cấu hình React Router và sử dụng HOC:

```jsx
// File: App.js
import React from 'react';
import { BrowserRouter as Router, Switch, Route, Redirect } from 'react-router-dom';
import withAuth from './withAuth';
import ProtectedComponent from './ProtectedComponent';
import LoginComponent from './LoginComponent';

const App = () => {
  return (
    <Router>
      <Switch>
        {/* Đăng nhập */}
        <Route path="/login" component={LoginComponent} />

        {/* Các trang bảo vệ sử dụng HOC */}
        <Route path="/protected" component={withAuth(ProtectedComponent)} />

        {/* Nếu người dùng truy cập trang không tồn tại, chuyển hướng về trang đăng nhập */}
        <Route render={() => <Redirect to="/login" />} />
      </Switch>
    </Router>
  );
};

export default App;
```

3. Tạo trang đăng nhập (`LoginComponent.js`):

```jsx
import React from 'react';

const LoginComponent = () => {
  // Xử lý đăng nhập ở đây
  return (
    <div>
      <h1>Trang đăng nhập</h1>
      {/* Các input và button đăng nhập */}
    </div>
  );
};

export default LoginComponent;
```

4. Tạo trang bảo vệ (`ProtectedComponent.js`):

```jsx
import React from 'react';

const ProtectedComponent = () => {
  return <div>Đây là trang cần được bảo vệ sau khi đăng nhập!</div>;
};

export default ProtectedComponent;
```

Bây giờ, khi người dùng truy cập vào URL `/login`, họ sẽ thấy trang đăng nhập. Sau khi đăng nhập thành công và nhận được JWT, người dùng sẽ được chuyển hướng đến trang `/protected` (do đã áp dụng HOC bảo vệ bằng `withAuth` cho `ProtectedComponent`). Nếu người dùng truy cập vào bất kỳ URL nào không tồn tại, họ sẽ được chuyển hướng về trang đăng nhập.

Lưu ý rằng trong thực tế, việc xử lý đăng nhập, xác thực JWT và chuyển hướng có thể phức tạp hơn, nhưng ví dụ này chỉ cung cấp một cơ sở để bạn bắt đầu tích hợp React Router và HOC bảo vệ vào ứng dụng React của mình.
