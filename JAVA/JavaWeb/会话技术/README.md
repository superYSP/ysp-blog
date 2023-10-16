## Cookie

#### Cookie的工作原理

由于HTTP是一种无状态协议，服务器没有办法单单从网络连接上面知道访问者的身份，为了解决这个问题，就诞生了Cookie。Cookie实际上是一小段的文本信息。Cookies是由服务器产生的。接下来我们描述一下Cookie产生的过程。浏览器第一次访问服务端时，服务器此时肯定不知道他的身份，所以创建一个独特的身份标识数据，格式为key=value，放入到Set-Cookie字段里，随着响应报文发给浏览器。浏览器看到有Set-Cookie字段以后就知道这是服务器给的身份标识，于是就保存起来，下次请求时会自动将此key=value值放入到Cookie字段中发给服务端。服务端收到请求报文后，发现Cookie字段中有值，就能根据此值识别用户的身份然后提供个性化的服务。相当于服务器给每个客户端都贴上了一个小纸条。上面记录了服务器给我们返回的一些信息。然后服务器再看到这张小纸条就知道我们是谁了。这就是Cookie的工作原理。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409210956478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)

比如说，项目启动以后我们输入路径http://localhost:8005/testCookies，然后查看发的请求。可以看到下面那张图使我们首次访问服务器时发送的请求，可以看到服务器返回的响应中有Set-Cookie字段。而里面的key=value值正是我们服务器中设置的值。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409211510872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
接下来我们再次刷新这个页面可以看到在请求体中已经设置了Cookie字段，并且将我们的值也带过去了。这样服务器就能够根据Cookie中的值记住我们的信息了。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409211655929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
接下来我们换一个请求呢？是不是Cookie也会带过去呢？接下来我们输入路径http://localhost:8005请求。我们可以看到Cookie字段还是被带过去了。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409211737302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)

##### 那么浏览器的Cookie是存放在哪呢？

Cookie是存储在客户端方，如果是使用的是Chrome浏览器的话，那么可以按照下面步骤查看cookie：
1.在计算机打开Chrome
2.在右上角，一次点击更多图标->设置
3.在隐私设置和安全性下方，点击网站设置
4.点击权限里的Cookie和网站数据，进入后然后点击查看cookie和网站数据，就能看到cookie了，如下图：
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409212519813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409212834457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409213122631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
然后可以根据域名进行搜索所管理的Cookie数据。所以是浏览器替你管理了Cookie的数据，如果此时你换成了Firefox等其他的浏览器，因为Cookie刚才是存储在Chrome里面的，所以服务器又蒙圈了，不知道你是谁，就会给Firefox再次贴上小纸条。

#### cookie的参数设置

Cookie就是服务器委托浏览器存储在客户端里的一些数据，而这些数据通常都会记录用户的关键识别信息。所以Cookie需要用一些其他的手段用来保护，防止外泄或者窃取，这些手段就是Cookie的属性。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409213543522.png)
cookie的内容主要包括name(名字)、value(值)、 maxAge(失
效时间)、path(路径),domain(域)和secure。
name : cookie的名字，一-旦创建，名称不可更改。
value : cookie的值，如果值为Unicode字符，需要为字符编
码。如果为二进制数据，则需要使用BASE64编码.
maxAge : cookie失效时间，单位秒。如果为正数，则该
cookie在maxAge后失效。如果为负数，该cookie为临时cookie ,关闭浏览器即失效，浏览器也不会以任何形式保存该cookie。如果为0，表示删除该cookie。默认为-1
path :该cookie的使用路径。如果设置为"/sessionWeb/" ，则
只有ContextPath为"/sessionWeb/“的程序可以访问该
cookie.如果设置为”/”，则本域名下ContextPath都可以访问
该cookie.

#### 使用cookie的缺点

如果浏览器使用的是cookie，那么所有的数据都保存在浏览器端，cookie可以被用户禁止。cookie不安全(对于敏感数据，需要加密)，cookie只能保存少量的数据(大约是4k)，cookie的数量也有限制(大约是几百个)，不同浏览器设置不一样，反正都不多。cookie只能保存字符串，对服务器压力小。

