# Day 014: Troubleshoot Apache Service Failure\*\*

## Objective

Troubleshoot the Apache HTTP Server startup failure and ensure Apache is running on port `6200` on all application servers.

## Task Requirements

- Identify the application server where Apache is failing.
- Resolve the port conflict preventing Apache from starting.
- Ensure Apache listens on port `6200`.
- Enable Apache to start automatically at boot.
- Verify Apache on all application servers.

## Steps

### 1. Check Apache Service Status

```bash
sudo systemctl status httpd --no-pager
```

Check whether Apache is running or has failed to start.

### 2. Identify the Port Conflict

```bash
sudo ss -tlnup
```

Identify the process listening on port `6200`. Sendmail was occupying `127.0.0.1:6200`, preventing Apache from binding to the required port.

### 3. Back Up the Sendmail Configuration

```bash
sudo cp -p /etc/mail/sendmail.mc /etc/mail/sendmail.mc.bak
```

Create a backup before modifying the configuration.

### 4. Change the Sendmail Listening Port

```bash
sudo vi /etc/mail/sendmail.mc
```

Change the original configuration:

```m4
DAEMON_OPTIONS(`Port=6200,Addr=127.0.0.1, Name=MTA')dnl
```

To:

```m4
DAEMON_OPTIONS(`Port=25,Addr=127.0.0.1, Name=MTA')dnl
```

This frees port `6200` for Apache.

### 5. Regenerate the Sendmail Configuration

```bash
sudo make -C /etc/mail
```

Run the build instructions in `/etc/mail` to regenerate the generated Sendmail configuration.

### 6. Restart Sendmail

```bash
sudo systemctl restart sendmail
```

Apply the updated configuration by restarting Sendmail.

### 7. Start and Enable Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Start Apache and configure it to start automatically at boot.

### 8. Verify Apache and Its Listening Port

```bash
sudo systemctl status httpd --no-pager
sudo ss -lntp | grep ':6200'
```

Confirm that Apache is active and listening on port `6200`.

### 9. Test HTTP Connectivity

From the jump host, run:

```bash
curl -I http://stapp01:6200
```

Check the HTTP response headers to confirm that the server responds to the request.

### 10. Verify the Remaining Application Servers

Repeat the service-status and listening-port checks on `stapp02` and `stapp03` to confirm that Apache is running on port `6200` on all application servers.

## What I Learned

- Troubleshooting service startup failures using `systemctl`.
- Identifying port conflicts using `ss`.
- Understanding how IP addresses and ports affect service binding.
- Modifying `sendmail.mc` and regenerating the generated configuration.
- Applying configuration changes by restarting services.
- Enabling services to start automatically at boot.
- Verifying service health and HTTP connectivity.
