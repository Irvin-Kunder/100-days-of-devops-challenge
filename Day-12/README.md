# Day 11: Linux Network Services

## Objective

Our monitoring tool has reported an issue in `Stratos Datacenter`. One of our app servers has an issue, as its Apache service is not reachable on port `5001` (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp01:5001` command from jump host.
`Note:` Please do not try to alter the existing `index.html` code, as it will lead to task failure.

## Task Requirements

a. Troubleshoot the Apache HTTP Server on stapp01.

b. Ensure Apache is reachable on port 5001.

c. Identify and resolve any service, port-conflict, or firewall issues.

d. Make Apache accessible from the jump host.

## Steps

1. Login to App Server 1

2. Check httpd status.

   ```bash
   sudo systemctl status httpd
   ```

   Httpd status failed and disabled.

3. Inspect Apache's listening port

   ```bash
   sudo grep -R "^[[:space:]]*Listen" /etc/httpd/conf /etc/httpd/conf.d/
   ```

   Confirmed that Apache was configured to listen on port 5001.

4. Identify the port conflict

   ```bash
   sudo ss -ltnp | grep ':5001'
   ```

   The output showed Sendmail listening on 127.0.0.1:5001.

5. Back up the Sendmail configuration

   ```bash
   cd /etc/mail
   cp sendmail.mc sendmail.mc.bak
   ```

   Created a backup before changing the configuration.

   ```bash
   vi sendmail.mc
   ```

   Edited sendmail.mc to change Sendmail's port so Apache could use port 5001.

6. Restart Sendmail

   ```bash
   sudo systemctl restart sendmail
   ```

   Restarted Sendmail to apply the configuration change.

7. Inspect iptables rules and Allow incoming TCP traffic

   ```bash
   sudo iptables -L -n
   ```

   Reviewed the existing firewall rules to identify the relevant traffic restrictions.

   ```bash
   sudo iptables -I INPUT 4 -p tcp --dport 5001 -j ACCEPT
   ```

8. Restart and verify Apache

   ```bash
   sudo systemctl restart httpd
   sudo systemctl status httpd --no-pager
   sudo ss -ltnp | grep ':5001'
   ```

   Confirmed that Apache was listening on port 5001.

9. Test from the jump host

   Tested the application using `curl`:

   ```bash
   curl http://stapp01:5001
   ```

   The application was successfully accessible.

## What I Learned

- How to troubleshoot Apache startup failures caused by port conflicts.
- How to identify processes using listening ports with ss.
- How to back up and edit Sendmail configuration safely.
- How to inspect and modify Linux firewall rules using iptables.
- Why the firewall destination port must match the service's actual listening port.
- How to verify service availability from the jump host using curl.
- Why existing application files such as index.html should remain unchanged when not required by the task.
