# REST API là gì? 7 Nguyên Tắc Quan Trọng Khi Thiết Kế API Chuẩn

> Tài liệu giới thiệu REST API dành cho người mới học Backend: dễ hiểu, dễ áp dụng, chuẩn thực tế dự án.

Trong hầu hết các hệ thống web hiện đại, API đóng vai trò là cầu nối giữa frontend, backend, mobile app và các service khác. Trong số nhiều kiểu API, **REST API** là chuẩn được sử dụng phổ biến nhất nhờ tính đơn giản, dễ mở rộng và phù hợp với kiến trúc web.

Bài viết này giúp bạn hiểu rõ **REST API là gì**, cách REST hoạt động trong dự án thực tế, và **7 nguyên tắc quan trọng để thiết kế API chuẩn**, dễ bảo trì và dễ mở rộng.

## Mục lục

- [REST API là gì?](#rest-api-là-gì)
- [Resource trong REST API là gì?](#resource-trong-rest-api-là-gì)
- [7 nguyên tắc thiết kế REST API chuẩn](#7-nguyên-tắc-thiết-kế-rest-api-chuẩn)
  - [1) Sử dụng đúng HTTP Method](#1-sử-dụng-đúng-http-method)
  - [2) Trả về HTTP Status Code phù hợp](#2-trả-về-http-status-code-phù-hợp)
  - [3) Thiết kế URL theo resource, không theo hành động](#3-thiết-kế-url-theo-resource-không-theo-hành-động)
  - [4) Versioning API ngay từ đầu](#4-versioning-api-ngay-từ-đầu)
  - [5) Áp dụng pagination cho danh sách dữ liệu](#5-áp-dụng-pagination-cho-danh-sách-dữ-liệu)
  - [6) Hỗ trợ filter và sort rõ ràng](#6-hỗ-trợ-filter-và-sort-rõ-ràng)
  - [7) Chuẩn hoá format trả lỗi](#7-chuẩn-hoá-format-trả-lỗi)
- [Ví dụ REST API đơn giản](#ví-dụ-rest-api-đơn-giản)
- [REST API có cần đúng 100% lý thuyết không?](#rest-api-có-cần-đúng-100-lý-thuyết-không)
- [Kết luận](#kết-luận)
- [Tài nguyên từ ITPrep](#tài-nguyên-từ-itprep)

---

## REST API là gì?

**API (Application Programming Interface)** là giao diện cho phép các hệ thống phần mềm giao tiếp với nhau.

**REST (Representational State Transfer)** là một phong cách kiến trúc định nghĩa cách tổ chức và truy cập tài nguyên thông qua HTTP.

Vì vậy, **REST API** là API được thiết kế theo phong cách REST, trong đó:

- Mỗi đối tượng trong hệ thống được xem là một tài nguyên (**resource**)
- Các thao tác lên tài nguyên được thực hiện thông qua **HTTP method**
- Dữ liệu trao đổi thường ở dạng **JSON**
- Server hoạt động theo mô hình **stateless**

## Resource trong REST API là gì?

**Resource** là khái niệm cốt lõi trong REST. Thay vì thiết kế API theo hành động, REST yêu cầu bạn thiết kế API theo **danh từ** (tài nguyên).

Ví dụ với tài nguyên `users`:

- `GET /users`: lấy danh sách người dùng
- `GET /users/1`: lấy thông tin người dùng có `id = 1`
- `POST /users`: tạo người dùng mới
- `PATCH /users/1`: cập nhật thông tin người dùng
- `DELETE /users/1`: xoá người dùng

Thiết kế này giúp API dễ hiểu, dễ đoán và nhất quán trong toàn bộ hệ thống.

## 7 nguyên tắc thiết kế REST API chuẩn

### 1) Sử dụng đúng HTTP Method

HTTP method thể hiện rõ mục đích của request:

- `GET`: lấy dữ liệu
- `POST`: tạo mới tài nguyên
- `PUT`: thay thế toàn bộ tài nguyên
- `PATCH`: cập nhật một phần tài nguyên
- `DELETE`: xoá tài nguyên

Trong dự án thực tế, `PATCH` thường được dùng nhiều hơn `PUT` vì chỉ cập nhật các trường cần thiết.

### 2) Trả về HTTP Status Code phù hợp

Status code giúp frontend và client xử lý logic chính xác mà không cần phụ thuộc vào message text.

Một số status code thường dùng:

- `200 OK`: request thành công
- `201 Created`: tạo mới thành công
- `204 No Content`: xoá thành công
- `400 Bad Request`: dữ liệu gửi lên không hợp lệ
- `401 Unauthorized`: chưa xác thực
- `403 Forbidden`: không có quyền
- `404 Not Found`: không tìm thấy tài nguyên
- `409 Conflict`: xung đột dữ liệu
- `422 Unprocessable Entity`: validate thất bại
- `500 Internal Server Error`: lỗi server

### 3) Thiết kế URL theo resource, không theo hành động

URL nên phản ánh tài nguyên, không nên chứa động từ.

Ví dụ tốt:

- `/users`
- `/users/{id}`
- `/users/{id}/orders`

Ví dụ không nên dùng:

- `/getUserList`
- `/createUser`
- `/deleteUserById`

Thiết kế đúng giúp API dễ mở rộng và dễ maintain về lâu dài.

### 4) Versioning API ngay từ đầu

API trong dự án thực tế luôn thay đổi theo thời gian. Versioning giúp nâng cấp API mà không phá vỡ hệ thống cũ.

Cách phổ biến:

- `/api/v1/users`
- `/api/v2/users`

Đây là best practice gần như bắt buộc với hệ thống production.

### 5) Áp dụng pagination cho danh sách dữ liệu

Không nên trả toàn bộ dữ liệu trong một request.

Ví dụ:

```http
GET /users?page=1&limit=20
```

Response nên bao gồm thông tin phân trang:

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 120
  }
}
```

Pagination giúp API ổn định và tránh quá tải.

### 6) Hỗ trợ filter và sort rõ ràng

API thực tế thường cần lọc và sắp xếp dữ liệu.

Ví dụ:

```http
GET /users?status=active
GET /users?role=admin
GET /users?sort=-created_at
```

Cách này linh hoạt và dễ mở rộng hơn so với việc tạo thêm endpoint mới.

### 7) Chuẩn hoá format trả lỗi

Tất cả lỗi nên tuân theo một cấu trúc thống nhất.

Ví dụ:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is invalid",
    "fields": {
      "email": "Invalid email format"
    }
  }
}
```

Chuẩn hoá error response giúp frontend xử lý lỗi và debug nhanh hơn.

## Ví dụ REST API đơn giản

### Tạo người dùng

```http
POST /users
Content-Type: application/json

{
  "name": "MNfine",
  "email": "mnfine@example.com"
}
```

### Response

```json
{
  "id": 1,
  "name": "MNfine",
  "email": "mnfine@example.com"
}
```

## REST API có cần đúng 100% lý thuyết không?

Trong thực tế, không phải hệ thống nào cũng tuân thủ REST một cách tuyệt đối. Điều quan trọng là:

- URL theo resource
- Dùng đúng HTTP method và status code
- Request và response nhất quán

Chỉ cần đảm bảo ba yếu tố này, API của bạn đã đạt chuẩn sử dụng trong hầu hết dự án thực tế.

## Kết luận

REST API là nền tảng quan trọng đối với Backend Developer. Khi bạn hiểu rõ cách thiết kế resource, sử dụng HTTP method hợp lý và tổ chức API nhất quán, bạn không chỉ viết API chạy được mà còn xây dựng được hệ thống dễ mở rộng, dễ bảo trì.

Nếu bạn đang học Backend Python, bước tiếp theo rất hợp lý là tìm hiểu cách triển khai REST API bằng **FastAPI** hoặc **Django REST Framework**, cũng như so sánh ưu nhược điểm của từng framework trong dự án thực tế.

## Tài nguyên từ ITPrep

- Website chính thức: https://itprep.com.vn/rest-api-7-nguyen-tac-thiet-ke-api-chuan/
- YouTube chính thức: https://www.youtube.com/watch?v=waxxHyX-4J4

---

**Từ khoá liên quan:** REST API, thiết kế REST API, HTTP method, status code, API versioning, pagination API, filter sort API, backend developer, FastAPI, Django REST Framework.
