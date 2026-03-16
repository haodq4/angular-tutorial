# 🏁 Part 6: HTTP, SECURITY & I18N

## 1. Giao tiếp Backend: Cấu hình mặc định & Sự trỗi dậy của `httpResource`

**A. Phòng chống XSS (Cross-Site Scripting)**

Nếu người dùng cố tình nhập mã độc `<script>Hack_Web</script>` vào ô bình luận, liệu web của bạn có bị hack khi in nó ra không?

**Tin vui:** Trong Angular, bạn không cần làm gì cả! Angular **tự động vô hiệu hóa (sanitize)** mọi đoạn mã HTML lạ khi bạn in ra bằng ``{{ }}``.


**B. Phân quyền Hiển thị (RBAC - Role-Based Access Control)**

Làm sao để giấu nút "Xóa hệ thống" nếu người dùng không phải là Admin? Rất đơn giản, hãy kết hợp `Signal` chứa thông tin User và lệnh `@if`.

## 2. Đa ngôn ngữ (i18n - Internationalization)

Nếu bạn làm dự án cho nhiều quốc gia, làm sao để web có thể đổi từ Tiếng Việt sang Tiếng Anh khi người dùng bấm nút? Thư viện `ngx-translate` là tiêu chuẩn quốc dân dễ học nhất.

**Bước 1: Cài đặt và tạo file Ngôn ngữ**

Tạo 2 file chứa chữ trong thư mục `src/assets/i18n/`:

***File `vi.json` (Tiếng Việt):***

***File `en.json` (Tiếng Anh):***

**Bước 2: Cấu hình trong `app.config.ts`**

Sử dụng hàm của thư viện để cấu hình cho ứng dụng Standalone. Bạn bắt buộc phải dùng `importProvidersFrom` để bọc cấu hình của `TranslateModule` lại.

**Bước 3: Sử dụng trên Giao diện (HTML)**

Để dịch một chữ, bạn chỉ cần gõ đúng cái "Key" (chìa khóa) mà bạn đã khai báo trong file JSON, và dùng dấu `| translate` (gọi là Pipe). Đừng quên import `TranslateModule` vào Component nhé!