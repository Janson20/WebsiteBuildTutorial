---
layout: default
title: React 网站开发
nav_order: 1
parent: 教程
---

# React 网站开发指南

本教程介绍如何使用React从零开始搭建一个基础网站。

## 一、环境准备

### 1.1 安装Node.js

```bash
# macOS/Linux (使用nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
nvm use --lts

# Windows (使用winget)
winget install OpenJS.NodeJS.LTS

# 验证安装
node -v
npm -v
```

### 1.2 配置npm镜像（可选）

```bash
# 使用淘宝镜像
npm config set registry https://registry.npmmirror.com

# 或使用pnpm
npm install -g pnpm
pnpm config set registry https://registry.npmmirror.com
```

## 二、创建项目

### 2.1 使用Vite创建项目

```bash
npm create vite@latest my-react-site -- --template react-ts
cd my-react-site
npm install
npm run dev
```

### 2.2 项目结构

```
my-react-site/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   │   └── react.svg
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   └── Card.tsx
│   ├── pages/
│   │   ├── Home.tsx
│   │   └── About.tsx
│   ├── styles/
│   │   └── index.css
│   ├── App.tsx
│   └── main.tsx
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## 三、核心代码实现

### 3.1 入口文件 `src/main.tsx`

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App'
import './styles/index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
)
```

### 3.2 主应用组件 `src/App.tsx`

```tsx
import { Routes, Route } from 'react-router-dom'
import Header from './components/Header'
import Footer from './components/Footer'
import Home from './pages/Home'
import About from './pages/About'

function App() {
  return (
    <div className="app">
      <Header />
      <main className="main-content">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </main>
      <Footer />
    </div>
  )
}

export default App
```

### 3.3 Header组件 `src/components/Header.tsx`

```tsx
import { Link } from 'react-router-dom'

function Header() {
  return (
    <header className="header">
      <div className="container">
        <Link to="/" className="logo">
          My React Site
        </Link>
        <nav className="nav">
          <Link to="/">首页</Link>
          <Link to="/about">关于</Link>
        </nav>
      </div>
    </header>
  )
}

export default Header
```

### 3.4 Footer组件 `src/components/Footer.tsx`

```tsx
function Footer() {
  return (
    <footer className="footer">
      <div className="container">
        <p>&copy; 2026 My React Site. All rights reserved.</p>
      </div>
    </footer>
  )
}

export default Footer
```

### 3.5 Card组件 `src/components/Card.tsx`

```tsx
interface CardProps {
  title: string
  description: string
  image?: string
}

function Card({ title, description, image }: CardProps) {
  return (
    <div className="card">
      {image && <img src={image} alt={title} className="card-image" />}
      <div className="card-content">
        <h3 className="card-title">{title}</h3>
        <p className="card-description">{description}</p>
      </div>
    </div>
  )
}

export default Card
```

### 3.6 Home页面 `src/pages/Home.tsx`

```tsx
import Card from '../components/Card'

function Home() {
  const features = [
    {
      title: '快速开发',
      description: '使用React组件化开发，提高开发效率',
      image: 'https://picsum.photos/300/200?random=1'
    },
    {
      title: '响应式设计',
      description: '自动适配不同设备屏幕尺寸',
      image: 'https://picsum.photos/300/200?random=2'
    },
    {
      title: '现代技术栈',
      description: '采用Vite + TypeScript构建',
      image: 'https://picsum.photos/300/200?random=3'
    }
  ]

  return (
    <div className="home">
      <section className="hero">
        <h1>欢迎来到我的网站</h1>
        <p>使用React构建的现代化网站</p>
      </section>
      
      <section className="features">
        <h2>特色功能</h2>
        <div className="card-grid">
          {features.map((feature, index) => (
            <Card key={index} {...feature} />
          ))}
        </div>
      </section>
    </div>
  )
}

export default Home
```

### 3.7 About页面 `src/pages/About.tsx`

```tsx
function About() {
  return (
    <div className="about">
      <h1>关于我们</h1>
      <p>这是一个使用React构建的示例网站。</p>
      
      <h2>技术栈</h2>
      <ul>
        <li>React 18</li>
        <li>TypeScript</li>
        <li>Vite</li>
        <li>React Router</li>
      </ul>
      
      <h2>联系方式</h2>
      <p>邮箱：contact@example.com</p>
    </div>
  )
}

export default About
```

### 3.8 样式文件 `src/styles/index.css`

```css
/* 全局样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
  color: #333;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Header样式 */
.header {
  background: #1a1a2e;
  color: white;
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: 100;
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
  text-decoration: none;
}

.nav a {
  color: white;
  text-decoration: none;
  margin-left: 2rem;
  transition: color 0.3s;
}

.nav a:hover {
  color: #4facfe;
}

/* 主内容 */
.main-content {
  min-height: calc(100vh - 160px);
  padding: 2rem 0;
}

/* Hero区域 */
.hero {
  text-align: center;
  padding: 4rem 0;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  margin: -2rem 0 2rem;
}

.hero h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
}

/* 卡片网格 */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  padding: 2rem 0;
}

.card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.card-image {
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
}

.card-description {
  color: #666;
}

/* Footer样式 */
.footer {
  background: #1a1a2e;
  color: white;
  text-align: center;
  padding: 2rem 0;
}

/* 关于页面 */
.about {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.about h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.about h2 {
  margin: 2rem 0 1rem;
  color: #667eea;
}

.about ul {
  padding-left: 2rem;
}

.about li {
  margin: 0.5rem 0;
}
```

## 四、安装依赖

```bash
npm install react-router-dom
```

## 五、构建与部署

### 5.1 本地开发

```bash
npm run dev
# 访问 http://localhost:5173
```

### 5.2 构建生产版本

```bash
npm run build
# 输出目录: dist/
```

### 5.3 预览构建结果

```bash
npm run preview
```

## 六、常见配置

### 6.1 配置路径别名 `vite.config.ts`

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})
```

### 6.2 环境变量

创建 `.env.local`:

```env
VITE_API_URL=https://api.example.com
VITE_SITE_NAME=My React Site
```

使用环境变量：

```tsx
const apiUrl = import.meta.env.VITE_API_URL
```

## 下一步

React网站开发完成后，继续阅读 [服务器部署指南]({{ site.baseurl }}/02-server-deployment) 学习服务器部署。
