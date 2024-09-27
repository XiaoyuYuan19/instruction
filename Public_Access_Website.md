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

    # HTTP server block for salacount.ddns.net (redirects HTTP to HTTPS)
    server {
        listen 80;
        server_name salacount.ddns.net;

        # Redirect all HTTP requests to HTTPS
        return 301 https://$host$request_uri;
    }

    # HTTPS server block for salacount.ddns.net
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

    # HTTP server block for aichar.ddns.net (redirects HTTP to HTTPS)
    server {
        listen 80;
        server_name aichar.ddns.net;

        # Redirect all HTTP requests to HTTPS
        return 301 https://$host$request_uri;
    }

    # HTTPS server block for aichar.ddns.net
    server {
        listen 443 ssl;
        server_name aichar.ddns.net;

        # SSL certificate and private key files (you will need to replace these paths with your own certificates)
        ssl_certificate "E:/Desktop/project/aichar/SSL/aichar.ddns.net-chain.pem";
        ssl_certificate_key "E:/Desktop/project/aichar/SSL/aichar.ddns.net-key.pem";

        # SSL protocol and ciphers
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Proxy requests to the Flask application running on 127.0.0.1:5000
        location / {
            proxy_pass http://127.0.0.1:5000;  # Forward requests to the Flask app
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # Custom error pages
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    # Additional server configurations can go here (e.g., other virtual hosts)
}

# Configure Nginx on Windows for Public Access to a Static Website with SSL and Dynamic DNS

## Introduction
This enhanced guide explains how to set up Nginx on a Windows machine to host a static website, configure SSL encryption, and set up Dynamic DNS (DDNS) for public access. We will walk you through the installation of Nginx, setting up port forwarding, configuring SSL encryption using `win-acme`, and implementing Dynamic DNS using No-IP to ensure your website remains accessible even with a dynamic public IP address.

## Requirements
- Windows operating system
- Nginx installed on Windows
- Access to your router to configure port forwarding
- No-IP client installed for dynamic DNS management
- Win-acme for SSL encryption (Let's Encrypt)
  
---

## Step 1: Install Nginx on Windows

### 1.1. Download Nginx:
- Go to the [Nginx official website](https://nginx.org/en/download.html) and download the latest stable version for Windows.

### 1.2. Extract Nginx:
- Unzip the downloaded file to a location on your system, such as `C:\nginx`.

### 1.3. Start Nginx:
- Open **Command Prompt**, navigate to the Nginx directory, and start Nginx with the following commands:
  ```bash
  cd C:\nginx\nginx-1.xx.x
  start nginx
  ```

### 1.4. Verify Installation:
- Open a browser and go to `http://localhost`. If you see the Nginx welcome page, the server is running successfully.

---

## Step 2: Configure Nginx to Host Your Website

### 2.1. Edit the Configuration:
- Navigate to the Nginx configuration file located in `C:\nginx\nginx-1.xx.x\conf\nginx.conf`.
- Open `nginx.conf` in a text editor (e.g., Notepad).

### 2.2. Modify the `nginx.conf` file:
- Locate the `server` block and modify it to point to the folder where your static website files are located:
  
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

- **root**: Set this to the folder where your website files (HTML, CSS, JavaScript) are located.
- **index**: Specify the default index file (usually `index.html`).

### 2.3. Save and Reload Nginx:
- After making changes to the `nginx.conf` file, save it.
- Reload Nginx by running the following command in **Command Prompt**:
  ```bash
  nginx -s reload
  ```

### 2.4. Verify the Website:
- Go to `http://localhost` in your browser to verify that your website is being served correctly.

---

## Step 3: Set Up Dynamic DNS with No-IP

Since your public IP address might change frequently, you can use No-IP to map a domain name to your dynamic IP address, ensuring constant access to your website.

### 3.1. Create a No-IP Account:
- Go to the [No-IP website](https://www.noip.com/) and create an account.
- Once registered, create a free dynamic DNS hostname (e.g., `yourname.ddns.net`).

### 3.2. Download the No-IP Dynamic Update Client:
- Download the [No-IP DUC client](https://www.noip.com/download) and install it on your Windows machine.

### 3.3. Configure No-IP Client:
- After installation, log in with your No-IP credentials and choose the hostname you created earlier.
- Ensure the client is running and correctly linked to your dynamic IP address.

### 3.4. Test the Dynamic DNS:
- Once configured, you should be able to access your local machine via the new hostname (e.g., `yourname.ddns.net`), though you will need to complete port forwarding for this to work externally.

---

## Step 4: Set Up Port Forwarding for Public Access

### 4.1. Find Your Local IP Address:
- Open **Command Prompt** and run the following command to find your local IP address:
  ```bash
  ipconfig
  ```
- Look for the **IPv4 address** of your active network connection (e.g., `192.168.1.100`).

### 4.2. Log into Your Router:
- Open your browser and enter your router’s IP address (commonly `192.168.1.1` or `192.168.0.1`).
- Log into your router’s admin interface.

### 4.3. Configure Port Forwarding:
- Go to the **Port Forwarding** or **Virtual Server** section in your router’s settings.
- Add a new rule with the following parameters:
  - **External Port**: 80 (for HTTP)
  - **Internal IP**: Your local IP address (e.g., `192.168.1.100`)
  - **Internal Port**: 80
  - **Protocol**: TCP
  - **Common Service Port**: Choose HTTP

- Repeat the same steps for port **443** (for HTTPS) when you configure SSL later.

### 4.4. Save Settings:
- Save the port forwarding rule in your router's configuration.

### 4.5. Get Your Public IP:
- Visit [WhatIsMyIP](https://www.whatismyip.com/) or search "What is my IP" on Google to find your public IP address.

### 4.6. Test Public Access:
- Now your website should be accessible from the internet using `http://<your-public-ip>` or `http://<yourname.ddns.net>`.

---

## Step 5: Configure Windows Firewall

### 5.1. Open Windows Firewall:
- Press `Win + R`, type `firewall.cpl`, and press Enter.

### 5.2. Allow HTTP Traffic:
- Click on **Advanced Settings** on the left sidebar.
- Select **Inbound Rules**, then click **New Rule** on the right sidebar.
- Choose **Port** as the rule type and click **Next**.
- Select **TCP** and enter **80** as the specific port.
- Click **Next**, and choose **Allow the connection**.
- Complete the rule setup and ensure that it is enabled.

### 5.3. Allow HTTPS Traffic:
- Repeat the above steps, but this time for **port 443** to allow HTTPS traffic through the firewall.

---

## Step 6: Configure SSL with Win-Acme

To enable HTTPS, we will use **win-acme** to obtain a free SSL certificate from **Let's Encrypt**.

### 6.1. Download and Install Win-Acme:
- Download the [win-acme client](https://www.win-acme.com/) and extract it to a folder on your system, e.g., `C:\win-acme`.

### 6.2. Run Win-Acme to Obtain an SSL Certificate:
- Open **Command Prompt**, navigate to the win-acme directory, and run the following command:
  ```bash
  wacs.exe
  ```
- Follow the interactive prompts:
  1. Select **N** (Create a new certificate with default settings).
  2. Choose **2** (Manual input) to enter your No-IP domain (e.g., `yourname.ddns.net`).
  3. Choose **2** (Serve verification files from memory) for HTTP-01 challenge verification.
  4. Select **2** (PEM encoded files) to export the certificate in a format suitable for Nginx.
  5. Specify a folder where the certificate and key should be saved, e.g., `C:\nginx\certs`.

### 6.3. Configure Nginx to Use SSL:
- Modify your Nginx configuration to include the SSL certificate and key paths generated by win-acme:
  
  ```nginx
  server {
      listen 443 ssl;
      server_name yourname.ddns.net;

      ssl_certificate "C:/nginx/certs/yourname.ddns.net-chain.pem";
      ssl_certificate_key "C:/nginx/certs/yourname.ddns.net-key.pem";

      ssl_protocols TLSv1.2 TLSv1.3;
      ssl_ciphers HIGH:!aNULL:!MD5;
      ssl_prefer_server_ciphers on;

      root C:/path/to/your/website;
      index index.html;
  }
  ```

### 6.4. Redirect HTTP to HTTPS:
- Add the following server block to automatically redirect HTTP traffic to HTTPS:
  
  ```nginx
  server {
      listen 80;
      server_name yourname.ddns.net;
      return 301 https://$host$request_uri;
  }
  ```

### 6.5. Reload Nginx:
- Reload Nginx to apply the changes:
  ```bash
  nginx -s reload
  ```

### 6.6. Test SSL:
- Visit `https://yourname.ddns.net` to verify that SSL is correctly configured and that your site is accessible via HTTPS.

---
## 7. 其他注意事项

### 7.1 IP绑定

路由器重启后，本机IP可能会发生变化，例如从192.168.1.103改变为192.168.1.100，导致路由器端口转发设置失效。
为了动态适应，在路由器设置页面找到“IP预留/IP与MAC绑定”功能，设置自己的物理地址和IP绑定。

查看物理地址的方法：在cmd中输入 ipconfig /all，找到物理地址。


---

## Conclusion

You have now successfully configured Nginx on Windows to serve a static website, enabled SSL encryption with Let's Encrypt, and used No-IP for dynamic DNS, ensuring your website is publicly accessible and secure.

**Security Note**: Always consider the security risks of exposing your local machine to the internet. Consider enabling additional security measures such as stronger firewall rules, fail2ban, or configuring

 Nginx for better security.
