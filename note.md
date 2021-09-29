# 杂记🤓🤓🤓
* (`递归`):不是说递归就要return，仅仅遍历一遍树状结构不用return东西
* (递归):return的东西原路返回了。
* (CommonJS):暴露是module.exports 注意有个"s"
* (CommonJS):CommonJS的导入方式是同步的
* (browserify):用来将commonjs方式require的包导入至代码上端
* (babel):用来将ES6代码转为ES5代码
* (node_modules):可执行文件在/node_modules/.bin目录下
* (CommonJS):CommonJS借助browserify工具实现require（browserify index.js -o bundle.js）
* (ES6 Module):es6-module借助babel进行es6->es5(babel ./src --out-dir ./lib),然后借助browserify实现require(browserify index.js -o bundle.js)
* (工作)困了的话就戴耳机听歌
* (git)终端查看git提交记录 git log --graph
* (gitk)一个git提交记录查看命令工具
* (代码健壮)对数组的判断:Array.isArray(data) && data.length || []
* (shell)接受参数：$0,$1
* (Generator生成器):*号声明的函数第一次执行产生生成器，后续通过next()进行逐步调用
* (call和apply):注意call和apply的调用者是被修改this指向的对象，call和apply的第一个参数是新的this，第二个开始参数是新this需要的参数
* (Generator生成器):next()方法中参数可以设置上一个yield的返回值
* (深拷贝浅拷贝):解构赋值是深拷贝，不要被结构赋值对象里面的基本数据类型和引用输入类型搞混
* (sass):注意sass待转换文件必须是xxx.sass文件(捂脸.gif)
* (sass):sass的几个关键点:嵌套、变量、混合器、继承、导入
* (sass):  :global用来覆盖UI库样式
* (闭包):闭包一定要有return吗？
* (call和apply)：注意call和apply修改了某函数的this执行并且立即执行，bind需要手动再执行
* (框架):框架中data的更新与UI层的渲染，注意复杂数据类型更新UI层无感的情况，注意key的使用
* (Object.prototype.toString):判断类型，Object.prototype.toString()永远的神。let ts = Object.prototype.toString() ts.call(待判断对象)
* (无穷大与无穷小):Infinity和-Infinity
* (引用数据类型)：注意引用数据类型之间的赋值问题。（写leetcode的回文字符串时候数组的赋值。应该用深拷贝而不是直接赋值）
* (异步):异步处理的发展:回调函数 --> promise --> Generator --> async await --> RXJS
* (函数):函数语句和函数表达式。函数语句存在提升的情况。
* (迭代)：基于函数的迭代比基于循环的迭代要慢一些。
* (dom):createDocumentFragment()作为子dom添加进父dom时，只会将documentFragment其所有的子孙节点添加给父dom，不包括docuemntFragment其自身。
* (虚拟DOM算法)：并不是虚拟DOM就比真实DOM操作要快,而是用虚拟DOM算法去操作真实DOM，比直接操作真实DOM性能更高。
* (虚拟DOM算法):虚拟DOM算法 = 虚拟DOM + DIFF算法
* (Math.random()):获取min和max之间的随机数：(Math.random() * (max - min)) + min
* (Math.random()):获取min和max之间的整数(包括max和min)：Math.floor((Math.random() * (max - min + 1))) + min
* (箭头函数):箭头函数后的函数体不带花括号即默认return
* (CSS选择器):选择除第一个元素后的其他元素:p:not(:nth-child(1)){color:pink}
* (some 和 every)：some中当return了一个true,就不再循环。every中return了一个false,就不再循环。
* (React 源码):render方法实际上是调用了React.createElement方法(ReactElement方法)
* (避免操作state):避免操作结构赋值取得的state的方法目前是：{...state.xxx,newKey:newValue},用扩展运算符扩出新的对象。或者JSON.paurse(JSON.stringify(state.xxx))深拷贝
* (浅拷贝的坑)：不要忘记浅拷贝的坑，类似moment对象的使用
* (some和every)：之前记住的some返回一个true就停止循环，every返回一个false就停止循环，注重的是循环的过程。some只要数组中某一元素满足条件即返回true,every需要数组中所有元素满足条件才返回ture,强调的是返回值。
* (好看的webstorm字体)：Raleway                                        │~                                                                     
* （元素的宽高）:元素的宽等于 width + border*2 + padding*2             │~                                                                     
* (content-box)：width就是width,border就是border,padding就是padding    │~                                                                     
* (border-box)：width+2*border+2*padding == 设置的width，width会被borde│~                                                                     
r和padding挤压。
* (事件委托):事件委托就是将元素的事件委托给他的父级元素或更外级的元素处理，实现机制就是事件冒泡。
* (addEveneListener):addEventListener有三个参数(event,function.useCapture),第三个参数决定事件处理函数在哪个阶段被调用(冒泡阶段 和 捕获阶段)
* (阻止事件冒泡): 子元素设置：event.stopPropagation() 或者直接 return false
* (事件委托):为父元素绑定某事件之后，通过event.target.nodeName.toLowerCase()==="li" 的方法判断被点击元素的tagName
