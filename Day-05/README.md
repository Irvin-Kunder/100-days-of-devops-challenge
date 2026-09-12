# Day 5: SElinux Installation and Configuration

## Objective

1. Install the required **SELinux** packages.

2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration.

3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

4. Disregard the current status of SELinux via the command line; the final status after the reboot should be **disabled**.

## Steps

1. Connect to the app server using `SSH`: Refer to User and Server Details in the lab.

    ```sh
    ssh user@server-name
    ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Run the update on the server.

    ```sh
    yum update
    ```

    Enter `Y` to install.

4. Install the Selinux packages.

    ```sh
    yum install selinux-policy selinux-policy-targeted policycoreutils policycoreutils-python-utils
    ```

5. Open the below file.

    ```sh
    vim /etc/selinux/config
    ```

    Change the value of SELINX from enforcing to disabled.

    ```sh
    SELINUX=disabled 
    ```

    Click esc and then save the file.

    ```sh
    :wq
    ```

## What I Learned

- Learned the fundamentals of SELinux, including its modes (Enforcing, Permissive, Disabled).
- Practiced installing SELinux packages and permanently disabling SELinux through configuration.
