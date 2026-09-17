# Day 3: Secure Root SSH Access

## Objective

To prevent SSH Login access on all app servers.

## Steps

1. Connect to the app server using `SSH`: Refer to User and Server Details in the lab.

   ```sh
   ssh user@server-name
   ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Edit the sshd_config file and change the value of PermitRootLogin to No.

   ```sh
   vim /etc/ssh/ssh_config
   ```

   `:?`: for search, PermitRootLogin within the file.
   `:wq`: save and quit the file.

## What I Learned

- SSH Security practices.
- To disable root access for security.
