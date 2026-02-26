## 📚 Lộ Trình Chinh Phục Angular (Modern Angular 18+)

Chào mừng bạn đến với Cẩm nang Lý thuyết & Hướng dẫn toàn diện về Angular. Dự án này không chứa code ứng dụng cụ thể mà đóng vai trò như một bản đồ tư duy (roadmap) và tài liệu tham khảo giúp bạn nắm vững hệ sinh thái Angular hiện đại (tập trung vào Angular 16-18+ với Standalone Components, Signals, và Control Flow mới).

---

### 📑 Mục lục

GIAI ĐOẠN 1: NỀN TẢNG BẮT BUỘC

GIAI ĐOẠN 2: NHẬP MÔN & CORE ARCHITECTURE

GIAI ĐOẠN 3: LOGIC, SERVICES & SIGNALS

GIAI ĐOẠN 4: ROUTING & FORMS

GIAI ĐOẠN 5: LUỒNG DỮ LIỆU BẤT ĐỒNG BỘ (RXJS)

GIAI ĐOẠN 6: HTTP, SECURITY & I18N

GIAI ĐOẠN 7: HIỆU NĂNG NÂNG CAO (PERFORMANCE)

GIAI ĐOẠN 8: KIẾN TRÚC MỞ RỘNG (ARCHITECTURE)

GIAI ĐOẠN 9: CHẤT LƯỢNG & TESTING

GIAI ĐOẠN 10: DEVOPS & AUTOMATION

---

#### 🏁 GIAI ĐOẠN 1: NỀN TẢNG BẮT BUỘC (PREREQUISITES)

Trước khi bước vào thế giới Angular, đây là những hành trang không thể thiếu:

##### TypeScript Modern

Strong Typing (Interfaces, Types, Enums).

Generics & Utility Types.

Decorators & Metadata.

Asynchronous Programming (Promises, Async/Await).

##### Web Essentials

HTML5 Semantic.

CSS Grid/Flexbox, SCSS Basics.

---

##### 🏗️ GIAI ĐOẠN 2: NHẬP MÔN & CORE ARCHITECTURE

Kiến trúc cốt lõi và cách Angular vận hành dưới góc nhìn hiện đại:

##### Project Setup

- Angular CLI, cấu hình angular.json.

##### Standalone Components

- Pattern thay thế cho NgModule (Tiêu chuẩn của Modern Angular).

##### Template Syntax & Control Flow

- New Syntax: `@if`, `@for`, `@switch`, `@empty`.

- Data Binding: Property binding, Event binding, Two-way binding.

##### Component Communication

- `@Input` & `@Output` (Kết hợp với `input()` & `output()` API mới).

- Template Variables (`#var`).

- Content Projection (`<ng-content>`).

##### Lifecycle Hooks

- Hiểu rõ vòng đời `ngOnInit`, `ngAfterViewInit`, `ngOnDestroy`.

---

##### ⚙️ GIAI ĐOẠN 3: LOGIC, SERVICES & SIGNALS

Quản lý trạng thái và chia sẻ logic trong ứng dụng:

##### Dependency Injection (DI)

- `providedIn: 'root'` vs Component-level providers.

- Sử dụng hàm `inject()` thay cho Constructor Injection truyền thống.

##### Angular Signals (State Management mới)

- Làm quen với `WritableSignal`, `computed()`, `effect()`.

- Chiến lược chuyển đổi qua lại giữa Signal và Observable.

##### Services

- Kỹ thuật Logic sharing, Singleton pattern.

---

##### 🛣️ GIAI ĐOẠN 4: ROUTING & FORMS

Điều hướng người dùng và xử lý dữ liệu đầu vào:

##### Angular Router

- Child Routes, Router Outlet.

- Lazy Loading: Tối ưu hóa bundle size.

- Route Guards: `CanActivate`, `CanDeactivate`, `Resolve`.

###### Angular Forms

- Reactive Forms: Chuyên sâu về `FormGroups`, `FormArrays`, Custom Validators.

- Template-driven Forms: Kiến thức cơ bản và khi nào nên dùng.

---

##### 🌊 GIAI ĐOẠN 5: LUỒNG DỮ LIỆU BẤT ĐỒNG BỘ (RXJS)

