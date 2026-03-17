# HTML 基础学习项目

本项目是一份系统性的 HTML 学习路线，面向已有编程基础（JS/TS）、目标技术栈为 **TypeScript + React + Next.js + Tailwind CSS** 的开发者。

每章包含三个文件：
- `demo.html` — 示例代码，含详细注释，直接用浏览器打开查看效果
- `exercise.html` — 练习文件，根据 `TODO` 提示自己动手编写
- `notes.md` — 知识点讲解 + React/Next.js 实际场景对照

---

## 学习路线

| 章节 | 主题 | 核心概念 |
|------|------|----------|
| [01 - 文档结构](./01-document-structure/) | HTML 文档骨架 | DOCTYPE、head/body、DOM 树、viewport |
| [02 - 文本语义](./02-text-semantics/) | 文本相关标签 | h1~h6、语义 vs 表现、SEO |
| [03 - 链接与图片](./03-links-and-images/) | 超链接与图片 | a/img、alt、Next.js Image 对照 |
| [04 - 列表与表格](./04-lists-and-tables/) | 结构化数据 | ul/ol/table、React .map() 对照 |
| [05 - 表单](./05-forms/) | 用户输入 | input/label/form、受控组件对照 |
| [**06 - 语义化布局**](./06-semantic-layout/) | **页面结构（重点）** | **header/main/article、组件命名** |
| [07 - head 与 SEO](./07-meta-and-head/) | 元数据与性能 | meta/og 标签、Next.js metadata API |

---

## 使用方式

1. 按章节顺序学习，先读 `notes.md` 了解概念
2. 打开 `demo.html`（用浏览器）查看实际效果
3. 在 `exercise.html` 中根据 `TODO` 提示自己动手写
4. 完成练习后，在浏览器中对照效果

> **推荐工具：** VS Code + Live Server 插件，保存后自动刷新浏览器

---

## 最终目标

完成全部章节后，你将能够：
- 理解并手写标准的语义化 HTML 页面
- 在 React/Next.js 组件中自然地使用正确的 HTML 结构
- 理解 HTML 语义化对 SEO、可访问性和代码维护性的意义
- 为学习 CSS（Tailwind）和 React 组件化打下坚实基础
