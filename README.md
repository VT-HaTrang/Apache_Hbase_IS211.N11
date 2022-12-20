## Apache_Hbase_IS211.N11
# MÔ TẢ PROJECT
Mục tiêu project này nhằm tìm hiểu cơ chế phân tán dữ liệu trong hệ quản trị CSDL Apache HBase. HBase là một distributed column-oriented database được xây dựng trên hệ thống tệp Hadoop, đây là mã nguồn mở nằm trong dự án của Apache. </br>
HBase là một mô hình dữ liệu tương tự như Google Big Table được thiết kế để cung cấp quyền truy cập ngẫu nhiên nhanh chóng vào lượng lớn dữ liệu có cấu trúc khổng lồ. Nó tận dụng khả năng chịu lỗi do Hệ thống tệp Hadoop (HDFS) cung cấp. HBase được viết bằng ngôn ngữ Java, có thể lưu trữ dữ liệu cực lớn từ terabytes đến petabytes. </br>
HBase thực chất là một NoSQL điển hình nên vì thế các table của HBase không có một schemas cố định nào và cũng không có mối quan hệ giữa các bảng. Hiện nay, có rất nhiều công ty và tập đoàn công nghệ lớn trên thế giới sử dụng HBase, có thể kể đến: Facebook, Twitter, Yahoo, Adobe…

**Các tính năng của HBase** </br>
- Thời gian lọc dữ liệu nhanh. </br>
- Lưu trữ dữ liệu Big-Data, có thể lưu trữ hàng tỷ rows và columns. </br>
- Có độ ổn định và giảm thiểu rủi ro (failover) khi lưu một lượng lớn dữ liệu. </br>
- Truy vấn dữ liệu theo thời gian thực. </br>
- Cung cấp giao thức REST, giúp trả về dữ liệu theo các định dạng khác nhau như plain test, json, xml. Nhờ đó chúng ta có thể khai thác dữ liệu không cần qua API từ phần mềm thứ 3. </br>
- Nhất quán cơ chế đọc và ghi dữ liệu dựa trên Hadoop. </br>
- Nhiều extension hỗ trợ Hbase cho nhiều ngôn ngữ như Java, PHP, Python… </br>
- Lưu trữ dữ liệu đáng tin cậy, được các hãng công nghệ trên thế giới sử dụng trên quy mô lớn.</br>

## HƯỚNG DẪN CÀI ĐẶT
# Yêu cầu
Các phiên bản được sử dụng xuyên suốt trong quá trình cài đặt:
-	VirtualBox
-	Ubuntu server 22.04 
-	Java 8
-	Hadoop 3.3.2
-	Apache Zookeeper 3.6.3
-	Apache HBase
</br>
Tạo 3 máy ảo sử dụng phiên bản Ubuntu server 22.04 trên VirtualBox bao gồm: tnmaster, tnslave1, tnslave2.

# Cài đặt Hadoop
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
-	Thực hiện chỉnh sửa file ~/.bashrc:
```php
sudo nano ~/.bashrc
```
Sau đó thêm vào những lệnh như sau:
-	Thực hiện các lệnh sau:
```php
source ~/.bashrc
git clone https://github.com/nilesh-g/hadoop-cluster-install.git
 ```
-	Copy từ hadoop-cluster-install/master qua máy tnmaster:
```php
cp hadoop-cluster-install/master/* hadoop-3.3.2/etc/hadoop/
```
-	Kiểm tra lại các file file:
```php
cd hadoop-3.3.2/etc/hadoop/
```
```php
sudo nano hadoop-env.sh
```
```php
sudo nano core-site.xml
```

```php
sudo nano hdfs-site.xml
``` 

```php
sudo nano mapred-site.xml
``` 

```php
sudo nano yarn-site.xml
```

```php
sudo nano workers
```


-	Thực hiện clone tnmaster thành 2 máy slave như sau:
 
 
 

Thực hiện ở cả 2 máy slaves những bước sau
-	Đăng nhập vào hadoopuser
 
-	Cấu hình file 01-netcfg.yaml:
 

-	Kiểm tra lại ip đã được chỉnh sửa:
 

-	Chỉnh sửa file /etc/hostname:
 

-	Copy từ hadoop-cluster-install/workder qua:
```php
cp hadoop-cluster-install/worker/* hadoop-3.3.2/etc/hadoop/
``` 

-	Ta thực hiện kiểm tra các file vừa copy:
 
 
 
 
 

	Mở 3 máy và thực hiện các lệnh sau:
-	Thực hiện các lệnh sau (trên tnmaster):
```php
ssh-keygen -t rsa -P “”
```




-	Copy qua các máy (trên tnmaster):
 
 
 
-	Kiểm tra kết nối bằng ssh tới các máy (trên tnmaster):
 
 
 
-	Thực hiện chạy các lệnh sau (trên tnmaster):
```php
cd ~
hdfs namenode -format
```
 

-	Thực hiện khởi chạy hadoop và kiểm tra quá trình hoàn tất (trên tnmaster):
```php
start-dfs.sh
start-yarn.sh
jps
```  





