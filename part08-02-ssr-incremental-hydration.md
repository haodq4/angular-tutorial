# 🏁 Part 8: KIẾN TRÚC MỞ RỘNG (ARCHITECTURE)X

## 2. Server-Side Rendering (SSR) & Incremental Hydration

Tối ưu hóa thời gian tải trang (Load time) và SEO là yêu cầu bắt buộc của các dự án lớn.

***Vấn đề của SSR cũ (Hydration toàn phần):***

Khi load web bằng SSR, server sẽ trả về một cục HTML tĩnh cực nhanh cho người dùng xem. Nhưng để các nút bấm click được, Angular phải chạy một tiến trình gọi là "Hydration" (Bù nước) – tức là gắn JavaScript vào các thẻ HTML đó.
Trước đây, Angular bắt trình duyệt tải và "bù nước" cho **toàn bộ trang web cùng một lúc**, khiến web bị đơ (đóng băng) mất 1-2 giây lúc mới vào.

***Giải pháp 2026: Incremental Hydration (Bù nước tăng dần):***

Angular cho phép bạn "chia nhỏ" tiến trình này. Phần HTML nào người dùng đang nhìn thấy thì bù nước trước. Phần nào ở tuốt dưới chân trang (Footer) thì từ từ hẵng bù nước!

**Kích hoạt Hydration Từng Phần với `@defer`**

Sử dụng cú pháp `@defer` kết hợp với các bộ kích hoạt `hydrate on`. Tính năng này giúp các trang web thương mại điện tử khổng lồ load mượt như lụa.

Lưu ý: Để dùng được tính năng này, bạn phải cấu hình `provideClientHydration(withIncrementalHydration())` trong `app.config.ts`.