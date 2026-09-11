# Day 7: Linux SSH Authentication

## Objective

Set up passwordless authentication from user `thor` on the jump host to all app servers through their respective sudo users.

## Steps

1. Connect to the jump server using `SSH`: Refer to User and Server Details in the lab.

    ```sh
    ssh user@server-name
    ```

2. Generate a public and private SSH key pair.

    ```sh
    ssh-keygen -t rsa -b 2048
    ```

    Press `Enter` to accept the default file location and follow the prompts.

    Copy the public key.

3. Log in to all app servers using the SSH command from Step 1. Then create the `.ssh` directory and `authorized_keys` file.

    ```sh
    mkdir -p ~/.ssh
    vi ~/.ssh/authorized_keys
    ```

    Paste the public key into `authorized_keys` and save the file.

4. Test the passwordless SSH connection from the jump server.

    ```sh
    ssh user@server-name
    ```

## What I Learned

- Learned how to configure passwordless SSH authentication using SSH key-based authentication.
- Learned the difference between public and private SSH keys and how they are used for authentication.
- Learned how to manually configure the `authorized_keys` file to allow SSH key-based authentication.
- Learned that `ssh-copy-id` can simplify the process of copying a public SSH key to a remote server.
- Improved my understanding of secure server-to-server authentication in a Linux environment.
