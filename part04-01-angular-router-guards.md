# 🏁 Part 4: ROUTING & FORMS

Bất kỳ ứng dụng web thực tế nào cũng cần chuyển trang (Routing) và cho phép người dùng nhập liệu (Forms). Trong Angular 2026, hai khái niệm này đã được lột xác hoàn toàn: Routing trở nên siêu nhẹ nhờ Functional Guards, và Forms đang bước vào kỷ nguyên của Signal Forms.

## Nền Tảng 1: Angular Router, Lazy Loading & Guards

Điều hướng trong Angular hiện đại (Standalone) cực kỳ trực quan. Bạn cấu hình mọi thứ trong file `app.routes.ts` thay vì phải bọc trong các Routing Modules lằng nhằng.

### 1. Routing Cơ Bản & RouterLink

***Làm sao để chuyển trang mà không bị tải lại (reload) toàn bộ web?***

Tuyệt đối không dùng thuộc tính `href` mặc định của thẻ `<a>`. Hãy dùng `routerLink`.

```html
<a href="/login">Đăng nhập</a>

<a routerLink="/login" routerLinkActive="active-class">Đăng nhập</a>

<router-outlet></router-outlet>
```

### 2. Lazy Loading (Tải lười)

***Tại sao phải Lazy Load?***

Nếu dự án có 100 trang, bạn không thể ép người dùng tải toàn bộ code của 100 trang đó ngay lần đầu vào web (bundle size khổng lồ). Lazy loading giúp: Người dùng vào trang nào, Angular mới tải code của trang đó về.

***Cú pháp Standalone (Cực gọn):***

Với Standalone Component, bạn dùng `loadComponent` và truyền vào một hàm `import()` động.

```js
// app.routes.ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';

export const routes: Routes =;
```

### 3. Route Guards (Bảo vệ luồng điều hướng)

***Route Guard là gì?***

Là những "người bảo vệ" đứng trước cửa mỗi trang. Ví dụ: Phải có Token đăng nhập mới được vào trang `/admin` (CanActivate). Có form đang nhập dở, hỏi người dùng có chắc chắn muốn thoát không (CanDeactivate).

***Căn bệnh của Class-based Guards (Cách cũ):***

Ngày xưa, mỗi Guard là một Class đồ sộ triển khai interface `CanActivate`, bắt buộc phải tạo file riêng và khai báo lằng nhằng.

***Cách làm hiện đại (Functional Guards + `inject()`):***

Angular 14+ đã khai tử Class Guards. Giờ đây, Guard chỉ là một **function bình thường** kết hợp với hàm `inject()`. Bạn có thể viết Guard ngay trong file routes!

```js
// app.routes.ts
import { inject } from '@angular/core';
import { Router, Routes } from '@angular/router';
import { AuthService } from './auth.service';

export const routes: Routes =); // Đá văng ra trang đăng nhập
          return false;
        }
      }
    ]
  }
];
```