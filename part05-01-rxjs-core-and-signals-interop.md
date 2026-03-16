# 🏁 Part 5: LUỒNG DỮ LIỆU BẤT ĐỒNG BỘ (RXJS)

RxJS là thư viện lập trình phản ứng (Reactive Programming) giúp xử lý các luồng dữ liệu bất đồng bộ. Mặc dù Angular hiện đại đã có Signals, RxJS vẫn là vị vua độc tôn trong việc xử lý các tác vụ liên quan đến thời gian và luồng sự kiện phức tạp.

## 1. RxJS vs Signals: Cuộc chiến hay Sự cộng sinh?

***Căn bệnh hiểu lầm:***

Rất nhiều người mới lầm tưởng rằng Signals sinh ra để tiêu diệt RxJS. Nếu bạn cố gắng dùng Signals để làm tính năng "Debounce khi gõ phím tìm kiếm", bạn sẽ viết ra những đoạn code cực kỳ tồi tệ.

***Quy tắc Vàng (Tiêu chuẩn 2026):***
- **Sử dụng Signals cho STATE (Trạng thái UI)**: Dữ liệu đang hiển thị trên màn hình là gì? Nút bấm này đang bật hay tắt? (Không có khái niệm thời gian).
- **Sử dụng RxJS cho STREAMS (Luồng sự kiện)**: Gọi HTTP Request, WebSocket, lắng nghe thao tác click chuột liên tục, xử lý Debounce/Throttle. (Gắn liền với trục thời gian và có thể hủy bỏ - Cancel).

## 2. Giao Thoa Hai Thế Giới (RxJS Interop)

Angular 2026 cung cấp các API để làm cầu nối hoàn hảo giữa RxJS và Signals. Nó giúp bạn tận dụng sức mạnh xử lý luồng của RxJS, nhưng lại hiển thị ra giao diện bằng Signals để tối ưu hiệu năng (Zoneless).

### 1. `toSignal`: Biến Observable thành Signal (Cứu tinh chống Memory Leak)

**Vấn đề cũ**: Dùng `.subscribe()` thì phải nhớ `.unsubscribe()` khi component bị hủy (ngOnDestroy), nếu quên sẽ gây rò rỉ bộ nhớ. Dùng `async` pipe trên HTML thì lại khó thao tác logic trong file TypeScript.

**Cách làm mới**: Hàm `toSignal()` tự động subscribe và **tự động unsubscribe** khi component bị hủy!

```js
import { Component, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { toSignal } from '@angular/core/rxjs-interop';

@Component({
  template: `
    @if (users()) {
      <ul>
        @for (u of users(); track u.id) { <li>{{u.name}}</li> }
      </ul>
    }
  `
})
export class UserComponent {
  private http = inject(HttpClient);

  // 1. Luồng dữ liệu RxJS (Observable)
  private users$ = this.http.get<User>('/api/users');

  // 2. Chuyển thành Signal. Giao diện bind thẳng vào biến này.
  // Không cần subscribe, không lo rò rỉ bộ nhớ!
  users = toSignal(this.users$);
}
```

### 2. `outputFromObservable`: Bắn sự kiện ra ngoài Component

Khi bạn có một luồng sự kiện phức tạp (ví dụ: người dùng kéo thả chuột, hoặc scroll) và muốn Component Con bắn sự kiện này lên Component Cha, Angular cung cấp `outputFromObservable`.

```js
import { Component, ElementRef, inject } from '@angular/core';
import { fromEvent } from 'rxjs';
import { outputFromObservable } from '@angular/core/rxjs-interop';

@Component({
  selector: 'app-scroll-tracker',
  template: `<div class="box">Cuộn tôi đi</div>`
})
export class ScrollTrackerComponent {
  private el = inject(ElementRef);

  // 1. Dùng RxJS để lắng nghe sự kiện scroll trên thẻ div
  private scrollEvents$ = fromEvent(this.el.nativeElement, 'scroll');

  // 2. Chuyển Observable thành Output để Component Cha có thể (scroll)="làm_gì_đó()"
  onScroll = outputFromObservable(this.scrollEvents$);
}
```