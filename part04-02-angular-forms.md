# 🏁 Part 4: ROUTING & FORMS

## Nền Tảng 2: Kỷ Nguyên Biểu Mẫu (Reactive Forms & Signal Forms)

Xử lý Form luôn là phần "khoai" nhất trong Frontend: Quản lý giá trị, báo lỗi, trạng thái người dùng đã chạm vào ô input chưa (touched/dirty)... Angular cung cấp những công cụ tối tân nhất để xử lý việc này.

### 1. Reactive Forms (Tiêu chuẩn Doanh nghiệp hiện tại)

***Tại sao không dùng Template-driven forms (dùng `[(ngModel)]`)?***

Template-driven forms chỉ hợp cho form có 1-2 ô input đơn giản. Với ứng dụng Enterprise, form rất phức tạp. **Reactive Forms** đẩy toàn bộ logic quản lý form vào file TypeScript, giúp code dễ test, dễ theo dõi sự thay đổi (Subscribe) và an toàn kiểu (Typed Forms).

```js
import { Component, inject } from '@angular/core';
import { ReactiveFormsModule, FormBuilder, Validators } from '@angular/forms';

@Component({
  selector: 'app-register',
  standalone: true,
  imports:, // Bắt buộc phải import module này
  template: `
    <form [formGroup]="registerForm" (ngSubmit)="onSubmit()">
      <input type="text" formControlName="username" placeholder="Tên đăng nhập">
      
      @if (registerForm.controls.username.invalid && registerForm.controls.username.touched) {
        <small class="error">Tên đăng nhập là bắt buộc (min 3 ký tự)</small>
      }

      <button type="submit" [disabled]="registerForm.invalid">Đăng ký</button>
    </form>
  `
})
export class RegisterComponent {
  private fb = inject(FormBuilder);

  // Khởi tạo form với FormBuilder (nonNullable giúp tránh lỗi null/undefined)
  registerForm = this.fb.nonNullable.group({
    username: ['', [Validators.required, Validators.minLength(3)]],
    password: ['', Validators.required]
  });

  onSubmit() {
    // Lấy toàn bộ dữ liệu an toàn kiểu (Typed)
    console.log(this.registerForm.getRawValue()); 
  }
}
```

### 2. Signal Forms (Tương lai của Angular 2026)

Mặc dù Reactive Forms rất mạnh, nhưng nó đi kèm quá nhiều mã lặp (boilerplate - như `FormGroup`, `FormControl`) và vẫn còn dính dáng đến RxJS.

Từ bản **Angular v21**, Google đã tung ra **Signal Forms** (hiện ở mức thử nghiệm - Developer Preview nhưng chắc chắn là xu hướng tương lai). Nó xóa sổ hoàn toàn boilerplate và đồng bộ 100% bằng Signals.

**Sự khác biệt cốt lõi của Signal Forms**:

Bạn không cần tạo Control hay Group nữa. Bạn chỉ cần tạo một `signal()` chứa dữ liệu gốc, truyền nó vào hàm `form()`, và gắn nó lên HTML bằng chỉ thị `[formField]`.

```js
import { Component, signal } from '@angular/core';
// Import từ thư viện mới
import { form, FormField } from '@angular/forms/signals'; 

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [FormField], // Import directive formField mới
  template: `
    <input type="email" [formField]="loginForm.email" placeholder="Email">
    <input type="password" [formField]="loginForm.password" placeholder="Mật khẩu">
    
    <button (click)="submit()">Login</button>
  `
})
export class LoginComponent {
  // 1. Dữ liệu gốc (Nguồn sự thật)
  loginModel = signal({
    email: '',
    password: ''
  });

  // 2. Tạo Form Tree trực tiếp từ model
  loginForm = form(this.loginModel);

  submit() {
    // 3. Đọc dữ liệu cực kỳ tự nhiên như một object
    console.log('Email:', this.loginForm.email.value());
    console.log('Password:', this.loginForm.password.value());
  }
}
```

***Tóm tắt thực chiến 2026:***

- Nếu bạn vào làm (maintain) dự án cũ hoặc cần độ ổn định tuyệt đối: Hãy master **Reactive Forms**.
- Nếu bạn tạo dự án mới cứng từ Angular v21 trở đi và thích sự tinh gọn: Hãy bắt đầu ứng dụng **Signal Forms**.