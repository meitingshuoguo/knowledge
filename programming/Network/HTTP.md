# 一次完整的HTTP请求所经历的7个步骤
1. 建立TCP连接
2. Web浏览器向Web服务器发送请求命令
3. Web浏览器发送请求头信息
4. Web服务器应答
5. Web服务器发送应答头信息
6. Web服务器向浏览器发送数据
7. Web服务器关闭TCP连接


# 状态码
- **2XX成功（这系列表明请求被正常处理了）**
	- 200 OK，表示从客户端发来的请求在服务器端被正确处理
	- 204 No content，表示请求成功，但响应报文不含实体的主体部分
	- 206 Partial Content，进行范围请求成功
- **3XX重定向（表明浏览器要执行特殊处理）**
	- 301 moved permanently，永久性重定向，表示资源已被分配了新的 URL
	- 302 found，临时性重定向，表示资源临时被分配了新的 URL
	- 303 see other，表示资源存在着另一个 URL，应使用 GET 方法获取资源（对于301/302/303响应，几乎所有浏览器都会删除报文主体并自动用GET重新请求）
	- 304 not modified，表示服务器允许访问资源，但请求未满足条件的情况（与重定向无关）
	- 307 temporary redirect，临时重定向，和302含义类似，但是期望客户端保持请求方法不变向新的地址发出请求
- **4XX客户端错误**
	- 400 bad request，请求报文存在语法错误
	- 401 unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息
	- 403 forbidden，表示对请求资源的访问被服务器拒绝，可在实体主体部分返回原因描述
	- 404 not found，表示在服务器上没有找到请求的资源
- **5XX服务器错误**
	- 500 internal sever error，表示服务器端在执行请求时发生了错误
	- 501 Not Implemented，表示服务器不支持当前请求所需要的某个功能
	- 503 service unavailable，表明服务器暂时处于超负载或正在停机维护，无法处理请求


# 通用首部字段
- Cache-Control 控制缓存的行为
- Connection 开启或关闭持久连接
- Date 创建报文时间
- Via 代理服务器相关信息，每经过一个代理服务器就会添加相关信息，用逗号分割

# 请求首部
- Accept 能正确接收的媒体类型：`application/json` `text/plain`
- Cookie 发送给服务器的Cookie信息
- Host 服务器的域名，用于区分单台服务器多个域名的虚拟主机，是HTTP/1.1唯一必须包含的字段。
- User-Agent
- If-Match 两端资源标记比较，只有判断条件为真服务端才会接受请求：`If-Mach: "123456`，和服务端文件标记比较
- If-Modified-Since 本地资源未修改返回 304（比较时间）
- If-None-Match 本地资源未修改返回 304（比较标记）


# 响应首部
- Age 资源在代理缓存中存在的时间
- ETag 资源标识，资源发生变化时标识也会发生改变
- Server 服务器名字：`Apache Nginx`
- Set-Cookie 需要存在客户端的信息，一般用于识别用户身份
- Content-Type 内容的媒体类型（如'application/json;charset=UTF-8'）
- Expires 内容的过期时间
- Last_modified 内容的最后修改时间


# 常见方法
- GET 从服务器获取资源或数据。
- PUT 将请求的主体部分存储在服务区上。
- POST 用于向服务器发送数据。
- DELETE 让服务器删除请求URL所指定的资源。
- OPOPTIONS 决定可以在服务器上执行哪些方法。
- HEAD 方法
	- 在不获取资源的情况下了解资源的情况(比如，判断其类型);
	- 通过查看响应中的状态码，看看某个对象是否存在;
	- 通过查看首部，测试资源是否被修改了。



# get请求和post请求区别
- 从协议的角度讲没啥区别。对这两者并没有什么硬性的区分。
- 从浏览器的角度讲会有一些区别  比如get请求的长度受限（浏览器限制的）
- 从接口规范/风格角度讲比如REST约定了GET、POST、PUT和DELETE分别对应获取、创建、替换和删除“资源”。就有一些约定上的区别。
- 从安全角度讲也没用区别，不存在哪个更安全。都不安全，如果是http请求的话。



# content-type
- application/x-www-form-urlencoded 表单提交数据（key1=val1&key2=val2）
- multipart/form-data  文件上传。
- application/json 常用


# WebSocket和Http的异同点
同：
- 建立在TCP之上，通过TCP协议来传输数据。
- 都是可靠性传输协议。
- 都是应用层协议。

异：
- WebSocket是HTML5中的协议，支持持久连接，HTTP1.0不支持持久连接。
- WebSocket是双向通信协议。HTTP是单向的。
-

# URI 和 URL 有什么区别
- URI = Uniform Resource Identifier 统一资源**标志符**
- URL = Uniform Resource Locator 统一资源**定位符**  
- URN = Uniform Resource Name 统一资源**名称**

**URI是抽象的定义，不管用什么方法表示，只要能定位一个资源，就叫URI。

**两种方法定位：1. URL：用地址定位；2. URN：用名称定位。**

举个例子：去村子找个具体的人（URI），如果用地址：某村多少号房子第几间房的主人 就是URL， 如果用身份证号+名字 去找就是URN了。

原来uri包括url和urn，后来urn没流行起来，导致几乎目前所有的uri都是url


# 持久连接，非持久连接
持久连接：使用同一个TCP连接发送和接受多个http请求/应答；   
非持久连接：一个TCP连接只能发送和接受一个 http请求/应答；

HTTP 1.0 都是非持久连接
HTTP 1.1  默认都是持久连接，除非特殊声明不支持。
				`Connection: Keep-Alive/close` 是用于 **HTTP持久连接** 的字段。
				Keep-Alive: timeout=5, max=100。5秒后断掉。最多允许100条请求。
HTTP/2  多条请求复用一个连接的机制。

例子：一次请求了六个接口  
		如果开启HTTP持久连接，则此 6 个请求走同一条 TCP 连接，走同一条 HTTP 流。5 秒之内的所有请求都复用此 TCP 连接。当 5秒钟之内没有其他请求后，此连接断开。（一条长连接）