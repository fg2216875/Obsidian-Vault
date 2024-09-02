1.在控制台中，開啟"Windows功能"(Win 10家用版會呈現這些選項)
![[Pasted image 20240223172550.png]]

(win 10企業版、專業版則要勾選"Hyper-V")
![[Pasted image 20240223172848.png]]

============================================
2.到官網下載docker desktop(https://www.docker.com/products/docker-desktop/)
執行安裝，並於安裝好後重新開機

若執行docker desktop出現此錯誤訊息，則需要到[微軟網站](https://learn.microsoft.com/zh-tw/windows/wsl/install)查閱
![[Pasted image 20240223173527.png]]

在PowerShell或Windows命令題是字元中，輸入下方安裝WSL指令後，並重新開機
``` PowerShell
wsl --install
```

=================================================
3.執行docker desktop，初始化設定選擇recommend項目，等處理好後若沒問題便安裝完成


參考:
https://ithelp.ithome.com.tw/articles/10340809