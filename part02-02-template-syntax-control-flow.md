# 🏁 Part 2: NHẬP MÔN & CORE ARCHITECTURE (Kiến trúc cốt lõi)

## Nền Tảng 2: Template Syntax & Control Flow

Angular cung cấp một hệ thống HTML mở rộng (Template Syntax) cực kỳ mạnh mẽ để kết nối trực tiếp giao diện (HTML) với logic (TypeScript).

### 1. Data Binding (Cơ chế liên kết dữ liệu)

**Làm sao để HTML và TypeScript nói chuyện với nhau?**

Có 4 phương thức giao tiếp cơ bản mà bạn phải nằm lòng:

- **Hiển thị dữ liệu (Interpolation)**: Dùng `{{ }}` để in giá trị từ biến ra màn hình.

- **Truyền thuộc tính (Property Binding)**: Dùng `[ ]` để gán giá trị của biến vào thuộc tính của thẻ HTML (như `src`, `disabled`, `class`).

- **Lắng nghe sự kiện (Event Binding)**: Dùng `( )` để bắt các sự kiện từ người dùng (như `click`, `keyup`, `submit`).

- `Liên kết 2 chiều (Two-way Binding)`: Dùng `[( )]` (còn được gọi vui là "Banana in a box") để đồng bộ dữ liệu hai chiều lập tức, chủ yếu dùng trong các form nhập liệu.

```html
<h1>Xin chào, {{ username }}!</h1>

<img [src]="avatarUrl" [disabled]="isLocked">

<button (click)="saveData()">Lưu lại</button>

<input [(ngModel)]="searchKeyword" placeholder="Nhập từ khóa...">
```

### 2. Control Flow (Cú pháp luồng điều khiển mới)

**Tạm biệt `*ngIf` và `*ngFor`:**

Angular đã loại bỏ cách viết cũ kỹ, nặng nề này. Thay vào đó là cú pháp Control Flow mới (bắt đầu bằng `@`) được tích hợp sâu thẳng vào bộ máy (engine) của framework, giúp code gọn hơn, không cần import `CommonModule` và chạy nhanh hơn đáng kể.

**Cú pháp rẽ nhánh (`@if` / `@switch`):**

Dùng để hiển thị hoặc ẩn các khối giao diện dựa trên điều kiện logic cụ thể.

```html
@if (userRole === 'admin') {
  <button>Xóa hệ thống</button>
} @else if (userRole === 'editor') {
  <button>Sửa bài</button>
} @else {
  <p>Bạn chỉ có quyền xem.</p>
}
```

**Cú pháp vòng lặp (`@for`):**

Bắt buộc phải có `track`. Tính năng này giúp Angular cấp một ID duy nhất cho mỗi phần tử. Khi danh sách thay đổi, framework chỉ cập nhật đúng phần tử đó trên giao diện thay vì phá hủy và vẽ lại toàn bộ danh sách, giúp tăng hiệu năng cực độ. Khối `@empty` xử lý luôn trường hợp mảng rỗng.

```html
<ul>
  @for (user of users; track user.id) {
    <li>{{ user.name }}</li>
  } @empty {
    <li>Chưa có người dùng nào trong hệ thống.</li>
  }
</ul>
```