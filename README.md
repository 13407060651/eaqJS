# eaqJS - 不一样的JS库
> 注意！此JS库正在进行简单改版，未来可能会看到更完整的版本哦！

还在用原生JS复杂的指令吗？或许你会说：“jQuery比你这垃圾强多了！”但是，jQuery虽然语法简洁，但是学起来不是特别容易。所以，如果你不喜欢jQuery等库，可以试试我的库。
* 100%原生JS
* 可以生成随机数
* 发送AJAX请求
* 使用`Now()`函数
* 进行各种加密 <span style="color:red;">还在开发！</span>
## 发送AJAX请求 ##
在原生JS里，发送AJAX函数是多么的麻烦：
```
//这里以GET请求举例，外部文件为get.php
var xmlhttp;
if(window.XMLHttpRequest){
    xmlhttp=new XMLHttpRequest();
}
else {
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.onreadystatechange=function(){
    if(xmlhttp.readyState==4&&xmlhttp.status==200){
        document.write(xmlhttp.responseText);
    }
    if(xmlhttp.readyState==4&&xmlhttp.status!=200){
        document.write("AJAX error:"+xmlhttp.status);
    }
}
xmlhttp.open("GET","get.php?ajax=yes&method=get",true);
xmlhttp.send();
```
而在eaqJS里，我们将其做到最简，并且支持在readyState改变时执行函数，执行的函数还能获得到readyState、status和responseText
```
//举例GET为get.php POST为post.php
ajax.get("get.php?ajax=yes&method=get",function(a,b,c){if(a==4) document.write(c)});
ajax.post("post.php?ajax=tes&method=post","Name=C&Gender=Female",function(a,b,c){if(a==4) document.write(c)});
```
看吧，使用`ajax.get()`函数是多么简单！那么，GET请求中各个值是什么意思呢？让我解释解释吧！

内容 | 解释 | 变量类型
--- | --- | ---
url | 这里为发送的url，为相对路径。发送绝对路径请注意==AJAX跨域==问题。 | string
readyfunction | 当readyState改变时，执行这个函数。当然，定义好局部函数A,B,C可以分别获取readyState、status和responseText | function
那么，`ajax.post()`函数呢？别急~这里也有！

内容 | 解释 | 变量类型
--- | --- | ---
url | 这里为发送的url，为相对路径。发送绝对路径请注意==AJAX跨域==问题。同样可以用GET请求的内容! | string
value | 发送POST请求的内容，格式和GET请求一样。 | string
readyfunction | 当readyState改变时，执行这个函数。当然，定义好局部函数A,B,C可以分别获取readyState、status和responseText | function

对于喜欢JSON的JSON党，我们也给了对应的方式！
```
//举例GET为get.php POST为post.php
ajax._get({
    url:"get.php?ajax=yes&method=get",
    onchange:function(a,b,c){
        console.log("readyState:"+a+" status:"+b);
    },
    onsuccess:function(a,b,c){
        document.write(c);
    },
    onerror:function(a,b,c){
        document.write("Unable to request ajax!\nreadyState:"+a+"\nstatus");
    }
});
ajax._post({
    url:"post.php?ajax=yes&method=get",
    HType:"Content-type",
    HContent:"application/www-form-encoded",
    value:"Name=C&Gender=Female",
    onchange:function(a,b,c){
        console.log("readyState:"+a+" status:"+b);
    },
    onsuccess:function(a,b,c){
        document.write(c);
    },
    onerror:function(a,b,c){
        document.write("Unable to request ajax!\nreadyState:"+a+"\nstatus");
    }
});
```
先让我解释一下它们的意思。

内容 | 解释 | 类型
--- | --- | ---
url | 这里为发送的url，为相对路径。发送绝对路径请注意==AJAX跨域==问题。 | string
HType | 发送数据类型类。默认为`Content-type`。 | string
HContent | 发送数据类型信息。默认为`application/www-form-encoded`。 | string
value | POST请求的值，根据HType决定发送什么。默认和GET请求格式一样 | Custom
onchange | 在readyState改变时执行的函数。 | function
onerror | 在出错时执行的函数。注：readyState为4，status不为200即为出错 | function
onsuccess | 在成功时执行的函数。注：readyState为4，status为200即为成功 | function
无论是`onchange`、`onerror`还是`onsuccess`，都可以通过3个值获得信息
值位置 | 获得的值 | 解释 | 类型
--- | --- | --- | ---
1 | xmlhttp.readyState | ajax请求的readyState。不懂的请学学ajax | number
2 | xmlhttp.status | ajax请求的statu值。200为成功，404为找不到，500为服务器出错 | number
3 | xmlhttp.responseText | ajax请求所返回的内容，若是数字请先`parseInt()`。 | string
## 使用`Now()`函数 ##
在JavaScript里，想获取日期是多么的麻烦！
1. new一个`Date()`对象
2. 创建一个空变量自己合成年月日
3. 月要+1

