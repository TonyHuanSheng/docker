# Docker
參考網址: https://joshhu.gitbooks.io/dockercommands/content/DockerImages/ImageBasic.html
https://peihsinsu.gitbooks.io/docker-note-book/content/common-docker-mysql.html
### Ubuntu 安裝步驟 
### 參考網址:https://blog.gtwang.org/virtualization/ubuntu-linux-install-docker-tutorial/



## Docker 下載步驟
### 移除舊版Docker
```
1. $ sudo yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-engine
```

### 安裝Docker 
```
2. $ sudo yum install -y yum-utils
   
   $ sudo yum-config-manager \
     --add-repo \
     https://download.docker.com/linux/centos/docker-ce.repo
   
   $ sudo yum install docker-ce docker-ce-cli containerd.io
```
### 啟動Docekr
```
3. $ sudo systemctl start docker
```
### 測試Dokcer
```
4. $ docker run hello-world
```
### 查詢Docker版本
```
$ docker version
```
### 常用指令
| 指令 | 說明 |
|-----------------|----------|
| run |新建及啟動 |
| stop(contain ID) |停止|
| start(contain ID)|啟動|
| ps -a |列表|
| rm (contain ID)|刪除|
|exec (contain ID)|進入容器(開新的console)|
|attach|進入容器(退出停止容器)|
|logs (contain ID)|查看容器內資訊|
|inspect|查看容器資訊|
|stats|查看容器使用資源狀態|


### 查詢映像檔 
```
$ docker search [key-workd]
$ docker search centos
```
### Docker Hub的個人映像檔名稱

Docker是公開的映像檔集散中心，上面有不同的使用者，建立不同的倉庫，其中放了不同的映像檔。
上面粗體的字，即映像檔名稱的組成。
標準的Docker Hub的個人映像檔名稱格式為：

```
<user name>/<repo name>:<tag name>
```
* user name : 使用者名稱。在Docker Hub上每個使用者都有一個獨立的名稱，這是Docker Hub上的最大單位。
* repo name : 倉庫名稱。在Docker Hub上的每一個使用者，都可以建立自己的倉庫，倉庫中可以放多個映像檔。
* tag name：要分辨同一個倉庫中的不同映像檔，就要用tag name來區分。
            如果該倉庫中只有一個映像檔，則tag name可以省略。
            如果該倉庫中有多個映像檔，在沒有指定tag name時，以最新的一個為主。
            同一個映像檔可以有多個tag name，可看做是別名。可以從相同的映像檔ID看出來。
### 透過search我們可以找尋docker hub中所提供的image(我們又稱映像檔)，而給定的關鍵字複雜程度，則會決定回覆的訊息多寡

| 名稱 | 介紹 |
|------|:--------:|
| Name | 映像檔名稱|
| DESCRIPTION | 映像檔描述 |
| STARS | 越多代表越多人使用 |
| OFFICIAL | 官方image|
| AUTOMATED | 自動化|

Docker hub除了Web的搜訓介面，也同時提供了網頁搜尋介面，
讓使用者可以透過Browser快速的搜尋查看可用的docker image，
查詢目前有哪些docker image可以用 
http://hub.docker.com

### 查看本地映像檔
搜尋local已經存在的docker image，已經存在的image在執行時候會直接可以執行，不用再經過下載的動作
```
$docker images
```
| 名稱 | 介紹 |
|------|:--------:|
| REPOSITORY | 倉庫位置與映像檔名稱|
| TAG | 映像檔標籤 |
| IMAGE ID | 映像檔ID |
| CREATED | 創建日期 |
| SIZE | 映像檔大小 |

images 常用參數 
* -a : 列出完整的映像檔層次資訊。每個映像檔是由不同層次組成的
* -q : 只列出映像檔ID。這在做映像檔批次處理時很方便。

