# NodeJs

Node.js 是一个 Javascript 运行环境(runtime)。发布于2009年5月，由Ryan Dahl开发，实质是对Chrome V8引擎进行了封装。 

优势：

1. NodeJs 语法完全是 js 语法，只要你懂 JS 基础就可以学会 Nodejs 后端开发。

2. NodeJs 超强的高并发能力。

3. 实现高性能服务器。

4. 开发周期短、开发成本低、学习成本低

## 安装

1.官网直接下载node：https://nodejs.org/zh-cn/

2.设置npm淘宝镜像源

第一种：直接下载cnpm

```powershell
npm install -g cnpm
```

第二种：给npm设置镜像源

```powershell
npm config set registry https://registry.npmmirror.com
```

3.下载nodemon，这个类似于vscode中的live Server，js内容改变会自动更新

```powershell
npm install -g nodemon
```

> 使用`npm list -g --depth 0`命令可以查看全局安装了哪些包，yarn可以使用`yarn global list`命令查看全局安装了哪些包

## 模块

核心模块（系统自带的模块）

- 文件操作的fs

- http服务操作的http

- url路径操作模块

- path路径处理模块

- os操作系统信息

第三方模块

art-template、jquery、vuex、express等，必须通过npm来下载才可以使用

自定义模块：自己创建的文件

### http模块

1.新建js文件

2.在文件中使用http模块

```js
// 1.引入http模块
let http = require('http')
// 2.creatServer搭建服务
http.createServer((req,res)=>{
	console.log('监听成功')
  // 为了识别html代码，设置请求头text/html
  // 为防止网页乱码设置请求头，使用utf-8编码
	res.setHeader("Content-Type","text/html;charset=utf-8")
	res.write('11111')
	// res.end('11111这是后台传输的数据~~')
	res.end('<em>11111这是后台传输的数据~~</em>')
}).listen(8000)
/* 上面的是链式调用，另一种实现方式 */
let app = http.createServer((req,res)=>{
	console.log('监听成功')
	res.setHeader("Content-Type","text/html;charset=utf-8")
	res.write('11111')
	res.end('<em>11111这是后台传输的数据~~</em>')
})
app.listen(8000)
```

### CommonJS

node应用由模块组成，采用的commonjs模块规范。

每一个文件就是一个模块，拥有自己独立的作用域，变量，以及方法等，对其他的模块都不可见。

CommonJS规范规定，每个模块内部，module变量代表当前模块。

这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。

加载某个模块，其实是加载该模块的module.exports属性。

require方法用于加载模块。

#### 导出模块

```js
let a = '新年快乐~~'

function add(...argu) {
    var temp = 0
    argu.forEach((value, index, array) => {
        temp += value
    })
    return temp
}
exports.str = a
exports.add = add
// 导出是成这样的 { str: '新年快乐~~', add: [Function: add] }
```

>不允许这样导出文件 exports = {a, b, c}，这样导出，得到的是一个空对象{}，必须要给定一个属性包裹，如：exports.obj = {str: a, add}   // { obj: { str: '新年快乐~~', add: [Function: add] } }

#### 导入模块

```js
// 引入导出模块
let obj = require('./03-导出模块')
console.log(obj) // { str: '新年快乐~~', add: [Function: add] }
```

`exports`和`module.exports`其实是相等的

```js
// exports和module.exports都没有赋值的情况下，它们是相等的。
console.log(exports === module.exports) // true
/*-----------------------------------------------------*/
// exports不允许使用exports = {a,b,c}方式导出，但是module.exports可以使用这种方式导出
let a = 1, b = 2, c = 3;
module.exports = { a, b, c }
// 导出结果：{ a: 1, b: 2, c: 3 }
```

#### 模块缓存

1.导出数据

```js
let a = 1
let b = 2
let c = 3
exports.obj = { a, b, c }
```

2.引入导出模块

```js
let { obj } = require('./03-模块缓存')
console.log(obj) // { a: 1, b: 2, c: 3 }
obj.d = 4
console.log(obj) // { a: 1, b: 2, c: 3, d: 4 }
// 重新引入导出模块
let {obj:obj1} = require('./03-模块缓存')
// 因为之前已经引入过了该模块，系统会自动引用该模块的变量，不会重复加载。
// 这样做的目的:避免重复加载,提高模块的加载效率
console.log(obj1) // { a: 1, b: 2, c: 3, d: 4 }
```

> 所以重复加载同一个模块会有模块缓存，不需要重复引入加载

### path模块

说明一下路径的`\`和`/`，Linux系统用的是`:/`，Windows系统用的是`:\`。

path模块中的方法返回值会自动转换为Windows系统所使用的`\`。

介绍两个通用文件变量`__filename`和`__dirname`，无需引入模块

```js
//指向当前文件所在的绝对路径
console.log(__filename) // E:\qianduan\前端化工程\02-url,path,fs模块\05-path模块.js
//指向当前js文件所在文件夹的绝对路径
console.log(__dirname) // E:\qianduan\前端化工程\02-url,path,fs模块
```

使用path模块，path模块中有许多方法，这里只介绍了三种。

join方法：路径拼接

```js
// 引入path核心模块
const path = require('path')

