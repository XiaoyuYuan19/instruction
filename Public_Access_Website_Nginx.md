#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    # HTTP server block (for redirecting HTTP to HTTPS)
    server {
        listen 80;
        server_name salacount.ddns.net;

        # Redirect all HTTP requests to HTTPS
        return 301 https://$host$request_uri;
    }

    # HTTPS server block
    server {
        listen 443 ssl;
        server_name salacount.ddns.net;

        # SSL certificate and private key files
        ssl_certificate "E:/Desktop/project/salary/SSL/salacount.ddns.net-chain.pem";
        ssl_certificate_key "E:/Desktop/project/salary/SSL/salacount.ddns.net-key.pem";

        # SSL protocol and ciphers
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Website root directory and index file
        root E:/Desktop/project/salary;
        index index.html;

        # Custom error pages
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    # Additional server configurations can go here (e.g., other virtual hosts)
}



# Configure Nginx on Windows for Public Access to a Static Website

## Introduction

This guide explains how to set up Nginx on a Windows machine to host a static website and configure port forwarding for public access. It walks you through the installation of Nginx, basic configuration, and setting up port forwarding so that the site can be accessed from the internet.

## Requirements
- Windows operating system
- Nginx installed on Windows
- Access to your router to configure port forwarding

## Step 1: Install Nginx on Windows

1. **Download Nginx**:
   - Go to [Nginx official website](https://nginx.org/en/download.html) and download the latest stable version for Windows.

2. **Extract Nginx**:
   - Unzip the downloaded file to a location on your system (e.g., `C:\nginx`).

3. **Start Nginx**:
   - Open **Command Prompt**, navigate to the Nginx directory, and start Nginx by running the following commands:

     ```bash
     cd C:\nginx\nginx-1.xx.x
     start nginx
     ```

4. **Verify Installation**:
   - Open a browser and go to `http://localhost`. If you see the Nginx welcome page, the server is running successfully.

## Step 2: Configure Nginx to Host Your Website

1. **Edit the Configuration**:
   - Go to the Nginx configuration file located in `C:\nginx\nginx-1.xx.x\conf\nginx.conf`.
   - Open `nginx.conf` in a text editor (e.g., Notepad).

2. **Modify the `nginx.conf` file**:
   - Locate the `server` block and change the root directory to point to the folder where your static website files are located:

     ```nginx
     server {
         listen       80;
         server_name  localhost;

         location / {
             root   C:/path/to/your/website;
             index  index.html index.htm;
         }
     }
     ```

   - `root`: Set this to the folder where your website files (HTML, CSS, JavaScript) are located.
   - `index`: Specify the default index file (usually `index.html`).

3. **Save and Reload Nginx**:
   - After making changes to the `nginx.conf` file, save it.
   - Reload Nginx by running the following command in **Command Prompt**:

     ```bash
     nginx -s reload
     ```

4. **Verify the Website**:
   - Go to `http://localhost` in your browser to verify that your website is now being served.

## Step 3: Set Up Port Forwarding for Public Access

1. **Find Your Local IP Address**:
   - Open **Command Prompt** and run the following command to find your local IP address:

     ```bash
     ipconfig
     ```

   - Look for the IPv4 address of your active network connection (e.g., `192.168.1.100`).

2. **Log into Your Router**:
   - Open your browser and enter your router’s IP address (commonly `192.168.1.1` or `192.168.0.1`).
   - Log into your router’s admin interface.

3. **Configure Port Forwarding**:
   - Go to the **Port Forwarding** or **Virtual Server** section in your router’s settings.
   - Add a new rule with the following parameters:
     - **External Port**: 80
     - **Internal IP**: Your local IP address (e.g., `192.168.1.100`)
     - **Internal Port**: 80
     - **Protocol**: TCP
     - **Common Service Port**: Choose **HTTP**

4. **Save Settings**:
   - Save the port forwarding rule in your router's configuration.

5. **Get Your Public IP**:
   - Visit [WhatIsMyIP](https://www.whatismyip.com/) or search "What is my IP" on Google to find your public IP address.
   
6. **Test Public Access**:
   - Now your website should be accessible from the internet using `http://<your-public-ip>`.

## Step 4: Configure Windows Firewall

1. **Open Windows Firewall**:
   - Press `Win + R`, type `firewall.cpl`, and press Enter.

2. **Allow HTTP Traffic**:
   - Click on **Advanced Settings** on the left sidebar.
   - Select **Inbound Rules**, then click **New Rule** on the right sidebar.
   - Choose **Port** as the rule type and click **Next**.
   - Select **TCP** and enter `80` as the specific port.
   - Click **Next**, and choose **Allow the connection**.
   - Complete the rule setup and ensure that it is enabled.

## Step 5: (Optional) Set Up Dynamic DNS

If your public IP address is dynamic and changes frequently, consider using a Dynamic DNS (DDNS) service like [No-IP](https://www.noip.com/) or [DynDNS](https://dyn.com/dns/) to link a domain name to your changing public IP address. This will ensure your website is always accessible via a consistent URL.

## Conclusion

You have successfully installed and configured Nginx on Windows to serve a static website and made it publicly accessible using port forwarding. If you have further needs such as enabling HTTPS or configuring additional security settings, consider setting up SSL certificates or firewall rules for enhanced protection.

---

### Notes

- Always consider the security risks of exposing your local machine to the internet. If your site contains sensitive data or handles user interactions, consider implementing HTTPS using SSL certificates and securing your server further.
- If using a dynamic IP, a DDNS solution can simplify access.
