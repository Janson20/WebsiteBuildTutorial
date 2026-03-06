---
layout: default
title: HTML/CSS/JS 网站开发
nav_order: 3
parent: 教程
---

# HTML/CSS/JS 网站开发指南

本教程介绍如何使用纯HTML、CSS和JavaScript从零开始搭建一个基础网站，无需任何框架。

## 一、环境准备

### 1.1 所需工具

- **代码编辑器**：VS Code（推荐）
- **浏览器**：Chrome、Firefox 或 Edge
- **本地服务器**（可选）：Live Server VS Code插件

### 1.2 VS Code推荐插件

```
- Live Server - 本地开发服务器
- HTML CSS Support - HTML/CSS智能提示
- Auto Rename Tag - 自动重命名配对标签
- Prettier - 代码格式化
```

## 二、项目结构

### 2.1 创建文件夹结构

```
my-html-site/
├── index.html
├── about.html
├── css/
│   ├── reset.css
│   ├── style.css
│   └── responsive.css
├── js/
│   └── main.js
├── images/
│   └── (图片文件)
└── favicon.ico
```

## 三、核心代码实现

### 3.1 首页 `index.html`

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="使用HTML/CSS/JS构建的现代化网站">
  <meta name="keywords" content="HTML, CSS, JavaScript, 网站开发">
  <title>我的网站 - 首页</title>
  <link rel="stylesheet" href="css/reset.css">
  <link rel="stylesheet" href="css/style.css">
  <link rel="stylesheet" href="css/responsive.css">
  <link rel="icon" href="favicon.ico" type="image/x-icon">
</head>
<body>
  <!-- 导航栏 -->
  <header class="header">
    <div class="container">
      <a href="index.html" class="logo">My Website</a>
      <button class="menu-toggle" aria-label="切换菜单">
        <span></span>
        <span></span>
        <span></span>
      </button>
      <nav class="nav">
        <ul class="nav-list">
          <li><a href="index.html" class="active">首页</a></li>
          <li><a href="about.html">关于</a></li>
          <li><a href="#features">功能</a></li>
          <li><a href="#contact">联系</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <!-- Hero区域 -->
  <section class="hero">
    <div class="container">
      <h1 class="hero-title">欢迎来到我的网站</h1>
      <p class="hero-subtitle">使用纯HTML、CSS和JavaScript构建的现代化网站</p>
      <a href="#features" class="btn btn-primary">了解更多</a>
    </div>
  </section>

  <!-- 功能特性 -->
  <section id="features" class="features">
    <div class="container">
      <h2 class="section-title">特色功能</h2>
      <div class="card-grid">
        <div class="card">
          <div class="card-image">
            <img src="https://picsum.photos/300/200?random=1" alt="性能优化">
          </div>
          <div class="card-content">
            <h3 class="card-title">轻量高效</h3>
            <p class="card-description">无框架依赖，加载速度快，性能优异</p>
          </div>
        </div>
        <div class="card">
          <div class="card-image">
            <img src="https://picsum.photos/300/200?random=2" alt="响应式设计">
          </div>
          <div class="card-content">
            <h3 class="card-title">响应式设计</h3>
            <p class="card-description">完美适配桌面端和移动端设备</p>
          </div>
        </div>
        <div class="card">
          <div class="card-image">
            <img src="https://picsum.photos/300/200?random=3" alt="易于维护">
          </div>
          <div class="card-content">
            <h3 class="card-title">易于维护</h3>
            <p class="card-description">代码结构清晰，便于理解和修改</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- 联系区域 -->
  <section id="contact" class="contact">
    <div class="container">
      <h2 class="section-title">联系我们</h2>
      <form class="contact-form" id="contactForm">
        <div class="form-group">
          <label for="name">姓名</label>
          <input type="text" id="name" name="name" required>
        </div>
        <div class="form-group">
          <label for="email">邮箱</label>
          <input type="email" id="email" name="email" required>
        </div>
        <div class="form-group">
          <label for="message">留言</label>
          <textarea id="message" name="message" rows="5" required></textarea>
        </div>
        <button type="submit" class="btn btn-primary">发送消息</button>
      </form>
    </div>
  </section>

  <!-- 页脚 -->
  <footer class="footer">
    <div class="container">
      <p>&copy; 2026 My Website. All rights reserved.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

