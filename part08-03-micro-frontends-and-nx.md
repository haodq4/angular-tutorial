# 🏁 Part 8: KIẾN TRÚC MỞ RỘNG (ARCHITECTURE)X

## 3. Micro Frontends với Nx & Module Federation

***Bài toán của các tập đoàn (Big Tech):***

Bạn có một ứng dụng Siêu App (như Shopee hoặc Momo) gồm 5 mảng lớn: Mua sắm, Đặt vé xe, Ví tiền, Chat, Game. Nếu 500 lập trình viên cùng code chung vào 1 source code duy nhất (Monolith), mỗi lần build sẽ mất cả tiếng đồng hồ, và một người làm lỗi có thể sập cả hệ thống.

***Giải pháp: Micro Frontends (MFE):***

Chia nhỏ Siêu App đó thành 5 App con độc lập. Team Mua Sắm tự code, tự deploy App Mua Sắm. Team Ví Tiền tự deploy App Ví Tiền. Người dùng vẫn tưởng họ đang dùng 1 web duy nhất, nhưng thực chất ở bên dưới, Angular đang lôi từng App con ghép lại với nhau ngay trên trình duyệt (Module Federation).

**Công cụ tiêu chuẩn: Nx Monorepo**

Trong giới Angular, không ai tự setup Micro Frontends bằng tay vì nó quá phức tạp. Tiêu chuẩn ngành là sử dụng **Nx** (một công cụ quản lý Workspace khổng lồ).

**Cách Nx tổ chức một dự án MFE:**

- **Host App (Vỏ bọc)**: Là cái khung web (chứa Header, Sidebar và Menu điều hướng).
- **Remote Apps (App con)**: Các chức năng tách rời (như `shop-app`, `admin-app`).

**Luồng điều hướng cực đỉnh của Host App:**

Trong file `app.routes.ts` của ứng dụng Host, nó không trỏ tới Component, mà nó **tải thẳng cả một App khác** thông qua mạng!

Lợi ích cốt lõi: Team Shop có thể tự cập nhật code, tự đưa lên production 10 lần/ngày mà không cần xin phép hay làm ảnh hưởng đến Team Admin.