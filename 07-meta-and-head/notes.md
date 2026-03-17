# 第七章：`<head>` 与 SEO / 性能

## 核心概念

前六章我们把 `<head>` 里的内容一笔带过。这章深入学习它——因为在 Next.js 全栈开发中，`<head>` 的管理是一个专门的话题，直接影响产品的 SEO 表现和加载性能。

---

### 1. `<head>` 的完整职责

```html
<head>
  <!-- 1. 字符编码（必须第一个）-->
  <meta charset="UTF-8" />

  <!-- 2. 视口配置 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- 3. 页面标题（SEO 最重要因素之一）-->
  <title>文章标题 | 网站名称</title>

  <!-- 4. 页面描述 -->
  <meta name="description" content="..." />

  <!-- 5. Open Graph（社交分享预览）-->
  <meta property="og:title" content="..." />
  <meta property="og:description" content="..." />
  <meta property="og:image" content="..." />
  <meta property="og:url" content="..." />

  <!-- 6. Twitter Card -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="..." />

  <!-- 7. Canonical URL（防止内容重复）-->
  <link rel="canonical" href="https://example.com/page" />

  <!-- 8. 图标 -->
  <link rel="icon" href="/favicon.ico" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

  <!-- 9. 样式表 -->
  <link rel="stylesheet" href="/styles.css" />

  <!-- 10. 预加载关键资源 -->
  <link rel="preload" href="/fonts/inter.woff2" as="font" crossorigin />

  <!-- 11. JavaScript（通常放 body 底部或用 defer）-->
  <script src="/app.js" defer></script>
</head>
```

---

### 2. `<title>` 标签

```html
<title>如何学好 HTML 语义化 | ZhangWei.dev</title>
```

**SEO 最佳实践：**
- 每页唯一，直接描述页面内容
- 格式：`页面内容 | 品牌名`（品牌名放后面）
- 长度：50~60 个字符（超出搜索引擎会截断）
- 包含目标关键词（靠前放）

---

### 3. Description meta

```html
<meta name="description" content="本文详解 HTML 语义化标签的使用场景，帮助你在 React/Next.js 中写出更好的组件结构。" />
```

- 显示在搜索结果的摘要文字
- 不直接影响排名，但影响点击率（CTR）
- 长度：120~160 个字符
- 每页唯一，有实质内容

---

### 4. Open Graph 协议

Open Graph 是 Facebook 制定、现已成为事实标准的社交分享元数据格式。当你分享链接到微信、Twitter、Slack 时，显示的预览卡片就来自 OG 标签。

```html
<meta property="og:type" content="article" />         <!-- 内容类型 -->
<meta property="og:title" content="文章标题" />        <!-- 卡片标题 -->
<meta property="og:description" content="摘要" />      <!-- 卡片描述 -->
<meta property="og:image" content="https://example.com/og-image.jpg" />  <!-- 预览图 -->
<meta property="og:url" content="https://example.com/article-slug" />    <!-- 规范 URL -->
<meta property="og:site_name" content="ZhangWei.dev" />                  <!-- 网站名 -->
<meta property="og:locale" content="zh_CN" />                            <!-- 语言 -->
```

**OG 图片要求：**
- 推荐尺寸：1200×630px
- 格式：JPEG 或 PNG
- 文件大小：< 8MB

```html
<!-- Twitter/X 卡片 -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@yourhandle" />
<meta name="twitter:title" content="文章标题" />
<meta name="twitter:description" content="描述" />
<meta name="twitter:image" content="https://example.com/twitter-image.jpg" />
```

---

### 5. Canonical URL

```html
<link rel="canonical" href="https://example.com/article/html-semantics" />
```

**解决什么问题？**

同一内容可能有多个 URL 访问：
- `https://example.com/article/html-semantics`
- `https://example.com/article/html-semantics?ref=twitter`
- `https://example.com/article/html-semantics/`（尾斜杠）

搜索引擎会认为这是三个不同页面，分散页面权重（"duplicate content" 问题）。Canonical 告诉搜索引擎："不管通过哪个 URL 访问，这个是权威 URL"。

---

### 6. 图标

