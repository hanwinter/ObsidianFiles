# Python 虚拟环境（venv）知识总结

---

# 一、什么是 Python 虚拟环境

Python 虚拟环境（Virtual Environment）是：

> 一个独立的 Python 运行环境。

其核心作用：

- 隔离不同项目的依赖
    
- 避免库版本冲突
    
- 防止污染系统 Python 环境
    
- 方便项目迁移与部署
    

---

# 二、虚拟环境包含什么

虚拟环境内部通常包含：

```text
venv/
├── bin/                  # python、pip 等可执行文件
├── lib/                  # 安装的第三方库
├── include/              # 头文件
└── pyvenv.cfg            # 虚拟环境配置
```

其中最重要的是：

```text
venv/lib/python3.x/site-packages/
```

这里保存：

- FastAPI
    
- OpenCV
    
- NumPy
    
- Pydantic
    
- 等第三方依赖
    

这些库都是真实下载到磁盘中的。

---

# 三、项目源代码与虚拟环境的关系

项目源代码通常不放在虚拟环境中。

推荐结构：

```text
myproject/
├── venv/                 # 虚拟环境
├── src/                  # 项目源码
│   ├── main.py
│   └── utils.py
├── requirements.txt
└── README.md
```

运行方式：

```bash
(venv) python src/main.py
```

即：

> 使用 venv 中的 Python 与依赖，去运行 src 中的源码。

---

# 四、创建虚拟环境

命令：

```bash
python3 -m venv venv
```

含义拆解：

## 1. python3

调用 Python3 解释器。

---

## 2. -m

表示：

> 运行 Python 模块。

等价于：

```python
import venv
```

---

## 3. venv（第一个）

Python 内置虚拟环境模块。

---

## 4. venv（第二个）

要创建的目录名称。

可替换：

```bash
python3 -m venv myenv
```

---

# 五、激活虚拟环境

Linux/macOS：

```bash
source venv/bin/activate
```

激活后：

```text
(venv) user@ubuntu$
```

说明当前终端已进入虚拟环境。

---

# 六、退出虚拟环境

命令：

```bash
deactivate
```

退出后恢复系统 Python。

---

# 七、pip install 的安装位置原理

关键点：

> pip 安装位置不取决于当前目录，而取决于当前激活的 Python 环境。

激活后：

```bash
which python
which pip
```

会指向：

```text
/home/han/project/venv/bin/python
/home/han/project/venv/bin/pip
```

因此：

```bash
pip install fastapi
```

无论在哪个目录执行，都会安装到：

```text
venv/lib/python3.x/site-packages/
```

---

# 八、系统 Python 与虚拟环境的关系

默认情况下：

- 虚拟环境不会使用系统已安装的库
    
- 虚拟环境有自己独立的 site-packages
    

即：

```text
系统Python库 ≠ 虚拟环境库
```

这样可以避免：

- 版本冲突
    
- 项目互相影响
    
- 系统污染
    

---

# 九、为什么同一终端只能激活一个虚拟环境

原因：

激活虚拟环境会修改当前终端的：

```bash
PATH
```

例如：

```bash
export PATH=/home/han/projectA/venv/bin:$PATH
```

这样：

```bash
python
pip
```

都会优先使用：

```text
projectA/venv/bin/
```

如果再次激活另一个环境：

```bash
source projectB/venv/bin/activate
```

PATH 会被覆盖。

因此：

> 一个终端一次只能激活一个虚拟环境。

---

# 十、多个虚拟环境同时运行

虽然一个终端只能激活一个环境：

但多个终端可以分别激活不同环境。

例如：

## 终端A

```bash
source projectA/venv/bin/activate
```

## 终端B

```bash
source projectB/venv/bin/activate
```

两者互不影响。

---

# 十一、关闭终端后的行为

虚拟环境激活仅对当前终端有效。

关闭终端后：

- PATH 修改失效
    
- 虚拟环境自动退出
    
- 恢复系统 Python
    

重新打开终端后，需要重新激活：

```bash
source venv/bin/activate
```

---

# 十二、requirements.txt

导出依赖：

```bash
pip freeze > requirements.txt
```

安装依赖：

```bash
pip install -r requirements.txt
```

作用：

- 项目迁移
    
- 团队协作
    
- 部署服务器
    
- 环境复现
    

---

# 十三、核心理解（最重要）

Python 虚拟环境本质上是：

> 为项目提供的一套独立 Python 运行时工具链。

包括：

- Python 解释器
    
- pip
    
- 第三方依赖
    

但不包含：

- 项目业务源码
    

源码只是“借用”虚拟环境运行。