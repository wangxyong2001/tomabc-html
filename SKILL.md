---
name: tomabc-html
description: |
  TomABC 官网设计与内容生产标准程序
  三域名架构、视觉系统、HTML 模板、logo 使用规范、学习笔记/视频系列发布
license: MIT
metadata:
  version: "2.2.0"
  author: "yongwang"
  changelog: |
    2.2.0 — 2026-08-13 新增第二套模板：DBS 玻璃感模板（已固定）
      - DBS 玻璃感模板正式登记为第二套模板，样式/logo 与第一套完全独立
      - 模板文件位于 dbs-template/，参考设计稿 5b26b23dd35c6813.png
      - 修复 tomabc-html/assets/ 下旧版灰色 dbs-logo 残留（已覆盖为正确版本）
    2.1.0 — 2026-07-22 Logo 统一
      - 统一为 tomabc.com 像素猫头 + "TomABC" 文字组合
      - 移除 Reverse Grok 横版组合 SVG（与线上不一致）
      - 外链页面用 <img src="/logo.svg">，单文件 HTML 用内联 SVG
---

# TomABC 官网制作流程

## 域名架构

| 域名 | 用途 | VPS 路径 |
|------|------|----------|
| `tomabc.com` | 品牌主站（Hero + 统计数据 + 子站入口） | `/www/wwwroot/www.tomabc.com/` |
| `demo.tomabc.com` | 探索实践（项目展示、视频系列、学习笔记） | `/www/wwwroot/demo.tomabc.com/` |
| `ai.tomabc.com` | AI 内容中心（Daily Brief 等） | `/www/wwwroot/ai.tomabc.com/` |

## 视觉系统

### 颜色

```css
--ground: #F4F2EC;     /* 暖白背景 */
--ground-alt: #F0EDE4;  /* 暖白辅助 */
--ink: #2A2A28;         /* 墨黑文字 */
--ink-soft: #5C5B56;    /* 浅黑 */
--ink-quiet: #8A8984;   /* 灰色 */
--hairline: #D9D6CD;    /* 分隔线 */
--hairline-light: #E8E5DD; /* 浅分隔线 */
--red: #C8161D;         /* 红色点缀 */
--code-bg: #EDEAE0;     /* 代码块背景 */
```

### 字体

系统字体栈，无外部字体依赖：
```css
font-family: system-ui, -apple-system, 'PingFang SC', 'Noto Sans CJK SC', 'Microsoft YaHei', sans-serif;
```

### 排版

- 正文字号: `clamp(15px, 2.3vw, 17px)`
- 行高: `1.9`
- 1px hairline 分隔线
- 无圆角大卡片，直角设计

## Logo 规范

### SVG 像素猫头

与 `tomabc.com` 统一：

```svg
<svg viewBox="0 0 36 36" width="24" height="24" xmlns="http://www.w3.org/2000/svg">
  <rect width="36" height="36" fill="#F4F2EC"/>
  <g shape-rendering="crispEdges">
    <polygon points="6,10 10,2 14,10" fill="#C8161D"/>
    <polygon points="22,10 26,2 30,10" fill="#C8161D"/>
    <rect x="4" y="10" width="28" height="18" rx="3" fill="#2A2A28"/>
    <circle cx="13" cy="18" r="3" fill="#F4F2EC"/>
    <circle cx="23" cy="18" r="3" fill="#F4F2EC"/>
    <circle cx="13" cy="18" r="1.5" fill="#2A2A28"/>
    <circle cx="23" cy="18" r="1.5" fill="#2A2A28"/>
    <polygon points="18,22 16,24 20,24" fill="#C8161D"/>
    <path d="M14 27 Q18 30 22 27" stroke="#F4F2EC" stroke-width="1.2" fill="none" stroke-linecap="round"/>
  </g>
</svg>
```

SVG 文件已部署到三个域名的根路径 `/logo.svg`。页面中引用方式：
- 部署到 VPS 的页面：`<img src="/logo.svg">`
- 单文件 HTML（如学习笔记）：内联 SVG

