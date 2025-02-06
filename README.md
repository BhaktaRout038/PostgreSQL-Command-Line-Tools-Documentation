
### **PostgreSQL Command-Line Tools Documentation**

This document provides an overview of common PostgreSQL command-line tools (`psql`, `pg_restore`, and `pg_dump`) and their usage, including how to restore multiple `.sql` files at once.

---

### 1. **psql** – PostgreSQL Command Line Tool

The `psql` command-line tool is used to interact with a PostgreSQL database. It allows you to execute SQL queries, run scripts, and perform administrative tasks.

#### Common `psql` Commands:

- **Connect to a Database:**

  ```bash
  psql -h <host> -U <username> -d <database_name>
  ```

- **Run SQL Commands from a File:**

  ```bash
  \i '<file_path>'
  ```

- **Exit `psql` Prompt:**

  ```sql
  \q
  ```

- **List Tables in the Database:**

  ```sql
  \dt
  ```

- **Describe a Table Structure:**

  ```sql
  \d <table_name>
  ```

- **Show Database Information:**

  ```sql
  \conninfo
  ```

- **Show All psql Meta-Commands:**

  ```sql
  \?
  ```

#### Example Command:
```bash
psql -h 192.168.142.21 -U postgres -d vidsanalytics
```

---

### 2. **pg_restore** – Restore PostgreSQL Database from Backup

`pg_restore` is used to restore a database from a custom-format dump created by `pg_dump`.

#### Common `pg_restore` Commands:

- **Restore from a Custom-Format Dump:**

  ```bash
  pg_restore -h <host> -U <username> -d <database_name> <dump_file_path>
  ```

- **Restore Specific Table:**
   below recommended for single table restore with schema & data both
   ```bash
     pg_restore -h <host> -U <username> -d <database_name>  "E:\demobackup\userdetails.sql"
   ```
   example
  
   ```bash
    pg_restore -h 192.168.142.21 -U postgres -d vidsanalytics "E:\demobackup\userdetails.sql"
   ```
    
  alternative

  ```bash
  pg_restore -h <host> -U <username> -d <database_name> -t <table_name> <dump_file_path>
  ```

- **Restore Multiple Tables:**

  To restore multiple tables, specify the `-t` option for each table:

  ```bash
  pg_restore -h <host> -U <username> -d <database_name> -t <table1> -t <table2> <dump_file_path>
  ```

#### Example Command:
```bash
pg_restore -h 192.168.142.21 -U postgres -d vidsanalytics -t tsuserdetails -t users -t orders "E:\demobackup\userdetails.sql"
```

---

### 3. **pg_dump** – Create a Backup of a PostgreSQL Database

`pg_dump` is used to create backups of PostgreSQL databases.

#### Common `pg_dump` Commands:

- **Backup the Entire Database:**

  ```bash
  pg_dump -h <host> -U <username> -d <database_name> -f <output_file_path>
  ```

- **Backup Only Specific Table:**

  ```bash
  pg_dump -h <host> -U <username> -d <database_name> -t <table_name> -f <output_file_path>
  ```

- **Create a Custom-Format Dump (for `pg_restore`):**

  ```bash
  pg_dump -h <host> -U <username> -d <database_name> -Fc -f <output_file_path>
  ```

#### Example Command:
```bash
pg_dump -h 192.168.142.21 -U postgres -d vidsanalytics -f "E:\demobackup\userdetails.sql"
```

---

### 4. **Restore Multiple Tables from Multiple `.sql` Files**

To restore multiple `.sql` files (such as `userdetails.sql`, `users.sql`, `orders.sql`, etc.) at once, use one of the following methods:

#### Method 1: Using Wildcards (Batch Restore)

If all your `.sql` files are in the same directory, you can restore them all at once using the following `psql` command with a wildcard `*`:

```bash
for %f in (E:\demobackup\*.sql) do psql -h 172.18.14.21 -U postgres -d vidsanalytics -f "%f"
```

#### Method 2: Using a Batch Script

You can create a `.bat` file to automate the process of restoring all `.sql` files in a folder:

1. Create a `.bat` file (e.g., `restore_multiple_tables.bat`).
2. Add the following content:

```bat
@echo off
for %%f in (E:\demobackup\*.sql) do (
    echo Restoring %%f
    psql -h 192.168.142.21 -U postgres -d vidsanalytics -f "%%f"
)
echo Restoration complete.
```

3. Run the script by double-clicking the `.bat` file or executing it from Command Prompt.

#### Method 3: Manually List Files

If you prefer to restore specific files, you can explicitly list each `.sql` file:

```bash
psql -h 192.168.142.21 -U postgres -d vidsanalytics -f "E:\demobackup\userdetails.sql"
psql -h 192.168.142.21 -U postgres -d vidsanalytics -f "E:\demobackup\users.sql"
psql -h 192.168.142.21 -U postgres -d vidsanalytics -f "E:\demobackup\orders.sql"
```

---

### 5. **General PostgreSQL Command-Line Options**

- **List All PostgreSQL Databases:**

  ```bash
  psql -U postgres -c "\l"
  ```

- **Show Table Names in a Database:**

  ```bash
  psql -U postgres -d your_database_name -c "\dt"
  ```

- **Describe a Table in the Database:**

  ```bash
  psql -U postgres -d your_database_name -c "\d your_table_name"
  ```

---

### Summary of Common Commands:
- **`psql`**: Command-line interface to interact with the database.
  - Connect: `psql -h <host> -U <username> -d <database>`
  - Run SQL: `\i '<file_path>'`
  - Exit: `\q`

- **`pg_restore`**: Restore a database from a custom-format dump.
  - Full Restore: `pg_restore -h <host> -U <username> -d <database> <dump_file>`
  - Restore Multiple Tables: `pg_restore -t <table1> -t <table2>`

- **`pg_dump`**: Create a backup of a database.
  - Full Backup: `pg_dump -h <host> -U <username> -d <database> -f <output_file>`
  - Table Backup: `pg_dump -t <table_name>`

- **Batch Restore Multiple `.sql` Files**: Use wildcards or a batch script to restore all `.sql` files in a folder.

These tools provide a powerful way to manage PostgreSQL databases directly from the command line. Let me know if you need further clarification or assistance!

--- 

Let me know if you need any additional details!
