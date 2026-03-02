# 🏁 Part 2: NHẬP MÔN & CORE ARCHITECTURE (Kiến trúc cốt lõi)

Giai đoạn này đánh dấu bước chân đầu tiên của bạn vào thế giới Angular hiện đại. Chúng ta sẽ bỏ qua những kiến thức cũ kỹ của các phiên bản trước (Angular 14 trở xuống) và đi thẳng vào kiến trúc cốt lõi tinh gọn được định hình cho năm 2026.

## Nền Tảng 1: Project Setup & Standalone Components

Khởi đầu với Angular hiện đại không còn đáng sợ với hàng tá cấu hình rườm rà. Mọi thứ giờ đây được tối giản hóa tối đa nhờ kiến trúc Standalone.

### 1. Khởi tạo dự án & Cấu hình (Project Setup)

**Cách khởi tạo nhanh nhất:**

Sử dụng Angular CLI - công cụ dòng lệnh chính thức. Từ Angular 17 trở đi, CLI tự động setup dự án theo chuẩn mới nhất (không dùng Module) và cấu hình sẵn công cụ build cực nhanh.

```sh
# Cài đặt Angular CLI toàn cầu
npm install -g @angular/cli

# Khởi tạo dự án mới (Mặc định Standalone & Zoneless ready)
ng new my-modern-app
```

**Trái tim của dự án (`app.config.ts` thay cho `app.module.ts`):**

Trong kiến trúc mới, file `app.module.ts` đã bị khai tử. Thay vào đó, toàn bộ cấu hình cấp ứng dụng (như Routing, gọi API) được khai báo tại `app.config.ts`. Nó sử dụng hàm `ApplicationConfig` chứa mảng `providers` để cấu hình hệ thống.

```js
// app.config.ts
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient, withFetch } from '@angular/common/http';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers:
};
```

### 2. Standalone Components (Kẻ hủy diệt NgModule)

**Căn bệnh phình to của ``NgModule``:**

Trước đây, mọi Component đều phải được "báo cáo" trong mảng `declarations` của `app.module.ts`. Khi dự án lớn lên, file này biến thành một "mớ bòng bong" phức tạp, gây khó khăn cho việc quản lý và chia nhỏ dung lượng tải trang (Lazy loading).

***Tiêu chuẩn hiện tại (Standalone Components):***

Mỗi Component giờ đây là một thực thể ***độc lập***. Nó tự quyết định việc nó cần dùng những gì thông qua mảng `imports` ngay bên trong Decorator `@Component`.

- ***Tính đóng gói cao***: Component nào cần thư viện nào thì tự import đúng thư viện đó.

- ***Dễ tái sử dụng***: Bốc component từ dự án này sang dự án khác cực kỳ dễ dàng.

```js
import { Component } from '@angular/core';
import { NgClass } from '@angular/common'; // Import chính xác thứ cần dùng
import { UserAvatarComponent } from './user-avatar.component';

@Component({
  selector: 'app-user-profile',
  standalone: true, // Đánh dấu đây là component độc lập (bắt buộc)
  imports: [NgClass, UserAvatarComponent], // Import các component/directive khác vào đây
  template: `
    <div class="profile-card" [ngClass]="{'active': isActive}">
      <app-user-avatar></app-user-avatar>
      <h2>Trang cá nhân</h2>
    </div>
  `
})
export class UserProfileComponent {
  isActive = true;
}
```