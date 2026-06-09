# **Step 5：接口与对接方式设计（Integration Design）**

## 一、接口设计总览

系统对外提供 4 类核心接口：

1. **标签查询接口** (Query Tag API)
2. **标签写入接口** (Write Tag API)
3. **人群查询接口** (Audience API)
4. **事件触发接口** (Event API)

这些接口将作为系统与外部其他系统进行数据交互的主要方式，保证标签系统能够为其他业务系统提供标签服务，支持数据流转与自动化管理。

---

## 二、标签查询接口（Query Tag API）

### 目标

提供查询标签的能力，可以根据不同条件获取标签信息。

### 核心功能

- 根据主体查询标签
- 根据标签类型查询标签
- 根据标签值筛选标签
- 根据标签状态筛选标签

### 请求参数示例

{  
  "entity_id": "1001",  
  "tag_code": "HEALTH_OBESE_HIGH_RISK",  
  "status": "active"  
}

### 返回结果示例

{  
  "code": 0,  
  "message": "success",  
  "data": [  
    {  
      "tag_code": "HEALTH_OBESE_HIGH_RISK",  
      "tag_name": "高危肥胖",  
      "status": "active",  
      "expire_at": "2024-12-31"  
    }  
  ]  
}

### 设计原则

- 支持多条件查询
- 返回的数据应尽量简洁
- 支持分页查询

---

## 三、标签写入接口（Write Tag API）

### 目标

提供标签写入和更新能力，用于给主体分配标签或更新标签状态。

### 核心功能

- 为主体分配标签
- 更新主体标签
- 标记标签为失效

### 请求参数示例

{  
  "entity_id": "1001",  
  "tag_code": "HEALTH_OBESE_HIGH_RISK",  
  "source": "rule_engine",  
  "status": "active",  
  "expire_at": "2024-12-31"  
}

### 返回结果示例

{  
  "code": 0,  
  "message": "success"  
}

### 设计原则

- 必须支持幂等性（同一请求多次执行，结果相同）
- 必须记录标签来源
- 更新时支持批量操作

---

## 四、人群查询接口（Audience API）

### 目标

提供查询人群信息的能力，可以根据标签或条件查询目标人群。

### 核心功能

- 根据标签查询人群
- 根据条件查询人群
- 获取人群统计数据

### 请求参数示例

{  
  "tag_code": "HEALTH_OBESE_HIGH_RISK",  
  "status": "active"  
}

### 返回结果示例

{  
  "code": 0,  
  "message": "success",  
  "data": {  
    "audience_count": 500,  
    "audience": [  
      {  
        "entity_id": "1001",  
        "tag": "HEALTH_OBESE_HIGH_RISK",  
        "status": "active"  
      }  
    ]  
  }  
}

### 设计原则

- 支持标签和条件组合查询
- 返回数据应简洁
- 人群统计支持聚合操作

---

## 五、事件触发接口（Event API）

### 目标

提供触发标签变更的能力，可以通过事件触发标签生成或失效。

### 核心功能

- 触发标签变更
- 触发标签计算
- 触发标签失效

### 请求参数示例

{  
  "entity_id": "1001",  
  "event": "completing_followup",  
  "triggered_tag": "FOLLOWUP_COMPLETED"  
}

### 返回结果示例

{  
  "code": 0,  
  "message": "success"  
}

### 设计原则

- 触发事件必须唯一
- 事件触发后必须记录日志
- 支持批量事件触发

---

## 六、接口设计原则（统一要求）

### 1）接口路径设计

/api/tag  
/api/user/tag  
/api/audience  
/api/event

### 2）统一返回结构

{  
  "code": 0,  
  "message": "success",  
  "data": {}  
}

### 3）错误码设计

|code|说明|
|---|---|
|0|成功|
|400|参数错误|
|401|未授权|
|404|未找到|
|500|系统错误|

---

## 七、接口安全与权限控制（必须确保）

### 安全设计

接口必须进行身份验证  
接口必须使用 HTTPS

### 权限控制

每个 API 必须有权限控制  
必须限制操作范围（如标签范围、主体范围）

---

## 八、幂等性与事务保证（关键设计）

接口设计必须保证幂等性：

无论接口调用多少次，效果相同

例如：

POST /api/user/tag → 给用户分配标签

---

## 九、下一步（Step 6）将进入：系统部署与运行机制设计

在下一阶段，我们将设计：

系统的部署架构  
性能优化与容错机制  
日志与监控

这将标志着系统进入 **生产准备阶段**。