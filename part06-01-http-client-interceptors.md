# 🏁 Part 6: HTTP, SECURITY & I18N

Ứng dụng của bạn không thể sống cô lập. Nó cần lấy dữ liệu từ Backend. Angular cung cấp các công cụ gọi API mạnh mẽ nhất trong thế giới Frontend, và ở phiên bản hiện đại, chúng đã được thiết kế lại để nhẹ hơn và phản ứng (reactive) hơn bao giờ hết.

## 1. Giao tiếp Backend: Cấu hình mặc định & Sự trỗi dậy của `httpResource`

### 1. Cấu hình `HttpClient` chuẩn 2026

***Tạm biệt HttpClientModule:***

Trong kiến trúc Standalone, bạn không import module nữa. Thay vào đó, bạn cấu hình HTTP client ở cấp độ toàn cục trong file `app.config.ts`. Đặc biệt, **bắt buộc phải thêm** `withFetch()` để Angular sử dụng API Fetch hiện đại của trình duyệt, giúp tăng tốc độ đáng kể và tối ưu tuyệt đối cho Server-Side Rendering (SSR).

### 2. Cuộc cách mạng `httpResource` (Thay thế `.subscribe()`)

***Căn bệnh cũ***: Dùng `HttpClient.get()` sẽ trả về Observable. Bạn phải subscribe nó, hoặc dùng `async` pipe. Nếu tham số thay đổi (như gõ từ khóa tìm kiếm), bạn phải dùng các pipe lằng nhằng của RxJS như `switchMap`.

***Giải pháp 2026 (`httpResource`):***
