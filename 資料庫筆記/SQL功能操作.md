## 新增欄位、資料表時，在查詢時欄位會出現紅字

在"編輯" => "IntelliSense" => 重新整理本機快取

======================================================================

## 開啟SQL Server Profiler

1.選取路徑:工具 => SQL Server Profiler

2.新增追蹤，連接要追蹤的 DB Server：

3.設定屬性(可直接執行預設值)

4.完成，可開始紀錄事件

===========================================================================================

## 建立登入者帳號

首先使用Windows驗證(或sa帳號)登入後，安全性->登入按右鍵選擇新增登入
![[Pasted image 20220601154335.png]]
輸入登入名稱(帳號)及SQL Server驗證的密碼，並按確定
![[Pasted image 20220601154348.png]]
重新登入選擇SQL Server驗證登入，打上帳密，便可看到安全性的登入有出現QQQQ了
代表創立帳號成功了。
![[Pasted image 20220601154402.png]]
![[Pasted image 20220601154409.png]]
## 創立與開啟資料庫

不過當要開啟原本創建好的資料庫或要創建新的資料庫時，會出現建立失敗，原因是還沒有給這帳號設定權限。
![[Pasted image 20220601154423.png]]
這時候請使用Windows驗證登入，登入後對QQQQ使用者右鍵屬性，並選擇使用者對應。
![[Pasted image 20220601154430.png]]
選擇要賦予權限的資料庫(orders)，並將下方 db_owner打勾。

db_owner 固定資料庫角色的成員可以在資料庫上執行所有的組態和維護活動，也可以在 SQL Server中卸除資料庫。 (在 SQL Database 和 SQL 資料倉儲中，某些維護活動需要伺服器層級的權限，而且無法由 db_owners執行。)
![[Pasted image 20220601154442.png]]
重新登入QQQQ後就可以建立資料庫及開啟orders資料庫了。

## 修改密碼

修改密碼，對要修改帳號右鍵屬性(或快速按兩下)，選一般，接著直接改密碼按確定就可以了。

## 刪除帳號

刪除帳號，須使用Windows登入後，接著對帳號右鍵刪除，便可刪除，

不過sa管理員帳戶以及目前正在登入的帳戶不可被刪除。

來自 <[https://ithelp.ithome.com.tw/articles/10214386](https://ithelp.ithome.com.tw/articles/10214386)>

========================================================================================

## 產生指令碼

1.對要產生指令碼的資料庫右鍵 => 工作 => 產生指令碼

2.下一步 => 選取特定的資料庫物件 => 下一步 => 進階

3.要編寫指令碼的資料類型 = 結構描述和資料

編寫DROP及CREATE的指令碼 = 編寫 DROP 及 CREATE 的指令碼

4.儲存為指令檔 => 檔案名稱(修改儲存位置)

=========================================================================================

## 建立trigger
```sql
CREATE TRIGGER [T_ProjectCostTrigger]

ON [T_ProjectCost] [after] [Update]     --在更新之後觸發

as begin

    INSERT INTO T_ProjectCostHistory   --將資料Insert至T_ProjectCostHistory

SELECT * FROM deleted;             --deleted: 選取修改前的資料，inserted: 選取修改後的資料

end

go
```
---------------------------

## 移除trigger
```sql
DROP TRIGGER IF EXISTS [T_ProjectCostTrigger]
```