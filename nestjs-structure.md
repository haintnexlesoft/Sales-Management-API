# Cấu trúc dự án NestJS gợi ý

```
src/
├── main.ts
├── app.module.ts
├── common/
│   ├── decorators/        # @CurrentUser, @Permissions
│   ├── guards/            # JwtAuthGuard, PermissionsGuard
│   ├── interceptors/      # TransformInterceptor, LoggingInterceptor
│   ├── filters/           # AllExceptionsFilter
│   ├── pipes/             # ValidationPipe (class-validator)
│   └── dto/               # PaginationDto, BaseResponseDto
├── config/                # ConfigModule (env, db, jwt)
├── database/
│   ├── migrations/
│   └── seeds/             # roles, permissions, default admin
├── modules/
│   ├── auth/              # login, refresh, jwt strategy
│   ├── users/             # users + employees
│   ├── roles/             # roles + permissions (RBAC)
│   ├── categories/
│   ├── attributes/
│   ├── products/          # products + variants
│   ├── suppliers/
│   ├── purchase-orders/
│   ├── warehouses/
│   ├── inventory/         # stock-items, stock-movements
│   ├── customers/
│   ├── customer-groups/
│   ├── promotions/        # validate engine
│   ├── orders/            # online + POS, state machine
│   ├── stores/
│   ├── settings/
│   └── reports/           # query builder, raw SQL aggregates
└── jobs/                  # bull queue (email, invoice PDF)
```

## Khuyến nghị kỹ thuật

- **ORM**: TypeORM hoặc Prisma. Postgres với `uuid` PK.
- **Validation**: `class-validator` + `class-transformer` toàn cục.
- **Auth**: JWT access (15p) + refresh token (7d, lưu DB để revoke).
- **Phân quyền**: `@Permissions('order.create')` decorator + `PermissionsGuard` đọc từ `user.roles[].permissions[]`.
- **Stock atomicity**: dùng `SERIALIZABLE` transaction hoặc row-level lock (`SELECT ... FOR UPDATE`) khi trừ tồn kho lúc tạo order/PO receive — tránh oversell.
- **Order state machine**: PENDING → CONFIRMED → PACKING → SHIPPING → COMPLETED (POS đi thẳng → COMPLETED). Chỉ trừ tồn khi CONFIRMED (online) hoặc tạo (POS).
- **Audit log**: interceptor ghi lại mọi POST/PATCH/DELETE.
- **Promotion engine**: tách service `PromotionEvaluatorService` để dùng chung trong validate + tạo order.
- **Report**: dùng raw SQL với `date_trunc()` cho group by; cân nhắc materialized view nếu data lớn.
- **Pagination**: cursor-based cho `/orders` (data lớn), offset cho phần còn lại.
- **API versioning**: `app.enableVersioning({ type: VersioningType.URI })` → `/v1/...`
- **Swagger**: `@nestjs/swagger` — import file `openapi.json` đính kèm để đối chiếu.
- **Testing**: e2e với `@nestjs/testing` + Postgres test container.

## Permission keys gợi ý

```
product.read, product.create, product.update, product.delete
category.*, attribute.*, supplier.*, purchase_order.*
warehouse.*, inventory.read, inventory.adjust, inventory.transfer
customer.*, customer_group.*, promotion.*
order.read, order.create, order.update, order.cancel, order.refund
employee.*, role.*, store.*, settings.*, report.*
```

## Roles mặc định

| Role | Quyền |
|---|---|
| `super_admin` | tất cả |
| `manager` | tất cả trừ `role.*`, `settings.*` |
| `cashier` | `order.create` (POS), `customer.*`, `product.read`, `promotion.read` |
| `warehouse_staff` | `inventory.*`, `purchase_order.*`, `product.read` |
| `sales_staff` | `order.read/create/update`, `customer.*`, `product.read` |
