# Day 1: Linux User Setup with Non-interactive Shell

## Objective

To create a user with non-interactive shell for your organization on a server.

## Technologies Used

- SSH access
- Linux user management commands

## Steps

1. Connect to the the app server using `SSH`: Refer to User and Server Details in the lab.

   ```sh
   ssh user@server-name
   ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Create non-interactive shell user.

   ```sh
   useradd -s sbin/nologin user-name
   ```

   `-s`: for shell, with path of nologin shell

4. To Verify the result you can either try to Cat or can try to login to that user.

   Using Cat:

   ```sh
   cat /etc/passwd
   ```

   Using login:

   ```sh
   su user-name
   ```

## What I Learned

- Non-interactive shells prevent direct user login
- Service accounts should use `/usr/sbin/nologin` or `/bin/false`
- Always verify user creation with multiple methods
- Understanding user shells is crucial for system security
