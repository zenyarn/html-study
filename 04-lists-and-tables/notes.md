# 第四章：列表与表格

## 核心概念

### 1. 三种列表

#### 无序列表 `<ul>` + `<li>`

```html
<ul>
  <li>TypeScript</li>
  <li>React</li>
  <li>Next.js</li>
</ul>
```

**适用场景：** 项目之间没有顺序关系
- 导航菜单（最常见！）
- 功能特性列表
- 标签/技能清单

#### 有序列表 `<ol>` + `<li>`

```html
<ol>
  <li>安装 Node.js</li>
  <li>运行 npm install</li>
  <li>运行 npm run dev</li>
</ol>
```

**适用场景：** 步骤、排名、有先后顺序的内容
- 安装教程
- 排行榜
- 食谱步骤

`<ol>` 的属性：
```html
<ol start="3">        <!-- 从第 3 开始编号 -->
<ol type="A">         <!-- 用字母编号：A, B, C -->
<ol type="i">         <!-- 用罗马数字：i, ii, iii -->
<ol reversed>         <!-- 倒序编号 -->
```

#### 描述列表 `<dl>` + `<dt>` + `<dd>`

```html
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language，超文本标记语言</dd>

  <dt>CSS</dt>
  <dd>Cascading Style Sheets，层叠样式表</dd>
</dl>
```

**适用场景：** 术语-定义对、问答、键值对展示
- 术语词典、FAQ
- 商品规格（尺寸：XL、材质：棉）
- 联系信息（姓名：张伟、职位：前端工程师）

---

### 2. 一个反直觉的真相：导航栏是 `<ul>`

这是实际开发中最常忽视的知识点：

```html
<!-- 绝大多数网站的导航栏都是这样写的 -->
<nav>
  <ul>
    <li><a href="/">首页</a></li>
    <li><a href="/about">关于</a></li>
    <li><a href="/contact">联系</a></li>
  </ul>
</nav>
```

**为什么？** 导航链接是"一组无序的项目"，语义上就是无序列表。通过 CSS 把默认的项目符号和竖向排列改成水平排列，就变成了我们看到的导航栏。

在 React + Tailwind 中：
```tsx
<nav>
  <ul className="flex gap-6 list-none">
    <li><Link href="/">首页</Link></li>
    <li><Link href="/about">关于</Link></li>
  </ul>
</nav>
```

---

### 3. 表格

表格用于展示**结构化的二维数据**，而不是用来做布局（用 CSS Grid/Flexbox 做布局）。

```html
<table>
  <caption>2026 年 Q1 销售数据</caption>
  <thead>
    <tr>
      <th scope="col">月份</th>
      <th scope="col">销售额</th>
      <th scope="col">环比增长</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1 月</td>
      <td>¥120,000</td>
      <td>+5%</td>
    </tr>
    <tr>
      <td>2 月</td>
      <td>¥98,000</td>
      <td>-18%</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>合计</td>
      <td>¥218,000</td>
      <td>—</td>
    </tr>
  </tfoot>
</table>
```

#### 表格结构元素

| 标签 | 作用 |
|------|------|
| `<table>` | 表格容器 |
| `<caption>` | 表格标题（像图片的 figcaption） |
| `<thead>` | 表头区域 |
| `<tbody>` | 表格主体 |
| `<tfoot>` | 表格脚部（合计行等） |
| `<tr>` | 表格行（table row） |
| `<th>` | 表头单元格（table header，默认加粗居中） |
| `<td>` | 数据单元格（table data） |

#### `scope` 属性

```html
<th scope="col">列标题</th>   <!-- 这个 th 是列的标题 -->
<th scope="row">行标题</th>   <!-- 这个 th 是行的标题 -->
```

`scope` 帮助屏幕阅读器理解哪个标题对应哪些数据单元格，是表格可访问性的关键。

#### 合并单元格

```html
<td colspan="2">跨两列</td>    <!-- 横向合并 2 个单元格 -->
<td rowspan="3">跨三行</td>   <!-- 纵向合并 3 个单元格 -->
```

---

### 4. 表格 vs div 布局

**不要用表格做页面布局！** 这是 2000 年代初的过时做法。

```html
<!-- 过时的错误用法：用表格布局页面 -->
<table>
  <tr>
    <td>左边栏</td>
    <td>主内容</td>
    <td>右边栏</td>
  </tr>
</table>

<!-- 现代做法：CSS Flexbox 或 Grid（配合 Tailwind） -->
<div class="flex gap-4">
  <aside class="w-64">左边栏</aside>
  <main class="flex-1">主内容</main>
  <aside class="w-64">右边栏</aside>
</div>
```

---

## React / Next.js 对照

### 用 `.map()` 渲染列表

```tsx
const skills = ["TypeScript", "React", "Next.js", "Tailwind CSS"];

// JSX 中用 .map() 生成列表项
function SkillList() {
  return (
    <ul>
      {skills.map((skill) => (
        <li key={skill}>{skill}</li>
        //  ↑ key 是必须的！React 用它识别哪个元素变化了
      ))}
    </ul>
  );
}
```

**`key` 的规则：**
- 必须在同级元素中唯一（不需要全局唯一）
- 尽量用稳定的 ID，避免用数组 index（排序变化时会出 bug）
- Key 不会传递给子组件（它是 React 内部使用的）

### 渲染表格

```tsx
const data = [
  { month: "1月", sales: 120000 },
  { month: "2月", sales: 98000 },
];

function SalesTable() {
  return (
    <table>
      <thead>
        <tr>
          <th scope="col">月份</th>
          <th scope="col">销售额</th>
        </tr>
      </thead>
      <tbody>
        {data.map((row) => (
          <tr key={row.month}>
            <td>{row.month}</td>
            <td>¥{row.sales.toLocaleString()}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

---

## 本章要点总结

1. `<ul>` 用于无顺序项目（含导航栏），`<ol>` 用于有顺序步骤，`<dl>` 用于术语-定义对
2. 导航栏的正确 HTML 结构是 `<nav><ul><li><a>`
3. 表格用于展示二维数据，不用于页面布局
4. `<th scope="col/row">` 增强表格的可访问性
5. React 中 `.map()` 渲染列表时 `key` 是必须的

---

## 练习说明

打开 `exercise.html`，用列表和表格展示你的技能与学习计划。
