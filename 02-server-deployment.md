---
layout: default
title: 服务器部署指南
nav_order: 2
parent: 教程
---

# 服务器部署指南

本教程介绍如何将网站部署到服务器，包括服务器选择、环境配置和应用部署。

## 一、服务器选择

### 1.1 云服务商对比

| 服务商 | 优势 | 适用场景 |
|--------|------|---------|
| 阿里云 | 国内访问快，生态完善 | 国内用户为主的项目 |
| 腾讯云 | 价格有竞争力，轻量应用服务器 | 个人项目、小型应用 |
| AWS | 全球节点多，服务全面 | 海外用户、企业级应用 |
| Vultr | 按小时计费，性价比高 | 海外项目、测试环境 |
| DigitalOcean | 简单易用，文档友好 | 海外项目、开发者 |

### 1.2 服务器配置建议

| 项目类型 | CPU | 内存 | 存储 | 带宽 |
|---------|-----|------|------|------|
| 静态网站 | 1核 | 1GB | 20GB | 1Mbps |
| 小型应用 | 2核 | 2GB | 40GB | 3Mbps |
| 中型应用 | 2核 | 4GB | 80GB | 5Mbps |
| 大型应用 | 4核+ | 8GB+ | 100GB+ | 10Mbps+ |

### 1.3 无服务器部署平台

对于静态站点和轻量应用，推荐使用无服务器平台：

| 平台 | 特点 | 免费额度 |
|------|------|---------|
| Vercel | Next.js官方托管 | 100GB/月 |
| Netlify | 静态站点托管 | 100GB/月 |
| GitHub Pages | 静态站点 | 100GB/月 |
| Cloudflare Pages | 全球CDN | 无限带宽 |

## 二、服务器初始配置

### 2.1 连接服务器

```bash
# SSH连接
ssh root@your_server_ip

# 使用密钥连接
ssh -i ~/.ssh/your_key.pem root@your_server_ip
```

### 2.2 更新系统

```bash
# Ubuntu/Debian
apt update && apt upgrade -y

# CentOS
yum update -y
```

### 2.3 创建普通用户

```bash
# 创建用户
adduser deployer

# 添加sudo权限
usermod -aG sudo deployer

# 切换用户
su - deployer
```

### 2.4 配置SSH安全

编辑 `/etc/ssh/sshd_config`：

```
# 禁用root登录
PermitRootLogin no

# 使用密钥登录
PasswordAuthentication no

# 更改SSH端口（可选）
Port 2222
```

重启SSH服务：

```bash
systemctl restart sshd
```

### 2.5 配置防火墙

```bash
# Ubuntu (ufw)
ufw allow 22
ufw allow 80
ufw allow 443
ufw enable

# CentOS (firewalld)
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
```

## 三、安装Web服务器

### 3.1 安装Nginx

```bash
# Ubuntu/Debian
apt install nginx -y

# CentOS
yum install nginx -y

# 启动并设置开机自启
systemctl start nginx
systemctl enable nginx
```

### 3.2 Nginx基本配置

编辑 `/etc/nginx/nginx.conf`：

```nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    sendfile on;
    keepalive_timeout 65;
    gzip on;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

### 3.3 配置站点

创建 `/etc/nginx/sites-available/your-site`：

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;

    root /var/www/your-site;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 静态资源缓存
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Gzip压缩
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;
}
```

启用站点：

```bash
ln -s /etc/nginx/sites-available/your-site /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

## 四、安装Node.js环境

### 4.1 使用NVM安装

```bash
# 安装NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc

# 安装Node.js
nvm install --lts
nvm use --lts
```

### 4.2 使用包管理器安装

```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
apt install -y nodejs

# CentOS
curl -fsSL https://rpm.nodesource.com/setup_lts.x | sudo bash -
yum install -y nodejs
```

### 4.3 安装PM2

```bash
npm install -g pm2
```

## 五、部署方式

### 5.1 手动部署

```bash
# 上传文件
scp -r ./dist/* deployer@your_server_ip:/var/www/your-site/

# 或使用rsync
rsync -avz ./dist/ deployer@your_server_ip:/var/www/your-site/
```

### 5.2 Git部署

```bash
# 在服务器上克隆仓库
cd /var/www
git clone https://github.com/your-username/your-repo.git your-site

# 构建
cd your-site
npm install
npm run build

# 链接到Nginx目录
ln -s /var/www/your-site/dist /var/www/your-site
```

### 5.3 使用PM2部署Node.js应用

创建 `ecosystem.config.js`：

```javascript
module.exports = {
  apps: [{
    name: 'my-app',
    script: 'npm',
    args: 'start',
    cwd: '/var/www/my-app',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'production',
      PORT: 3000
    }
  }]
};
```

启动应用：

```bash
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

### 5.4 CI/CD自动部署

#### GitHub Actions示例

创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to server
        uses: appleboy/scp-action@v0.1.4
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          source: "dist/*"
          target: "/var/www/your-site"
```

## 六、SSL证书配置

### 6.1 使用Let's Encrypt

```bash
# 安装Certbot
apt install certbot python3-certbot-nginx -y

# 获取证书
certbot --nginx -d your-domain.com -d www.your-domain.com

# 自动续期
certbot renew --dry-run
```

### 6.2 手动配置SSL

Nginx SSL配置：

```nginx
server {
    listen 443 ssl http2;
    server_name your-domain.com;

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;
    ssl_prefer_server_ciphers off;

    # 其余配置...
}

server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$server_name$request_uri;
}
```

## 七、常用命令

### 7.1 Nginx命令

```bash
# 测试配置
nginx -t

# 重载配置
systemctl reload nginx

# 重启服务
systemctl restart nginx

# 查看状态
systemctl status nginx

# 查看日志
tail -f /var/log/nginx/error.log
```

### 7.2 PM2命令

```bash
# 查看应用状态
pm2 status

# 查看日志
pm2 logs

# 重启应用
pm2 restart all

# 监控
pm2 monit
```

### 7.3 系统监控

```bash
# 查看资源使用
htop

# 查看磁盘使用
df -h

# 查看网络连接
netstat -tlnp
```

## 八、常见问题

### Q1: 502 Bad Gateway

原因：后端服务未运行或端口配置错误

解决：
```bash
# 检查应用是否运行
pm2 status

# 检查端口
netstat -tlnp | grep 3000

# 检查Nginx日志
tail -f /var/log/nginx/error.log
```

### Q2: 权限问题

```bash
# 修改文件所有者
chown -R www-data:www-data /var/www/your-site

# 修改权限
chmod -R 755 /var/www/your-site
```

### Q3: 内存不足

```bash
# 创建交换空间
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# 永久生效
echo '/swapfile none swap sw 0 0' >> /etc/fstab
```

## 下一步

服务器部署完成后，继续阅读 [03-domain-purchase.md](./03-domain-purchase.md) 学习域名购买。
