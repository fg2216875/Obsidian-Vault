### 找出執行效率低落的t-sql
```sql
select * from (
	SELECT top 100
		DB_NAME(st.dbid) AS database_name,
		st.text AS query_text, 
		qs.creation_time,  
		qs.execution_count,  --
		qs.total_elapsed_time / 1000000 AS total_elapsed_time_s,  
		qs.max_elapsed_time / 1000000 AS max_elapsed_time_s,   
		qs.total_worker_time / 1000000 AS total_cpu_time_s,
		qs.total_logical_reads,
		qs.total_physical_reads,
		qs.total_elapsed_time / 1000000 / qs.execution_count as averageRatio  
	FROM 
		sys.dm_exec_query_stats qs
	CROSS APPLY 
		sys.dm_exec_sql_text(qs.sql_handle) st
	where creation_time >= '2024-10-01'
	ORDER BY 
		max_elapsed_time_s DESC  -- 根據最長耗時排序
) a
where a.averageRatio > 0
order by a.averageRatio desc
```

| 欄位                     | 說明            |
| ---------------------- | ------------- |
| `query_text`           | 執行的t-sql語法    |
| `creation_time`        | 查詢首次執行時間      |
| `execution_count`      | 執行總次數         |
| `total_elapsed_time_s` | 執行總花費時間(s)    |
| `max_elapsed_time_s`   | 單次執行最高花費時間(s) |
| `total_cpu_time_s`     | CPU花費使用時間     |
| `total_elapsed_time`   | 單次查詢平均花費時間    |
| `start_time`           | 該指令開始執行的時間。   |
| `total_elapsed_time`   | 執行了多久（毫秒）。    |

---
### 查詢目前連線數
```sql
--USE XXX
SELECT cntr_value AS User_Connections
FROM sys.sysperfinfo AS sp
WHERE sp.object_name = 'SQLServer:General Statistics'
AND sp.counter_name = 'User Connections'
```

---
### 查詢連線資訊、IP
```sql
SELECT s.session_id, s.login_name, s.host_name, c.client_net_address, s.program_name
FROM sys.dm_exec_sessions s
JOIN sys.dm_exec_connections c
ON s.session_id = c.session_id
WHERE s.login_name != 'sa'
AND s.session_id <> @@SPID
ORDER BY s.login_name;
```

---
### 修改資料庫名稱
```sql
EXEC sp_renamedb 'OldDatabaseName', 'NewDatabaseName';
```

如果有人連線會導致無法刪除，可用這個指令先查詢有哪些人連線
```sql
SELECT * FROM sys.sysprocesses WHERE dbid = DB_ID('DB_NAME');

KILL <SPID>; --替換 `<SPID>` 為上一個查詢結果中的 `SPID` 值
```

----
### 查詢目前所有連線正在執行的 SQL 指令內容
```sql
SELECT
    r.session_id,
    s.login_name,
    s.host_name,
    s.program_name,
    r.status,
    r.command,
    r.start_time,
    r.cpu_time,
    r.total_elapsed_time,
    t.text AS sql_text
FROM
    sys.dm_exec_requests r
JOIN
    sys.dm_exec_sessions s ON r.session_id = s.session_id
CROSS APPLY
    sys.dm_exec_sql_text(r.sql_handle) t
ORDER BY
    r.start_time DESC;
```

| 欄位                   | 說明                                                      |
| -------------------- | ------------------------------------------------------- |
| `session_id`         | 等同於 `spid`，用來識別連線。                                      |
| `login_name`         | 登入帳號名稱。                                                 |
| `host_name`          | 發出連線的機器名稱。                                              |
| `program_name`       | 發出連線的程式名稱（例如 `Microsoft SQL Server Management Studio`）。 |
| `command`            | 目前執行的命令（可能是 `AWAITING COMMAND`）。                        |
| `sql_text`           | SQL 原始語句內容。                                             |
| `status`             | 連線狀態。                                                   |
| `start_time`         | 該指令開始執行的時間。                                             |
| `total_elapsed_time` | 執行了多久（毫秒）。                                              |

-----
