# PDF 绘本骑马钉排版知识库补充更新（新增内容）

以下内容为对前面知识库的增量更新，主要补充：

- Crop / Media / Bleed 的最终结论
    
- MediaBox 与 CropBox 的真实关系
    
- 最终稳定工作流
    
- 为什么 InDesign 中会重新显示 44×22
    
- 最终固定推荐设置
    

建议合并到原知识库中对应章节。

---

# 二十三、PDF 页面框（Page Box）与 InDesign 的真实关系（新增核心）

## 1. 为什么 Acrobat 看起来已经拆页成功

例如：

在 Adobe Acrobat 中：

```text
44 × 22 cm
```

经过：

```text
裁剪页面（Crop Pages）
```

后。

阅读时：

会显示：

```text
22 × 22 cm
```

看起来：

# 已经拆页成功。

---

## 2. 为什么导入 InDesign 后又变成 44×22

因为：

你实际上：

# 只修改了：

# CropBox

没有真正修改：

# MediaBox

---

## 3. Acrobat 与 InDesign 默认读取的 Box 不同

|软件|默认读取|
|---|---|
|Acrobat 阅读显示|CropBox|
|InDesign 默认置入|MediaBox|

---

## 4. 这意味着什么

Acrobat：

```text
只显示裁切区域
```

所以：

你看到：

```text
22 × 22
```

---

但：

InDesign：

```text
仍然读取原始页面尺寸
```

因此：

又看到：

```text
44 × 22
```

甚至：

# 隐藏区域也重新出现。

---

# 二十四、Crop / Media / Bleed 的真正含义（最终版）

## 1. 它们不影响打印质量（最重要）

在 InDesign：

```text
Crop
Media
Bleed
```

这些选项：

# 只决定：

# “读取哪个页面边界”

不会：

- 降dpi
    
- 重压缩
    
- 改像素
    
- 改图像质量
    

---

## 2. Media（MediaBox）

表示：

# PDF真实纸张边界

例如：

```text
44 × 22 cm
```

完整原页面。

---

### 选择 Media 后

InDesign：

# 会显示整个原页面

包括：

- 黑边
    
- 留白
    
- 隐藏区域
    
- 出血
    

---

## 3. Crop（CropBox）

表示：

# 显示区域

---

### 选择 Crop 后

InDesign：

# 只显示裁切后的区域

例如：

```text
22 × 22 cm
```

---

## 4. Bleed（BleedBox）

表示：

# 出血区域

通常：

比 TrimBox：

```text
多3mm
```

用于印刷裁切。

---

## 5. 当前绘本场景最终推荐

对于：

```text
Spread PDF 拆页
绘本
骑马钉
```

最终固定推荐：

|项目|推荐|
|---|---|
|Crop To|Crop|
|不推荐|Media|
|暂不使用|Bleed|

---

# 二十五、最终固定工作流（最终版）

以后固定使用下面流程。

不再切换工具。

---

## 最终推荐工具分工

|工具|用途|
|---|---|
|Acrobat Pro|拆页、检查、小册子打印|
|PDFsam Basic|交替合并|
|InDesign|排版、骑马钉输出|

---

# 二十六、最终稳定流程（固定版）

```text
Spread PDF（44×22）
↓
Acrobat：
裁剪页面
生成 left.pdf / right.pdf
↓
PDFsam：
交替混合（Neutral）
↓
得到32页 single_pages.pdf
↓
InDesign：
A5对页文档
PlaceMultipagePDF.jsx
↓
置入PDF：
Crop To = Crop
↓
统一缩放
对象样式统一
↓
导出高质量PDF
↓
Acrobat：
小册子打印
↓
A4双面打印
↓
骑马钉
```

---

# 二十七、PDFsam 最终推荐设置（固定）

## 输出文件压缩

必须：

# 选择：

# 中性（Neutral）

---

## 不要选择：

```text
压缩
```

否则：

可能：

- JPEG重压缩
    
- 图片变糊
    
- 绘本细节损失
    

---

## PDF版本

推荐：

```text
默认
```

不要强制低版本。

---

# 二十八、最终必须牢记的原则（重要）

## 安全操作（无损）

|操作|是否安全|
|---|---|
|裁页面框|√|
|交替混合|√|
|插入页面|√|
|重组页面|√|

---

## 危险操作（可能降质）

|操作|风险|
|---|---|
|打印为PDF|重新渲染|
|在线压缩|不可控|
|最小文件大小|降dpi|
|强压缩PDF|图片损失|

---

# 二十九、最终核心理解（非常重要）

## PDF 页面并不只有一个尺寸

PDF 页面可能同时存在：

|Box|作用|
|---|---|
|MediaBox|原始页面|
|CropBox|显示区域|
|BleedBox|出血|
|TrimBox|成品尺寸|

---

## Acrobat 与 InDesign 读取的 Box 不同

这是：

# “Acrobat 正常”

但：

# “InDesign 又显示44×22”

的根本原因。

---

# 三十、最终固定推荐（不要再改）

## InDesign 置入 PDF 时

必须：

```text
显示导入选项
```

---

## Crop To：

固定：

# Crop

---

## 不再使用：

# Media

---

# 三十一、最终一句话总结（最终版）

# Acrobat 裁页面后：

# 实际主要修改的是 CropBox。

因此：

# InDesign 置入时：

# 必须选择：

# Crop To = Crop

否则：

会重新读取：

# MediaBox（44×22 原始页面）。