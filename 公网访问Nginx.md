# 使用 Nginx 在 Windows 上配置并通过公网访问静态网站
## Introduction | 介绍

This tutorial will guide you through setting up Nginx on a Windows machine to host a static website and configure port forwarding for public access.  
本教程将引导你在 Windows 电脑上安装 Nginx，托管静态网站，并通过端口转发配置实现公网访问。

## Requirements | 所需条件
- Windows operating system  
  Windows 操作系统
- Nginx installed on Windows  
  Windows 上安装 Nginx
- Access to your router for port forwarding setup  
  能访问路由器进行端口转发配置

## Step 1: Install Nginx on Windows | 第一步：在 Windows 上安装 Nginx

1. Download the Nginx stable version for Windows from [Nginx official website](https://nginx.org/en/download.html).  
   从 [Nginx 官方网站](https://nginx.org/en/download.html) 下载 Nginx 稳定版 Windows 版本。

2. Unzip the downloaded file to a location of your choice (e.g., `C:\nginx`).  
   将下载的文件解压到你希望放置的目录（例如 `C:\nginx`）。

3. Open **Command Prompt** and navigate to the Nginx directory, then start Nginx by running:  
   打开 **命令提示符** 并进入 Nginx 目录，运行以下命令启动 Nginx：

   ```bash
   cd C:\nginx\nginx-1.xx.x
   start nginx
   ```

4. Open a browser and visit `http://localhost` to verify that Nginx is running. You should see the Nginx welcome page.  
   打开浏览器，访问 `http://localhost`，如果看到 Nginx 欢迎页面，说明 Nginx 已成功启动。

## Step 2: Configure Nginx to Host Your Website | 第二步：配置 Nginx 托管你的网站

1. Locate and open the `nginx.conf` file in the `C:\nginx\nginx-1.xx.x\conf` directory using Notepad or any text editor.  
   在 `C:\nginx\nginx-1.xx.x\conf` 目录下找到并打开 `nginx.conf` 文件，用记事本或其他文本编辑器编辑。

2. Modify the configuration to point to your static website folder:

   修改配置文件，指向你存放静态网站的文件夹：
   
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

   - `root` should be set to the directory where your website files (HTML, CSS, JavaScript) are located.  
     `root` 需设置为你的静态网页文件所在的目录（HTML、CSS、JavaScript）。

   - `index` specifies the default file (e.g., `index.html`).  
     `index` 指定默认文件（例如 `index.html`）。

3. Save the changes and reload Nginx with the following command:  
   保存修改，并通过以下命令重新加载 Nginx 配置：

   ```bash
   nginx -s reload
   ```

4. Test again by visiting `http://localhost`. You should now see your website.  
   再次访问 `http://localhost`，现在应该能看到你的网站了。

## Step 3: Set Up Port Forwarding for Public Access | 第三步：设置端口转发以实现公网访问

1. Find your local IP address by running the following command in **Command Prompt**:  
   在 **命令提示符** 中运行以下命令，查找你的本地 IP 地址：

   ```bash
   ipconfig
   ```

   Look for your IPv4 address under the active network adapter (e.g., `192.168.1.100`).  
   查找当前活动的网络适配器下的 IPv4 地址（例如 `192.168.1.100`）。

2. Log in to your router’s admin interface by typing the router IP (often `192.168.1.1` or `192.168.0.1`) into your browser.  
   在浏览器中输入路由器的 IP 地址（通常是 `192.168.1.1` 或 `192.168.0.1`）以登录路由器管理界面。

3. Locate the **Port Forwarding** or **Virtual Server** section in your router’s settings.  
   在路由器设置中找到 **端口转发** 或 **虚拟服务器** 选项。

4. Add a new port forwarding rule with the following settings:  
   添加一个新的端口转发规则，配置如下：

   - **Service Port/External Port**: 80  
     **服务端口/外部端口**：80
   - **Internal IP**: Your local IP address (e.g., `192.168.1.100`)  
     **内部 IP**：你的本地 IP 地址（例如 `192.168.1.100`）
   - **Internal Port**: 80  
     **内部端口**：80
   - **Protocol**: TCP  
     **协议**：TCP
   - **Common Service Port**: Choose **HTTP**  
     **常用服务端口号**：选择 **HTTP**

5. Save the settings.  
   保存设置。

6. Find your public IP by visiting [WhatIsMyIP](https://www.whatismyip.com/) or searching "What is my IP" on Google.  
   访问 [WhatIsMyIP](https://www.whatismyip.com/) 或在谷歌搜索“我的 IP”，查找你的公网 IP。

7. Now, your website should be accessible via `http://<your-public-ip>`.  
   现在，你的网站可以通过 `http://<你的公网 IP>` 访问了。

## Step 4: Configure Firewall to Allow Traffic | 第四步：配置防火墙允许流量通过

1. Open Windows Firewall settings by pressing `Win + R` and typing `firewall.cpl`.  
   按 `Win + R`，输入 `firewall.cpl` 打开 Windows 防火墙设置。

2. Go to **Advanced Settings** on the left.  
   点击左侧的 **高级设置**。

3. Under **Inbound Rules**, create a new rule to allow TCP traffic on port 80:  
   在 **入站规则** 下，创建一条新规则，允许端口 80 上的 TCP 流量：

   - Rule Type: **Port**  
     规则类型：**端口**
   - Protocol: **TCP**  
     协议：**TCP**
   - Specific Port: **80**  
     指定端口：**80**
   - Action: **Allow the connection**  
     动作：**允许连接**

4. Complete the rule setup and ensure it is enabled.  
   完成规则设置，并确保该规则已启用。

## Step 5: (Optional) Set Up Dynamic DNS | 第五步：（可选）设置动态 DNS

If your public IP changes frequently (dynamic IP), you can set up Dynamic DNS (DDNS) using services like No-IP or DynDNS to associate a domain name with your changing IP.  
如果你的公网 IP 经常变化（动态 IP），可以使用 No-IP 或 DynDNS 等服务设置动态 DNS，将域名与变化的 IP 地址关联。

## Conclusion | 总结

You have now successfully configured Nginx to serve a static website on Windows and made it publicly accessible through port forwarding.  
现在你已经成功配置了 Nginx 在 Windows 上托管静态网站，并通过端口转发实现公网访问。

---

### Notes | 注意事项
- Ensure your site is secure if exposed to the public, especially if it contains sensitive information.  
  如果你的网站暴露在公网中，尤其是包含敏感信息时，请确保网站的安全性。
- Consider setting up HTTPS using SSL certificates for encryption.  
  考虑使用 SSL 证书设置 HTTPS 加密通信。
