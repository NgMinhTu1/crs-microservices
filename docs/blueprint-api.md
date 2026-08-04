# Blueprint API - CRS Microservices

## 1. auth-service (Cổng 8081 | Tiền tố Gateway: /api/auth)
| Method | Endpoint | Mô tả | Yêu cầu |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/login` | Đăng nhập, trả về JWT | Public |
| `POST` | `/auth/register` | Đăng ký tài khoản (tùy chọn) | Public |

## 2. course-service (Cổng 8082 | Tiền tố Gateway: /api/courses)
| Method | Endpoint | Mô tả | Yêu cầu |
| :--- | :--- | :--- | :--- |
| `GET` | `/courses` | Danh sách môn học, có search + phân trang | Public |
| `GET` | `/courses/{id}` | Chi tiết 1 môn học | Public |
| `POST` | `/courses` | Thêm môn học | ADMIN |
| `PUT` | `/courses/{id}` | Sửa môn học | ADMIN |
| `DELETE` | `/courses/{id}` | Xóa môn học | ADMIN |

### API Nội Bộ (Chỉ gọi từ registration-service, KHÔNG lộ ra Gateway cho Frontend)
| Method | Endpoint | Mô tả |
| :--- | :--- | :--- |
| `PATCH` | `/internal/courses/{id}/reserve-seat` | Kiểm tra còn chỗ, trừ `soChoConLai` (transactional) |
| `PATCH` | `/internal/courses/{id}/release-seat` | Hoàn trả 1 chỗ khi hủy đăng ký |

## 3. registration-service (Cổng 8083 | Tiền tố Gateway: /api/registrations)
| Method | Endpoint | Mô tả | Yêu cầu |
| :--- | :--- | :--- | :--- |
| `POST` | `/registrations` | Đăng ký học phần (gọi ngầm sang course-service) | STUDENT |
| `GET` | `/registrations/my` | Danh sách đăng ký của tôi | STUDENT |
| `DELETE` | `/registrations/{id}` | Hủy đăng ký (gọi ngầm release-seat) | STUDENT / ADMIN |