```html
<!-- 浏览器标签页图标 -->
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="icon" href="/favicon.svg" type="image/svg+xml" />

<!-- iOS 主屏幕图标 -->
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />

<!-- PWA manifest -->
<link rel="manifest" href="/manifest.json" />
```

**现代推荐：**
- 使用 SVG favicon（支持深色模式适配）
- 配合 `favicon.ico` 兼容旧浏览器

---

### 7. 样式表加载

```html
<!-- 外部 CSS：阻塞渲染（浏览器先下载 CSS 再渲染） -->
<link rel="stylesheet" href="/styles.css" />

<!-- 预连接到 CDN（减少 DNS + TLS 时间） -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />

<!-- 预加载关键资源（字体、首屏图片等） -->
<link rel="preload" href="/fonts/inter-v13-latin-regular.woff2" as="font" type="font/woff2" crossorigin />
```

---

### 8. JavaScript 的 `defer` 和 `async`

```html
<!-- 阻塞解析（最慢，避免用） -->
<script src="/app.js"></script>

<!-- defer：下载不阻塞，等 HTML 解析完后按顺序执行 -->
<script src="/app.js" defer></script>

<!-- async：下载不阻塞，下载完立即执行（不保证顺序） -->
<script src="/analytics.js" async></script>
```

| 属性 | 下载 | 执行时机 | 顺序保证 | 适合场景 |
|------|------|----------|----------|----------|
| 无 | 阻塞 | 立即 | ✓ | 不推荐 |
| `defer` | 不阻塞 | DOMContentLoaded 前 | ✓ | 大多数脚本 |
| `async` | 不阻塞 | 下载完立即 | ✗ | 独立脚本（埋点、广告） |

**实际开发中：** 在 Next.js 里，你几乎不需要手写 script 标签，框架自动处理。但理解这些知识对性能优化和调试很重要。

---

## React / Next.js 对照

### Next.js 中管理 `<head>` 的两种方式

#### Pages Router（旧版）

```tsx
import Head from "next/head";

export default function BlogPost({ post }) {
  return (
    <>
      <Head>
        <title>{`${post.title} | ZhangWei.dev`}</title>
        <meta name="description" content={post.excerpt} />
        <meta property="og:title" content={post.title} />
        <meta property="og:image" content={post.coverImage} />
        <link rel="canonical" href={`https://example.com/blog/${post.slug}`} />
      </Head>
      <article>...</article>
    </>
  );
}
```

#### App Router（推荐，Next.js 13+）

```tsx
// app/blog/[slug]/page.tsx
import type { Metadata } from "next";

// 静态 metadata
export const metadata: Metadata = {
  title: "文章标题 | ZhangWei.dev",
  description: "文章描述",
};

// 动态 metadata（从数据库或 API 获取）
export async function generateMetadata({ params }): Promise<Metadata> {
  const post = await getPost(params.slug);
  return {
    title: `${post.title} | ZhangWei.dev`,
    description: post.excerpt,
    openGraph: {
      title: post.title,
      description: post.excerpt,
      images: [{ url: post.coverImage, width: 1200, height: 630 }],
      url: `https://example.com/blog/${post.slug}`,
    },
    twitter: {
      card: "summary_large_image",
      title: post.title,
    },
    alternates: {
      canonical: `https://example.com/blog/${post.slug}`,
    },
  };
}

export default function BlogPost() {
  return <article>...</article>;
}
```

Next.js App Router 的 `generateMetadata` 函数会自动将配置对象转换为正确的 HTML `<meta>` 标签，同时处理去重、覆盖逻辑。

**这就是为什么要学好原生 HTML meta 标签——Next.js 只是把它们封装了，理解底层才能用好上层。**

---

## 本章要点总结

1. `<title>` 每页唯一，50~60 字符，包含关键词
2. `description` 影响搜索点击率，120~160 字符
3. Open Graph 标签控制社交分享时的预览卡片效果
4. Canonical 解决 URL 重复问题，保护页面权重
5. `defer` vs `async`：优先用 defer，独立脚本（埋点）用 async
6. Next.js App Router 用 `generateMetadata` 管理动态 meta

---

## 练习说明

打开 `exercise.html`，为你在第六章练习中写的博客页面，补充完整的 `<head>` 内容，包括 title、description、og 标签和 canonical。
