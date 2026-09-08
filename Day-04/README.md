# Day 4: Script Execution Permissions

## Objective

To grant executable permission to script file. Ensure all users can execute.

## Steps

1. Connect to the app server using `SSH`: Refer to User and Server Details in the lab.

    ```sh
    ssh user@server-name
    ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Check file permission.

    ```sh
    ls -al /tmp
    ```

    `-a`: all files including hidden files.
    `-l`: to long list files details.

4. Change the file permissions.

    ```sh
    chmod +rx <file_name>
    ```sh

    `+r': to provide read permissions.
    `+x': to provide write permissions.

## What I Learned

- To provide permissions to the file.
