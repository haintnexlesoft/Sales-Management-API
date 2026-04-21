# Sales Management API — Design Spec

Bộ tài liệu thiết kế API cho phần mềm quản lý bán hàng, dùng làm blueprint khi implement bằng **NestJS**.

## Files

| File | Nội dung |
|---|---|
| `openapi.json` | OpenAPI 3.1 spec đầy đủ (import vào Swagger UI / Postman / NestJS) |
| `endpoints.md` | Bảng tổng hợp tất cả endpoints theo module |
| `erd.md` | Sơ đồ ERD (Mermaid) + danh sách bảng |
| `nestjs-structure.md` | Cấu trúc thư mục + best practices NestJS |

## Modules được thiết kế

1. **Auth** — JWT + refresh token
2. **Employees & Roles** — RBAC với permission keys
3. **Catalog** — Categories (cây), Attributes, Products + Variants
4. **Suppliers & Purchase Orders** — Nhập hàng
5. **Warehouses & Inventory** — Tồn kho, xuất/nhập/chuyển/điều chỉnh
6. **Customers & Customer Groups** — CRM cơ bản
7. **Promotions** — % / số tiền cố định / mua X tặng Y / free ship
8. **Orders** — Online + POS, state machine, hủy, refund, hóa đơn PDF
9. **Stores & Settings** — Đa cửa hàng, cấu hình
10. **Reports** — Doanh thu, top sản phẩm, tồn kho, nhân viên, khách hàng

## Cách dùng

1. Import `openapi.json` vào Swagger Editor (https://editor.swagger.io) để xem trực quan.
2. Dùng OpenAPI Generator để sinh skeleton NestJS:
   ```bash
   npx @openapitools/openapi-generator-cli generate \
     -i openapi.json -g typescript-axios -o ./client
   ```
3. Tham khảo `nestjs-structure.md` để dựng project.
