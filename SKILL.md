---
name: print-editorial-ui
description: |
  一套「纸与墨」的印刷版面风格前端设计系统。刻意避开深色卡片、玻璃模糊、
  渐变按钮、大圆角等通用后台模板语汇，改从纸质菜单与编辑部版面取形：
  象牙纸底色、墨黑文字、朱砂红单一强调色。所有层级只靠 1px 细线与留白区分，
  没有圆角、没有投影、没有模糊。包含完整 CSS 令牌、布局骨架、通用组件、
  动效体系、响应式断点、暗色主题、以及 22 个单色线描 SVG 插图。
agent_created: true
---

# Print Editorial UI

## 用途

当用户需要：
- 搭建一套**不撞脸通用 admin 模板**的 Web 界面
- 做**门店经营系统、后台管理、数据看板、点单收银**等场景
- 追求**纸质印刷感**的排版（菜单、报纸、杂志风格）
- 需要**暗色主题**且不是简单反色
- 需要一套**统一的商品/分类线描插图**

时，使用本 Skill。

## 核心原则

1. **底色是纸**：`#f5f2eb`（象牙白），不是纯白也不是深灰
2. **文字是墨**：`#16150f`，带一点暖调的墨黑
3. **唯一强调色是朱砂红**：`#c1442e`，全站只此一个跳色
4. **没有圆角、没有投影、没有模糊、没有渐变背景**
5. **层级只靠 1px 细线 + 留白**
6. **三套字体各司其职**：宋体（标题/金额）、无衬线（正文）、等宽（编号/价格）
7. **横向报头 + 底部状态栏**：替代左侧栏 + 卡片网格

## 快速开始

复制 `assets/styles/` 下的四个 CSS 文件到项目，按顺序引入：

```html
<link rel="stylesheet" href="tokens.css">   <!-- 必须先 -->
<link rel="stylesheet" href="base.css">     <!-- 布局骨架 -->
<link rel="stylesheet" href="components.css"><!-- 通用组件 -->
<link rel="stylesheet" href="animations.css"><!-- 动效 -->
```

然后复制 `assets/templates/index.html` 作为起点，按需删减。

暗色主题切换：

```js
document.documentElement.dataset.theme = 'dark';
```

## 设计令牌（tokens.css）

所有视觉值都收敛在 CSS 变量里，改一行即可全局生效。

**色彩：** `--bg`, `--bg-2`, `--panel`, `--ink`, `--text`, `--text-2`, `--text-3`, `--line`, `--line-2`, `--brand`, `--brand-soft`, `--mint`, `--rose`, `--amber`...

**字体：** `--serif`（宋体栈）、`--sans`（系统无衬线）、`--mono`（等宽）

**动效：** `--ease-out` (cubic-bezier 0.22,1,0.36,1), `--ease-spring`, `--dur-fast` (140ms), `--dur` (280ms), `--dur-slow` (520ms)

**尺寸：** `--masthead-h` (76px), `--status-h` (26px), `--max-w` (1520px)

完整令牌表见 `references/design-system.md`。

## 布局骨架（base.css）

```
.app
├── .masthead      报头：品牌名 + 横向导航 + 时钟/操作
├── .main          主内容区（内部滚动）
│   └── .view      视图容器
│       └── .page-head  页头：编号 + 标题 + eyebrow
└── .statusbar     底部状态栏
```

**`.masthead`**：高度 76px，底部 1.5px 重线（`--ink`），flex 横向排列。
品牌名用 serif 27px，导航项带编号（`.mnav__no`，mono 字体）。

**`.main`**：`flex: 1`，`overflow: auto`，内部滚动不带动外层。

**`.statusbar`**：高度 26px，顶部 1px 细线，flex 横向，左右分布信息项。
中间用 `.statusbar__spacer` 撑开。

## 组件（components.css）