let p1 = path.join('file','01.js')
console.log(p1) // file\01.js

let p2 = path.join(__dirname,'05-path模块.js') 
console.log(p2) // E:\qianduan\前端化工程\02-url,path,fs模块\05-path模块.js
console.log(p2 ===__filename) // true
```

resolve方法：将参数转换成绝对路径

```js
const path = require('path')
// 1.不带参数时：返回的是当前文件所在文件夹的绝对路径
console.log(path.resolve()) // E:\qianduan\前端化工程\02-url,path,fs模块
// 2.带参数
// 2.1 带没有/开头的参数：返回的是当前文件所在文件夹的绝对路径与参数拼接的结果
console.log(path.resolve('a.js')) // E:\qianduan\前端化工程\02-url,path,fs模块\a.js
// 2.2 带./开头的参数，和2.1的效果一样
console.log(path.resolve('./b.js')) // E:\qianduan\前端化工程\02-url,path,fs模块\b.js
console.log(path.resolve('./a','./b.js')) //E:\qianduan\前端化工程\02-url,path,fs模块\a\b.js
// 2.3 带/开头的参数：返回的是根路径(盘符)拼接最后一个前面加/的文件名再拼接剩下的参数
console.log(path.resolve('/a')) // E:\a
console.log(path.resolve('/a','b')) // E:\a\b
console.log(path.resolve('/a','b','c','/d')) // E:\d
console.log(path.resolve('/a','b','c','/d','e')) // E:\d\e
```

parse方法：解析路径

```js
const path =require('path')
console.log(path.parse('C:/Users/Guai/Desktop/教学/01-初识node/02-http模块.js'))
/*
{
  root: 'C:/',
  dir: 'C:/Users/Guai/Desktop/教学/01-初识node',
  base: '02-http模块.js',
  ext: '.js',
  name: '02-http模块'
}
*/
```

### url模块

通常使用的是url模块中的URL类

```js
let {URL} = require('url')
// URL:也是对路径进行解析
let url  = new URL('http://127.0.0.1:8000/login?username=zx&&password=123456')
console.log(url)
/*
URL {
  href: 'http://127.0.0.1:8000/login?username=zx&&password=123456',
  origin: 'http://127.0.0.1:8000',
  protocol: 'http:',
  username: '',
  password: '',
  host: '127.0.0.1:8000',
  hostname: '127.0.0.1',
  port: '8000',
  pathname: '/login',
  search: '?username=zx&&password=123456',
  searchParams: URLSearchParams { 'username' => 'zx', 'password' => '123456' },
  hash: ''
}
*/
```

### querystring模块

stringify方法：对参数进行拼接(地址栏型)

```js
let queryString = require('querystring')
console.log(queryString)
/*
{
  unescapeBuffer: [Function: unescapeBuffer],
  unescape: [Function: qsUnescape],
  escape: [Function: qsEscape],
  stringify: [Function: stringify],
  encode: [Function: stringify],
  parse: [Function: parse],
  decode: [Function: parse]
}
*/
let stringfly = queryString.stringify({
    username: 'zx',
    password: 123456
})
console.log(stringfly) // username=zx&password=123456
```

### fs模块

对文件进行操作

1.stat方法：判断是否是文件还是文件夹

  ```js
let fs = require('fs')
// 第一个参数是文件地址
fs.stat('./aa/bb',(err,stats)=>{
  if(err){
    console.log(err)
    return false
  }
  // isFile方法判断是否是文件
  else if(stats.isFile()){
    console.log('文件')
  }
  // isDirectory判断是否是文件夹
  else if(stats.isDirectory()){
    console.log('文件夹')
  }
})
  ```

2.mkdir方法：创建文件夹

```js
let fs = require('fs')
// 第一个参数是要创建夹的地址和名字
// 这里就是在该文件同级目录下创建了一个javascript文件夹
fs.mkdir('./javascript', (err) => {
    if(err){
        console.log(err)
        return false
    }
    console.log('创建文件夹成功')
})
```

3.writeFile方法：写文件内容

```js
let fs = require('fs')
// 第一个参数是要写入内容的文件地址，如果地址中没有该文件，则会创建一个新文件
// 第二个参数就是要写入的内容
fs.writeFile('./zx.txt','这是我写入的内容',(err) => {
  if(err){
    console.log(err)
    return false
  }
  console.log('写入成功')
})
```

4.appendFileSync()方法：通过异步的方法将文本内容或数据添加或追加到文件里，如果文件不存在则会自动创建

```js
let fs = require('fs')
// 第一个参数是要写入内容的文件地址，如果地址中没有该文件，则会创建一个新文件
// 第二个参数就是要写入的内容或追加的内容
fs.appendFileSync('./zx.txt','这是新加的内容')
```

5.readFile方法：读取文件的内容

```js
let fs = require('fs')
// 第一个参数是要读取内容的文件地址
fs.readFile('zx.txt',(err,data)=>{
  if(err){
    console.log(err)
    return false
  }
  // data打印出来的是一堆16进制数据，需要通过toString方法转换
  console.log(data.toString())
})
```

6.readdir方法：读取目标文件夹下文件或文件夹的名称

```js
let fs = require('fs')
// 第一个参数是要读取子目录名称的文件夹地址
fs.readdir('./javascript',(err,data) => {
  if(err){
    console.log(err)
    return false
  }
  console.log(data) // [ 'html', 'js' ]
})
```

7.rename方法：剪切文件或者修改文件名字

```js
let fs = require('fs')
// 第一个参数是目标文件
// 第二个参数是修改的文件名或移动后的文件名
fs.rename('zixuan.txt','zx.txt',(err) => {
  // 这里将名字改成了zx
  if(err){
    console.log(err)
    return false
  }
  console.log('修改成功')
})
fs.rename('zixuan.txt','./javascript/zx.txt',(err) => {
  // 这里将文件移动到了javascript文件夹下并将名字修改成zx
  if(err){
    console.log(err)
    return false
  }
  console.log('修改成功')
})
```

8.rmdir方法：删除目录，只能删除空的文件夹

```js
let fs = require('fs')
// 第一个参数是要删除的目标文件夹
fs.rmdir('./javascript', (err) => {
  if (err) {
    console.log(err)
    return false
  }
  console.log('删除成功')
})
```

9.unlink方法：删除文件

```js
let fs = require('fs')
// 第一个参数是要删除的目标文件
fs.unlink('data.json',(err) => {
  if(err){
    console.log(err)
    return false
  }
  console.log('删除成功~~')
})
```

#### 暴力删除文件夹

```js
let fs = require('fs')
let Path  = require('path')

