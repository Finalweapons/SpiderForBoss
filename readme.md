<h2>项目进度:</h2>
<p>JS解密只做到了获取JS文件,并没有作后续的解密,目前已经发现的是在第一次请求页面的Get请求时,给定requests不要自动重定向url的属性,即可在请求的返回头中一个名为location的属性中找到加密所需的参数</p>
<p>其中,seed参数为服务端给出,ts为当前时间的毫秒数,name为加密Cookie所需要用的JS文件的文件名称.而整个location的值在前方加上Boss的域名即为可以<br>式返回页面内容的get请求的Header中的Referer参数</p>
<p>加密JS的名字大约24小时左右会进行变化,而js文件中的内容也会有所变动,函数本身的加密方式不会进行改变,但是里面加密函数的参数时会发生变化的</p>
<p>由于不是很懂JavaScript加上加密JS做了代码混淆,所以我后续选择了使用Selenium来直接获取Cookie,如果你是一名JS大佬,可以尝试调试分析下</p>
<p>其实一开始也做了使用execjs直接执行js文件来获取zpstoken的尝试,但是js中需要用到浏览器里的window对象,而execjs是使用的nodejs的引擎,并且网上使用npm安装jsdom的方法也没有奏效,这才转向了Selenium</p>
<p>不得不说这个Boss的加密是真的神奇,现在采用的方式是使用Selenium打开页面获取Cookies后将Cookies转交给Requests去请求页面内容,但是有的时候Selenium获取到的Cookie居然不能用,目前猜测应该是Boss的服务端会根据请求内容决定Cookies的过期时间,因为在请求页面的时候会向服务端发送几个没有返回值的get请求,里面会提交当前页面的url以及各项参数等等.并且在GitHub上看其他人的项目也有提到使用加密JS生成出来的zpstoken不能用的情况</p>
<p>目前已经写了解析页面的函数:parserhtml,后续会继续更新将解析出来的数据生成词云等</p>
<p>写了一个全Selenium版本,效率没有想象中的那么低下,没开无头模式,有需要的可以自行开启.<br>全Selenium版本会自动将数据存到Data.xlsx中,并且针对所需的技术栈生成一张词云图,名为test.png</p>
<h2>原理解析</h2>
<p>Boss直聘每次请求页面都会先重定向到security-check.html并且加载一次加密JS,经过该页面加密(生成设置Cookie)和检测后,会再次跳转重定向到原url的界面.其生成的Cookie中__zp__stoken和acw_tc为两个关键值,acw是访问页面后服务器返回的,而stoken则是在客户端加密后产生的
且stoken的过期时间大概有几个小时,所以这里我为了提升效率使用selenium获取一次Cookie后就直接用requests来请求页面了
</p>
<p>全Selenium版本,请求页面后获取浏览器加载解析后的源码并加以解析,更加方便快捷,而且效率损失不大</p>
<h2>使用方法</h2>
<p>预先安装好需要的模块,如Selenium,wordcloud等,具体的可以打开py文件看上面的引入</p>
<p>建议直接运行全Selenium版.py即可</p>
<img src="test.png">
