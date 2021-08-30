# 杂记
* (递归):不是说递归就要return，仅仅遍历一遍树状结构不用return东西
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
