# Day 019: Install and Configure Web Application

## Objective

To install and configure Apache HTTP Server on App Server 3 and host two static websites using separate URL paths on port `8085`.

## Task Requirements

- Install the `httpd` package and its dependencies on App Server 3.
- Configure Apache to listen on port `8085`.
- Host the `ecommerce` website at `http://localhost:8085/ecommerce/`.
- Host the `games` website at `http://localhost:8085/games/`.
- Verify both websites using the `curl` command.

## Steps

### 1. Connect to App Server 3

Connect to App Server 3 from the jump host:

```bash
ssh <username>@stapp03
```

### 2. Install Apache HTTP Server

Install the Apache HTTP Server package:

```bash
sudo yum install httpd -y
```

Verify the installation:

```bash
httpd -v
```

### 3. Configure Apache to Listen on Port 8085

Edit the Apache configuration file:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Change the listening port from `80` to `8085`:

```apache
Listen 8085
```

### 4. Create Website Directories

Create separate directories for both websites under Apache's default document root:

```bash
sudo mkdir -p /var/www/html/ecommerce
sudo mkdir -p /var/www/html/games
```

### 5. Copy Website Backups from the Jump Host

Copy the website backups from the jump host to App Server 3:

```bash
sudo scp -r /home/thor/ecommerce <username>@stapp03:/tmp/
sudo scp -r /home/thor/games <username>@stapp03:/tmp/
```

On App Server 3, copy the website contents into their respective Apache directories:

```bash
sudo cp -r /tmp/ecommerce/. /var/www/html/ecommerce/
sudo cp -r /tmp/games/. /var/www/html/games/
```

### 6. Set Website Ownership and Permissions

Set Apache as the owner of both website directories:

```bash
sudo chown -R apache:apache /var/www/html/ecommerce
sudo chown -R apache:apache /var/www/html/games
```

Set directory and file permissions:

```bash
sudo chmod -R 755 /var/www/html/ecommerce
sudo chmod -R 755 /var/www/html/games
```

### 7. Enable and Start Apache

Enable Apache to start automatically during system boot and restart the service:

```bash
sudo systemctl enable httpd
sudo systemctl restart httpd
sudo systemctl status httpd
```

### 8. Verify Both Websites

Test the ecommerce website:

```bash
curl http://localhost:8085/ecommerce/
```

Test the games website:

```bash
curl http://localhost:8085/games/
```

Both URLs should return the respective website content.

## What I Learned

- How to install and configure Apache HTTP Server on a Linux server.
- How to change Apache's listening port using the `Listen` directive.
- How Apache uses its `DocumentRoot` to locate website files.
- How to host multiple static websites using separate directories and URL paths.
- How to transfer website backups between servers using `scp`.
- How to configure file ownership and permissions for Apache.
- How to manage Apache services using `systemctl`.
- How to verify website availability and HTTP responses using `curl`.
