## markdown基本语法



### 一、标题

# 标题一# #

## 标题二## ##

### 标题三###  ###

#### 标题四#### ####

字体

 **文字加粗**（**）

*斜体* (*)

***斜体加粗文字***  (***)

~~删除线的文字~~(~~)

### 三、引用（>  、>>、  >>>）

> 引用

> > 引用

### 四、分割线(--- 、----、*******、********)



---

### 五、引用图片(![]())(<img src="filename" width="" height="">可以改变图片大小</img>)

![引用图片](https://upload-images.jianshu.io/upload_images/6860761-fd2f51090a890873.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/550/format/webp)

<img src="https://upload-images.jianshu.io/upload_images/6860761-fd2f51090a890873.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/550/format/webp" width="400" height="200"></img>

### 六、 超链接 <font size=5><font size=5 color="#9D9D9D">([]())</font></font>

[简书](https://www.jianshu.com/p/191d1e21f7ed)



```xml
<a href="https://www.jianshu.com/u/1f5ac0cf6a8b" target="_blank">简书</a>
```



### 七、无序列表（  \- + *）

- 无序列表
- 无序列表
- 无序列表
- 无序列表

###  八、有序列表（1. 、2. 、。。。）

1. 有序列表

2. 有序列表

3. 有序列表

4. 有序列表

   

### 九、列表嵌套

（**上一级和下一级之间敲三个空格即可**）



- 无序嵌套

  - 无序嵌套
  - 无序嵌套   
  - 无序嵌套      

  ​      

1. 有序嵌套
   1. 有序嵌套
   2. 有序嵌套
   3. 有序嵌套

### 十、表格

|表头 | 表头 | 表头|

---|:--:|---:

内容 | 内容 | 内容



| 表头 | 表头 | 表头 |
| :--: | :--: | :--: |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |



### 十一、代码(`)(````)

`单行代码`（`）

````java
#代码块（````）
#代码块
#代码块   
````



### 十二、流程图(flow)

````flow
​```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```
````



## 注.小技巧

#### 一、字符转义（\ 反斜杠 + 特殊字符）

\***

\---

\````

\- + *

\>>>>

\####

\![]()

\[]()

#### 二、兼容html

> 例如

<br />

<hr />

<font>

语法 ： <font size=5><font size=5 color="#7373B9">兼容html</font></font>



# 快捷键

## 一：菜单栏

- 文件：alt+F
- 编辑：alt+E
- 段落：alt+P
- 格式：alt+O
- 视图：alt+V
- 主题：alt+T
- 帮助：alt+H

## 二：文件

- 新建：Ctrl+N
- 新建窗口：Ctrl+Shift+N
- 打开：Ctrl+O
- 快速打开：Ctrl+P
- 保存：Ctrl+S
- 另存为：Ctrl+Shift+S
- 偏好：Ctrl+,
- 关闭：Ctrl+W

## 三：编辑

- 撤销：Ctrl+Z
- 重做：Ctrl+Y
- 剪切：Ctrl+X
- 复制：Ctrl+C
- 粘贴：Ctrl+V
- 复制为MarkDown：Ctrl+Shift+C
- 粘贴为纯文本：Ctrl+Shift+V
- 全选：Ctrl+A
- 选中当前行/句：Ctrl+L
- 选中当前格式文本：Ctrl+E
- 选中当前词：Ctrl+D
- 跳转到文首：Ctrl+Home
- 跳转到所选内容：Ctrl+J
- 跳转到文末：Ctrl+End
- 查找：Ctrl+F
- 查找下一个：F3
- 查找上一个：Shift+F3
- 替换：Ctrl+H

## 四：段落

- 标题：Ctrl+1/2/3/4/5
- 段落：Ctrl+0
- 增大标题级别：Ctrl+=
- 减少标题级别：Ctrl+-
- 表格：Ctrl+T
- 代码块：Ctrl+Shift+K
- 公式块：Ctrl+Shift+M
- 引用：Ctrl+Shift+Q
- 有序列表：Ctrl+Shift+[
- 无序列表：Ctrl+Shift+]
- 增加缩进：Ctrl+]
- 减少缩进：Ctrl+[

## 五：格式

- 加粗：Ctrl+B
- 斜体：Ctrl+I
- 下划线：Ctrl+U
- 代码：Ctrl+Shift+`
- 删除线：Alt+Shift+5
- 超链接：Ctrl+K
- 图像：Ctrl+Shift+I
- 清除样式：Ctrl+

## 六：视图

- 显示隐藏侧边栏：Ctrl+Shift+L
- 大纲视图：Ctrl+Shift+1
- 文档列表视图：Ctrl+Shift+2
- 文件树视图：Ctrl+Shift+3
- 源代码模式：Ctrl+/
- 专注模式：F8
- 打字机模式：F9
- 切换全屏：F11
- 实际大小：Ctrl+Shift+0
- 放大：Ctrl+Shift+=
- 缩小：Ctrl+Shift+-
- 应用内窗口切换：Ctrl+Tab
- 打开DevTools：Shift+F12







