| 类名 | 说明 |
|------|------|
| `.btn` / `.btn--primary` / `.btn--ghost` / `.btn--danger` | 按钮，primary 有底部朱砂条滑入效果 |
| `.input` | 输入框，无边框只有底部线 |
| `.card` / `.card--fill` | 卡片，无圆角，fill 让内容撑满 |
| `.table` / `.table-wrap` | 表格，thead 重线，tbody 细线，hover 变色 |
| `.badge` / `.badge--paid` / `.badge--open` / `.badge--void` | 状态徽章 |
| `.seg` / `.seg__btn` | 分段器，无圆角，激活态底部朱砂条 |
| `.modal` / `.modal__panel` / `.scrim` | 弹窗遮罩 |
| `.drawer` | 右侧抽屉，左侧 1.5px 重线边框 |
| `.toast` / `.toasts` | 右下角通知，带朱砂倒计时条 |
| `.field` / `.field__label` | 表单字段 |
| `.empty` | 空状态 |

## 动效（animations.css）

| 类/关键帧 | 效果 | 时长 |
|-----------|------|------|
| `.reveal-line > span` | 文字遮罩上滑 | 860ms |
| `.stagger > *` | 子元素交错淡入 | 460ms，delay = i×38ms |
| `[data-reveal].is-in` | IntersectionObserver 触发淡入 | 560ms，delay = i×60ms |
| `print-in` | 小票条目「打印」效果 | 460ms |
| `bar-grow` | 横向条从左侧展开 | 760ms |
| `bar-grow-y` | 纵向柱从底部升起 | 720ms |
| `draw-stroke` | SVG 描边绘制 | 1500ms |
| `.view-enter` | 页面进入 | 400ms |
| `::view-transition-old/new(root)` | 页面切换过渡 | 200ms/400ms |

**入场编排：** 给需要交错的元素加 `style="--i:0"` `style="--i:1"` 等，配合 `.stagger` 父容器或 `[data-reveal]` + IntersectionObserver。

**降级：** `prefers-reduced-motion: reduce` 关闭所有动画。

## 响应式

| 断点 | 变化 |
|------|------|
| `1240px` | 三栏变单栏、小票收窄 |
| `1000px` | 报头收紧、隐藏时钟、KPI 变两栏 |
| `860px` | 登录页隐藏左栏、点单台小票→底部 dock |
| `640px` | 手机：隐藏导航编号、状态栏精简 |

## 暗色主题

通过 `html[data-theme="dark"]` 切换。不是简单反色：
- 纸 `#f5f2eb` → 墨 `#121110`
- 墨 `#16150f` → 纸 `#f0ece1`
- 朱砂红保持（略亮 `#e0603f`）
- 语义色整体提亮

## 插图系统（art.js）

`assets/art.js` 提供：

```js
import { productArt, categoryArt } from './art.js';

// 按商品名匹配插图
const svg = productArt('燕麦拿铁', '咖啡');
// 返回：<svg viewBox="0 0 48 48"...>...</svg>

// 按分类兜底
categoryArt('甜点');
```

- 22 个商品图形（咖啡杯、茶壶、汉堡、蛋糕等）
- 5 个分类兜底图形
- 1 个通用兜底
- 统一 48×48 视框、`currentColor` 描边、跟随主题

## 示例页面

`assets/templates/index.html` 包含完整示例：
- 报头 + 导航 + 时钟
- KPI 四栏数字卡片
- 三栏布局（带竖线分隔）
- 菜单列表（含线描插图）
- 数据表格（含徽章）
- 按钮组 + 分段器
- 底部状态栏
- 主题切换 + 实时时钟

## 注意事项

1. **必须先引入 tokens.css**，其他文件依赖其中的 CSS 变量
2. 字体栈依赖系统字体，如需保证宋体效果，建议引入 `"Noto Serif SC"` Web Font
3. `body::before` 有极淡的稿纸网格背景，如不需要可删除
4. 暗色主题需要手动设置 `html[data-theme="dark"]`，不会自动跟随系统
5. View Transition API 需要浏览器支持，不支持的浏览器自动降级为普通淡入