function del(path) {
  /* fs.statSync(path)
   statSync是同步版，只接收一个path变量
   fs.statSync(path)其实是一个fs.stats的一个实例 */
  // 先获取到这个地址所对应的对象
  let stats = fs.statSync(path)
  //判断是否为文件夹
  if (stats.isDirectory()) {
    //判断是否为空的文件夹
    //读取文件夹当中的所有内容
    /*fs.readdirSync(path)
    	readdirSync是同步版，只接受path变量
    	返回一个包含“指定目录下所有文件名称”的数组对象
    */
    let fileList = fs.readdirSync(path)
    if(fileList){
      // E:/qianduan/前端化工程/03-暴力删除,爬虫/aa/ + filepath
      fileList.forEach(filepath=>{
        //递归的思路
        del( Path.join(path,filepath))
      })
    }
    //当里面的文件全部删除后，删除空的文件夹
    fs.rmdirSync(path)
  }
  //这是文件
  if (stats.isFile()) {
    fs.unlinkSync(path)
  }
}
del('E:/qianduan/前端化工程/03-暴力删除,爬虫/aa')
```

#### 文件流读取数据

在文件比较小的情况下，可以使用readfiles的方式去读取；如果文件比较大的情况下；可以用文件流的形式去读取。

```js
let fs = require('fs')
//流的形式读取文件
var readStream = fs.createReadStream('input.txt')
//保存数据
var str = ''
//读取数据
readStream.on('data', (chunk) => {
  str += chunk
})
//读取完成
readStream.on('end',()=>{
  console.log(str)
})
//读取失败
readStream.on('error',(err)=>{
  console.log(err)
})
```

#### 文件流写入数据

```js
let fs = require('fs')
let data = '我是要写入的内容~~~~\n'
// 创建流，并指定存放的地方
var writeStream = fs.createWriteStream('output.txt')
for (var i = 0; i < 500; i++) {
    writeStream.write(data,'utf-8')
}
//标记写入完成
writeStream.end()
//写入完成
writeStream.on('finish',()=>{
    console.log('写入完成')
})
//写入失败
writeStream.on('error',()=>{
    console.log('失败')
})
```

#### 管道流

用流的形式传递数据

```js
let fs = require('fs')
// 创建一个可读流
var readStream = fs.createReadStream('output.txt')
// 创建一个可写流
var writeStream = fs.createWriteStream('beifen.txt')
// 将读取到的流以pipe的方式写入到指定文件中
readStream.pipe(writeStream)
console.log('程序执行完毕')

```

### request模块

这是第三方模块，需要使用`npm install`的方式下载

```js
let request = require('request')
request.get({
  url: 'https://www.baidu.com/' // 你要爬取的网站
}, (err, res, body) => {
  if (err) {
    console.log(err)
    return false
  }
  // err：错误信息
  // res：作出响应,返回你需要的信息
  // body：响应中的body信息，一般是网站页面
  console.log(body)
})
```

#### 爬取图片

```js
const fs = require('fs')
let request = require('request')
request.get({
  url: 'http://photo.china.com.cn/'//你要爬取的网站
}, (err, res, body) => {
  if (err) {
    console.log(err)
    return false
  }
  //  http://images.china.cn/site1000/2019-10/23/a01dd522-0fce-430f-928c-003330f5faaa.jpg
  var reg = /http:\/\/images\.china\.cn\/site1000\/\d+-\d+\/\d+\/[\da-z-]+\.jpg/g
	// match()方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
  let arr = body.match(reg)
  console.log(arr)
  if (arr) {
    arr.forEach((item) => {
      fs.appendFileSync('./imgSrc.txt',item +'\n' ) 
    })
  }
})
```

#### 以流的形式爬取视频

```js
const request = require('request')
const fs =require('fs')

