# 🏁 Part 3: LOGIC, SERVICES & SIGNALS

## Nền Tảng 2: Angular Signals (Kỷ Nguyên Phản Ứng Mới)

Trước đây, Angular phụ thuộc nặng nề vào thư viện RxJS để quản lý trạng thái, nhưng nó có đường cong học tập quá dốc và dễ gây rò rỉ bộ nhớ. Giờ đây, Signals chính thức trở thành "trái tim" của mô hình phản ứng trong Angular hiện đại.

### 1. Ba Nguyên Thủy Cốt Lõi Của Signals

##### 1. Writable Signal (`signal`) - Nguồn sự thật duy nhất:

Đây là một "vỏ bọc" chứa dữ liệu. Bạn có quyền đọc và thay đổi nó. Khi dữ liệu bên trong thay đổi, nó sẽ tự động "hét lên" để Angular biết chính xác vị trí DOM nào cần vẽ lại.

##### 2. Computed Signal (`computed`) - Trạng thái phái sinh:

Tự động tính toán lại một giá trị mới dựa trên các Signal gốc. Điểm thiên tài của nó là sự **ghi nhớ (memoized)**: nếu dữ liệu gốc không đổi, nó sẽ trực tiếp trả về kết quả cũ mà không thèm tính lại, giúp tiết kiệm CPU tuyệt đối.

##### 3. Effect (`effect`) - Phản ứng phụ:

Một đoạn code sẽ tự động chạy bất cứ khi nào Signal mà nó đang "đọc" bị thay đổi. Dùng để làm các việc bên ngoài luồng UI như: lưu LocalStorage, vẽ Chart, hoặc ghi log.

```js
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <p>Giá trị: {{ count() }}</p>
    <p>Nhân đôi: {{ doubleCount() }}</p>
    <button (click)="increase()">Tăng</button>
  `
})
export class CounterComponent {
  // 1. Writable Signal
  count = signal<number>(0);

  // 2. Computed Signal (Tự động nhân đôi mỗi khi count thay đổi)
  doubleCount = computed(() => this.count() * 2);

  constructor() {
    // 3. Effect (Tự động in log mỗi khi count bị thay đổi)
    effect(() => {
      console.log(`Giá trị hiện tại đang là: ${this.count()}`);
    });
  }

  increase() {
    // update() dùng khi giá trị mới phụ thuộc vào giá trị cũ
    this.count.update(val => val + 1);
    
    // NẾU muốn ghi đè giá trị tĩnh luôn thì dùng: this.count.set(10);
  }
}
```

### 2. Các Trụ Cột Mới Nhất (Từ Angular v19+)

##### API `linkedSignal` là gì?

**Căn bệnh đồng bộ trạng thái:**
Đôi khi bạn nhận dữ liệu từ Component cha truyền xuống (thông qua `input`), và bạn muốn lưu nó vào một biến cục bộ để người dùng chỉnh sửa. Nhưng nếu cha truyền dữ liệu mới xuống, biến cục bộ của bạn phải tự động reset theo. Ngày xưa, bạn phải dùng lifecycle hook ngOnChanges rất phức tạp.

**Giải pháp** `linkedSignal`:
Angular sinh ra `linkedSignal` để giải quyết triệt để bài toán này. Nó tạo ra một Signal cục bộ (có thể `.set()` được), nhưng nó tự động gài "chế độ tự hủy/reset" mỗi khi nguồn dữ liệu gốc thay đổi.

```js
import { Component, input, linkedSignal } from '@angular/core';

@Component({
  selector: 'app-user-edit',
  template: `
    <input 
      [ngModel]="editableName()" 
      (ngModelChange)="editableName.set($event)">
  `
})
export class UserEditComponent {
  // Nguồn dữ liệu từ Component Cha (ví dụ: ID thay đổi từ 1 -> 2)
  userId = input.required<string>();

  // editableName cho phép người dùng sửa đổi thoải mái.
  // NHƯNG nếu userId từ cha đổi sang số khác, nó sẽ lập tức reset lại thành User_idMoi
  editableName = linkedSignal(() => `User_${this.userId()}`);
}
```

### 3. Giao Thoa Với RxJS (Interoperability)

***Có Signals rồi, có nên vứt bỏ RxJS không?:***

**KHÔNG**. Bạn cần hiểu rõ ranh giới:

- **Signals**: Sinh ra để quản lý **Trạng thái (State)** đồng bộ (ví dụ: đang mở modal nào, số đếm là bao nhiêu).

- **RxJS**: Sinh ra để quản lý **Dòng chảy sự kiện (Events/Streams)** bất đồng bộ (ví dụ: debounce người dùng gõ phím, xử lý websocket, gọi HTTP request).

***Chuyển đổi qua lại cực kỳ dễ dàng:***

Angular cung cấp hai hàm `toSignal` và `toObservable` để làm cầu nối giữa hai thế giới này.

```js
import { Component, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { toSignal } from '@angular/core/rxjs-interop';

@Component({
  template: `
    @if (users()) {
      <p>Có {{ users()?.length }} người dùng.</p>
    }
  `
})
export class UserListComponent {
  private http = inject(HttpClient);

  // Gọi API (trả về Observable của RxJS)
  private users$ = this.http.get<User>('/api/users');

  // Chuyển Observable thành Signal bằng toSignal()
  // Angular sẽ TỰ ĐỘNG subscribe và TỰ ĐỘNG unsubscribe giùm bạn!
  users = toSignal(this.users$); 
}
```