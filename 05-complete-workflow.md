---
layout: default
title: 完整实战案例
nav_order: 5
parent: 教程
---

# 完整实战案例

本教程通过一个完整的案例，演示从零开始搭建网站的完整流程。

## 项目概述

假设我们要搭建一个个人技术博客网站：

- **域名**：`techblog.com`
- **技术栈**：Next.js + Markdown
- **服务器**：阿里云ECS
- **CDN**：Cloudflare

## 第一阶段：项目开发

### 1.1 创建项目

```bash
# 创建Next.js项目
npx create-next-app@latest techblog --typescript --tailwind

# 选择配置
✓ Would you like to use ESLint? Yes
✓ Would you like to use App Router? Yes
✓ Would you like to customize the default import alias? No

cd techblog
```

### 1.2 项目结构

```
techblog/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── blog/
│   │   └── [slug]/page.tsx
│   └── api/
├── components/
│   ├── Header.tsx
│   ├── Footer.tsx
│   └── PostCard.tsx
├── content/
│   └── posts/
│       ├── getting-started.md
│       └── deployment-guide.md
├── lib/
│   └── posts.ts
├── public/
├── package.json
└── next.config.js
```

### 1.3 关键代码

`lib/posts.ts`：

```typescript
import fs from 'fs';
import path from 'path';
import matter from 'gray-matter';

const postsDirectory = path.join(process.cwd(), 'content/posts');

export function getPostSlugs() {
  return fs.readdirSync(postsDirectory).filter(file => file.endsWith('.md'));
}

export function getPostBySlug(slug: string) {
  const realSlug = slug.replace(/\.md$/, '');
  const fullPath = path.join(postsDirectory, `${realSlug}.md`);
  const fileContents = fs.readFileSync(fullPath, 'utf8');
  const { data, content } = matter(fileContents);

  return {
    slug: realSlug,
    meta: data,
    content,
  };
}

export function getAllPosts() {
  const slugs = getPostSlugs();
  return slugs
    .map(slug => getPostBySlug(slug))
    .sort((a, b) => (a.meta.date < b.meta.date ? 1 : -1));
}
```

`app/blog/[slug]/page.tsx`：

```typescript
import { getPostBySlug, getAllPosts } from '@/lib/posts';
import { notFound } from 'next/navigation';

export async function generateStaticParams() {
  const posts = getAllPosts();
  return posts.map(post => ({ slug: post.slug }));
}

export default function BlogPost({ params }: { params: { slug: string } }) {
  const post = getPostBySlug(params.slug);
  
  if (!post) {
    notFound();
  }

  return (
    <article className="prose lg:prose-xl mx-auto p-8">
      <h1>{post.meta.title}</h1>
      <time>{post.meta.date}</time>
      <div dangerouslySetInnerHTML={{ __html: post.content }} />
    </article>
  );
}
```

### 1.4 环境变量

```bash
# .env.local
NEXT_PUBLIC_SITE_URL=https://techblog.com
NEXT_PUBLIC_SITE_NAME=Tech Blog
```

### 1.5 本地测试

```bash
npm run dev
# 访问 http://localhost:3000
```

### 1.6 构建测试

```bash
npm run build
npm run start
# 访问 http://localhost:3000
```

## 第二阶段：购买域名

### 2.1 查询域名

1. 登录阿里云
2. 搜索 `techblog.com`
3. 查看价格和可用性

### 2.2 购买流程

```
1. 添加到购物车
2. 选择购买年限：3年
3. 选择信息模板（已完成实名认证）
4. 支付订单
```

### 2.3 域名管理

```
控制台 → 域名 → 域名列表
查看：techblog.com
状态：已注册
```

## 第三阶段：购买服务器

### 3.1 选择配置

| 项目 | 配置 |
|------|------|
| 地域 | 华东1（杭州） |
| 实例规格 | 2核4GB |
| 镜像 | Ubuntu 22.04 |
| 存储 | 40GB SSD |
| 带宽 | 3Mbps |

### 3.2 购买步骤

```
1. 选择配置
2. 设置登录凭证（密钥对）
3. 确认订单
4. 支付
```

### 3.3 连接服务器

```bash
ssh -i ~/.ssh/aliyun-key.pem root@服务器IP
```

## 第四阶段：服务器配置

### 4.1 系统更新

```bash
apt update && apt upgrade -y
```

### 4.2 安装Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs
npm install -g pm2
```

### 4.3 安装Nginx

```bash
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

### 4.4 配置Nginx

创建 `/etc/nginx/sites-available/techblog`：

```nginx
server {
    listen 80;
    server_name techblog.com www.techblog.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

启用配置：

```bash
ln -s /etc/nginx/sites-available/techblog /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

### 4.5 配置防火墙

```bash
ufw allow 22
ufw allow 80
ufw allow 443
ufw enable
```

## 第五阶段：部署应用

### 5.1 上传代码

方式1：使用Git