注意：SVG 内含 `#F4F2EC` 背景矩形，在任何底色上视觉一致。`shape-rendering="crispEdges"` 保持像素风格。

### Logo CSS

与 `tomabc.com` 统一：

```css
.logo{text-decoration:none;color:inherit;display:flex;flex-direction:column;line-height:1.2}
.logo-top{font-size:clamp(26px,3.8vw,32px);letter-spacing:0.04em}
.logo-sub{font-size:12px;letter-spacing:0.18em;color:var(--ink-quiet);font-weight:400}
```

### Logo HTML

与 `tomabc.com` 统一：

```html
<a class="logo" href="https://tomabc.com">
  <span style="display:flex;align-items:center;gap:4px">
    <img src="/logo.svg" width="90" height="90" alt="" style="flex-shrink:0">
    <div>
      <div class="logo-top"><span style="font-weight:500">Tom</span><span style="font-weight:600;color:var(--red)">ABC</span></div>
      <div class="logo-sub">Always Be Curious</div>
    </div>
  </span>
</a>
```

- 像素猫头 90×90（首页），其他页面 81×81
- "Tom" 500 字重 + "ABC" 600 字重红色 `#C8161D`
- "Always Be Curious": 12px，`var(--ink-quiet)`，字距 0.18em
- 图片与文字水平排列，间距 4px

### 单文件 HTML 内联版

学习笔记等离线 HTML 使用内联 SVG 替代外链图片：

```html
<a class="logo" href="https://tomabc.com">
  <span style="display:flex;align-items:center;gap:4px">
    <svg viewBox="0 0 36 36" width="81" height="81" xmlns="http://www.w3.org/2000/svg" style="flex-shrink:0">
      <rect width="36" height="36" fill="#F4F2EC"/>
      <g shape-rendering="crispEdges">
        <polygon points="6,10 10,2 14,10" fill="#C8161D"/>
        <polygon points="22,10 26,2 30,10" fill="#C8161D"/>
        <rect x="4" y="10" width="28" height="18" rx="3" fill="#2A2A28"/>
        <circle cx="13" cy="18" r="3" fill="#F4F2EC"/>
        <circle cx="23" cy="18" r="3" fill="#F4F2EC"/>
        <circle cx="13" cy="18" r="1.5" fill="#2A2A28"/>
        <circle cx="23" cy="18" r="1.5" fill="#2A2A28"/>
        <polygon points="18,22 16,24 20,24" fill="#C8161D"/>
        <path d="M14 27 Q18 30 22 27" stroke="#F4F2EC" stroke-width="1.2" fill="none" stroke-linecap="round"/>
      </g>
    </svg>
    <div>
      <div class="logo-top"><span style="font-weight:500">Tom</span><span style="font-weight:600;color:var(--red)">ABC</span></div>
      <div class="logo-sub">Always Be Curious</div>
    </div>
  </span>
</a>
```

## HTML 模板

### 全局约束

- **纯静态 HTML**，无框架
- **内联 CSS**（不产生额外请求）
- **无 emoji**（所有页面禁止使用 emoji）
- **无 AI 术语**（自然语言描述）
- 统计数据必须**真实准确**

### 页面结构

每个页面遵循：

1. **半透明固定头栏** (`.sticky-top`)
   - nav（logo + 导航链接）
   - 章节标签（如有）
   - 音频栏（如有）

2. **主体内容**
   - h1 标题
   - 内容区（段落、代码块、表格、列表、mermaid 图）

3. **footer** — TomABC / 探索实践 链接

### 导航栏结构

