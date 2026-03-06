---
layout: default
title: 网站性能优化
nav_order: 6
parent: 拓展内容
---

# 网站性能优化指南

本教程介绍如何对网站进行全方位的性能优化，提升用户体验和SEO排名。

## 一、性能指标

### 1.1 核心Web指标 (Core Web Vitals)

| 指标 | 全称 | 含义 | 良好标准 |
|------|------|------|---------|
| LCP | Largest Contentful Paint | 最大内容绘制时间 | ≤ 2.5秒 |
| INP | Interaction to Next Paint | 交互到下次绘制 | ≤ 200ms |
| CLS | Cumulative Layout Shift | 累积布局偏移 | ≤ 0.1 |

### 1.2 其他重要指标

| 指标 | 含义 | 良好标准 |
|------|------|---------|
| FCP | 首次内容绘制 | ≤ 1.8秒 |
| TTFB | 首字节时间 | ≤ 800ms |
| TTI | 可交互时间 | ≤ 3.8秒 |

### 1.3 性能测试工具

```
在线工具：
- PageSpeed Insights: https://pagespeed.web.dev/
- WebPageTest: https://www.webpagetest.org/
- GTmetrix: https://gtmetrix.com/

浏览器工具：
- Chrome DevTools (Lighthouse)
- Firefox Developer Tools
- WebKit Web Inspector
```

## 二、资源加载优化

### 2.1 图片优化

#### 选择正确的格式

```
JPEG - 照片、复杂图像
PNG  - 需要透明背景的图像
WebP - 现代格式，体积更小（推荐）
AVIF - 最新格式，压缩率更高
SVG  - 图标、Logo、简单图形
```

#### 图片压缩

```bash
# 使用命令行工具
npm install -g imagemin-cli

# 压缩图片
imagemin images/* --out-dir=dist/images

# 转换为WebP
npx sharp -i input.png -o output.webp
```

#### 响应式图片

```html
<picture>
  <source 
    srcset="image-small.webp 400w,
            image-medium.webp 800w,
            image-large.webp 1200w"
    sizes="(max-width: 600px) 400px,
           (max-width: 1000px) 800px,
           1200px"
    type="image/webp">
  <source 
    srcset="image-small.jpg 400w,
            image-medium.jpg 800w,
            image-large.jpg 1200w"
    sizes="(max-width: 600px) 400px,
           (max-width: 1000px) 800px,
           1200px">
  <img src="image-medium.jpg" alt="描述" loading="lazy">
</picture>
```

#### 懒加载

```html
<!-- 原生懒加载 -->
<img src="image.jpg" loading="lazy" alt="描述">

<!-- 懒加载背景图 -->
<div class="lazy-bg" data-bg="image.jpg"></div>

<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const bg = entry.target.dataset.bg;
      entry.target.style.backgroundImage = `url(${bg})`;
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('.lazy-bg').forEach(el => observer.observe(el));
</script>
```

### 2.2 CSS优化

#### 关键CSS内联

```html
<head>
  <style>
    /* 内联关键CSS */
    .header { background: #1a1a2e; }
    .hero { min-height: 100vh; }
  </style>
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

#### CSS压缩

```bash
# 使用cssnano
npm install cssnano-cli -g
cssnano styles.css styles.min.css

# 使用PurgeCSS移除未使用的CSS
npx purgecss --css styles.css --content index.html --output styles.min.css
```

### 2.3 JavaScript优化

#### 代码分割

```javascript
// 动态导入
const module = await import('./heavy-module.js');

// React懒加载
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

// Vue异步组件
const AsyncComponent = defineAsyncComponent(() => 
  import('./AsyncComponent.vue')
);
```

#### 延迟加载非关键JS

```html
<!-- defer：HTML解析完成后执行 -->
<script src="script.js" defer></script>

<!-- async：下载完成后立即执行 -->
<script src="analytics.js" async></script>

<!-- 动态加载 -->
<script>
  window.addEventListener('load', () => {
    const script = document.createElement('script');
    script.src = 'non-critical.js';
    document.body.appendChild(script);
  });
</script>
```

#### Tree Shaking

```javascript
// 正确的ES模块导出
export function usedFunction() { }
export function unusedFunction() { }

// 导入时只导入需要的
import { usedFunction } from './module';

// 避免默认导出整个对象
// 不推荐
export default { usedFunction, unusedFunction };

// 推荐
export { usedFunction, unusedFunction };
```

### 2.4 字体优化

```html
<!-- 预加载关键字体 -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>

<!-- 使用font-display -->
<style>
@font-face {
  font-family: 'MyFont';
  src: url('font.woff2') format('woff2');
  font-display: swap; /* 或 optional, fallback */
}
</style>

<!-- 限制字体变体数量 -->
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
```

## 三、缓存策略

### 3.1 浏览器缓存

Nginx配置：

```nginx
# 静态资源长期缓存
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}

# HTML文件不缓存
location ~* \.html$ {
    expires -1;
    add_header Cache-Control "no-cache, no-store, must-revalidate";
}

# API请求不缓存
location /api/ {
    add_header Cache-Control "no-cache, no-store, must-revalidate";
}
```

### 3.2 文件版本控制

```html
<!-- 使用文件哈希 -->
<link rel="stylesheet" href="styles.a1b2c3d4.css">
<script src="app.e5f6g7h8.js"></script>

<!-- 构建工具自动处理 -->
<!-- Vite/Webpack会自动添加哈希 -->
```

### 3.3 Service Worker缓存

```javascript
// 注册Service Worker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js');
  });
}

