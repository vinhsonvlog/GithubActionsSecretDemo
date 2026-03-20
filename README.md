# Hướng dẫn sử dụng các "Robot" (Actions) từ Marketplace

## 1. Giới thiệu
Đây là quy trình sử dụng các "Robot" (Actions) từ GitHub Marketplace để tự động hóa các tác vụ CI/CD như:
- Lấy code từ GitHub.
- Đăng nhập vào Docker Hub.
- Đóng gói và đẩy image Docker lên Docker Hub.

## 2. Các bước thực hiện

### Bước 1: Lấy code từ GitHub
Sử dụng Action **Checkout** để lấy code từ repository GitHub.

```yaml
- name: Bước 2 - Tải code
  uses: actions/checkout@v6
```

### Bước 2: Đăng nhập vào Docker Hub
Sử dụng Action **Docker Login** để đăng nhập vào Docker Hub. Bạn cần cung cấp thông tin đăng nhập của mình.

```yaml
- name: Bước 3 - Đăng nhập Docker Hub
  uses: docker/login-action@v1
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

### Bước 3: Đóng gói và đẩy image Docker
Sử dụng Action **Build and push Docker images** để đóng gói và đẩy image Docker lên Docker Hub.

```yaml
- name: Bước 4 - Đóng gói & Đẩy Image
  uses: docker/build-push-action@v2
  with:
    context: .
    push: true
    tags: ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

## 3. To use the access token from your Docker CLI client
1. Chạy lệnh sau để đăng nhập vào Docker Hub:

```bash
docker login -u vinhsonvlog
```

## 4. Mẫu file workflow
Dưới đây là một mẫu file workflow sử dụng các Actions trên:

```yaml
name: Thử nghiệm đọc file README  #Tên quy trình

on:
  push:
    branches: ["main"]  #Chạy khi có code mới đẩy lên nhánh main

jobs:
  thuc_hanh_doc_file:
    runs-on: ubuntu-latest  #Mượn máy ảo Ubuntu của GitHub để chạy

    steps:
    - name: Bước 1 - Xin chào
      run: echo "Hello HUTECH!"  #Lệnh in ra màn hình

    - name: Bước 2 - Tải code
      uses: actions/checkout@v6  #Robot tự động copy code vào máy ảo

    - name: Bước 3 - Đọc file
      run: cat README.md  #Mở nội dung file README.md để xem

    - name: Bước 4 - Đăng nhập Docker Hub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Bước 5 - Đóng gói & Đẩy Image
      uses: docker/build-push-action@v2
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

## 5. Kết luận
Sử dụng các "Robot" từ GitHub Marketplace giúp tự động hóa quy trình CI/CD, tiết kiệm thời gian và giảm thiểu lỗi. Hãy thử áp dụng các bước trên để triển khai ứng dụng của bạn một cách nhanh chóng và hiệu quả.