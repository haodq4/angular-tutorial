# 🏁 Part 9: CHẤT LƯỢNG & TESTING (Unit Test)

Nếu Unit Test kiểm tra từng "bánh răng", thì E2E (End-to-End) Test sẽ lái thử chiếc xe. Sau khi xe chạy ngon, ta phải đảm bảo mọi người (kể cả người khiếm thị) đều lái được nó (Accessibility).

## 1. End-to-End (E2E) Testing với Playwright / Cypress

***Tạm biệt Protractor:***

Công cụ test E2E cũ của Angular đã chết. Hiện tại có 2 thế lực thống trị là **Cypress** và **Playwright**.
Cộng đồng Angular 2026 đang rất chuộng **Playwright** (do Microsoft phát triển) vì nó nhanh, test được nhiều tab cùng lúc, và hỗ trợ đa trình duyệt cực tốt.

***Mô phỏng Playwright E2E:***
Thay vì viết code trong ứng dụng, E2E mở một trình duyệt thật bằng code, bấm click, điền form y hệt người dùng.

## 2. Web Accessibility (A11y) & Angular Aria

***Nỗi khổ của việc làm A11y thủ công:***

Để làm một cái Menu xỏ xuống (Dropdown/Combobox) đúng chuẩn: bạn phải xử lý phím Mũi Tên Lên/Xuống để chọn item, phím Enter để xác nhận, phím ESC để đóng, và gán hàng tá thẻ `aria-expanded`, `aria-activedescendant`... Cực kỳ tốn thời gian!

***Phao cứu sinh: Thư viện `@angular/aria` (Mới có từ v21):***

Đây là bộ thư viện "Headless UI" (Chỉ có logic chuẩn, không có CSS) của chính chủ Angular. Nó làm giùm bạn 100% logic phức tạp của A11y. Bạn chỉ việc gắn các directive (`ngCombobox`, `ngToolbar`,...) vào HTML của bạn, rồi tự viết CSS theo ý thích.

***Ví dụ: Gắn Toolbar siêu chuẩn bằng Angular Aria:***

Chỉ với vài từ khóa `ngToolbarWidget`, Angular đã gánh toàn bộ logic Accessibility phức tạp.