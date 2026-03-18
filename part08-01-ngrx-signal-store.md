# 🏁 Part 8: KIẾN TRÚC MỞ RỘNG (ARCHITECTURE)

Khi ứng dụng lớn lên, việc truyền dữ liệu qua lại giữa hàng chục Component bằng input() và output() sẽ tạo ra một mớ bòng bong (Prop Drilling). Giải pháp là dùng một "Kho chứa dữ liệu trung tâm" (Global Store).

## 1. NgRx Signal Store: Quản lý State Tiêu Chuẩn 2026

***Căn bệnh của NgRx truyền thống (Redux pattern):***

Ngày xưa, để dùng NgRx, bạn phải tạo 5 file khác nhau (Actions, Reducers, Effects, Selectors, State) chỉ để làm một tính năng bé tí. Cực kỳ nhiều mã lặp (boilerplate) và học rất mệt.

***Cách làm hiện đại (NgRx Signal Store):***

Được ra mắt và hoàn thiện để đón đầu kỷ nguyên Angular Signals, **NgRx Signal Store** cực kỳ nhẹ, không cần RxJS (trừ khi gọi API), và code ngắn hơn 70% so với cách cũ. Nó gom tất cả vào một hàm `signalStore`.

***Ví dụ Thực Chiến: Xây dựng Kho chứa "Quản lý Công Việc" (TaskStore)***

Dưới đây là cách bạn tạo một Global Store dùng chung cho toàn bộ dự án:

Cách Component lấy Kho dữ liệu ra dùng: