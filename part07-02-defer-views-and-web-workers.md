# 🏁 Part 7: HIỆU NĂNG NÂNG CAO (PERFORMANCE)

Giao diện tải quá chậm? Trình duyệt bị đơ khi xử lý tính toán nặng? Đây là 2 vũ khí giúp bạn giải quyết dứt điểm.

## 1. Deferrable Views (Tải lười giao diện với `@defer`)

***Lazy Loading cấp độ Component:***

Trước đây, ta chỉ có thể Lazy Load (tải chậm) từng Trang web (Routing). Còn bây giờ với `@defer`, bạn có thể tải chậm từng Khối giao diện (Component) ngay trên cùng một trang!

***Ví dụ thực tế:***

Bạn có một trang Dashboard, bên dưới cùng có một Component Biểu đồ khổng lồ tốn rất nhiều MB. Đừng ép người dùng tải file JS của biểu đồ đó ngay khi vừa vào web! Hãy dùng `@defer` để khi nào người dùng **cuộn chuột (scroll)** tới đúng chỗ đó thì mới tải.

**Các điều kiện kích hoạt (`on...`) hay dùng nhất:**
    - `on viewport`: Cuộn chuột tới nơi mới tải (Tốt nhất cho SEO và Tốc độ load lần đầu).
    - `on hover`: Rê chuột vào mới tải.
    - `on interaction`: Click vào mới tải (Rất hợp làm Modal/Popup).
    - `on idle`: Đợi trình duyệt rảnh rỗi không làm gì thì âm thầm tải nền.

## 2. Web Workers (Tách luồng xử lý tính toán)

***Sự thật phũ phàng về JavaScript:***

JavaScript chạy trên **Đơn luồng (Single Thread)**. Nghĩa là tại một thời điểm, nó chỉ làm được 1 việc. Nếu bạn bắt nó xử lý một vòng lặp 1 tỷ lần, toàn bộ giao diện web sẽ bị "đơ" (freeze), người dùng không thể cuộn trang hay bấm nút.

***Giải pháp: Web Workers:***

Web Worker là cách bạn thuê một "nhân viên chạy việc vặt" ở dưới background. Bạn ném cho nó cục dữ liệu nặng, nó tự tính toán trong góc kín mà không làm đơ giao diện. Tính xong nó trả kết quả về cho bạn.

***Cách dùng cực dễ trong Angular:***

    - Mở Terminal gõ lệnh tạo Worker (Angular sẽ tự tạo file và cấu hình hệ thống build): `ng generate web-worker shared/math`
    - Viết code xử lý nặng trong file math.worker.ts:
    - Sử dụng trong Component: