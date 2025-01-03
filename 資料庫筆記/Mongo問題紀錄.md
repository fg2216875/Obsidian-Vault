### 突然無法連線至MongoDB的狀況

1.有可能是MongoDB服務被停止，導致出現連線錯誤的情形
解決方式:

到架設的主機上，若是windows系統可開啟CMD輸入指令，檢查 MongoDB 服務狀態
```cmd
sc query MongoDB
```

若服務未啟動，可以手動啟動：
```cmd
net start MongoDB
```

如果需要停止服務，可以執行：
```cmd
net stop MongoDB
```

備註:
可使用 Windows 系統事件檢視器，左側選單選擇 `Windows Logs -> System`查看系統執行狀況