### 3.2 关于页面 `about.html`

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="关于我们 - HTML/CSS/JS网站">
  <title>关于我们 - 我的网站</title>
  <link rel="stylesheet" href="css/reset.css">
  <link rel="stylesheet" href="css/style.css">
  <link rel="stylesheet" href="css/responsive.css">
  <link rel="icon" href="favicon.ico" type="image/x-icon">
</head>
<body>
  <!-- 导航栏 -->
  <header class="header">
    <div class="container">
      <a href="index.html" class="logo">My Website</a>
      <button class="menu-toggle" aria-label="切换菜单">
        <span></span>
        <span></span>
        <span></span>
      </button>
      <nav class="nav">
        <ul class="nav-list">
          <li><a href="index.html">首页</a></li>
          <li><a href="about.html" class="active">关于</a></li>
          <li><a href="index.html#features">功能</a></li>
          <li><a href="index.html#contact">联系</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <!-- 主内容 -->
  <main class="main-content">
    <div class="container">
      <section class="about-section">
        <h1>关于我们</h1>
        <p class="lead">这是一个使用纯HTML、CSS和JavaScript构建的示例网站。</p>
        
        <h2>我们的使命</h2>
        <p>我们致力于展示现代Web开发的基础技术，帮助开发者理解Web的本质。</p>
        
        <h2>技术栈</h2>
        <ul class="tech-list">
          <li>
            <strong>HTML5</strong>
            <p>语义化标签，结构清晰的网页</p>
          </li>
          <li>
            <strong>CSS3</strong>
            <p>Flexbox、Grid、动画、响应式设计</p>
          </li>
          <li>
            <strong>JavaScript</strong>
            <p>原生JS，无框架依赖</p>
          </li>
        </ul>
        
        <h2>为什么选择原生技术？</h2>
        <div class="benefits-grid">
          <div class="benefit-item">
            <h3>无需构建</h3>
            <p>直接在浏览器中运行，无需Node.js或打包工具</p>
          </div>
          <div class="benefit-item">
            <h3>学习成本低</h3>
            <p>专注于Web基础，理解浏览器工作原理</p>
          </div>
          <div class="benefit-item">
            <h3>性能优异</h3>
            <p>无框架开销，加载和执行速度快</p>
          </div>
          <div class="benefit-item">
            <h3>长期维护</h3>
            <p>不依赖框架版本更新，代码长期有效</p>
          </div>
        </div>
      </section>
    </div>
  </main>

  <!-- 页脚 -->
  <footer class="footer">
    <div class="container">
      <p>&copy; 2026 My Website. All rights reserved.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

### 3.3 CSS重置文件 `css/reset.css`

```css
/* CSS Reset */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 
               'Helvetica Neue', Arial, sans-serif;
  line-height: 1.6;
  color: #333;
  background-color: #f5f5f5;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  text-decoration: none;
  color: inherit;
}

ul, ol {
  list-style: none;
}

button {
  cursor: pointer;
  border: none;
  background: none;
  font-family: inherit;
}

input, textarea {
  font-family: inherit;
  font-size: inherit;
}
```

### 3.4 主样式文件 `css/style.css`

