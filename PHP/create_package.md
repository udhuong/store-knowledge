```sh
packages/
└── vendor-name/
    └── package-name/
        ├── src/
        │   ├── Console/               # Các command Artisan cho package
        │   ├── Contracts/             # Interface, Contract cho services
        │   ├── Exceptions/            # Exception riêng cho package
        │   ├── Facades/               # Facade class nếu cần
        │   ├── Helpers/               # Helper functions
        │   ├── Http/
        │   │   ├── Controllers/       # Controllers
        │   │   ├── Middleware/        # Middleware riêng
        │   │   └── Requests/          # Form Request validation
        │   ├── Jobs/                  # Queue jobs
        │   ├── Listeners/             # Event listeners
        │   ├── Models/                # Models riêng
        │   ├── Providers/             # Service Provider chính
        │   ├── Repositories/          # Repository pattern
        │   ├── Services/              # Business logic
        │   ├── Traits/                # Trait dùng chung
        │   └── Support/               # Các class hỗ trợ
        │
        ├── database/
        │   ├── factories/             # Model factories
        │   ├── migrations/            # Migrations riêng cho package
        │   └── seeders/               # Seeders nếu cần
        │
        ├── routes/
        │   ├── web.php                # Web routes
        │   └── api.php                # API routes
        │
        ├── resources/
        │   ├── lang/                  # Đa ngôn ngữ
        │   └── views/                 # Blade views
        │
        ├── tests/                     # Unit / Feature tests cho package
        │
        ├── composer.json              # Composer config
        ├── README.md                  # Giới thiệu package
        └── package.json               # Nếu có asset frontend

```
