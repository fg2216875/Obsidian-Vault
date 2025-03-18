## chinese_taiwan_stroke_ci_as和Chinese_PRC_CI_AS之間的排序規則衝突

當這兩個排序規則在同一查詢中一起使用時，會發生衝突，通常是因為排序規則的區別會影響字串的比較和排序方式。

**解決方式:**
1.**在查詢中強制統一排序規則** 使用 `COLLATE` 關鍵字明確指定列或常數使用的排序規則。
```sql
SELECT * FROM TableA a
JOIN TableB b
ON a.Column1 COLLATE Chinese_PRC_CI_AS = b.Column2 COLLATE Chinese_PRC_CI_AS;
```

2.**統一資料庫表的排序規則** ，對表的排序規則進行修改，統一所有涉及的欄位排序規則。
```sql
ALTER TABLE TableName
ALTER COLUMN ColumnName NVARCHAR(100) COLLATE Chinese_Taiwan_Stroke_CI_AS;
```
**注意**：這可能需要時間並影響現有資料，因此應該謹慎執行，並在進行前做好備份。

#### 處理含有索引或約束的欄位

1.首先刪除依賴於該欄位的索引或約束。
```sql
DROP INDEX IndexName ON TableName;
```

2.修改欄位為資料庫預設排序規則。
```sql
ALTER TABLE TableName
ALTER COLUMN ColumnName NVARCHAR(100) COLLATE Chinese_PRC_CI_AS;
```

3.在修改完成後，重新創建索引或約束。
```sql
CREATE INDEX IndexName ON TableName(ColumnName);
```

---
## UPDATE 陳述式與 FOREIGN KEY 條件約束 "FK_XXX_YYY" 衝突。

#### 1.更新的值不存在於父表
當嘗試更新Table_B(子表)的外部鍵欄位值時，由於這個值在Table_A(父表)的主鍵(PK)並不存在，因此導致這個錯誤發生。

#### 2.父表記錄正被參考且設定不允許更新或刪除
如果父表中某筆記錄正在被子表引用，而外部鍵限制不允許更新或刪除父表的該記錄，會導致錯誤。

### 解決方式:

1.確認父表中是否有該值
```sql
--找出子表中有哪些外鍵值，不存在於父表的主鍵中
SELECT DISTINCT ColumnA
FROM TableA -- 子表
WHERE ColumnA IS NOT NULL -- 避免 NULL 值影響結果
AND ColumnA NOT IN (
    SELECT ColumnA
    FROM TableB -- 父表
);
```

2.更新子表資料前處理父表資料

3.調整或暫時禁用外部鍵
```sql
ALTER TABLE Orders NOCHECK CONSTRAINT FK_Orders_Customers;
-- 執行操作
ALTER TABLE Orders CHECK CONSTRAINT FK_Orders_Customers;
```

---
## 無法以資料庫主體執行，因為主體 "dbo" 不存在、無法模擬這種主體、或者您沒有權限。

在SSMS要建立"資料庫圖表"時，突然出現這個錯誤訊息，經過查看資料得知
可能導致此問題發生的常見案例如下:
### 1.在同一伺服器上還原，但原登入不存在或無效

1. **備份資料庫的時候**，某個使用者（例如 `sa`）是該資料庫的 `dbo`（擁有者），這個使用者是 SQL Server 登入或 Windows 驗證登入。
2. **之後在同一伺服器上還原該資料庫**，但原本建立資料庫的登入（`sa`）已不存在或無效：
    - 如果是 **SQL Server 登入**，可能該登入被刪除。
    - 如果是 **Windows 登入**，該登入的使用者可能已被移除，例如員工離職，Active Directory 停用了該使用者的帳號。

**結果：**
- 還原的資料庫仍然試圖指向原本的 `sa` 作為 `dbo` 或主要登入，但系統無法找到此使用者，導致錯誤。


### 2.在不同伺服器上還原，但主體無法匹配

1. **資料庫是在伺服器 A 上建立的**，其主體為登入`sa`，該登入的 `sid`（安全識別碼）是伺服器 A 特定的。
2. **將資料庫備份還原到伺服器 B**，伺服器 B 上同樣存在一個名為 `sa` 的登入，但由於 SQL Server 是使用 `sid` 來識別登入，而不是名稱，伺服器 A 的 `sid` 與伺服器 B 的 `sid` 不一致。

**結果：**
- 還原的資料庫指向的 `sa` 登入，因 `sid` 不匹配，被視為無效。

### 解決方式：

1.確認資料庫的當前擁有者，如果 `Owner_Name` 也為 `NULL`，則表示資料庫目前沒有正確的擁有者。
```sql
SELECT NAME AS Database_Name, owner_sid, SUSER_SNAME(owner_sid) AS OwnerName FROM sys.databases WHERE NAME = 'Db_Name';
```
結果:
```
Database_Name    owner_sid                            OwnerName
--------------- -----------------------------------------------
Db_Name          0xD63086DD7277BC4EB88013D359AF73A6   sa
```

2.接著檢查 sys.database_principals 中的`Owner_Name`值 
```sql
SELECT SUSER_SNAME(sid) AS Owner_Name, sid 
FROM Db_Name.sys.database_principals 
WHERE NAME = 'dbo';
```
結果:
```
Owner_Name         sid 
------------------ ---------------------------------------------
NULL               0xDB79ED7B6731CF4E8DC7DF02871E3E36
```

如果Owner_Name為`null`的話，表示資料庫 `dbo` 使用者（Database Owner）的 `SID` 在伺服器層級找不到對應的登入帳號，需要執行下列的指令，將 dbo 變更為有效的使用者。

3.重新設定資料庫的擁有者（例如 `sa`）
```sql
USE [YourDatabase];
ALTER AUTHORIZATION ON DATABASE::[YourDatabase] TO [sa];
```

參考:
[MSSQLSERVER_15517](https://learn.microsoft.com/zh-tw/sql/relational-databases/errors-events/mssqlserver-15517-database-engine-error?view=sql-server-ver16)


