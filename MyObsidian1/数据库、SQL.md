# 数据库、SQL、前端构建、代码签名、AI沙盒模式核心知识总结

---

# 一、PostgreSQL 数据库结构核心概念

## PostgreSQL 对象树结构

```text
Database
 ├── Schemas
 ├── Extensions
 ├── Functions
 ├── Triggers
 ├── FDW
 └── Replication
```

---

## 1. Schemas（模式）

作用：

```text
数据库中的命名空间
```

用于：

- 分类表
    
- 分类视图
    
- 分类函数
    

类似：

```text
medical_rules.rule_definition
```

推荐：

```text
rule_core
rule_runtime
audit
dictionary
```

---

## 2. Extensions（扩展）

用于增强 PostgreSQL 功能。

常见扩展：

|扩展|功能|
|---|---|
|uuid-ossp|UUID生成|
|pgcrypto|加密|
|pg_trgm|模糊搜索|
|postgis|GIS|

---

## 3. FDW（Foreign Data Wrapper）

作用：

```text
PostgreSQL 访问外部数据库
```

支持：

- MySQL
    
- Oracle
    
- PostgreSQL
    

实现：

```text
PostgreSQL 查询 MySQL 数据
```

---

# 二、SQL 核心语法

---

## 1. SELECT 多字段

多个字段：

```sql
SELECT id, name, age
FROM users;
```

规则：

```text
字段之间用 ,
```

---

## 2. WHERE 多条件

多个条件：

```sql
WHERE age > 18
AND city = 'Beijing'
```

连接符：

|运算符|含义|
|---|---|
|AND|同时满足|
|OR|任意满足|
|NOT|取反|

---

## 3. 一个字段匹配多个值

推荐：

```sql
WHERE name IN ('张三','李四')
```

等价：

```sql
WHERE name='张三'
OR name='李四'
```

---

## 4. SQL 结尾分号

```sql
SELECT * FROM users;
```

作用：

```text
SQL语句结束符
```

单条SQL通常可省略。

多条SQL建议必须写。

---

## 5. SQL 大小写规则

SQL关键字：

```text
不区分大小写
```

推荐规范：

```text
SELECT / FROM / WHERE 大写
表名字段名小写
```

推荐：

```sql
SELECT id, name
FROM users;
```

---

# 三、DBeaver 数据库管理

## DBeaver 特点

支持：

- MySQL
    
- PostgreSQL
    
- SQL Server
    
- Oracle
    
- SQLite
    

适合作为统一数据库管理工具。

---

# 四、DBeaver 导出数据

---

## 导出流程

```text
Export Data
 ├── 导出目标
 ├── 抽取设置
 ├── 格式设置
 ├── 输出
 └── 确认
```

---

## 1. 导出目标

决定导出格式：

|格式|用途|
|---|---|
|CSV|通用文本数据|
|XLSX|Excel|
|JSON|API数据|
|SQL|INSERT语句|

---

## 2. 抽取设置

控制：

```text
数据库读取方式
```

常见：

|参数|含义|
|---|---|
|Maximum rows|最大导出行数|
|Fetch size|每次读取数量|
|Segment size|分批读取|

---

## 3. 格式设置

控制：

```text
文件内容格式
```

例如：

|设置|说明|
|---|---|
|Delimiter|分隔符|
|Include Header|是否导出列名|
|Quote Character|引号字符|

---

## 4. 输出

设置：

- 文件路径
    
- 编码
    
- 是否覆盖
    

推荐编码：

```text
UTF-8
```

---

# 五、CSV 格式

CSV：

```text
Comma-Separated Values
```

即：

```text
逗号分隔值
```

示例：

```text
id,name,age
1,Tom,20
2,Alice,25
```

特点：

- 纯文本
    
- 通用性强
    
- Excel可直接打开
    

---

# 六、DBeaver 导出视图数据

---

## 导出20条数据

### MySQL / PostgreSQL

```sql
SELECT *
FROM view_name
LIMIT 20;
```

### SQL Server

```sql
SELECT TOP 20 *
FROM view_name;
```

然后：

```text
右键结果集
→ Export Resultset
```

---

## 导出视图DDL

```text
右键 View
→ Generate SQL
→ DDL
```

得到：

```sql
CREATE VIEW ...
```

---

# 七、SQL Server 连接配置（DBeaver）

---

## 常见配置

|字段|推荐|
|---|---|
|Database|master|
|Schema|dbo|
|Authentication|SQL Server Authentication|
|Username|sa|

---

## Show all schemas

建议：

```text
勾选
```

作用：

```text
显示所有 schema
```

---

## Trust server certificate

开发环境：

```text
建议勾选
```

作用：

```text
忽略SSL证书验证
```

避免连接失败。

---

# 八、npm run build 本质

---

## 核心作用

```text
开发代码
↓
生产环境静态资源
```

---

## 构建过程

```text
源码
 ↓
模块打包
 ↓
代码压缩
 ↓
Tree Shaking
 ↓
资源处理
 ↓
生成 dist
```

---

## dist 目录

最终部署文件：

```text
dist/
 ├── index.html
 └── assets/
```

---

## 前端开发流程

```text
npm run dev
    ↓
开发调试

npm run build
    ↓
生成生产版本
```

---

# 九、Windows 代码签名

---

## 本质

```text
数字证书 + 文件签名
```

作用：

1. 证明软件来源
    
2. 防止文件被篡改
    
3. 提升Windows信任
    

---

## 没有签名会怎样

Windows 提示：

```text
未知发布者
```

---

## 签名后

显示：

```text
Verified Publisher
```

---

## 常见工具

```text
signtool
```

---

# 十、CA证书登录

---

## 本质

```text
数字证书身份认证
```

代替：

```text
用户名 + 密码
```

---

## 常见场景

|场景|是否需要|
|---|---|
|普通系统|不需要|
|HTTPS网站|需要|
|政府系统|必须|
|金融系统|必须|

---

## HTTPS 本质

浏览器中的：

```text
https://
```

就是 SSL/TLS 证书。

---

# 十一、Codex / Cursor 沙盒模式

---

## Sandbox（沙盒）

本质：

```text
隔离运行环境
```

作用：

- 限制AI权限
    
- 防止误删文件
    
- 防止破坏系统
    

---

## “代理启动沙盒模式”

意思：

```text
AI代理正在安全隔离环境中运行
```

不是错误。

---

## 沙盒限制

通常：

|操作|是否允许|
|---|---|
|修改当前项目|✔|
|删除系统文件|❌|
|修改系统设置|❌|

---

# 十二、Cursor 对话回滚机制

---

## Continue without reverting

作用：

```text
保留当前文件
重新生成对话分支
```

---

## Continue and revert

作用：

```text
回滚文件
删除后续对话
```

类似：

```text
git reset --hard
```

---

## 最佳实践

阶段性：

```bash
git commit
```

避免代码丢失。

---

# 十三、推荐开发工具体系

---

## 数据库工具

推荐：

DBeaver

原因：

```text
统一管理多数据库
```

---

## 前端构建

推荐：

- Vite
    
- Webpack
    

---

## Python后端

推荐：

- FastAPI
    
- Uvicorn
    

启动：

```bash
uvicorn app.main:app --reload
```

---

# 十四、适用于当前项目的推荐架构

```text
Vue 前端
    ↓
FastAPI 后端
    ↓
PostgreSQL 规则数据库
    ↓
MySQL 业务数据库
```

数据库分析：

```text
DBeaver
统一管理
```