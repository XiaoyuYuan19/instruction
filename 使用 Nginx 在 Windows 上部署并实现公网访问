# README: 使用 Nginx 在 Windows 上部署并实现公网访问 (Deploy Nginx on Windows and Enable Public Access)

## 1. 前提条件 (Prerequisites)
- **操作系统 (Operating System)**: Windows 7/8/10/11
- **Nginx**: 最新的 Nginx 版本
- **公网 IP (Public IP)**: 用于在互联网上访问你的网站
- **路由器 (Router)**: 支持端口转发（Port Forwarding）

---

## 2. 步骤 1: 安装 Nginx (Step 1: Install Nginx)

1. **下载 Nginx (Download Nginx)**:
   - 访问 [Nginx 官方下载页面](https://nginx.org/en/download.html)。
   - 下载最新的 Windows 版本（例如 `nginx-1.xx.x.zip`）。
   
2. **解压并安装 (Extract and Install)**:
   - 将下载的 `.zip` 文件解压到一个目录中（例如 `C:\nginx`）。
   
3. **启动 Nginx (Start Nginx)**:
   - 打开命令提示符，导航到 Nginx 安装目录（例如 `C:\nginx\nginx-1.xx.x`）。
   - 输入以下命令启动 Nginx：
   
     ```bash
     start nginx
     ```
   
4. **测试 Nginx (Test Nginx)**:
   - 打开浏览器并输入 `http://localhost`。如果你看到 Nginx 的欢迎页面，说明 Nginx 已经成功启动。

---

## 3. 步骤 2: 配置 Nginx (Step 2: Configure Nginx)

1. **打开 Nginx 配置文件 (Open Nginx Configuration File)**:
   - 导航到 Nginx 安装目录，找到 `conf/nginx.conf` 文件，并用文本编辑器打开。

2. **修改配置 (Modify Configuration)**:
   - 找到 `server` 块并进行如下修改，将 `root` 设置为你的网站文件路径：

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
     - `root`：将路径替换为你静态网站所在的实际目录（例如 `C:/Users/你的用户名/Desktop/my_website`）。
     - `index`：确保主页文件为 `index.html`。

3. **保存并重新加载 Nginx (Save and Reload Nginx)**:
   - 保存修改后的 `nginx.conf` 文件。
   - 在命令提示符中输入以下命令，重新加载 Nginx 配置：
   
     ```bash
     nginx -s reload
     ```

4. **测试本地访问 (Test Local Access)**:
   - 在浏览器中再次访问 `http://localhost`，此时应显示你的网站内容。

---

## 4. 步骤 3: 设置公网访问 (Step 3: Set Up Public Access)

### 1. 获取本地 IP 地址 (Get Local IP Address)

1. 打开命令提示符并运行以下命令来查看你的本地 IP 地址：

   ```bash
   ipconfig
   ```

2. 找到你的网络适配器，记下 IPv4 地址（例如 `192.168.1.100`）。

### 2. 设置路由器端口转发 (Set Up Port Forwarding on Router)

1. **登录路由器管理界面 (Log in to Router Management Interface)**:
   - 打开浏览器，输入路由器 IP 地址（通常是 `192.168.1.1` 或 `192.168.0.1`），然后使用管理员凭据登录。

2. **找到“端口转发”选项 (Find Port Forwarding Option)**:
   - 在路由器管理界面中，找到“端口转发”或“虚拟服务器”选项。

3. **添加端口转发规则 (Add Port Forwarding Rule)**:
   - 添加一条新的规则：
     - **常用服务端口号 (Service Type)**：选择 **HTTP**（用于端口 80）。
     - **本地 IP 地址 (Local IP Address)**：输入你的本机局域网 IP 地址（例如 `192.168.1.100`）。
     - **外部端口 (External Port)**：80
     - **内部端口 (Internal Port)**：80
     - **协议 (Protocol)**：TCP

4. **保存设置 (Save Settings)**：保存配置并确保规则生效。

### 3. 获取公网 IP 地址 (Get Public IP Address)

1. 通过访问 [WhatIsMyIP](https://www.whatismyip.com/) 或谷歌搜索“我的 IP”，获取你的公网 IP 地址。

2. 其他用户可以通过 `http://<你的公网 IP>` 来访问你的网站。

---

## 5. 步骤 4: 配置 Windows 防火墙 (Step 4: Configure Windows Firewall)

1. **打开 Windows 防火墙 (Open Windows Firewall Settings)**:
   - 按 `Win + R`，输入 `firewall.cpl` 打开防火墙设置。

2. **允许端口 80 (Allow Port 80)**：
   - 点击左侧的“高级设置”。
   - 选择“入站规则”，点击“新建规则”。
   - 选择“端口”，点击“下一步”。
   - 选择“TCP”，在“特定本地端口”中输入 `80`，点击“下一步”。
   - 选择“允许连接”，完成规则创建。

---

## 6. 公网访问的安全性 (Security for Public Access)

1. **启用 HTTPS (Enable HTTPS)**：
   - 使用免费的 Let's Encrypt SSL 证书来加密网站流量，避免未经加密的 HTTP 通信带来的安全问题。

2. **动态 DNS (Dynamic DNS)**：
   - 如果你的公网 IP 经常变化，使用动态 DNS（DDNS）服务可以为你的网站提供一个固定的域名，如 No-IP 或 DynDNS。

3. **访问控制 (Access Control)**：
   - 考虑在防火墙或路由器中设置 IP 白名单，限制哪些 IP 地址可以访问你的网站，确保安全。

---

## 7. 总结 (Summary)

1. **安装并配置 Nginx (Install and Configure Nginx)**：确保 Nginx 正确指向你的网站目录。
2. **设置端口转发 (Set Up Port Forwarding)**：通过路由器将外部端口 80 流量转发到本机的 Nginx 服务器。
3. **允许防火墙通过 80 端口 (Allow Firewall to Pass Port 80)**：确保防火墙不会阻止 HTTP 流量。
4. **公网访问 (Enable Public Access)**：通过公网 IP 或 DDNS 实现远程访问。
