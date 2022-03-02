# 1. MF 和乾坤的优劣势

1．single-spa：模块级微前端

2．乾坤：基于single-spa,却丢失了sigle-spa模块的颗粒度，应用级微前端

3．MF：基于webpack5的模块级微前端方案

# 2. Qiankun的沙箱隔离主要实现了三种模式：

LegacySandbox  单例模式

ProxySandBox 多应用模式

snapshotSanBox  不支持以上的降级方案

# 3. 微前端是为了解决什么问题

历史项目共享，应用/模块 重复性使用，

提高团队的跨地，跨项目，跨技术栈的协同能力和模块化能力

开箱即用，为插拔式开发，低代码拼装上，提供了无限想象，拓展能力

# 4. http1.1,http2.0,https 的不同

多路复用,头部压缩,server push

https是http请求上，通过CA证书对称加密验证传送的步骤。

1.浏览器传输一个 随机数 和加密方法列表，服务器返回 证书，服务端随机数，加密方法列表，公钥等

2.浏览器验证正式合法性后（CA校验域名IP是否符合），浏览器使用公钥，浏览器随机数，服务端随机数，加密方法生成 新的随机数

3.服务器对称加密验证通过后，使用公钥生成 随机密钥，则后面就使用该密钥进行通信，加解密。

 

总结

1.CA证书保证了公钥的可靠性。
2.服务端私钥+公钥的非对称加解密保证了客户端生成的随机数传输安全，不会被中间人拦截获取。But，非对称加密对服务端开销大。
3.所以利用随机数的对称加密保证后续通讯的安全性，也可以降低服务器的解密开销。
4.HTTPS只针对传输内容进行加密，保证的是客户端和网站之间的信息就算被拦截也无法破解。如果不是全站HTTPS，仅仅只是在登录页采用HTTPS，那些HTTP连接的页面同样是危险的，从HTTP->HTTPS跳转依然可能被劫持。国内的部分银行就是这样，对安全性的考量还比不上百度，百度早就全站HTTPS了

# 5. webpack，loader和plugin 区别

webpack的打包原理

识别入口文件

通过逐层识别模块依赖(Commonjs、amd或者es6的import，webpack都会对其进行分析，来获取代码的依赖)

webpack做的就是分析代码，转换代码，编译代码，输出代码

最终形成打包后的代码

什么是loader

loader是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中

处理一个文件可以使用多个loader，loader的执行顺序和配置中的顺序是相反的，即最后一个loader最先执行，第一个loader最后执行

第一个执行的loader接收源文件内容作为参数，其它loader接收前一个执行的loader的返回值作为参数，最后执行的loader会返回此模块的JavaScript源码

什么是plugin

在webpack运行的生命周期中会广播出许多事件，plugin可以监听这些事件，在合适的时机通过webpack提供的API改变输出结果。

loader和plugin的区别

对于loader，它是一个转换器，将A文件进行编译形成B文件，这里操作的是文件，比如将A.scss转换为A.css，单纯的文件转换过程

plugin是一个扩展器，它丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务

# 6. 首屏幕优化 有哪些手段

CDN加速

三方依赖使用script

SSR渲染

gzip压缩

Service work 缓存处理

模块抽离vendor

# 7. Vue3.0的优化

静态标记

事件缓存

双向数据绑定核心优化

options 编程方式升级为hooks编程方式

# 8. React 和 vue 的diff 区别

1.Vue进行diff时，调用patch打补丁函数，一边比较一边给真实的DOM打补丁

2.Vue对比节点，当节点元素类型相同，但是className不同时，认为是不同类型的元素，删除重新创建，而react则认为是同类型节点，进行修改操作

3.① Vue的列表比对，采用从两端到中间的方式，旧集合和新集合两端各存在两个指针，两两进行比较，如果匹配上了就按照新集合去调整旧集合，每次对比结束后，指针向队列中间移动；

②而react则是从左往右依次对比，利用元素的index和标识lastIndex进行比较，如果满足index < lastIndex就移动元素，删除和添加则各自按照规则调整；

③当一个集合把最后一个节点移动到最前面，react会把前面的节点依次向后移动，而Vue只会把最后一个节点放在最前面，这样的操作来看，Vue的diff性能是高于react的

# 9. AMD，CMD的区别

