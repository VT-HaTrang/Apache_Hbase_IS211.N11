# 1. Cài đặt Zookeeper
HBase phân tán phụ thuộc vào cụm ZooKeeper đang chạy. Tất cả các nút và ứng dụng khách tham gia cần có khả năng truy cập cụm ZooKeeper đang chạy, do đó ta cần cài đặt zookeeper. </br>
- Thực hiện tải zookeeper và apache-storm về máy về máy (trên tnmaster): </br>
```text
cd ~
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.6.3/apache-zookeeper-3.6.3-bin.tar.gz 
wget https://dlcdn.apache.org/storm/apache-storm-2.2.1/apache-storm-2.2.1.tar.gz
```
- Thực hiện copy file zookeeper qua các máy slave (trên tnmaster):
```text
scp apache-zookeeper-3.6.3-bin.tar.gz hadoopuser@192.168.56.51:/home/hadoopuser
scp apache-zookeeper-3.6.3-bin.tar.gz hadoopuser@192.168.56.52:/home/hadoopuser
```
- Thực hiện copy file storm qua các máy slave (trên tnmaster):
```text
scp apache-storm-2.2.1.tar.gz hadoopuser@192.168.56.51:/home/hadoopuser
scp apache-storm-2.2.1.tar.gz hadoopuser@192.168.56.51:/home/hadoopuser
```
-	Giải nén file cài và move vào folder /usr/local/zookeeper (trên cả 3 máy):
```text
tar -zxvf apache-zookeeper-3.6.3-bin.tar.gz
sudo mv apache-zookeeper-3.6.3-bin /usr/local/zookeeper
```
-	Giải nén file cài storm và move vào folder /usr/local/storm (trên 3 máy):
```text
tar -zxvf apache-storm-2.2.1.tar.gz
sudo mv apache-storm-2.2.1 /usr/local/storm
```
-	Add những dòng sau vào file ~/.bashrc (trên cả 3 máy):
```python
export ZOOKEEPER_HOME=/usr/local/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
export STORM_HOME=/usr/local/storm
export PATH=$PATH:$STORM_HOME/bin
```
Sau đó thực hiện lệnh: ```~/.bashrc```
- Di chuyển vào folder Zookeeper và tạo folder data và logs (trên cả 3 máy):
```text
cd $ZOOKEEPER_HOME/
mkdir data
mkdir logs
```
-	Di chuyển vào folde conf và thực hiện copy file zoo_sample.cfg (trên 3 máy):
```text
cd conf/
cp zoo_sample.cfg zoo.cfg
```
-	Thực hiện chỉnh sửa file zoo.cfg (trên cả 3 máy):
```python
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/logs
server.1=tnmaster:2888:3888
server.2=tnslave1:2888:3888
server.3=tnslave2:2888:3888
```
-	Thực hiện chỉnh sửa (trên cả 3 máy):
```python sudo nano /usr/local/zookeeper/data/myid```
![image](https://user-images.githubusercontent.com/88712945/209456104-b666371a-cdb6-464f-bf53-c535437f2b64.png)
![image](https://user-images.githubusercontent.com/88712945/209456108-a97a204a-a0db-46e5-8a31-0483ec03d8a7.png)
![image](https://user-images.githubusercontent.com/88712945/209456112-cb50605d-ec51-4f89-a5fe-94c300bc1742.png)
-	Thực hiện chạy start zookeeper và kiểm tra tiến trình hoàn tất (trên 3 máy):
```python 
zkServer.sh start
jps
```





