# 1. Giới thiệu
  **Helm** là phần mềm quản lý gói(package manager) cho kubernetes 
  - **Helm** sử dụng 1 dạng đóng gói đặt tên là chart , chart là tập hợp các file liên quan đến tài nguyên của Kubernetes

  **Chart File Structure**
```
wordpress/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  values.yaml         # The default configuration values for this chart
  values.schema.json  # OPTIONAL: A JSON Schema for imposing a structure on the values.yaml file
  charts/             # A directory containing any charts upon which this chart depends.
  crds/               # Custom Resource Definitions
  templates/          # A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
```
 **Chart.yaml file**
 ```
apiVersion: The chart API version (required)
name: The name of the chart (required)
version: A SemVer 2 version (required)
kubeVersion: A SemVer range of compatible Kubernetes versions (optional)
description: A single-sentence description of this project (optional)
type: The type of the chart (optional)
keywords:
  - A list of keywords about this project (optional)
home: The URL of this projects home page (optional)
sources:
  - A list of URLs to source code for this project (optional)
dependencies: # A list of the chart requirements (optional)
  - name: The name of the chart (nginx)
    version: The version of the chart ("1.2.3")
    repository: (optional) The repository URL ("https://example.com/charts") or alias ("@repo-name")
    condition: (optional) A yaml path that resolves to a boolean, used for enabling/disabling charts (e.g. subchart1.enabled )
    tags: # (optional)
      - Tags can be used to group charts for enabling/disabling together
    import-values: # (optional)
      - ImportValues holds the mapping of source values to parent key to be imported. Each item can be a string or pair of child/parent sublist items.
    alias: (optional) Alias to be used for the chart. Useful when you have to add the same chart multiple times
maintainers: # (optional)
  - name: The maintainers name (required for each maintainer)
    email: The maintainers email (optional for each maintainer)
    url: A URL for the maintainer (optional for each maintainer)
icon: A URL to an SVG or PNG image to be used as an icon (optional).
appVersion: The version of the app that this contains (optional). Needn't be SemVer. Quotes recommended.
deprecated: Whether this chart is deprecated (optional, boolean)
annotations:
  example: A list of annotations keyed by name (optional).
```
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
  helm install $name_release $chart_name/$chartpkg --version $number -n $namespace
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
 - Nâng cấp version lên version mới
 ```
  helm upgrade $name_release $chart_name/$chartpkg --version $newversionnumber -n $namespace
 ```
 - Xem lịch sử cập nhật của release
 ```
  helm history $name_release
 ```
 - Quay trở lại version cũ
 ```
   helm rollback $name $number_revision
  ```
  - Lấy ra tất cả thông tin của chart
  ```
   helm get all $name_release 
  ```
  - Lấy ra thông tin manifest
  ```
   helm get mainifest $name_release
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
   - kiểm tra xem chart có vấn đề gì không
   ```
    helm lint
   ```
   - gen ra manifest kiểm tra chart đã viết 
   ```
     helm template --release-name $name $directory -f values.yaml -n $namespace 
   ```
   - Thêm dependencies
   Ví dụ:
   ```
    dependencies:
      - name: redis-ha
        version: 4.23.0
        repository: https://dandydeveloper.github.io/charts/
        condition: redis-ha.enabled
   ```
   - list thông tin về dependency
   ```
    helm dependency list
   ```
   - Build dependency 
   ```
    helm dependency build
   ```
   - Update dependency
   ```
    helm dependency update
   ```
