# 🏁 Part 10: DEVOPS & AUTOMATION (Tối ưu & Tự động hóa)

## 1. Build Optimization & Bundle Analyzer (Tối ưu dung lượng)

***Căn bệnh "Bundle béo phì":***

Khi dự án lớn lên, các lập trình viên thường cài vô tội vạ các thư viện bên ngoài (như Lodash, MomentJS,...). Hậu quả là file JavaScript cuối cùng (bundle) khi build ra nặng tới hàng chục MB, khiến người dùng dùng mạng 3G/4G phải chờ mòn mỏi.

***Cách 1: Cấu hình Budgets (Ngân sách dung lượng):***

Angular cho phép bạn đặt ra "cảnh báo đỏ" ngay trong file `angular.json`. Nếu ai đó code làm file vượt quá dung lượng cho phép, quá trình Build sẽ báo lỗi và đánh rớt ngay lập tức.

***Cách 2: Khám bệnh bằng Bundle Analyzer:***

Làm sao biết thư viện nào đang ăn nhiều dung lượng nhất? Hãy dùng công cụ Bundle Analyzer.

- Chạy build tạo file thống kê: `ng build --stats-json`
- Chạy tool để xem biểu đồ: `npx webpack-bundle-analyzer dist/your-app/stats.json`
Kết quả sẽ hiển thị một biểu đồ khối trực quan, giúp bạn "trảm" ngay những thư viện rác không cần thiết.

## 2. Custom Schematics (Tự động hóa sinh code)

***Vấn đề của Team lớn:***

Mỗi khi tạo một Component mới, team của bạn quy định phải có file `.html`, `.scss`, và file `.stories.ts` (cho Storybook). Nếu dùng lệnh `ng generate component` mặc định thì không đủ, anh em lập trình viên toàn phải tự copy-paste bằng tay, vừa chậm vừa dễ sai quy chuẩn.

***Giải pháp 2026: Custom Schematics:***

Bạn có thể viết ra một kịch bản sinh code (Schematic) riêng cho công ty. Thay vì dùng lệnh mặc định của Angular, khi gõ `ng generate my-company:component`, nó sẽ tự động tạo ra đúng chuẩn các file, đúng template mẫu và cấu trúc thư mục mà Lead Developer (Sếp) yêu cầu. Điều này đảm bảo tính nhất quán (Consistency) tuyệt đối cho dự án.