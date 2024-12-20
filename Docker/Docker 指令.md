使用方式:
**打開cmd輸入指令**
### 查看所有容器(包含停止的容器)
```
docker ps -a
```

---
### 停止整組容器(容器和網路不會被刪除)
```
docker-compose stop
```

---
### 啟動整組容器
```
docker-compose up [選項]
```

`-d`：以**背景模式**啟動容器（終端窗口不會被容器運行的輸出日誌占據，建議在不需要即時查看日誌時使用）。
`docker-compose up -d`

`--build`：在啟動容器前重新構建映像檔（適合在 Dockerfile 更新時使用）
`docker-compose up --build

---
### 停止並清理整組容器
```
docker-compose down [選項]
```

`--rmi all`：刪除所有相關的映像檔。
`docker-compose down --rmi all`

重新啟動主機建議使用`docker-compose down`，確保容器、網路和相關資源被乾淨地移除

---
