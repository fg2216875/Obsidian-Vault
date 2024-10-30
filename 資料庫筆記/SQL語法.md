
## 基本CRUD:
```sql
新增資料:
Insert into T_XXXXX
 values ('2330','台積電')

更新資料:
update T_XXXX 
set Status = 0
where StockNO = '2330'

刪除資料:
delete from T_StockName
where StockNo = '2330'

新增資料表:
Create Table T_TEST
(TestID int IDENTITY(1,1) PRIMARY KEY, 
TestName nvarchar(30)
)
--IDENTITY(1,1)為設定流水號，左'1'為起始值，右'1'為每次增加量
--若需要在該欄位設定為主鍵時，在後面加上'PRIMARY KEY'即可

刪除整個資料表:
DROP TABLE T_XXXX;

僅刪除資料表內容，但保留結構 (TRUNCATE TABLE)
TRUNCATE TABLE T_XXXX;
```

---
## 在舊有Table新增欄位:

T-SQL：ALTER TABLE [我的資料表名稱] ADD   [欄位名稱]  [型態]

```sql
ALTER TABLE T_CashDaily ADD FundsTransferNumber int
```

---
## 變更資料欄位型態(但是變更欄位型態時有許多限制，詳細資料可參考MSDN):

T-SQL：ALTER TABLE [我的資料表名稱] ALTER COLUMN [欄位名稱] [型態]

```sql
ALTER  TABLE  T_EmpList  ALTER  COLUMN  EmpID int
```

---
## 刪除欄位:

T-SQL：ALTER TABLE [我的資料表名稱] DROP COLUMN [欄位名稱]

```sql
ALTER  TABLE  T_ProjectResource  DROP  COLUMN  AssignKey
```

---
## 一次新增多筆資料 (INSERT INTO SELECT)

運用逗號分隔資料，但要加入大量資料時則會變得不好使用
```sql
INSERT INTO table_name

VALUES (value1_1, value2_2, value3_3,···),

(value2_1, value2_2, value2_3,···),

(value3_1, value3_2, value3_3,···),

······;
```
## 或利用子查詢，從其它的資料表中取得資料來作一次多筆新增：

註:Insert子查詢不需要加values關鍵字
```sql
INSERT INTO table_name (column1, column2, column3,...)

SELECT othercolumn1, othercolumn2, othercolumn3,...

FROM othertable_name

where othercolumn1 = XXX;
```
## Insert子查詢+混和變數寫法:

註:如果要把變數Insert至特定欄位上，在select部分加上宣告的變數，並對應到該位置即可
```sql
declare @CreateDate date set @CreateDate = '2021/11/23';

declare @CreateUser int set @CreateUser = 172;

Insert into T_ProjectResourceHistory (PR_ID,RID,Price,WorkDay,Note,Subtotal,PC_ID,CreateDate,CreateUser)

select *,@CreateDate,@CreateUser from T_ProjectResource

where PC_ID = 2
```

---
## 新增資料並回傳主鍵
```sql
Insert into T_ProjectCostHistory

(PC_ID,ProjectID,ContractPrice,ContractPriceNoTax,AcceptancePrice,Expenses,GrossProfit,GrossProfitRate)

OUTPUT INSERTED.PCH_ID   --在OUTPUT後面選擇要回傳的欄位

select PC_ID,ProjectID,ContractPrice,ContractPriceNoTax,AcceptancePrice,Expenses,GrossProfit,GrossProfitRate

from T_ProjectCost where PC_ID = 15
```
```C#
var PCH_ID = connection.QuerySingle<int>(sql, parameters);  //使用Dapper執行SQL語法後，取得該筆新增資料的主鍵值
```

---
## 將使用子查詢得到的資料做更新
```sql
update b1
set b1.defaultguid = XXX
from cSC_BusinessUnit b1
where b1.BusinessUnitGUID = 7
```

----
## 取得目前資料中的下一筆資料
```sql
declare @PCH_ID int set @PCH_ID = 999999;

with RecordView as(       --先取得篩選後的資料
select PC_ID,PCH_ID,ContractPrice,CreateDate
from T_ProjectCostHistory 
where PC_ID = 23
	union ALL
select PC_ID,999999,ContractPrice,UpdateDate
from T_ProjectCost
where PC_ID = 23
)
SELECT * FROM RecordView --從篩選後的資料來源，取得目前的資料
where PCH_ID = @PCH_ID
	union
SELECT TOP 1 * FROM RecordView --從篩選後的資料來源，取得目前資料中的下一筆資料
where PCH_ID > @PCH_ID
order by CreateDate
```