```css
/* 容器 */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* 按钮 */
.btn {
  display: inline-block;
  padding: 12px 30px;
  border-radius: 30px;
  font-weight: 600;
  transition: all 0.3s ease;
  cursor: pointer;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
}

/* 导航栏 */
.header {
  background: #1a1a2e;
  color: white;
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: 1000;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  color: white;
}

.nav-list {
  display: flex;
  gap: 2rem;
}

.nav-list a {
  color: white;
  transition: color 0.3s;
  position: relative;
}

.nav-list a::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: #667eea;
  transition: width 0.3s;
}

.nav-list a:hover::after,
.nav-list a.active::after {
  width: 100%;
}

.menu-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
}

.menu-toggle span {
  width: 25px;
  height: 3px;
  background: white;
  border-radius: 3px;
  transition: all 0.3s;
}

/* Hero区域 */
.hero {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
  padding: 6rem 0;
}

.hero-title {
  font-size: 3rem;
  margin-bottom: 1rem;
  animation: fadeInUp 0.8s ease;
}

.hero-subtitle {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  opacity: 0.9;
  animation: fadeInUp 0.8s ease 0.2s both;
}

.hero .btn {
  animation: fadeInUp 0.8s ease 0.4s both;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 功能特性 */
.features {
  padding: 5rem 0;
}

.section-title {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 3rem;
  color: #1a1a2e;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.card-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

.card-title {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
  color: #1a1a2e;
}

.card-description {
  color: #666;
}

/* 联系区域 */
.contact {
  padding: 5rem 0;
  background: white;
}

.contact-form {
  max-width: 600px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: #1a1a2e;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #667eea;
}

/* 关于页面 */
.main-content {
  padding: 3rem 0;
}

.about-section {
  max-width: 800px;
  margin: 0 auto;
}

.about-section h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: #1a1a2e;
}

.about-section .lead {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 2rem;
}

.about-section h2 {
  color: #667eea;
  margin: 2rem 0 1rem;
}

.about-section p {
  color: #666;
  margin-bottom: 1rem;
}

.tech-list {
  margin: 1rem 0;
}

.tech-list li {
  padding: 1rem;
  background: #f8f9fa;
  margin-bottom: 1rem;
  border-radius: 8px;
  border-left: 4px solid #667eea;
}

.benefits-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.benefit-item {
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.benefit-item h3 {
  margin-bottom: 0.5rem;
}

.benefit-item p {
  font-size: 0.9rem;
  opacity: 0.9;
}

/* 页脚 */
.footer {
  background: #1a1a2e;
  color: white;
  text-align: center;
  padding: 2rem 0;
}
```

### 3.5 响应式样式 `css/responsive.css`

```css
/* 响应式设计 */
@media (max-width: 768px) {
  /* 导航 */
  .menu-toggle {
    display: flex;
  }
  
  .nav {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background: #1a1a2e;
    padding: 1rem;
    display: none;
  }
  
  .nav.active {
    display: block;
  }
  
  .nav-list {
    flex-direction: column;
    gap: 1rem;
  }
  
  /* Hero */
  .hero {
    padding: 4rem 0;
  }
  
  .hero-title {
    font-size: 2rem;
  }
  
  .hero-subtitle {
    font-size: 1rem;
  }
  
  /* 卡片 */
  .card-grid {
    grid-template-columns: 1fr;
  }
  
  /* 功能网格 */
  .benefits-grid {
    grid-template-columns: 1fr;
  }
  
  /* 表单 */
  .contact-form {
    padding: 0 1rem;
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: 1.75rem;
  }
  
  .btn {
    padding: 10px 20px;
    font-size: 0.9rem;
  }
}
```

### 3.6 JavaScript文件 `js/main.js`

