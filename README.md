import mysql.connector

# 1. Connect to MySQL
con = mysql.connector.connect(
    host="localhost",
    user="root",
    password="Sana@220803",
    database="etll.py"  # Specify the database name here
)
cursor = con.cursor()

print("Connected to MySQL")

cursor.execute("""
CREATE TABLE IF NOT EXISTS Employee (
    ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Department VARCHAR(100),
    Salary DECIMAL(10, 2)
)
""")


# =========================================================
# 1. SELECT ALL DATA
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_All AS
SELECT *
FROM Employee
""")

print("1. All data extracted")


# =========================================================
# 2. WHERE - FILTER DATA
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_High_Salary AS
SELECT *
FROM Employee
WHERE Salary > 50000
""")

print("2. Filtered data extracted")


# =========================================================
# 3. SELECT SPECIFIC COLUMNS
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Selected_Columns AS
SELECT ID, Name, Salary
FROM Employee
""")

print("3. Selected columns extracted")


# =========================================================
# 4. DISTINCT
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Departments AS
SELECT DISTINCT Department
FROM Employee
""")

print("4. Distinct departments extracted")


# =========================================================
# 5. ORDER BY
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Salary_Sorted AS
SELECT *
FROM Employee
ORDER BY Salary DESC
""")

print("5. Sorted data extracted")


# =========================================================
# 6. GROUP BY + COUNT
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Department_Count AS
SELECT Department, COUNT(*) AS Employee_Count
FROM Employee
GROUP BY Department
""")

print("6. Department count extracted")


# =========================================================
# 7. GROUP BY + AVG
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Average_Salary AS
SELECT Department, AVG(Salary) AS Average_Salary
FROM Employee
GROUP BY Department
""")

print("7. Average salary extracted")


# =========================================================
# 8. HAVING
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_High_Average_Salary AS
SELECT Department, AVG(Salary) AS Average_Salary
FROM Employee
GROUP BY Department
HAVING AVG(Salary) > 50000
""")

print("8. HAVING result extracted")


# =========================================================
# 9. BETWEEN
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Salary_Range AS
SELECT *
FROM Employee
WHERE Salary BETWEEN 40000 AND 70000
""")

print("9. BETWEEN result extracted")


# =========================================================
# 10. IN
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Selected_Departments AS
SELECT *
FROM Employee
WHERE Department IN ('IT', 'HR')
""")

print("10. IN result extracted")


# =========================================================
# 11. LIKE
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Name_Search AS
SELECT *
FROM Employee
WHERE Name LIKE 'A%'
""")

print("11. LIKE result extracted")


# =========================================================
# 12. CASE
# =========================================================

cursor.execute("""
CREATE TABLE IF NOT EXISTS Backup_Salary_Category AS
SELECT
    ID,
    Name,
    Department,
    Salary,
    CASE
        WHEN Salary >= 70000 THEN 'High'
        WHEN Salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS Salary_Category
FROM Employee
""")

print("12. CASE result extracted")


# =========================================================
# SAVE ALL CHANGES
# =========================================================

con.commit()

print("\nETL completed successfully!")


# =========================================================
# SHOW CREATED BACKUP TABLES
# =========================================================

cursor.execute("SHOW TABLES")

print("\nTables in database:")

for table in cursor.fetchall():
    print(table[0])


# =========================================================
# CLOSE CONNECTION
# =========================================================

cursor.close()
con.close()

print("\nMySQL connection closed")
