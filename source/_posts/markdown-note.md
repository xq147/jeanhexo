---
title: Markdown之杂记
date: 2021-11-06 11:57:31
tags: [随笔]
excerpt: 嗯嗯~~ 这是个人Markdown学习笔记，仅仅为自己工作学习提供一个参考不做其他用处。
---

# 注意
vue3 之后Markdown可能不兼容非HTML正规语法，本地运行可正常进行，但是打包时会失败！！！

### 首行缩进
- 半角的空格（英文下使用）：
&ensp; 或 &#8194;

- 全角的空格（中文下使用）：
&emsp; 或 &#8195;

- 不断行的空格：
&nbsp; 或 &#160;

## 2021/11/06

### 图片
```html
<!--开始居中对齐-->
<center>
    
![底部图片](http://zh.mweb.im/asset/img/set-up-git.gif "图片")
    
</center> 
<!--结束居中对齐-->
```
<!--开始居中对齐-->
<center>  

![底部图片](http://zh.mweb.im/asset/img/set-up-git.gif "图片")

</center>
 <!--结束居中对齐-->

更多详细信息: [图片](https://www.runoob.com/markdown/md-image.html)

### 字体

```html
 <font face="黑体">嘿嘿~黑体字</font>
 <font face="微软雅黑">嘿嘿~微软雅黑</font>
 <font face="STCAIYUN">嘿嘿~华文彩云</font>
 <font color=#0099ff size=12 face="黑体">嘿嘿~黑体</font>
 <font color=gray size=5>gray色</font>
 <font color=#00ffff size=3>哈哈哈</font>
```
<font face="黑体">嘿嘿~黑体字</font>
<font face="微软雅黑">嘿嘿~微软雅黑</font>
<font face="STCAIYUN">嘿嘿~华文彩云</font>
<font color=#0099ff size=12 face="黑体">嘿嘿~黑体</font>
<font color=gray size=5>gray色</font>
<font color=#00ffff size=3>哈哈哈</font>

[comment]: <> (更多信息: [Writing]&#40;https://hexo.io/docs/writing.html&#41;)

### 表格

```html
|订单记录|流水记录|
|:-|:-|
|0010|0011|
```
|订单记录|流水记录|
|:-|:-|
|0010|0011|

更多详细信息: [表格](https://www.runoob.com/markdown/md-table.html)

### 流程图

More info: [高级技巧](https://www.runoob.com/markdown/md-advance.html)

### 特殊字符
|特殊字符|描述|字符的代码|
|-|-|-|
| |空格符| &nbsp|
|<|小于号| &lt|
|>|大于号| &&gt|
|&|	和号 |&amp|
|￥|人民币|&yen|
|©|	版权|&copy|
****|®|	注册商标|&reg|
|°C|摄氏度|&deg|
|±|	正负号|&plusmn|
|×|	乘号|&times|
|÷|	除号|&divide|
|²|	平方（上标²）|&sup2|
|³|	立方（上标³）|&sup3|
