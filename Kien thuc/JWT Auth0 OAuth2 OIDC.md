## JWT

- Cấu trúc JWT: Header.Payload.Signature.
- Spring Security kết hợp với JWT cung cấp giải pháp bảo mật stateless (ứng dụng không trạng thái) tiện lợi và hiệu quả cho ứng dụng RESTful
- JWT thường được dùng để xác thực người dùng (authentication) và phân quyền truy cập (authorization).
- Một JWT gồm ba phần: Header, Payload, và Signature, thường được mã hóa bằng thuật toán HMAC hoặc RSA.

## Auth0

**Auth0** là một **dịch vụ cung cấp authentication và authorization** (cung cấp sẵn OAuth 2.0, OpenID Connect, SAML, v.v.) giúp lập trình viên triển khai dễ dàng mà không cần tự xây dựng hệ thống xác thực.

## OAuth 2.0

**OAuth 2.0** là một **giao thức (protocol)** ủy quyền truy cập tài nguyên một cách an toàn mà không cần chia sẻ mật khẩu.l                                  jr45t4ngg 

**Ví dụ thực tế:**

* Khi bạn đăng nhập vào một trang web bằng  **Google/Facebook** , trang web này sử dụng OAuth 2.0 để yêu cầu quyền truy cập vào thông tin của bạn mà không cần biết mật khẩu Google/Facebook của bạn.

## **Khi nào chọn Auth0, khi nào chọn OAuth 2.0?**

**Dùng Auth0 khi** :

* Cần một hệ thống authentication nhanh chóng, có hỗ trợ  **SSO, MFA** .
* Muốn quản lý user dễ dàng qua Auth0 Dashboard.

**Dùng OAuth 2.0 khi** :

* Chỉ cần đăng nhập bằng  **Google, Facebook, GitHub** .
* Muốn quản lý user bằng **database riêng** và chỉ sử dụng OAuth2 để xác thực danh tính.

## **Tóm tắt**

* **Auth0** giúp xác thực và ủy quyền với **JWT** dễ dàng trong Spring Boot.
* **OAuth2** giúp đăng nhập với Google, Facebook mà không cần lưu trữ mật khẩu người dùng.
* **Auth0 phù hợp cho hệ thống lớn** , trong khi  **OAuth2 thích hợp khi chỉ cần đăng nhập qua bên thứ ba** .

## OpenID Connect
