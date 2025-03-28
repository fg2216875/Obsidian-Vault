狀況:
對資料表刪除 isenabled 欄位後，使用EF查詢資料的程式碼出現錯誤

解決方式:
查看狀況為下方第2點，將類別的 isenabled 屬性移除後，就回復正常了

### 1.資料庫有多的欄位，但 Class 沒有對應的屬性:
如果 `EXAM_GROUPS` 資料表有 `ExamGroups` 類別中 **沒有的欄位**，EF **仍然可以正常查詢**，但不會載入這些額外的欄位
```sql
CREATE TABLE EXAM_GROUPS (
    examgroupid NVARCHAR(50) PRIMARY KEY,
    examgroupname NVARCHAR(100),
    isdeleted BIT,
    createtime DATETIME,
    -- 額外的欄位
    extra_column1 NVARCHAR(50),
    extra_column2 INT
)
```

---
### 2.資料庫缺少 ExamGroups 類別中定義的屬性:
如果 `EXAM_GROUPS` 表 **缺少 `ExamGroups` 類別中某些定義的屬性**，則可能會發生問題。例如，假設 `EXAM_GROUPS` 缺少 `isenabled` 欄位：
```csharp
public class ExamGroups
{
    [Key]
    public string examgroupid { get; set; }
    public string examgroupname { get; set; }
    public bool isdeleted { get; set; }
    public bool isenabled { get; set; } // ⚠️ 如果資料表缺少此欄位，可能會出錯
    public DateTime createtime { get; set; }
}
```

如果不想修改資料庫，但希望 EF 忽略這些欄位，可以加上 `[NotMapped]`
```csharp
[NotMapped]
public bool isenabled { get; set; }
```

---
### 3.資料表的欄位型別與 Class 屬性型別不符
假設 `EXAM_GROUPS` 中的 `createtime` 欄位類型是 `VARCHAR(50)`，但在 `ExamGroups` 類別中定義為 `DateTime`
```csharp
public DateTime createtime { get; set; } // 但資料表的 createtime 是 VARCHAR(50)
```

當 EF 查詢時，可能會因為型別不符合而拋出錯誤

----
### 4.使用 `DbContext` 時未正確載入資料
即使 `ExamGroups` 的欄位與資料表完全匹配，但如果 `DbContext` **沒有正確載入資料**，查詢仍可能會返回空結果


註:
碰到第2、3點時，會碰到拋出錯誤訊息的狀況