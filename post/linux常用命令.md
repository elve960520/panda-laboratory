##端口相关
### lsof -i 
    lsof -i 用以显示符合条件的进程情况，lsof(list open files)是一个列出当前系统打开文件的工具。
### lsof -i:端口号
    lsof -i:端口号，用于查看某一端口的占用情况
### netstat -tunlp
    netstat -tunlp用于显示tcp，udp的端口和进程等相关情况
### netstat -tunlp|grep 端口号
    netstat -tunlp|grep 端口号，用于查看指定端口号的进程情况，如查看22端口的情况，netstat -tunlp|grep 22
### kill -9 dip
    杀死dip号的进程
## 本地和服务器文件交互
### 从服务器上下载文件
    scp username@servername:/path/filename /var/www/local_dir（本地目录）
    例如scp root@192.168.0.101:/var/www/test.txt  把192.168.0.101上的/var/www/test.txt 的文件下载到/var/www/local_dir（本地目录）
### 上传本地文件到服务器
    scp /path/filename username@servername:/path   
    例如scp /var/www/test.php  root@192.168.0.101:/var/www/  把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中
### 从服务器下载整个目录
    scp -r username@servername:/var/www/remote_dir/（远程目录） /var/www/local_dir（本地目录）
    例如:scp -r root@192.168.0.101:/var/www/test  /var/www/  
### 上传目录到服务器
    scp  -r local_dir username@servername:remote_dir
    例如：scp -r test  root@192.168.0.101:/var/www/   把当前目录下的test目录上传到服务器的/var/www/ 目录
## 常用命令
### 移动
    mv /path/filename /path/filename
### 复制
    cp /path/filename /path/filename