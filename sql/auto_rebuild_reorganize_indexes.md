
# Auto Rebuild or Reorganize Indexes Based on Fragmentation

### Step 1: Create the Temporary Table
```sql
USE YourDatabaseName; -- Replace with your actual DB name

IF OBJECT_ID('tempdb..##FragIndexes') IS NOT NULL DROP TABLE ##FragIndexes;

CREATE TABLE ##FragIndexes
(
    DatabaseName     SYSNAME NULL,
    SchemaName       SYSNAME NULL,
    TableName        SYSNAME NULL,
    IndexName        SYSNAME NULL,
    [Fragmentation%] FLOAT NULL,
    record_count     INT NULL,
    page_count       INT NULL
);
```

---

### Step 2: Insert Index Fragmentation Info
```sql
INSERT INTO ##FragIndexes
SELECT
    DB_NAME(DB_ID()) AS DatabaseName,
    ss.name AS SchemaName,
    OBJECT_NAME(s.object_id) AS TableName,
    i.name AS IndexName,
    s.avg_fragmentation_in_percent AS [Fragmentation%],
    s.record_count,
    s.page_count
FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'SAMPLED') s
INNER JOIN sys.indexes i 
    ON s.object_id = i.object_id AND s.index_id = i.index_id AND i.name IS NOT NULL
INNER JOIN sys.objects o 
    ON s.object_id = o.object_id
INNER JOIN sys.schemas ss 
    ON ss.schema_id = o.schema_id
WHERE s.database_id = DB_ID()
  AND s.page_count > 500;
```

---

### Step 3: (Optional) View Fragmented Indexes
```sql
SELECT * FROM ##FragIndexes;
```

---

### Step 4: Generate Index Rebuild/Reorganize Script
```sql
DECLARE @TableIndexes NVARCHAR(MAX) = '';

SELECT @TableIndexes = @TableIndexes +
CASE
    WHEN [Fragmentation%] > 35 THEN 
        CHAR(13) + 'ALTER INDEX ' + QUOTENAME(IndexName) + ' ON ' 
        + QUOTENAME(SchemaName) + '.' + QUOTENAME(TableName) + ' REBUILD;' + CHAR(13)
    WHEN [Fragmentation%] > 5 THEN 
        CHAR(13) + 'ALTER INDEX ' + QUOTENAME(IndexName) + ' ON ' 
        + QUOTENAME(SchemaName) + '.' + QUOTENAME(TableName) + ' REORGANIZE;' + CHAR(13)
    ELSE ''
END
FROM ##FragIndexes
WHERE [Fragmentation%] > 5;
```

---

### Step 5: Print Query (Useful for Large Output Debugging)
```sql
DECLARE @StartOffset INT = 1;
DECLARE @Length INT = 4000;

WHILE @StartOffset < LEN(@TableIndexes)
BEGIN
    PRINT SUBSTRING(@TableIndexes, @StartOffset, @Length);
    SET @StartOffset = @StartOffset + @Length;
END
```

---

### Step 6: Execute Index Optimization
```sql
EXEC sp_executesql @TableIndexes;
```

---

### Step 7: Cleanup
```sql
DROP TABLE ##FragIndexes;
```

---

### Tips
- Run during off-peak hours to avoid performance issues.
- Change `'SAMPLED'` to `'DETAILED'` for more accurate stats (but slower).
- Filter by specific schemas or tables if needed.
