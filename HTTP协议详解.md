# HTTP协议

## URL定义

> URL(Uniform Resource Locator) 地址用于描述一个网络上的资源,  基本格式如下
>
> schema://host[:port#]/path/.../[?query**-**string]\[\#anchor]

- scheme               指定低层使用的协议(例如：http, https, ftp)
- host                   HTTP服务器的IP地址或者域名
- port#                 HTTP服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/
- path                   访问资源的路径
- query**-**string       发送给http服务器的数据
- anchor**-**             锚

### URL 的一个例子

http://www.mywebsite.com/sj/test/test.aspx?name=sviergn&x=true#stuff

- Schema:                 http
- host:                   www.mywebsite.com
- path:                   /sj/test/test.aspx
- Query String:           name=sviergn&x=true
- Anchor:                 stuff

## HTTP特点

1. 支持客户/服务器模式。支持基本认证和安全认证。 

2. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

3. 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。

4. 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

5. 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理==没有记忆能力==。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

   **HTTP协议是无状态的和Connection: keep无状态的面向连接的协议，无状态不代表HTTP不能保持TCP连接，更不能代表HTTP使用的是UDP协议（无连接）**

   从HTTP/1.1起，默认都开启了Keep**-**Alive，保持连接特性，简单地说，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。 Keep-Alive不会永久保持连接它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间 

    

## HTTP消息结构

### Request消息结构

1. Request Line

2. Request Header

3. Body

   > Body和Header之间有个空行

![1620612931241](C:\Users\ZWX945~1\AppData\Local\Temp\1620612931241.png)

![1620617644077](F:\zpyWorkspace\Paper\1620617644077.png)

#### Request Line请求行	

  - Method   请求方法 

    - GET         请求获取Request-URI所标识的资源  
    - POST      在Request-URI所标识的资源后附加新的数据 常用于提交表单
    - HEAD     请求获取由Request-URI所标识的资源的响应消息报头  
    - PUT         请求服务器存储一个资源，并用Request-URI作为其标识  DELETE  请求服务器删除Request-URI所标识的资源  
    - TRACE    请求服务器回送收到的请求信息，主要用于测试或诊断  
    - CONNECT 保留将来使用  
    - OPTIONS  请求查询服务器的性能，或者查询与资源相关的选项和需求 

  - path-resource   请求的资源

  - Version-number  HTTP协议的版本

    > 示例：GET  http://www.cnblogs.com/ HTTP/1.1 

#### Request Header消息报头

> HTTP消息报头包括普通报头、请求报头、响应报头、实体报头。 
>
>  每一个报头域都是由名字+“：”+空格+值 组成，消息报头域的名字是大小写无关的

1. 普通报头

   > 在普通报头中，有少数报头域用于所有的请求和响应消息，但并不用于被传输的实体，只用于传输的消息。

   - che-Control   用于指定缓存指令，缓存指令是单向的（响应中出现的缓存指令在请求中未必会出现），且是独立的（一个消息的缓存指令不会影响另一个消息处理的缓存机制），HTTP1.0使用的类似的报头域为Pragma。

     - 请求时的缓存指令包括：no-cache（用于指示请求或响应消息不能缓存）、no-store、max-age、max-stale、min-fresh、only-if-cached;

     -  响应时的缓存指令包括：public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age、s-maxage.

       ```python
       eg：为了指示IE浏览器（客户端）不要缓存页面，服务器端的JSP程序可以编写如下：response.sehHeader("Cache-Control","no-cache");
       //response.setHeader("Pragma","no-cache");作用相当于上述代码，通常两者//合用这句代码将在发送的响应消息中设置普通报头域：Cache-Control:no-cache
       ```

   - Date普通报头域表示消息产生的日期和时间

   - Connection普通报头域允许发送指定连接的选项。例如指定连接是连续，或者指定“close”选项，通知服务器，在响应完成后，关闭连接

   

