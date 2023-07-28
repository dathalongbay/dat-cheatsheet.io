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