---
## 取得資料中第X筆~第N筆
```sql
select * from T_WorkTimeInfo
where CreateUserID = 162
order by CreateTime
OFFSET 20 ROWS              --從第幾筆開始
FETCH NEXT 20 ROWS ONLY     --設定要取得的數量
```

----
## 於多筆重複資料中取得該重複群組中最新一筆資料

1.先對進度檔建立該案件群組進度排序資料，使用 ROW_NUMBER() 及 Partition By 語法
```sql
Select M_SN,STATUS,CDate, ROW_NUMBER() Over (Partition By M_SN Order By CDate Desc) As Sort From T_CS
```

2.查詢出來的條件已經依照CDate做遞減排序並編上序號，只需要將Sort編號為1的資料撈出來，就是該案件最新一筆的資料
```sql
Select M_SN,STATUS,CDate From
( Select M_SN,STATUS,CDate, ROW_NUMBER() Over (Partition By M_SN Order By CDate Desc) As Sort From T_CS) TMP_S
Where TMP_S.Sort=1
```
參考: <[https://dotblogs.com.tw/joysdw12/2011/12/28/63596](https://dotblogs.com.tw/joysdw12/2011/12/28/63596)>

----
## 一次執行多個Sql語法:

```sql
begin
Insert into T_XXXXX
values ('2330','台積電');
update T_XXXX 
set Status = 0
where StockNO = '2330';
end;
```
1.每個要執行的SQL段落結尾要加';'
2.須包含在begin ... end 之中

---
## 複製舊資料表的資料到新資料表中:

```sql
select * into T_NewTable from T_OldTable
```

---
## 將資料分組後再做distinct，去除指定欄位中的重複資料:

題目:
**Input:** 
DailySales table:
+-----------+-----------+---------+------------+
| date_id   | make_name | lead_id | partner_id |
+-----------+-----------+---------+------------+
| 2020-12-8 | toyota    | 0       | 1          |
| 2020-12-8 | toyota    | 1       | 0          |
| 2020-12-8 | toyota    | 1       | 2          |
| 2020-12-7 | toyota    | 0       | 2          |
| 2020-12-7 | toyota    | 0       | 1          |
| 2020-12-8 | honda     | 1       | 2          |
| 2020-12-8 | honda     | 2       | 1          |
| 2020-12-7 | honda     | 0       | 1          |
| 2020-12-7 | honda     | 1       | 2          |
| 2020-12-7 | honda     | 2       | 1          |
+-----------+-----------+---------+------------+
**Output:** 
+-----------+-----------+--------------+-----------------+
| date_id   | make_name | unique_leads | unique_partners |
+-----------+-----------+--------------+-----------------+
| 2020-12-8 | toyota    | 2            | 3               |
| 2020-12-7 | toyota    | 1            | 2               |
| 2020-12-8 | honda     | 2            | 2               |
| 2020-12-7 | honda     | 3            | 2               |
+-----------+-----------+--------------+-----------------+
```sql
select date_id,make_name,count(distinct(lead_id)) as unique_leads,count(distinct(partner_id)) as unique_partners 
from DailySales 
group by date_id,make_name
```

----
## PATINDEX 在字串中返回關鍵字的位置:

1.使用%搜尋特定關鍵字
```sql
Declare @input varchar(20) = 'Hello world!'
SELECT PATINDEX('%lo%', @input)
-- result : 4
```

2.*使用 '[ ]' 搜尋特定關鍵字*
在 A123456789 字串中找尋 英文開頭 ( 左邊沒加上%)，以及英文字母在中間及最右邊位置，都可正常找到並返回關鍵字位置
```sql
Declare @input varchar(20) = 'A123456789'
SELECT PATINDEX('A-z%', @input)
-- result : 1

Declare @input2 varchar(20) = '123456789A'
SELECT PATINDEX('%[A-z]', @input2)
-- result : 10

Declare @input3 varchar(20) = '123456Q789'
SELECT PATINDEX('%[A-z]%', @input3)
-- result : 7
```
參考:https://marcus116.blogspot.com/2019/03/mssql-t-sql-patindex.html

---
## 將子查詢資料更新到指定table中

```sql
UPDATE W SET W.Salary = A.Salary 
FROM tableW W 
JOIN ( 
	SELECT EmployeeID, Salary 
	FROM tableA 
	WHERE Active = 1 ) A ON W.EmployeeID = A.EmployeeID
```