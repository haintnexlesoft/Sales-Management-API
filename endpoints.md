# Danh sách Endpoints

> Base URL: `/v1` — Auth: `Bearer <JWT>` (trừ `/auth/login`, `/auth/refresh`)

## 🔐 Auth
| Method | Path | Mô tả |
|---|---|---|
| POST | `/auth/login` | Đăng nhập email/password |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Đăng xuất (revoke refresh) |
| GET  | `/auth/me` | Thông tin user hiện tại + permissions |
| POST | `/auth/change-password` | Đổi mật khẩu |

## 👥 Nhân viên & Phân quyền
| Method | Path | Permission |
|---|---|---|
| GET/POST | `/employees` | `employee.read` / `employee.create` |
| GET/PATCH/DELETE | `/employees/{id}` | `employee.*` |
| GET/POST | `/roles` | `role.*` |
| GET/PATCH/DELETE | `/roles/{id}` | `role.*` |
| GET | `/permissions` | `role.read` |

## 📦 Catalog
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/categories` | CRUD danh mục |
| GET | `/categories/tree` | Cây danh mục |
| GET/POST | `/attributes` | CRUD thuộc tính + values |
| GET/POST | `/products` | CRUD sản phẩm + variants |
| GET | `/products/{id}/variants` | Danh sách biến thể |
| POST | `/products/import` | Import Excel/CSV |

## 🏭 Nhà cung cấp & Đơn nhập
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/suppliers` | CRUD NCC |
| GET/POST | `/purchase-orders` | Tạo phiếu nhập (DRAFT) |
| POST | `/purchase-orders/{id}/receive` | Nhập kho → cộng tồn |

## 🏬 Kho hàng
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/warehouses` | CRUD kho |
| GET | `/inventory/stock` | Tra cứu tồn kho |
| GET | `/inventory/movements` | Lịch sử biến động |
| POST | `/inventory/adjust` | Điều chỉnh tồn (kiểm kê) |
| POST | `/inventory/transfer` | Chuyển kho |

## 🧑 Khách hàng
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/customers` | CRUD KH |
| GET/POST | `/customer-groups` | Nhóm KH (kèm % giảm giá) |
| GET | `/customers/{id}/orders` | Lịch sử mua |

## 🎟️ Khuyến mại
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/promotions` | CRUD chương trình KM |
| POST | `/promotions/validate` | Kiểm tra mã & tính giảm giá |

## 🛒 Đơn hàng
| Method | Path | Mô tả |
|---|---|---|
| GET | `/orders` | Lọc theo channel, status, date |
| GET | `/orders/{id}` | Chi tiết |
| POST | `/orders/online` | Tạo đơn từ website |
| POST | `/orders/pos` | Tạo đơn tại quầy (auto thanh toán) |
| PATCH | `/orders/{id}/status` | Đổi trạng thái (workflow) |
| POST | `/orders/{id}/cancel` | Hủy + hoàn tồn kho |
| POST | `/orders/{id}/refund` | Hoàn tiền |
| GET | `/orders/{id}/invoice` | Xuất PDF hóa đơn |

## 🏪 Cửa hàng & Cài đặt
| Method | Path | Mô tả |
|---|---|---|
| GET/POST | `/stores` | CRUD cửa hàng |
| GET/PUT | `/settings` | Cấu hình hệ thống |

## 📊 Báo cáo
| Method | Path | Mô tả |
|---|---|---|
| GET | `/reports/sales` | Doanh thu theo ngày/tuần/tháng |
| GET | `/reports/top-products` | Sản phẩm bán chạy |
| GET | `/reports/inventory` | Tồn kho hiện tại |
| GET | `/reports/employees` | Doanh số nhân viên |
| GET | `/reports/customers` | Top KH theo chi tiêu |
