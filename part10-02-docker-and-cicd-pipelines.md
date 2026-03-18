# 🏁 Part 10: DEVOPS & AUTOMATION (Docker & CI/CD)

Code xong rồi, làm sao đưa lên mạng? Quên việc kéo thả file qua FTP đi. Ở quy mô doanh nghiệp, mọi thứ phải được tự động hóa 100%.

## 1. Dockerizing Angular (Đóng gói ứng dụng vào Container)

***Docker là gì?***

Docker đóng gói code của bạn cùng với môi trường chạy (NodeJS, Nginx) vào một chiếc "hộp" (Container). Chiếc hộp này quăng lên máy chủ nào (AWS, Google Cloud, DigitalOcean) cũng chạy được chính xác y như trên máy cá nhân của bạn, dẹp bỏ câu nói bào chữa kinh điển: "Ơ, trên máy em vẫn chạy bình thường mà!".

***Multi-stage Dockerfile (Kỹ thuật Build Siêu Tốc):***

Đây là file cấu hình Docker chuẩn mực nhất cho Angular. Nó chia làm 2 giai đoạn:
- Lấy NodeJS để build code Angular ra file HTML/CSS/JS thuần.
- Vứt bỏ NodeJS đi (để Image siêu nhẹ), chỉ lấy Nginx để chạy web.

```docker
# --- Giai đoạn 1: Build (Dùng Node) ---
FROM node:20-alpine AS build
WORKDIR /app

# Cài thư viện và Build code
COPY package*.json./
RUN npm ci
COPY..
RUN npm run build --configuration=production

# --- Giai đoạn 2: Production (Dùng Nginx) ---
FROM nginx:alpine

# Copy file đã build từ Giai đoạn 1 sang thư mục của Nginx
COPY --from=build /app/dist/my-modern-app/browser /usr/share/nginx/html

# Copy file cấu hình Nginx (Bắt buộc để hỗ trợ Angular Routing không bị lỗi 404)
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```


## 2. CI/CD Pipelines (Dây chuyền tự động triển khai)
***CI/CD là gì?***

Continuous Integration / Continuous Deployment (Tích hợp và Triển khai liên tục).
Thay vì tự gõ lệnh test và build, bạn cài một "con robot" trên GitHub Actions hoặc GitLab CI. Mỗi khi bạn gõ lệnh `git push` đẩy code lên, con robot này sẽ tự động: Chạy Unit Test -> Chạy Build -> Nhét vào Docker -> Đẩy thẳng lên Server.

**Ví dụ thực chiến: GitHub Actions** (`.github/workflows/deploy.yml`):

```yaml
name: Angular 2026 CI/CD Pipeline

# Kích hoạt robot khi có người push code lên nhánh 'main'
on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Bước 1: Kéo source code về máy ảo
      - name: Checkout code
        uses: actions/checkout@v4

      # Bước 2: Cài NodeJS
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      # Bước 3: Cài thư viện và chạy Test (Vitest)
      - name: Install & Test
        run: |
          npm ci
          npm run test -- --run

      # Bước 4: Build bản Production
      - name: Build Angular
        run: npm run build --configuration=production

      # Bước 5: Deploy lên Server (Ví dụ tự động đẩy lên Firebase Hosting)
      - name: Deploy to Firebase
        uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: '${{ secrets.GITHUB_TOKEN }}'
          firebaseServiceAccount: '${{ secrets.FIREBASE_SERVICE_ACCOUNT }}'
          channelId: live
          projectId: your-project-id
```