2. 请求报头

   > 请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息。

   - Accept  Accept     用于指定客户端接受哪些类型的信息。

      ```python
      eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；
      	Accept：text/html，表明客户端希望接受html文本。
      ```

   - Accept-Charset     用于指定客户端接受的字符集。

     ```python
      eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。  
     ```

   - Accept-Encoding  类似于Accept，但是它是用于指定可接受的内容编码。

     ```python
      eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。  
     ```

   - Accept-Language  类似于Accept，但是它是用于指定一种自然语言。

      ```python
      eg：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。  
      ```

    - Authorization  主要用于证明客户端有权查看某个资源。

      ```python
      当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。 
      ```

   - Host（发送请求时，该报头域是==必需==的）  主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的

      ```python
      eg：  我们在浏览器中输入：<http://www.guet.edu.cn/index.html>  浏览器发送的请求消息中，就会包含Host请求报头域，如下：  Host：[www.guet.edu.cn](http://www.guet.edu.cn/)  此处使用缺省端口号80，若指定了端口号，则变成：Host：[www.guet.edu.cn](http://www.guet.edu.cn/):指定端口号  
      ```

   - User-Agent允许客户端将它的操作系统、浏览器和其它属性告诉服务器。

      ```python
      我们上网登陆论坛的时候，往往会看到一些欢迎信息，其中列出了你的操作系统的名称和版本，你所使用的浏览器的名称和版本，实际上，服务器应用程序就是从User-Agent这个请求报头域中获取到这些信息。不过，这个报头域不是必需的，如果我们自己编写一个浏览器，不使用User-Agent请求报头域，那么服务器端就无法得知我们的信息了。
      ```

3. 响应报头

   > 响应报头允许服务器传递不能放在状态行中的附加响应信息，以及关于服务器的信息和对Request-URI所标识的资源进行下一步访问的信息。  

   - Location  用于重定向接受者到一个新的位置。Location响应报头域常用在更换域名的时候。  

   - Server  Server 包含了服务器用来处理请求的软件信息。与User-Agent请求报头域是相对应的。

     ```PYTHON
     Server：Apache-Coyote/1.1   
     ```

   - WWW-Authenticate 必须被包含在401（未授权的）响应消息中，客户端收到401响应消息时候，并发送Authorization报头域请求服务器对其进行验证时，服务端响应报头就包含该报头域。  

     ```PYTHON
     WWW-Authenticate:Basic realm="Basic Auth Test!"  //可以看出服务器对请求资源采用的是基本验证机制。 
     ```

4. 实体报头

   > 请求和响应消息都可以传送一个实体。一个实体由实体报头域和实体正文组成，但并不是说实体报头域和实体正文要在一起发送，可以只发送实体报头域。实体报头定义了关于实体正文（eg：有无实体正文）和请求所标识的资源的元信息。 

   - Content-Encoding 被用作媒体类型的修饰符，它的值指示了已经被应用到实体正文的附加内容的编码，因而要获得Content-Type报头域中所引用的媒体类型，必须采用相应的解码机制。Content-Encoding这样用于记录文档的压缩方法

     ```python
     Content-Encoding：gzip 
     ```

   - Content-Language描述了资源所用的自然语言。没有设置该域则认为实体内容将提供给所有的语言阅读者。

     ```python
     Content-Language:da  
     ```

   - Content-Length 指明实体正文的长度，以字节方式存储的十进制数字来表示。  

   - Content-Type  指明发送给接收者的实体正文的媒体类型。

     ```python
     Content-Type:text/html;charset=ISO-8859-1  
     
     Content-Type:text/html;charset=GB2312  
     ```

   - Last-Modified 指示资源的最后修改日期和时间。  

   - Expires  Expires实体报头域给出响应过期的日期和时间。为了让代理服务器或浏览器在一段时间以后更新缓存中(再次访问曾访问过的页面时，直接从缓存中加载，缩短响应时间和降低服务器负载)的页面，我们可以使用Expires实体报头域指定页面过期的时间。

     ```python
     Expires：Thu，15 Sep 2006 16:23:12 GMT  
     ```

     HTTP1.1的客户端和缓存必须将其他非法的日期格式（包括0）看作已经过期。

     ```python
     eg：为了让浏览器不要缓存页面，我们也可以利用Expires实体报头域，设置为0，jsp中程序如下：
     response.setDateHeader("Expires","0");
     ```

     

#### Body消息正文



### Response消息结构

![1620615533664](F:\zpyWorkspace\Paper\1620615533664.png)

![1620617625905](F:\zpyWorkspace\Paper\1620617625905.png)

## GET 方法和 POST 方法的区别

> Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息. 

1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456.  POST方法是把提交的数据放在HTTP包的Body中.
2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.
3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。
4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.

 

## 状态码

> Response 消息中的第一行叫做状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成。
>
> 状态码用来告诉HTTP客户端,HTTP服务器是否产生了预期的Response.

HTTP/1.1中定义了5类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别

