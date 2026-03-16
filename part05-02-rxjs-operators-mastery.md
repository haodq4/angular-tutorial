# 🏁 Part 5: LUỒNG DỮ LIỆU BẤT ĐỒNG BỘ (RXJS)

Sức mạnh thực sự của RxJS nằm ở các Operators (Toán tử). Chúng giống như các đường ống dẫn nước, giúp bạn lọc, biến đổi, và nhào nặn luồng dữ liệu trước khi nó chảy đến điểm cuối (giao diện).

## 1. Pipeable Operators Cơ Bản

Mọi toán tử đều phải được bọc bên trong hàm `.pipe()`.

- `map`: Biến đổi dữ liệu (Giống map của Array).
- `filter`: Chặn dữ liệu không thỏa mãn điều kiện.
- `take(n)`: Chỉ lấy đúng `n` giá trị đầu tiên rồi tự động ngắt kết nối (complete).
- `distinctUntilChanged`: Chặn không cho emit nếu giá trị mới giống hệt giá trị ngay trước đó.

```js
// Lắng nghe input, chỉ lấy text dài hơn 3 ký tự, không lặp lại từ cũ
searchInput$.pipe(
  filter(text => text.length > 3),
  distinctUntilChanged(),
  map(text => text.toUpperCase())
).subscribe(console.log);
```

## 2. Flattening Operators

Đây là câu hỏi phỏng vấn số 1 về RxJS. Khi bạn có một Observable bên trong một Observable khác (Ví dụ: Bấm 1 nút -> Gọi 1 API), bạn phải dùng Flattening Operators. Nếu dùng nhầm, app của bạn sẽ gặp lỗi cực kỳ khó debug.

Hãy ghi nhớ câu thần chú sau:

***`mergeMap` (Nhập vào - Chạy song song):***
- **Cách thức**: Nhận bao nhiêu request, nó gọi API bấy nhiêu cái cùng một lúc.
- **Dùng khi nào?**: Khi bạn muốn các tác vụ chạy song song không đợi nhau. Ví dụ: Chọn 5 file và bấm nút "Upload tất cả".

***`switchMap` (Hủy cũ - Lấy mới):***
- **Cách thức**: Đang gọi API 1 mà có request số 2 bay vào -> Nó lập tức HỦY API 1 và chỉ chạy API 2.
- **Dùng khi nào?**: Tính năng **Search Autocomplete**. Người dùng gõ "A" (gọi API 1), gõ tiếp "B" thành "AB" (gọi API 2). Rõ ràng ta không cần kết quả của chữ "A" nữa, hủy nó đi cho đỡ kẹt mạng!

***`concatMap` (Xếp hàng - Tuần tự):***
- **Cách thức**: Bắt buộc phải chạy xong request 1 thì mới được phép chạy tiếp request 2. Xếp hàng nghiêm ngặt.
- **Dùng khi nào?**: Khi **thứ tự** là yếu tố sống còn. Ví dụ: Sửa, rồi Lưu, rồi Gửi email báo cáo. Không thể gửi email khi chưa lưu xong.

***`exhaustMap` (Bỏ qua cái mới khi đang bận):***
- **Cách thức**: Đang chạy request 1 mà có request 2 bay vào -> Nó **LỜ ĐI** (ignore) request 2. Chạy xong request 1 mới chịu nhận lệnh mới.
- **Dùng khi nào?**: Chống Spam Click! Người dùng bấm nút "Đăng nhập" hoặc "Thanh toán" 10 lần liên tục. `exhaustMap` sẽ chỉ xử lý cú click đầu tiên và phớt lờ 9 cú click sau cho đến khi API phản hồi.

```js
// VÍ DỤ: TÍNH NĂNG TÌM KIẾM CHUẨN MỰC
this.searchTerm$.pipe(
  debounceTime(300), // Đợi user ngừng gõ 300ms mới chạy
  distinctUntilChanged(), // Nếu chữ không đổi thì không chạy
  switchMap(keyword => this.http.get(`/api/search?q=${keyword}`)) // Hủy API cũ nếu user gõ tiếp
).subscribe(results => {
  this.searchResults.set(results);
});
```

## 3. Error Handling (Xử lý lỗi với `catchError`)

***Căn bệnh chết Stream:***

Trong RxJS, nếu một Observable bị lỗi và bạn không bắt nó, **toàn bộ luồng (stream) sẽ bị chết** và không bao giờ phản ứng với thao tác của người dùng nữa.

**Cách xử lý chuẩn xác (`catchError`):**

Phải sử dụng `catchError` để bắt lỗi và trả về một luồng Observable thay thế (ví dụ: trả về mảng rỗng `of()` hoặc hiển thị thông báo lỗi), giúp stream chính vẫn sống sót.

```js
this.searchTerm$.pipe(
  switchMap(keyword => 
    this.http.get(`/api/search?q=${keyword}`).pipe(
      // BẮT BUỘC đặt catchError Ở TRONG switchMap
      // Nếu đặt bên ngoài, tính năng search sẽ bị liệt luôn sau 1 lần lỗi!
      catchError(error => {
        console.error('Lỗi khi tìm kiếm:', error);
        return of(); // Trả về mảng rỗng để app không bị crash
      })
    )
  )
).subscribe();
```

**Kỹ năng phụ (`retry`):**

Nếu API dễ bị chập chờn do mạng, dùng toán tử `retry(3)` trước khi `catchError`. RxJS sẽ tự động gọi lại API tối đa 3 lần nếu thất bại, trước khi thực sự báo lỗi.