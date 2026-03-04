# 🏁 Part 3: LOGIC, SERVICES & SIGNALS

Giai đoạn này giúp ứng dụng của bạn trở nên chuyên nghiệp hơn. Nguyên tắc cốt lõi của Angular hiện đại: Component chỉ nên đóng vai trò là "bộ mặt" để hiển thị HTML. Mọi logic tính toán phức tạp, lưu trữ trạng thái (state) hoặc gọi API **bắt buộc** phải được đẩy ra một nơi khác để tái sử dụng. Nơi đó gọi là Service.

## Nền Tảng 1: Services & Dependency Injection (DI)

### 1. Services & Singleton Pattern

**Service thực chất là gì?:**

Nó đơn giản là một class TypeScript thông thường chứa các hàm xử lý logic nghiệp vụ. Việc tách code ra Service giúp bạn tuân thủ nguyên tắc DRY (Don't Repeat Yourself) – viết một lần, dùng ở hàng chục Component khác nhau.

**Singleton Pattern (Kẻ độc tôn toàn hệ thống):**

Khi bạn gắn cờ `providedIn: 'root'` lên trên đầu một Service, Angular sẽ chỉ tạo ra **duy nhất một bản sao (instance)** của Service này cho toàn bộ ứng dụng.

- **Chia sẻ dữ liệu (State Sharing)**: Nếu Component A cập nhật giỏ hàng trong Service, Component B gọi Service đó ra sẽ thấy ngay dữ liệu mới nhất, vì cả hai đang chọc vào cùng một vùng nhớ.
- **Tối ưu bộ nhớ**: Không sinh ra hàng loạt object dư thừa khi người dùng chuyển trang.

```js
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root' // Biến Service này thành Singleton (Kẻ độc tôn)
})
export class CartService {
  // Trạng thái giỏ hàng được dùng chung cho toàn App
  totalItems = signal<number>(0);

  addToCart() {
    this.totalItems.update(val => val + 1);
  }
}
```

### 2. Dependency Injection: Sự trỗi dậy của hàm `inject()`

Dependency Injection (DI) là cơ chế Angular "bơm" (inject) các Service vào trong Component để bạn sử dụng, thay vì bạn phải tự khởi tạo bằng tay (ví dụ: `new CartService()`).

**Căn bệnh của Constructor Injection (Cách cũ):**

Trước đây, bạn bắt buộc phải khai báo Service bên trong tham số của hàm `constructor`. Khi một Component con kế thừa Component cha, bạn phải hì hục truyền lại toàn bộ các Service này thông qua hàm `super(...)`. Điều này tạo ra một "địa ngục boilerplate" cực kỳ mệt mỏi và dễ dính lỗi khi dự án lớn lên.

**Cách làm hiện đại (Hàm `inject()`):**

Từ Angular v14 trở đi, hàm `inject()` ra đời và trở thành tiêu chuẩn vàng. Nó giúp code của bạn gọn gàng, đọc mượt hơn và loại bỏ hoàn toàn mớ bòng bong khi kế thừa class.

**Constructor Injection (Cách cũ - Không nên dùng):**

- **Nhược điểm**: Mã lặp rườm rà, kế thừa class rất khổ sở.

```js
@Component({...})
export class ProductCardComponent {
  // Phải nhét mọi thứ vào tham số constructor
  constructor(
    private cartService: CartService,
    private router: Router
  ) {}
}
```

**Sử dụng `inject()` (Cách mới - Khuyên dùng 100%):**

- **Ưu điểm**: Khai báo trực tiếp như một thuộc tính bình thường, cực kỳ sạch sẽ và dễ refactor (cấu trúc lại code).

```js
import { Component, inject } from '@angular/core';
import { Router } from '@angular/router';
import { CartService } from './cart.service';

@Component({
  selector: 'app-product-card',
  template: `<button (click)="buy()">Thêm vào giỏ</button>`
})
export class ProductCardComponent {
  // Bơm Service vào Component cực gọn gàng bằng inject()
  private cartService = inject(CartService);
  private router = inject(Router);

  buy() {
    this.cartService.addToCart();
    this.router.navigate(['/checkout']);
  }
}
```