## Session机制







Cookie是存储在客户端方，Session是存储在服务端方，客户端只存储SessionId。
在上面我们了解了什么是Cookie，既然浏览器已经通过Cookie实现了有状态这一需求，那么为什么又来了一个Session呢？这里我们想象一下，如果将账户的一些信息都存入Cookie中的话，一旦信息被拦截，那么我们所有的账户信息都会丢失掉。所以就出现了Session，在一次会话中将重要信息保存在Session中，浏览器只记录SessionId一个SessionId对应一次会话请求。
Session机制是一种服务端的机制 ，服务器使用一种类似散列表的结构来保存信息。当程序需要为某个客户端的请求创建一个session的时候 ，服务器首先检查这个客户端里的请求里是否已包含了一个session标识-sessionID ,如果已经包含一个sessionID ，则说明以前E经为此客户端创建过session，服务器就按照sessionID把这个session检索出来使用。
如果客户端请求不包含sessionlD，则为此客户端创建一个session并且声称一个与此session相关联的sessionID ，sessionID的值应该是一个既不会重复 ，又不容易被找到规律以仿造的字符串(服务器会自动创建)，这个sessionID将被在本次响应中返回给客户端保存。

这里我们写一个新的方法来测试Session是如何产生的，我们在请求参数中加上HttpSession session，然后再浏览器中输入http://localhost:8005/testSession进行访问可以看到在服务器的返回头中在Cookie中生成了一个SessionId。然后浏览器记住此SessionId下次访问时可以带着此Id，然后就能根据此Id找到存储在服务端的信息了。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409220349925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
此时我们访问路径http://localhost:8005/testGetSession，发现得到了我们上面存储在Session中的信息。那么Session什么时候过期呢？

客户端：和Cookie过期一致，如果没设置，默认是关了浏览器就没了，即再打开浏览器的时候初次请求头中是没有SessionId了。
服务端：服务端的过期是真的过期，即服务器端的Session存储的数据结构多久不可用了，默认是30分钟。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409220604462.png)
Session的管理是在容器中被管理的，什么是容器呢？Tomcat、Jetty等都是容器。接下来我们拿最常用的Tomcat为例来看下Tomcat是如何管理Session的。在ManageBase的createSession是用来创建Session的。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409220820720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
到此我们明白了Session是如何创建出来的，创建出来后Session会被保存到一个ConcurrentHashMap中。可以看StandardSession类。
protected Map sessions = new ConcurrentHashMap<>();
到这里大家应该对Session有简单的了解了。

> Session是存储在Tomcat的容器中，所以如果后端机器是多台的话，因此多个机器间是无法共享Session的，此时可以使用Spring提供的分布式Session的解决方案，是将Session放在了Redis中。

#### 使用session的缺点

一般是寄生在cookie下的，当cookie被禁止，session也被禁止，当然可以通过url重写来摆脱cookie。当用户访问量很大时，对服务器压力大。我们现在知道session是将用户信息储存在服务器上面,如果访问服务器的用户越来越多，那么服务器上面的session也越来越多，session会对服务器造成压力，影响服务器的负载如果。session内容过于复杂，当大量客户访问服务器时还可能会导致内存溢出。用户信息丢失，或者说用户访问的不是这台服务器的情况下就会出现数据库丢失。

## cookie和session的区别







具体来说cookie机制采用的是在客户端保持状态的方案,而session机制采用的是在服务器端保持状态的方案。同时我们也看到，由于采用服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助于cookie机制来达到保存标识的目的。cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session。session会在一定时间内保存在服务器上。 当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie。单个cookie保存的数据不能超过4k，很多浏览器都限制一个站点最多保存20个cookie.

**Token**

Session是将要验证的信息存储在服务端，并以SessionId和数据进行对应，SessionId由客户端存储，在请求时将SessionId也带过去，因此实现了状态的对应。而Token是在服务端将用户信息经过Base64Url编码过后传给在客户端，每次用户请求的时候都会带上这一段信息，因此服务端拿到此信息进行解密后就知道此用户是谁了，这个方法叫做JWT(Json Web Token)。

