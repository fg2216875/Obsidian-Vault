# [ASP.NET Core 3 系列 - 程式生命週期]
ASP.NET DI 容器提供三種選項：

1.  Singleton  
    整個 Process 只建立一個實例，任何時候都共用它。
2.  Scoped  
    在網頁 Request 處理過程(指接到瀏覽器請求到回傳結果前的執行期間)共用一個實例。
3.  Transient  
    每次要求元件時就建立一個新的，永不共用。