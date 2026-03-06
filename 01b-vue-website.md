---
layout: default
title: Vue 网站开发
nav_order: 2
parent: 教程
---

# Vue 网站开发指南

本教程介绍如何使用Vue 3从零开始搭建一个基础网站。

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
npm create vite@latest my-vue-site -- --template vue-ts
cd my-vue-site
npm install
npm run dev
```

### 2.2 项目结构

```
my-vue-site/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   │   └── vue.svg
│   ├── components/
│   │   ├── TheHeader.vue
│   │   ├── TheFooter.vue
│   │   └── FeatureCard.vue
│   ├── views/
│   │   ├── HomeView.vue
│   │   └── AboutView.vue
│   ├── router/
│   │   └── index.ts
│   ├── styles/
│   │   └── main.css
│   ├── App.vue
│   └── main.ts
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## 三、核心代码实现

### 3.1 入口文件 `src/main.ts`

```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import './styles/main.css'

createApp(App)
  .use(router)
  .mount('#app')
```

### 3.2 路由配置 `src/router/index.ts`

```ts
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('../views/AboutView.vue')
    }
  ]
})

export default router
```

### 3.3 主应用组件 `src/App.vue`

```vue
<script setup lang="ts">
import TheHeader from './components/TheHeader.vue'
import TheFooter from './components/TheFooter.vue'
</script>

<template>
  <div class="app">
    <TheHeader />
    <main class="main-content">
      <router-view />
    </main>
    <TheFooter />
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
}
</style>
```

### 3.4 Header组件 `src/components/TheHeader.vue`

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}
</script>

<template>
  <header class="header">
    <div class="container">
      <RouterLink to="/" class="logo">
        My Vue Site
      </RouterLink>
      
      <button class="menu-toggle" @click="toggleMenu">
        <span></span>
        <span></span>
        <span></span>
      </button>
      
      <nav class="nav" :class="{ open: isMenuOpen }">
        <RouterLink to="/">首页</RouterLink>
        <RouterLink to="/about">关于</RouterLink>
      </nav>
    </div>
  </header>
</template>

<style scoped>
.header {
  background: #1a1a2e;
  color: white;
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: 100;
}

.header .container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
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

.nav {
  display: flex;
  gap: 2rem;
}

.nav a {
  color: white;
  text-decoration: none;
  transition: color 0.3s;
}

.nav a:hover,
.nav a.router-link-active {
  color: #42b883;
}

.menu-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}

.menu-toggle span {
  display: block;
  width: 25px;
  height: 3px;
  background: white;
  border-radius: 3px;
}

@media (max-width: 768px) {
  .menu-toggle {
    display: flex;
  }
  
  .nav {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background: #1a1a2e;
    flex-direction: column;
    padding: 1rem;
    gap: 1rem;
  }
  
  .nav.open {
    display: flex;
  }
}
</style>
```

### 3.5 Footer组件 `src/components/TheFooter.vue`

```vue
<script setup lang="ts">
const currentYear = new Date().getFullYear()
</script>

<template>
  <footer class="footer">
    <div class="container">
      <p>&copy; {{ currentYear }} My Vue Site. All rights reserved.</p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: #1a1a2e;
  color: white;
  text-align: center;
  padding: 2rem 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
</style>
```

### 3.6 Card组件 `src/components/FeatureCard.vue`

```vue
<script setup lang="ts">
interface Props {
  title: string
  description: string
  image?: string
}

defineProps<Props>()
</script>

<template>
  <div class="card">
    <img v-if="image" :src="image" :alt="title" class="card-image" />
    <div class="card-content">
      <h3 class="card-title">{{ title }}</h3>
      <p class="card-description">{{ description }}</p>
    </div>
  </div>
</template>

<style scoped>
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
  color: #333;
}

.card-description {
  color: #666;
  line-height: 1.6;
}
</style>
```

### 3.7 Home页面 `src/views/HomeView.vue`

```vue
<script setup lang="ts">
import FeatureCard from '../components/FeatureCard.vue'

const features = [
  {
    title: '组合式API',
    description: '使用Vue 3 Composition API，代码更易组织和复用',
    image: 'https://picsum.photos/300/200?random=1'
  },
  {
    title: '响应式数据',
    description: '自动追踪依赖，数据变化视图自动更新',
    image: 'https://picsum.photos/300/200?random=2'
  },
  {
    title: '单文件组件',
    description: '模板、脚本、样式组合在一个文件中',
    image: 'https://picsum.photos/300/200?random=3'
  }
]
</script>

<template>
  <div class="home">
    <section class="hero">
      <h1>欢迎来到我的网站</h1>
      <p>使用Vue 3构建的现代化网站</p>
    </section>
    
    <section class="features container">
      <h2>特色功能</h2>
      <div class="card-grid">
        <FeatureCard
          v-for="feature in features"
          :key="feature.title"
          v-bind="feature"
        />
      </div>
    </section>
  </div>
</template>

<style scoped>
.hero {
  text-align: center;
  padding: 4rem 0;
  background: linear-gradient(135deg, #42b883 0%, #35495e 100%);
  color: white;
  margin-bottom: 2rem;
}

.hero h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.features {
  padding: 2rem 20px;
}

.features h2 {
  text-align: center;
  margin-bottom: 2rem;
  font-size: 2rem;
  color: #35495e;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}
</style>
```

### 3.8 About页面 `src/views/AboutView.vue`

```vue
<script setup lang="ts">
const techStack = [
  { name: 'Vue 3', description: '渐进式JavaScript框架' },
  { name: 'TypeScript', description: '类型安全的JavaScript超集' },
  { name: 'Vite', description: '下一代前端构建工具' },
  { name: 'Vue Router', description: '官方路由管理器' }
]
</script>

<template>
  <div class="about">
    <h1>关于我们</h1>
    <p class="intro">这是一个使用Vue 3构建的示例网站，展示了现代前端开发的最佳实践。</p>
    
    <section class="tech-section">
      <h2>技术栈</h2>
      <div class="tech-list">
        <div v-for="tech in techStack" :key="tech.name" class="tech-item">
          <h3>{{ tech.name }}</h3>
          <p>{{ tech.description }}</p>
        </div>
      </div>
    </section>
    
    <section class="contact">
      <h2>联系方式</h2>
      <p>邮箱：contact@example.com</p>
      <p>GitHub：github.com/example</p>
    </section>
  </div>
</template>

<style scoped>
.about {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.about h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: #35495e;
}

.intro {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 2rem;
}

.tech-section {
  margin: 2rem 0;
}

.tech-section h2,
.contact h2 {
  color: #42b883;
  margin-bottom: 1rem;
}

.tech-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}

.tech-item {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  border-left: 4px solid #42b883;
}

.tech-item h3 {
  margin-bottom: 0.5rem;
  color: #35495e;
}

.tech-item p {
  color: #666;
  font-size: 0.9rem;
}

.contact {
  margin-top: 2rem;
}

.contact p {
  margin: 0.5rem 0;
  color: #666;
}
</style>
```

### 3.9 全局样式 `src/styles/main.css`

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
  background: #f5f5f5;
}

a {
  color: inherit;
  text-decoration: none;
}

img {
  max-width: 100%;
}
```

## 四、安装依赖

```bash
npm install vue-router
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
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
  plugins: [vue()],
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
VITE_SITE_NAME=My Vue Site
```

使用环境变量：

```ts
const apiUrl = import.meta.env.VITE_API_URL
```

## 下一步

Vue网站开发完成后，继续阅读 [服务器部署指南]({{ site.baseurl }}/02-server-deployment) 学习服务器部署。
