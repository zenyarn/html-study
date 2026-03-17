# 第五章：表单

## 核心概念

表单是用户与网页交互的主要方式——登录、注册、搜索、下单，都是表单。深入理解表单 HTML，是后续在 React 中处理表单状态的基础。

---

### 1. 表单基础结构

```html
<form action="/api/register" method="POST">
  <!-- 表单控件 -->
  <button type="submit">提交</button>
</form>
```

| 属性 | 作用 |
|------|------|
| `action` | 表单数据提交的 URL |
| `method` | HTTP 方法：`GET`（数据附在 URL）或 `POST`（数据在请求体） |

**在 React/Next.js 中：**
- 通常不写 `action` 和 `method`，由 JS 拦截提交事件处理
- Next.js 14+ 的 Server Actions 则会重新用到 `action` 属性

---

### 2. `<label>` 是最容易被忽视的重要标签

```html
<!-- 错误：没有 label，点击文字无法聚焦到输入框 -->
用户名：<input type="text" name="username" />

<!-- 正确方式一：for + id 关联（推荐） -->
<label for="username">用户名：</label>
<input type="text" id="username" name="username" />

<!-- 正确方式二：label 包裹 input -->
<label>
  用户名：
  <input type="text" name="username" />
</label>
```

**为什么 label 很重要？**
1. **可访问性**：屏幕阅读器读到 input 时会先读出对应的 label
2. **点击区域扩大**：点击 label 文字，对应的 input 自动获得焦点
3. **移动端友好**：更大的点击目标，减少误触

**在 React 中：`for` → `htmlFor`**
```tsx
<label htmlFor="username">用户名：</label>
<input type="text" id="username" name="username" />
```

---

### 3. `<input>` 的各种 type

```html
<!-- 文本类 -->
<input type="text" />           <!-- 普通文本 -->
<input type="email" />          <!-- 邮箱（自动格式验证） -->
<input type="password" />       <!-- 密码（字符隐藏） -->
<input type="search" />         <!-- 搜索框（显示清除按钮） -->
<input type="tel" />            <!-- 电话（移动端显示数字键盘） -->
<input type="url" />            <!-- URL（自动格式验证） -->
<input type="number" />         <!-- 数字（可设 min/max/step） -->

<!-- 选择类 -->
<input type="checkbox" />       <!-- 复选框 -->
<input type="radio" />          <!-- 单选按钮 -->
<input type="range" />          <!-- 滑块 -->
<input type="color" />          <!-- 颜色选择器 -->

<!-- 时间日期类 -->
<input type="date" />           <!-- 日期选择 -->
<input type="time" />           <!-- 时间选择 -->
<input type="datetime-local" /> <!-- 日期+时间 -->

<!-- 文件/按钮类 -->
<input type="file" />           <!-- 文件上传 -->
<input type="submit" />         <!-- 提交按钮 -->
<input type="reset" />          <!-- 重置按钮 -->
<input type="hidden" />         <!-- 隐藏字段（传额外数据） -->
```

**使用正确的 type 的好处：**
- 移动端显示对应键盘（email type → 显示 @ 键）
- 浏览器内置基础格式验证
- 语义清晰，辅助技术正确处理

---

### 4. 常用表单属性

```html
<input
  type="text"
  id="username"
  name="username"          <!-- 提交时的字段名，必须有 -->
  placeholder="请输入用户名"  <!-- 占位提示文字（不替代 label） -->
  value="默认值"            <!-- 初始值 -->
  required                 <!-- 必填（省略值等同于 required="required"） -->
  minlength="3"            <!-- 最少字符数 -->
  maxlength="20"           <!-- 最多字符数 -->
  pattern="[A-Za-z0-9]+"  <!-- 正则验证 -->
  disabled                 <!-- 禁用 -->
  readonly                 <!-- 只读 -->
  autocomplete="username"  <!-- 自动填充提示 -->
/>
```

**`placeholder` ≠ `label`**

这是非常常见的错误：
```html
<!-- 错误：placeholder 当 label 用 -->
<input type="email" placeholder="你的邮箱" />
<!-- 问题：用户开始输入后占位文字消失，忘记这个字段是什么 -->
<!-- 屏幕阅读器对 placeholder 的支持不稳定 -->

<!-- 正确 -->
<label for="email">邮箱地址</label>
<input type="email" id="email" placeholder="例：hello@example.com" />
<!-- placeholder 提供示例格式，label 提供字段含义 -->
```

