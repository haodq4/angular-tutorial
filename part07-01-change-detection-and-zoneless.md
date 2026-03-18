# 🏁 Part 7: HIỆU NĂNG NÂNG CAO (PERFORMANCE)

Trong phần này, chúng ta sẽ tìm hiểu cách Angular cập nhật giao diện và cách ép nó không được làm việc thừa thãi.

## 1. Change Detection (Cơ chế phát hiện thay đổi)

***Change Detection là gì?***

Hiểu đơn giản: Khi dữ liệu (biến) trong file TypeScript thay đổi, Angular phải quét qua file HTML để cập nhật lại giao diện. Quá trình quét này gọi là Change Detection.

***Căn bệnh quét dư thừa (Default Strategy):***

Mặc định, khi bạn click một nút bấm ở Component Con, Angular sẽ chạy đi quét lại toàn bộ các Component từ Cha xuống Con trong ứng dụng để xem có ai thay đổi không. Nếu ứng dụng có 1000 phần tử, việc này sẽ làm giật lag trình duyệt.

***Thuốc giải: `OnPush` Strategy:***

Đây là "luật bất thành văn" của dự án lớn. Khi bật `OnPush`, bạn cấm Angular tự động quét Component này. Nó sẽ CHỈ quét lại và cập nhật giao diện khi:

    - Dữ liệu từ Component cha truyền xuống (qua input()) bị thay đổi.
    - Một Signal bên trong Component bị thay đổi.
    - Bạn chủ động kích hoạt sự kiện click ngay trên Component đó.

## 2. Kỷ nguyên Zoneless (Kiến trúc Phi vùng)

***Zone.js là gì và tại sao nó bị "khai tử"?***

Từ xưa đến nay, Angular dùng một thư viện tên là `Zone.js`. Thư viện này âm thầm theo dõi mọi cú click chuột, mọi hàm `setTimeout`, mọi API được gọi. Cứ có gì nhúc nhích là nó hét lên: "Angular ơi, quét giao diện đi!".

***Nhược điểm:*** Nó quét quá nhiều (hao pin, tốn CPU), làm nặng file web tải về, và lỗi văng ra (stack traces) rất khó debug.

***Zoneless (Tiêu chuẩn của Angular 2026):***

Bắt đầu từ phiên bản 21, dự án tạo mới sẽ ***mặc định không dùng Zone.js nữa***. Angular sẽ không tự đi đoán xem khi nào cần cập nhật UI, mà nó chờ bạn báo cáo rõ ràng thông qua ***Signals*** hoặc thao tác trực tiếp.

***Cách kích hoạt Zoneless (nếu nâng cấp từ dự án cũ):***

Trong file `app.config.ts`, bạn thêm `provideZonelessChangeDetection()`.

**⚠️ Lưu ý sống còn khi code Zoneless:**
Nếu bạn dùng các biến kiểu cũ để hứng dữ liệu bất đồng bộ (như gọi API xong gán vào biến), giao diện sẽ bị "đóng băng". **Bắt buộc phải dùng Signal.**