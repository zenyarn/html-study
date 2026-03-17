# 第一章：HTML 文档结构

## 核心概念

### 1. 什么是 HTML？

HTML（HyperText Markup Language）不是编程语言，而是**标记语言**。它描述内容的结构和语义，而不是逻辑。类比你已知的语言：

- C++/Python：描述"做什么"（逻辑）
- HTML：描述"是什么"（结构和含义）
- CSS：描述"长什么样"（样式）
- JS/TS：描述"怎么动"（行为）

---

### 2. 标准文档骨架解析

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>页面标题</title>
  </head>
  <body>
    <!-- 页面内容 -->
  </body>
</html>
```

**逐行解释：**

#### `<!DOCTYPE html>`
- 这不是标签，是文档类型声明
- 告诉浏览器："请用 HTML5 标准解析这个文件"
- 如果省略，浏览器会进入"怪异模式"（Quirks Mode），样式渲染会出现不可预期的差异
- **记住：永远放在第一行，没有例外**

#### `<html lang="zh-CN">`
- 根元素，所有内容必须在它内部
- `lang` 属性声明页面主要语言，用于：
  - 屏幕阅读器正确发音
  - 搜索引擎了解目标语言
  - 浏览器翻译功能的触发依据

#### `<head>` 标签
- 包含"关于这个页面"的信息，但**不显示在页面上**
- 给浏览器、搜索引擎、社交平台读的元数据

#### `<meta charset="UTF-8">`
- 声明字符编码，告诉浏览器"用 UTF-8 解读这个文件的字节"
- UTF-8 支持几乎所有语言字符（中文、日文、emoji 等）
- **必须放在 `<head>` 最开始**，否则在 title 解析前可能出现乱码
- 类比：这相当于告诉文本编辑器"用 UTF-8 打开这个文件"

#### `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
- 控制移动端浏览器的视口行为
- 没有这行：手机浏览器默认模拟 980px 宽的桌面，页面会被缩小显示
- 有这行：视口宽度等于设备宽度，`initial-scale=1.0` 表示不缩放
- **对响应式设计来说，这行是必须的**

#### `<title>`
- 显示在浏览器标签页上的文字
- 对 SEO 影响极大（搜索结果标题来源之一）
- 每个页面应有唯一、描述性的 title

#### `<body>` 标签
- 用户实际看到的所有内容都放在这里

---

### 3. DOM 树

HTML 解析后会生成 **DOM（Document Object Model）树**，这是一棵真正的树形数据结构：

```
Document
└── html
    ├── head
    │   ├── meta (charset)
    │   ├── meta (viewport)
    │   └── title
    └── body
        └── ...内容节点
```

这和你写 C++ 时的树形数据结构是完全一样的概念。浏览器通过操作这棵树来渲染和更新页面。

---

### 4. 元素 vs 标签

- **标签（Tag）**：`<p>`、`</p>` 这样的语法标记
- **元素（Element）**：开始标签 + 内容 + 结束标签 = 一个完整的元素

```html
<p>这是一个段落元素</p>
<!-- 开始标签 ^   ^ 结束标签 -->
```

**自闭合标签**（void elements）没有内容和结束标签：
```html
<meta charset="UTF-8" />
<img src="photo.jpg" alt="照片" />
<br />
<input type="text" />
```

---

## React / Next.js 对照

### Next.js 的 `app/layout.tsx` 就是 HTML 骨架

在 Next.js App Router 中，根布局文件的作用与 HTML 骨架几乎完全对应：

```tsx
// app/layout.tsx（Next.js）
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="zh-CN">        {/* 对应 <html lang="zh-CN"> */}
      <head />                 {/* Next.js 会自动注入 charset 和 viewport */}
      <body>
        {children}             {/* 对应 <body> 内容 */}
      </body>
    </html>
  );
}
```

Next.js 会自动处理 `charset` 和 `viewport` meta 标签，但你仍然需要理解它们的原理，因为：
1. 有时需要自定义（比如多语言网站切换 `lang`）
2. 调试移动端问题时需要理解 viewport 的工作方式
3. 面试时这是基础考点

### React 的虚拟 DOM vs HTML 的真实 DOM

React 在内存中维护一棵**虚拟 DOM 树**，与 HTML 解析生成的真实 DOM 树结构相同，但更新效率更高。理解 DOM 树结构，就能理解 React 为什么这样组织组件。

### JSX 中的差异

React JSX 与 HTML 几乎相同，但有几处重要区别：
```tsx
// HTML
<div class="container">

// JSX（React）
<div className="container">   // class → className（因为 class 是 JS 保留字）

// HTML
<label for="username">

// JSX
<label htmlFor="username">    // for → htmlFor（同样原因）
```

---

## 本章要点总结

1. `<!DOCTYPE html>` 必须在第一行
2. `<html lang="">` 声明语言，影响可访问性和 SEO
3. `<meta charset="UTF-8">` 必须在 head 最前面
4. viewport meta 是移动端响应式的基础
5. HTML 解析为 DOM 树，React 虚拟 DOM 是同样的概念

---

## 练习说明

打开 `exercise.html`，根据 `TODO` 注释，从零手写一个标准的 HTML 文档骨架。不要复制 demo，尝试默写——这是真正记住结构的最快方式。
