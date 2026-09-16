# Day 10: Bash Script for Website Media Archiving

## Objective

Create a Bash script to automate the process of archiving website media files from App Server 2 and transferring the archive to the Nautilus Storage Server for long-term validation and retrieval.

## Task Requirements

The script should:

1. Create a ZIP archive of `/var/www/html/media`.
2. Name the archive `xfusioncorp_media.zip`.
3. Store the archive temporarily in `/archives/` on App Server 2.
4. Copy the archive to `/archives/` on the Nautilus Storage Server.
5. Use passwordless SSH authentication for the file transfer.
6. Allow the respective server user to execute the script.
7. Not use `sudo` inside the script.

The script must be located at:

```text
/scripts/media_archive.sh
```

## Steps

1. Connect to the app 2 using `SSH`: Refer to User and Server Details in the lab.

    ```sh
    ssh user@server-name
    ```

2. Create the Bash Script.

    ```sh
    vi /scripts/media_archive.sh
    ```

Script:

```bash
#!/bin/bash

SOURCE="/var/www/html/media"
ARCHIVE="/archives/xfusioncorp_media.zip"
REMOTE_USER="<storage-user>"
REMOTE_HOST="<storage-server>"
REMOTE_PATH="/archives/xfusioncorp_media.zip"

zip -r "$ARCHIVE" "$SOURCE"

scp "$ARCHIVE" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}"
```

> Replace `<storage-user>` and `<storage-server>` with the actual values provided by the lab.


3. Install Zip.

   ```sh
   sudo yum install zip -y
   ```

```bash
zip -v
```

4. Generate an SSH key

```bash
ssh-keygen -t rsa -b 2048
```

Copy the public key to the storage server:

```bash
ssh-copy-id <storage-user>@<storage-server>
```


5. Make the Script Executable

```bash
chmod +x /scripts/media_archive.sh
```

Verify:

```bash
ls -l /scripts/media_archive.sh
```

6. Execute the Script

Run the script using the appropriate server user:

```bash
/scripts/media_archive.sh
```

```bash
ls -lh /archives/xfusioncorp_media.zip
```

7. ## Verification

Check that the archive was created on App Server 2:

```bash
ls -lh /archives/xfusioncorp_media.zip
```

Verify the archive on the Nautilus Storage Server:

```bash
ssh <storage-user>@<storage-server> "ls -lh /archives/xfusioncorp_media.zip"
```

## What I Learned

- Learned how to automate repetitive file-archiving tasks using Bash.
- Learned how to create ZIP archives using the `zip` command.
- Learned how to transfer files between Linux servers using `scp`.
- Learned how SSH key-based authentication enables passwordless file transfers.
- Learned how to use variables in Bash scripts to make commands easier to maintain and reuse.
- Practiced separating package installation from application/script logic.
