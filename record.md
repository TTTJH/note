#### 准备面试整体规划：
* 整体信息来源：
	* 掘金博客 八股文、众多基础问题、林三胖相关。
	* 剑指offer
	* 牛客网面经面试题
	* 牛客网刷题库
	* 数据结构与算法*排序

* 八股文：
	* 根据掘金这篇八股文博客内容进行自行扩展。
	* HTTTP:
		* http: 从服务器传输超文本至本地浏览器的超文本传输协议。
		* https:更安全的http通道，基于http进行SSL层安全封装。
		* http和https的区别：
			* http是明文传输，https是加密传输，所以https安全性更高。
			* http的默认端口是80,https的默认端口是443
			* http是无状态简单链接，https会产生更多的开销。
			* https是需要付费的证书的。
		* TCP/IP的通信传输流：
			* 作为发送端的客户端在 应用层 发出一个HTTP请求，这是HTTP协议的职责。
			* 接着传输层把从应用层接受到的数据(HTTP请求报文)进行分割，并在各个报文上打上标记序号以及端口号转发给网络层，这是TCP协议的职责。
			* 网络层增加作为通信目的地的MAC地址后转发给链路层，这是IP协议的职责。
		* 报文的组成结构：
			* http请求报文组成内容如下：
				* 请求方法、请求URI、HTTP版本、请求首部字段(请求头)、请求体
				* ——————————————————————————————————————
				*|Method  URI  HTTP版本                 |
				*|Host:xxxx                             | <-- 请求头内容
		        	*|Date:xxxx				| <-- 请求头内容
				*|.......				| <-- 请求头内容
				*|id:xxx,name:xxx                       | <--- 请求体内容，存放参数
				* ——————————————————————————————————————
			* http响应报文组成内容如下：
				* http版本、状态码、状态码的原因短句、响应首部字段(响应头)、响应体
				* ——————————————————————————————————————
				*|http版本  状态码  状态码原因          |
				*|set-cookies:xxxxx                     | <-- 响应头内容
				*|.....					| <-- 响应头内容			
				*|<html>                                | <--  响应体内容
				*|....					| <--- 响应体内容
				*|</html>                               | <--- 响应体内容
				* ——————————————————————————————————————
			* 值得注意的是，每两个部分之间用空格隔开(Method URI这就算两个部分)，最后一个部分后面有一个换行。
		* HTTP响应状态码：
			* 204：请求成功，但没有资源返回。响应报文中不允许有很任何实体的主体，一般在只需要客户端向服务端发送信息，而服务端不需要返回新内容的时候使用。
			* 206：范围请求成功，成功响应范围请求，响应头中的content-range指定范围实体内容。
			* 资源重定向解释：
				* 重定向状态码用来告诉浏览器客户端，它所访问的资源已经被移动，web服务器发送一个重定向状态码和一个可选的location header，告诉客户端新的资源地址在哪。浏览器客户端会自动用location中提供的地址，重新发送新的request,整个过程对于用户来说是透明的。
			* 301：永久重定向
			* 302：临时重定向
			* 304: 客户端缓存资源是最新的，通知客户端使用缓存。
		* HTTPS的通信过程：
			* 客户端发送client_random、TSL版本、加密套件列表。
			* 服务器响应server_random、server_params、确定TSL版本、加密套件列表、服务器使用的证书。
			* 客户端验证来自服务端的证书以及内部签名，验证通过后传递client_params参数给服务器
				* 并且客户端根据server_params和client_params计算出pre_random.
				* 然后根据pre_random、client_random以及server_random计算出secret
			* 服务端接收来自客户端的client_params,根据client_params和server_params计算出pre_random
				* 然后根据pre_random、client_random和server_random计算出secret
			* 之后的传输都使用secret进行加解密。
			* 理解：
				* 在此途中，客户端两次发送请求都有client_random或者client_params,第一次是发送的client_random,第二次是在通过了服务器返回的安全证书验证后发送的client_param.
				* 双方都在交流后获取了足够的参数计算出了pre_random最终计算出一致的密匙secret保护之后的请求与响应。
		    客户端：------------------——————————————————-------------------------------------------------------	
				 | ^                ▲             | 发送利用  |▲ 双方交换Finished后，
				 | | 双方协商	    | 服务端发    | 公匙加密  || 证明服务器成功解密
				 | | SLL版本、      | 送公开密匙  | 过的密码  || 客户端加密报文，
				 v | 加密组件列表   | 证书	  v 字符串    v| SSL连接建立成功，开始HTTP的请求与响应
		    服务端：------------------------------------——————————————————--------------------------------------

		
		* HTTP三次握手：
			* 第一次握手：客户端给服务端发送一个SYN报文，并指明客户端的初始化序列号ISN，此时客户端处于SYN_SEND状态。
			* 第二次握手：服务器收到来自客户端的SYN报文之后，会以自己的SYN报文作为应答，并且也指定自己的舒适化序列号ISN，同时会把ISN+1作为ACK的值，表示自己已经收到了来自客户端的SYN,此时服务器处于SYN_RVCD(received瑞细)状态。
			* 第三次握手：客户端收到来自服务端的SYN报文之后，也会把来自服务端的ISN+1作为ACK的值进行发送，表明客户端已经接受到了服务端的SYN报文，此时客户端处于established(意思大不里)状态,服务器接收到ACK报文后，也进入eastablished状态，双方建立链接。
		* 三次握手的目的：
			* 第一次握手：客户端发包，服务端接受。服务端目前确定客户端发送能力以及服务器接受能力正常。
			* 第二次握手：服务器发包，客户端接受。服务端目前确定客户端发送能力和服务器的收发能力正常。
			* 第三次握手：客户端发包，服务器接受。服务器目前确定客户端收发能力和服务器的收发能力都正常。
			* 防止延迟到达的请求被服务端处理为全新的请求，开启连接状态，造成资源浪费。
				* 防止失效的连接请求突然又传送至服务端，产生错误。
					* 失效的连接请求是指，客户端发出的请求没有收到服务端的确认，于是经过一段时间之后，客户端又重新发送连接请求且建立成功，完成数据传输。
					* 然后网络延迟的第一次请求延迟到达服务端，服务端以为客户端又发起了新请求，于是同意建立连接，并向客户端发送确认，客户端并不会给予理会，服务端却一直开启等待请求连接，造成了资源浪费。 
		* 半连接队列：
			* 服务器接受到来自客户端第一次发送的SYN时，会处于SYN_RCVD状态，此时双方还没完全建立连接，服务器会把此种状态下请求链接放在一个队列里，我们把这种队列成为半连接队列。
		* 四次挥手:
			* 客户端发送一个FIN报文，报文中会指定一个序列号，此时客户端处于FIN_WAIT状态。
			* 服务器收到FIN报文之后，会发送ACK报文，并根据客户端的序列号+1作为ACK的序列号，表明已经收到客户端的报文了，此时服务端处于CLOSE_WAIT状态。
			* 服务器也可以断开连接了，和客户端第一次挥手一样，发送FIN报文，指定一个序列号。此时服务端处于LAST_ACK状态。
			* 客户端收到FIN后，一样发送一个ACK报文作为应答，且把服务端的序列号+1作为自己ACK序列号，此时客户端处于TIME_WAIT状态。需要过一阵子确保服务端收到自己的ACK报文之后才会进入CLOSED状态。
			* 服务端收到ACK报文之后，就处于关闭连接了，处于CLOSED状态。
		* HTTP代理：通常连接是属于客户端与服务端的连接，而代理则是充当中间商的角色，它使得客户端无需知道源服务器的IP是多少，数量有多少，就能够得到自己所需要的资源。
		* HTTP代理的功能：
			* 负载均衡。客户端的请求只会先到达代理服务器，后面到底有多少源服务器，IP是多少，对于客户端来说是未知的。当代理器拿到这个请求的时候，可以通过特定的算法分发给不同的服务器，让各台服务器的负载尽量保持平均。
			* 缓存代理。将内容缓存到代理服务器，使得客户端可以直接从代理服务器获得资源不用请求到服务器那里。
		* HTTP代理相关的头部字段：
			* Via
				* via头部字段用于记录 代理服务器 在HTTP传输过程中自己的身份。
				* 举个例子：目前有两个代理服务器，在客户端发送请求之后会经历这样的一个过程。
					* 客户端 --> proxy_server1 --> proxy_server2 --> 源服务器
				* 当源服务器收到请求之后，请求头中的via字段的值就是:proxy_server1,proxy_server2
				* 而当源服务器响应客户端时，客户端在响应头中拿到的via字段的值就是:proxy_server2,proxy_server1
				* via中的代理顺序就是http报文传达的顺序。
			* X-Forwarded-For:
				* 字面意思就是 为谁而转发，他记录着请求方的IP地址。
				* 这意味着每经过一个不同的代理，这个字段的名字都要变。
				* 从客户端到proxy_server1,这个字段的值从客户端IP变为proxy_server1的IP。
				* 从proxy_server1到proxy_server2，这个字段的值从proxy_server1的IP变为proxy_server2的IP。
			* X-Real-IP:
				* 是一种获取用户真实IP的字段，不管中间经过多少代理，这个字段始终记录最初的客户端IP。

			* X-Forwarded-Host:记录客户端的域名(不包括代理的)
			* X-Forwarded-Proto:记录客户端的协议(不包括代理的)
		* 说说你对Cookies的理解：
			* 由于HTTP是无状态协议，每次的HTTP请求都是独立的、无关的、默认不需要保留状态信息。
			* 为此引入了cookie进行状态保存，cookie以键值对的形式存储，可以在控制台的application中看到。
			* cookie的请求或响应报文中的表现形式如下：
				* 请求头：Cookie:name=xxx;sex=xxx
				* 响应头：Set-Cookie:a=xxx (服务端可以通过响应头中的set-cookies字段来对客户端写入Cookie)
			* cookie相关属性：
				* cookie的有效期可以通过Expires或Max-Age来设置。
					* Expires是过期时间。
					* Max-Age用的是一段时间间隔，单位是秒，从浏览器收到报文开始计算。
				* Domain和path,用来给cookie绑定域名和路径，发送请求之前，发现域名和路径这两个不匹配，就不会带上Cookie.
				* Secure和HttpOnly：Secure说明只能通过HTTPS传输Cookie.HttpOnly说明只能通过HTTP协议传输，不能通过JS访问，这是预防XSS（恶意代码注入）攻击的重要手段。
			* Cookie的缺点：
				* 容量只有4KB
				* 性能问题：如果不指定Domain和Path的话每次请求都会携带，会产生性能问题。
				* 安全问题：如果不置HttpOnly标识的情况下，Cookie可以直接通过JS脚本获取。
			* 例子：创建一个开启了HttpOnly属性以及存在一小时的cookie
				* document.cookie("test=test;HttpOnly;max-age=60*60")
		* HTTP队头阻塞问题：
			* HTTP传输是基于 请求-应答 的模式进行的，报文必须是一发一收。里面的任务被放在一个任务队列中串行执行，一旦队首的请求处理的太慢，就会阻塞后面请求的处理，这就是HTTP队头阻塞问题。
			* HTTP1.1中的解决方法：
				* 并发连接。对于一个域名运行分配多个长连接，相当于增加了任务队列，不至于一个阻塞阻塞所有任务。
				* 域名分片。一个域名可以并发多个长连接，那么再多分出几个域名，可以并发出更多的长连接。
		* keep-alive长连接：
			* 长连接就是任意一方没有明确提出断开连接，则保持TCP连接状态。
			* 长连接的好处在于避免了TCP重复连接与断开所产生的额外开销，减轻服务端压力。
		* HTTP如何处理表单数据的提交?
			* HTTP中，有两种主要的表单提交方式，体现在两种不同的Content-Type取值。
			* application/x-www-form-urlencoded
				* 表单的默认类型。非字母数字的其他字符将进行编码转换。
			* multipart/form-data
				* 请求头中的Content-Type字段会包括boundary,且boundary的值由浏览器制定。
			* 在实际场景中，对于图片等文件的上传，基本采用multipart/from-data而不用application/x-www-form-urlencoded,因为没有必要做URL编码产生额外的耗时。 	
		* 对于定长和不定长的数据，HTTP怎样传输的？
			* 定长包体：通过Content-Leng指定包体长度。
			* 不定长包体：Transfer-Encoding:chunked表示分块传输数据。
		* get和post请求在缓存方面有什么区别？
			* 一般会对get请求进行缓存，很少会对post请求进行缓存，因为get请求一般是用来请求数据的。
		* get请求是否限制了传参长度？
			* HTTP协议未规定GET和POST的长度限制，是浏览器对URI长度进行限制。
		* TCP和UDP的区别？
			* TCP是面向连接的(只有在确认通信对端存在时才发送数据 )、可靠的流协议，可以进行丢包的重发控制，可以对次序乱掉的分包进行顺序控制。
			* UDP是面向无连接不可靠的协议。它是将应用程序发来的数据在收到的那一刻，立刻按照原样发送到网络上的一种机制。不具备顺序控制和重发控制的功能，细节功能需要采用UDP的应用程序去处理。（多用于对高速传输和实时性有要求的通信或广播通信)
		* eTag字段：服务器资源的URI虽然没有变，但是当资源更新后，eTag是会改变的。
		* 什么是负载均衡？
			* 将客户端发送的请求数量按照一定的规则分发到不同的服务器处理的过程，称为负载均衡。
		* 正向代理和反向代理：
			* 正向代理：用户非常明确自己要访问的源服务器地址；而源服务器只清除请求来自哪个代理服务器，而不清除来自哪个具体的客户端；正向代理模式屏蔽隐藏了真实的客户端信息。正向代理是一个位于客户端和源服务器之间的服务器，为了试客户端获取源服务器内容，客户端向代理服务器发送请求并指定目标(源服务器)，然后代理服务器转交请求至源服务器并获取内容然后转发给客户端。
			* 反向代理：多个客户端发送的请求，nginx接收后按照一定的规则分发给了不同的服务器，客户端不知道自己访问了一个服务器，反向代理代理的是服务器，他代服务端接受请求,反向代理隐藏了服务端的信息。
				* 反向代理可用于做负载均衡，或保护内外服务器信息。
	
		* Nginx是什么？
			* Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器。
		* 参数进行编码的原因？
			* 可直接说避免跨平台出现混乱的情况，达到就像不同的语言都用JSON文件一样的效果。
		* 什么是WebSocket？
			* HTML5开始提供的一种客户端与服务器全双工通信的网络技术，属于应用层协议，它基于TCP传输协议，服用HTTP的握手通道。
			* WebScoket支持全双工通信，实时性更好。
			* WS客户端、服务端进行数据交换的时候，协议控制的数据包头部较小，控制开销更少。
		* WebSocket中的心跳是为了解决什么问题？
			* 为了定时发送消息保持持续连接，定时给后端发送消息，让后端知道连接还在持续避免被视作超时自动断线。
			* 为了检测后端的连接状态是否正常。通过与后端的持续接收发确保后端状态正常，否则应该执行重连。
		* 谈谈你对HTTP2的理解：
			* 首先先谈谈HTTP1.1的缺点：
				* 对头阻塞问题：HTTP是通过请求-响应的方式进行数据传输，该活动被放入队列中进行存储，如果队头前面的响应尚未返回，将阻塞后续的请求与响应。
				* 请求响应首部内容过多且重复。
				* 服务端不可推送消息。
				* 明文传输。
			* HTTP2的优势：
				* 多路复用：
					* 同域名下所有的通信都可以在单个连接上完成。减少TCP连接数量和TCP连接启动慢的问题
					* 单个连接可以承受任意数量的双向数据流。
					* 数据流分成多个帧进行发送，多个帧之间可以乱序，因为可以根据帧的首部标识进行重新组装。
				* Header压缩，采用专门算法先存储之前发送的键值对，之后只发送差异数据，渐进式的更新。
				* 帧作为最小的数据传输单位，以二进制传输代替原本的明文传输，原本的报文消息被划分为更小的数据帧。
				* Server Push:服务器可以主动向客户端发送消息。比如当客户端请求HTML的时候服务端主动发送相对于的CSS/JS文件。
			* HTTP2的缺点：
				* TCP是可靠传输，当HTTP2中帧丢失时，整个TCP都要开始等待重传，就会阻塞该TCP连接中的所有请求。相反HTTP1开启了多个TCP连接，出现丢包时只会影响其中一个TCP连接的重传。
				
		* 强缓存和协商缓存是什么？
			* 浏览器缓存分为强缓存和协商缓存，两者有两个比较明显的区别。
			* 如果浏览器命中强缓存，则不需要给服务器发送请求；而协商缓存最终由服务器来决定是否使用缓存，即客户端与服务器之间存在一次通信。
			* 在chrome中强缓存(虽然没有发出真实的http请求)的请求状态码返回是200；而协商缓存命中是走缓存的话，请求的状态码是304.
		* 注意！缓存都是在第二次请求可用的。
		* 怎样判断是否命中强缓存？
			* 根据缓存内容中的Expires相对时间或者Cache-Control中的绝对时间来判断，命中则使用强缓存，否则协商缓存。
		* Cache-Control（强缓存字段）:
			* 首部字段Cache-Control是用来操作缓存机制的字段：常用值如下：
				* max-age: 单位是s,根据返回头中的Date加上max-age时间标识该时间范围内缓存有效。
				* no-cache: 不使用本地缓存，使用协商缓存。
				* no-store: 直接禁止浏览器使用缓存数据，每次请求资源都会向服务器请求完整的资源。
		* If-Modified-Since(协商缓存字段)：
		* Last-Modified(协商缓存字段)：
			* 请求头中携带If-Modified-Since告诉服务端客户端最后一次缓存该资源的日期。
			* 服务端判断如果在该日期之后该资源不存在修改，则返回304，提示客户端使用协议缓存。
			* 如果服务端判断在该日期之后该资源进行过修改，则携带Lasr-Modified字段以及响应主体内容。

		* Etag:(协商缓存字段)：
		* If-None-Match:(协商缓存字段)：
			* 只要资源有改动，Etag就会改变。
			* 服务端根据文件的哈希值计算得出Etag并且返回给浏览器。
			* 当请求头携带if-None-Match字段后，服务器比较两者是否一致来决定文件是否有改动。
		
		* 有了Last-Modified之后，为什么还会出现Etag?
			* 当只是最后改动时间变了，资源内容没变，Etag更加合适。
			* Etag可以精确到秒以下级别。
		* 什么是CDN？
			* CDN是内容分发网络。
			* 根据部署在各地的服务器，通过中心平台的负载均衡，内容分发调度，使用户就近获取所需内容，提高用户访问速度和命中率。
		* 即时通讯：短轮询、长轮询、SSE和websocket之间的区别？
			* 短轮询：浏览器每隔一段时间向浏览器发送http请求，服务端在接收到请求之后，不论是否有数据更新，都直接进行响应。这种方式实现的即时通信，本质上还是浏览器发起请求，服务器响应请求的过程。优点是比较简单，便于理解，缺点是需要不断的建立http连接，服务端的压力很大。
			* 长轮询：客户端向服务端发送请求，当服务器收到来自客户端的请求时，服务端不会直接进行响应，而是先将请求挂起，然后判断服务端数据是否有更新，如果有更新，则进行响应，如果一直没有更新，则到达一定的时间限制才返回。客户端处理好服务端返回的响应之后，再次发出请求，重新建立连接。长轮询与短轮询比起来，减少了很多不必要的http请求与响应，相对应节约资源。
			* SSE:服务器使用流信息向客户端推送消息。严格上说，http协议无法做到服务器主动推送消息，但是有一种变通方法，就是服务端向客户端声明，接下来要发送的是流信息，也就是说发送的不是一个数据包，而是一个数据流，会持续不断的发送过来。因此客户端不会关闭连接，会一直等着服务端发过来新的数据流，视频播放就是这样的例子。
			* WebSocket:是HTML5定义的可实现全双工通信的新协议，该协议运行服务端向客户端推送信息，服务端与客户端通信双方是平等的。
		* HTTP1.0和HTTP1.1的区别：
			* HTTP1.1采用默认长连接，HTTP1.0默认非持久连接。
			* HTTP1.1支持了范围请求，通过请求头中的content-range字段请求资源的某一部分，返回码是206
			* HTTP1.1支持新方法PUT、OPTION
		* CORS:
			* CORS是一个W3C标准，全称为“跨域资源共享”。
			* 他允许浏览器向跨源服务器发出XMLHhttpRequest请求，从而克服了AJAX只能同源使用的限制。
			* 整个CORS通信过程，都是浏览器完成的，浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，前端无感知。
			* 实现CORS的关键是服务端配置。
			* CORS将请求分为两类：简单请求 和 非简单请求
				* 简单请求需要满足的条件：
					* 请求方法是如下之一：
						* GET
						* POST
						* HEAD
					* HTTP的头部字段不超出以下几种：
						* Accept
						* Accept-Language
						* Content-Language
						* Last-Event-ID
						* Content-Type(只限于三个值)
							* application/x-www-form-urlencoded
							* multipart/form-data
							* text/plain
			* 对于简单请求：
				* 浏览器直接发出CORS请求，在请求头部信息中，添加Origin字段。
				* Origin字段用来说明，本次请求来自哪一个源(协议+域名+端口)	
				* 服务端根据Origin内容，来决定是否同意本次请求。
				* 如果服务端判定客户端传递过来的Origin不在其许可范围内，服务器会返回一个正常的HTTP响应。之后浏览器发现，这个响应并不包含Access-Control-Allow-Origin字段，就知道应该抛出错误，被XMLHttpRequest的onerror回调函数捕获。
				* 如果服务端判定客户端传递过来的Origin在其许可范围内，服务端返回的响应，会多出几个头部信息字段：
					* Access-Control-Allow-Origin:
						* 该字段是必须的。他要么是请求时Origin的字段，要么是*,标识接受任意。
					* Access-Control-Allow-Credentials:
						* 该值是个布尔值，标识是否允许发送Cookie
					* Access-Control-Expose-Headers:
						* 指定获取其他头部字段。
			* 对于非简单请求：
				* 非简单请求就是有特殊要求的请求。
				* 比如请求方法是PUT或者DELETE，Content-Type字段的类型是application/json
				* 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为“预检测请求”。
				* 预检测请求先询问服务器，当前网页所在的域名是否在服务器许可名单之内，以及可以使用哪些HTTP头部字段，只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。			
				* 预检测请求包括两个特殊的字段：
					* Access-Control-Request-Method:
						* 该字段是必须的，列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT.
					* Access-Control-Request-Headers:
						* 该字段是个逗号分隔符字符串，指定浏览器CORS请求会额外发送的头部信息字段。
				* 预检测请求的回应：
					* 如果服务端检查了预检测请求头中的Origin、Access-Control-Request-Method、Access-Control-Request-Headers之后允许该跨域请求就正常做出回应，如果不允许则触发错误被XMLHttpRequest捕获。
				* 预检测请求服务器的回应如下：
					* Access-Control-Allow-Methods:
						* 该字符串以逗号作为分隔符，表明服务器支持的所有跨域请求的方法。
					* Access-Control-Allow-Headers:
						* 该字符串以逗号作为分隔符，表明服务器所支持的所有头部字段。
					* Access-Control-Allow-Credentials:
						* 布尔值，表明服务器是否同意浏览器携带cookie
					* Access-Control-Max-Age:
						* 可选字段，表明本次预检测请求的有效期，在有效期范围内，下次CORS请求前可不必进行预检测请求。
				* 一旦服务器通过了预检测请求，以后每次浏览器正常的CORS请求，都会跟简单请求一样，请求头携带一个Origin字段，响应头中携带一个Access-Control-Allow-Origin.
		* HTTP请求头和响应头有哪些比较重要的字段？
			* 通用首部：
				* Cache-Control:控制缓存的行为。
				* Date:创建报文的日期时间。
				* Connection:表明是否需要持久连接(keep-alive)
				* Via：代理服务器相关信息
				* Transer-Encoding:文件传输编码
			* 请求头部：
				* Accept:指定客户端能够接受的内容类型，不同类型字符串先后顺序为优先级。		
				* Authorization：web的认证信息。
				* Host:请求资源所在的服务器的主机名端口号，在多个虚拟主机运行在同一个IP的时候很有用。
				* User-Agent:客户端程序信息，例如浏览器种类、操作系统信息
			* 响应头部：
				* Location:令客户端重定向至指定URL
				* ETag:能够标识资源唯一的字符串
				* Server:HTTP服务器信息，例如Apache Nginx
		* 一个特殊的请求方法：
			* HEAD:获取报文首部，与GET相比，不返回报文主体部分。
		* GET和POST的区别：
			* GET请求多用于从服务器获取资源的场景，POST请求多用于会对服务器资源产生影响的场景。
			* GET请求一般会调用缓存，POST一般不会。
			* GET请求的请求主体为空，POST请求的请求主体存放参数。
			* GET请求参数携带在URL后缀，相对安全性较低。
			* POST传参支持的数据类型更多。
		* DNS协议介绍：
			* DNS就是域名系统，提供主机名到IP地址的转换服务。
			* 客戶端向DNS服務器(DNS有自己的IP地址)发送域名查询请求，DNS服务器告知客户端所请求的WEB服务器的IP地址。
			* DNS占用53端口
			* DNS完整的查询过程：
				* 首先会在浏览器的缓存中查找对应的IP地址，如果找到则直接返回，否则进行下一步。
				* 将请求发送给 本地DNS服务器，在本地域名服务器缓存中查询，如果查找到就直接返回，否则进行下一步。
				* 本地DNS服务器向 根域名服务器发送请求，根域名服务器会返回一个所查询域名的 顶级域名 服务器地址。
				* 本地DNS服务器向 顶级域名服务器 发送请求，接受请求的服务器查询自己的缓存，如果找到就直接返回，否则就返回所查询域权威域名服务器地址。
				* 本地DNS服务器向 权威域名服务器 发送请求，域名服务器返回对应的结果。
				* 本地DNS服务器将缓存结果保存在缓存中，便于下次使用。
				* 本地DNS服务器将返回结果返回给浏览器。
		* 描述从输入网址到页面展示的过程。
			* DNS域名解析。
			* HTTP连接
				* 三次握手
				* （四次挥手）
			* HTTP请求-描述一下HTTP报文结构
			* HTTP响应-描述一下HTTP报文结构
			* 浏览器解析渲染页面
				* 构建DOM树 
				* 构建CSS规则树
				* 构建render树：WEB浏览器将DOM和CSS结合，构建出渲染树
				* 布局(Layout):计算出每个节点在屏幕的位置
				* 绘制(Painting):浏览器开始渲染并绘制页面。
		* URI、URL、URN：
			* URI代表资源的名称
			* URL代表资源的路径
			* URN代表资源的唯一标识
			* URL和URN都是URI的子集。
		* 跨域：
			* 跨域是浏览器基于同源策略的一种手段。
			* POSTMAN可以获取接口数据，但是浏览器却不行，因为这就是浏览器做的操作。
	* 数据结构复习：
		* 什么是集合？
			* 集合表示一组互不相同的元素(也就是不重复的元素)
			* 集合以[值，值]的形式存储元素。	
		* 什么是字典？
			* 字典也用于表示一组不重复的值。
			* 字典以[键，值]对的形式来存储数据。
			* 字典也成为映射
		* 说出复制链表的思路：
			* 首先cur指向head
			* 接着新构造一个节点实例对象newHeader,并且newCur也指向这个新的节点实例对象
			* 开始while循环，种植条件是cur为null
			* 循环过程中不断的生成新的实例化节点对象并赋值给newHeader的next,生成实例化对象的是传递的element参数是cur.element
			* 循环过程中更新cur = cur.next,newCur = newCur.next
			* 最终返回newHeader.next这就是新链表的头部
		* 说出复制复杂链表的思路：
			* Map映射的方法：
				* 循环一遍现有链表，在循环当中以cur作为Map的key,new Node(cur.element)作为Map的value.
				* 重置cur=head,再次循环现有链表，通过cur这个key用Map.prototype.get来得到其对应new Node，设置该节点对应的next属性和random属性
				* 其next属性和random属性指向的节点，也是通过cur.next和cur.random作为key的方式用Map.prototype.get(cur.next)和Map.prototype.get(cur.random)来得到。
			* 链表拼接+拆分的方法：
				* 循环一遍现有链表，每次循环中new Node(cur.element)生成cur值相同节点，新节点next指向cur.next，cur的next修改指向新节点，cu更新为cur.next.next;一遍循环下来，原有链表基础上新添加了一个链表。
				* 重置cur=head,循环一遍链表，cur.next的random修改指向为cur.random.next,也就是新节点的random指向其cur的random节点的下一个节点。一遍循环下来，新节点的random也被分配完毕。
				* 重置cur=head,循环一遍链表，暂存cur.next为tmp，修改cur.next为cur.next.next,也就是第一个链表跳过第二个链表内容进行连接，cur = tmp,下一次循环从暂存节点开始。一遍循环下来，一条链表就被拆分为两条。
			
		* Set集合：
			* Set集合有哪些属性、方法？
				* size属性
				* add方法
				* delete方法
				* has方法
				* clear方法
			* Set集合的四个遍历方法：
				* Set.prototype.keys()返回key的遍历器
				* Set.prototype.values()返回values的遍历器
				* Set.prototype.entries()返回键值对的遍历器
				* Set.prototype.forEach()使用回调函数遍历每个成员
		* Map：
			* Map有哪些属性和方法？
				* size属性
				* set方法
				* get方法
				* has方法
				* delete方法
				* clear方法				
			* Map的四个遍历方法:
				* Set.prototype.keys()返回key的遍历器
				* Set.prototype.values()返回values的遍历器
				* Set.prototype.entries()返回键值对的遍历器
				* Set.prototype.forEach()使用回调函数遍历每个成员
				