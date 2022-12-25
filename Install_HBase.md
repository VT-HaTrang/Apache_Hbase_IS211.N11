# 1. Cài đặt HBase
**Để cài đặt HBase, ta thực hiện theo các bước sau:**
-	Thực hiện download Hbase (trên 3tnmaster):
```python
cd ~
wget https://archive.apache.org/dist/hbase/2.4.0/hbase-2.4.0-bin.tar.gz
```
-	Thực hiện copy qua các máy slave (trên 3tnmaster):
```python
scp hbase-2.4.0-bin.tar.gz hadoopuser@192.168.56.51:/home/hadoopuser
scp hbase-2.4.0-bin.tar.gz hadoopuser@192.168.56.52:/home/hadoopuser
```
-	Giải nén file cài và move vào folder */usr/local/hbase* (trên cả 3 máy):
```python
sudo tar -zxvf hbase-2.4.0-bin.tar.gz
sudo mv hbase-2.4.0 /usr/local/hbase
```
-	Thêm các dòng sau vào file *.bashrc* (trên cả 3 máy):
```python
export HBASE_HOME=/usr/local/hbase
export PATH=$PATH:$HBASE_HOME/bin
```
Sau đó chạy lệnh ```python source ~/.bashrc```
-	Thưc hiện chỉnh sửa hbase-env.sh (trên cả 3 máy):
```python
sudo nano /usr/local/hbase/conf/hbase-env.sh
```
Sau đó thêm các lệnh sau:
```python
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export HBASE_PID_DIR=/usr/local/hbase/pids #nếu chưa có folder pids thì phải tạo thêm.
export HBASE_MANAGES_ZK=false
```
*Note: nếu chưa có folder pids thì phải tạo thêm.*
-	Thay đổi file hbase-site.xml (trên 3tnmaster):
```python
sudo nano /usr/local/hbase/conf/hbase-site.xml
```
Sau đó thêm các dòng sau
![image](https://user-images.githubusercontent.com/88712945/209456231-a48dc777-3bd1-4a20-b222-e88d8096390f.png)
-	Thực hiện sửa đổi file hbase-site.xml (trên slave1, 2)
```python
sudo nano /usr/local/hbase/conf/hbase-site.xml
```
Sau đó thêm các dòng như ảnh bên dưới
![image](https://user-images.githubusercontent.com/88712945/209456244-9b4cf2b1-86fa-4d22-ab42-4dbbea45d2b0.png)
-	Thực hiện chỉnh sửa file regionservers (trên 3tnmaster):
```python
sudo nano /usr/local/hbase/conf/regionservers
```
![image](https://user-images.githubusercontent.com/88712945/209456252-6cea472a-b846-4dfe-aab9-ed475e0c4598.png)
-	Thực hiện khởi động hadoop cluster (trên tnmaster):
```python
start-all.sh 
```
-	Thực hiện khơi động Hbase (trên tnmaster):
```python 
start-hbase.sh
```
-	Kiểm tra quá trình hoàn tất:
![image](https://user-images.githubusercontent.com/88712945/209456267-547f2d2c-ebbc-4760-aeed-18b652d8f395.png)
-	Kiểm tra trên web:
![image](https://user-images.githubusercontent.com/88712945/209456274-ff6af387-1e1f-4be0-8367-8119cc7950ab.png)

