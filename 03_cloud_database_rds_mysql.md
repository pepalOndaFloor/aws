# Assignment 7: Create a Cloud Database with Amazon RDS and MySQL

## Objective
Create a MySQL database on Amazon RDS, connect to it, create a table, insert records, and run SQL queries.

## Requirements
- AWS account and access to the AWS Management Console
- MySQL Workbench or a MySQL command-line client
- A secure database password

## Instructions

### 1. Create an RDS database
1. Sign in to AWS and open **Amazon RDS**.
2. Select **Create database**.
3. Choose **MySQL** as the database engine.
4. Choose an appropriate template. Use a Free Tier option only if it is available and suitable for your account.
5. Set a DB instance identifier, such as `shubham-db`.
6. Set the master username, such as `admin`, and create a strong password.
7. Choose an instance configuration and storage settings suitable for the practical.
8. Configure connectivity. For a lab connection from your computer, make sure the VPC, subnet group, public-access setting, and security group match your intended connection method.
9. Create the database and wait until its status is **Available**.

### 2. Configure database connectivity
1. Open the RDS database details page.
2. Copy the database **Endpoint** and note the port, normally `3306` for MySQL.
3. In the database's VPC security group, add an inbound MySQL/Aurora rule on port `3306` from your specific client IP address.
4. Avoid allowing port `3306` from `0.0.0.0/0`. If the database is private, connect from a permitted network or a bastion/compute instance instead.

### 3. Connect using MySQL Workbench
1. Open MySQL Workbench and create a new connection.
2. Enter a connection name.
3. Enter the RDS endpoint as the hostname.
4. Set the port to `3306`.
5. Enter the master username, such as `admin`.
6. Enter the password when prompted.
7. Select **Test Connection**. If successful, save the connection and open it.

### 4. Create and select the database
Run:

```sql
CREATE DATABASE students_db;
USE students_db;
```

### 5. Create the students table
Run:

```sql
CREATE TABLE students_info (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    course VARCHAR(50),
    marks INT
);
```

### 6. Insert sample records
Run:

```sql
INSERT INTO students_info (id, name, age, course, marks) VALUES
(1, 'Shubham', 22, 'SYMCA', 95),
(2, 'Akshay', 23, 'FYMCA', 90),
(3, 'Athrava', 22, 'FYMCA', 96),
(4, 'Yash', 24, 'SYMCA', 88),
(5, 'Suraj', 23, 'SYMCA', 95);
```

### 7. Display all records
Run:

```sql
SELECT * FROM students_info;
```

### 8. Run the required queries
Display students with marks greater than 80:

```sql
SELECT *
FROM students_info
WHERE marks > 80;
```

Display only student names:

```sql
SELECT name
FROM students_info;
```

## Expected result
The `students_db` database and `students_info` table are created, five sample records are inserted, and the queries return the expected rows.

## Troubleshooting
- **Connection timeout:** Check the endpoint, Region, network route, and security-group inbound rule.
- **Access denied:** Verify the username and password.
- **Unknown database:** Run `CREATE DATABASE students_db;` and then `USE students_db;`.
- **Table already exists:** Reuse the existing table or remove it only if you are sure its data is not needed.

## Security and cost
Keep the database password private. Restrict network access to trusted sources. Review the RDS pricing and delete the test database when finished if you no longer need it.
