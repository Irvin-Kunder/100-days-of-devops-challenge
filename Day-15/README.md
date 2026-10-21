# **Day 015: Setup SSL for Nginx**

## Objective

Install and configure Nginx on App Server 2, enable HTTPS using the provided self-signed SSL certificate, and serve a welcome page.

## Task Requirements

- Install Nginx on App Server 2 (`stapp02`).
- Store the provided SSL certificate and private key.
- Configure Nginx to serve HTTPS on port `443`.
- Create an `index.html` file containing `Welcome!`.
- Verify HTTPS access from the jump host using the server hostname.

## Steps

### 1. Install Nginx

Install Nginx on App Server 2.

```bash
sudo yum install nginx -y
```

### 2. Create a Directory for SSL Certificates

Create a dedicated directory for the SSL certificate and private key.

```bash
sudo mkdir -p /etc/certs
```

### 3. Copy the Certificate and Private Key

Copy the provided certificate and key from `/tmp`.

```bash
sudo cp /tmp/nautilus.* /etc/certs/
```

Verify the files.

```bash
ls -l /etc/certs/
```

The directory should contain `nautilus.crt` and `nautilus.key`.

### 4. Configure Nginx for HTTPS

Edit the Nginx configuration file.

```bash
sudo vi /etc/nginx/nginx.conf
```

Configure the HTTPS server block to listen on port `443` and use the certificate and private key.

```nginx
listen 443 ssl;
ssl_certificate /etc/certs/nautilus.crt;
ssl_certificate_key /etc/certs/nautilus.key;
```

Ensure the server block serves content from `/usr/share/nginx/html`.

### 5. Create the Welcome Page

Create the HTML file in the Nginx document root.

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

Verify the contents.

```bash
cat /usr/share/nginx/html/index.html
```

Expected output:

```text
Welcome!
```

### 6. Test the Nginx Configuration

Validate the configuration before restarting Nginx.

```bash
sudo nginx -t
```

A successful test confirms that the configuration syntax is valid and the required files can be loaded.

### 7. Restart and Verify Nginx

Restart Nginx to apply the changes.

```bash
sudo systemctl restart nginx
```

Check the service status.

```bash
sudo systemctl status nginx --no-pager
```

### 8. Verify HTTPS Connectivity

From the jump host, test HTTPS access using the server hostname.

```bash
curl -Ik https://stapp02/
```

The `HTTP/2 200` response confirmed successful HTTPS access.

### 9. Verify the Welcome Page

Run the following command from the jump host.

```bash
curl -k https://stapp02/
```

Expected output:

```text
Welcome!
```

The `-k` option allows `curl` to accept the self-signed certificate without validating its trust chain.

## What I Learned

- How to install and configure Nginx on Linux.
- How to store SSL certificates and private keys in a dedicated directory.
- How to configure Nginx to serve HTTPS using an SSL certificate.
- How to validate Nginx configuration using `nginx -t`.
- How to restart and verify services using `systemctl`.
- How to test HTTPS connectivity and page content using `curl`.
- Why self-signed certificates require special consideration during certificate validation.
