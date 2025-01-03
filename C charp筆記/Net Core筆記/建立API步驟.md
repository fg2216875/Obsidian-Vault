1.先建立對應DB資料表的class
![[Pasted image 20220601153006.png]]
2.在appsettings.json檔案中，在下方多添加一個資料庫的連線字串
![[Pasted image 20220601153013.png]]
3.在專案底下建立DapperContext類別，然後注入IConfiguration介面，讓我們可以從appsettings.json檔案中取得連線字串。
而我們也可以建立一個會回傳連線字串的CreateConnection函式來使用。
![[Pasted image 20220601153024.png]]
4.來到Startup.cs，在ConfigureServices裡註冊剛剛建立的DapperContext類別
![[Pasted image 20220601153032.png]]
5.先建立一個空白的ICompanyRepository介面
![[Pasted image 20220601153047.png]]
6.接著建立CompanyRepository類別並繼承ICompanyRepository介面，另寫一個私有層級的變數，接取建構子傳來的值
![[Pasted image 20220601153101.png]]
7.回到Startup.cs，在ConfigureServices中，註冊ICompanyRepository介面，以及實作介面的CompanyRepository
![[Pasted image 20220601153115.png]]
8.接著回到ICompanyRepository介面，定義GetCompanies方法
![[Pasted image 20220601153122.png]]
9.在CompanyRepository中，實作GetCompanies方法
![[Pasted image 20220601153128.png]]
10.在這裡，我們通過 DI 注入我們的資料庫，並使用它來呼叫我們的GetCompanies方法。
![[Pasted image 20220601153135.png]]
11.最後到Postman(或是程式中)，將網址輸入上去並測試API是否有正確回傳資料
![[Pasted image 20220601153143.png]]
這裡有幾點注意事項：
-   由於我們沒有任何類型的業務邏輯，所以沒有創建一個服務層來包裝我們的Repository。對於這種類型的應用程式，服務層只是用來呼叫Repository的方法，若加入服務層則會讓結構變得複雜。假如碰到規模較大的應用程式，依然建議使用服務層。
-   我們在控制器中的每個操作中使用 try-catch 塊，只是為了舉例。
-   我們不會解釋控制器的邏輯，我們假設您熟悉 Web API 開發。如果您想了解更多相關信息，可以閱讀我們的[ASP.NET Core Web API](https://code-maze.com/net-core-series/)系列

來自 <[https://code-maze.com/using-dapper-with-asp-net-core-web-api/](https://code-maze.com/using-dapper-with-asp-net-core-web-api/)>