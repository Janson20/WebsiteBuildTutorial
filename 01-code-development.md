# 代码编写指南

本教程介绍网站项目从零开始创建、开发规范和最佳实践。

## 一、项目初始化

### 1.1 选择技术栈

根据项目需求选择合适的技术：

| 项目类型 | 推荐技术 | 适用场景 |
|---------|---------|---------|
| 静态博客 | Hugo, Hexo, VitePress | 个人博客、文档站 |
| SPA应用 | React, Vue | 交互式Web应用 |
| SSR应用 | Next.js, Nuxt.js | SEO友好的网站 |
| 全栈应用 | Next.js, Nuxt.js | 需要后端API的应用 |

### 1.2 创建项目

#### 使用Vite创建React项目

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

#### 使用Vite创建Vue项目

```bash
npm create vite@latest my-app -- --template vue
cd my-app
npm install
npm run dev
```

#### 创建Next.js项目

```bash
npx create-next-app@latest my-app
cd my-app
npm run dev
```

#### 创建Nuxt.js项目

```bash
npx nuxi@latest init my-app
cd my-app
npm install
npm run dev
```

### 1.3 初始化Git仓库

```bash
git init
git add .
git commit -m "Initial commit"
```

## 二、项目结构规范

### 2.1 React项目结构

```
my-app/
├── public/           # 静态资源
│   └── favicon.ico
├── src/
│   ├── assets/       # 图片、字体等资源
│   ├── components/   # 可复用组件
│   ├── pages/        # 页面组件
│   ├── hooks/        # 自定义Hooks
│   ├── utils/        # 工具函数
│   ├── services/     # API服务
│   ├── styles/       # 全局样式
│   ├── App.jsx       # 根组件
│   └── main.jsx      # 入口文件
├── .gitignore
├── index.html
├── package.json
└── vite.config.js
```

### 2.2 Vue项目结构

```
my-app/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── views/        # 页面视图
│   ├── router/       # 路由配置
│   ├── store/        # 状态管理
│   ├── composables/  # 组合式函数
│   ├── utils/
│   ├── App.vue
│   └── main.js
├── .gitignore
├── index.html
├── package.json
└── vite.config.js
```

### 2.3 Next.js项目结构

```
my-app/
├── app/              # App Router
│   ├── layout.jsx
│   ├── page.jsx
│   └── api/          # API路由
├── components/
├── lib/              # 工具库
├── public/
├── .gitignore
├── package.json
└── next.config.js
```

## 三、代码规范

### 3.1 命名规范

```javascript
// 组件名：PascalCase
function UserProfile() { }

// 变量/函数：camelCase
const userName = 'John';
function getUserData() { }

// 常量：UPPER_SNAKE_CASE
const API_BASE_URL = 'https://api.example.com';

// 文件名：kebab-case
// user-profile.jsx
// api-service.js
```

### 3.2 ESLint配置

```bash
npm install -D eslint @eslint/js
npx eslint --init
```

`.eslintrc.json`示例：

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended"
  ],
  "rules": {
    "indent": ["error", 2],
    "quotes": ["error", "single"],
    "semi": ["error", "always"]
  }
}
```

### 3.3 Prettier配置

```bash
npm install -D prettier
```

`.prettierrc`示例：

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80
}
```

### 3.4 Git提交规范

遵循Conventional Commits：

```
feat: 添加用户登录功能
fix: 修复页面样式问题
docs: 更新README文档
style: 代码格式调整
refactor: 重构用户模块
test: 添加单元测试
chore: 更新构建配置
```

## 四、环境变量管理

### 4.1 创建环境变量文件

```
# .env.development
VITE_API_URL=http://localhost:3000/api
VITE_APP_TITLE=开发环境

# .env.production
VITE_API_URL=https://api.example.com
VITE_APP_TITLE=生产环境
```

### 4.2 使用环境变量

```javascript
// Vite项目
const apiUrl = import.meta.env.VITE_API_URL;

// Next.js项目
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

### 4.3 添加到.gitignore

```gitignore
.env.local
.env.*.local
```

## 五、构建与打包

### 5.1 构建命令

```bash
# Vite项目
npm run build

# Next.js项目
npm run build
```

### 5.2 预览构建结果

```bash
# Vite项目
npm run preview

# Next.js项目
npm run start
```

### 5.3 构建优化

#### 代码分割

```javascript
// React.lazy
const Dashboard = React.lazy(() => import('./pages/Dashboard'));

// Vue路由懒加载
const Dashboard = () => import('./views/Dashboard.vue');
```

#### 图片优化

```javascript
// 使用现代图片格式
<picture>
  <source srcset="/image.webp" type="image/webp">
  <img src="/image.jpg" alt="description">
</picture>
```

## 六、测试

### 6.1 单元测试

```bash
npm install -D vitest @testing-library/react
```

`vitest.config.js`：

```javascript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'jsdom',
  },
});
```

### 6.2 测试示例

```javascript
// Button.test.jsx
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import Button from './Button';

describe('Button', () => {
  it('renders with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
});
```

## 七、常见问题

### Q1: 如何处理跨域问题？

开发环境在`vite.config.js`中配置代理：

```javascript
export default defineConfig({
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
      },
    },
  },
});
```

### Q2: 如何优化打包体积？

- 使用动态导入实现代码分割
- 配置Tree Shaking
- 压缩图片和资源
- 使用CDN加载大型依赖

### Q3: 如何配置路径别名？

```javascript
// vite.config.js
import path from 'path';

export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```

## 下一步

项目开发完成后，继续阅读 [02-server-deployment.md](./02-server-deployment.md) 学习服务器部署。
