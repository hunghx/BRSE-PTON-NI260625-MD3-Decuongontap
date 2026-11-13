# ĐỀ THỬ **JAVA FUNDAMENTAL – JDBC (PHIÊN BẢN THỬ)**

**Thời gian:** 180 phút **Tổng điểm:** 100

---

## 1️⃣ YÊU CẦU CHUNG
- **Ngôn ngữ / Framework:** Java SE (Console), MySQL.
- **IDE:** IntelliJ IDEA / MySQL Workbench.
- **Repo GitHub:** Tạo repository theo cú pháp `[MãLớp]_[HọTên]` (ví dụ `CS01_2024_LeVanB`).
- **Các file trong repo:**
  - `src/` – mã nguồn Java.
  - `structure.sql` – script tạo CSDL và các stored procedure.
  - `README.md` – hướng dẫn biên dịch và chạy.
- **Lưu ý:** Không sử dụng AI để viết code. Mã phải do bạn tự thực hiện.

---

## 2️⃣ BÀI TẬP THỰC HÀNH

### 2.1. Thiết kế CSDL
- **Tên DB:** `db_movie_rental`
- **Bảng:** `rental_records` (phiếu thuê phim)

| Column          | Kiểu dữ liệu   | Ràng buộc   | Mô tả |
|----------------|----------------|-------------|------|
| `rental_id`    | `SERIAL`       | `PK`        | Mã phiếu thuê |
| `movie_title`  | `VARCHAR(150)` | `NOT NULL`  | Tên phim |
| `customer_name`| `VARCHAR(100)` | `NOT NULL`  | Tên khách thuê |
| `rent_date`    | `TIMESTAMP`    | `NOT NULL`  | Ngày thuê |
| `due_date`     | `TIMESTAMP`    | `NOT NULL`  | Hạn trả |
| `quantity`     | `INT`          | `NOT NULL`  | Số bản sao thuê |
| `status`       | `VARCHAR(30)`  | `NOT NULL`  | `Rented`, `Returned`, `Late` |

> **Ghi chú:** `SERIAL` trong MySQL ≈ `BIGINT UNSIGNED NOT NULL AUTO_INCREMENT`.

### 2.2. Stored Procedures (MySQL)
| STT | Tên Procedure | Mô tả |
|-----|---------------|------|
| a | `sp_get_all_rentals` | Trả về toàn bộ phiếu thuê. |
| b | `sp_add_rental` | Thêm một phiếu thuê mới (các tham số: `movie_title`, `customer_name`, `rent_date`, `due_date`, `quantity`, `status`). |
| c | `sp_get_rentals_by_customer` | Nhận `IN customer_name` → trả về các phiếu của khách đó. |
| d | `sp_update_rental` | Cập nhật các trường (ngoại trừ `rental_id`) dựa trên `IN rental_id`. |
| e | `sp_delete_rental` | Xóa phiếu dựa trên `IN rental_id`. |
| f | `sp_search_by_movie_title` | Nhận chuỗi tìm kiếm → trả về các phiếu mà `movie_title` **chứa** chuỗi (case‑insensitive). |

> Các procedure nên dùng `IN`/`OUT` tham số hợp lý và `SELECT …` để trả về `ResultSet`.

### 2.3. Ứng dụng Java Console
Xây dựng một chương trình Java console hiển thị menu và thực thi các chức năng sau (sử dụng JDBC và các stored procedure trên):

```
================= MOVIE RENTAL MANAGEMENT =================
1. Xem danh sách tất cả phiếu thuê
2. Thêm mới phiếu thuê
3. Cập nhật thông tin phiếu thuê
4. Xóa phiếu thuê
5. Tìm kiếm phiếu thuê theo tên khách
6. Tìm kiếm phiếu thuê theo tên phim
7. Thoát
```

---

## 3️⃣ ĐÁNH GIÁ (100 điểm)
| STT | Nội dung | Tiêu chí | Điểm |
|-----|----------|----------|------|
| 1 | **CSDL & Script** | Đúng tên DB, bảng, ràng buộc, và tạo đầy đủ stored procedure. | 10 |
| 2 | **Stored Procedure** | Đúng cú pháp, tham số, và `SELECT`/`INSERT`/`UPDATE`/`DELETE` hợp lý. | 15 |
| 3 | **Kết nối JDBC** | Phương thức `getConnection()` đúng URL, username/password, dùng `try‑with‑resources`. | 8 |
| 4 | **Hiển thị danh sách** | Gọi `sp_get_all_rentals`, in ra mọi cột, format rõ ràng. | 10 |
| 5 | **Thêm phiếu** | Gọi `sp_add_rental`, nhập dữ liệu từ bàn phím, validate (không null, `quantity > 0`, ngày hợp lệ). | 20 |
| 6 | **Cập nhật phiếu** | Gọi `sp_update_rental` dựa trên `rental_id`, không cho phép sửa `rental_id`. Validate giống mục 5. | 20 |
| 7 | **Xóa phiếu** | Gọi `sp_delete_rental`, yêu cầu xác nhận người dùng trước khi thực hiện. | 12 |
| 8 | **Tìm kiếm theo khách** | Gọi `sp_get_rentals_by_customer`, không phân biệt hoa/thường. | 5 |
| 9 | **Tìm kiếm theo phim** | Gọi `sp_search_by_movie_title`, hỗ trợ tìm gần đúng, không phân biệt hoa/thường. | 5 |
|10 | **Thoát** | Đóng mọi tài nguyên (Connection, Statements) an toàn, không báo lỗi. | 5 |
|**Tổng**| | | **100** |

---

## 4️⃣ HƯỚNG DẪN NỘP BÀI
1. **GitHub:** Đẩy toàn bộ thư mục dự án (có `src/`, `structure.sql`, `README.md`) lên repo đã tạo.
2. **File nộp:** Gửi link repo qua hệ thống LMS/Canvas/Google Classroom (theo yêu cầu giảng viên).
3. **Kiểm tra:** Trước khi nộp, chạy `structure.sql` trên MySQL Workbench để xác nhận không có lỗi, sau đó chạy chương trình Java để kiểm tra mọi chức năng.

---

## 5️⃣ MỤC LƯU Ý
- **Transaction:** Không bắt buộc trong đề này – bạn không cần triển khai `commit/rollback`.
- **Exception handling:** Sử dụng `try‑with‑resources` để tự động đóng `Connection`, `CallableStatement`, `ResultSet`.
- **Không sao chép:** Đề nghị tự viết code, tránh copy mẫu từ Internet/AI.

Chúc các em ôn luyện hiệu quả và đạt kết quả tốt!