request('https://vd2.bdstatic.com/mda-nbe0whkhmmzywvxu/720p/h264_delogo/1644885820101382517/mda-nbe0whkhmmzywvxu.mp4?v_from_s=hkapp-haokan-nanjing&auth_key=1645019889-0-0-25c9a1a8c5480adbad313fbe0e82097f&bcevod_channel=searchbox_feed&pd=1&pt=3&logid=1689858061&vid=998662053351900729&abtest=17451_2&klogid=1689858061')
    .pipe(
       fs.createWriteStream('./mp4/video.mp4')
    )
```

#### 爬取小说

这里需要用到另一个第三方模块`cheerio`，也是需要下载的。

`cheerio`跟`jquery`类似，它把jquery库中一些内容进行了删减，保存了真正需要的API。

```js
let request = require('request')
let cheerio = require('cheerio')
let fs = require('fs')

request("http://huayu.zongheng.com/showchapter/1092749.html", (err, res, body) => {
  if (err) return
  // 使用cheerio模块加载body
  let $ = cheerio.load(body)
  // 使用cheerio模块解析html
  $('.col-4 a').each(function (index) {
    // 进行跳转,并且爬取文本内容
    content($(this).prop('href'), index)
  })
})
function content(url, index) {
  request(url, (err, res, body) => {
    let $ = cheerio.load(body)
    let txt = $('.content').text()
    txt = txt.replace(/。/g, n => n + '\n')
    fs.writeFileSync(`./xiaoshuo/${index + 1}`, txt)
  })
}
```

### art-template模块

将模板中的变量进行替换。这是个第三方模块，需要`npm install`下载。

```js
/* 模板引擎.html */
<body>
  我的名字是:{{name}}
	我的年龄是:{{age}}
	我们现在的课程是:{{course}}
