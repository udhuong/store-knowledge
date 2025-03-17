## Package

### Cài đặt package tùy chỉnh

```json
{
  "repositories": [
    {
      "type": "path",
      "url": "vendor/vendor-name/package-name"
    },
    {
      "type": "vcs",
      "url": "https://github.com/my-vendor/my-package.git"
    },
    {
      "type": "package",
      "package": {
        "name": "my-vendor/my-package",
        "version": "1.0.0",
        "source": {
          "url": "https://github.com/my-vendor/my-package.git",
          "type": "git",
          "reference": "master"
        },
        "autoload": {
          "psr-4": {
            "MyVendor\\MyPackage\\": "src/"
          }
        }
      }
    },
    {
      "type": "composer",
      "url": "https://packagist.mycompany.com"
    },
    {
      "type": "artifact",
      "url": "/path/to/artifacts/"
    }
  ]
}
```

* **`repositories`** : Khai báo một hoặc nhiều kho package tùy chỉnh.
* **`type`** : Xác định loại repository mà Composer sử dụng.
* **`url`** : Đường dẫn đến package, có thể là thư mục local hoặc URL.

Các type:

* `path` (Dùng package từ thư mục cục bộ)
* `vcs` (Sử dụng Git, GitHub, GitLab, Bitbucket, SVN, Mercurial)
* `package` (Thêm package thủ công)
* `composer` (Dùng một kho Composer khác ngoài Packagist)
* `artifact` (Sử dụng package từ file `.zip`, `.tar.gz`)

### Các loại phiên bản trong Composer

| Kiểu phiên bản              | Ý nghĩa                                           |
| ------------------------------ | --------------------------------------------------- |
| `1.0.0`,`2.1.3`            | Phiên bản ổn định                              |
| `1.0.0-beta`,`2.0.0-alpha` | Phiên bản thử nghiệm (chưa chính thức)       |
| `1.0.0-rc1`                  | Phiên bản "release candidate" (gần hoàn thiện) |
| `dev-main`                   | Lấy code từ nhánh `main`trong Git              |
| `dev-master`                 | Lấy code từ nhánh `master`trong Git            |
| `dev-develop`                | Lấy code từ nhánh `develop`trong Git           |
| `dev-feature-branch`         | Lấy code từ nhánh `feature-branch`trong Git    |
