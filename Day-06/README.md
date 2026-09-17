# Day 6: Create a Cron Job

## Objective

a. Install the `cronie` package on all app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

## Steps

1. Connect to all app server using `SSH`: Refer to User and Server Details in the lab.

   ```sh
   ssh user@server-name
   ```

2. Once connected Switch to Root.

   ```sh
   sudo su
   ```

3. Install `cronie` package.

   ```sh
   yum install cronie -y
   ```

4. Start crond service.

   ```sh
   systemctl enable crond
   systemctl start crond
   systemctl status crond
   ```

5. Create cron schedule.

   ```sh
   crontab -e
   */5 * * * * echo hello > /tmp/cron_text
   ```

   `-e:` Edit current user crontab.

6. Verify crontab.

   ```sh
   crontab -l
   ```

   `-l:` list current user crontab.

## What I Learned

- Learned how to install and configure Cron on Linux using the cronie package and crond service.
- Used `*/5 * * * *` to schedule a task to run every 5 minutes for the root user.
- Used Crontab.guru to create and understand cron schedule expressions.
