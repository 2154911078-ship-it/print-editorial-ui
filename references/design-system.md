# Print Editorial UI — 设计系统参考

## 色彩体系

### 纸张与油墨（核心）

| Token | 亮色 | 暗色 | 用途 |
|-------|------|------|------|
| `--bg` | `#f5f2eb` | `#121110` | 页面背景 |
| `--bg-2` | `#ece7db` | `#1a1815` | 次级背景、登录右栏 |
| `--panel` | `#f5f2eb` | `#121110` | 卡片、面板 |
| `--panel-2` | `#ece7db` | `#1a1815` | hover 背景 |
| `--panel-3` | `#e2dccd` | `#232019` | 更深一层 |
| `--ink` | `#16150f` | `#f0ece1` | 主文字、重墨 |
| `--text` | `#16150f` | `#f0ece1` | 正文 |
| `--text-2` | `#4b463b` | `#b2ac9d` | 次要文字 |
| `--text-3` | `#8a8373` | `#7c7768` | 辅助文字、编号 |
| `--line` | `#d7d1c2` | `#2e2a23` | 分隔线 |
| `--line-2` | `#c3bcab` | `#423d33` | 虚线、次级线 |
| `--line-strong` | `#16150f` | `#f0ece1` | 重线（报头底边） |

### 朱砂红（唯一强调色）

| Token | 亮色 | 暗色 | 用途 |
|-------|------|------|------|
| `--brand` | `#c1442e` | `#e0603f` | 强调、激活态、hover 指示条 |
| `--brand-2` | `#a3341f` | `#f0795a` | 更深一级 |
| `--brand-soft` | `rgba(193,68,46,0.09)` | `rgba(224,96,63,0.12)` | 淡背景 |

### 语义色（低饱和，不抢戏）

| Token | 亮色 | 暗色 | 用途 |
|-------|------|------|------|
| `--mint` | `#2f5d4a` | `#6fae91` | 成功、上涨 |
| `--rose` | `#a83232` | `#d97b7b` | 危险、下跌 |
| `--amber` | `#8f6112` | `#d0a05a` | 警告 |
| `--green` | `#2f6b4f` | `#6fae91` | 同 mint，用于 delta |
| `--blue` | `#2b4f7a` | `#8ab0d8` | 信息 |
| `--violet` | `#5b4a7a` | `#a596c9` | 特殊 |

## 字体体系

三套字体各司其职：

| Token | 字体栈 | 用途 |
|-------|--------|------|
| `--serif` | `"Songti SC", "Noto Serif SC", "Source Han Serif SC", "STSong", "SimSun", Georgia, "Times New Roman", serif` | 标题、金额、大数字（印刷感） |
| `--sans` | `-apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", "PingFang SC", "Microsoft YaHei", "Noto Sans SC", sans-serif` | 正文、UI 文字 |
| `--mono` | `"SF Mono", "JetBrains Mono", "Roboto Mono", Consolas, "Courier New", monospace` | 编号、SKU、时间、价格 |

字号规范：
- 报头品牌名：27px serif
- 页面标题：24px serif
- 卡片标题：15px sans
- 正文：13.5px sans，行高 1.6
- 金额大数字：40px serif
- 编号/标签：9px mono，letter-spacing 0.12–0.16em

## 布局骨架

```
.app (flex column, 100dvh)
├── .masthead (76px, border-bottom 1.5px solid --ink)
│   ├── .masthead__brand (logo + 副标题)
│   ├── .masthead__nav (横向导航, 可滚动)
│   └── .masthead__meta (时钟 + 操作按钮)
├── .main (flex-1, overflow auto)
│   └── .view (padding: 28px 34px 34px)
│       └── .page-head (编号 + 标题 + eyebrow)
└── .statusbar (26px, border-top 1px solid --line)
```

**关键原则：**
- 没有左侧栏，没有卡片网格
- 所有层级只靠 `1px` 细线与留白区分
- 没有圆角（或极小）、没有投影、没有模糊

## 组件清单

