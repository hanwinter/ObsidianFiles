# FastAPI / Web API / WebSocket 知识总结

---

# 1. FastAPI 是什么

## 定义

FastAPI 是一个基于 Python 的现代 Web 后端框架，用于快速构建：

- Web API
    
- WebSocket 服务
    
- AI 推理接口
    
- IoT（物联网）接口
    
- 实时数据服务
    

底层基于：

- Starlette（异步 Web 框架）
    
- Pydantic（数据校验）
    

---

# 2. FastAPI 的核心特点

|特点|说明|
|---|---|
|高性能|接近 Node.js / Go|
|支持异步|async / await|
|自动接口文档|自动生成 Swagger|
|自动数据校验|基于 Pydantic|
|开发速度快|代码量少|
|适合 AI/IoT|非常适合摄像头与智能设备|

---

# 3. FastAPI 的典型应用场景

## 智能摄像头

- 上传图片
    
- 上传识别结果
    
- AI 推理服务
    
- 实时报警
    

## 智能家居

- 设备控制
    
- 状态同步
    
- Home Assistant 集成
    

## AI 服务

- YOLOv8
    
- OCR
    
- NLP
    
- 语音识别
    

---

# 4. 什么是 Web API

## 定义

Web API 是：

> 应用与应用之间，通过 HTTP 网络协议进行通信的接口。

它是：

- 软件之间的“接口”
    
- 网络上的功能调用入口
    

---

# 5. Web API 的特点

|特点|说明|
|---|---|
|基于 HTTP|GET / POST / PUT / DELETE|
|请求-响应模式|一次请求，一次返回|
|通常返回 JSON|最常见|
|短连接|请求结束即断开|
|非实时|不适合实时推送|

---

# 6. Web API 的工作方式

客户端：

```text
发送请求
```

服务器：

```text
接收请求 → 处理 → 返回结果
```

例如：

```http
POST /upload
```

返回：

```json
{
  "status": "ok"
}
```

---

# 7. Web API 的典型使用场景

## 适合：

- 上传数据
    
- 查询数据
    
- 获取设备状态
    
- 提交表单
    
- 控制一次设备操作
    

## 不适合：

- 实时推送
    
- 实时聊天
    
- 实时视频事件
    

---

# 8. 什么是 WebSocket

## 定义

WebSocket 是：

> 基于 TCP 的持续双向实时通信协议。

它允许：

- 客户端主动发消息
    
- 服务器主动发消息
    

双方保持长期连接。

---

# 9. WebSocket 的核心特点

|特点|说明|
|---|---|
|长连接|建立一次长期保持|
|双向通信|双方都能主动发消息|
|实时性强|延迟极低|
|支持推送|服务器主动通知|
|适合实时系统|摄像头/IoT常用|

---

# 10. WebSocket 的通信流程

## 第一步

客户端连接：

```text
ws://server/ws
```

## 第二步

服务器接受连接：

```python
await ws.accept()
```

## 第三步

双方持续通信：

```python
receive_text()
send_text()
```

---

# 11. WebSocket 的典型场景

## 智能摄像头

- 实时报警
    
- 实时 AI 检测结果
    
- 实时状态变化
    

## 智能家居

- 门锁状态变化
    
- 灯光状态同步
    
- 实时传感器数据
    

## Web 前端

- 实时通知
    
- 实时日志
    
- 实时聊天
    

---

# 12. Web API 与 WebSocket 的区别

|项目|Web API|WebSocket|
|---|---|---|
|协议|HTTP|WebSocket|
|连接方式|短连接|长连接|
|通信模式|请求-响应|双向实时|
|是否实时|否|是|
|是否支持服务器主动推送|否|是|
|适合|查询/上传|实时推送|

---

# 13. 如何理解两者关系

## Web API

像：

```text
问一次 → 回答一次
```

## WebSocket

像：

```text
建立持续通话
```

---

# 14. WebSocket 是否属于应用间接口

是。

但：

## Web API

属于：

```text
一次性交互接口
```

## WebSocket

属于：

```text
实时双向通信接口
```

二者都是：

> 应用与应用之间的通信接口。

只是通信模式不同。

---

# 15. FastAPI 与 WebSocket 的关系

FastAPI：

- 不仅支持 Web API
    
- 也支持 WebSocket
    

即：

```text
一个 FastAPI 服务
同时支持：
- HTTP API
- WebSocket
```

---

# 16. FastAPI Web API 示例

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/status")
def status():
    return {"status": "ok"}
```

---

# 17. FastAPI WebSocket 示例

```python
from fastapi import FastAPI, WebSocket

app = FastAPI()

@app.websocket("/ws")
async def ws(ws: WebSocket):
    await ws.accept()

    while True:
        msg = await ws.receive_text()
        await ws.send_text(msg)
```

---

# 18. 是否都需要服务器

## 结论

无论：

- Web API
    
- WebSocket
    

都必须运行在服务器上。

---

# 19. 为什么需要服务器

服务器负责：

- 监听网络端口
    
- 接收请求
    
- 维持 WebSocket 长连接
    
- 返回数据
    
- 推送消息
    

---

# 20. FastAPI 的运行方式

通常使用：

```bash
uvicorn main:app --reload
```

其中：

|组件|作用|
|---|---|
|FastAPI|Web 框架|
|Uvicorn|ASGI 服务器|

---

# 21. 智能摄像头项目中的推荐架构

## 推荐组合

```text
FastAPI
├── Web API
│   ├── 上传图片
│   ├── 查询状态
│   └── 设备控制
│
└── WebSocket
    ├── 实时报警
    ├── 实时状态
    └── AI推理结果推送
```

---

# 22. 核心理解（最重要）

## Web API

适合：

```text
一次请求 → 一次响应
```

## WebSocket

适合：

```text
持续连接 → 实时双向通信
```

---

# 23. 最终结论

在智能家居 / 智能摄像头 / AI 边缘计算中：

## Web API 负责：

- 管理
    
- 查询
    
- 上传
    
- 配置
    

## WebSocket 负责：

- 实时推送
    
- 实时控制
    
- 实时状态同步
    

## FastAPI 负责：

- 提供整个后端服务框架
    

## Uvicorn 负责：

- 真正运行服务器程序