这里，我们变得更简单了。
```
//不区分大小写
Now() //直接获取Date()变量
Now("year") //获取年，可以改成month,day,hour,minute,second,unix
Now("date") //获取打包好的日期，可以改成time
//区分大小写，根据new好的对象获取日期，不支持直接打包
Dt.year //同上
Dt.jsons //以JSON返回日期时间。
```
英语和JS好的人一看就懂了，可一般人很难懂，所以还是得解释一下
函数 | 解释 | 类型 | 例子
--- | --- | --- | ---
`Now("Year")` | 获取当前年份。 | number | 2006`Now("month")` | 获取当前月份，不用+1 | number | 6
`Now("day")` | 获取当前日。 | number | 12
`Now("hour")` | 获取当前小时 | number | 19
`Now("minute")` | 获取当前分钟 | number | 30
`Now("second")` | 获取当前秒 | number | 29
`Now("unix")` | 获取当前UNIX时间戳，不懂得自行Google | number | 1150111829
`Now("date")` | 获取打包好的日期，可以用`Now("Date").split("/")`切割成年月日 | string | 2006/6/12
`Now("time")` | 获取打包好的时间 | string | 19:29:30
当然，对象法支持以上除打包日期时间以外的所有功能，还可以以JSONS集体返回信息。
## 生成随机内容 ##
### 随机数字(整数) ###
对于随机，JS原生仅仅支持`Math.random();`来生成0到1的随机小数，想要生成指定数字，十分麻烦。在这里我们简化了其操作！仅需`rand(min,max);`即可生成制定范围的数字。
变量 | 解释 | 类型
--- | --- | ---
min | 随机数字最小值，必须是整数。 | number
max | 随机数字最大值，必须是整数。 | number
啥？怎么生成小数？只要把min和max乘以10，然后结果除以10就可以了！
```javascript
rand(100,200);//一百到二百的整数
rand(0,612)/1000; //0到0.612的三位小数
```
### 随机数字串 ###
上面生成的是数字，可是有时候人们往往需要随机数字串（比如QQ号），在也有它的函数哦~
只需`Random.snum(long);`即可生成！`long`为数字串的长度，必须是整数（1.5个数字？）。
举例：
```javascript
var Result = Random.(10); //输出"2477819731" 此处为例子，是我的QQ号
```
### 随机字母串 ###
有时候，随机数字不够用，那么加上字母，就可以做出更多可能性了！比如激活码、用户唯一辨识KEY、分享文件、中低强度密码时随机生成的分享链接。想生成空格的话，请自行在源代码里修改！

可以用`Random.sword(long);`生成！`long`为字母串的长度！
```javascript
var Result0 = Random.sword(5); //输出"S4m5O" 此处为例子，无意义
_sword_characters+=" "; //加入空格
var Result1 = Random.sword(13); //输出"I love Hanhan" 此处为例子
```
### 随机ASCII字符串 ###
ASCII字符表包涵了数字、字母和许多特殊字符，一般不用于用户ID、激活码。而用于高强度随机密码、安全的验证KEY。想生成空格，请在源代码内修改~

可以用`Random.ascii(long);`生成！`long`为ASCII字符串的长度。
```javascript
Random.ascii(17); //输出"Fav0ur|te:Hanhan!" 此处为例子
_ascii_characters+=" "; //加入空格
Random.ascii(25); //输出"HI! i Love y0U@! @Hanhan!" 此处为例子
```
### 随机汉字字符 ###
正在开发...
## 日志 ##
* \[2018-1-25\] 版本1
  1. 插件成立
  2. 支持不完善的`Now()`函数
  3. 支持获取关于信息
  4. 支持`debug模式`（并没有什么卵用）
* \[2018-1-27\] 版本2
  1. 完善关于信息
  2. 完善`Now()`函数
  3. 为喜欢`new`对象的人`new`了个Dt来获取日期
  4. 支持ajax以get和post方式
* \[2018-1-28\] 版本3
  1. 修正各种Bug
  2. ajax支持用对象来集成信息
* \[2018-1-29\] 版本3 - new
  1. 加入了生成随机数功能
  2. 支持生成随机字符串
  3. 支持生成随机ASCII编码串

## 源代码 ##
这里是GitHub - - 直接下载JS就可以了...
## 结束 #
这就是eaqJS了！如果有BUG、建议可以联系我！不说了，我要去借助这个JS制作自己的网站去了！
QQ: 2477819731 ~~文章里出现过~~ Email:[dffzmxj@qq.com](mailto:dffzmxj@qq.com)
