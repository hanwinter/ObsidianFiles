# Vue / Node.js / 前端工程化学习总结

## 一、前端开发的本质

传统前端：

```text
HTML + CSS + JavaScript
```

作用：

|技术|作用|
|---|---|
|HTML|页面结构|
|CSS|页面样式|
|JavaScript|页面行为与交互|

本质上：

- HTML/CSS/JS 文件只是文本文件
    
- 最终由浏览器解析执行
    

---

# 二、现代前端为什么需要 Vue？

传统开发的问题：

- DOM 操作复杂
    
- 页面逻辑混乱
    
- 代码难维护
    
- 组件无法复用
    

因此出现：

- Vue.js
    

Vue 的核心能力：

|能力|说明|
|---|---|
|组件化|页面拆分成组件|
|响应式|数据变化自动更新UI|
|虚拟DOM|高效更新页面|
|单文件组件|template/script/style统一管理|

Vue 本质：

> 一个运行在浏览器中的前端框架

---

# 三、Vue 的真正运行位置

Vue 最终运行在：

```text
浏览器 JavaScript 引擎
```

例如：

|浏览器|JS引擎|
|---|---|
|Chrome|V8|
|Edge|V8|
|Firefox|SpiderMonkey|

因此：

```text
Vue 最终运行时 = 浏览器
```

不是 Node.js。

---

# 四、Node.js 是什么？

- Node.js
    

定义：

> JavaScript 的服务器端运行环境

本质：

```text
Node.js = V8引擎 + 操作系统API
```

作用：

- 在浏览器外运行 JavaScript
    
- 提供文件系统、网络、进程等能力
    

---

# 五、“服务器端”是什么意思？

“服务器端”不是指某台特殊机器。

真正含义：

```text
承担服务角色的一侧
```

典型 C/S 模型：

```text
浏览器（客户端）
        ↓ 请求
Node.js服务（服务器端）
        ↓ 返回数据
浏览器渲染
```

核心区别：

|客户端|服务器端|
|---|---|
|主动发请求|被动监听请求|
|展示UI|处理业务|
|浏览器沙盒|可访问系统资源|

---

# 六、Node.js 在 Vue 开发中的作用

Node.js 主要负责：

## 1. 项目构建

例如：

```bash
npm run serve
```

背后：

```text
Node.js
    ↓
启动 Vue CLI / Webpack / Vite
    ↓
编译 .vue 文件
    ↓
生成浏览器可运行代码
```

---

## 2. 包管理

Node.js 提供：

|工具|作用|
|---|---|
|npm|包管理|
|pnpm|高性能包管理|
|yarn|npm替代方案|

---

## 3. 开发服务器

例如：

```bash
npm run serve
```

会启动：

```text
localhost:8080
```

用于本地开发。

---

# 七、Vue CLI 是什么？

- Vue CLI
    

本质：

> 一个运行在 Node.js 上的 JavaScript 工具

命令：

```bash
vue create my-project
```

执行过程：

```text
PowerShell
    ↓
vue.cmd
    ↓
node.exe
    ↓
执行 Vue CLI JS代码
    ↓
生成项目
```

因此：

```text
Vue CLI 本身运行在 Node.js 中
```

---

# 八、Vue CLI 创建项目时发生了什么？

执行：

```bash
vue create my-project
```

CLI 会：

## 1. 创建目录

```text
my-project
```

---

## 2. 生成工程结构

```text
my-project
├── node_modules
├── public
├── src
├── package.json
└── vue.config.js
```

---

## 3. 自动安装依赖

本质：

```bash
npm install
```

下载：

- Vue
    
- Babel
    
- ESLint
    
- Webpack
    
- Vue CLI Service
    

---

# 九、npm install -g 的本质

命令：

```bash
npm install -g @vue/cli
```

含义：

```text
全局安装
```

安装位置：

## Windows 默认：

```text
C:\Users\用户名\AppData\Roaming\npm
```

结构：

```text
npm
├── vue.cmd
└── node_modules
    └── @vue/cli
```

---

# 十、为什么任何目录都能执行 vue 命令？

因为：

```text
npm global目录加入了 PATH 环境变量
```

系统执行：

```text
vue
    ↓
查找 PATH
    ↓
找到 vue.cmd
    ↓
node 执行 CLI
```

---

# 十一、Docker 中运行 Vue / Node.js 的本质

关键点：

```text
是否挂载 volume
```

---

## 情况1：未挂载

```bash
docker run node
```

项目创建在：

```text
容器内部文件系统
```

删除容器：

```text
文件丢失
```

---

## 情况2：挂载 volume（推荐）

```bash
docker run -v D:\project:/app
```

则：

```text
容器内 /app
    ↔
Windows D:\project
```

此时：

```text
代码实际保存在宿主机
```

容器只是运行环境。

---

# 十二、现代前端工程化流程

完整链路：

```text
VS Code
    ↓ 写代码
.vue 文件
    ↓
Node.js + Vue CLI/Vite
    ↓
Webpack/Vite 构建
    ↓
浏览器加载JS
    ↓
Vue Runtime执行
    ↓
DOM渲染
```

---

# 十三、Vue CLI 与 Vite

当前：

- Vue CLI 已进入维护模式
    
- 官方推荐：
    
    - Vite
        

现代创建方式：

```bash
npm create vue@latest
```

而不是：

```bash
vue create
```

---

# 十四、当前学习阶段建议

推荐路线：

## 第一阶段

先理解：

- Node.js
    
- npm
    
- Vue CLI
    
- 项目结构
    
- package.json
    
- node_modules
    
- 开发服务器
    

---

## 第二阶段

再学习：

- Vite
    
- Vue Router
    
- Pinia/Vuex
    
- Axios
    
- 组件通信
    
- Composition API
    

---

## 第三阶段

再进入：

- Docker化
    
- Nginx部署
    
- 前后端分离
    
- CI/CD
    
- SSR
    
- 微前端
    

---

# 十五、核心认知总结

## 1. Vue 是什么？

```text
浏览器中的前端框架
```

---

## 2. Node.js 是什么？

```text
JavaScript运行环境
```

---

## 3. Vue CLI 是什么？

```text
运行在 Node.js 中的工程化工具
```

---

## 4. npm 是什么？

```text
JavaScript包管理系统
```

---

## 5. 浏览器最终负责什么？

```text
执行Vue代码并渲染页面
```

---

# 十六、一句话总总结

现代 Vue 前端开发本质：

```text
VS Code 写代码
    ↓
Node.js 工程化构建
    ↓
浏览器运行 Vue
    ↓
生成最终页面
```