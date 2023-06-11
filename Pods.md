### Giới thiệu
-  Pods là 1 nhóm các container được đặt cùng vị trí với nhau, các container trong 1 pod chia sẻ tài nguyên và mạng cục bộ của pod.
- Một pod càng có container nhất càng tốt( 1 Pod chỉ nên chạy 1 container)
- Một pod chỉ chạy được trên 1 node và ko có chuyện 1 pod có thể chạy trên 2 node

### Tại sao cần có pods
- Các container được thiết kế để chi chạy 1process trên mỗi container
- Để có thể nhóm các container vào cùng 1 đơn vị để quản lý,các container có thể giao tiếp với nhau do dùng chung kết nối mạng trong 1 pod,không bị phân tán ra trên các node khác nhau cùng cluster

### Cách tạo pods với YAML file

- Metadata bao gồm name, namespace, labels và các thông tin khác của pod.
- Spec chứa các mô tả thực tế về nội dung của  pod như  pod’s con-tainers, volumes và các dữ liệu khác
- Status chứa thông tin về pod đang chạy, điều kiện , trạng thái của container, Pod's internal IP và các thông tin cơ bản khác

```
apiVersion: v1
kind: Loại 
metadata: 
	name: nginx
	labels:
		app: nginxapp
spec:
	containers:
	- name: n1
	  image: nginx:1.17.6
	  resources:
	  	limits:
			memory: "128Mi"
			cpu: "100m"
	 ports:
	 	- containerPort: 80
```


### Chỉ định labels khi tạo pods
```
apiVersion: v1
kind: Pod
metadata:
  name: kubia-manual-v270
  Pods: running containers in Kubernetes
  labels:
    creation_method: manual
    env: prod
spec:
  containers:
  - image: luksa/kubia
    name: kubia
    ports:
      - containerPort: 8080
        protocol: TCP
```
### Chỉ định node được dùng khi tạo pod vs lables
![image](https://github.com/nacdanh98/Yang-NT-K8S/assets/49748262/f5f41468-1db8-4a9d-9c72-a2b73ca6f3d2)

### Lệnh kubectl thao tác với pods
1. Tạo pod từ file yaml
```
  $ kubectl create -f name_pod.yaml
```
2. Lấy thông tin pod trình bày dưới dạng file yaml và json
```
  $ kubectl get po name_pod -o yaml
  $ kubectl get po name_pod -o json
```
3. Liệt kê pod 
```
  $ kubectl get pods
 ```
4. Đọc log của pod
```
  $ kubectl logs name_pod
```
5. Đọc log của container trong pod
```
  $ kubectl logs name_pod -c name_container
```
6. FORWARDING A LOCAL NETWORK PORT TO A PORT IN THE POD
```
  $ kubectl port-forward name_pod 8888:8080
```
7. Hiển thị labels trên các pods
```
  $ kubectl get po --show-labels
```
8. Liệt kê pod đang sử dụng label
```
  $ kubectl get po -l creation_method=manual
```
9. Tạo resources với namespace cụ thể
```
  $ kubectl create -f file.yaml -n name_of_namespace
```
10. Xóa pod với tên_pod
```
  $ kubectl delete po kubia-gpu
```
11. Xóa pod với labels
```
  $ kubectl delete po -l creation_method=manual
```
12. Xóa namespace
```
  $ kubectl delete ns custom-namespace
```
13. Xóa tất cả resouce trong Namespace
```
  $ kubectl delete all --all
```
  
 
