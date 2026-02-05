# AWS Amplify là gì?

**AWS Amplify** là một bộ công cụ và dịch vụ giúp xây dựng, triển khai và host **ứng dụng web và mobile full-stack** một cách nhanh chóng.

Amplify đặc biệt phù hợp cho các ứng dụng:
- Frontend + backend serverless
- Phát triển nhanh (rapid development)

---

# Amplify cung cấp những gì?

Amplify cho bạn **một nền tảng tập trung** để cấu hình và quản lý:

## 1. Hosting Frontend
- Website tĩnh: React, Vue, Angular, Next.js, v.v.
- Framework hỗ trợ SSR (Server-Side Rendering)
- CDN toàn cầu (CloudFront ở phía sau)

---

## 2. Authentication (Xác thực)
- Dựa trên **Amazon Cognito**
- Đăng ký / đăng nhập người dùng
- Social login (Google, Facebook, Apple…)
- MFA, chính sách mật khẩu

---

## 3. API
- **GraphQL** với AWS AppSync
- **REST API** với API Gateway + Lambda
- Dễ tích hợp từ frontend bằng SDK của Amplify

---

## 4. Storage
- Lưu file với **Amazon S3**
- File public / private theo user
- Phân quyền bằng Cognito

---

## 5. Pub/Sub & Real-time
- Real-time data với GraphQL Subscriptions
- Xây dựng trên AppSync

---

## 6. Analytics & Monitoring
- Theo dõi hành vi người dùng
- App monitoring
- Tích hợp với CloudWatch, Pinpoint

---

## 7. AI/ML Predictions (ít dùng trong kiến trúc mới)
- Tích hợp nhanh với:
  - Rekognition
  - Translate
  - Polly
  - Comprehend  
- Phần này hiện nay ít được nhấn mạnh.

---

# CI/CD với AWS Amplify

Amplify có **CI/CD tích hợp sẵn** cho frontend:

## Kết nối source code
Amplify có thể kết nối với:
- GitHub
- GitLab
- Bitbucket
- AWS CodeCommit

---

## Mỗi branch = một môi trường deploy

Amplify hỗ trợ deploy theo branch:

- main → production
- dev → development
- feature/* → preview environment

Mỗi branch sẽ tự động:
- Build
- Test
- Deploy
- Có URL riêng

Rất phù hợp cho:
- Review tính năng
- QA testing
- Multi-environment

---

# Custom Domain (Route 53)

Bạn có thể gắn **custom domain** cho Amplify:

- Ví dụ: www.example.com
- Tích hợp rất tốt với:
  - Amazon Route 53
  - Nhà cung cấp domain khác

Amplify sẽ tự động:
- Cấp SSL (ACM)
- Mapping subdomain
- Redirect (www ↔ root domain)

---

# Kiến trúc điển hình với Amplify

Frontend:
  Amplify Hosting (S3 + CloudFront)

Backend:
  Cognito (Auth)
  AppSync (GraphQL)
  API Gateway + Lambda (REST)
  DynamoDB
  S3

---

# Khi nào nên dùng Amplify?

## Nên dùng khi:
- Muốn phát triển nhanh frontend + serverless backend
- Muốn auth, API, storage sẵn sàng
- Muốn CI/CD đơn giản cho frontend
- Không muốn quản lý hạ tầng phức tạp

## Không phù hợp khi:
- Cần kiểm soát sâu VPC, EC2, ECS, EKS
- Backend phức tạp, yêu cầu kiến trúc custom
- Pipeline CI/CD rất đặc thù