```bash
# 在服务器上
cd /var/www
git clone https://github.com/yourusername/techblog.git
cd techblog
```

方式2：使用rsync

```bash
# 在本地
rsync -avz --exclude 'node_modules' ./ root@服务器IP:/var/www/techblog/
```

### 5.2 安装依赖并构建

```bash
cd /var/www/techblog
npm install
npm run build
```

### 5.3 启动应用

创建PM2配置 `ecosystem.config.js`：

```javascript
module.exports = {
  apps: [{
    name: 'techblog',
    script: 'npm',
    args: 'start',
    cwd: '/var/www/techblog',
    instances: 2,
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'production',
      PORT: 3000
    }
  }]
};
```

启动：

```bash
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

### 5.4 验证部署

```bash
curl http://localhost:3000
```

## 第六阶段：配置Cloudflare

### 6.1 添加站点

```
1. 登录Cloudflare
2. Add site: techblog.com
3. 选择Free计划
4. 记录名称服务器：
   bob.ns.cloudflare.com
   coco.ns.cloudflare.com
```

### 6.2 修改DNS服务器

在阿里云域名控制台：

```
域名管理 → DNS修改 → 修改DNS服务器
输入Cloudflare提供的名称服务器
```

等待生效（约2-6小时）

### 6.3 配置DNS记录

```
类型    名称    内容           代理状态
A       @       服务器IP      已代理 🟠
CNAME   www     techblog.com  已代理 🟠
```

### 6.4 配置SSL

```
SSL/TLS → 概述
加密模式: 完全(严格)

SSL/TLS → 边缘证书
始终使用HTTPS: 开启
HSTS: 开启
```

### 6.5 性能优化

```
速度 → 优化
Auto Minify: HTML, CSS, JS 全选
Brotli: 开启
Early Hints: 开启
```

## 第七阶段：验证上线

### 7.1 DNS检查

```bash
dig techblog.com
# 应返回Cloudflare的IP

dig www.techblog.com
# 应返回CNAME指向techblog.com
```

### 7.2 访问测试

```bash
curl -I https://techblog.com
# 检查HTTP状态码和响应头
```

### 7.3 SSL检查

访问 [SSL Labs](https://www.ssllabs.com/ssltest/) 测试SSL配置。

### 7.4 性能测试

访问 [PageSpeed Insights](https://pagespeed.web.dev/) 测试性能。

## 后续维护

### 更新网站

```bash
# 在服务器上
cd /var/www/techblog
git pull
npm install
npm run build
pm2 restart techblog
```

### 查看日志

```bash
# 应用日志
pm2 logs techblog

# Nginx日志
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### 监控状态

```bash
pm2 monit
htop
```

### 备份

```bash
# 备份网站文件
tar -czf techblog-backup-$(date +%Y%m%d).tar.gz /var/www/techblog

# 定时备份（crontab）
0 2 * * * tar -czf /backup/techblog-$(date +\%Y\%m\%d).tar.gz /var/www/techblog
```

## 完整流程图

```
┌─────────────────┐
│  项目开发        │
│  (本地环境)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  购买域名        │
│  (阿里云)        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  购买服务器      │
│  (阿里云ECS)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  服务器配置      │
│  (Nginx/Node)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  部署应用        │
│  (PM2)          │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  配置Cloudflare  │
│  (DNS/SSL/CDN)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  验证上线        │
│  (测试访问)      │
└─────────────────┘
```

## 成本估算

| 项目 | 费用 | 周期 |
|------|------|------|
| 域名 | ¥180 | 3年 |
| 服务器 | ¥1500 | 年 |
| Cloudflare | 免费 | - |
| **总计** | ¥1560 | 首年 |

## 常见问题排查

### 网站无法访问

```
1. 检查服务器状态: systemctl status nginx
2. 检查应用状态: pm2 status
3. 检查防火墙: ufw status
4. 检查DNS: dig techblog.com
5. 检查Cloudflare状态面板
```

### 样式/资源加载失败

```
1. 检查浏览器控制台错误
2. 检查Nginx静态文件路径
3. 清除Cloudflare缓存
4. 检查CSP策略
```

### SSL证书错误

```
1. 确认SSL模式配置正确
2. 检查源站证书是否过期
3. 清除浏览器缓存
4. 检查混合内容问题
```

## 总结

通过以上七个阶段，我们完成了：

1. ✅ 使用Next.js开发了博客网站
2. ✅ 在阿里云购买了域名和服务器
3. ✅ 配置了服务器环境
4. ✅ 部署了应用
5. ✅ 配置了Cloudflare DNS和CDN
6. ✅ 启用了HTTPS
7. ✅ 验证网站正常访问

网站已成功上线，后续可以进行[性能优化]({{ site.baseurl }}/06a-performance-optimization)、[SEO优化]({{ site.baseurl }}/06b-seo-optimization)和[内容更新]({{ site.baseurl }}/06c-content-update)。
