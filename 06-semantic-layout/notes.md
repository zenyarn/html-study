# 第六章：语义化布局（核心重点章）

## 为什么这章是重点？

前五章学的是"局部语义"——某个词语、某张图片、某个输入框是什么。这章学的是"整体语义"——整个页面的结构是什么，各部分的角色是什么。

在 React/Next.js 中，每个组件都对应一个 HTML 结构单元。能否正确使用语义布局标签，决定了你的组件结构是否清晰、可维护、对 SEO 友好。

---

## 核心语义布局标签

### 总览

```html
<body>
  <header>          <!-- 页面/章节的页眉 -->
    <nav>           <!-- 导航区域 -->
  </header>

  <main>            <!-- 页面的主要内容（每页只有一个） -->
    <article>       <!-- 独立的完整内容块（文章、帖子、评论） -->
      <section>     <!-- 主题相关的内容分组 -->
    </article>
    <aside>         <!-- 与主内容相关但非核心的边栏内容 -->
  </main>

  <footer>          <!-- 页面/章节的页脚 -->
</body>
```

---

### `<header>`

```html
<!-- 页面级 header：包含 logo、站点导航、登录入口 -->
<header>
  <a href="/" aria-label="回到首页">
    <img src="/logo.svg" alt="我的博客" width="120" height="40" />
  </a>
  <nav>...</nav>
</header>

<!-- 文章级 header：包含文章标题、作者、发布时间 -->
<article>
  <header>
    <h1>如何学好 HTML 语义化</h1>
    <p>作者：张伟 | <time datetime="2026-03-17">2026 年 3 月 17 日</time></p>
  </header>
  <p>文章正文...</p>
</article>
```

`<header>` 不只是页面的顶部，它可以出现在 `<article>`、`<section>` 内部，表示该区块的头部。

---

### `<nav>`

```html
<nav aria-label="主导航">
  <ul>
    <li><a href="/">首页</a></li>
    <li><a href="/blog">博客</a></li>
    <li><a href="/about">关于</a></li>
  </ul>
</nav>

<!-- 页面内可以有多个 nav，用 aria-label 区分 -->
<nav aria-label="文章目录">
  <ol>
    <li><a href="#intro">简介</a></li>
    <li><a href="#basics">基础知识</a></li>
  </ol>
</nav>
```

`<nav>` 用于**主要的导航链接组**，不是每个链接都需要放在 nav 里（如页脚的版权链接就不用）。

---

### `<main>`

```html
<main>
  <!-- 页面的主要内容 -->
</main>
```

**规则：**
- 每个页面**只能有一个 `<main>`**
- 不能嵌套在 `<header>`、`<footer>`、`<nav>`、`<article>`、`<aside>` 内
- 跳转链接（"跳过导航，直接到主内容"）的目标就是 `<main>`

---

### `<article>`

```html
<article>
  <header>
    <h2>TypeScript 入门指南</h2>
    <p>作者：张伟 | <time datetime="2026-03-17">2026-03-17</time></p>
  </header>
  <p>正文内容...</p>
  <footer>
    <p>标签：<a href="/tag/typescript">TypeScript</a></p>
  </footer>
</article>
```

`<article>` 的判断标准：**"这段内容独立拿出去，单独看还有完整意义吗？"**

适合：
- 博客文章、新闻稿
- 论坛帖子、评论
- 产品卡片（独立的产品描述）
- 小组件（天气卡、股票卡）

---

### `<section>`

```html
<article>
  <h1>全栈开发学习路线</h1>

  <section>
    <h2>第一阶段：前端基础</h2>
    <p>HTML、CSS、JavaScript...</p>
  </section>

  <section>
    <h2>第二阶段：框架学习</h2>
    <p>React、Next.js...</p>
  </section>
</article>
```

`<section>` 的判断标准：**"这是文档中一个有主题的章节，需要有标题。"**

- `<section>` 通常需要有一个标题（h2~h6）
- 如果内容只是"视觉分区"（用于加背景色等），用 `<div>`
- 不要用 `<section>` 作为通用包裹容器（那是 `<div>` 的职责）

---

### `<aside>`

```html
<main>
  <article>
    <!-- 主要文章内容 -->
  </article>

  <aside>
    <h2>相关文章</h2>
    <ul>
      <li><a href="#">CSS 布局完全指南</a></li>
      <li><a href="#">JavaScript 进阶技巧</a></li>
    </ul>

    <h2>关于作者</h2>
    <p>张伟，全栈开发者，专注 Web 开发...</p>
  </aside>
</main>
```

`<aside>` 包含与主内容相关但不是核心的内容：
- 侧边栏（相关文章、标签云）
- 广告
- 作者简介
- 注释、说明框

---

### `<footer>`

```html
<!-- 页面级 footer -->
<footer>
  <p>© 2026 张伟的博客. 保留所有权利。</p>
  <nav aria-label="页脚链接">
    <a href="/privacy">隐私政策</a>
    <a href="/terms">服务条款</a>
  </nav>
</footer>

<!-- 文章级 footer -->
<article>
  <p>文章正文...</p>
  <footer>
    <p>最后更新：<time datetime="2026-03-17">2026-03-17</time></p>
    <p>分类：<a href="/category/html">HTML</a></p>
  </footer>
</article>
```

