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
				* 解析HTML文件，创建DOM树。
					* 浏览器解析HTML代码，然后创建一个DOM树。每一个HTML标签都有其一个对应的节点，每一个文本也会有其对应的文本节点。DOM树的根节点就是documentElement，对应的标签是HTML标签。
				* 解析CSS，计算出最终的样式数据。解析CSS的时候会按照标签中的style > 内联样式 > 外联样式来进行优先级。
				* 将CSS和DOM合并，构建渲染树。
					* 渲染树和DOM树的区别是前者不仅仅是节点与html对应，并且每个节点中都存储对应的css属性。
				* 布局和绘制：
					* 一旦渲染树创建完成，浏览器就可以根据渲染树将页面绘制到屏幕上。
					* 注意以上步骤不是一次性完成的，如果DOM和CSS被修改，以上过程会重复执行。
					* Repaint（重绘）
						* 重绘是指不影响元素在网页中位置的元素样式被改变时，例如background-color、border-color、visibility,浏览器会根据元素的新属性重新绘制一次使元素呈现新的外观。
					* Reflow（重排）
						* 改变元素在网页中的某个位置属性，就会引发重排。
						* 重绘不一定会重排，重排一定会重绘。
				* 基于重绘和重排，有什么浏览器优化渲染建议？
					* 将多次需要改变位置的元素positon设置为absoluted或者fixed使其脱离文档流，改变其位置时不会引发重排。
					* 不去挨个改变css属性，可以直接给待修改元素加个类名。
		* URI、URL、URN：
			* URI代表资源的名称
			* URL代表资源的路径
			* URN代表资源的唯一标识
			* URL和URN都是URI的子集。
		* 跨域：
			* 跨域是浏览器基于同源策略的一种手段。
			* POSTMAN可以获取接口数据，但是浏览器却不行，因为这就是浏览器做的操作。
		* 进程与线程：
			* 进程是系统进行资源分配和调度的一个独立单位。
			* 线程是CPU调度和分派的基本单位，它是比进程更小的能够独立运行的基本单位。
			* 一个进程至少由一个线程组成。
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
			
		* 说说选择排序的思路：
			* 以arr.length-1为终点，1为起点该范围内所有item和arr[0]进行大小比较，较小者每次被替换指arr[0]
			* 第一轮比较完成之后arr[0]现在就是数组最小值。
			* 接着以length-1为终点，2为起点该范围内所有item和arr[1]进行大小比较，最小者每次被替换至arr[1]的位置。		
		* 说说冒泡排序的思路：
			* 从0项开始，与其右边元素(i+1)对比，如果右边较小，两者调换位置。
			* 每轮循环结束，最大值已经在数组末尾处。
		* 说说插入排序的思路：
			* 从第二项开始(索引为1项目),一直与其左边部分项进行比较，如果比自身大就交换，与左半部分比较的停止条件是当前左边项已经不大于当前项，或者左边已经没有项了。
		* 说一说归并排序的思路：
		* 说一说快速排序的思路：
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
				
