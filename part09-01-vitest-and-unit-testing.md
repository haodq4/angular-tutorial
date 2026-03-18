# 🏁 Part 9: CHẤT LƯỢNG & TESTING (Unit Test)

Testing (Viết test) là ranh giới phân biệt giữa một dự án "làm cho vui" và một dự án "triệu đô". Khi code của bạn có test, bạn có thể tự tin sửa code, nâng cấp Angular mà không sợ làm sập hệ thống.

## 1. Sự thoái trào của Karma và Kỷ nguyên Vitest

***Tạm biệt Karma & Jasmine:***

Hơn một thập kỷ qua, Angular mặc định dùng Karma để chạy test. Tuy nhiên nó quá chậm và cấu hình rất cồng kềnh. Từ Angular v21, Karma đã chính thức bị gỡ bỏ khỏi các dự án tạo mới.

***Vị Vua mới - Vitest:***

Angular 2026 mặc định sử dụng Vitest. Nó chạy cực nhanh, hỗ trợ hoàn hảo TypeScript và ES6+ mà không cần cấu hình phức tạp. Lệnh chạy vẫn giữ nguyên: `ng test`.

## 2. Viết Unit Test cho Component (Signals & Zoneless)

**Căn bệnh của test cũ:**

Ngày xưa bạn phải gọi `fixture.detectChanges()` liên tục ở mọi nơi để ép Angular cập nhật giao diện, nếu quên 1 dòng là test tạch.

***Test kiểu mới (Zoneless 2026):***

Vì chúng ta dùng Signals và kiến trúc Phi vùng (Zoneless), mọi thứ trở nên trực quan hơn rất nhiều. Bạn chỉ cần thay đổi giá trị Signal và dùng `await fixture.whenStable()` để chờ Angular tự động cập nhật xong mọi thứ.

## 3. Test việc gọi API (Mocking httpResource)

Khi test Component có gọi API, **tuyệt đối không được gọi API thật**. Bạn phải tạo ra một "máy chủ giả" (Mock Backend) để chặn request và trả về data giả. Angular cung cấp `HttpTestingController` làm chuyện này cực kỳ mượt mà.