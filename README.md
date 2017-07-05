# 关于 CSS 摘抄记录
## 什么是CSS hack
```
由于不同的浏览器，甚至同一浏览器的不同版本对CSS的解析认识不一样，导致生成的页面效果不一致，写出针对不同浏览器CSS code就称为CSS hack。
```
+ 常用的CSS hack 有三种方式，CSS 内部hack、选择器hack、HTML 头部引用，其中第一种最常用。

## CSS 内部hack

正经的CSS是这么写的
```
<!DOCTYPE html>
<html>
    <head>
        <title>Test</title>
        <style type="text/css" >
            .test
            {
                background-color:green;
            }
        </style>
    </head>
    <body>
        <div class="test" style="height:100px; width:100px; line-height:100px; margin:50px; border:1px solid #000;"></div>
    </body>
<html>
```

但是在CSS3中常见一些这样的写法
```
/*Mozilla内核浏览器：firefox3.5+*/
  -moz-transform: rotate | scale | skew | translate ;
 /*Webkit内核浏览器：Safari and Chrome*/
  -webkit-transform: rotate | scale | skew | translate ;
 /*Opera*/
  -o-transform: rotate | scale | skew | translate ;
 /*IE9*/
  -ms-transform: rotate | scale | skew | translate ;
 /*W3C标准*/
  transform: rotate | scale | skew | translate ;
  ```

  **CSS 内部hack 语法**是这样的 selector{<hack>?property:value<hack>?;}，上面代码所示的是在属性名称之前的hack，当然还有这样的

`background-color:green;`

属性前面加个“*”这样的写法只会对IE6、7生效，其它版本IE及现代浏览器会忽略这条指令（没有特殊说明，本文所有hack都是指在声明了DOCTYPE的文档的效果）

`-background-color:green;`
+ 属性前面有个“-”这样的只有IE6识别，还有些在后面的hack

`background-color:green!important;`
+ 这样在属性值后面添加“!important”的写法只有IE6不能识别，其它版本IE及现代浏览器都可以识别，还有“+”、“\0”、”\9” 等

## 选择器hack

+ 选择器hanck主要是针对IE浏览器，其实并不怎么常用，语法是这样的：<hack> selector{ sRules }

针对IE9的hack可以这么写

    :root .test
    {
        background-color:green;
    }

## HTML头部引用

HTML头部引用就比较特殊了，类似于程序语句，只能使用在HTML文件里，而不能在CSS文件中使用，并且只有在IE浏览器下才能执行，这个代码在非IE浏览下非单不是执行该条件下的定义，而是当做注释视而不见。
```
<!– 默认先调用css.css样式表 –>
<link rel="stylesheet" type="text/css" href="css.css" />
<!–[if IE 7]>
<!– 如果IE浏览器版是7,调用ie7.css样式表 –>
<link rel="stylesheet" type="text/css" href="ie7.css" />
<![endif]–>
<!–[if lte IE 6]>
<!– 如果IE浏览器版本小于等于6,调用ie.css样式表 –>
<link rel="stylesheet" type="text/css" href="ie.css" />
<![endif]–>
```

lte：就是Less than or equal to的简写，也就是小于或等于的意思。

lt ：就是Less than的简写，也就是小于的意思。

gte：就是Greater than or equal to的简写，也就是大于或等于的意思。

gt ：就是Greater than的简写，也就是大于的意思。

! ：就是不等于的意思，跟javascript里的不等于判断符相同。

## 书写顺序

看看，看看，这么多姿势，那么一个效果，好多种写法，什么顺序写才能保证各个浏览器都得到希望的效果呢？因为CSS只要是同一级别，出现重复属性设置，后出现的会覆盖前面出现的，所以在书写的时候一般把识别能力强的写前面，看个例子

`_background-color:red;`   
`background-color:green;`  
如果希望DIV在IE6上是红色，其它是绿色，上面的写法可不可以呢？试一下发现所有浏览器上都是绿色，因为在IE6解析的时候，第一句能够识别，背景设为红色，但是第二句所有浏览器都识别，IE6也不例外，背景颜色又被设为绿色，所以得反过来写

`background-color:green;`  
`_background-color:red;`

总结出的规律就是：**先一般，再特殊**。有兴趣的同学可以试试试试下面CSS，看看和你想的效果是否一样

`background-color:blue; /*所有浏览器*/`  
`background-color:red\9;/*所有的ie*/`  
`background-color:yellow\0; /* ie8+*/`  
`+background-color:pink; /*+ ie7*/`