---

## `<div>` soup 问题

"div soup"（div 汤）指的是到处都是无语义 `<div>` 的代码，是初学者最常见的问题：

```html
<!-- div soup：机器无法理解结构 -->
<div class="header">
  <div class="logo">我的博客</div>
  <div class="nav">
    <div class="nav-item"><a href="/">首页</a></div>
  </div>
</div>
<div class="content">
  <div class="post">
    <div class="post-title">文章标题</div>
    <div class="post-body">内容...</div>
  </div>
</div>
<div class="sidebar">...</div>
<div class="footer">版权信息</div>

<!-- 语义化：清晰、可访问、SEO 友好 -->
<header>
  <a href="/"><img src="/logo.svg" alt="我的博客" /></a>
  <nav><ul>...</ul></nav>
</header>
<main>
  <article>
    <h1>文章标题</h1>
    <p>内容...</p>
  </article>
  <aside>...</aside>
</main>
<footer>版权信息</footer>
```

---

## ARIA 基础

ARIA（Accessible Rich Internet Applications）为 HTML 补充语义：

```html
<!-- aria-label：给没有文字内容的元素添加标签 -->
<button aria-label="关闭对话框">✕</button>

<!-- aria-labelledby：指向另一个元素作为当前元素的标签 -->
<section aria-labelledby="features-title">
  <h2 id="features-title">功能特性</h2>
  ...
</section>

<!-- aria-describedby：指向描述性文字 -->
<input type="password" aria-describedby="pwd-requirements" />
<p id="pwd-requirements">密码至少 8 位，包含大小写字母和数字。</p>

<!-- role：覆盖默认语义（谨慎使用） -->
<div role="alert">操作成功！</div>
<!-- 屏幕阅读器遇到 role="alert" 会立即播报 -->
```

**原则：优先使用语义标签（它们内置了正确的 ARIA 角色），只在语义标签不够用时才添加 ARIA 属性。**

---

## React / Next.js 对照

### Next.js App Router 的布局与语义标签

```tsx
// app/layout.tsx（根布局）
export default function RootLayout({ children }) {
  return (
    <html lang="zh-CN">
      <body>
        <header>
          <nav>...</nav>
        </header>
        <main>{children}</main>  {/* children 是每个页面的主内容 */}
        <footer>...</footer>
      </body>
    </html>
  );
}

// app/blog/[slug]/page.tsx（文章页）
export default function BlogPost() {
  return (
    <article>
      <header>
        <h1>{post.title}</h1>
        <time datetime={post.date}>{formatDate(post.date)}</time>
      </header>
      <section>{post.content}</section>
    </article>
  );
}
```

### 组件命名应反映语义结构

```tsx
// 不好：命名与 HTML 结构脱节
const TopBar = () => <div>...</div>;
const ContentWrapper = () => <div>...</div>;
const SidePanel = () => <div>...</div>;

// 好：组件名 = 语义标签的含义
const Header = () => <header>...</header>;
const MainContent = () => <main>...</main>;
const Sidebar = () => <aside>...</aside>;
const Footer = () => <footer>...</footer>;

// 文章组件
const BlogPostCard = () => (
  <article>
    <h2>文章标题</h2>
    <p>摘要...</p>
  </article>
);
```

### Tailwind 配合语义标签

```tsx
// 不好：所有布局用 div + className
<div className="bg-gray-900 text-white px-6 py-4 flex items-center">
  <div className="flex gap-6">...</div>
</div>

// 好：语义 + 样式分离
<header className="bg-gray-900 text-white px-6 py-4 flex items-center">
  <nav className="flex gap-6">...</nav>
</header>
```

---

## 典型页面布局图

### 博客文章页

```
┌─────────────────────────────────────────┐
│  <header>                               │
│    logo    <nav>首页 博客 关于</nav>      │
├────────────────────────┬────────────────┤
│  <main>                │  <aside>       │
│    <article>           │                │
│      <header>          │  相关文章       │
│        <h1>标题</h1>   │  作者简介       │
│        发布时间         │  标签云         │
│      </header>         │                │
│      正文内容...        │                │
│      <footer>          │                │
│        标签 分类        │                │
│      </footer>         │                │
│    </article>          │                │
│                        │                │
│    <section>评论区</section>            │
├────────────────────────┴────────────────┤
│  <footer>                               │
│    版权 | 隐私政策 | 联系我              │
└─────────────────────────────────────────┘
```

---

## 本章要点总结

1. `<main>` 每页只有一个，包含页面核心内容
2. `<article>` 用于独立完整的内容（能单独存在有意义）
3. `<section>` 用于有主题的章节，通常需要标题
4. `<aside>` 用于相关但非核心的内容（侧边栏、补充说明）
5. `<header>` 和 `<footer>` 可以出现在 article/section 内部
6. 避免 div soup，用语义标签让结构自我说明
7. React 组件命名应与语义结构对应

---

## 练习说明

打开 `exercise.html`，用语义布局标签搭建一个完整的博客文章页面，包含页眉导航、主内容区（文章+评论区）、侧边栏和页脚。
