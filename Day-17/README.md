# Day 015: PostgreSQL Database and User Setup

## Objective

Set up a PostgreSQL database and user on the Nautilus database server (`stdb01`) as a prerequisite for deploying a newly developed application in the Stratos Data Center.

## Task Requirements

- Create a PostgreSQL user named `kodekloud_sam`.
- Set the user's password to `BruCStnMT5`.
- Create a database named `kodekloud_db6`.
- Grant all database-level privileges on `kodekloud_db6` to `kodekloud_sam`.
- Do not restart the PostgreSQL service.

## Steps

### 1. Access the Nautilus Database Server

```bash
ssh peter@stdb01
```

### 2. Access the PostgreSQL Database Server

Switched to the PostgreSQL system user.

```bash
sudo su - postgres
```

### 3. Create the Database

Created the database `kodekloud_db6`.

```bash
psql -c "CREATE DATABASE kodekloud_db6;"
```

### 4. Create the PostgreSQL User

Created the login role `kodekloud_sam` and assigned the required password.

```bash
psql -c "CREATE ROLE kodekloud_sam LOGIN PASSWORD 'BruCStnMT5';"
```

In PostgreSQL, creating a role with the `LOGIN` attribute is equivalent to creating a user.

### 5. Grant Database Privileges

Granted all available database-level privileges on `kodekloud_db6` to `kodekloud_sam`.

```bash
psql -c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_sam;"
```

### 6. Verify the Database and User

Listed PostgreSQL databases to verify that `kodekloud_db6` was created.

```bash
psql -c "\l"
```

Listed PostgreSQL roles and users to verify that `kodekloud_sam` was created.

```bash
psql -c "\du"
```

## What I Learned

- PostgreSQL roles and users can be created using `CREATE ROLE` and `CREATE USER`.
- The `LOGIN` attribute allows a PostgreSQL role to authenticate.
- PostgreSQL databases can be created using `CREATE DATABASE`.
- Database-level permissions can be assigned using `GRANT ALL PRIVILEGES`.
- `psql -c` allows SQL commands to be executed directly from the Linux terminal.
- The `\l` command lists PostgreSQL databases.
- The `\du` command lists PostgreSQL roles and users.
- PostgreSQL database and user changes do not require a service restart.
- Database-level privileges do not automatically grant privileges on all tables and sequences inside the database.
