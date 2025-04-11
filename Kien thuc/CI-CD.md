## CI/CD là gì?

**CI/CD** ( **Continuous Integration / Continuous Deployment** ) là quy trình **tự động hóa** trong phát triển phần mềm, giúp triển khai mã nguồn nhanh chóng, đáng tin cậy và giảm thiểu lỗi.

Nó bao gồm 3 phần chính:

1. **CI (Continuous Integration)** - Tích hợp liên tục
2. **CD (Continuous Delivery)** - Phát hành liên tục
3. **CD (Continuous Deployment)** - Triển khai liên tục

### Continuous Integration (CI) – Tích hợp liên tục

**Mục đích:**

* Tự động kiểm tra code mới khi có thay đổi (commit/push).
* Chạy unit test, integration test để đảm bảo code hoạt động đúng.
* Phát hiện lỗi sớm để sửa nhanh hơn.

**Cách hoạt động**

* **Lập trình viên đẩy code** lên GitHub/GitLab/Bitbucket.
* **Hệ thống CI tự động kiểm tra** :
  * Kiểm tra cú pháp, lỗi lint.
  * Chạy các unit test, integration test.
  * Kiểm tra bảo mật, dependencies.

* Nếu thành công → Code hợp lệ để triển khai.
* Nếu thất bại → CI gửi thông báo để fix lỗi.

**Công cụ phổ biến cho CI**

* GitHub Actions
* GitLab CI/CD
* Jenkins
* CircleCI
* Travis CI

### Continuous Delivery (CD) – Phát hành liên tục

**Mục đích:**

* Sau khi CI hoàn thành, code sẵn sàng để triển khai lên môi trường staging/production.
* Việc triển khai vẫn cần **duyệt tay** (manual approval).
* Giảm rủi ro khi release bản mới.

**Cách hoạt động:**

* Code đã vượt qua CI
* CD chuẩn bị bản build (artifact, container, v.v.).
* Deploy code lên môi trường staging/test để kiểm tra.
* Nếu kiểm tra ổn, có thể bấm nút  **Deploy to Production** .

**Công cụ phổ biến cho CD:**

* GitHub Actions
* GitLab CI/CD
* ArgoCD
* Spinnaker
* AWS CodePipeline

### Continuous Deployment (CD) – Triển khai liên tục

**Mục đích:**

* Mọi thay đổi hợp lệ sẽ **tự động triển khai** lên production  **mà không cần con người can thiệp** .
* Phù hợp với ứng dụng cần cập nhật nhanh như Facebook, Amazon, Netflix.
* Cần có  **CI/CD pipeline cực kỳ mạnh và ổn định** .

**Cách hoạt động:**

1. Code vượt qua CI ✅
2. Code vượt qua CD ✅
3. Hệ thống tự động deploy code lên production  **mà không cần duyệt tay** .
4. Nếu có lỗi, rollback tự động về phiên bản trước.

**Khi nào nên dùng?**

* Phần mềm cần cập nhật liên tục (daily release).
* Có hệ thống **monitoring tốt** để rollback khi lỗi xảy ra.
* Có quy trình kiểm thử nghiêm ngặt trước khi deploy.

**Công cụ phổ biến:**

* Kubernetes + ArgoCD
* Jenkins + Spinnaker
* AWS CodeDeploy
* GitHub Actions

## **CI/CD Pipeline hoạt động thế nào?**

**CI/CD pipeline** là tập hợp các bước tự động hóa giúp CI/CD hoạt động trơn tru.

**Ví dụ Pipeline CI/CD đơn giản:**

1. **Developer push code lên GitHub.**
2. **CI chạy tự động:**
   * Kiểm tra lỗi code (lint).
   * Chạy unit test, integration test.
   * Build container Docker.
3. **CD Deploy code lên Staging:**
   * Kiểm tra lại trên môi trường giả lập production.
4. **CD Deploy lên Production:**
   * Nếu test thành công, deploy lên production.

## **Lợi ích của CI/CD**

* **Giảm lỗi code:** Mọi commit được kiểm tra tự động.
* **Tăng tốc độ release:** Code mới có thể deploy ngay sau khi kiểm tra.
* **Rollback dễ dàng:** Nếu có lỗi, có thể rollback về bản trước.
* **Tự động hóa quy trình:** Giảm tải công việc thủ công cho dev.