### 下載映像檔 Docker pull
從Docker Hub下載映像檔，沒有加任何Registry的位址時，就預設從官方的Registry下載(registry.hub.docker.com)。
要將某一個倉庫的所有映像檔都下載回來，可使用-a參數。但這樣要小心，因為有可能會太大，下載需要很長時間。
```
$ docker pull <映像檔名稱>
$ docker pull ubuntu:latest
```
### 創建並啟動容器
```
$ docker run <參數><image name><想執行的語句>
$ docker run -d --name mysql -p 3306:3306 mysql
```
* -i :讓容器標準輸入保持打開
* -t :讓Docker分配一個虛擬終端(pseudo-tty)並綁到容器的標準輸入上
* -d :背景執行
* -e :設定環境變數
* -P :Port 對應Port(host port:containar port)
* -v :資料對應
* --name :設定容器名稱
* --link :串接其他容器
* --network :加入指定網路

### 查看容器列表
```
$ docker ps -a 
```
| 名稱 | 介紹 |
|------|:--------:|
| CONTAINER ID| 容器ID |
| IMAGE | 映像檔名稱 |
| COMMAND | 執行指令 |
| CREATED | 創建時間 |
| STATUS | 容器狀態|
| POATS |開啟的Port號|
| NAMES |容器名稱|

### 將映像檔存入/匯出電腦檔案格式 docker save/load

Docker的映像檔雖然名為檔案，但其格式十分複雜。
你也不知道他存在哪，也不知道什麼格式，如果想要和其它人交換時 ，不想上傳到Docker Hub，
不想自己架設私有Docker Registry，就可以用這兩個參數存成tarball格式及匯出。
存入時別忘了加-o參數，要不然只會在顯示不會真的壓縮。

```
$ docker save -o weddemo.tar joshhu/webdemo:u12
```
### 將tarball還原回映像檔格式，則用load。可輸入
```
$ docker load --input webdemo.tar
```

### 刪除映像檔 docker rmi
這個指令刪除本機中存放的映像檔。但如果有容器還在使用這個映像檔，則無法刪除。如果硬要刪除，可以下-f參數強迫刪除。

前面提到映像檔是以層次的方式來存放，因此一個映像檔會有多個層次。你可以下--no-prune=true這個參數，只殺掉有tag name的映像檔。

以一個標準的映像檔來說，只會殺掉最上面一層，因為建立時的其它中間層次並不會有tag name，

這樣做的好處是可以留下許多映像檔共用的母層次。這也是Docker映像檔指令中，唯一能處理到層次這個等級的參數了

```
$ docker rmi --no-prune=true <user name>/<repo name>:<tag name>
```
### 一次刪掉所有的映像檔
配合Linux的批次指令來一次清乾淨所的映像檔

```
$ docker rmi -f $(docker images -aq)
```

### Docker run mysql
```docker
$ systemctl start docker
$ docker pull mysql
$ mkdir -p ~/mysql/data ~/mysql/logs ~/mysql/conf
$ cd mysql
$ docker run -dit --name dockermysql -p 3307:3306 \
                  -v $PWD/conf/my.cmf:/etc/mysql/my.cnf \
                  -v $PWD/logs:/logs \
                  -v $PWD/data:/mysql_data \
                  -e MYSQL_ROOT_PASSWORD=123456 \
                  -d mysql
$ docker exec -it dockermysql mysql -u root -p
Enter password:
mysql>
```
如果docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"rootfs_linux.go:58: mounting \\\"/home/user/mysql/conf/conf/my.cnf\\\" to rootfs \\\"/var/lib/docker/overlay2/3c051f3ae1e441904607b793c172c2a3d818c231b78c28681ab37facdb8917c9/merged\\\" at \\\"/var/lib/docker/overlay2/3c051f3ae1e441904607b793c172c2a3d818c231b78c28681ab37facdb8917c9/merged/etc/mysql/my.cnf\\\" caused \\\"not a directory\\\"\"": unknown: Are you trying to mount a directory onto a file (or vice-versa)? Check if the specified host path exists and is the expected type
某個映像檔是目錄而不是文件，造成映射錯誤
```
$ sudo rm -r conf
$ mkdir conf
$ cd conf
$ sudo touch my.cnf

$ cd mysql
$ docker run -dit --name dockermysql -p 3307:3306 \
                  -v $PWD/conf/my.cnf:/etc/mysql/my.cnf \
                  -v $PWD/logs:/logs \
                  -v $PWD/data:/mysql_data \
                  -e MYSQL_ROOT_PASSWORD=123456 \
                  -d mysql
```

