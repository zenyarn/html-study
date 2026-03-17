# 第三章：链接与图片

## 核心概念

### 1. `<a>` 超链接

`<a>`（anchor，锚点）是 Web 最核心的元素之一——正是超链接构成了"超文本"网络。

```html
<a href="https://example.com">点击跳转</a>
```

#### 常用属性

| 属性 | 作用 | 示例 |
|------|------|------|
| `href` | 链接目标 URL | `href="https://..."` |
| `target` | 在哪里打开链接 | `target="_blank"` |
| `rel` | 与目标页面的关系 | `rel="noopener noreferrer"` |
| `download` | 触发下载而非导航 | `download="文件名"` |

#### `target="_blank"` 的安全问题

```html
<!-- 危险写法：新标签页打开，但目标页面能通过 window.opener 访问当前页 -->
<a href="https://example.com" target="_blank">跳转</a>

<!-- 安全写法：必须同时加 rel -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">跳转</a>
```

`rel="noopener noreferrer"` 做了两件事：
- `noopener`：切断新标签页与当前页的 `window.opener` 引用（防止恶意网站篡改当前页）
- `noreferrer`：不发送 `Referer` 请求头（保护用户隐私，对方无法知道用户从哪跳来）

**记住：只要用 `target="_blank"`，就必须同时加 `rel="noopener noreferrer"`。** 这是前端安全的基础常识。

#### href 的几种形式

```html
<!-- 外部链接 -->
<a href="https://github.com">GitHub</a>

<!-- 站内绝对路径 -->
<a href="/about">关于我们</a>

<!-- 站内相对路径 -->
<a href="../contact.html">联系</a>

<!-- 锚点链接（跳转到页面内的某个位置） -->
<a href="#section-2">跳到第二节</a>
<!-- 目标元素需要有对应的 id -->
<h2 id="section-2">第二节</h2>

<!-- 邮件链接 -->
<a href="mailto:hello@example.com">发邮件</a>

<!-- 电话链接（移动端会拨打） -->
<a href="tel:+8613800138000">拨打电话</a>

<!-- 空链接（占位，JS 处理点击事件）—— 不推荐 -->
<a href="#">点击</a>
<!-- 更好的方式：如果不是导航，用 button 而不是 a -->
```

---

### 2. `<img>` 图片

```html
<img src="photo.jpg" alt="一只橘猫坐在窗台上" width="400" height="300" />
```

#### `alt` 属性为什么重要？

`alt`（alternative text）是图片无法加载或用户使用屏幕阅读器时的替代文字。

**三种情况：**
```html
<!-- 有意义的图片：写描述性 alt -->
<img src="cat.jpg" alt="一只橘猫坐在窗台上，阳光斜射进来" />

<!-- 纯装饰性图片：alt 为空字符串（不要省略 alt，要写空字符串）-->
<img src="decorative-line.svg" alt="" />
<!-- 空字符串告诉屏幕阅读器"跳过这张图，不需要读" -->
<!-- 省略 alt 则屏幕阅读器会读出文件名，非常难听 -->

<!-- 作为按钮的图片：alt 写按钮功能 -->
<a href="/home"><img src="logo.png" alt="首页" /></a>
```

**坏的 alt：**
```html
<img src="photo.jpg" alt="图片" />         <!-- 无意义 -->
<img src="photo.jpg" alt="photo.jpg" />    <!-- 读文件名 -->
<img src="photo.jpg" />                    <!-- 省略 alt，SEO 和可访问性双输 -->
```

#### `width` 和 `height` 属性的作用

```html
<img src="photo.jpg" alt="..." width="400" height="300" />
```

**为什么要写 width 和 height？**

浏览器在图片加载完成之前不知道图片的尺寸，会导致**布局抖动（Layout Shift）**——页面先渲染好，图片加载后撑开空间，整个布局跳动一下。

指定 width/height 后，浏览器提前预留好空间，避免抖动。这直接影响 **CLS（Cumulative Layout Shift）** 指标，是 Google Core Web Vitals 的重要评分项。

---

### 3. `<figure>` 与 `<figcaption>`

```html
<figure>
  <img src="chart.png" alt="2026 年 Q1 用户增长曲线，三月份达到峰值" />
  <figcaption>图 1：2026 年第一季度用户增长数据</figcaption>
</figure>
```

- `<figure>`：表示"独立的、可引用的内容块"（图表、代码块、引用等）
- `<figcaption>`：figure 的说明/标题

语义价值：搜索引擎和屏幕阅读器知道图片和说明文字是配套的。

---

### 4. 图片格式选择

| 格式 | 适合场景 | 说明 |
|------|----------|------|
| JPEG/JPG | 照片、复杂图像 | 有损压缩，文件小，不支持透明 |
| PNG | 需要透明背景的图、截图 | 无损压缩，文件较大 |
| **WebP** | 几乎所有场景 | 现代格式，比 JPEG/PNG 小 25~35% |
| **SVG** | 图标、Logo、矢量图 | 可无限缩放，文件极小 |
| GIF | 简单动画 | 已被 WebP 动画取代，尽量不用 |

**实际工作建议：**
- 图标/Logo → SVG（或用图标库如 Lucide React）
- 照片 → WebP（Next.js 的 `<Image>` 组件会自动转换）
- 截图 → PNG（或 WebP）

---

## React / Next.js 对照

### Next.js `<Link>` vs 原生 `<a>`

```tsx
// 原生 HTML
<a href="/about">关于</a>
// 每次点击都是完整的页面加载（浏览器请求新页面）

// Next.js Link 组件
import Link from "next/link";
<Link href="/about">关于</Link>
// 客户端导航：只加载差异部分，无整页刷新
// 自动预加载（用户鼠标悬停时提前请求数据）
// 底层渲染为 <a> 标签，所有 HTML 语义保留
```

**何时用哪个：**
- 站内跳转 → 始终用 `<Link>`（性能更好）
- 外部链接 → 用原生 `<a href="..." target="_blank" rel="noopener noreferrer">`

### Next.js `<Image>` vs 原生 `<img>`

```tsx
// 原生 HTML（需要手动处理一切）
<img src="/photo.jpg" alt="..." width={400} height={300} />

// Next.js Image 组件（自动处理）
import Image from "next/image";
<Image
  src="/photo.jpg"
  alt="..."
  width={400}
  height={300}
/>
```

Next.js `<Image>` 自动做到：
1. **格式转换**：自动转为 WebP/AVIF
2. **懒加载**：页面外的图片延迟加载
3. **尺寸优化**：按设备分辨率提供合适尺寸
4. **防止布局抖动**：强制要求 width/height

**理解了原生 `<img>` 的知识，才能理解 `<Image>` 在背后做了什么优化。**

---

## 本章要点总结

1. `target="_blank"` 必须配合 `rel="noopener noreferrer"` 使用
2. `alt` 属性不能省略，纯装饰图用 `alt=""`，有内容的图写描述性 alt
3. 写 `width` 和 `height` 可以防止布局抖动（CLS 优化）
4. `<figure>` + `<figcaption>` 是图片与说明的正确语义组合
5. 生产环境优先用 WebP 格式，图标用 SVG

---

## 练习说明

打开 `exercise.html`，搭建一个包含导航链接和配图的个人主页结构。
