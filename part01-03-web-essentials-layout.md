# 🏁 Part 1: NỀN TẢNG BẮT BUỘC (PREREQUISITES)

## Nền Tảng 3: Chuẩn Mực Web Mới (Semantic HTML & Layout)

Dù bạn dùng framework mạnh đến đâu, kết quả cuối cùng in ra trình duyệt vẫn là HTML và CSS. Viết HTML/CSS đúng chuẩn là ranh giới giữa một lập trình viên "biết code" và một "kỹ sư Web chuyên nghiệp".

### 1. Semantic HTML & Khả năng tiếp cận (Accessibility - A11y)

***Căn bệnh lạm dụng thẻ*** `<div>`:
Rất nhiều người mới học có thói quen bọc mọi thứ bằng thẻ `<div>`. Trình duyệt vẫn hiển thị đẹp nhờ CSS, nhưng nó khiến code trở thành một "đống rác vô nghĩa" đối với máy móc.

***Semantic HTML (HTML Ngữ Nghĩa) là gì?***

Là việc sử dụng các thẻ nói lên chính xác nội dung chứa bên trong nó: `<header>`, `<main>`, `<nav>`, `<article>`, `<section>`, `<footer>`.

***Tại sao điều này lại mang tính sống còn?***

- ***SEO & AI Agents***: Bot của Google và các công cụ Trí tuệ nhân tạo (AI) dựa vào các thẻ này để đọc hiểu cấu trúc trang web của bạn.

- ***Khả năng tiếp cận (Accessibility - A11y)***: Các phần mềm đọc màn hình (Screen Readers) cho người khiếm thị dựa hoàn toàn vào Semantic HTML để phát âm nội dung. Các đạo luật như EAA (Châu Âu) hay ADA (Mỹ) đang buộc các website phải tuân thủ chuẩn tiếp cận này, nếu không sẽ bị phạt.

```html
<div class="header">
  <div class="menu">
    <div class="item">Trang chủ</div>
  </div>
</div>

<header>
  <nav>
    <a href="/">Trang chủ</a>
  </nav>
</header>
```

### 2. Bố cục dàn trang: CSS Grid vs Flexbox

Đây là 2 hệ thống dàn trang (layout) hiện đại nhất. Lỗi sai phổ biến nhất là cố gắng dùng Flexbox để giải quyết mọi bài toán. Hãy hiểu sự khác biệt về ***Số chiều không gian*** của chúng:

**Flexbox (Bố cục 1 Chiều - 1D)**

- **Cách hoạt động**: Chỉ xử lý layout theo **một trục tại một thời điểm** (hoặc là dóng theo hàng ngang, hoặc là dóng theo cột dọc).

- **Triết lý**: "Content-first" - Kích thước của phần tử thường được định đoạt bởi nội dung bên trong nó.

- **Dùng khi nào?** Căn giữa một phần tử, dàn đều các nút bấm, làm thanh điều hướng (Navbar), hoặc sắp xếp các component nhỏ.

**CSS Grid (Bố cục 2 Chiều - 2D)**

- **Cách hoạt động**: Kiểm soát đồng thời cả Hàng (Rows) VÀ Cột (Columns) cùng một lúc.

- **Triết lý**: "Layout-first" - Bạn vẽ sẵn một hệ thống lưới vững chắc trước, sau đó mới chỉ định các phần tử giao diện nằm ở tọa độ nào trên lưới đó.

- **Dùng khi nào?** Xây dựng bộ khung lớn của toàn bộ trang web (Ví dụ: Header ở trên cùng, Sidebar bên trái, Nội dung chính ở giữa, Footer bên dưới).

**Tóm tắt thực chiến**: Hãy dùng **CSS Grid** để xây dựng khung móng nhà và chia các căn phòng, sau đó đi vào từng căn phòng và dùng **Flexbox** để sắp xếp đồ đạc bên trong cho gọn gàng.