common.js 基于module.exports的规范 同步的加载方案

运行时加载，一个模块就是一个对象，加载一个模块就是加载这个模块的所有方法，然后读取其中需要的方法。

AMD  是RequireJS推广过程中产生的定义规范 是异步的加载方案

AMD | 速度快 | 会浪费资源 | 提前加载依赖，浪费资源

CMD 是seaJs的推广规范，同步加载方案

CMD | 只有真正需要才加载依赖 | 性能较差 | 直到使用的时候才加载依赖，

ES6/export/import

编译时加载，值饮用，按需加载

# 10. CI，CR，CD的流程

# 11. 浏览器强缓存

强缓存

Expires  http1.0字段 设置缓存过期的绝对时间

Cache-control http1.1字段，

  设置缓存过期的相对时间

   1. no-store 2. max-age 3.no-cache

  协商缓存：

Last-Modified & If-Modified-Since

服务器通过 Last-Modified 字段告知客户端，资源最后一次被修改的时间，例如

Last-Modified: Mon, 10 Nov 2018 09:10:11 GMT

浏览器将这个值和内容一起记录在缓存数据库中。

下一次请求相同资源时时，浏览器从自己的缓存中找出“不确定是否过期的”缓存。因此在请求头中将上次的 Last-Modified 的值写入到请求头的 If-Modified-Since 字段

服务器会将 If-Modified-Since 的值与 Last-Modified 字段进行对比。如果相等，则表示未修改，响应 304；反之，则表示修改了，响应 200 状态码，并返回数据。

但是他还是有一定缺陷的：

如果资源更新的速度是秒以下单位，那么该缓存是不能被使用的，因为它的时间单位最低是秒。

如果文件是通过服务器动态生成的，那么该方法的更新时间永远是生成的时间，尽管文件可能没有变化，所以起不到缓存的作用。

Etag & If-None-Match

为了解决上述问题，出现了一组新的字段 Etag 和 If-None-Match

Etag 存储的是文件的特殊标识(一般都是 hash 生成的)，服务器存储着文件的 Etag 字段。之后的流程和 Last-Modified 一致，只是 Last-Modified 字段和它所表示的更新时间改变成了 Etag 字段和它所表示的文件 hash，把 If-Modified-Since 变成了 If-None-Match。服务器同样进行比较，命中返回 304, 不命中返回新资源和 200。

Etag 的优先级高于 Last-Modified

 
当浏览器要请求资源时

调用 Service Worker 的 fetch 事件响应

查看 memory cache

查看 disk cache。这里又细分：

如果有强制缓存且未失效，则使用强制缓存，不请求服务器。这时的状态码全部是 200

如果有强制缓存但已失效，使用对比缓存，比较后确定 304 还是 200

 

发送网络请求，等待网络响应

把响应内容存入 disk cache (如果 HTTP 头信息配置可以存的话)

把响应内容 的引用 存入 memory cache (无视 HTTP 头信息的配置)

把响应内容存入 Service Worker 的 Cache Storage (如果 Service Worker 的脚本调用了 cache.put())

# 12. 市场新的一些技术关注

web3.0  基于区块链的去中心化生态

dapp，基于区块链的去中心化app

vite vue3.0的打包工具

比webpack 冷启动更快，热更新更快

原因：1.vite 使用go 编译，比node.js 更快  2.原生ESM上执行，且利用缓存

 

# 13. 装饰器

   装饰器是对类、函数、属性之类的一种装饰，可以针对其添加一些额外的行为。通俗的理解可以认为就是在原有代码外层包装了一层处理逻辑。

 

# 14. promsie 控制并发

function limitRunTask(tasks, limit) {

return new Promise(function(resolve,reject){

let index = 0, alive = 0, finish = 0, result = [];

function next() {

if(finish >=tasks.length) {

resolve(result);

return;

}

while(alive < limit && index < tasks.length) {

alive++;

const promise = tasks[index]();

const curIndex = index;

promise.then(function(value) {

alive--;

finish++;

result[curIndex] = value;

next();

});

index++

}

}

next();

})

}

finally 的实现

Promise.prototype.finally = function (callback) {

  let P = this.constructor;

  return this.then(

    value  => P.resolve(callback()).then(() => value),

    reason => P.resolve(callback()).then(() => { throw reason })

  );

};

 

 

 
