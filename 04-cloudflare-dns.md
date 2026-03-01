---
layout: default
title: Cloudflare DNS配置指南
nav_order: 4
parent: 教程
---

# Cloudflare DNS配置指南

本教程介绍如何使用Cloudflare配置DNS解析、CDN加速和安全防护。

## 一、Cloudflare简介

### 1.1 什么是Cloudflare

Cloudflare是全球领先的CDN和网络安全服务商，提供：

- **DNS解析**：全球最快的DNS服务
- **CDN加速**：全球300+节点
- **安全防护**：DDoS防护、WAF
- **SSL证书**：免费SSL/TLS加密
- **页面托管**：Cloudflare Pages

### 1.2 为什么选择Cloudflare

| 功能 | Cloudflare | 传统方案 |
|------|-----------|---------|
| DNS解析 | 免费，全球Anycast | 通常收费 |
| SSL证书 | 免费，自动续期 | 需购买或手动配置 |
| CDN | 免费，无限流量 | 按流量计费 |
| DDoS防护 | 免费基础防护 | 昂贵 |
| 隐藏源IP | 支持 | 需额外配置 |

## 二、账号注册与站点添加

### 2.1 注册账号

1. 访问 [Cloudflare官网](https://www.cloudflare.com/)
2. 点击"Sign Up"注册
3. 验证邮箱

### 2.2 添加站点

#### 步骤1：添加域名

```
1. 登录后点击"Add a site"
2. 输入域名（不带www）
3. 选择计划（Free免费版即可）
```

#### 步骤2：选择计划

| 计划 | 价格 | 功能 |
|------|------|------|
| Free | 免费 | 基础DNS、CDN、SSL |
| Pro | $20/月 | 高级安全、图片优化 |
| Business | $200/月 | 企业级功能 |
| Enterprise | 定制 | 定制方案

#### 步骤3：获取名称服务器

添加站点后，Cloudflare会分配两个名称服务器：

```
Nameserver 1: bob.ns.cloudflare.com
Nameserver 2: coco.ns.cloudflare.com
```

## 三、修改域名DNS服务器

### 3.1 阿里云修改

1. 登录阿里云控制台
2. 进入域名列表
3. 点击域名 → 管理
4. 选择"DNS修改"
5. 修改为Cloudflare提供的名称服务器
6. 等待生效（最长48小时，通常几小时内）

### 3.2 腾讯云修改

1. 登录腾讯云控制台
2. 进入域名注册 → 我的域名
3. 点击管理 → 修改DNS服务器
4. 填写Cloudflare名称服务器

### 3.3 Namecheap修改

1. 登录Namecheap
2. Domain List → Manage
3. Nameservers → Custom DNS
4. 填写Cloudflare名称服务器

### 3.4 验证DNS生效

```bash
# Windows
nslookup -type=ns your-domain.com

# Linux/Mac
dig ns your-domain.com

# 在线工具
https://dnschecker.org/
```

## 四、DNS记录配置

### 4.1 常用记录类型

#### A记录

指向IPv4地址：

```
类型: A
名称: @ (或留空)
内容: 192.168.1.1
代理状态: 已代理（橙色云朵）
TTL: 自动
```

#### AAAA记录

指向IPv6地址：

```
类型: AAAA
名称: @
内容: 2001:db8::1
代理状态: 已代理
```

#### CNAME记录

指向另一个域名：

```
类型: CNAME
名称: www
内容: your-domain.com
代理状态: 已代理
```

#### MX记录（邮件）

```
类型: MX
名称: @
内容: mail.your-domain.com
优先级: 10
代理状态: 仅DNS（灰色云朵）
```

### 4.2 典型配置示例

#### 静态网站配置

```
类型    名称    内容              代理状态
A       @       192.168.1.1      已代理
CNAME   www     your-domain.com  已代理
```

#### 带API的网站配置

```
类型    名称    内容              代理状态
A       @       192.168.1.1      已代理
CNAME   www     your-domain.com  已代理
A       api     192.168.1.2      已代理
```

#### 全部转发到www

```
类型    名称    内容              代理状态
CNAME   @       www.your-domain.com  已代理
A       www     192.168.1.1         已代理
```

### 4.3 代理状态说明

| 图标 | 状态 | 说明 |
|------|------|------|
| 🟠 橙色云朵 | 已代理 | 流量经过Cloudflare CDN |
| ⚪ 灰色云朵 | 仅DNS | 只做DNS解析，不经过CDN |

### 4.4 DNS生效检查

```bash
# 检查解析是否经过Cloudflare
dig your-domain.com

# 如果看到Cloudflare的IP，说明已生效
# 例如：104.21.50.100
```

## 五、SSL/TLS配置

### 5.1 加密模式

Cloudflare提供四种SSL模式：

| 模式 | 说明 | 适用场景 |
|------|------|---------|
| 关闭 | 无加密 | 不推荐 |
| 灵活 | Cloudflare到用户HTTPS，到源站HTTP | 源站无SSL |
| 完全 | 全程HTTPS，不验证源站证书 | 自签名证书 |
| 完全(严格) | 全程HTTPS，验证源站证书 | 推荐方案 |

### 5.2 配置步骤

1. 进入SSL/TLS → 概述
2. 选择"完全(严格)"模式
3. 开启"始终使用HTTPS"

### 5.3 源站证书配置

#### 方法1：使用Cloudflare Origin证书

1. SSL/TLS → 源服务器
2. 创建证书
3. 设置有效期（15年）
4. 下载证书和私钥

在Nginx中配置：

```nginx
server {
    listen 443 ssl http2;
    server_name your-domain.com;

    ssl_certificate /etc/nginx/ssl/origin.pem;
    ssl_certificate_key /etc/nginx/ssl/origin-key.pem;

    # 其余配置...
}
```

#### 方法2：使用Let's Encrypt

在源服务器上：

```bash
certbot certonly --nginx -d your-domain.com
```

注意：使用Let's Encrypt时，需关闭代理获取证书：

```
1. 暂时将A记录设为"仅DNS"
2. 获取证书
3. 改回"已代理"
```

### 5.4 HTTPS重定向

在Cloudflare中设置：

```
SSL/TLS → 边缘证书 → 始终使用HTTPS：开启
```

或在Page Rules中：

```
URL: your-domain.com/*
设置: Always Use HTTPS
```

## 六、CDN缓存配置

### 6.1 缓存规则

进入"规则" → "页面规则"：

```
URL: your-domain.com/static/*
设置:
- 缓存级别: 缓存所有内容
- 边缘缓存TTL: 1个月
- 浏览器缓存TTL: 4小时
```

### 6.2 不缓存动态内容

```
URL: your-domain.com/api/*
设置:
- 缓存级别: 绕过
```

### 6.3 清除缓存

```
缓存 → 配置 → 清除所有内容
或使用API清除特定URL
```

## 七、安全配置

### 7.1 安全级别

```
安全性 → 设置
安全级别: 中（推荐）
挑战通过期: 30分钟
浏览器完整性检查: 开启
```

### 7.2 防火墙规则

#### 屏蔽特定国家

```
安全性 → WAF
创建防火墙规则:
- 字段: 国家
- 运算符: 等于
- 值: 选择要屏蔽的国家
- 操作: 阻止
```

#### 限制管理后台访问

```
规则:
- 字段: URI路径
- 运算符: 包含
- 值: /admin
- 且
- 字段: IP地址
- 运算符: 不等于
- 值: 你的IP
- 操作: 阻止
```

### 7.3 速率限制

```
安全性 → WAF → 速率限制规则
创建规则:
- URL: /api/*
- 请求: 100次/分钟
- 操作: 阻止
- 持续时间: 10分钟
```

### 7.4 Bot防护

```
安全性 → Bots
机器人攻击模式: 启用
```

## 八、性能优化

### 8.1 速度优化

```
速度 → 优化
Auto Minify: HTML, CSS, JS 全部开启
Brotli: 开启
Early Hints: 开启
Rocket Loader: 可选开启
```

### 8.2 图片优化

```
速度 → 优化
Polish: 开启（自动压缩图片）
WebP: 开启
Mirage: 开启（自适应图片加载）
```

### 8.3 缓存优化

```nginx
# 源服务器Nginx配置
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}
```

## 九、常用配置

### 9.1 301重定向

```
规则 → 页面规则
URL: old-domain.com/*
设置: 转发URL (301 - 永久重定向)
目标: https://new-domain.com/$1
```

### 9.2 子域名配置

```
DNS记录:
类型    名称     内容              代理状态
A       blog     192.168.1.1      已代理
A       api      192.168.1.2      已代理
CNAME   shop     shop.example.com 已代理
```

### 9.3 反向代理配置

使用Cloudflare Workers实现：

```javascript
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)
  url.hostname = 'backend-server.com'
  return fetch(url, request)
}
```

## 十、监控与分析

### 10.1 流量分析

```
分析 → 流量
- 请求数统计
- 带宽使用
- 独立访客
- 威胁分析
```

### 10.2 日志查看

```
分析 → 日志
- 实时日志（付费功能）
- 事件日志
```

### 10.3 告警通知

```
通知 → 事件
配置告警:
- DDoS攻击
- WAF阻止
- SSL证书即将过期
- 站点离线
```

## 十一、常见问题

### Q1: DNS生效慢怎么办？

```bash
# 检查当前DNS
dig your-domain.com

# 刷新本地DNS缓存
# Windows
ipconfig /flushdns

# Mac
sudo dscacheutil -flushcache

# Linux
sudo systemd-resolve --flush-caches
```

### Q2: 显示"重定向次数过多"

原因：SSL模式配置错误

解决：
```
1. 如果源站无SSL，选择"灵活"模式
2. 如果源站有SSL，选择"完全"或"完全(严格)"
3. 检查源站是否强制HTTPS
```

### Q3: 网站无法访问

排查步骤：
```
1. 检查DNS解析：dig your-domain.com
2. 检查源站是否正常：直接访问源站IP
3. 检查防火墙规则
4. 查看Cloudflare分析日志
```

### Q4: API请求失败

可能原因：
```
1. 缓存了动态内容 → 设置缓存绕过
2. 跨域问题 → 配置CORS头
3. 请求超时 → 调整超时设置
```

### Q5: 如何隐藏源站IP？

```
1. 使用Cloudflare代理（橙色云朵）
2. 源站只允许Cloudflare IP访问
3. 更换源站IP（如果已泄露）
```

## 下一步

DNS配置完成后，继续阅读 [05-complete-workflow.md](./05-complete-workflow.md) 查看完整的实战案例。