- 1XX  提示信息 - 表示请求已被成功接收，继续处理
- 2XX  成功 - 表示请求已被成功接收，理解，接受
- 3XX  重定向 - 要完成请求必须进行更进一步的处理
- 4XX  客户端错误 -  请求有语法错误或请求无法实现
- 5XX  服务器端错误 -   服务器未能实现合法的请求

状态码详解看下篇内容

- 200 OK 最常见的就是成功响应状态码200了， 这表明该请求被成功地完成，所请求的资源发送回客户端

- 302 Found重定向，新的URL会在response 中的Location中返回，浏览器将会自动使用新的URL发出新的Request

  例如在IE中输入， http://www.google.com. HTTP服务器会返回302， IE取到Response中Location header的新URL, 又重新发送了一个Request.

  ![1620617557112](F:\zpyWorkspace\Paper\1620617557112.png)

- 304 Not Modified 代表上次的文档已经被缓存了， 还可以继续使用，如果不想使用本地缓存可以用Ctrl+F5 强制刷新页面 

  ![1620617574055](F:\zpyWorkspace\Paper\1620617574055.png)

- 400 Bad Request  客户端请求与语法错误，不能被服务器所理解

- 403 Forbidden 服务器收到请求，但是拒绝提供服务

- 404 Not Found 请求资源不存在（输错了URL）比如在IE中输入一个错误的URL， http://www.cnblogs.com/tesdf.aspx

  ![1620617593318](F:\zpyWorkspace\Paper\1620617593318.png)

- 500 Internal Server Error 服务器发生了不可预期的错误

- 503 Server Unavailable 服务器当前不能处理客户端的请求，一段时间后可能恢复正常

------



## 解决HTTP无状态的问题

### 通过Cookies保存状态信息

