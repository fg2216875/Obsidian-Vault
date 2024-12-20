### 1.比對查詢結果的總筆數
使用 `COUNT` 函數來檢查調整前後的查詢結果筆數是否一致，如果一致再往下比對資料

### 2.比對完整的查詢結果
以交集確認一致性，透過 `EXCEPT` 比對調整前後的結果，檢查有無資料遺失或新增。

```sql
SELECT DISTINCT JCU.Id 
FROM [mainTable] IGS 
INNER JOIN [tableA] JC ON JC.Code = IGS.code  
WHERE IGS.memberid = 'XXX' 
EXCEPT 
-- 調整後的 SQL 查詢 
SELECT DISTINCT JCU.Id 
FROM [mainTable] IGS 
INNER JOIN [tableA] JC ON JC.Code = IGS.code  
INNER JOIN [tableB] JCU ON JCU.Id = JC.id 
INNER JOIN [tableC] MOGS ON MOGS.groupid = IGS.groupid 
WHERE MOGS.memberid = 'XXX';
```
如果兩者的結果集均為空，則查詢結果一致