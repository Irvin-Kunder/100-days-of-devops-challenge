# Day 018: Install and Configure DB Server

## Objective

To install and configure MariaDB on the Nautilus DB Server, create a database and user, and grant the user full permissions on the database.

## Task Requirements

- Install and configure the MariaDB server.
- Create a database named `kodekloud_db1`.
- Create a user named `kodekloud_rin` with the required password.
- Grant full permissions to the user on `kodekloud_db1`.

## Steps

### 1. Connect to the Nautilus DB Server

Connect to the database server from the jump host using the provided username:

```bash
ssh <username>@stdb01
```

### 2. Check and Install MariaDB

Check whether MariaDB is installed:

```bash
mysql --version
```

Install MariaDB using the package manager:

```bash
sudo yum install mariadb-server -y
```

### 3. Enable and Start MariaDB

Enable MariaDB to start automatically during system boot, start the service, and check its status:

```bash
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo systemctl status mariadb
```

### 4. Log in to MariaDB

Access the MariaDB prompt:

```bash
sudo mysql
```

### 5. Create the Database

Create the required database:

```sql
CREATE DATABASE kodekloud_db1;
```

### 6. Create the Database User

Create the user and assign the required password:

```sql
CREATE USER 'kodekloud_rin'@'%' IDENTIFIED BY 'dCV3szSGNA';
```

The `%` host value allows the account to connect from any host, subject to network and MariaDB access controls.

### 7. Grant Full Permissions

Grant all privileges on the database to the newly created user:

```sql
GRANT ALL PRIVILEGES ON kodekloud_db1.* TO 'kodekloud_rin'@'%';
```

Apply the privilege changes:

```sql
FLUSH PRIVILEGES;
```

### 8. Verify the Configuration

Verify that the database exists:

```sql
SHOW DATABASES;
```

Verify that the user exists:

```sql
SELECT User, Host FROM mysql.user WHERE User = 'kodekloud_rin';
```

Check the user's granted permissions:

```sql
SHOW GRANTS FOR 'kodekloud_rin'@'%';
```

## What I Learned

- How to check whether MariaDB is installed and install the server package.
- How to enable, start, and check the status of a MariaDB service using `systemctl`.
- How to create databases and user accounts in MariaDB.
- How to grant database-level privileges using `GRANT ALL PRIVILEGES`.
- How to verify database creation, user accounts, and permissions using SQL queries.
- The meaning of `%` in a MariaDB user account's host field.