</body>
/* art-template.js */
let template = require('art-template')
let fs = require('fs')
fs.readFile('./模板引擎.html', (err, data) => {
  if (err) {
    console.log(err)
    return false
  }
  let temp = template.render(data.toString(), {
    name: 'zx',
    age: 18,
    course: 'node'
  })
  console.log(temp) // 打印出来的是模板被替换的html代码
})
```

### express框架

这是个第三方模块，需要下载。该模块用于http请求。

- 简单使用

```js
let express = require('express')
let app = express()
// 可以匹配多个路径，每个路径不同展示不同的页面
app.get('/', (req, res) => {
  res.send('这是根路径~~')
})
app.get('/login', (req, res) => {
  res.send('login页面')
})
app.get('/index', (req, res) => {
  res.send('index页面')
})
app.listen(8000, () => {
  console.log(`监听8000端口成功~~`)
})
```

- 动态匹配和传递参数

```js
let express = require('express')
let app = express()
// http://localhost:8000/course/web?pagenum=1
app.get('/course/:name', (req, res) => {
  let { name } = req.params
  let { pagenum } =req.query
  res.send(`${name}课程第${pagenum}页`) // 页面展示：web课程第1页
})
app.listen(8000, () => {
  console.log(`监听8000端口成功~~`)
})
```

- 使用错误路由中间件来匹配不存在的路由，当路由不匹配时，将会调用路由中间件

```js
let express = require('express')
let app = express()
// http://localhost:8000/courdeedsada
app.get('/course', (req, res) => {
  res.send('course课程页面')
})
app.use((req,res)=>{
  res.send('这是404页面')
})
app.listen(8000, () => {
  console.log(`监听8000端口成功~~`)
})
```

- 通过中间件调用自身的API，去开放指定目录中的资源

1.在js文件的同级目录下新建public文件夹，在public文件夹下新建html文件夹，在html文件夹下新建404.html文件

2.在js文件中指定开放目录

```js
app.use(express.static('./public/'))
```

3.使用开放目录中的文件

```js
res.redirect('./html/404.html')
```

总代码

```js
let express = require('express')
let app = express()
//通过中间件调用自身的API,去开放指定目录中的资源
app.use(express.static('./public/'))
// http://localhost:8000/courdeedsada
app.get('/course', (req, res) => {
  res.send('course课程页面')
})
app.use((req,res)=>{
  res.redirect('./html/404.html')
})
app.listen(8000, () => {
  console.log(`监听8000端口成功~~`)
})
```

#### express结合art-template

这里需要用到另一个包`express-art-template`，也是需要下载。在这代码中虽然没有用到`art-template`这个包，但还是需要下载这个包的，因为`express-art-template`这个包里面依赖了很多`art-template`这个包中的内容。

```js
var express = require('express');
var app = express();
// 第一个参数指定的是要进行模板匹配文件后缀名，
// 如：要匹配的是html文件，这里写的就是html；匹配的是css文件，这里写的就是css
app.engine('html', require('express-art-template'));
app.get('/roles', (req, res) => {
  /* res.render有一个默认的规则,它会自动从views目录中查找该模板文件
  	所以和中间不同的是它需要在该文件同级目录下新建一个views目录 */
  res.render('./html/roles.html', {
    name: 'zx',
    age: 18,
    content: '帅的不要不要的!!!!!!!'
  });
});
app.listen(8000, () => {
  console.log('监听8000端口成功~~')
})
```

#### express处理post请求

需要用express自带的方法来开启获取post参数

```js
app.use(express.urlencoded({ extended: false }))
```

```js
let express = require('express')
const app = express()
//express框架的自带方法获取post参数
app.use(express.urlencoded({ extended: false }))
app.engine('html', require('express-art-template'));
//通过中间件去调用自身expressAPI去开发指定目录的资源
app.use(express.static('./public/'))
app.get('/', (req, res) => {
  res.send('根路径')
})
app.get('/login', (req, res) => {
  // 这里渲染模板的是views目录
  res.render('./html/login.html', {
    name: '账号',
    psw: '密码'
  })
})
app.post('/post', (req, res) => {
  // 获取post参数是req.body
  if (req.body.name == 'zx' && req.body.psw == '123456') {
    // 这里跳转的地方是通过中间件开放的public目录
    res.redirect('./html/index.html')
  }
})
app.listen(3000, () => {
  console.log('监听3000端口号成功~')
})
```

## cookie

cookie是存储于访问者的计算机中的变量。可以让我们用同一个浏览器访问同一个域名的时候共享数据。

HTTP是无状态协议。当你浏览了一个页面，然后跳转到同一个网站的另一个页面，服务器无法认识到这是同一个浏览器在访问同一个网站。每一次的访问，都是没有任何关系的。

cookie特点：

- cookie保存在浏览器本地
- 正常设置的cookie是不加密的，用户可以自由看到
- 用户可以删除cookie
- cookie可以修改
- cookie可以用于攻击
- cookie存储量很小，最大只有4k

使用时需要下载cookie包

```powershell
npm install cookie-parser
```

```js
let express = require('express')
let app = express()
// 引入
let cookieParser =require('cookie-parser')
// 设置中间件
app.use(cookieParser())
app.get('/',(req,res)=>{
  // 获取cookies需要设置中间件
  console.log(req.cookies) // { username00: 'zixuan00' }
  res.send('这是8000端口的根路径')
})
app.get('/set',(req,res)=>{
    //去设置cookie信息，通过键值对的形式，参数有三个，最后一个是配置信息option
  res.cookie('username00','zixuan00',{
    maxAge: 1000000, // 设置过期时间
    domain: '.zixuan.com', // 指定那个域名能共享cookie信息
    path:'/index', // 设置缓存的路径,只有/index才能访问得到cookie信息
    secure:false, // 设置是否只能通过https来传递此条cookie  true:https   false:http
    httpOnly:true // 设置为true时不能通过document.cookie来访问cookie信息,防止程序(JS)脚本来读取Cookie信息,防止XSS攻击
  })
  res.send('设置cookie内容')
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~')
})
```

## session

session运行在服务端，当客户端第一次访问服务器时，可以将用户的登录信息保存。当用户访问其他页面时，可以判断客户的登录状态，做出提示，相当于登录拦截。

session可以和数据库等结合做持久化操作，当服务器挂掉时也不会导致某些客户信息(购物车)丢失。

使用时需要安装session包

```powershell
npm install express-session
```

```js
let express = require('express')
let request = require('request')
let app = express()
//引入session
let session = require('express-session')
//express框架的自带方法获取post参数
app.use(express.urlencoded({ extended: false }))
//通过中间件调用自身的API,去开放指定目录中的资源
app.use(express.static('./public/'))
//设置seesion 的中间件
app.use(session({
  secret: 'keyboard cat', // 一个string类型的字符串，作为服务器端生成session签名
  name: '', // response中sessionID这个cookie的名称，默认为connect.sid，也可以自己设置
  resave: false, // 强制保存session即使它没有变化，默认为true，建议改为false
  saveUninitialized: true,//无论有没有session cookie 每次请求都设置一个session cookie
  cookie: {}, // 设置返回到前端值，默认值:{path:'/',httpOnly:true,secure:false,maxAge:null}
  rolling: true // 在每次请求时都会更新cookie信息,重置cookie过期时间（默认false）
}
))
app.get('/', (req, res) => {
  res.send('这是8003端口的根路径')
})
app.get('/index', (req, res) => {
  if (req.session.userinfo) {
    res.redirect('./html/index.html')
  } else {
    res.redirect('./html/login.html')
  }
})
app.post('/login', (req, res) => {
    //登录成功
  if (req.body.user == 'zx' && req.body.psw == '123456') {
    req.session.userinfo = 'zx'// 这是会员标志
    request('http://127.0.0.1:8003/index', (err) => {
      if (err) {
        console.log(err)
        return false
      }
      res.redirect('./html/index.html')
    })
  }
  else {
    res.send('信息有误')
  }
})
app.listen(8003, () => {
  console.log('监听8003端口成功~~')
})
```

## session和cookie的区别

1.cookie数据存放在客户的浏览器上，session数据放在服务器上

2.cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session

3.session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie

4.单个cookie保存的数据不能超过4k，很多浏览器都限制一个站点最多保存20个cookie

## mongoDB

1.下载安装mongoDB

2.下载模块mongoose

```powershell
npm install mongoose
```

3.自定义数据库模块

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
/*
    定义表格式
    name 姓名  string
    psw  密码  string
    age  年龄  number
    sex  男/女 0 / 1 number
*/

//3.指定表规则
const Schema = mongoose.Schema
//Schema:对表结构的定义
let userSchema = new Schema({
    //require:是否必填
    name: { type: String, required: true },
    pwd: { type: String, required: true },
    age: { type: Number, required: true }
})
//4.建表
const User = mongoose.model('user', userSchema)
//5.导出: 多次利用
module.exports = User
```

