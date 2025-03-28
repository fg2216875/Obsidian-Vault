錯誤訊息:
Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool. This may have occurred because all pooled connections were in use and max pool size was reached.

這個錯誤通常跟應用程式的兩個地方有關：  
1. 資料庫連線字串所使用的 connection pool 參數。
2. 程式的寫法有問題，導致資料庫連線沒有盡快釋放，以至於連線池爆滿。

---
#### Connection Pool(連線池):
connection pool 裡面會存放一些已經建立好、但目前沒人使用的連線，目的是讓用戶端快速取用。每當應用程式用完一條資料庫連線（亦即關閉連線）時，這條連線並沒有真正與資料庫伺服器斷開，而是先放到一個池子裏面。若該連線在池子裡面閒置了一段時間之後都沒有再被應用程式取用，才會真的釋放掉。

---
#### Connection Pool 的數量:
一個應用程式可能會建立不只一個 connection pool。如果一個應用程式使用了 10 個 pools，而每個 pool 裡面的尖峰連線數量是 100 條連線，那麼此應用程式對後端資料庫的連線總數大約就等於 1000。

connection pool 的數量越多，對 server 端的連線壓力自然也越大。所以應該要先確保 connection pool 數量越少越好，然後才去調整 pooling 的參數

不同的應用程式會使用不同的 connection pool，同一個應用程式當中如果使用了不同的連線字串，也會有各自的 pool。當 ADO.NET 在建立連線時，會去尋找現有的連線池，看看有沒有使用相同連線字串的池子，有則取用；若沒有，就建立一個新的連線池。  
  
除了連線字串之外，使用者身分也會令 ADO.NET 建立不同的 connection pool。結論就是<font style="color:red">連線字串最好都長得一模一樣，例如把連線字串放在應用程式組態檔裡面，而且連線字串裡面所使用的身分驗證方式，如非必要，應使用 SQL Sever 的特定 user ID，而不要用整合式驗證，以免每一個 Windows 使用者帳戶就有一個專屬的連線池，導致產生一大堆小池子</font>
這種狀況叫做 pool fragmentation）。

---
#### 使用連線字串控制 connection pooling 參數 :
連線字串有幾個參數可以控制 connection pooling 行為，底下是幾個比較常用的參數（for SQL Server）：  
- **Connection Lifetime** - 當用戶端關閉某個連線，亦即將那個連線還給 pool 時，這個連線就閒置在池子裏面，等其他用戶端取用。如果這個連線在池子裡閒置的時間超過此參數的設定值，就會被真的釋放掉（SQL Server 那頭的總連線數會少一個）。預設值為 0。
- **Max Pool Size** - 連線池裡面最多可以有幾個連線。預設值為 100。當連線池中的連線數量已經達到最大，此時應用程式若要再建立新的連線，就會先等待，等超過特定時間都還沒有可用的連線，就會發生連線逾時的錯誤。
- **Min Pool Size** - 連線池裡面最少要保持幾個連線。預設值為 0。若設定為 5，即表示當一個 pool 建立時，就要預先配置好 5 個可用連線，以便前五個用戶端直接提取。
- **Pooling** - 是否啟用 pooling，預設為 True。

如果應用程式發生第一個錯誤訊息的狀況，不用急著把 Max Pool Size 調大，而是要先確定程式寫法有沒有問題，也就是遵循這個原則：連線越晚建立越好，越早釋放越好。千萬別依賴 .NET 自動資源回收機制來幫你回收資料庫連線，否則在應用程式負載的尖峰期，連線池很快就會爆滿。

---
### connection pool特性整理:
- 連線字串要完全相同才能共享 Pool，故要避免動態產生不同連線字串，以免搞出一堆 Pool 失去共用資源的意義(術語叫 Pool Fragmentation)
- Pool 最大連線數(Max Pool Size)預設為 100 條
- Pool 連線數達上限時需排隊等待，若逾時將拋出錯誤，預設等待上限(Connection Timeout)為 15 秒
- 最低連線數(Min Pool Size)是每個 Pool 維持的基本連線數(最低數量)，預設為 0
- Pool 中的連線若閒置 4-8 分鐘或發生斷線(Severed Connection)，將被從 Pool 移除。

參考:
https://www.huanlintalk.com/2012/05/net-connection-pool.html
https://blog.darkthread.net/blog/sql-conn-pooling-experiment/