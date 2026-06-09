# 用户画像与标签中心
## 模块与数据映射设计（Step 3：Module–Data Mapping）

---

# 一、本阶段目标（必须明确）

本阶段的目标不是设计接口，也不是设计页面，而是：

```text
明确每一个能力模块，具体操作哪些核心数据实体
```

这一步的意义是：

```text
让系统从“架构设计”进入“工程实现准备阶段”
```

---

# 二、核心原则（工程级）

```text
一个模块可以操作多个实体
一个实体可以被多个模块使用
但必须有“主负责模块”
```

否则会出现：

```text
数据无人负责
逻辑重复实现
权限难以控制
```

---

# 三、模块与核心实体总览（系统级矩阵）

```text
模块名称                    主实体                     关联实体

1 主体统一管理               Entity                     EntityType
2 主体关系管理               EntityRelation             Entity
3 标签体系管理               Tag                        EntityType
4 标签规则引擎               TagRule                    Tag
5 标签计算与分配             EntityTag                  TagRule / TagHistory
6 用户画像管理               Entity                     EntityTag
7 人群管理                   Audience                   Tag / Entity / EntityRelation
8 标签应用中心               EntityTag                  Audience / Tag
```

---

# 四、逐模块数据映射（标准定义）

---

# 1 主体统一管理（Entity Registry）

## 主负责实体

```text
Entity
```

## 关联实体

```text
EntityType
```

## 本模块负责的数据操作

```text
创建主体
更新主体状态
维护主体唯一ID
管理主体类型
```

## 不允许操作

```text
标签生成
标签规则
标签历史
```

---

# 2 主体关系管理（Entity Relation）

## 主负责实体

```text
EntityRelation
```

## 关联实体

```text
Entity
```

## 本模块负责的数据操作

```text
建立主体关系
更新主体关系
删除主体关系
查询主体关系
```

## 不允许操作

```text
标签定义
标签生成
人群定义
```

---

# 3 标签体系管理（Tag Dictionary）

## 主负责实体

```text
Tag
```

## 关联实体

```text
EntityType
```

## 本模块负责的数据操作

```text
创建标签
更新标签
设置标签生命周期
设置标签优先级
启用 / 停用标签
```

## 不允许操作

```text
生成标签
分配标签
执行规则
```

---

# 4 标签规则引擎（Rule Engine）

## 主负责实体

```text
TagRule
```

## 关联实体

```text
Tag
```

## 本模块负责的数据操作

```text
创建规则
更新规则
版本管理
启用 / 停用规则
规则测试
```

## 不允许操作

```text
直接修改标签
直接写入 EntityTag
```

---

# 5 标签计算与分配（Tag Assignment）

## 主负责实体

```text
EntityTag
```

## 关联实体

```text
TagRule
TagHistory
Tag
```

## 本模块负责的数据操作

```text
执行规则
生成标签
更新标签
失效标签
记录标签历史
```

## 关键说明（非常重要）

```text
这是系统中唯一允许写入 EntityTag 的模块
```

---

# 6 用户画像管理（Profile Management）

## 主负责实体

```text
Entity
```

## 关联实体

```text
EntityTag
EntityRelation
```

## 本模块负责的数据操作

```text
读取主体信息
读取标签集合
生成画像视图
展示风险状态
```

## 不允许操作

```text
修改标签
执行规则
```

---

# 7 人群管理（Audience Management）

## 主负责实体

```text
Audience
```

## 关联实体

```text
Tag
Entity
EntityRelation
```

## 本模块负责的数据操作

```text
创建人群定义
更新人群条件
预览人群
统计人群数量
保存人群规则
```

## 不允许操作

```text
发送消息
执行标签规则
```

---

# 8 标签应用中心（Tag Service / Application）

## 主负责实体

```text
EntityTag
```

## 关联实体

```text
Audience
Tag
```

## 本模块负责的数据操作

```text
提供标签查询服务
提供标签写入接口
提供人群查询服务
记录调用日志
```

## 关键说明

```text
所有外部系统必须通过本模块访问标签数据
```

---

# 五、系统数据写入路径（唯一标准路径）

```text
外部系统
      │
      ▼
Tag Service
      │
      ▼
Tag Assignment
      │
      ▼
EntityTag
      │
      ▼
TagHistory
```

---

# 六、本阶段完成标志（工程判断标准）

当满足以下条件，说明 Step 3 完成：

```text
每个模块都有明确主实体
每个实体都有明确责任模块
数据写入路径唯一
模块之间没有职责冲突
```

---

# 七、下一步（Step 4）将进入：子级功能模块设计（页面级）

下一阶段将正式进入：

```text
系统页面结构设计（Menu / Page Level）
```

将回答三个问题：

```text
每个模块有哪些页面？
每个页面解决什么问题？
每个页面操作哪些数据？
```

这是系统进入：

```text
产品级设计阶段
```