简单使用自定义模块

```js
let express = require('express')
let User = require('./model/user') // 自定义模块
let app = express()
// 使用中间件开放目录资源
app.use(express.static('./static/'))
// express框架的自带方法获取post参数
app.use(express.urlencoded({extended:true}))
app.get('/login',(req,res)=>{
  res.redirect('login.html')
})
app.post('/regist',(req,res)=>{
  //获取数据,准备数据调用方法,添加到数据库当中去
  let userData = {
    name:req.body.name,
    pwd:req.body.pwd,
    age:req.body.age,
  }
  /*crud
      create 创建
      find 查询
      updata  更新
      delete 删除
    */
  // 添加数据到数据库中
  User.create(userData)
  res.send('注册成功~~~')
})
app.listen(8001,()=>{
    console.log('监听8001端口成功~~')
})
```

### create添加

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
/*
    定义表格式
    name 姓名  string
    psw  密码  string
    age  年龄  number
    sex  男/女 0 / 1 number
*/
//3.指定表规则
const Schema = mongoose.Schema
//Schema:对表结构的定义
let userSchema = new Schema({
    //require:是否必填
    name: { type: String, required: true },
    pwd: { type: String, required: true },
    age: { type: Number, required: true }
})
//4.建表
const User = mongoose.model('user', userSchema)
/* 增加单条数据方法 */
var user =new User(
  {
    name:'小绿',
    pwd:'123456',
    age:25
  }
)
// 使用这种方式只能创建一条数据
user.save((err,data)=>{
  if(err){
    console.log(err)
    return false
  }else{
    console.log(data)    
  }
})
/* 增加多条数据方法 */
User.create(
  {
    name: '小黑',
    pwd: '123456',
    age: 18
  },
  {
    name: '小红',
    pwd: '123456',
    age: 20
  },
  {
    name: '小紫',
    pwd: '123456',
    age: 25
  }
)
```

### find查询

#### 条件查询

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
/*
    定义表格式
    name 姓名  string
    psw  密码  string
    age  年龄  number
    sex  男/女 0 / 1 number
*/
//3.指定表规则
const Schema = mongoose.Schema
//Schema:对表结构的定义
let userSchema = new Schema({
    //require:是否必填
    name: { type: String, required: true },
    pwd: { type: String, required: true },
    age: { type: Number, required: true }
})
//4.建表
const User = mongoose.model('user', userSchema)
//5.条件查询
/* 普通条件查询
var wherestr = {'name':'小黑'}
*/
/*$nor取反
var wherestr = {$nor: [{'name':'小绿'},{'age':25}]}
*/
/*$or或者
var wherestr = {$or: [{'name':'小绿'},{'age':20}]}
*/
/*$gt $gte $lt $lte $ne  大于 大于等于 小于 小于等于 不等于
var wherestr = { age: { $gt: 20 } }
*/
/*$in $nin 在/不在 指定的这个数据是否存在
var wherestr = { age: { $in: 20 } }
var wherestr = { age: { $in: [20, 25]} }
*/
/*$exists 判断该属性是否存在
var wherestr = { age: { $exists: true } }
*/
/*可以使用javascript代码
var wherestr = { $where: "this.age ===18" }
*/
/*正则
var wherestr = { name: /小紫/ }
*/
User.find(wherestr,(err,data)=>{
  if(err){
    console.log(err)
    return false
  }else{
    console.log(data)
  }
})
```

#### 查询

```js
/*引入、连接数据库、创建表省略*/
/*Model.find( conditions, [projection], [options], [callback] );
	conditions  查询条件
	projection  返回内容选项
	options     查询配置选项
	callback    回调函数，参数err data，可用Promise代替

	options:    查询配置选项 常用skip limit sort 
		{skip:2}        略过前2条数据
		{limit:5}       最多返回5条属性
		{sort:{age:1}}  按照age项升序排列
*/
var wherestr = { 'pwd': '123456' }
User.find(
  wherestr, //查询条件
  // 返回内容：初始化设定的表结构要不全部指定为0，要不全部指定为1，_id可以与设定的不同
  { name: 1, pwd: 1, age: 1, _id: 0 },
  // { skip: 2, limit: 5, sort: { age: 1 } },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
/*查询一条findOne*/
User.findOne(
  wherestr, //查询条件
  { name: 1, pwd: 1, age: 1, _id: 0 },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
/*通过id查询数据findById*/
User.findById(
  '6218cf773c2ec6f8b3ec6d1f',
  { name: 1, pwd: 1, age: 1, _id: 1 },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
```

