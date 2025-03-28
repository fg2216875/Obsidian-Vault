## 將Web API架設到IIS中:

1.點選專案然後選擇【發佈】
![[Pasted image 20220601152434.png]]
2.點選資料夾，然後設定要發佈的路徑
![[Pasted image 20220601152443.png]]
3.在IIS的預設網站，按滑鼠右鍵，點選【新增應用程式】，對應上述目錄即可。
![[Pasted image 20220601152453.png]]
4.若出現 500.19 錯誤，將該目錄安全性授予 IIS_IUSRS 群組。
![[Pasted image 20220601152503.png]]

來自 https://ithelp.ithome.com.tw/questions/10201966

------
## IIS 跑 ASP.NET Core 出現 500.19 0x8007000d 錯誤

![[Pasted image 20220601152719.png]]
貌似 web.config 有錯，但設定來源的說明只有 -1: 0: 兩個莫名其妙的數字，下方說事件檢視器可能有線索，巡了一趟只看到幾個不知所云的警告：
![[Pasted image 20220601152726.png]]
如果在 IIS 模組清單沒看見ASP.NET Core 整合模組，表示還未安裝

官方文件 [Host ASP.NET Core on Windows with IIS](https://docs.microsoft.com/zh-tw/aspnet/core/host-and-deploy/iis/?view=aspnetcore-5.0&WT.mc_id=DOP-MVP-37580) 有下載連結，重新安裝：
![[Pasted image 20220601152743.png]]
最新版是 5.0.4，但 IIS 模組名稱仍是 AspNetCoreModuleV2：
![[Pasted image 20220601152749.png]]
安裝完畢，IIS 就能正常跑 ASP.NET Core 囉~

如要移除舊版 Microsoft .NET Core X - Windows Server Hosting，記得要裝新版。
![[Pasted image 20220601152758.png]]

來自 <[https://blog.darkthread.net/blog/aspnetcore-500-19-0x8007000d/](https://blog.darkthread.net/blog/aspnetcore-500-19-0x8007000d/)>
