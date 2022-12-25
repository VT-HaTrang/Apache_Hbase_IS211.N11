# 1. Cài đặt Hadoop
Trước khi chúng ta bắt đầu định cấu hình HBase, chúng ta cần có một cụm Hadoop đang chạy, đây sẽ là bộ lưu trữ cho HBase (dữ liệu lưu trữ Hbase trong Hệ thống tệp phân tán Hadoop). </br>
Các bước cài Hadoop trên 3 máy: </br>
Đầu tiên, thực hiện các bước sau trên máy tnmaster:
-	Tạo user hadoopuser và thực hiện đăng nhập:
```php
sudo adduser hadoopuser
sudo usermod -aG sudo hadoopuser
```
- Thực hiện đăng nhập vào hadoopuser
-	Thực hiện update và cài đặt jdk:
```php
sudo apt update
sudo apt install openjdk-8-jdk
```
-	Thực hiện download hadoop:
```php
wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.2/hadoop-3.3.2.tar.gz
tar -zxvf hadoop-3.3.2.tar.gz
```
- Thay đổi Adapter 2:
![image](https://user-images.githubusercontent.com/88712945/208700218-42e9a866-dd32-4422-bd49-7bc93067f17b.png)

-	Thực hiện chỉnh sửa file 01-netcfg.yaml và cấu hình mạng như dưới đây:
```php 
sudo vim /etc/netplan/01-netcfg.yaml
```
![image](https://user-images.githubusercontent.com/88712945/208697818-429dac9a-4bc3-497d-988b-55825fb37a2f.png) </br>

-	Thực hiện apply netplan và kiểm tra ip:
```php
sudo netplan apply
sudo ip a
```
-	Thực hiện chỉnh sửa file host và thay đổi hostname: 
```php
sudo nano /etc/hosts
```
![image](https://user-images.githubusercontent.com/88712945/208700794-7361d8fa-29f6-4386-b588-2bb3daac5076.png)

![image](https://user-images.githubusercontent.com/88712945/208700589-a7ea5d3c-7b92-45a3-ba4e-c4191c1db48e.png)

-	Thực hiện chỉnh sửa file ~/.bashrc:
```php
sudo nano ~/.bashrc
```
![image](https://user-images.githubusercontent.com/88712945/208700888-3fd09a5e-bca2-4f2e-ac48-595a22997b9a.png)
Sau đó thêm vào những lệnh như sau:
-	Thực hiện các lệnh sau:
```php
source ~/.bashrc
git clone https://github.com/nilesh-g/hadoop-cluster-install.git
 ```
 ![image](https://user-images.githubusercontent.com/88712945/208700930-b288a437-ef33-4601-b321-a74274b904b4.png)

-	Copy từ hadoop-cluster-install/master qua máy tnmaster:
```php
cp hadoop-cluster-install/master/* hadoop-3.3.2/etc/hadoop/
```
![image](https://user-images.githubusercontent.com/88712945/208701043-d49be5cd-ab3c-4376-8e18-a28bf89ae8ba.png)

-	Kiểm tra lại các file file:
```php
cd hadoop-3.3.2/etc/hadoop/
```
![image](https://user-images.githubusercontent.com/88712945/208701083-7a4c040f-6e0b-480a-a6e1-114458cc7ebc.png)

```php
sudo nano hadoop-env.sh
```
![image](https://user-images.githubusercontent.com/88712945/208701187-f32767ca-30b0-4d5d-abd6-6b3e2cb3e865.png)

```php
sudo nano core-site.xml
```
![image](https://user-images.githubusercontent.com/88712945/208701240-3824ddbc-53fc-456b-96a9-e39cfbb08b7e.png)

```php
sudo nano hdfs-site.xml
``` 
![image](https://user-images.githubusercontent.com/88712945/208701307-958f06c6-76f0-42ed-955a-4b1bb38d8239.png)

```php
sudo nano mapred-site.xml
``` 
![image](https://user-images.githubusercontent.com/88712945/208701371-b5282b04-db03-4aef-9b78-f5490d1a8320.png)

```php
sudo nano yarn-site.xml
```
![image](https://user-images.githubusercontent.com/88712945/208701391-d0bdb1b8-3d22-49d8-8545-ded5ec8c3be7.png)

```php
sudo nano workers
```
![image](https://user-images.githubusercontent.com/88712945/208701437-bfdbeafa-8e1d-49ee-870b-6571e7f576a0.png)
-	Thực hiện clone tnmaster thành 2 máy slave như sau:
Thực hiện ở cả 2 máy slaves những bước sau
-	Đăng nhập vào hadoopuser
-	Cấu hình file 01-netcfg.yaml:
![image](https://user-images.githubusercontent.com/88712945/208701727-e932a7fb-fe0e-44dc-ab4e-7f5d1f798e1d.png)
-	Kiểm tra lại ip đã được chỉnh sửa:
 ![image](https://user-images.githubusercontent.com/88712945/208701801-34274d6f-aa6e-42e7-8690-1b79d45745e6.png)
-	Chỉnh sửa file /etc/hostname:
 ![image](https://user-images.githubusercontent.com/88712945/208701894-4c4b0336-0069-4b7b-aa1d-fd1603a7da07.png)
-	Copy từ hadoop-cluster-install/workder qua:
```php
cp hadoop-cluster-install/worker/* hadoop-3.3.2/etc/hadoop/
``` 
-	Ta thực hiện kiểm tra các file vừa copy:
 ![image](https://user-images.githubusercontent.com/88712945/208702088-3dabbd50-c94c-48d1-8722-4a42c2ba0299.png)
	Mở 3 máy và thực hiện các lệnh sau:
-	Thực hiện các lệnh sau (trên tnmaster):
```php
ssh-keygen -t rsa -P “”
```
-	Copy qua các máy (trên tnmaster):
![image](https://user-images.githubusercontent.com/88712945/208702239-382cef5c-8c0f-4c5f-aac4-f0f081ba9b65.png)
![image](https://user-images.githubusercontent.com/88712945/208702267-ff2eea2d-8358-498b-931e-0279cba085f5.png)
![image](https://user-images.githubusercontent.com/88712945/208702288-793bc60f-f3c4-4d8f-b9e9-137fa3af21d9.png)
-	Kiểm tra kết nối bằng ssh tới các máy (trên tnmaster):
 ![image](https://user-images.githubusercontent.com/88712945/208702638-a53ea19e-a5eb-4b35-9018-8077e003cf21.png)
 ![image](https://user-images.githubusercontent.com/88712945/208702541-42efcc3c-3b9b-4cf2-92ba-1da11ab1bf76.png)
 ![image](https://user-images.githubusercontent.com/88712945/208702488-e3b2960f-c1dc-4b8a-8d7c-fe6461a6a01f.png)
-	Thực hiện chạy các lệnh sau (trên tnmaster):
```php
cd ~
hdfs namenode -format
```
![image](https://user-images.githubusercontent.com/88712945/208702748-440672c6-525d-4324-90b5-551787a08cc0.png)
![image](https://user-images.githubusercontent.com/88712945/208702788-24845b41-8c1b-41d4-91c5-b181e588935a.png)

-	Thực hiện khởi chạy hadoop và kiểm tra quá trình hoàn tất (trên tnmaster):
```php
start-dfs.sh
start-yarn.sh
jps
``` 
![image](https://user-images.githubusercontent.com/88712945/208703262-68762fcd-1510-46ee-9456-7b5f83c20e95.png)





