前端开发规范，覆盖HTML，JS，CSS
## 概览
- 代码必须使用4个空格符而不是tab键进行缩进（不要混用！）
- 代码缩进。统一使用四个空格进行代码缩进，使得各编辑器表现一致（各编辑器有相关配置）

``` 
<div class="jdc">
    <a href="#"></a>
</div>
```

# HTML

## DOCTYPE 声明
一个DOCTYPE必须包含以下部分，并严格按照顺序出现：
> A string that is an ASCII case-insensitive match for the string “<!DOCTYPE”.
One or more space characters.
A string that is an ASCII case-insensitive match for the string “html”.
Optionally, a DOCTYPE legacy string or an obsolete permitted DOCTYPE string (defined below).
Zero or more space characters.
A “>” (U+003E) character.

1. 一个ASCII字符串 “<!DOCTYPE” ，大小写不敏感
1. 一个或多个空白字符
1. 一个ASCII字符串”html”，大小写不敏感
1. 一个可选的历史遗留的DOCTYPE字符串 （DOCTYPE legacy string），或者一个可选的已过时但被允许的DOCTYPE字符串 （obsolete permitted DOCTYPE string） 字符串
1. 一个或多个空白字符
1. 一个编码为 U+003E 的字符 “>”

团队约定
HTML文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明：

```
 <!DOCTYPE html>
```

## 更多关于 DOCTYPE声明
[the doctype](http://www.w3.org/TR/2014/REC-html5-20141028/syntax.html#the-doctype)

## 页面LANG
团队约定
推荐使用的属性值： `zh-CN`
```
<html lang="zh-CN">
```
## CHARSET
团队约定
一般情况下统一使用 “UTF-8” 编码
``` 
<meta charset="UTF-8">
```
## 元素及标签闭合

HTML元素共有以下5种：
 
- 空元素：area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr
- 原始文本元素：script、style
- RCDATA元素：textarea、title
- 外来元素：来自MathML命名空间和SVG命名空间的元素。
- 常规元素：其他HTML允许的元素都称为常规元素。

元素标签的闭合应遵循以下原则：

- 原始文本元素、RCDATA元素以及常规元素都有一个开始标签来表示开始，一个结束标签来表示结束。
- [某些元素的开始和结束标签](http://www.w3.org/TR/html5/syntax.html#optional-tags)是可以省略的，如果规定标签不能被省略，那么就绝对不能省略它。
- 空元素只有一个开始标签，且不能为空元素设置结束标签。
- 外来元素可以有一个开始标签和配对的结束标签，或者只有一个自闭合的开始标签，且后者情况下该元素不能有结束标签。

团队约定

为了能让浏览器更好的解析代码以及能让代码具有更好的可读性，有如下约定：
 
- 所有具有开始标签和结束标签的元素都要写上起止标签，某些允许省略开始标签或和束标签的元素亦都要写上。
- 空元素标签都不加 “/” 字符

*推荐：*
```
<div>
    <h1>我是h1标题</h1>
    <p>我是一段文字，我有始有终，浏览器能正确解析</p>
</div>
 
<br>
```

*不推荐：*
``` 
<div>
    <h1>我是h1标题</h1>
    <p>我是一段文字，我有始无终，浏览器亦能正确解析
</div>
 
<br/>
```

## 书写风格
### HTML代码大小写
HTML标签名、类名、标签属性和大部分属性值统一用小写
 
*推荐：*
``` 
<div class="demo"></div>
```
*不推荐：*

```
<div class="DEMO"></div>
 
<DIV CLASS="DEMO"></DIV>
```

HTML文本、CDATA、JavaScript、meta标签某些属性等内容可大小写混合

```javascript 
<!-- 优先使用 IE 最新版本和 Chrome Frame -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
 
<!-- HTML文本内容 -->
<h1>I AM WHAT I AM </h1>
 
<!-- JavaScript 内容 -->
<script type="text/javascript">
    var demoName = 'demoName';
    ...
</script>
 
<!-- CDATA 内容 -->
<script type="text/javascript"><![CDATA[
...
]]></script>
```
### 类型属性
不需要为 CSS、JS 指定类型属性，HTML5 中默认已包含
 
*推荐：*

```
<link rel="stylesheet" href="" >
<script src=""></script>
```
*不推荐：*

```
<link rel="stylesheet" type="text/css" href="" >
<script type="text/javascript" src="" ></script>
```

### 元素属性
- 元素属性值使用双引号语法
- 元素属性值可以写上的都写上

*推荐：*

``` 
<input type="text">
 
<input type="radio" name="name" checked="checked" >
```
*不推荐：*

``` 
<input type=text>   
<input type='text'>
 
<input type="radio" name="name" checked >
```

