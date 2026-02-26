# 🏁 Part 1: NỀN TẢNG BẮT BUỘC (PREREQUISITES)

## Nền Tảng 2: Decorators & Lập Trình Bất Đồng Bộ

Để hiểu cách Angular lắp ráp các thành phần lại với nhau, bạn buộc phải hiểu về Decorators. Đồng thời, vì web app luôn phải gọi API, kỹ năng xử lý Bất đồng bộ (Async) là không thể thiếu.

### 1. Decorator (`@`): Ma thuật cốt lõi của Angular

Nếu bạn thấy một từ khóa có dấu `@` đằng trước (ví dụ: `@Component`, `@Injectable`), đó là một Decorator.

Bản chất Decorator chỉ là một **hàm Javascript thông thường**, được dùng để "đính kèm" thêm các siêu dữ liệu (metadata) hoặc sửa đổi hành vi của một Class, Hàm, hoặc Thuộc tính ngay tại thời điểm bạn khai báo nó.

`Ví dụ`: Bạn viết một class `UserProfile`. Angular sẽ không biết class này để làm gì cho đến khi bạn "dán nhãn" `@Component` lên nó.

```js
// Đây là cách bạn dán nhãn cho Angular biết: 
// "Hãy lấy class này, nối nó với file HTML và CSS tương ứng nhé!"
@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.css']
})
export class UserProfileComponent {
  username = 'Alex';
}
```

### 2. Lập trình bất đồng bộ: `async / await`

Trong phát triển Web, khi bạn gửi yêu cầu (Request) lên máy chủ để lấy dữ liệu, trình duyệt sẽ không "đứng hình" để chờ. Nó sẽ tiếp tục chạy các dòng code khác. Khi dữ liệu về, nó mới xử lý. Đây gọi là Bất đồng bộ (Asynchronous).

Trước đây, người ta dùng `.then().catch()` (Promises), nhưng nó dễ dẫn đến tình trạng code bị lồng ghép quá sâu (Callback Hell). Cú pháp `async/await` ra đời để giúp code bất đồng bộ trông giống hệt như code chạy tuần tự từ trên xuống dưới.

```js
// ❌ CÁCH CŨ: Dùng then/catch (Khó đọc khi logic phức tạp)
function fetchUser() {
  httpClient.get('/api/users/1')
   .then(user => {
      console.log(user);
    })
   .catch(error => {
      console.error(error);
    });
}

// ✅ CÁCH MỚI: Dùng async/await + try/catch (Trực quan, dễ debug)
async function fetchUserModern() {
  try {
    // Code sẽ "tạm dừng" ở dòng này cho đến khi lấy được data
    const user = await httpClient.get('/api/users/1'); 
    console.log(user);
  } catch (error) {
    console.error('Lỗi lấy dữ liệu:', error);
  }
}
```