---
name: tomabc-html
description: |
  TomABC.com AI Daily Brief 网站开发和维护技能
  处理 Next.js 前端、HTML 模板生成、样式系统和内容渲染
license: MIT
metadata:
  openclaw:
    requires:
      bins: ["node", "npm"]
    emoji: "🌐"
    os: ["darwin", "linux"]
  author: "yongwang"
  version: "1.0.0"
---

# TomABC HTML - AI Daily Brief 网站开发

TomABC.com 的 AI 每日简报前端项目。支持静态 HTML 原型和 Next.js 应用两种模式。

## 项目结构

```
/Users/yongwang/projects/
├── tomabc-web/          # Next.js 应用（正式版）
│   ├── app/
│   │   ├── layout.tsx   # 根布局
│   │   ├── page.tsx     # 首页（简报列表）
│   │   └── globals.css  # 全局样式
│   ├── components/      # React 组件
│   ├── content/         # 简报内容（Markdown/JSON）
│   ├── public/          # 静态资源
│   └── demo.html        # 静态 HTML 演示版
│
└── tomabc-prototype/    # HTML 原型
    ├── index.html       # 主原型页面
    └── archive.html     # 归档页面
```

## 开发命令

### Next.js 应用（tomabc-web）

```bash
cd /Users/yongwang/projects/tomabc-web
npm run dev    # 启动开发服务器 (http://localhost:3000)
npm run build  # 生产构建
```

### 静态原型（tomabc-prototype）

直接在浏览器中打开 HTML 文件即可预览。

## 设计规范

- 主色调: `#667eea` (靛蓝) → `#764ba2` (紫)
- 字体: -apple-system, sans-serif
- 卡片风格: 圆角 + 毛玻璃效果 (backdrop-filter: blur)
- 响应式: 移动端优先，768px 断点

## 内容格式

简报内容以 Markdown/JSON 格式存放在 `tomabc-web/content/` 目录，渲染为 HTML 卡片布局。
每条简报包含：标题、日期、分类（新闻/安全/行业/技术）、内容正文。