Token相比较于Session的优点在于，当后端系统有多台时，由于是客户端访问时直接带着数据，因此无需做共享数据的操作。

### Token的优点

简洁：可以通过URL,POST参数或者是在HTTP头参数发送，因为数据量小，传输速度也很快。
自包含：由于串包含了用户所需要的信息，避免了多次查询数据库，因为Token是以Json的形式保存在客户端的，所以JWT是跨语言的，不需要在服务端保存会话信息，特别适用于分布式微服务。

##### JWT的结构

实际的JWT大概长下面的这样，它是一个很长的字符串，中间用.分割成三部分：
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409221426104.png)
**JWT是有三部分组成的**
**Header**
是一个Json对象，描述JWT的元数据，通常是下面这样子的：
{
“alg”: “HS256”,
“typ”: “JWT”
}
上面代码中，alg属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；typ属性表示这个令牌（token）的类型（type），JWT 令牌统一写为JWT。 最后，将上面的 JSON 对象使用 Base64URL 算法转成字符串。

> JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 api.example.com/?token=xxx）。Base64 有三个字符+、/和=，在 URL 里面有特殊含义，所以要被替换掉：=被省略、+替换成-，/替换成_ 。这就是 Base64URL 算法。

**Payload**
Payload部分也是一个Json对象，用来存放实际需要传输的数据，JWT官方规定了下面几个官方的字段供选用。

iss (issuer)：签发人
exp (expiration time)：过期时间
sub (subject)：主题
aud (audience)：受众
nbf (Not Before)：生效时间
iat (Issued At)：签发时间
jti (JWT ID)：编号







当然除了官方提供的这几个字段我们也能够自己定义私有字段，下面就是一个例子：
{
“name”: “xiaoMing”,
“age”: 14
}
默认情况下JWT是不加密的，任何人只要在网上进行Base64解码就可以读到信息，所以一般不要将秘密信息放在这个部分。这个Json对象也要用Base64URL 算法转成字符串。

**Signature**
Signature部分是对前面的两部分的数据进行签名，防止数据篡改。

首先需要定义一个秘钥，这个秘钥只有服务器才知道，不能泄露给用户，然后使用Header中指定的签名算法(默认情况是HMAC SHA256)，算出签名以后将Header、Payload、Signature三部分拼成一个字符串，每个部分用.分割开来，就可以返给用户了。

> HS256可以使用单个密钥为给定的数据样本创建签名。当消息与签名一起传输时，接收方可以使用相同的密钥来验证签名是否与消息匹配。
> ![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409222202931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)

**Java中如何使用Token**
上面我们介绍了关于JWT的一些概念，接下来如何使用呢？首先在项目中引入Jar包。
compile(‘io.jsonwebtoken:jjwt:0.9.0’)
然后编码如下:
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409222316471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
发现输出的Token如下:
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzdWJqZWN0IiwiaXNzIjoiaXNzdWVyIiwibmFtZSI6InhpYW9NaW5nIiwiYWdlIjoxNH0.3KOWQ-oYvBSzslW5vgB1D-JpCwS-HkWGyWdXCP5l3Ko

此时在网上随便找个Base64解码的网站就能将信息解码出来。
![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200409222415521.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NjM1MDEy,size_16,color_FFFFFF,t_70)
**总结**
相信大家看到这应该对Cookie、Session、Token有一定的了解了，接下来再回顾一下重要的知识点







Cookie是存储在客户端的
Session是存储在服务端的，可以理解为一个状态列表。拥有一个唯一会话标识SessionId。可以根据SessionId在服务端查询到存储的信息。
Session会引发一个问题，即后端多台机器时Session共享的问题，解决方案可以使用Spring提供的框架。
Token类似一个令牌，无状态的，服务端所需的信息被Base64编码后放到Token中，服务器可以直接解码出其中的数据。