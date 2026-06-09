# Windows + FlClash 配置 CMD 代理完整指南

适用场景：

- Windows 10 / Windows 11
    
- FlClash
    
- CMD
    
- PowerShell
    
- Git
    
- Python(pip)
    
- Node.js(npm)
    
- Vue开发环境
    
- Codex CLI
    
- Claude Code
    

---

# 一、确认 FlClash 代理端口

打开：

```text
FlClash → 配置 → 覆写（Override）
```

查看：

```yaml
mixed-port: 7890
```

当前你的配置：

```yaml
mixed-port: 7890
```

说明代理地址为：

```text
127.0.0.1:7890
```

其中：

```text
127.0.0.1
```

表示本机

```text
7890
```

表示代理端口

---

# 二、测试当前是否走代理

打开 CMD：

```cmd
curl https://ipinfo.io/ip
```

返回：

```text
123.123.123.123
```

通常是你的真实公网IP。

记录下来。

---

# 三、CMD 临时配置代理

仅当前 CMD 窗口有效。

执行：

```cmd
set HTTP_PROXY=http://127.0.0.1:7890
set HTTPS_PROXY=http://127.0.0.1:7890
```

查看：

```cmd
echo %HTTP_PROXY%
```

输出：

```text
http://127.0.0.1:7890
```

说明配置成功。

---

# 四、验证代理是否生效

再次执行：

```cmd
curl https://ipinfo.io/ip
```

或者：

```cmd
curl https://ipinfo.io/json
```

如果显示的是节点IP：

```json
{
  "country": "US"
}
```

说明已经通过 FlClash 出口访问网络。

---

# 五、取消 CMD 代理

当前窗口取消：

```cmd
set HTTP_PROXY=
set HTTPS_PROXY=
```

验证：

```cmd
echo %HTTP_PROXY%
```

为空即可。

---

# 六、永久配置 CMD 代理

管理员 CMD：

```cmd
setx HTTP_PROXY http://127.0.0.1:7890
setx HTTPS_PROXY http://127.0.0.1:7890
```

关闭所有终端。

重新打开 CMD：

```cmd
echo %HTTP_PROXY%
```

输出：

```text
http://127.0.0.1:7890
```

说明永久配置成功。

---

# 七、PowerShell 配置代理

当前窗口：

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```

查看：

```powershell
$env:HTTP_PROXY
```

测试：

```powershell
curl https://ipinfo.io/ip
```

---

# 八、Git 配置代理

查看当前配置：

```cmd
git config --global --get http.proxy
```

配置：

```cmd
git config --global http.proxy http://127.0.0.1:7890

git config --global https.proxy http://127.0.0.1:7890
```

查看：

```cmd
git config --global --list
```

应该看到：

```text
http.proxy=http://127.0.0.1:7890
https.proxy=http://127.0.0.1:7890
```

---

# 九、取消 Git 代理

```cmd
git config --global --unset http.proxy

git config --global --unset https.proxy
```

验证：

```cmd
git config --global --list
```

不再显示 proxy 配置。

---

# 十、Python（pip）使用代理

推荐直接使用系统环境变量：

```cmd
HTTP_PROXY
HTTPS_PROXY
```

pip 会自动读取。

测试：

```cmd
pip install requests
```

如果能正常下载，则说明代理生效。

也可手动指定：

```cmd
pip install requests --proxy=http://127.0.0.1:7890
```

---

# 十一、Node.js / npm 使用代理

查看：

```cmd
npm config get proxy
```

设置：

```cmd
npm config set proxy http://127.0.0.1:7890

npm config set https-proxy http://127.0.0.1:7890
```

验证：

```cmd
npm config list
```

---

# 十二、查看系统所有代理环境变量

CMD：

```cmd
set | findstr PROXY
```

输出示例：

```text
HTTP_PROXY=http://127.0.0.1:7890
HTTPS_PROXY=http://127.0.0.1:7890
```

---

# 十三、开发环境推荐配置

对于你的环境：

```text
Python
Vue
Node.js
GitHub
VSCode
Codex CLI
Claude Code
```

推荐一次性执行：

```cmd
setx HTTP_PROXY http://127.0.0.1:7890

setx HTTPS_PROXY http://127.0.0.1:7890

git config --global http.proxy http://127.0.0.1:7890

git config --global https.proxy http://127.0.0.1:7890
```

然后：

```text
关闭所有终端
重新打开终端
```

即可让大部分开发工具自动走 FlClash。

---

# 十四、常用排查命令

查看代理：

```cmd
echo %HTTP_PROXY%
```

查看 Git：

```cmd
git config --global --get http.proxy
```

查看公网 IP：

```cmd
curl https://ipinfo.io/ip
```

查看详细网络信息：

```cmd
curl https://ipinfo.io/json
```

查看所有环境变量：

```cmd
set | findstr PROXY
```

---

# 最终推荐（最简单）

你的 FlClash 配置已经明确：

```yaml
mixed-port: 7890
```

因此只需执行一次：

```cmd
setx HTTP_PROXY http://127.0.0.1:7890
setx HTTPS_PROXY http://127.0.0.1:7890
```

以后：

- CMD
    
- PowerShell
    
- Python
    
- pip
    
- npm
    
- Vue
    
- Git
    
- VSCode 插件
    
- Codex CLI
    

大多数都会自动通过 FlClash 的 `127.0.0.1:7890` 出网，无需每次重复配置。