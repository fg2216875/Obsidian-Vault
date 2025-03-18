#### 1.在 MongoDB 設定檔 `mongod.conf` 中設定自動 logRotate
(設定檔可能在C:\Program Files\MongoDB\....)

#### 2.找到"systemLog:"，在下一層加上"logRotate"
```
systemLog:
  destination: file
  path: /var/log/mongodb/mongod.log
  logAppend: true
  logRotate: rename  # 或 size
  # 若選 size，還可以加 maxSizeMB 控制最大大小
```
- `logRotate: reopen`：配合外部 logrotate 工具  
- `logRotate: size`：自動依大小切割 (新版本支援)
- 在 **YAML 格式** 中，`#` 開頭的行代表「**註解 (Comment)**」

**注意縮排**，YAML 格式一定要用正確的縮排（通常是兩個空格），否則 MongoDB 無法解析。

#### 3.重新啟動MongoDB服務
```powershell
Restart-Service MongoDB
```
或是
```cmd
net stop MongoDB
net start MongoDB
```

#### 4.設定排程自動切割Log檔案
步驟 1：開啟記事本

步驟 2：輸入批次檔內容
```
@echo off
mongo --eval "db.adminCommand({ logRotate: "server" })"
```

步驟 3：儲存為 .bat 檔案
1. 在記事本點選 **檔案 > 另存新檔**。
2. 選擇儲存位置（例如：桌面、D:\MongoDB\scripts）。
3. 檔名輸入：`mongo_logrotate.bat`
4. **檔案類型**選擇 **所有檔案** (避免儲存成 `.txt`)。
5. **編碼**選擇 **ANSI** (或 UTF-8 也可以)。

步驟 4 : 使用`工作排程器`設定執行頻率

---
註:
1.要確認權限驗證的部分
2.先使用Mongo Shell輸入指令確認
```
db.adminCommand( { logRotate : "server" } ) 
```

