#### 找出執行效率低落的t-sql
```sql
select * from (
	SELECT top 100
		DB_NAME(st.dbid) AS database_name,
		st.text AS query_text,  --執行的t-sql
		qs.creation_time,  -- 查詢首次執行時間
		qs.execution_count,  --執行總次數
		qs.total_elapsed_time / 1000000 AS total_elapsed_time_s,  --執行總花費時間(s)
		qs.max_elapsed_time / 1000000 AS max_elapsed_time_s,   --單次執行最高花費時間(s)
		qs.total_worker_time / 1000000 AS total_cpu_time_s,
		qs.total_logical_reads,
		qs.total_physical_reads,
		qs.total_elapsed_time / 1000000 / qs.execution_count as averageRatio  --單次查詢平均花費時間
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

---
#### 查詢目前連線數
```sql
--USE XXX
SELECT cntr_value AS User_Connections
FROM sys.sysperfinfo AS sp
WHERE sp.object_name = 'SQLServer:General Statistics'
AND sp.counter_name = 'User Connections'
```

---
#### 查詢連線資訊、IP
```sql
SELECT s.session_id, s.login_name, s.host_name, c.client_net_address, s.program_name
FROM sys.dm_exec_sessions s
JOIN sys.dm_exec_connections c
ON s.session_id = c.session_id
WHERE s.login_name != 'sa'
AND s.session_id <> @@SPID
ORDER BY s.login_name;
```