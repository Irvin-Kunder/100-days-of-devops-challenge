# Day 8: Day 9: MariaDB Troubleshooting

## Objective

Troubleshot and resolve why MariaDB is down.

## Steps

1. Connect to the Database using `SSH`: Refer to User and Server Details in the lab.

   ```sh
   ssh user@server-name
   ```

2. Check Maria DB Status.

   ```sh
   sudo systemctl status mariadb
   ```

3. If MariaDB is disabled Enable it.

   ```sh
   sudo systemctl enable MariaDB
   ```

4. Still wasn't able to start checked logs to troubleshot.

   ```sh
   sudo systemctl status mariadb -l --no-pager
   sudo journalctl -u mariadb -n 100 --no-pager
   sudo tail -100 /var/log/mariadb/mariadb.log
   ```

5. Based on the logs found out there was a file permission issue.

   ```sh
   [ERROR] mariadbd: Can't create/write to file '/run/mariadb/mariadb.pid'
   (Errcode: 13 "Permission denied")

   [ERROR] Can't start server: can't create PID file: Permission denied
   ```

6. Provided right user permissions to the file.

   First checked directory permission
   
   ```sh
   sudo ls -ld /run/mariadb
   ```

   Output:
   
   ```sh
   drwxr-xr-x 2 root mysql 40 Sep 14 09:29 /run/MariaDB
   ```
   
   Since MariaDB runs under the mysql user, this ownership prevented MariaDB from creating its PID file.

   Change Owner from root to MySQL.
   
   ```sh
   sudo chown mysql:mysql /run/MariaDB
   ```

7. Restart MariaDB.

   ```sh
   sudo systemctl restart MariaDB
   ```
   
   Then verify the MariaDB Status.
   
   ```sh
   sudo systemctl status MariaDB
   ```

## What I Learned

- MariaDB may initialize successfully but still fail during the final startup stage.
- Log analysis is critical for identifying the actual root cause.
- Errcode: 13 means Permission denied.
- Linux service users need appropriate ownership and permissions on runtime directories.