```html
<div class="sticky-top">
  <div class="nav-wrap">
    <a class="logo" href="https://tomabc.com">
      <span style="display:flex;align-items:center;gap:4px">
        <svg viewBox="0 0 36 36" width="81" height="81" xmlns="http://www.w3.org/2000/svg" style="flex-shrink:0">
          <rect width="36" height="36" fill="#F4F2EC"/>
          <g shape-rendering="crispEdges">
            <polygon points="6,10 10,2 14,10" fill="#C8161D"/>
            <polygon points="22,10 26,2 30,10" fill="#C8161D"/>
            <rect x="4" y="10" width="28" height="18" rx="3" fill="#2A2A28"/>
            <circle cx="13" cy="18" r="3" fill="#F4F2EC"/>
            <circle cx="23" cy="18" r="3" fill="#F4F2EC"/>
            <circle cx="13" cy="18" r="1.5" fill="#2A2A28"/>
            <circle cx="23" cy="18" r="1.5" fill="#2A2A28"/>
            <polygon points="18,22 16,24 20,24" fill="#C8161D"/>
            <path d="M14 27 Q18 30 22 27" stroke="#F4F2EC" stroke-width="1.2" fill="none" stroke-linecap="round"/>
          </g>
        </svg>
        <div>
          <div class="logo-top"><span style="font-weight:500">Tom</span><span style="font-weight:600;color:var(--red)">ABC</span></div>
          <div class="logo-sub">Always Be Curious</div>
        </div>
      </span>
    </a>
    <div class="nav-meta">
      <!-- 页面标签 / 元信息，右对齐 -->
    </div>
  </div>
</div>
```

- 与 `tomabc.com` 首页 logo 完全一致：像素猫头 + "Tom" + 红色 "ABC" + "Always Be Curious"
- 外链页面使用 `<img src="/logo.svg">`，单文件 HTML 使用内联 SVG

### 导航栏文案

| 页面 | 导航链接 |
|------|---------|
| 首页 tomabc.com | 探索 / 内容 |
| demo.tomabc.com | 全部 / 框架 / 视频 / 服务 / 设计 / 工具 / 研究 |
| 视频系列页 | 学习笔记 / 系列作品 / 内容 |
| Footer 统一 | 主站 / 探索实践 / 内容 |

### 音频栏

```html
<div class="audio-bar">
  <div class="audio-bar-icon"><svg viewBox="0 0 24 24"><polygon points="5,3 19,12 5,21"/></svg></div>
  <span class="audio-bar-label">配音</span>
  <audio controls preload="none">
    <source src="audio/ch{XX}.mp3" type="audio/mpeg">
  </audio>
</div>
```

### Mermaid 流程图

mermaid 代码块渲染为：
```html
<pre class="mermaid">\n{diagram text}\n</pre>
```

引用 Mermaid.js CDN：
```html
<script src="https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true,theme:'base',themeVariables:{primaryColor:'#EDEAE0',primaryTextColor:'#2A2A28',primaryBorderColor:'#D9D6CD',lineColor:'#C8161D',secondaryColor:'#E8E5DD',secondaryTextColor:'#5C5B56',tertiaryColor:'#F4F2EC',tertiaryTextColor:'#5C5B56',background:'transparent'},fontFamily:'system-ui,-apple-system,sans-serif'});</script>
```

### 响应式

- 640px 断点：导航和 footer 改纵向排列
- 所有间距使用 `clamp()` 函数

## 第二套模板：DBS 玻璃感模板（v1.0 · 已固定）

> 2026-08-13 固定。与第一套（TomABC）**完全独立**：样式文件、logo、设计令牌均不共享。DBS 页面禁止混用 TomABC 资源（含像素猫 logo、`--ground` 暖白令牌、`#C8161D` 红色）。

### 定位

DBS 品牌页面（论文精读、深度内容）。视觉风格：粉紫渐变背景 + 彩色光斑 + 半透明白色毛玻璃卡片。参考设计稿：`5b26b23dd35c6813.png`。

### 设计令牌

```css
--ink:#2A2A28; --ink-soft:#4A4A48; --ink-quiet:#8A8984;
--red:#E50014;                     /* DBS 红，唯一强调色 */
--glass:rgba(255,255,255,0.55);    /* 玻璃卡片底 */
--glass-strong:rgba(255,255,255,0.75);
--glass-border:rgba(255,255,255,0.7);
--shadow:0 10px 40px rgba(120,90,160,0.12);
--radius:20px;
```

背景：`linear-gradient(180deg,#F2DDE0 0%,#E6DDEB 30%,#E4E3EF 55%,#F7F5F8 80%,#FFFFFF 100%)` + `background-attachment:fixed`；body 首部放 3 个固定彩色光斑 `.blob`（`filter:blur(90px)`，颜色 `#F2B8C4 / #C9BCE8 / #F4D3B0`）。