通过Cookies，服务器就可以清楚的知道请求2和请求1来自同一个客户端。![img](http://3ms.huawei.com/km/static/blog/images/gif/grey.gif)

### 通过Session保存状态信息

Session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。当程序需要为某个客户端的请求创建一个session的时候，服务器首先检查这个客户端的请求里是否已包含了一个session标识 - 称为 session id，如果已包含一个session id则说明以前已经为此客户端创建过session，服务器就按照session id把这个 session检索出来使用（如果检索不到，可能会新建一个），如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个session id将被在本次响应中返回给客户端保存。

#### Session的实现方式

1. 使用Cookie来实现服务器给每个Session分配一个唯一的JSESSIONID，并通过Cookie发送给客户端。当客户端发起新的请求的时候，将在Cookie头中携带这个JSESSIONID。这样服务器能够找到这个客户端对应的Session。![img](http://3ms.huawei.com/km/static/blog/images/gif/grey.gif)

2. 使用URL回写来实现

   URL回写是指服务器在发送给浏览器页面的所有链接中都携带JSESSIONID的参数，这样客户端点击任何一个链接都会把JSESSIONID带会服务器。如果直接在浏览器输入服务端资源的url来请求该资源，那么Session是匹配不到的。

3. Tomcat对Session的实现，是一开始同时使用Cookie和URL回写机制，如果发现客户端支持Cookie，就继续使用Cookie，停止使用URL回写。如果发现Cookie被禁用，就一直使用URL回写。jsp开发处理到Session的时候，对页面中的链接记得使用response.encodeURL() 。

#### Cookie和Session的不同点

   1. Cookie将状态保存在客户端，Session将状态保存在服务器端；
   2. Cookies是服务器在本地机器上存储的小段文本并随每一个请求发送至同一个服务器。Cookie最早在RFC2109中实现，后续RFC2965做了增强。网络服务器用HTTP头向客户端发送cookies，在客户终端，浏览器解析这些cookies并将它们保存为一个本地文件，它会自动将同一服务器的任何请求缚上这些cookies。Session并没有在HTTP的协议中定义；
   3. Session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁用cookie时，这个值也可能设置为由get来返回给服务器；
   4. 就安全性来说：当你访问一个使用session 的站点，同时在自己机子上建立一个cookie，建议在服务器端的SESSION机制更安全些。因为它不会任意读取客户存储的信息。

### 通过表单变量保持状态

除了Cookies之外，还可以使用表单变量来保持状态，比如Asp.net就通过一个叫ViewState的Input=“hidden”的框来保持状态,比如:<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="/wEPDwUKMjA0OTM4MTAwNGRkXUfhlDv1Cs7/qhBlyZROCzlvf5U=" />这个原理和Cookies大同小异，只是每次请求和响应所附带的信息变成了表单变量。

### 通过QueryString保持状态

QueryString通过将信息保存在所请求地址的末尾来向服务器传送信息，通常和表单结合使用，一个典型的QueryString比如:www.xxx.com/xxx.aspx?var1=value&var2=value2

------



## HTTP应用

### 断点续传的实现原理

> HTTP协议的GET方法，支持只请求某个资源的某一部分；连接断开重连时，客户端只请求该资源未下载的部分，而不是重新请求整个资源，来实现断点续传。

- 206 Partial Content 部分内容响应；

- Range 请求的资源范围；

- Content-Range 响应的资源范围；在

  ```python
  Eg1：Range: bytes=306302- ：请求这个资源从306302个字节到末尾的部分；
  Eg2：Content-Range: bytes 306302-604047/604048：响应中指示携带的是该资源的第306302-604047的字节，该资源共604048个字节；
  ```

  客户端通过并发的请求相同资源的不同片段，来实现对某个资源的并发分块下载。从而达到快速下载的目的。目前流行的FlashGet和迅雷基本都是这个原理。

### 多线程下载的原理

> 下载工具开启多个发出HTTP请求的线程；每个http请求只请求资源文件的一部分：Content-Range: bytes 20000-40000/47000；合并每个线程下载的文件。

### http代理

> http代理服务器代理服务器英文全称是Proxy Server，其功能就是代理网络用户去取得网络信息。形象的说：它是网络信息的中转站。

代理服务器是介于浏览器和Web服务器之间的一台服务器，有了它之后，浏览器不是直接到Web服务器去取回网页而是向代理服务器发出请求，Request信号会先送到代理服务器，由代理服务器来取回浏览器所需要的信息并传送给你的浏览器。而且，大部分代理服务器都具有==缓冲==的功能，就好象一个大的Cache，它有很大的存储空间，它不断将新取得数据储存到它本机的存储器上，如果浏览器所请求的数据在它本机的存储器上已经存在而且是最新的，那么它就不重新从Web服务器取数据，而直接将存储器上的数据传送给用户的浏览器，这样就能显著提高浏览速度和效率。

更重要的是：Proxy Server(代理服务器)是Internet链路级网关所提供的一种重要的==安全功能==，它的工作主要在开放系统互联(OSI)模型的对话层。

#### http代理服务器的主要功能

1. 突破自身IP访问限制，访问国外站点。如：教育网、169网等网络用户可以通过代理访问国外网站；
2. 访问一些单位或团体内部资源，如某大学FTP(前提是该代理地址在该资源的允许访问范围之内)，使用教育网内地址段免费代理服务器，就可以用于对教育网开放的各类FTP下载上传，以及各类资料查询共享等服务；
3. 突破中国电信的IP封锁：中国电信用户有很多网站是被限制访问的，这种限制是人为的，不同Serve对地址的封锁是不同的。所以不能访问时可以换一个国外的代理服务器试试；
4. 提高访问速度：通常代理服务器都设置一个较大的硬盘缓冲区，当有外界的信息通过时，同时也将其保存到缓冲区中，当其他用户再访问相同的信息时，则直接由缓冲区中取出信息，传给用户，以提高访问速度；
5. 隐藏真实IP：上网者也可以通过这种方法隐藏自己的IP，免受攻击。对于客户端浏览器而言，http代理服务器相当于服务器。而对于Web服务器而言，http代理服务器又担当了客户端的角色。

### 虚拟主机

> 虚拟主机：是在网络服务器上划分出一定的磁盘空间供用户放置站点、应用组件等，提供必要的站点功能与数据存放、传输功能。

 所谓虚拟主机，也叫“网站空间”就是把一台运行在互联网上的服务器划分成多个“虚拟”的服务器，每一个虚拟主机都具有独立的域名和完整的Internet服务器（支持WWW、FTP、E-mail等）功能。一台服务器上的不同虚拟主机是各自独立的，并由用户自行管理。但一台服务器主机只能够支持一定数量的虚拟主机，当超过这个数量时，用户将会感到性能急剧下降。虚拟主机的**实现原理**虚拟主机是用同一个WEB服务器，为不同域名网站提供服务的技术。Apache、Tomcat等均可通过配置实现这个功能。相关的HTTP消息头：Host。例如：Host: www.baidu.com客户端发送HTTP请求的时候，会携带Host头，Host头记录的是客户端输入的域名。这样服务器可以根据Host头确认客户要访问的是哪一个域名。

------

## HTTP认证方式

> HTTP请求报头： Authorization
>
> HTTP响应报头： WWW-Authenticate
>
> HTTP认证是基于质询/回应(challenge/response)的认证模式。

### 基本认证 basic authentication

> 基本认证是一种用来允许Web浏览器或其他客户端程序在请求时提供用户名和口令形式的身份凭证的一种登录验证方式。把 "用户名+冒号+密码"用BASE64算法加密后的字符串放在http request 中的header Authorization中发送给服务端。

客户端对于每一个realm，通过提供用户名和密码来进行认证的方式。包含密码的明文传递。当浏览器访问使用基本认证的网站的时候， 浏览器会提示你输入用户名和密码，假如用户名密码错误的话，服务器会返回401

#### 基本认证步骤

1. 客户端访问一个受http基本认证保护的资源。

2. 服务器返回401状态，要求客户端提供用户名和密码进行认证。（验证失败的时候，响应头会加上WWW-Authenticate: Basic realm="请求域"。）401 UnauthorizedWWW-Authenticate： Basic realm="WallyWorld"

3. 客户端将输入的用户名密码用Base64进行编码后，采用非加密的明文方式传送给服务器。Authorization: Basic xxxxxxxxxx.

4. 服务器将Authorization头中的用户名密码解码并取出，进行验证，如果认证成功，则返回相应的资源。如果认证失败，则仍返回401状态，要求重新进行认证。


#### 特记事项

1. Http是无状态的，同一个客户端对同一个realm内资源的每一个访问会被要求进行认证。

2. 客户端通常会缓存用户名和密码，并和authentication realm一起保存，所以，一般不需要你重新输入用户名和密码。

3. 以非加密的明文方式传输，虽然转换成了不易被人直接识别的字符串，但是无法防止用户名密码被恶意盗用。虽然用肉眼看不出来，但用程序很容易解密。

#### 优点

基本认证的一个优点是基本上所有流行的网页浏览器都支持基本认证。基本认证很少在可公开访问的互联网网站上使用，有时候会在小的私有系统中使用（如路由器网页管理接口）。后来的机制HTTP**摘要认证**是为替代基本认证而开发的，允许密钥以相对安全的方式在不安全的通道上传输。

程序员和系统管理员有时会在可信网络环境中使用基本认证，使用Telnet或其他明文网络协议工具手动地测试Web服务器。这是一个麻烦的过程，但是网络上传输的内容是人可读的，以便进行诊断。

#### 缺点

虽然基本认证非常容易实现，但该方案建立在以下的假设的基础上

1. 客户端和服务器主机之间的连接是安全可信的。特别是，如果没有使用SSL/TLS这样的传输 层安全的协议，那么以明文传输的密钥和口令很容易被拦截。

2. 该方案也同样没有对服务器返回的信息提供保护。现存的浏览器保存认证信息直到标签页或浏览器被关闭，或者用户清除历史记录。HTTP没有为服务器提供一种方法指示客户端丢弃这些被缓存的密钥。这意味着服务 器端在用户不关闭浏览器的情况下，并没有一种有效的方法来让用户登出。

   

### 摘要认证 digest authentication

> *这个认证可以看做是基本认证的增强版本，不包含密码的明文传递。引入了一系列安全增强的选项；“保护质量”(qop)、随机数计数器由客户端增加、以及客户生成的随机数。* 

在HTTP摘要认证中使用 MD5 加密是为了达成"不可逆的"，也就是说，当输出已知的时候，确定原始的输入应该是相当困难的。如果密码本身太过简单，也许可以 通过尝试所有可能的输入来找到对应的输出（穷举攻击），甚至可以通过字典或者适当的查找表加快查找速度。 

#### 摘要认证步骤

1. 客户端请求一个需要认证的页面，但是不提供用户名和密码。通常这是由于用户简单的输入了一个地址或者在页面中点击了某个超链接。

2. 服务器返回401 "Unauthorized" 响应代码，并提供认证域(realm)，以及一个随机生成的、只使用一次的数值，称为密码随机数 nonce。

3. 此时，浏览器会向用户提示认证域(realm)（通常是所访问的计算机或系统的描述），并且提示用户名和密码。用户此时可以选择取消。

4. 一旦提供了用户名和密码，客户端会重新发送同样的请求，但是添加了一个认证头包括了响应代码。注意：客户端可能已经拥有了用户名和密码，因此不需要提示用户，比如以前存储在浏览器里的。

   示例

   ```
   客户端请求 (无认证)：
   GET /dir/index.html HTTP/1.0
   Host: localhost
   (跟随一个新行，形式为一个回车再跟一个换行）
   ```

   ```
   服务器响应：
   HTTP/1.0 401 Unauthorized
   Server: HTTPd/0.9
   Date: Sun, 10 Apr 2005 20:26:47 GMT
   WWW-Authenticate: Digest realm="testrealm@host.com",   //认证域
                           qop="auth,auth-int",   //保护质量
                           nonce="dcd98b7102dd2f0e8b11d0f600bfb0c093",  //服务器密码随机数
                           opaque="5ccc069c403ebaf9f0171e9517f40e41"
   Content-Type: text/html
   Content-Length: 311
   
   <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
    "http://www.w3.org/TR/1999/REC-html401-19991224/loose.dtd">
   <HTML>
     <HEAD>
       <TITLE>Error</TITLE>
       <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=ISO-8859-1">
     </HEAD>
     <BODY><H1>401 Unauthorized.</H1></BODY>
   </HTML>
   ```

   ```
   客户端请求 (用户名 "Mufasa", 密码 "Circle Of Life")：
   GET /dir/index.html HTTP/1.0
   Host: localhost
   Authorization: Digest username="Mufasa",
                        realm="testrealm@host.com",
                        nonce="dcd98b7102dd2f0e8b11d0f600bfb0c093",
                        uri="/dir/index.html",
                        qop=auth,
                        nc=00000001,    //请求计数
                        cnonce="0a4f113b",   //客户端密码随机数
                        response="6629fae49393a05397450978507c4ef1",
                        opaque="5ccc069c403ebaf9f0171e9517f40e41"
   (跟随一个新行，形式如前所述)。
   ```

   ```
   服务器响应：
   HTTP/1.0 200 OK
   Server: HTTPd/0.9
   Date: Sun, 10 Apr 2005 20:27:03 GMT
   Content-Type: text/html
   Content-Length: 7984
   (随后是一个空行，然后是所请求受限制的HTML页面)
   ```

   response 值由三步计算而成。当多个数值合并的时候，使用冒号作为分割符：

   1. 对用户名、认证域(realm)以及密码的合并值计算 MD5 哈希值，结果称为 HA1

   2. 对HTTP方法以及URI的摘要的合并值计算 MD5 哈希值，例如，"GET" 和 "/dir/index.html"，结果称为 HA2。

   3. 对HA1、服务器密码随机数(nonce)、请求计数(nc)、客户端密码随机数(cnonce)、保护质量(qop)以及 HA2 的合并值计算 MD5 哈希值。结果即为客户端提供的 response 值。因为服务器拥有与客户端同样的信息，因此服务器可以进行同样的计算，以验证客户端提交的 response 值的正确性。

      在上面给出的例子中，结果是如下计算的。 

      HA1 = MD5( "Mufasa:testrealm@host.com:Circle Of Life" ) =939e7578ed9e3c518a452acee763bce9

      HA2 = MD5( "GET:/dir/index.html" )       = 39aff3a2bab6126f332b942af96d3366Response = MD5( "939e7578ed9e3c518a452acee763bce9:\                         dcd98b7102dd2f0e8b11d0f600bfb0c093:\00000001:0a4f113b:auth:\39aff3a2bab6126f332b942af96d3366" )= 6629fae49393a05397450978507c4ef1

此时客户端可以提交一个新的请求，重复使用服务器密码随机数(nonce)（服务器仅在每次“401”响应后发行新的nonce），但是提供新的客户端密码随机数(cnonce)。

在后续的请求中，十六进制请求计数器(nc)必须比前一次使用的时候要大，否则攻击者可以简单的使用同样的认证信息重放老的请求。由服务器来确保在每个发出的密码随机数nonce时，计数器是在增加的，并拒绝掉任何错误的请求。

显然，改变HTTP方法和/或计数器数值都会导致不同的 response值。服务器应当记住最近所生成的服务器密码随机数nonce的值。也可以在发行每一个密码随机数nonce后，记住过一段时间让它们过期。如果客户端使用了一个过期的值，服务器应该响应“401”状态号，并且在认证头中添加stale=TRUE，表明客户端应当使用新提供的服务器密码随机数nonce重发请求，而不必提示用户其它用户名和口令。

服务器不需要保存任何过期的密码随机数，它可以简单的认为所有不认识的数值都是过期的。服务器也可以只允许每一个服务器密码随机数nonce使用一次，当然，这样就会迫使客户端在发送每个请求的时候重复认证过程。需要注意的是，在生成后立刻过期服务器密码随机数nonce是不行的，因为客户端将没有任何机会来使用这个nonce。
