# Day 8: Install Ansible

## Objective

Install ansible version 4.8.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

## Steps

1. Connect to the jump server using `SSH`: Refer to User and Server Details in the lab.

   ```sh
   ssh user@server-name
   ```

2. Install ansible.

   ```sh
   sudo pip3 install ansible==4.8.0
   ```

## What I Learned

- Learned how to install a specific version of Ansible using `pip3`.
- Learned how version pinning (`ansible==4.8.0`) ensures the required Ansible version is installed.
- Improved my understanding of Python package management with `pip3` for DevOps tools.