### update修改

```js
/*引入、连接数据库、创建表省略*/
/*Model.update(conditions,doc,[options],[callback])
	conditions   查询条件（和find一样）
	doc          要修改的内容
	options      选项
	callback     回调 参数err,info(更新成功的信息) ，可以用Promise代替
*/
/*修改一条数据*/
User.updateOne(
  { 'pwd': '123456' },
  { 'pwd': '111111' },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
// 增加表结构
userSchema.add({ 'sex': { type: String, require: true } })
User.updateOne(
  { 'pwd': '123456' },
  { 'sex': '男' },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
/*修改多条数据*/
User.updateMany(
  { 'pwd': '123456' },
  { 'pwd': '111111' },
  (err, data) => {
    if (err) {
      console.log(err)
      return false
    } else {
      console.log(data)
    }
  }
)
```

### 验证

存入数据库的数据符合设置的规则

#### 验证必填

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
//3.指定表规则
let userSchema = new mongoose.Schema(
  {
    // required:是否必填
    pwd: {
      type: String,
      required: [true, 'pwd字段不能为空']
    },
  }
)
//4.建表
let User = mongoose.model('user', userSchema)
User.create(
  {   
    pwd: '222222',
  }
)
```

#### 验证选项

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
//3.指定表规则
let userSchema = new mongoose.Schema(
  {
    // min：最小数值 max：最大数值
    age: {
      type: Number,
      min:[18,'未成年禁止访问'],
      max:100
    },
    // enum：枚举，数据必须为其中一个
    sex: {
      type: String,
      enum:['男','女']
    },
    // minlength：最小字符长度 maxlength：最大字符长度
    pwd: {
      type: String,
      maxlength:12,
      minlength:6
    },
  }
)
//4.建表
let User = mongoose.model('user', userSchema)
User.create(
  {   
    age:18,
    sex:'不知道',
    pwd:'123451234512345'
  }
)
```

#### 自定义验证

```js
//1.引入模块
let mongoose = require('mongoose')
//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
let userSchema = new mongoose.Schema(
  {
    name:{type:String},
    pwd: {
      type: String,
      //密码长度必须要要在6-18之间,  包含字母,下划线,数字三个中的两个
      validate: (value) => {
        if (/[A-Za-z]/.test(value) + /_/.test(value) + /\d/.test(value) > 1) {
          return /^\w{6,18}$/.test(value)
        } else {
          return false
        }
      }
    },
    age:{type:Number}
  }
)
let User = mongoose.model('user', userSchema)
User.create(
  {
    name: 'zixuan666666',
    pwd: '12345aa612345aa612345aa612345aa6',
    age: 25
  }
)
```

### 中间件

定义钩子函数

```js
//1.引入模块
let mongoose = require('mongoose')

//2.连接数据库
mongoose.connect('mongodb://localhost:27017/teacher', {
  useNewUrlParser: true, //URL字符串解析器
  useUnifiedTopology: true, //服务器的监视引擎
}).then(() => {
  console.log('连接数据库成功')
}).catch((err) => {
  console.log('数据库连接失败')
  console.log(err)
})
//3.指定表规则
const Schema = mongoose.Schema
//Schema:对表结构的定义
let userSchema = new Schema({
    //require:是否必填
    name: { type: String, require: true },
    pwd: { type: String, requie: true },
    age: { type: Number, requie: true }
})
// 定义勾子，执行命令前先执行这个函数
userSchema.pre('find', (next) => {
  console.log('find方法执行之前,先执行这里的内容')
  // 必须写next()，要不然执行不下去
  next()
})
// 定义勾子，完成执行命令后先执行这个函数
userSchema.post('find', (date) => {
  console.log(date)
  console.log('find方法执行之后,再执行这里')
})
//4.建表
const User = mongoose.model('user', userSchema)
//5.条件查询
User.find(
  { 'name': '小紫' },
  (err, date) => {
    console.log('这是find方法本身的回调函数')
  }
)
```

> 先执行pre函数，再执行post函数，最后才触发find回调函数

## koa框架