### 按钮 `.btn`
- `.btn--primary`：墨底白字，hover 时底部朱砂条滑入
- `.btn--ghost`：透明底，hover 背景 `--panel-2`
- `.btn--danger`：朱砂底
- `.btn--sm` / `.btn--lg` / `.btn--block`：尺寸变体
- `.btn--loading`：旋转指示器

### 输入框 `.input`
- 无边框底边，只有底部 1px `--line`
- focus 时底部变 `--ink`
- placeholder 用 `--text-3`

### 卡片 `.card`
- 无圆角、无投影
- 顶部可能有 1.5px `--ink` 重线（标题区）
- `.card--fill`：内容撑满，底部对齐

### 表格 `.table`
- thead 底部 1.5px `--ink`
- tbody 行底部 1px `--line`
- hover 时背景 `--panel-2`
- `.cell-strong` 粗体名称 / `.cell-num` 等宽数字右对齐

### 徽章 `.badge`
- 小圆角（2px）或方角
- 背景色 + 文字色配对
- `.badge--open`（灰）/ `.badge--paid`（绿）/ `.badge--void`（红）/ `.badge--brand`（朱砂）

### 分段器 `.seg`
- 无圆角按钮组
- 激活态有底部 2px 朱砂指示条
- 按钮间无间隔，共用边框

### 遮罩层 `.modal` / `.drawer`
- 遮罩：`--overlay` 背景，opacity 过渡
- 面板：无圆角，`.modal` 居中，`.drawer` 右侧滑入
- 边框：1.5px `--ink`（drawer 左侧）

### Toast `.toast`
- 右下角滑入
- 底部有 2px 朱砂进度条（自动关闭倒计时）

## 动效体系

**原则：** 慢、短、硬。不使用弹跳与光效。

| 动画 | 时长 | 说明 |
|------|------|------|
| `mask-up` | 860ms | 文字遮罩上滑，标题入场 |
| `print-in` | 460ms | 小票条目「打印」出来（clip-path + translateY） |
| `fade-up` | 400ms | 页面进入 |
| `bar-grow` | 760ms | 横向条从左侧展开 |
| `bar-grow-y` | 720ms | 纵向柱从底部升起 |
| `draw-stroke` | 1500ms | SVG 折线描边绘制 |
| `data-reveal` | 560ms | 交错入场，delay = i × 60ms |
| `stagger` | 460ms | 子元素交错，delay = i × 38ms |

**View Transition：** 页面切换使用 `document.startViewTransition`，旧页面上滑淡出，新页面从下方淡入。

**降级：** `@media (prefers-reduced-motion: reduce)` 关闭所有动画。

## 响应式断点

| 断点 | 关键变化 |
|------|----------|
| `1240px` | 点单台小票收窄、KPI 数字缩小、三栏变单栏 |
| `1000px` | 报头收紧、隐藏时钟、KPI 变两栏 |
| `860px` | 登录页隐藏左栏、点单台隐藏小票显示底部 dock、表格最小宽度 |
| `640px` | 手机：隐藏导航编号、状态栏隐藏标语和快捷键、分类栏居中 |

## 暗色主题

不是简单的反色：
- 纸 → 墨（`#f5f2eb` → `#121110`）
- 墨 → 纸（`#16150f` → `#f0ece1`）
- 朱砂红保持不变（`#c1442e` → `#e0603f` 略亮）
- 语义色整体提亮
- 通过 `html[data-theme="dark"]` 切换

## 插图系统

见 `assets/art.js`：
- 22 个商品线描 SVG + 5 个分类兜底 + 通用图标
- 统一 48×48 视框、`currentColor` 描边
- 默认 `stroke-width="1.6"`
- 跟随主题色自动变化

## 文件清单

```
assets/styles/
├── tokens.css      # 设计令牌（色彩、字体、 easing、尺寸）
├── base.css        # 重置、布局骨架（报头/主内容/状态栏）
├── components.css  # 通用组件（按钮/输入框/卡片/表格/徽章/分段器/遮罩/Toast）
└── animations.css  # 动效关键帧与入场编排

assets/templates/
└── index.html      # 完整示例页面（KPI/卡片/菜单/表格/按钮）

assets/
└── art.js          # 商品线描插图模块
```
