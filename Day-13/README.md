# Day 013: Secure Apache Port Using iptables

## Objective

Secure Apache on all app servers in the Stratos Datacenter by restricting access to TCP port `8083`. Allow connections only from the Load Balancer (LBR) host and block access from all other sources. Ensure firewall rules persist after a reboot.

## Task Requirements

- Install `iptables` and its required dependencies on each app server.
- Allow the LBR host to access TCP port `8083`.
- Block incoming traffic to port `8083` from all other sources.
- Persist firewall rules across system reboots.
- Verify that the website remains accessible through the Load Balancer.

## Steps

The following commands were executed as root on each app server.

### 1. Install iptables and dependencies

```bash
yum install -y iptables iptables-services
```

Installed the `iptables` firewall utilities and `iptables-services` for managing firewall rules through systemd.

### 2. Enable and start the iptables service

```bash
systemctl enable --now iptables
```

Enabled the firewall service to start automatically at boot and started it immediately.

### 3. Allow traffic from the Load Balancer

```bash
iptables -I INPUT 1 -p tcp -s 10.244.240.148 --dport 8083 -j ACCEPT
```

Inserted an `ACCEPT` rule at position 1 in the `INPUT` chain. This permits TCP traffic to port `8083` when the source IP is the Load Balancer.

### 4. Block traffic from other sources

```bash
iptables -I INPUT 2 -p tcp --dport 8083 -j DROP
```

Inserted a `DROP` rule at position 2 to silently discard other incoming TCP traffic destined for port `8083`.

The rule order is important: the LBR's `ACCEPT` rule must be evaluated before the general `DROP` rule.

### 5. Save the firewall rules

```bash
iptables-save | sudo tee /etc/sysconfig/iptables
```

Saved the current firewall rules to `/etc/sysconfig/iptables` so the `iptables-services` service can restore them after a reboot.

### 6. Enable iptables at boot

```bash
systemctl enable iptables
```

Ensured that the iptables service is enabled at system startup.

### 7. Verify the firewall rules

```bash
iptables -L INPUT -n --line-numbers
```

Checked the `INPUT` chain and confirmed the rule order, source IP, destination port, and actions.

### 8. Test through the Load Balancer

After applying the firewall rules on all app servers, switched to the Load Balancer host and tested the websites using `curl`.

Example:

```bash
curl http://stapp01:8083
curl http://stapp02:8083
curl http://stapp03:8083
```

Successful responses confirm that the application servers remain reachable from the Load Balancer.

## What I Learned

- How to install and manage `iptables` on Linux servers.
- How to use source IP addresses to allow traffic from a trusted Load Balancer.
- How to restrict access to a specific TCP port without disabling the firewall.
- Why firewall rule order matters when using `ACCEPT` and `DROP`.
- How to persist firewall rules using `iptables-save` and `iptables-services`.
- How to verify firewall rules with line numbers.
- How to validate application connectivity from the Load Balancer using `curl`.
