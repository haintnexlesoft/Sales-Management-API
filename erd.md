# Sơ đồ ERD (Entity Relationship)

```mermaid
erDiagram
    USER ||--o{ USER_ROLE : has
    ROLE ||--o{ USER_ROLE : has
    ROLE ||--o{ ROLE_PERMISSION : has
    PERMISSION ||--o{ ROLE_PERMISSION : has
    USER ||--o| EMPLOYEE : extends
    EMPLOYEE }o--|| STORE : works_at
    STORE ||--|| WAREHOUSE : default

    CATEGORY ||--o{ CATEGORY : parent
    CATEGORY ||--o{ PRODUCT : contains
    SUPPLIER ||--o{ PRODUCT : supplies
    PRODUCT ||--|{ PRODUCT_VARIANT : has
    ATTRIBUTE ||--o{ ATTRIBUTE_VALUE : has
    PRODUCT_VARIANT ||--o{ VARIANT_ATTRIBUTE : has
    ATTRIBUTE_VALUE ||--o{ VARIANT_ATTRIBUTE : has

    WAREHOUSE ||--o{ STOCK_ITEM : holds
    PRODUCT_VARIANT ||--o{ STOCK_ITEM : tracked
    WAREHOUSE ||--o{ STOCK_MOVEMENT : logs
    PRODUCT_VARIANT ||--o{ STOCK_MOVEMENT : involves

    SUPPLIER ||--o{ PURCHASE_ORDER : receives
    PURCHASE_ORDER ||--|{ PURCHASE_ORDER_ITEM : has
    PRODUCT_VARIANT ||--o{ PURCHASE_ORDER_ITEM : refs

    CUSTOMER }o--o| CUSTOMER_GROUP : belongs
    CUSTOMER ||--o{ ORDER : places
    EMPLOYEE ||--o{ ORDER : created
    STORE ||--o{ ORDER : at
    ORDER ||--|{ ORDER_ITEM : has
    PRODUCT_VARIANT ||--o{ ORDER_ITEM : refs
    ORDER }o--o| PROMOTION : applies
    ORDER ||--o{ PAYMENT : has
    ORDER ||--o{ SHIPMENT : has
```

## Bảng chính

| Bảng | Mô tả |
|---|---|
| `users` | Tài khoản đăng nhập (cả khách hàng & nhân viên nội bộ) |
| `employees` | Hồ sơ nhân viên, FK -> users |
| `roles`, `permissions`, `user_roles`, `role_permissions` | Phân quyền RBAC |
| `categories` | Danh mục cây (self-ref `parent_id`) |
| `attributes`, `attribute_values` | Thuộc tính (Color, Size...) |
| `products` | Sản phẩm gốc |
| `product_variants` | Biến thể có SKU/giá riêng |
| `variant_attributes` | Liên kết variant ↔ attribute_value |
| `warehouses` | Kho hàng |
| `stock_items` | Tồn kho hiện tại theo (warehouse, variant) |
| `stock_movements` | Lịch sử IN/OUT/TRANSFER/ADJUSTMENT |
| `suppliers` | Nhà cung cấp |
| `purchase_orders`, `purchase_order_items` | Đơn nhập hàng |
| `customers`, `customer_groups` | Khách hàng & nhóm |
| `promotions` | Khuyến mại (PERCENT/FIXED/BXGY/FREE_SHIP) |
| `orders`, `order_items` | Đơn bán (channel: ONLINE/POS) |
| `payments` | Thanh toán đơn |
| `shipments` | Vận chuyển đơn online |
| `stores` | Cửa hàng vật lý |
| `settings` | Cấu hình hệ thống (key/value) |
| `audit_logs` | Lịch sử thao tác |