* 前端八股文：
		* 说一下BFC是什么？
			* BFC就是块级格式化上下文。
			* BFC内部的box会在垂直方向上挨个放置。
			* BFC内部垂直方向上的box的margin值会重叠。
			* BFC区域不会和float box重叠。
		* 如何触发BFC？
			* 根元素html
			* float的值不为none
			* overflow的值不为visible
			* display设置为flex
			* position设置为absolute / fixed
		* 如何让一个元素水平居中？
			* text-align:center;
			* margin:0 auto;
			* 绝对定位+transform	
			* flex布局：justify-content:center;
			* display:inline-block;text-align:center; 
			* line-height;
		* 隐藏某个元素的方法：
			* opacity:0;不会改变页面布局，但是该元素仍然可触发事件。
			* visibility:不改变页面布局，不会触发事件。
			* display:改变页面布局，不会触发事件。
	* 说一下js的基础数据类型：
		* Number、String、boolean、null、undefined、Symbol
	* 说一下JS的引用数据类型：
		* Objcet,Function,Array,Date,Set,Map
	* 说一下typeof的缺点：
		* 不能区分null、Array、Oject，都返回Objcet
	* 说一下instanceof的缺点：
		* 不能对基本数据类型进行判断。
	* 最准确的数据类型判断方法是什么：
		* Object.prototype.toString.call()
	* 说一下var、let、const的区别：
		* var的最小活动范围是函数作用域；而let和const的最小活动范围是块作用域。
			* 这里可以指出一个经典的例子：一个for循环里面搞个setTimeout延时器输出i的值，用var和let定义i的区别。
		* var存在变量提升，let和const不存在变量提升。
		* var允许相同作用域下重复声明，let和const不允许。
		* 在全局作用域中，用var声明的变量会成为window全局对象的属性，let和const可不会。
	* 说一下你对闭包的理解：
		* 闭包是指有权访问另一个函数作用域中变量的函数。
		* JS在函数执行完之后，内存也会随之被回收，但由于内部函数还在保持着对外部函数变量的引用，即使上级函数执行完，作用域也不会随之销毁。
		* 闭包典型应用场景：
			* 防抖和节流：
	* 说一下你对this的理解：
		* this在普通函数中指向window
		* this在对象方法中指向该对象
		* new关键字生成新对象并且构造函数中的this指向该对象。
		* this在箭头函数中关联其外层对象
		* call、apply、bind后this指向修改指向为第一个参数。
	* 说一下你对原型和原型链的理解：
		* 首先明确两个：构造函数 和 实例化对象
		* 实例化对象的__proto__属性指向一个对象。
		* 构造函数的prototype属性指向一个对象。
		* 构造函数的prototype属性与其实例化对象的__proto__属性指向的就是同一个对象。
		* 当实例化函数自身不准备某个待查找属性时，会去其__proto__指向的对象身上寻找。
	* 说一下你对new关键字的理解：
		* 首先创建一个空对象。
		* 让该对象的__proto__指向其构造函数的prototype
		* 让其构造函数的this指向该对象为该对象进行内容填充。
	* 说一下eventLoop事件循环机制：
		* 同步任务进入主线程，异步任务进入任务队列，主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。
		* 上述过程的不断重复就是事件循环。
		* 其中异步代码还分为宏任务和微任务。
	* 说一下浏览器中事件循环：
		* 浏览器中是这样执行微任务和宏任务的。
		* 执行清空完毕微任务队列后。
		* 去取出一个宏任务执行完毕，查看微任务，有的话清空微任务队列。
	* 说一说await之后的代码有什么特别的地方？
		* await关键词右边代码变成同步代码。
		* await关键词下方代码变成微任务代码。
	* 说一说防抖的思路：
		* 定义一个函数，函数内声明个timer变量存储时间器。
		* 该函数return为另一个函数，return函数中clearTimeout掉timer,赋值新setTimeout.
		* 就是多个闭包函数(return函数)维护同一个timer变量的故事。
	* 说一说节流的思路：
		* 定义一个函数，该函数维护一个lastTime变量。
		* 该函数return另一个函数，return函数中通过当前时间nowTime和lastTime的对比是否超过delay来决定是否执行对应fn
	* 说一说MVC、MVP、MVVM：
		* 我对MVC理解就好比，js请求回调函数中获取数据，然后dom操作修改页面展示效果。
		* 对mvp理解就是像那种ejs，jsp模板，只要在js中修改对应数据，页面会跟着改变。
		* MVVM就是双向绑定，页面上修改他js中保存的数据会变，反之亦然。
	* 说一说如何实现浏览器内多个标签页之间的通信？
		* localStorage.注意sessionStorage是会话级存储空间，每个标签页都是单独的。
		* websocket
		* setInterval+cookie
		* broadcast channel:广播频道
	* 说一说你对这几个页面生命周期事件的理解：
		* DOMContentLoaded：
			* 浏览器已经完成HTML加载，构建好了DOM树。
		* load:
			* 浏览器完成了所有资源加载，包括图片、样式等。
		* befoeunload：
			* 用户正在离开，我们可以在这个阶段检查用户是否保存了更改，并询问他是否真的要离开。
		* unload:
			* 用户真的要离开了。
	* 说一说什么是DOM:
		* DOM就是由HTML标签对应生成的文档对象模型，他把文档当作一个对象，这个对象定义了处理网页内容的方法。
	* 说一说什么是BOM:
		* BOM是指浏览器对象模型，他把浏览器当作一个对象，这个对象定义了与浏览器交互的方法。
		* window全局对象
			* window下面属性：
				* location
				* navigator
				* screen
				* document
	* 说一说你对设备独立像素的理解：
		* 一个设备独立像素可能包含一个或者多个物理像素点，包含的越多则越清晰。
		* 假设有台A旧手机分辨率为320*480，其屏幕上显示着一张图片
		* 然后有台B新手机分辨率为640*960，其屏幕上也显示着旧手机显示的那张图片
		* 那请问在新手机上这个图片的显示效果会缩放一倍嘛？（这样猜测原因是新手机分辨率是旧手机分辨率的一倍）
		* 实际上是不会的，不管新·手机分辨率多高，他们所展示的界面比例都是类似的。
		* 因为新技术（iphone4视网膜屏幕）的诞生，新手机将2*2的像素当作旧手机的1像素使用，使得显示效果更好，显示元素大小不变。
		* 假设某元素宽度为300个像素，那么A旧手机就会用300个物理像素去渲染它，而B新手机会用600个物理像素去渲染他。
		* 所以我们必须用 一种单位 同事告诉 不同分辨率的手机，他们在界面上显示元素的大小是多少，这个单位就是 设备独立像素(DIP/DP)
		* 就比如上面那个300像素宽的元素，在A手机中被300个物理像素渲染，在B手机中被600个元素渲染，实际上，该元素的宽度是300个设备独立像素。
		* 在chrome的移动端开发模式下，设备对应的像素大小就是该设备的设备独立像素，例如iPhone6的实际物理像素是1080*1920,它的设备独立像素是414*736.
	* 说一说你对设备像素比的理解：
		* 设备像素比device pixel ratio简称dpr,即 物理像素 和 设备独立像素 的比值。
		* 在浏览器中我们可以根据window.devicePixelRatio来获取dpr
		* 在CSS中使用媒体查询min-device-pixel-ratio区分dpr
		* 在CSS中用到最多的px单位，即css像素，当页面缩放比例为100%(默认)，一个css像素 等于一个 设备独立像素。
			* 但是CSS像素是很容易被改变的，当用户对浏览器进行了放大，css像素会被放大，这时一个css像素会跨越更多的物理像素。
			* 页面缩放系数 = css像素 / 设备独立像素
		* 当设备像素比DPR为1：1的时候，设备使用1*1个物理像素显示1个css像素
		* 当设备像素比dpr为2：1的时候，设备使用2*2个物理像素显示一个css像素
	* 说一说你对viewport标签的理解：
		* 该标签定义视区宽高，可缩放范围以及是否可缩放，以及页面整体宽高设置。
	* 说一说你对移动端1px的理解：
		* 在设备像素比大于1的屏幕上，我们设置的1px实际上会被多个物理像素渲染，这就会出现1px在有些屏幕比较粗的情况。
		* 可以通过媒体查询来区别设置：
			* @media only screen and (-min-device-pixel-ratio:2) 
			* 然后用背景图片代替线条宽度设置或者用scale缩小宽度。
	* 说一说HTML5的新特性：
		* 语义化标签：<header>、<footer>、<nav>
		* 音频视频标签：audio、video
		* 数据存储：localStorage、sessionStorage
		* Canvas、Websocket
		* history API
	* 说一说html标签中href和src的区别：
		* href是超文本引用，他指向资源引用位置。
		* src目的把资源下载到页面中。
		* href不会阻塞对文档的处理，src会阻塞对文档的处理。
	* 说一说你对iframe的理解：
		* iframe可以在一个网页中嵌套另一个网页的内容。
		* 优点：
			* 一定程度上属于模块化。
			* 便利不同团队协助开发。
			* 便于第三方内容的添加。
		* 缺点：
			* 不利于搜索引擎搜索。
			* 移动端兼容性问题。
			* 增加请求次数，页面性能降低。
	* 说出两个内联块元素：
		* img
		* input
	* 说一下script标签中的defer和async
		* <script src="xxx"></script>
		* <script src="xxx" defer></script>
		* <script src="xxx" async></script>
		* 不加任何表示的script引入遇到没有属性的script标签，会暂停解析，去发送网络请求获取该JS代码执行后再进行HTML解析，可见会阻塞对HTML的解析。
		* defer:带有该属性的script标签，获取JS代码的请求是异步的，不会阻塞HTML解析。一旦网络请求回来之后，如果此时HTML还未解析完，浏览器会等待HTML解析完毕之后再执行先前获取的JS代码。
		* async:此属性使得script脚本异步加载，不会阻塞HTML解析。一旦网络请求回来之后，不论此时HTML是否解析完毕，立刻执行先去获取的JS代码。
	* 说一说常见的meta属性：
		* charset:指定html编码格式，常见是utf-8
		* name & content:
			* viewport 定义视窗宽高以及缩放范围
			* keyword 提供搜索引擎关键字
			* description 网页整体描述
	* 说出flex布局元素垂直居中的两种方法：(grid同理)
		* .box{
			display:flex;
			justify-content:center;
			align-items:center;
		}

		* .box{
			display:flex;
			justify-content:center;		
		}
		.item{
			align-self:center;
		}
	* 说一下使用flex布局时候，末尾行剩余元素怎样居左对齐。
		* 子元素宽度固定的情况：
			* 当该flex布局第一行有3个元素
			* 第二行有两个元素
			* 每个元素宽度为20vw
			* // 最后一个元素
			* .son:last-child:nth-child(3n-1){
				margin-right:clac(20vw*1);
				}
			* // 倒数第二个元素
			* .son:last-child:nth-child(3n-2){
				margin-right:clac(20vw*2);
				}
		* 或者直接在缺几个div补几个空div
	* 说一下相邻行内元素之间重新空隙的原因以及解决方法：
		* 原因:html标签之间的回车换行符号被解析成了空格了。
		* 解决方法：
			* 设置容器字体大小为0，那个由换行符转换而来的空格大小就被设置为0了。
			```html
			<space>
				<a>111</a>
				<a>123</a>
			</space>
			```
			```css
				space{
					font-size:0;
				}
				a{
					font-size:12px;
				}
			```
	* 说一说display:none和visibiliy:none的区别：
		* visibility:none不会引发重排。
		* visibility元素不会脱离文档流。
	* 说一说flex:1的含义：
		* flex-grow:1;flex-shrink:1;flex-basic:0%;
	* 说一说flex:0%的含义：
		* flex-grow:1;flex-shrink:1;flex-basic:0%;
	* 说一说flex:0 0 的含义：
		* flex-grow:0;flex-shrink:0;flex-basic:0%;
	* 说一说1px问题的解决方案：
		* 给该元素设置::before伪元素，该伪元素content为空，然后宽高为200%，border:1px,使用绝对定位居中然后transform:scale(0.5, 0.5);
	* 你觉得动画最小时间间隔是多久？
		* 一般屏幕刷新率都是60HZ，即1秒刷新60次，即1/60 * 1000ms = 16.7ms
	* 元素竖向的百分比设定是相对于容器的高度嘛？
		* 当按照百分比设定一个元素的宽度时，是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如padding-top,padding-bottom,margin-top,margin-bottom等，当按照百分比设定他们的时候，依据的也是父容器的宽度而不是高度。
	* 说一说CSS优化的建议：
		* 关键内容部分的CSS采用内敛方式
		* 异步加载CSS
			* 怎样实现异步加载呢，可以通过JS对link标签进行控制修改其具体属性
		* 选择器的嵌套层级不要太多，避免使用后代选择器
			* 因为选择器是从右往左进行匹配：
				* 例如#markdown .content h3匹配规则如下：
					* 先匹配h3
					* 去除祖先不是.content的元素
					* 去除祖先不是#markdown的元素 
		* 类名语义化
		* 减少!important的使用
		* 避免重复的部分
		* CSS动画尽量用transform而不是left和top
	* 说一说怎样让chrome支持小于12px的文字
		* zoom属性设置百分比或者小于等于1的小数
		* transform:scale(小于等于1的小数)
	* 说一说sass怎样定义混入
		* @mixin 和 @include
	* 什么是回流(也就是重排)？
		* 浏览器根据样式计算每个盒子在页面的大小与位置
	* 什么是重绘？
		* 计算元素位置、大小外其他样式属性
	* 什么时候触发重排？
		* 页面一开始渲染的时候。
		* DOM操作
		* 元素的位置发生变化的时候
		* 元素的尺寸发生变化的时候
		* 浏览器窗口发生变化的时候
		* 获取浏览器offsetTop、offsetLeft、offsetWidth、offsetHeight、scrollTop等属性时候浏览器为了获取这些值而进行计算，也会进行回流。	
	* 如何减少回流？
		* 如果向设定元素的样式，尽量使用类名修改来实现。
		* 复杂动画为该元素设置positon:absolute脱离文档流减少对其他元素的影响
		* 不要用table布局，容易牵一发动全身
	* 说一说你对px、em、rem、vw的理解：
		* px是指像素单位。
		* em是相对父节点字体大小计算，如果自身定义了font-size则按照自身来计算
		* rem是相对于根节点字体大小计算
		* vw是相对于页面视窗大小计算。
	* 说一下二分查找法的两种解法的思路：
		* 一种是递归
		* 一种是双指针
	* 说一下快速排序的思路：
	* 说一下快速排序、归并排序、堆排序的时间复杂度：
	* 请你简单说一下sort排序源码：
		* 当数组长度小于10使用插入排序，因为对于插排和快排，理论上时间复杂度是O(n^2)和O(n*logN),
		* 但插排的最好情况下时间复杂度是O(n),所以小规模数据用插入排序
		* sort中采用的快速排序使用中间位数的方法，减少快速排序出现最差情况O(n^2)的概率。
	* 说出几个在做promise题时候注意的问题：
		* async函数中await的new Promise如果返回值的话就不执行await下面的代码了
		* .then函数中的参数期待的是函数，如果不是函数的话会发生透传
		* 注意定时器的延迟时间
		* promise函数resolve之后的代码仍会继续执行
		* xx.then()是放到微任务队列中去的
	* 说一说你对Generator函数的理解：
		* function关键字与函数名直接有一个星号
		* 函数体内部使用yeild表达式，定义不同的内部状态
		```javascript
		function* helloWorldGenerator(){
			yield "hello";
			yield "world";
			return "ending";
		}
		```
		* Generator函数会返回一个遍历器对象，而通过yield关键字可以暂停generator函数返回的遍历器对象的状态。
		* 遇到yield关键字，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。
		* 下一次调用next方法时，再继续往下执行，知道遇到下一个yield表达式
		* 如果没有遇到yield关键字就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回对象的value属性值。
		* 没有return的话，value为undeinfed
		* 每次next方法返回的对象的value属性对于状态值，done判断是否存在下一个状态
	* 说一说generator和async await的区别：
		* async函数的返回值是promise对象，这比Generator函数的返回值是一个Iterator更方便，你可以使用then进行下一步。
		* async函数之间调用即可，Generator函数需要调用next方法才可执行。
		* async await的语义化更好
	* 说一下async中不同情况的不同结果：
		* async函数中无await的情况：
			* return一个字面量会得到一个PromiseStatus：resolved的Promise
			* throw一个error会得到一个PromiseStatus: rejected的Promise
		* async函数中有await的情况：
			* async函数会返回一个PromiseStatus:pending的Promise
			* Promise的resolve会使得await的代码节点获取相对于的结果，并继续向下执行。
			* Promise的reject会使得await的代码节点自动抛出相对应的异常，终止向下执行。
	* 说一下快速生成1-100数组的方法：
		* Array.from({length:100},(item,index)=>index+1);
		* 参数1是伪数组属性，参数二是回调函数。
	* 说一下undefined和ReferenceError：xxx is not defined的区别：
		* undefined是声明后未赋值。
		* ReferenceError是尝试引用一个未定义的变量或者函数时。
	* 说一下Math常用的三个取整方法：
		* Math.ceil():向上取整
		* Math.round():四舍五入
		* Math.floor()向下取整
	* 解释一下如下代码：Array.prototype.slice.apply(arguments);	
		* 使用Array原型对象上的slice方法将arguments转换为数组。
	* 说一下mouseenter和mouseover的区别：
		* mouseenter不会冒泡，mouseover会冒泡。
	* 说一下几种常见的异步代码编写方式：
		* 回调函数
		* Promise对象
		* async await
	* 说一下clientWidth/clientHeight和offsetWidth/offsetHeight的区别：
		* 前者是内容宽高，包括content+padding
		* 后者包括content+padding+border+滚动条
	* 说一下clientTop/clientLeft和offsetLeft/offsetRight的区别：
		* 前者是border宽度
		* 后者是距离其父元素的距离。
	* 说一下怎样检测浏览器版本信息：
		* window.navigator.userAgent
	* 说一下什么是点击穿透现象：
		* 就是移动端触发元素单机事件时候可能会触发其底下元素的单机事件，就像冒泡反过来一样。
		* 解决方法是避免混用touch和click
	* 怎样判断当前的运行环境是node还是浏览器
		* 判断全局环境中的this是不是window，
	* setTimeout为什么不能保证能够及时执行？
		* 同步任务先执行，setTimeout真正执行回调函数之前有段等待线程的过程。
	* 说一下几种常见错误：
		* ReferenceError:引用一个未定义变量
		* RangeError：超过数组最大长度。
	* 说一下Promise.all和Promise.allSettled的区别：
		* 前者的promise数组如果有reject的话整个结果就是reject的参数
		* 后者的promise数组就算出现reject也会整个promise对应结果输出。
	* 说一下JS怎样阻止冒泡和默认事件。
		* 组织冒泡：event.stopPropagation()
		* 组织默认事件：event.preventDefault()
		* return false;
	* 说一下浏览器为什么要设置同源策略：
		* 保持A网站和B网站请求与响应的独立性。
		* 不然用户访问了A网站，又去访问B网站，B网站是恶意网站的话，就可能拿着A网站cookie请求A网站后台导致A网站信息泄露。
	* 说一下XML和JSON的区别：
		* JSON是一种独立于编程语言的数据交换格式。
		* XML是一种标记语言，XML应该多用于信息保存或者信息传输吧，我平常没用过。
	* 说一下函数的length:
		* 函数的length就是他第一个有默认值的参数之前的参数个数。
	* 说一下for...of和for...in的区别
		* for..of用于遍历配置了Iterator接口属性的数据结构
		* for...in可用于遍历对象
	* 说一下类数组转数组的方法：
		* Array.prototype.slice.call(likeArray);
		* Array.from(likeArray);	
		* Array.prototype.splice.call(likeArray,0);
	* 说一说js脚本延迟加载的方式有哪些？
		* 添加defer属性
		* 添加async属性
		* 动态修改js的src
		* script标签放在body下面
	* 扩展运算符 和 Objcet.assign是深拷贝还是浅拷贝？
		* 答：浅拷贝
	* typeof NaN的结果是什么？
		* number,且NaN不等于其自身。
	* Object.is()和普通判等的区别：
		* Object.is区分了些特殊情况+0不等于-0,NaN等于NaN
	* 说一下什么是Promise值穿透：
		* Promise的then和catch参数期望是函数，如果不是函数，该then返回promise的data.
	* 如何计算当前html中有多少种标签？
		* 4步
			* 获取所有元素document.querySelectorAll(*")
			* 伪数组转换：[...document.querySelectorAll("*")]
			* 去重：new Set([...document.querySelectorAll("*")]).size
	* 说一说你对window.requestAnimationFrame的理解：
		* 该API使得动画的执行频率是根据设备刷新率/1000ms来动态计算的：
			60HZ / 1000MS = 16.7ms的执行频率
		* 并且页面隐藏的时候不会执行动画节省性能
		* 而且自带节流
	* 什么是虚拟DOM？
		* 虚拟DOM是通过JS对象模拟出来的DOM什么是虚拟DOM？
		* 虚拟DOM是通过JS对象模拟出来的DOM节点。
	* 虚拟DOM一定更快嘛？
		* 不一定，如果页面上只有一个按钮，点击+1，肯定还是直接操作DOM更快。
	* 
	* 虚拟DOM一定更快嘛？
		* 不一定，如果页面上只有一个按钮，点击+1，肯定还是直接操作DOM更快。
	* 谈谈你对浏览器中进程和线程的理解：
		* 浏览器是多进程的，主要包括以下进程：
			* Browser进程：浏览器的主进程，唯一，负责创建和销毁其他进程、网络资源的下载与管理、浏览器界面的展示、浏览器的前进与后退
			* GPU进程：用于3D绘制
			* 第三方插件进程：每种类型插件对应一个进程，使用该插件的时候才会创建。
			* 浏览器内核(浏览器渲染进程)：
				* 浏览器的内核又是多线程的：
					* GUI渲染线程：负责渲染浏览器界面，当页面需要进行重绘或者重排的时候，该线程就会执行
					* JS引擎线程：也称为js内核(例如v8引擎)，负责处理JS脚本程序，解析JS脚本，运行代码。
					* 事件触发线程：用于控制浏览器的事件循环。
					* 定时触发器线程：setTimeout和setInterval所在的线程。
					* 异步HTTP请求线程：在XMLHttpRequest连接后通过浏览器新开一个线程请求，检测到状态变更的时候，就将回调函数放入事件队列中，由JS引擎执行。
					* 注意GUI渲染线程和JS引擎是互斥的(界面渲染和JS代码解析执行互斥)
				* JS的单线程：
					* 所谓的单线程，是指JS引擎中负责解释和执行JS代码的线程唯一，同一时间上只能执行一件任务。
	* 说一说你对Object.definedProperty的理解：
		* Object.definedProperty可以在一个对象上定义新属性或者修改现有属性，返回该对象。
		* 参数1是被操作对象，参数2是被操作对象的属性，参数3是配置项对象
		* 配置项有如下：
			* configurable:
			* enumberable:	
			* value:
			* writable:	
			* get:
			* set:
	* 说一说什么是尾调用？
		* A函数的最后一步是调用B函数，这就是尾调用。
		* 因为函数的调用会在内存中形成调用记录，如果在函数A中调用函数B，等到函数B执行完毕，A和B的调用记录才会都消失。
		* 如果是尾调用，执行到函数B的时候，函数A已经都执行完毕了，可以直接删除函数A的执行记录，只保留B函数的调用记录，节省内存。
		* 尾递归同理，即A函数最后一步调用A函数自身。
	* 写出一个为对象部署Iterator遍历接口的过程：
		```javascript
	let myIterable = {
    	a: 1,
    	b: 2,
    	c: 3
	}
	myIterable[Symbol.iterator] = function() {
  	let self = this;
  	let arr = Object.keys(self);
  	let index = 0;
  		return {
    		next() {
      			return index < arr.length ? {value: self[arr[index++]], done: false} : {value: undefined, done: true};
    			}
  			}
		}

		var it = myIterable[Symbol.iterator]();

		it.next();

		for(const i of myIterable) {
  			console.log(i);
		}
		```
	* 说一下可枚举性(enumerable)是什么？
		* 如果一个属性的enumerable是false,他将不会被for...in、Object.keys、JSON.stringify方法给枚举到。
		* 可以通过Object.definedPorperty来定义enumerable
		* 顺带一提，for...in会遍历对象继承自原型对象上的属性，Object.keys只会遍历自身的属性。
	* 说一下forEach中可以使用await嘛？有哪些循环可以使用await
		* forEach使用await无效，也不支持break;
		* for循环和for...of可以
	* 如何中断Promise？
		```
			function timerOuter(promise, time){
				const promise2 = new Promise((resolve,reject) => {
					setTimeout(()=>{
						reject("timeout");
					}, time);
				})
				return Promise.race([promise, promise2]);
			}
		```
	* 说一下你对栈和堆的理解：
		* 内存的角度上来说栈和堆
			* 栈：栈由操作系统自动分配释放，用于存放函数的参数值、局部变量等，变量以先后定义顺序依次压入栈中，且栈的内存地址的生长方向由高到底，所以后定义的变量地址低于先定义的变量。栈中存储的数据的生命周期随着函数的执行完毕而释放。
			* 堆：堆是需要手动释放的内存空间不一定相邻的，存放内容由程序员决定。
		* 数据结构角度上来说栈和堆：
	说一说实现微前端的方案：
		* 栈即先进后出的数据结构。基本操作有入栈、出栈、获取栈顶
			* 堆：一种树状结构，分为大根堆和小根堆，一般用数组来存储堆。
	* 说一下Object.create是干嘛的？
		* 传入一个对象，然后生成新对象，并且新对象的.__proto__指向参数对象。
	* 说一下箭头函数能当作构造函数嘛？为什么？
		* 不能，先简述一下new的过程。
		* 但是箭头函数是没有自身的this的，其寻找其外部最近函数上下文当作自身的this且不可再改变this指向。
	* 说一说什么是微前端？
		* 微前端是一种类似微服务的架构，将多个小型前端应用聚合为一的应用。
		* 各个小前端还可以独立开发，独立部署。
	* 说一说微前端的价值：
		* 整合新老项目。
		* 单个项目进行拆分，单独模块维护。
	* 说一说实现微前端的方案：
		* iframe
		* web component
	* [] == ![]的结果是什么？
		* []转换为0
		* ![]首先转换为布尔值，![]为!true为false
		* 0 == false
		* true
	* forEach中return有效果嘛？如何中断forEach?
		* 不会，
		* 可以使用every或者some代替forEach
	* 解释一下什么是内存泄漏？是什么原因导致的？
		* 程序中已动态分配的堆内存由于某种原因未释放或者无法释放
		* 原因可能是因为闭包或者
	* 说一下你对diff算法的理解：
		* DIFF算法是一种比对算法。
		* 通过对比新虚拟DOM和旧虚拟DOM，找出是哪个虚拟DOM进行了改动，找到这个有改动的虚拟DOM并且只更新该虚拟DOM对应的真实DOM，而不更新其他没有改动的节点，进而提高效率。
		* DIFF算法在比较的时候只会进行同层级比较，所以应该是广度优先遍历。
		* patch方法：
			* 该方法会首先判断新旧虚拟DOM还是否是同一种类型的标签：
				* 两者不是同一种标签了直接将整个节点替换未新虚拟节点。
				* 两者是同一种标签的话继续向下执行
			* 补充：判断条件如下:两者key值是否一样、标签名称是否一样、是否都为注释节点、是否都定义了data
		* patchVnode方法：
			* 先找到对应的真实dom对象
			* 判断newVnode和oldVnode是否指向同一个对象，如果是，直接return
			* 如果都有文本节点且不相等，将el的文本节点设置未newVnode的文本节点。
			* 如果oldVnode有子节点而newVnode没有子节点，则删除el的子节点。
			* 如果oldVnode没有子节点而newVnode有子节点，则将newVnode的子节点真实化后添加到el
			* 如果oldVnode和newVnode都有子节点，则执行updateChildren函数比较子节点。
			* 说明的时候可以画个草图：真实DOM，旧虚拟DOM，新虚拟DOM
	* 解释一下为什么不建议用index作为元素的key
		* 假设有个li列表，key为1，2，3，内容分别为A,B,C
		* 在li列表头部添加个新li,现在列表key为1,2,3,4,内容分别为new,A,B,C
		* 在添加后操作只后，新旧虚拟DOM的对比的patch阶段，他会把新虚拟dom中的key1,2,3仍当做是旧虚拟dom标签的文本发生了改变，新虚拟dom的key4当作是新添加的标签，所以key1,2,3,4整个都会进行真实dom的更新。
		* 如果key是唯一的，在添加操作之后，新旧虚拟DOM的对比阶段就可以知道后三项无改动，第一项为新添加项，针对性更新真实DOM即可。
	* 说一下虚拟DOM和操作真实DOM相比的优势：
		* 当使用传统方法操作DOM的时候，会触发重排，浏览器会从构建Dom树开始重新执行一遍流程。
		* 当你在一次需要更新10个节点的操作当中，浏览器收到第一个更新DOM请求后并不知道后续还有9个更新操作，最终会执行10次DOM更新操作。
		* 同样是更新10个节点，虚拟DOM并不会直接进行更新，而是将这10次更新的内容保存为一个JS对象，最终将该JS对象一次性的渲染映射到真实DOM上。
	* 说一说些网页性能优化方案：
		* 对于html来说：
			* 减少dom操作，减少重排次数。
		* 对于css来说：		
			* 减少选择器嵌套层数，
			* CSS动画元素多设置postion:absolute/fixed减少重排
		* 对于UI库：实现UI库按需加载
		* 实现图片懒加载。
		* 使用CDN
		* 图片压缩
		* 前端开启缓存
	* 说一说常见的WEB攻击方式：
		* XSS跨站脚本攻击：
			* 存储型XSS：攻击者将恶意代码放在目标网站数据库中，用户打开该网站，服务端将恶意代码返回给了用户本地浏览器执行，攻击者就通过恶意代码被执行获取了用户的相关信息，进行冒充用户的行为。
			* 前端防止XSS恶意代码的方法有：前端向数据库存储内容的时候对于大于号小于号需要进行转义。在使用innerHTML等API的时候注意用户自定义行为
		* CSRF:
			* 受害者登入了A网站，保存了cookie
			* 受害者登入了B恶意网站，B恶意网站拿着上面那个cookie访问了A网站后台，冒充受害者进行操作。
			* 浏览器的同源策略就是防止这种攻击
	* 说一说什么是单点登入？
		* 例如淘宝、天猫都是阿里旗下的，当用户登入淘宝之后，再打开天猫，系统自动帮用户登入了天猫，这就是单点登入。
		* 同域名下的单点登入：
			* cookie的domain属性设置为当前域名的主域名，path属性设置为根路径，那么共同主域名下的网站就可以实现单点登入，例如tieba.baidu.com和map.baidu.com
		* 不同域名下的单点登入：
			* 首先需要一个统一的认证中心，集中处理用户信息。
			* 认证中心下的业务系统A不处理用户登入信息，再业务系统A发起请求后跳转至认证中心验证是否登入，如果未登入就跳转至认证中心登入页面，用户登入成功后跳转至业务系统A，此时业务系统登入成功后的认证信息就存储在认证中心中。
			* 然后现在有业务系统B，系统B进行请求被重定向到了认证中心，认证中心检测到用户已经登入，则重定向回系统B，进行登入后的操作。
	* 说一说SPA的优势吧：
		* 内容改变不用重新加载整个页面。
		* 打包后只有一个index.html
	* 说一说实现SPA的原理：
		* 监听地址栏中hash变化而驱动界面变化
	* 说一说HASH路由的原理，写出他的核心代码：
		* 监听地址栏中的hash变化而驱动界面变化
		* 大致代码如下：
		```javascirpt
		class Router{
    		constructor(){
        	this.routers = {}; // 用来存放路径名称以及回调函数   
            	window.addEventListener('load', (e) => console.log(e), false);  
        	window.addEventListener('hashchange', (e) => console.log(e), false);
		}
    		// - 路由注册函数 -
    		route(path, callback){
        	this.routers[path] = callback;
    		}
    		// - 路由更新函数 -
    		push(path){
        	window.location.hash = path;
        	this.routers[path] && this.routers[path]();
    		}
		}

		let miniRouter = new Router();

		miniRouter.route("/test", () => console.log("切换到了test页面"));
	miniRouter.push("/test");
		```
		* history路由同理主要api换为history.pushState、history.replaceState、history.popState
		
	* 怎么判断元素是否在可视区域？
		* element.offsetTop - document.documentElement.scrollTop <= document.documentElement.clientTop
		* element.getBoundingClient的top和left要大于等于0，element.getBoundingClienReact的bottom要大于等于document.docuemntElement.clientHeight并且element.getBoundingClientReact的right要大于等于document.documentElement.clientWidth
	* 说一下cookie的Domain和Path属性：
		* Domian指定了cookie可以送达的主机名称
		* Path指定了一个URL路径，这个路径必须出现在要请求的资源路径中才会发送cookie首部。
	* 说一下什么是内存泄漏？JS中有哪些内存泄漏的情况？
		* 内存泄漏是指由于疏忽或者错误程序未能释放以及不再使用的内存。
		* 常见的内存泄漏的情况：
			* 意外的全局变量：
				* 在函数内定义变量并没有使用var或者let关键字，这使得该变量不会随着函数的执行完毕而销毁，而是在全局作用域中一直存在。
			* 闭包：
				* 内部函数持续引用外部函数变量，使得外部函数作用域并不会进行回收。
			* 取消时间监听：
				* 在使用事件监听addEventListener的时候，在不监听的情况下使用removeEventListener取消对元素事件的监听。
	* 说一说BOM中的常见对象：
		* window:全局对象，所有全局变量都是其属性
		* location:控制hash,控制url,控制刷新
		* navigator:保留浏览器属性
		* history:控制url前进后退
	* 说一说DOM中的常见操作：
		* 创建节点：createElement
		* 创建文本节点：createTextNode
		* 获取节点：querySelector
		* 获取节点：parentElement
		* 更新节点：innetHTML
		* 更新节点：appendChild
		* 更新节点：insertBefore
		* 更新节点：setAttribute
		* 更新节点：classList
		* 删除节点：remvoeChild
	* setTimeout中的回调函数回到主栈执行时是在全局执行上下文的环境中执行的，其this指向window
	* 说一下ajax的原理：
		* ajax的原理简单来说就是通过XMLHttpRequest来对服务器发出异步请求，从服务器获取数据，然后用js操作DOM更新页面。
	* 写出XmlHttpRequqest的基本使用过程：
	```
		    const xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status >= 200 && xhr.status <= 300){
                console.log(xhr.responseText);
            }else{
                console.log(xhr.status);
            }
        }
    }
    xhr.open("get","https://api.apiopen.top/getJoke?page=1&count=2&type=video");
    xhr.send();
	```
	* 说一说你对执行栈的理解(也就是调用栈)
		* 执行栈，也叫做调用栈，先进后出结构，用于存储代码在执行期间所创建的所有执行上下文。
		* 当JS引擎开始执行第一行代码的时候，他会创建一个全局执行上下文压入执行栈中。
		* 当引擎碰到一个函数的时候，他会创建一个函数执行上下文压入执行栈中。
		* 引擎回执行位于执行栈栈顶的执行上下文，当该函数执行完毕后，对应的执行上下文就会被弹出，然后控制流程到达执行栈的下一个执行上下文。
	* 说一说JS中的继承：
		* 原型链继承：	
			```
			    function Father(){
        this.name = "father";
        
    }

    function Son(){
        this.age = 12;
    }

    Son.prototype = new Father();

    const father = new Father();
    const son1 = new Son();
    console.log(son1.name);
		* 不过原型链继承的话是存在问题的，如果如果父构造函数有属性值是引用数据类型的话，子构造函数实例化A直接修改该属性的话，子构造函数实例化B对应的该属性也会被改动，因为两者共享着父构造函数的prototype对象
			```
		* 构造函数修改this继承：
			```
			    function Father(){
        this.name = "father";
        
    }

    function Son(){
        Father.call(this);
        this.age = 12;
    }

    const son1 = new Son();

    console.log(son1.name);
			```
			* 这种写法的话呢，有一个弊端是指父构造函数的prototype指向的原型对象上的属性和方法，子构造函数实例化对象是拿不到的。
		* 所以可以直接让以上两种方法组合来实现继承。
	* 说一说浅拷贝和深拷贝：
		* 什么是浅拷贝：
			* 浅拷贝就是创建新的数据，这个数据有着原始数据属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用数据类型，拷贝的就是内存地址。
		* 以下是浅拷贝的方法：
			* Object.assing
			* Array.prototype.slice()
			* Array.prototype.concat()
			* 扩展运算符也是浅拷贝
		* 深拷贝新的数据和原始数据完全相同且是不同的内存地址，修改一个引入数据类型的属性，不会影响另一个对象的属性。
		* 常见深拷贝的方法：
			* JSON.parse(JSON.stringify(obj));
			* 自定义的递归函数
			```
    function deepClone(target){
        if(typeof target !== 'object'){
            return target;
        }
        let obj = Array.isArray(target) ? [] : {};        
        for(let item in target){
            obj[item] = deepClone(target[item]);
        }
        return obj;
    }
			```
	* 说一说显式转换和隐式转换有哪些方法：
		* 显示转换：
			* Number()
			* String()
			* Boolean()
		* 隐式转换：
			* ==
			* !==
			* 字符串+其他东西
	* 说一说字符串常用方法：
		* concat():
		* slice():
		* repeat()
		* indexOf()
		* chartAt()
		* split()
		* search()
		* replace()			
	* 说一说数组常用方法：
		* push()
		* unshift()
		* splice()
		* concat()
		* pop()
		* shift()
		* slice()
		* indexOf()
		* includes()
		* find()
		* sort()
		* reverse()
		* join()
		* some()
		* every()
		* forEach()
		* fliter()
		* map()

	
	 