### 页面骨架

1. 3 个 `.blob` 光斑（`<body>` 首个子元素）
2. 玻璃导航 `.glass-nav`（`rgba(255,255,255,0.45)` + `backdrop-filter:blur(18px) saturate(160%)`，内含 `.nav-wrap` + logo；**无其他导航链接**）
3. 主体 `.page`：每个 h2 章节包在 `.glass-card` 内（`rgba(255,255,255,0.55)` + `blur(20px)` + 圆角 20px + 内侧高光）
4. `.footer`

### Logo 规范（DBS）

- 文件：`dbs-template/dbs-logo.png`（透明底，内容红色 `#E50014` 不透明）/ `dbs-logo.b64`（内联 base64）/ `dbs-logo.jpg`（原图）
- 单文件 HTML：`<img src="data:image/png;base64,{b64}" alt="DBS" style="width:auto;height:clamp(48px,7vh,64px);flex-shrink:0">`
- 部署页面：`<img src="/dbs-logo.png">`
- **保持原色，禁止灰度化 / 变透明**；logo 图片底部小字已裁除，不得重新添加

### 样式文件规则

- 单文件 HTML：内联 `dbs-template/dbs.css` 全部内容到 `<style>`
- 多页面站点：`<link rel="stylesheet" href="/dbs.css">`
- 二选一，禁止同时使用；`dbs.css` 与第一套样式完全独立，不得互相引用

### 全局约束

与第一套一致：无 emoji、无 AI 术语、统计数据真实准确。

### 模板文件（dbs-template/）

| 文件 | 用途 |
|------|------|
| `dbs.css` | 独立玻璃风样式表 |
| `dbs-logo.png / .b64 / .jpg` | DBS logo（透明底 / 内联 base64 / 原图） |
| `encrypted-reasoning-blobs.html` | 锁定示例（论文精读页，已发布页完整副本，含 10 张玻璃卡片） |

## 内容类型

### 视频系列页面

路径: `demo.tomabc.com/videos/<series-name>/`

每系列包含：
- `index.html` — 播放页面（前端框架构建或纯 HTML）
- `audio/` — 配音 MP3 文件
- 无视频 MP4（仅演示页面 + 音频）

### 学习笔记（HTML + 音频）

路径: `demo.tomabc.com/cc-scratch/` 等

批量生成流程：
1. 源文件：Markdown
2. 转换：Python 脚本 `cc-scratch-builder.py` → HTML
3. 音频：`edge-tts --voice zh-CN-XiaoxiaoNeural --file <narration.txt> --write-media <output.mp3>`
4. 口播稿需口语化，跳过代码参数名
5. **敏感词过滤（发布前必做）**：`python3 tools/sensitive_filter/sensitive_filter.py check <内容目录>`
   - 无命中 → 直接发布
   - 有命中 → `clean` 出清洗版 + 报告 → 人工确认后发布清洗版（原文件不动）
   - 误伤治理：把误伤词追加到 `tools/sensitive_filter/dict/whitelist/tech.txt`（每行一词）后重跑（详见该工具 README）
6. 部署：rsync 到 VPS `/www/wwwroot/demo.tomabc.com/<path>/`
7. 更新索引页，标记就绪章节

### 口播配音规则

- 语音: `zh-CN-XiaoxiaoNeural`
- 单章一个完整 MP3（不按 step 拆分）
- 跳过代码块中的参数名和下划线变量名
- 章节转换处加自然停顿

## 部署

```bash
# SSH 配置（密钥认证, 禁止密码回退）
Host tomabc.com
    HostName tomabc.com
    User tomabc
    Port 60022
    IdentityFile ~/.ssh/id_ed25519_vps

# 上传文件
rsync -avz <local-path> tomabc.com:<remote-path>
```

VPS 配置文件位于 `/www/wwwroot/` 下按域名分目录。

## 参考

- [TomABC 官网设计定型记忆](../../.claude/projects/-home-nvidia-workspace-claude/memory/tomabc-website-final-design.md)
