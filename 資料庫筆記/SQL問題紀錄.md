### chinese_taiwan_stroke_ci_as和Chinese_PRC_CI_AS之間的排序規則衝突

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
