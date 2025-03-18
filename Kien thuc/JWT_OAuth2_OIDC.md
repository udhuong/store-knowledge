## JWT

- Cấu trúc JWT: Header.Payload.Signature.
- Spring Security kết hợp với JWT cung cấp giải pháp bảo mật stateless (ứng dụng không trạng thái) tiện lợi và hiệu quả cho ứng dụng RESTful
- JWT thường được dùng để xác thực người dùng (authentication) và phân quyền truy cập (authorization).
- Một JWT gồm ba phần: Header, Payload, và Signature, thường được mã hóa bằng thuật toán HMAC hoặc RSA.

## Auth0

**Auth0** là một dịch vụ quản lý xác thực và ủy quyền, giúp bạn dễ dàng triển khai **JWT Authentication** mà không cần tự xây dựng từ đầu.

* Là một nền tảng cung cấp dịch vụ xác thực (authentication) và quản lý danh tính (identity management).
* Hỗ trợ nhiều phương thức đăng nhập như Google, Facebook, GitHub, SSO (Single Sign-On), MFA (Multi-Factor Authentication).
* Giúp các lập trình viên dễ dàng tích hợp hệ thống đăng nhập mà không cần tự xây dựng từ đầu.

## Auth2

* Là một giao thức ủy quyền (authorization) giúp ứng dụng cấp quyền truy cập dữ liệu thay mặt người dùng mà không cần chia sẻ mật khẩu.
* Thường được sử dụng để đăng nhập bằng Google, Facebook, GitHub,...
* OAuth 2.0 có 4 cách cấp quyền chính: Authorization Code, Implicit, Password, và Client Credentials.

## **Khi nào chọn Auth0, khi nào chọn OAuth2?**

✅  **Dùng Auth0 khi** :

✔️ Cần một hệ thống authentication nhanh chóng, có hỗ trợ  **SSO, MFA** .

✔️ Muốn quản lý user dễ dàng qua Auth0 Dashboard.

✅  **Dùng OAuth2 khi** :

✔️ Chỉ cần đăng nhập bằng  **Google, Facebook, GitHub** .

✔️ Muốn quản lý user bằng **database riêng** và chỉ sử dụng OAuth2 để xác thực danh tính.

## **Tóm tắt**

* **Auth0** giúp xác thực và ủy quyền với **JWT** dễ dàng trong Spring Boot.
* **OAuth2** giúp đăng nhập với Google, Facebook mà không cần lưu trữ mật khẩu người dùng.
* **Auth0 phù hợp cho hệ thống lớn** , trong khi  **OAuth2 thích hợp khi chỉ cần đăng nhập qua bên thứ ba** .
