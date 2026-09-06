# Day 2: Temporary User Setup with Expiry

## Objective

To create a user with a set expire date for your organization on a server. 

## Technologies Used

- SSH access
- Linux user management commands

## Steps

1. Connect to the app server using `SSH`: Refer to User and Server Details in the lab.

    ```sh
    ssh user@server-name
    ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Create user with expire date.

    ```sh
    useradd -e <date> <user-name>
    ```

    `-e`: for expiredate, to set expire date for new accounts.

3. To Verify the result you can either try to Cat or can try to login to that user.
    
    Using Cat:

    ```sh
    cat /etc/passwd
    ```

    Using chage:

    ```sh
    chage -l <user-name>
    ```

    `-l`: for list, to show account aging info.

## What I Learned

- To set expire for temporary accounts.
