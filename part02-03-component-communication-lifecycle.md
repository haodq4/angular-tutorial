# 🏁 Part 2: NHẬP MÔN & CORE ARCHITECTURE (Kiến trúc cốt lõi)

## Nền Tảng 3: Component Communication & Lifecycle Hooks

Khi một ứng dụng lớn lên, bạn không thể nhét mọi thứ vào một component. Bạn phải chia nhỏ chúng ra và thiết lập đường dây liên lạc giữa chúng.

### 1. Giao tiếp Component (Tư duy Signal API Mới)

**Thay thế cho `@Input` và `@Output` kiểu cũ:**

Angular 2026 sử dụng **Signal API** (`input()` và `output()`) để truyền nhận dữ liệu. Cách mới này an toàn kiểu (type-safe) tuyệt đối, tự động cập nhật UI và dễ đọc hơn rất nhiều.

- **Component Con**: Dùng `input()` để nhận dữ liệu. Có thể ép buộc cha phải truyền data bằng `input.required()`. Dùng `output()` để bắn các sự kiện ngược lên cho cha.
- **Component Cha**: Dùng ngoặc vuông `[ ]` để truyền data xuống con, và ngoặc tròn `( )` để lắng nghe sự kiện từ con bắn lên.

```js
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-product-card',
  template: `
    <h3>{{ productName() }}</h3> 
    <p>Giá: {{ price() }}</p>

    <button (click)="buy.emit(productName())">Mua ngay</button>
  `
})
export class ProductCardComponent {
  // Bắt buộc Component cha phải truyền tên sản phẩm
  productName = input.required<string>(); 
  
  // Input tùy chọn, nếu cha không truyền thì mặc định là 0
  price = input<number>(0); 
  
  // Nơi phát sự kiện lên cha
  buy = output<string>(); 
}
```

### 2. Template Variables & Content Projection

**Template Variables (`#var`):**

Là kỹ thuật "gắn thẻ/đặt tên" cho một phần tử HTML ngay trên giao diện. Nó cho phép bạn lấy giá trị hoặc gọi hàm của phần tử đó từ một vị trí khác mà không cần viết code TypeScript phức tạp.

```html
<input #phoneInput type="text" placeholder="Nhập số điện thoại...">
<button (click)="call(phoneInput.value)">Gọi ngay</button>
```

***Content Projection (`<ng-content>`):**

Cho phép bạn tạo ra các Component dạng "khung" (như Modal, Card) và nhét nội dung HTML tùy ý vào giữa chúng từ bên ngoài.

```html
<div class="card">
  <div class="header">
    <ng-content select="[card-title]"></ng-content> 
  </div>
  <div class="body">
    <ng-content></ng-content> 
  </div>
</div>

<app-card>
  <h2 card-title>Đây là tiêu đề chèn vào header</h2>
  <p>Đây là nội dung chèn vào thẻ body.</p>
</app-card>
```

### 3. Lifecycle Hooks (Vòng đời Component)

**Mọi component đều có sinh ra và chết đi:**

Angular cho phép bạn "móc" (hook) vào các thời khắc quan trọng trong vòng đời của một component để thực thi logic.

- `ngOnInit`: Chạy ngay sau khi component khởi tạo. **Nơi lý tưởng nhất để gọi API lấy dữ liệu lần đầu.**

- `ngOnDestroy`: Chạy ngay trước khi component bị tiêu hủy. **Bắt buộc dùng để "dọn rác"** (hủy setInterval, sự kiện lắng nghe window) để ngăn chặn lỗi rò rỉ bộ nhớ (memory leak).

- `afterNextRender` **(Chuẩn mới thay cho ngAfterViewInit)**: Chạy sau khi toàn bộ HTML (DOM) đã được vẽ xong. Hook này an toàn tuyệt đối với SSR (Server-Side Rendering) vì nó đảm bảo code của bạn chỉ chạy trên trình duyệt (nơi có Window/Document) thay vì chạy trên máy chủ. Rất phù hợp để tích hợp thư viện biểu đồ như Chart.js.

```js
import { Component, OnInit, OnDestroy, afterNextRender } from '@angular/core';

@Component({...})
export class DemoComponent implements OnInit, OnDestroy {
  
  constructor() {
    // API mới đảm bảo chỉ chạy trên trình duyệt, tránh lỗi SSR
    afterNextRender(() => {
      console.log('UI đã vẽ xong. Khởi tạo Chart.js tại đây!');
    });
  }

  ngOnInit() {
    console.log('Component đã sẵn sàng! Tiến hành gọi API.');
  }

  ngOnDestroy() {
    console.log('Component sắp bị hủy! Tiến hành dọn dẹp bộ nhớ.');
  }
}
```