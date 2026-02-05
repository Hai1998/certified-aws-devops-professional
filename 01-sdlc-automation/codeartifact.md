# AWS CodeArtifact

- Dùng để lưu trữ và quản lý các thư viện và phần mềm phụ thuộc (software dependencies)
- Việc lưu trữ và lấy các dependency này được gọi là quản lý artifact (artifact management)
- CodeArtifact là dịch vụ quản lý artifact an toàn, có khả năng mở rộng và tiết kiệm chi phí cho phát triển phần mềm
- Hỗ trợ các công cụ quản lý dependency phổ biến như:
  - Maven
  - Gradle
  - npm
  - yarn
  - twine
  - pip
  - NuGet
- Developers và AWS CodeBuild có thể lấy dependency trực tiếp từ CodeArtifact
- CodeArtifact có thể hoạt động như một proxy cho các public repository như:
  - npm
  - Maven Central
  - PyPI
  - NuGet
- CodeArtifact sẽ cache các dependency này

---

## Tích hợp EventBridge

- Các sự kiện của CodeArtifact (ví dụ: khi một phiên bản package được tạo, sửa hoặc xóa) sẽ được gửi (emit) tới Amazon EventBridge
- Các sự kiện này có thể dùng làm input cho:
  - AWS Lambda
  - Amazon SNS
  - Amazon SQS
- Các sự kiện cũng có thể kích hoạt AWS CodePipeline

---

## Resource Policy

- Có thể dùng để cấp quyền cho AWS account khác truy cập CodeArtifact
- Một principal chỉ có thể:
  - Đọc tất cả package trong một repository
  - Hoặc không được đọc package nào
- Không hỗ trợ phân quyền chi tiết theo từng package

---

## Upstream Repositories

- Một CodeArtifact repository có thể có các CodeArtifact repository khác làm upstream repository
- Có upstream cho phép package manager client truy cập package từ nhiều repository thông qua một endpoint duy nhất
- Có thể cấu hình tối đa 10 upstream repositories cho một repository
- Một repository có thể có external connection (chỉ 1 cái)
  - Ví dụ: kết nối tới public npm repository

---

## External Connections

- External connection là kết nối giữa CodeArtifact repository và một public/external repository như:
  - Maven Central
  - npm
  - PyPI
  - NuGet
- Cho phép tải các package chưa có sẵn trong CodeArtifact repository
- Một repository chỉ được phép có tối đa 1 external connection

---

## Retention

- Nếu một phiên bản package được tìm thấy trong upstream repository, một reference đến package đó sẽ được giữ lại (retained) và luôn có sẵn từ downstream repository
- Phiên bản package được retain sẽ không bị ảnh hưởng bởi các thay đổi trong upstream repository
- Các repository trung gian KHÔNG lưu package

Ví dụ:

A -> B -> C -> npm

Nếu B và C không có phiên bản mà A yêu cầu,  
thì package được tải về sẽ chỉ được retain trong A (không lưu ở B và C)

---

## Domains

- Domain của CodeArtifact giúp quản lý nhiều repository trong toàn bộ tổ chức dễ dàng hơn
- Có thể dùng domain để áp dụng quyền (permissions) cho nhiều repository thuộc các AWS account khác nhau
- Một asset chỉ được lưu một lần trong domain, kể cả khi nó có sẵn trong nhiều repository

Domains hữu ích cho:

### Khử trùng lặp lưu trữ (Deduplicate storage)
- Asset chỉ cần lưu một lần trong domain dù được dùng bởi nhiều repository

### Sao chép nhanh (Fast copying)
- Chỉ cập nhật metadata khi pull package từ upstream CodeArtifact repository vào downstream repository

### Chia sẻ dễ dàng giữa các team
- Tất cả asset và metadata trong domain được mã hóa bằng một KMS key duy nhất

### Áp dụng policy trên nhiều repository
Domain administrator có thể áp dụng policy như:
- Hạn chế AWS account nào được truy cập các repository trong domain
- Kiểm soát ai được cấu hình kết nối tới public repository
