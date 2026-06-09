# Node.js、NVM、Codex、MCP 学习总结（知识库版）

## 一、Codex CLI 安装问题总结

### 1. 初始安装

执行：

```bash
npm install -g @openai/codex
```

出现大量警告：

```text
Unsupported engine
wanted: node >=16
current: node 14.16.1
```

但最终显示：

```text
+ @openai/codex@0.135.0
added 1 package
```

说明：

- 包安装成功
    
- 当前 Node 版本过低
    
- 不保证完全兼容
    

### 2. 验证安装

执行：

```bash
codex --version
```

输出：

```text
codex-cli 0.135.0
```

说明：

- Codex 已安装成功
    
- PATH 配置正常
    
- 命令可执行
    

执行：

```bash
codex
```

出现欢迎界面：

```text
Welcome to Codex
```

说明：

- Codex 已能够正常启动
    
- 当前环境可使用
    

### 3. 升级后出现问题

Codex 自动升级：

```text
@openai/codex 0.136.0
```

启动时报错：

```text
Missing optional dependency
@openai/codex-win32-x64
```

原因：

```text
Node 14 与新版 Codex 不兼容
```

结论：

- 0.135.0 勉强支持 Node14
    
- 0.136.0 开始出现兼容性问题
    
- 应升级 Node
    

---

# 二、Node.js 核心知识

## Node.js 是什么

Node.js 是：

```text
JavaScript Runtime
(JavaScript运行环境)
```

作用：

```text
让JavaScript脱离浏览器运行
```

例如：

```text
浏览器运行JavaScript
↓
Chrome / Edge
```

```text
Node运行JavaScript
↓
服务器
命令行
构建工具
自动化脚本
```

---

## Node.js 与 Vue 的关系

Vue：

```text
前端框架
```

Node：

```text
运行环境
```

关系：

```text
浏览器
    ↓
Vue
    ↓
Vite(Node)
    ↓
Python(FastAPI)
    ↓
MySQL
```

注意：

```text
Vue ≠ Node
```

---

## Node 与 npm 的关系

安装 Node 后自动包含：

```text
Node.js
├─ node
└─ npm
```

查看版本：

```bash
node -v
npm -v
```

npm：

```text
Node Package Manager
```

作用：

```text
安装依赖包
```

例如：

```bash
npm install vue
npm install -g @openai/codex
```

---

## Node 版本现状

|版本|状态|
|---|---|
|Node14|已淘汰|
|Node16|较旧|
|Node18|LTS|
|Node20|推荐|
|Node22|推荐|

当前建议：

```text
Node22 LTS
```

原因：

支持：

- Codex
    
- Vue3
    
- Vite
    
- MCP
    
- AI工具链
    

---

# 三、NVM 核心知识

## 什么是 NVM

NVM：

```text
Node Version Manager
```

Node版本管理器。

作用：

```text
同一台电脑管理多个Node版本
```

例如：

```text
Node14
Node20
Node22
```

共存。

---

## 常用命令

查看版本：

```bash
nvm list
```

安装：

```bash
nvm install 22
```

切换：

```bash
nvm use 22
```

验证：

```bash
node -v
```

---

## 为什么使用 NVM

不同项目要求不同 Node 版本：

```text
老项目
↓
Node14
```

```text
Vue项目
↓
Node20/22
```

```text
Codex
↓
Node20/22
```

因此需要版本管理。

---

# 四、终端代理知识

## 浏览器能翻墙 ≠ 终端能翻墙

常见情况：

```text
浏览器
↓
走代理
```

```text
PowerShell
↓
不走代理
```

导致：

```text
npm
git
codex
```

访问失败。

---

## 查看公网IP

执行：

```bash
curl ifconfig.me
```

或：

```bash
curl ipinfo.io/ip
```

判断：

```text
浏览器IP
=
终端IP
```

说明：

```text
终端走代理
```

否则：

```text
终端未走代理
```

---

## HTTP代理配置

例如机场：

```text
127.0.0.1:7890
```

设置：

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```

---

## 推荐方式

开启机场：

```text
TUN Mode
```

或：

```text
增强模式
```

效果：

```text
浏览器
PowerShell
Git
npm
Python
Docker
Codex
```

全部自动走代理。

无需单独配置。

---

# 五、MCP 核心知识

## MCP 是什么

MCP：

```text
Model Context Protocol
模型上下文协议
```

作用：

```text
AI连接外部系统的统一标准
```

可以理解为：

```text
AI时代的USB接口
```

---

## MCP 架构

```text
MCP Client
      ↓
MCP Protocol
      ↓
MCP Server
```

例如：

```text
Codex
 ↓
MCP
 ↓
MySQL
```

---

## MCP 与 FHIR 的区别

FHIR：

```text
系统 ↔ 系统
```

例如：

```text
HIS
↓
FHIR
↓
体检系统
```

---

MCP：

```text
AI ↔ 系统
```

例如：

```text
Codex
↓
MCP
↓
体检系统
```

---

## 类比理解

|领域|标准|
|---|---|
|Web|HTTP|
|医疗|FHIR|
|数据库|JDBC|
|AI工具|MCP|

---

# 六、医疗行业中的 MCP 应用

结合五健筛查系统：

未来架构：

```text
体检系统
     ↓
FHIR标准化
     ↓
MCP工具层
     ↓
AI智能助手
     ↓
自动分析与报告
```

---

可提供的 MCP 工具：

```text
query_student()
query_school()
query_positive()
query_followup()
query_report()
```

示例：

```text
医生：
查询五年级近视率

AI：
调用MCP工具
读取数据库
返回统计结果
```

---

# 七、个人学习路线（当前最适合）

## 第一阶段（现在）

重点学习：

```text
Python
Vue
SQL
FHIR
Codex
```

达到：

```text
能够独立开发医疗业务系统
```

---

## 第二阶段（未来）

学习：

```text
MCP
Agent
Tool Calling
RAG
知识库
```

达到：

```text
构建AI辅助医疗系统
```

---

## 第三阶段（长期）

目标架构：

```text
体检数据
    ↓
FHIR标准化
    ↓
MCP工具层
    ↓
AI医疗助手
    ↓
智能分析与报告生成
```

形成：

```text
医疗业务系统
+
AI智能分析平台
```

这是未来医疗信息化的重要发展方向。