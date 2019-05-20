# docker_2
# Docker use centos image
## 首先先將centos的images從Docker Hub pull 下來
>docker pull centos

### 因為對Linux還不夠熟悉所以想先將 container 裝上所需工具後再 commit 為 image。

### 客製 image 正統作法是透過 dockerfile 來 build 出適合的 image，不是將 container 裝上所需工具後再 commit 為 image

# 實作流程
### 啟動centos
>docker pull centos

### !遇到問題 (Timeout的關係)
>Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)

![](https://blog.johnwu.cc/images/x404.png)

![](https://blog.johnwu.cc/images/x405.png)

之後便可成功下載

### 之後先進入centos
>docker run -i -t centos

之後類似虛擬機可在裡面下指令

### 我想用這個當基礎來建立環境，所以我先commit他的image(自首不能大寫)
>docker commit 853d first-centos

### commit完之後便可以看到該名稱在我們的images裡

### 之後我嘗試在裡面安裝http,mysql,apache,php
>docker run -i -t first-centos
>yum install httpd
>systemctl start httpd.service

但在安裝完後httpd後
卡在 systemctl start httpd.service 

### 經查詢後
Docker的設計理念是在容器里面不運行後台服務
正确的使用容器方法是將里面的服务運行在前台

> 解決辦法，以特權模式運行容器

創建容器

> docker run -d --privileged=true first-centos /usr/sbin/init

進入容器

> docker exec -it centos7 /bin/bash
> systemctl start httpd.service 

沒跑出錯誤訊息代表成功
