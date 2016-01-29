JavaScript 对象属性及方法
===
### 数组(Array)：系列元素的有序集合；
**属性：**
length：用于获取数组元素的个数，既最大下标加 1
**方法：**
sort(function):在未指定排序号的情况下,按照元素的字母顺序排列,如果不是字符串类型则转换成字符串,在排序;
reverse():颠倒数组中元素的顺序；
concat(array1,arrayn):用于将N个数组合并到array1数组中；
join(string):用于将数组中元素合并为字符串，string为分隔符，如省略参数，则直接合并，不加分隔；
slice(start,stop):用于返回数组中start到stop中的元素，如果参数为负，则表示倒数start或stop个元素；
toString():将数组所有元素返回一个字符串，其间用逗号分隔；

### 字符串（string）
**属性**：
length:用于返回字符串的长度，用法与数组一样；
**方法：**
anchor():该方法创建如同html中的anchor一样的标记，格式 ,通过下列方法访问 string.anchor(chorName)
例如 document.write("到天轰穿.net\ajax\atlas 博客察看更多教程"+myLink.link("http://www.cnblogs.com/thcjp/"))
toUpperCase():将字符串转换成大写；
toLowerCase():将字符串转换成小写；
indexOf(a,b):从第 b 个字符查找字符 a 在字符串中出现的位置并返回,如果 b 省略，则默认从 0 位置查找；
chartAt(i):返回字符串中第 i 个字符；
substring(start,end):返回字符串中从 start - end 之间的全部字符（但是不返回end本身那个字符哦）；
sub():将指定的字符串用下标格式显示;

### 日期(Date)
**定义方法：**
- var newdt=new Date() -->创建时间对象并赋值为当前时间;
- var newdt=new Date(milliseconds) --> 创建时间对象，且以GTM的延迟时间来设置对象的值，单位为毫秒；
- var newdt=new Date(string) -->使用特定的时间字符串为新创建的时间对象赋值，其格式与Date对象的parse方法匹配；
- var newdt=new Date(年,月,日,小时,分,秒,毫秒) -->按照年,月,日,小时,分,秒,毫秒 的顺序为对象赋值;

**方法: 分 获取时间;设置时间;格式转换**

**A:获取时间**
getDate() -----获取当前完整时间;
getYear()------获取当前的年
getMonths()----获取当前的月份
getDay()-------获取当前的日期 天
getHours()-----获取当前的小时
getMinutes()---获取当前的分钟
getSeconds()---获取当前的秒
getTime()------获取当前的时间，单位 秒
getTimeZoneOffset--获取当前的时区偏移信息

**b：设置时间**
对照上面的获取，把get换成 set 即可，例如 getDate() ---> setDate()

**c:转换方法**
toGTMString() ------转换成格林威治标准时间表达式的字符串；
toLocaleString()----转换成当地时间表达的字符串
toString()----------把时间转换成字符串
parse---------------从表示时间的字符串中读出时间
UTC-----------------返回从格林威治标准时间到指定时间的差距，单位为 毫秒

### Math 数学
**属性**：注意，数学对象中的属性是指读的
E (=2.7182) ------自然对数的底（具体意思，我不明白，唉，和数学密切的东西我都不明白，郁闷！）
LN10(=2.30259) ---10的自然对数
LN2(=0.69315)-----2的自然对数；
PI(=3.1415926)----圆周
SQRT1_2(=0.7071)--1/2的平方根
SQRT2(=1.4142)----2的平方根
LOG2E(=1.44269)---以2为底,E的对数
LOG10E(=0.43429)--以10为底E的对数

**方法**
sin(a) ---- 求a的正弦值
cos(a)------求a的余弦值
tan(a)------求a的正切值
asin(a)-----求a的反正弦值
atan(a)-----求a的反余弦值
exp(a)------求a的指数
log(a)------求a的自然对数
Pow(a,i)----求a的i次方（乘方）
round(a)----对a进行四舍五入运算
sqrt(a)-----求a的平方根
abs(a)------求a的绝对值
random()----取随机数
max(a,b)----取较大的数
min(a,b)----取较小的数

**注意：函数的参数均是浮点类型，三角函数的参数为弧度值，而不是度**

### javascript的内置函数

escape() 与 unescape() :对字符串进行 编码与解码
eval(字符串):用于执行字符串所代表的运算或语句
例如：var a=0; var str1="a+=a"; eval(str1);

parseInt() 和 parseFloat():将文本框的值转换成整数 或 浮点数
注意：parseInt()不是对数字进行四舍五入操作，而是切尾

isNaN():完整的E文是（is not a number），顾名思义是 判断字符串是否是数字，例如 if(isNaN("天轰穿系列教程"))

### 自定义对象:有初始化对象和定义构造函数的对象两种方法
**a：初始化对象**
例如： 对象={属性1:值1;属性2:值2;......属性n:值n} ，注意，每个属性\值对之间用分号隔开；

**b： 定义构造函数的对象**
例如:
function 函数名(属性1，属性2，。。。属性N){

this.属性1=属性值1；
this.属性2=属性值2；
this.属性n=属性值n；

this.方法名1=函数名1；
this.方法名2=函数名2；

}

注意：方法名和函数名可以同名，但是在方法调用函数前，函数必须已经定义好，否则会出错

为自定义的函数创建新的实例一样是使用 new 语句。

### 浏览器对象
#### window对象：他属于处于所有对象的最高级

**属性**：主要的有如下
closed----------用于判断窗口是否关闭；
opener----------存放open()方法打开窗口的父口；
defaultstatus---状态栏默认显示的信息；
status----------状态栏当前显示的信息
Document,Location,History---很重要，稍后详细说，要是不想等，直接看这里

**方法**：
alert(text)-------------弹出一个提示信息框
confirm(text)-----------确认信息框，参数为确认信息
prompt(text,default)----弹出输入对话框,参数为提示信息和缺省值

### document对象：包括当前网页的各种特征,如标题\URL\背景\语言\修改时间等

**属性:**
title------------文档标题
lastModified-----文件最后修改时间
URL--------------文档对应的页面地址
Cookie-----------用来创建和获取Cookie信息
bgColor----------文档的背景色
fgColor----------文档的前景色
location---------保存文档所有的页面地址信息
alinkcolor-------激活连接的颜色
linkcolor--------链接的颜色
vlinkcolor-------已浏览过的链接的颜色

**方法：**
write(text)-----向文档写入文字或标签，不换行
writeln(text)---向文档写入文字或标签,在最后一个字符处换行
open()----------打开一个新文档 例如 open("地址","窗口名字","样式")
close()---------关闭当前文档

### Location对象： 包含当前文档所有的页面地址信息
**属性：**
protocol-----------通信协议
host---------------页面所在web服务器的主名称
port---------------服务器通信的端口号
pathname-----------文档在服务器上的路
hash---------------页面跳转的锚标记信息
searce-------------页面提交到服务器上搜索的信息
hostname-----------主机的名称和端口号，中间用冒号隔开
href---------------完整的URL地址

**方法：**
assign(URL)--------将页面导航到另一个地址上去
reload-------------刷新页面
replace(URL)-------使用指定URL的页面代替当前页面

### History：该对象包括以前访问过的URL信息

属性 ：length,返回URL数量，方法主要是 go(n) ,通过该方法载入相对的页面
方法: pushState()