Trái tim của việc xử lý dữ liệu phức tạp trong Angular:

##### Observables & Observers

- Hiểu bản chất về Stream dữ liệu.

##### Pipeable Operators

- Transformation: `map`, `scan`.

- Filtering: `filter`, `take`, `distinctUntilChanged`.

- Flattening (Rất Quan Trọng): `switchMap`, `mergeMap`, `concatMap`, `exhaustMap`.

##### Error Handling

- Kỹ thuật bắt lỗi và thử lại với `catchError`, `retry`.

---

##### 🔐 GIAI ĐOẠN 6: HTTP, SECURITY & I18N

Giao tiếp với Server, bảo mật và đa ngôn ngữ:

##### HttpClient

- REST API Integration.

##### HTTP Interceptors

- Gắn Token tự động, Handling lỗi 401/403, Global Loading spinner.

##### Security

- Authentication (JWT/OIDC).

- Role-based Access Control (RBAC).

- XSS Prevention & sử dụng `DomSanitizer`.

##### Internationalization (i18n)

- Cấu hình đa ngôn ngữ với `ngx-translate` hoặc Angular i18n native.



##### ⚡ GIAI ĐOẠN 7: HIỆU NĂNG NÂNG CAO (PERFORMANCE)

Tối ưu hóa để ứng dụng chạy mượt mà trên mọi thiết bị:

##### Change Detection

- Hiểu sự khác biệt giữa `Default` và `OnPush` strategy.

##### Zoneless Angular

- Cấu hình ứng dụng không cần `Zone.js` (Tính năng đột phá từ Angular 18+).

##### Deferrable Views

- Sử dụng block `@defer` với các trigger thông minh (on idle, on viewport, on hover).

##### Web Workers

- Kỹ thuật offload và xử lý tác vụ nặng ở background thread.

---

##### 🏛️ GIAI ĐOẠN 8: KIẾN TRÚC MỞ RỘNG (ARCHITECTURE)

Xây dựng nền móng vững chắc cho các dự án Enterprise (Quy mô lớn):

##### State Management

- NgRx Store (Redux pattern) cho dự án cực lớn và phức tạp.

- NgRx Signal Store hoặc Elf cho các dự án kiến trúc hiện đại, gọn nhẹ.

##### Server-Side Rendering (SSR)

- Áp dụng Angular Universal, kỹ thuật Hydration.

##### Micro Frontends

- Nx Monorepo: Quản lý nhiều App và Library tập trung.

- Module Federation: Chạy độc lập và ghép nối các app ở runtime.

---

##### 🧪 GIAI ĐOẠN 9: CHẤT LƯỢNG & TESTING

Đảm bảo code chạy đúng và không bị lỗi (regression):

##### Unit Testing

- Sử dụng Jasmine & Karma hoặc Jest (Testing Components, Services, Pipes).

##### Integration Testing

- Kỹ thuật giả lập (Mocking) HTTP & Dependencies.

##### End-to-End (E2E)

- Viết kịch bản test UI/UX tự động bằng Cypress hoặc Playwright.

##### Accessibility (a11y)

- Tuân thủ tiêu chuẩn web với ARIA roles, Keyboard navigation.

---

##### 🚀 GIAI ĐOẠN 10: DEVOPS & AUTOMATION

Đưa ứng dụng từ môi trường dev lên production:

##### Custom Schematics

- Tự động hóa việc generate code theo chuẩn riêng của team.

##### Build Optimization

-Phân tích với Bundle Analyzer, cấu hình Budget warnings/errors.

##### CI/CD

- Kỹ thuật Dockerize Angular, thiết lập GitHub Actions / GitLab CI pipeline để deploy tự động.

---

##### 💡 Cách sử dụng Repository này

Vì đây là một dự án hướng dẫn lý thuyết, bạn hãy điều hướng qua các thư mục (hoặc các file Markdown tương ứng) đại diện cho từng giai đoạn ở Mục lục bên trên. Mỗi phần sẽ bao gồm:

- Giải thích khái niệm chuyên sâu.

- Các Best Practices (Thực hành tốt nhất) được khuyên dùng.

- Các đoạn code snippet ví dụ minh họa trực quan.

Chúc bạn học tập tốt và chinh phục thành công Angular! 🚀