[Koa](https://koa.bootcss.com/) 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。

下载koa框架

```powershell
npm install koa
```

简单使用

```js
const koa = require('koa')
const app = new koa()
app.use( async ctx => {
  // 原生
  //res.end()
  // express
  // res.send()
  // koa
  //ctx => context :包含了req,res对象
  ctx.body = 'hello koa~'
  // 页面显示：hello koa~
})
app.listen(8001,()=>{
    console.log('监听8001端口成功~~')
})
```

### koa-router

想要和express一样使用get，post方法，需要先安装`koa-router`，`npm install koa-router`

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
// 实例化
const router = new Router()
// 启动路由
app.use(router.routes())
/* 普通写法 */
router.get('/index', async ctx=>{
  ctx.body ='首页'
})
router.get('/web', async ctx=>{
  ctx.body ='web课程页面'
})
router.get('/python', async ctx=>{
  ctx.body ='python课程页面'
})
/*koa-router还可以使用链式调用，来写接口*/
router
  .get('/index', async ctx => {
  	ctx.body = '首页'
	})
  .get('/web', async ctx => {
  	ctx.body = 'web课程页面'
	})
  .get('/python', async ctx => {
  	ctx.body = 'python课程页面'
	})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

### get请求

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
// 实例化
const router = new Router()
// 启动路由
app.use(router.routes())
// http://127.0.0.1:8003/index?name=zx&pwd=123456
router.get('/index', async ctx => {
  ctx.body = '首页'
  console.log(ctx.query) // { name: 'zx', pwd: '123456' }
  console.log(ctx.request.query) // { name: 'zx', pwd: '123456' }
  console.log(ctx.querystring) // name=zx&pwd=123456
  console.log(ctx.url) // /index?name=zx&pwd=123456
  // 对get请求来说，ctx.query和ctx.request.query获取到的值是一样的
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

### 静态资源处理

想要和express一样开放指定目录资源，需要下载`koa-static`，`npm install koa-static`

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
const router = new Router()
app.use(router.routes())
// 引入包
const serve = require('koa-static')
// 配置开放目录路径
app.use(serve(__dirname +'/public'))
router.get('/login', async ctx => {
  // ctx.body = '登录页面'
  ctx.redirect('/html/login.html')
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

### post请求

想要获取到post请求的数据，需要下载`koa-bodyparser`，`npm install koa-bodyparser`

```js
const koa = require('koa')
const app = new koa()
// 引入包
const bodyParser = require('koa-bodyparser')
// 使用bodyParser，必须在启动路由之前配置bodyParser插件
app.use(bodyParser())

const Router = require('koa-router')
const router = new Router()
// 启动路由
app.use(router.routes())

const serve = require('koa-static')
// 配置开放目录路径
app.use(serve(__dirname +'/public'))

router
	.get('/login', async ctx => {
  	// ctx.body = '登录页面'
  	ctx.redirect('/html/login.html')
	})
  .post('/post',async ctx=>{
    // console.log(ctx.body)//不能用,获取不到
    // console.log(ctx.request.body)//可以用,可以获取到
    let {user} = ctx.request.body
    let {pwd} = ctx.request.body
    if(user==='zx'&&pwd==='123456'){
      ctx.body= '登录成功~'
    }else{
      ctx.body ='登录失败'
    }
    // post请求需要使用ctx.request.body获取参数，不能像get一样可以省略request
  })
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

> 必须要在启动路由之前配置bodyParser插件

### 动态路由

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
const router = new Router()
// 启动路由
app.use(router.routes())
router.get('/course/:aid/:temp', async ctx => {
  // console.log(ctx.params.aid)
  // 直接通过ctx.params获取动态路由的值，一定要切记要完全匹配路径
  ctx.body = `二级路由:${ctx.params.aid},三级路由:${ctx.params.temp}`
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

### 错误路由中间件

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
const router = new Router()
// 启动路由
app.use(router.routes())
// 设置开放资源
const serve = require('koa-static')
app.use(serve(__dirname + '/public'))
// 错误路由中间件
app.use(ctx => {
  if (ctx.status === 404) {
    ctx.redirect('./html/404.html')
  }
})
//koa定义路由的时候,先统一处理为404,响应之后的status为200
router.get('/index', async ctx => {
  console.log(ctx.status) // 404
  ctx.body = '首页'
  console.log(ctx.status) // 200
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

> koa定义路由的时候,先统一处理为404,响应之后的status为200

## ejs模板

这是第三方模块，需要下载`ejs`，`npm install ejs`。ejs模板和art-template模板类似，但是书写不同，ejs模板创建的文件是以`.ejs`为后缀名的，但是里面的代码和html类似；而且里面的模板表达式不同，ejs模板的有多种表达式，templat是以`{{}}`作为模板识别的。详见[ejs](https://ejs.co/)

```js
const koa = require('koa')
const app = new koa()
const Router = require('koa-router')
const router = new Router()
// 启动路由
app.use(router.routes())
// 设置开放资源
const serve = require('koa-static')
app.use(serve(__dirname + '/public'))
// 错误路由中间件
app.use(ctx => {
  if (ctx.status === 404) {
    ctx.redirect('./html/404.html')
  }
})
router.get('/index', async ctx => {
  var temp ='我是temp'
  var arr = ['111','222','333']
  ejs.renderFile(
    './public/ejs/index.ejs',
    {
      msg: '我是ejs后台返回的数据',
      msg1: '<em>我是ejs后台返回的数据</em>',
      msg2: temp,
      arr
    },
    (err, date) => {
      if (err) {
        console.log(err)
        return false
      }
      ctx.body = date
    }
  )
})
app.listen(8000,()=>{
  console.log('监听8000端口成功~~'')
})
```

./public/ejs/index.ejs

```ejs
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
  <h1>ejs的首页</h1>
  <%= msg %> <br>
  <%- msg1 %> <br>
  <%= msg2 %> <br>
  <ul>
    <% for(var i=0; i<arr.length; i++) {%>
      <li>
        <%=arr[i]%>
      </li>
    <% }%>
  </ul>
</body>
</html>
```

