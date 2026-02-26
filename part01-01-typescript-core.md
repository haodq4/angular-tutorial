# 🏁 Part 1: NỀN TẢNG BẮT BUỘC (PREREQUISITES)

## Nền Tảng 1: TypeScript Cốt Lõi (Modern TypeScript)

Angular được xây dựng hoàn toàn bằng TypeScript. Việc nắm vững TypeScript không chỉ giúp code ít lỗi hơn mà còn giúp bạn tận dụng được các tính năng "nhắc code" (IntelliSense) cực mạnh của các IDE. Dưới đây là những khái niệm sống còn.

### 1. Interface VS Type
Khi muốn định nghĩa "hình dáng" của một dữ liệu (ví dụ: User có id, name, age), bạn dùng `Interface` hay `Type`? Cả hai đều làm được, nhưng chúng có thế mạnh riêng.

- `Interface`: Sinh ra để mô phỏng các thực thể (Objects/Classes) theo hướng **đối tượng** (OOP). Điểm mạnh tuyệt đối của nó là khả năng kế thừa rất tự nhiên bằng từ khóa `extends`.
- `Type`: Là công cụ đa năng mang hơi hướng **hàm học (Functional)**. Type mạnh nhất khi bạn cần kết hợp các kiểu dữ liệu phức tạp (Union - Kiểu này HOẶC kiểu kia, Intersection - Kết hợp các kiểu).

**Công thức thực chiến**: Dùng `Interface` cho hầu hết các Object (Mô hình dữ liệu, Payload API). Dùng `Type` khi cần gom nhóm nhiều trạng thái (Union Strings) hoặc viết các kiểu logic phức tạp.

```js
// --- DÙNG TYPE CHO UNION (Interface không làm được) ---
// Biến Status chỉ được phép nhận 1 trong 3 chuỗi này
type Status = 'pending' | 'approved' | 'rejected';

// --- DÙNG INTERFACE CHO OBJECT ---
interface User {
  id: string;
  name: string;
  status: Status; // Sử dụng Type Status ở trên
}

// --- KẾ THỪA RẤT GỌN VỚI INTERFACE ---
interface Admin extends User {
  role: 'superadmin' | 'moderator';
}
```


### 2. Enum vs Literal Types: Tại sao nên bỏ Enum?

Trong các ngôn ngữ khác, Enum dùng để liệt kê các hằng số. Nhưng trong TypeScript, Enum đang bị giới chuyên gia hạn chế sử dụng.

**Vấn đề**: Khi đoạn code TypeScript được dịch sang JavaScript để chạy trên trình duyệt, các kiểu dữ liệu như `Interface` hay `Type` sẽ bốc hơi hoàn toàn (Zero overhead). Nhưng `Enum` thì không! Nó tự động sinh ra những Object JavaScript và các hàm ánh xạ ngược (reverse mapping) cực kỳ cồng kềnh, làm nặng file code của bạn một cách vô ích.

**Giải pháp tối ưu nhất**: Sử dụng **Literal** Types (Union Strings) kết hợp với Object và `as const`.

```js
// ❌ CÁCH CŨ (Dùng Enum - Cồng kềnh, sinh code thừa)
enum OrderStatus {
  Draft = 'DRAFT',
  Paid = 'PAID'
}

// ✅ CÁCH MỚI (Dùng Literal Types - Sạch, nhẹ, tự động nhắc code)
type OrderStatusType = 'DRAFT' | 'PAID';

// NẾU BẠN CẦN LƯU MÃ SỐ (Ví dụ HTTP Status Codes)
// Dùng Object kết hợp 'as const' để khóa chết giá trị (Read-only)
const HTTP_CODES = {
  NotFound: 404,
  Success: 200,
  Error: 500
} as const;

// Trích xuất Type tự động từ Object trên
type HttpCode = typeof HTTP_CODES; 
// TypeScript sẽ tự hiểu HttpCode là: 404 | 200 | 500
```

### 3. Generics: Viết code một lần, dùng mọi nơi

Generics (`<T>`) giống như việc bạn truyền "biến số" cho kiểu dữ liệu. Thay vì ép cứng một hàm chỉ nhận `string` hay `number`, Generics cho phép hàm tự động "đoán" xem bạn đang truyền vào cái gì, và trả về đúng kiểu đó.

```js
// Hàm lấy phần tử đầu tiên của mảng. 
// <T> đại diện cho bất kỳ kiểu dữ liệu nào.
function getFirstElement<T>(arr: T): T {
  return arr;
}

// Khi dùng, TypeScript đủ thông minh để tự nội suy kiểu:
const so = getFirstElement([1, 2, 3]); // 'so' tự động là number
const chu = getFirstElement(['a', 'b']); // 'chu' tự động là string
```

### 4. Utility Types: "Bùa chú" nhào nặn dữ liệu

Bạn có một interface `Post` với 20 thuộc tính. Bây giờ bạn cần viết chức năng "Chỉnh sửa bài viết" (Update), người dùng chỉ cần gửi lên những trường họ muốn sửa. Chẳng lẽ bạn phải viết lại một interface mới với 20 thuộc tính đi kèm dấu `?` (optional)?

Utility Types giải quyết chuyện này trong 1 nốt nhạc mà không làm code bị lặp (DRY).

- `Partial<T>`: Biến mọi thuộc tính của T thành không bắt buộc.

- `Pick<T, K>`: Bốc đúng những thuộc tính bạn cần từ T.

- `Omit<T, K>`: Giữ lại toàn bộ T, nhưng vứt đi một vài thuộc tính.

```js
interface Post {
  id: string;
  title: string;
  content: string;
  createdAt: Date;
}

// API Update chỉ cần các trường optional
type UpdatePayload = Partial<Post>; 
// { id?: string, title?: string, content?: string, createdAt?: Date }

// Component giao diện Card Thu Nhỏ chỉ cần lấy title và content
type PostCard = Pick<Post, 'title' | 'content'>;

// Khi tạo bài mới, không cần truyền id và createdAt (Server sẽ tự tạo)
type CreatePayload = Omit<Post, 'id' | 'createdAt'>;
```