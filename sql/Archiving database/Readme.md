# SQL Server Archiving Guide  
**Author:** Jose Antonio "Tony" Acebuche  

---

## 📌 Purpose  
To reduce the size of the production database, improve performance, and comply with data retention policies by safely archiving old or inactive data.

> 🧠 **Note:** Archiving databases is one of my regular practices to keep systems optimized and avoid performance bottlenecks.  
> 🛠️ I also use this archiving process on **live production databases** to manage space and improve overall performance — especially in systems handling large transaction volumes like kiosks and POS terminals.

---

## ✅ Benefits of Archiving
- Faster query performance and indexing
- Smaller database size and quicker backups
- Lower risk of storage overload
- Compliance with data retention regulations
- Historical data still retrievable when needed

---

## 🕒 When to Archive
- After old data reaches its retention period (e.g., > 2 years)
- Before a major upgrade or migration
- When disk space is running low
- During quarterly or biannual cleanups

---

## 🛠 Common Archiving Methods
1. **Full Database Backup (.bak)** stored in archive folder or NAS
2. **Exporting to CSV or Excel** using SSMS or T-SQL
3. **Archiving to a separate database/table** within SQL Server
4. **Setting a database to Read-Only** for legacy systems

---

## 🔐 Best Practices
- ✅ Always perform a full backup before archiving
- ✅ Validate row counts before deleting from production
- ✅ Avoid archiving during peak hours
- ✅ Use batch deletions to avoid table locks
- ✅ Rebuild indexes after deleting archived data

---

## 🧪 Example SQL Workflow

```sql
-- Step 1: Archive old records
INSERT INTO ArchiveDB.dbo.OldTransactions
SELECT * FROM ProductionDB.dbo.Transactions
WHERE CreatedDate < '2023-01-01';

-- Step 2: Validate row count
SELECT COUNT(*) FROM ArchiveDB.dbo.OldTransactions;
SELECT COUNT(*) FROM ProductionDB.dbo.Transactions WHERE CreatedDate < '2023-01-01';

-- Step 3: Delete from production
DELETE FROM ProductionDB.dbo.Transactions
WHERE CreatedDate < '2023-01-01';

-- Step 4: Rebuild indexes
ALTER INDEX ALL ON ProductionDB.dbo.Transactions REBUILD;