```javascript
// DOM加载完成后执行
document.addEventListener('DOMContentLoaded', function() {
  // 移动端菜单切换
  initMobileMenu();
  
  // 平滑滚动
  initSmoothScroll();
  
  // 表单处理
  initContactForm();
  
  // 滚动动画
  initScrollAnimations();
});

// 移动端菜单
function initMobileMenu() {
  const menuToggle = document.querySelector('.menu-toggle');
  const nav = document.querySelector('.nav');
  
  if (menuToggle && nav) {
    menuToggle.addEventListener('click', function() {
      nav.classList.toggle('active');
      this.classList.toggle('active');
    });
    
    // 点击导航链接后关闭菜单
    const navLinks = nav.querySelectorAll('a');
    navLinks.forEach(link => {
      link.addEventListener('click', function() {
        nav.classList.remove('active');
        menuToggle.classList.remove('active');
      });
    });
  }
}

// 平滑滚动
function initSmoothScroll() {
  const links = document.querySelectorAll('a[href^="#"]');
  
  links.forEach(link => {
    link.addEventListener('click', function(e) {
      const href = this.getAttribute('href');
      if (href === '#') return;
      
      const target = document.querySelector(href);
      if (target) {
        e.preventDefault();
        target.scrollIntoView({
          behavior: 'smooth',
          block: 'start'
        });
      }
    });
  });
}

// 联系表单
function initContactForm() {
  const form = document.getElementById('contactForm');
  
  if (form) {
    form.addEventListener('submit', function(e) {
      e.preventDefault();
      
      // 获取表单数据
      const formData = new FormData(form);
      const data = {
        name: formData.get('name'),
        email: formData.get('email'),
        message: formData.get('message')
      };
      
      // 模拟提交（实际项目中应发送到服务器）
      console.log('表单数据:', data);
      alert('感谢您的留言！我们会尽快回复。');
      form.reset();
    });
  }
}

// 滚动动画
function initScrollAnimations() {
  const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
  };
  
  const observer = new IntersectionObserver(function(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('animate-in');
      }
    });
  }, observerOptions);
  
  // 观察卡片元素
  const cards = document.querySelectorAll('.card');
  cards.forEach(card => {
    card.style.opacity = '0';
    card.style.transform = 'translateY(30px)';
    card.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
    observer.observe(card);
  });
}

// 添加动画类样式
const style = document.createElement('style');
style.textContent = `
  .animate-in {
    opacity: 1 !important;
    transform: translateY(0) !important;
  }
`;
document.head.appendChild(style);
```

## 四、本地开发

### 4.1 直接打开

双击`index.html`文件即可在浏览器中查看。

### 4.2 使用Live Server

1. 在VS Code中安装Live Server插件
2. 右键点击`index.html`
3. 选择"Open with Live Server"
4. 自动在浏览器中打开`http://127.0.0.1:5500`

### 4.3 使用Python简单服务器

```bash
# Python 3
python -m http.server 8000

# 访问 http://localhost:8000
```

### 4.4 使用Node.js服务器

```bash
# 安装serve
npm install -g serve

# 运行
serve .

# 访问 http://localhost:3000
```

## 五、部署

### 5.1 静态托管平台

直接上传整个文件夹到以下平台：

| 平台 | 特点 |
|------|------|
| GitHub Pages | 免费，支持自定义域名 |
| Netlify | 免费，自动部署 |
| Vercel | 免费，全球CDN |
| Cloudflare Pages | 免费，无限带宽 |

### 5.2 传统服务器

上传所有文件到服务器的web目录：

```bash
# 使用scp上传
scp -r ./* user@server:/var/www/html/

# 或使用rsync
rsync -avz . user@server:/var/www/html/
```

## 六、SEO优化

### 6.1 Meta标签

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="网站描述，150字以内">
  <meta name="keywords" content="关键词1, 关键词2">
  <meta name="author" content="作者名">
  
  <!-- Open Graph -->
  <meta property="og:title" content="页面标题">
  <meta property="og:description" content="页面描述">
  <meta property="og:image" content="分享图片URL">
  <meta property="og:url" content="页面URL">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="页面标题">
  <meta name="twitter:description" content="页面描述">
</head>
```

### 6.2 语义化HTML

```html
<!-- 使用语义化标签 -->
<header></header>
<nav></nav>
<main>
  <article>
    <section></section>
  </article>
  <aside></aside>
</main>
<footer></footer>
```

## 下一步

HTML网站开发完成后，继续阅读 [服务器部署指南]({{ site.baseurl }}/02-server-deployment) 学习服务器部署。
