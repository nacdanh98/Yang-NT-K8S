# 1. Giới thiệu
  **Helm** là phần mềm quản lý gói(package manager) cho kubernetes 
# 2 Cài đặt
## Yêu cầu :
  - 1 Kubernetes cluster
  - Chọn cấu hình security áp dụng
## Cài từ source binary
- Download file from here : https://github.com/helm/helm/releases
- Gõ lệnh và di chuyển file về thư mục
``` 
  tar  -xvzf helm-v3.0.0-linux-amd64.tar.gz
  mv linux-amd64/helm /usr/local/bin/helm
```
## Cài tự động từ Script
```
  $ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
  $ chmod 700 get_helm.sh
  $ ./get_helm.sh
```
## Cài từ package manager cho Ubuntu
```
  curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
  sudo apt-get install apt-transport-https --yes
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
  sudo apt-get update
  sudo apt-get install helm
```
# Cách sử dụng helm chart có sẵn
Trang tìm kiếm repo public https://artifacthub.io/packages/search?kind=0
- List thông tin repo 
```
  helm repo list
```
- Add thêm repo mới
```
  helm repo add $name $link_helm_chart
```
- Xóa repo
```
  helm repo remove $name
 ```
 - Cập nhật thông tin mới cho repo
 ```
  helm repo update
 ```
 - Xem dịch vụ bên trong repo
 ```
  heml search repo $name
 ```
 - Cài đặt từ package bên trong chart
 ```
  helm install $name $chart_name/$chartpkg --version $number -n $namespace
 ```
 - List thông tin
 ```
  helm ls 
 ```
 - Gỡ cài đặt 
 ```
  helm uninstall $name_release
 ```
 - Check trạng thái bản release
 ```
  helm status $name_release
 ```
 - Quay trở lại version cũ
 ```
  helm rollback v
  ```
  # Cách viết helm chart
  - Tạo chart mới
  ```
    helm create chart
  ```
  - Đóng gói chart
  ```
    helm package . -d $directory
   ```
   - Tạo file index.yaml
   ```
    helm repo index .
   ```