# Day 016: Nginx Load Balancer Configuration

## Objective

Configure Nginx on the Load Balancer (LBR) server `stlb01` to distribute incoming HTTP traffic across all Nautilus application servers in the Stratos Data Center, improving website availability and handling increased traffic.

## Task Requirements

- Install Nginx on the LBR server if it is not already installed.
- Configure load balancing inside the `http` context in `/etc/nginx/nginx.conf`.
- Include all application servers in the upstream configuration.
- Preserve the existing Apache port `3000` on all app servers.
- Ensure Apache and Nginx services are running.
- Verify website accessibility using `curl http://stlb01:80`.

## Steps

### 1. Install Nginx

Installed Nginx on the LBR server using the package manager.

```bash
yum install nginx -y
```

### 2. Enable and Start Nginx

Enabled Nginx to start automatically at boot and started the service.

```bash
systemctl enable nginx
systemctl start nginx
```

Verified the service status:

```bash
systemctl status nginx
```

### 3. Configure Nginx Load Balancing

Edited the main Nginx configuration file.

```bash
vi /etc/nginx/nginx.conf
```

Configured an upstream group containing all three application servers, using their existing Apache port `3000`.

```nginx
http {
    upstream nautilus_app {
        server stapp01:3000;
        server stapp02:3000;
        server stapp03:3000;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://nautilus_app;
        }
    }
}
```

**Note:** The configuration above illustrates the required directives. The upstream and server blocks must be added inside the existing `http` context, preserving other required settings in the main configuration file.

Nginx distributes incoming requests across the configured upstream servers using round-robin load balancing by default.

### 4. Validate the Nginx Configuration

Tested the configuration for syntax errors.

```bash
nginx -t
```

Restarted Nginx to apply the configuration.

```bash
systemctl restart nginx
```

Verified the service status again.

```bash
systemctl status nginx
```

### 5. Test Website Accessibility

Executed the following command to access the website through the load balancer.

```bash
curl http://stlb01:80
```

A successful HTTP response confirms that the website is accessible through the load balancer. Backend access logs can be checked to identify which application server handled a request.

## What I Learned

- Installing and managing the Nginx service on Linux.
- Configuring an Nginx upstream group for multiple application servers.
- Using reverse proxying to forward HTTP requests to backend Apache servers.
- Preserving existing backend service ports during load balancer configuration.
- Validating Nginx configuration syntax with `nginx -t`.
- Testing website accessibility using `curl`.
- Understanding how round-robin load balancing distributes incoming requests across backend servers.
