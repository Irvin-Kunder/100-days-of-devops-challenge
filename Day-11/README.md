# Day 11: Deploy Java Application on Tomcat

## Objective

Install and configure the **Tomcat application server** on App Server 2 in the Stratos Datacenter and deploy the provided `ROOT.war` Java application.

## Task Requirements

a. Install `tomcat` server on `App Server 2`.

b. Configure it to run on port `5002`.

c. There is a `ROOT.war` file on `Jump host` at location `/tmp`.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp02:5002`

## Steps

1. Install Tomcat.

   Installed Tomcat on App Server 2:

   ```bash
   sudo yum install tomcat -y
   ```

2. Check Tomcat Configuration.

   Checked the configured ports in the Tomcat `server.xml` file:

   ```bash
   cat /etc/tomcat/server.xml | grep -i 'port'
   ```

3. Configure Tomcat Port.

   Opened the Tomcat configuration file:

   ```bash
   sudo vi /etc/tomcat/server.xml
   ```

   Changed the HTTP connector port from the default `8080` to:

   ```text
   5002
   ```

4. Enable Tomcat.

   Configured Tomcat to start automatically during system boot:

   ```bash
   sudo systemctl enable tomcat
   ```

   Checked the service:

   ```bash
   sudo systemctl status tomcat
   ```

5. Start Tomcat.

   Started the Tomcat service:

   ```bash
   sudo systemctl start tomcat
   ```

   Verified the service status:

   ```bash
   sudo systemctl status tomcat
   ```

6. Copy ROOT.war to App Server 2.

   The `ROOT.war` file was available on the Jump Host under:

   From Jump Server Copied the WAR file to App Server 2:

   ```bash
   scp /tmp/ROOT.war steve@stapp02:/tmp/
   ```

7. Deploy the WAR File.

   Moved the WAR file into Tomcat's web applications directory:

   ```bash
   sudo mv /tmp/ROOT.war /var/lib/tomcat/webapps/
   ```

   Verified the file is moved:

   ```bash
   ls -lrt /var/lib/tomcat/webapps/
   ```

   Because the application was deployed as `ROOT.war`, Tomcat serves it directly from the base URL.

8. Restart Tomcat.

   Restarted Tomcat to load the deployed application:

   ```bash
   sudo systemctl restart tomcat
   ```

9. Test the Application.

   Tested the application using `curl`:

   ```bash
   curl http://stapp02:5002
   ```

   The application was successfully accessible through:

   ```text
   http://stapp02:5002
   ```

## What I Learned

- Learned how to install and manage **Tomcat** on a Linux server.
- Learned how to change the Tomcat HTTP connector port using `server.xml`.
- Learned how to use `systemctl` to enable, start, stop, restart, and check the status of services.
- Learned how to transfer files between Linux servers using `scp`.
- Learned how to deploy a Java `.war` application on Tomcat.
- Learned that deploying an application as `ROOT.war` makes it accessible directly from the Tomcat base URL.
- Learned how to verify a web application from the command line using `curl`.