---

### 5. 单选与复选

```html
<!-- 单选按钮（同组用相同 name） -->
<fieldset>
  <legend>性别</legend>
  <label>
    <input type="radio" name="gender" value="male" /> 男
  </label>
  <label>
    <input type="radio" name="gender" value="female" /> 女
  </label>
  <label>
    <input type="radio" name="gender" value="other" /> 其他
  </label>
</fieldset>

<!-- 复选框 -->
<fieldset>
  <legend>兴趣爱好</legend>
  <label>
    <input type="checkbox" name="hobby" value="coding" /> 编程
  </label>
  <label>
    <input type="checkbox" name="hobby" value="reading" /> 阅读
  </label>
</fieldset>
```

`<fieldset>` + `<legend>` 的作用：
- `<fieldset>`：将相关的表单控件分组
- `<legend>`：这个分组的标题
- 语义上告诉屏幕阅读器"这几个控件是相关的"
- 对 radio/checkbox 分组尤其重要

---

### 6. 下拉菜单与文本域

```html
<!-- 下拉菜单 -->
<label for="city">城市</label>
<select id="city" name="city">
  <option value="">-- 请选择 --</option>
  <optgroup label="华北">
    <option value="beijing">北京</option>
    <option value="tianjin">天津</option>
  </optgroup>
  <optgroup label="华东">
    <option value="shanghai" selected>上海</option>
    <option value="hangzhou">杭州</option>
  </optgroup>
</select>

<!-- 多行文本域 -->
<label for="bio">个人简介</label>
<textarea
  id="bio"
  name="bio"
  rows="5"
  cols="40"
  placeholder="介绍一下自己..."
  maxlength="500"
></textarea>
<!-- textarea 用结束标签，不是自闭合标签 -->
<!-- 初始值放在标签之间，不用 value 属性 -->
```

---

## React / Next.js 对照

### 受控组件 vs 非受控组件

这是 React 表单中最核心的概念，原生 HTML 的理解是基础：

```tsx
// 非受控组件（Native HTML 方式，React 不管理状态）
function UncontrolledForm() {
  const inputRef = useRef<HTMLInputElement>(null);
  const handleSubmit = () => {
    console.log(inputRef.current?.value); // 直接读 DOM
  };
  return <input type="text" ref={inputRef} />;
}

// 受控组件（React 管理状态，推荐）
function ControlledForm() {
  const [value, setValue] = useState("");
  return (
    <input
      type="text"
      value={value}                    // 值由 state 控制
      onChange={(e) => setValue(e.target.value)} // 同步更新 state
    />
  );
}
```

### React Hook Form（实际项目常用）

实际项目通常用 React Hook Form 库，它结合了两种方式的优点：
```tsx
import { useForm } from "react-hook-form";

function RegisterForm() {
  const { register, handleSubmit } = useForm();
  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <label htmlFor="email">邮箱</label>
      <input id="email" type="email" {...register("email", { required: true })} />
      <button type="submit">注册</button>
    </form>
  );
}
// 注意：即使用了库，HTML 的 label/id/name/type 结构依然重要
```

### Next.js Server Actions

```tsx
// Next.js 14+ 可以用 Server Actions 直接提交到服务端
async function registerAction(formData: FormData) {
  "use server";
  const email = formData.get("email");
  // 服务端处理...
}

function RegisterForm() {
  return (
    <form action={registerAction}>  {/* action 重新派上用场了！ */}
      <input type="email" name="email" required />
      <button type="submit">注册</button>
    </form>
  );
}
```

---

## 本章要点总结

1. 每个 input 都必须有对应的 label（可访问性 + 用户体验）
2. 使用正确的 input type 触发对应移动端键盘和浏览器验证
3. `placeholder` 是辅助提示，不能替代 label
4. radio/checkbox 分组用 `<fieldset>` + `<legend>`
5. `name` 属性是表单提交的字段名，必须有
6. React 中 `for` → `htmlFor`，受控组件是推荐方式

---

## 练习说明

打开 `exercise.html`，用语义化的 HTML 写一个完整的注册表单，包含用户名、邮箱、密码、性别选择、兴趣爱好和个人简介。
