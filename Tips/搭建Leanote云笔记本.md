# 搭建Leanote云笔记本
> Leanote 是目前为止发现的最有 bigger 的云笔记，具备 markdown 输入，
  代码高亮，多人协作，笔记历史记录，笔记内导航，直接发布为博客等等能力。
  本实验将带您一步步搭建属于自己的云笔记本，您将可以通过云笔记记录生活工作的点滴。 

## 安装MongoDB
1. 下载MongoDB
```
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.1.tgz
```
2. 解压MongoDB源码
```
tar -xzvf mongodb-linux-x86_64-3.0.1.tgz
```
3. 创建用于存储的文件夹目录
```
mkdir -p /data/db
```
4. 编辑`/etc/profile`，在文件末尾追加以下配置
```

export PATH=$PATH:/home/mongodb-linux-x86_64-3.0.1/bin
```
5. 执行以下命令，使上述修改生效
```
source /etc/profile
```
6. 启动MongoDB
```
mongod --bind_ip localhost --port 27017 --dbpath /data/db/ --logpath=/var/log/mongod.log --fork
```
## 安装Leanote
1. 下载Leanote源代码
```
wget https://iweb.dl.sourceforge.net/project/leanote-bin/2.4/leanote-linux-amd64-v2.4.bin.tar.gz
```
2. 解压缩Leanote源代码
```
tar -zxvf leanote-linux-amd64-v2.4.bin.tar.gz
```
3. 编辑Leanote配置文件
编辑`app.conf`，在文件中找到`app.secret=`项，并修改为如下内容：
```
app.secret=qcloud666
```
4. 初始化数据库
```
mongorestore -h localhost -d leanote --dir /home/leanote/mongodb_backup/leanote_install_data/
```
5. 启动Leanote服务
```
nohup /bin/bash /home/leanote/bin/run.sh >> /var/log/leanote.log 2>&1 &
```
6. 访问Leanote云笔记本
```
初始账户名：admin
初始密码：abc123
默认端口：9000
```
