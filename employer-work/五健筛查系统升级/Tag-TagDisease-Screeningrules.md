很好，这一步就进入**可直接落库设计**阶段了。  
我们以 **BMI（体质指数）** 为唯一示例，把三张核心表设计成：

```text
可配置
可扩展
可上线
```

并且严格遵循你当前系统的现实背景：  
**已有体检结果表 + 已有疾病库，只新增能力，不重构系统。**

---

# 一、先明确 BMI 这项业务规则（必须先冻结）

这是整个表设计的依据。

## BMI 判定标准（儿童青少年常见简化版）

> 这里先用一个**工程可落地的简化版本**，后续可以升级为  
> **按年龄 / 性别 / 百分位** 的复杂规则。

|BMI范围|判定结果|对应疾病|标签建议|
|---|---|---|---|
|< 18.5|偏瘦|营养不良|需关注|
|18.5–23.9|正常|无|健康|
|24.0–27.9|超重|超重|需关注|
|≥ 28.0|肥胖|肥胖|高危|

---

# 二、先设计 Tag 表（标签定义）

这是**最基础表**，必须先存在。

---

# 表：tag（标签表）

```sql
CREATE TABLE tag (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    tag_code VARCHAR(50) NOT NULL UNIQUE,
    tag_name VARCHAR(100) NOT NULL,

    tag_level INT DEFAULT 1,
    tag_color VARCHAR(20),

    description VARCHAR(255),

    is_active BOOLEAN DEFAULT TRUE,

    created_time DATETIME,
    updated_time DATETIME
);
```

---

# 字段说明（工程级）

|字段|类型|必填|示例|说明|
|---|---|---|---|---|
|id|BIGINT|是|1|主键|
|tag_code|VARCHAR|是|HIGH_RISK|业务编码|
|tag_name|VARCHAR|是|高危|展示名称|
|tag_level|INT|是|3|风险等级|
|tag_color|VARCHAR|否|red|UI颜色|
|description|VARCHAR|否|需重点干预|描述|
|is_active|BOOLEAN|是|TRUE|是否启用|

---

# 建议初始化数据（必须有）

```text
id | tag_code      | tag_name | tag_level
-----------------------------------------
1  | HEALTHY       | 健康     | 1
2  | ATTENTION     | 需关注   | 2
3  | HIGH_RISK     | 高危     | 3
```

---

# 三、设计 disease_tag 表（疾病与标签关系）

这是：

```text
一个疾病 → 多个标签
一个标签 → 多个疾病
```

的关系表。

---

# 表：disease_tag

```sql
CREATE TABLE disease_tag (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    disease_id BIGINT NOT NULL,
    tag_id BIGINT NOT NULL,

    is_primary BOOLEAN DEFAULT TRUE,

    created_time DATETIME
);
```

---

# 字段说明

|字段|示例|说明|
|---|---|---|
|disease_id|101|对应疾病库ID|
|tag_id|2|对应标签|
|is_primary|TRUE|是否主标签|

---

# BMI 示例数据

假设疾病库已有：

```text
101 营养不良
102 超重
103 肥胖
```

---

# disease_tag 示例

```text
id | disease_id | tag_id | is_primary
-------------------------------------
1  | 101        | 2      | TRUE
2  | 102        | 2      | TRUE
3  | 103        | 3      | TRUE
```

含义：

```text
营养不良 → 需关注
超重 → 需关注
肥胖 → 高危
```

---

# 四、设计 screening_rule 表（核心规则表）

这是系统的 **决策核心表**。

---

# 表：screening_rule

```sql
CREATE TABLE screening_rule (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    rule_code VARCHAR(50) NOT NULL UNIQUE,
    rule_name VARCHAR(100),

    screening_item_code VARCHAR(50) NOT NULL,

    operator VARCHAR(10) NOT NULL,

    threshold_value DECIMAL(6,2),

    result_status VARCHAR(20),

    disease_id BIGINT,

    priority INT DEFAULT 1,

    is_active BOOLEAN DEFAULT TRUE,

    created_time DATETIME,
    updated_time DATETIME
);
```

---

# 字段说明（关键）

|字段|示例|说明|
|---|---|---|
|rule_code|BMI_OBESITY|规则编码|
|screening_item_code|BMI|项目标识|
|operator|>=|运算符|
|threshold_value|28|阈值|
|result_status|阳性|判定结果|
|disease_id|103|关联疾病|
|priority|1|优先级|

---

# BMI 规则初始化数据（非常关键）

```text
id | rule_code        | operator | threshold | disease | priority
-----------------------------------------------------------------
1  | BMI_UNDERWEIGHT  | <        | 18.5      | 营养不良 | 1
2  | BMI_NORMAL       | BETWEEN  | 18.5-23.9 | 无      | 2
3  | BMI_OVERWEIGHT   | >=       | 24.0      | 超重    | 3
4  | BMI_OBESITY      | >=       | 28.0      | 肥胖    | 4
```

---

# 注意一个关键工程点

```text
priority 越小
优先级越高
```

所以：

```text
肥胖
必须先判断
```

否则：

```text
BMI = 30
```

可能先匹配：

```text
>= 24
```

导致错误。

---

# 推荐优先级（正确）

```text
肥胖      priority = 1
超重      priority = 2
偏瘦      priority = 3
正常      priority = 4
```

---

# 五、执行流程（真实运行路径）

系统在汇总 BMI 时执行：

---

## Step 1

读取 BMI

```text
BMI = 29.4
```

---

## Step 2

查询规则

```sql
SELECT *
FROM screening_rule
WHERE screening_item_code = 'BMI'
AND is_active = TRUE
ORDER BY priority ASC;
```

---

## Step 3

匹配规则

```text
BMI >= 28
```

匹配：

```text
BMI_OBESITY
```

---

## Step 4

得到疾病

```text
肥胖
```

---

## Step 5

查询标签

```sql
SELECT tag_id
FROM disease_tag
WHERE disease_id = 103;
```

---

## Step 6

写入 student_tag

```text
学生：张三
标签：高危
疾病：肥胖
```

---

# 六、你现在这 3 张表，已经足够上线 BMI 自动判读

这是一个：

```text
完整闭环
```

```text
BMI
→ 规则
→ 疾病
→ 标签
```

---

# 七、下一步最合理的扩展顺序（建议）

不是再设计更多表，而是按业务扩展。

---

# Step 1（建议下一步）

设计：

```text
student_tag 表
```

用于记录：

```text
某个学生
在某次体检
得到的标签
```

---

# Step 2

把同一结构复制到：

```text
视力
龋齿
脊柱侧弯
心理测评
```

---

# Step 3（未来）

升级规则为：

```text
按年龄
按性别
按百分位
```

例如：

```text
男
10岁
BMI >= P95
```

但现在：

```text
不需要
```

---

如果你继续推进，下一步我建议直接做：

```text
student_tag 表（受检者标签表）
```

这是整个链路的最后一块拼图。