// sw.js - Service Worker
const CACHE_NAME = 'v1';
const ASSETS = [
  '/',
  '/styles.css',
  '/app.js'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(ASSETS))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then(response => response || fetch(event.request))
  );
});
```

## 四、CDN加速

### 4.1 CDN原理

```
用户请求 → CDN边缘节点 → 源站服务器
              ↓
         缓存命中则直接返回
```

### 4.2 Cloudflare缓存规则

```
页面规则配置：

规则1 - 静态资源
URL: *your-domain.com/static/*
- 缓存级别: 缓存所有内容
- 边缘缓存TTL: 1个月
- 浏览器缓存TTL: 4小时

规则2 - HTML页面
URL: *your-domain.com/*.html
- 缓存级别: 绕过
- 浏览器缓存TTL: 4小时

规则3 - API
URL: *your-domain.com/api/*
- 缓存级别: 绕过
```

### 4.3 使用第三方CDN

```html
<!-- 从CDN加载常用库 -->
<script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.prod.js"></script>
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>

<!-- 预连接CDN域名 -->
<link rel="preconnect" href="https://cdn.jsdelivr.net">
<link rel="dns-prefetch" href="https://unpkg.com">
```

## 五、服务器优化

### 5.1 启用Gzip/Brotli压缩

```nginx
# Gzip压缩
gzip on;
gzip_vary on;
gzip_min_length 1024;
gzip_proxied any;
gzip_comp_level 6;
gzip_types text/plain text/css text/xml application/json application/javascript application/xml;

# Brotli压缩（需安装ngx_brotli模块）
brotli on;
brotli_comp_level 6;
brotli_types text/plain text/css text/xml application/json application/javascript application/xml;
```

### 5.2 HTTP/2推送

```nginx
server {
    listen 443 ssl http2;
    
    # HTTP/2服务器推送
    http2_push /styles.css;
    http2_push /app.js;
    
    # 或使用Link头
    add_header Link "</styles.css>; rel=preload; as=style";
}
```

### 5.3 减少请求次数

```nginx
# 合并小文件（使用CSS Sprites或内联）

# 内联小图片（Base64）
<style>
.icon {
  background-image: url('data:image/svg+xml;base64,...');
}
</style>

# 使用SVG Sprites
<svg class="icon">
  <use xlink:href="#icon-name"></use>
</svg>
```

## 六、代码层面优化

### 6.1 减少重排重绘

```javascript
// 批量修改DOM
// 不推荐
element.style.width = '100px';
element.style.height = '100px';
element.style.margin = '10px';

// 推荐
element.classList.add('active');

// 使用DocumentFragment
const fragment = document.createDocumentFragment();
items.forEach(item => {
  fragment.appendChild(createElement(item));
});
container.appendChild(fragment);

// 使用requestAnimationFrame
function animate() {
  requestAnimationFrame(() => {
    // 动画代码
  });
}
```

### 6.2 虚拟列表

```javascript
// 长列表虚拟滚动
class VirtualList {
  constructor(container, itemHeight, renderItem) {
    this.container = container;
    this.itemHeight = itemHeight;
    this.renderItem = renderItem;
    this.visibleItems = Math.ceil(container.clientHeight / itemHeight);
    
    container.addEventListener('scroll', this.onScroll.bind(this));
  }
  
  onScroll() {
    const scrollTop = this.container.scrollTop;
    const startIndex = Math.floor(scrollTop / this.itemHeight);
    this.render(startIndex);
  }
  
  render(startIndex) {
    // 只渲染可见项
  }
}
```

### 6.3 防抖与节流

```javascript
// 防抖
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// 节流
function throttle(fn, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// 使用示例
window.addEventListener('scroll', throttle(handleScroll, 100));
searchInput.addEventListener('input', debounce(search, 300));
```

## 七、性能监控

### 7.1 Performance API

```javascript
// 页面加载性能
const timing = performance.timing;
const metrics = {
  dns: timing.domainLookupEnd - timing.domainLookupStart,
  tcp: timing.connectEnd - timing.connectStart,
  ttfb: timing.responseStart - timing.requestStart,
  download: timing.responseEnd - timing.responseStart,
  domReady: timing.domContentLoadedEventEnd - timing.navigationStart,
  load: timing.loadEventEnd - timing.navigationStart
};

// Web Vitals
import { getCLS, getLCP, getINP } from 'web-vitals';

getCLS(console.log);
getLCP(console.log);
getINP(console.log);
```

### 7.2 性能预算

```json
// performance-budget.json
{
  "budgets": [{
    "resourceType": "script",
    "budget": 300
  }, {
    "resourceType": "stylesheet",
    "budget": 100
  }, {
    "resourceType": "image",
    "budget": 500
  }, {
    "resourceType": "total",
    "budget": 1000
  }]
}
```

## 八、优化检查清单

### 首屏优化

- [ ] 关键CSS内联
- [ ] 图片懒加载
- [ ] 预加载关键资源
- [ ] 减少阻塞资源
- [ ] 压缩所有资源

### 加载优化

- [ ] 启用Gzip/Brotli
- [ ] 使用CDN
- [ ] 设置缓存策略
- [ ] HTTP/2多路复用
- [ ] Service Worker缓存

### 运行优化

- [ ] 减少重排重绘
- [ ] 使用虚拟列表
- [ ] 合理使用防抖节流
- [ ] Web Worker处理复杂计算
- [ ] 避免内存泄漏

### 监控优化

- [ ] 配置性能监控
- [ ] 设置性能预算
- [ ] 定期性能测试
- [ ] 用户行为分析
- [ ] 错误追踪

## 下一步

性能优化完成后，继续阅读 [SEO优化指南]({{ site.baseurl }}/06b-seo-optimization) 学习搜索引擎优化。
