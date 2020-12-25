<center><h1>
    nodejs学习
</center>



nodejs不是一个独立的语言，他是使用JavaScript进行开发的语言，并且它不像php，jsp，asp等语言需要web服务器的支持才可以运行。



​	nodejs特点：三个特点其实说的就是一个特点，谁离开谁都不行，举例来说，nodejs就像一个餐厅的抠门老板，单线程就像一个服务员进行整个饭店中所有客户的要求进行处理，当然当客人点过菜厨师在做的时候，就去做其他的事情而不是在等待，而多线程就像是厨师在做菜，服务员在等厨师做菜一样！

​		缺点：不健壮，不能作为真正的服务支撑，作为一个小工具倒是可以

​		1.单线程

​				nodejs只有一个线程，不像其他语言对每一个用户都创建一个2MB的线程，这样就造成了一个8GB的服务器也就同时4000人同时在线，在硬件成本比较			浪费，而nodejs使用一个线程，当有用户连接进来了，就会出发一个内部事件，通过触发非阻塞I/O和事件驱动机制，让本身实现宏观上的并行。大大的节			省了服务器创建和销毁线程的事件。

​				当然其本身也有缺点，那就是如果当一个用户造成线程崩溃，那麻所有的用户都会完蛋。

​		2.非阻塞I/0

​				在线程处理中，单线程在进行I/0数据处理时，往往需要数据的返回才能进行下一个事件的处理。而nodejs中会将请求在I/O数据在处理等待时去处理其他			的请求来节省时间资源，当前面这个io执行完之后通过回调函数通过事件驱动通知执行。所以在非阻塞的情况下的CPU使用率永远都是100%。

​		3.事件驱动

​				nodejs的整理处于一个事件的无限循环中，比如用户A进行连接，发送一个请求，请求通过回调函数将结果返回给用户，事件驱动接着处理下面的用户的			请求，如果该用户的请求存在io读取数据库访问通信等操作，线程不会等待这些操作，而是会继续下一个用户的请求处理，当前面的数据请求完成后会继续			生成一个请求放入队列中，当排队到这个请求时通过回调函数返回给用户！当然这个配对机制是有优先级的，他会优先将老用户请求的提前处理

​			![image-20201223152001102](/root/.config/Typora/typora-user-images/image-20201223152001102.png)				







第一个Hello word



http简单使用

```js
var http = require('http');

var server = http.createServer(function(request, response){

    response.writeHead(200,{'Content-type':'text/html;charset=UTF-8'});
    response.end("Hello word Nodejs");
}).listen(8080);
```



fs文件处理

```js
var http = require('http');

var fs  = require('fs');

var server = http.createServer(function(request, response){

    var filepath = '';
    if(request.url == '/index'){
        filepath = './index.html'
    }else {
        filepath = './404.html'
    }

    fs.readFile(filepath, function(err, data){
        response.writeHead(200,{'Content-type':'text/html;charset=UTF-8'});
        if (err){
            console.log("异常了")
            response.end('异常了');
        }else{
            response.writeHead(200,{'Content-type':'text/html;charset=UTF-8'});
            console.log(data)
            response.end(data);
        }
        
    })
}).listen(8080);


```



commonJS:自定义模块和核心模块

​	commonJS就是将单独独立的模块抽离出来提供给其他模块进行使用

​	自定义模块其实和核心模块的原理相同

​	Exports进行导出

```js
function appendUrl(url) {
    return "http://127.0.0.1/8080"+url
}
exports.appendu = appendUrl;//1


var obj = {
    apd1 : function (url) {
        return "http://127.0.0.1:8081"+url;
    },

    apd2 : function () {
        return "这是第二个测试方法";
    }
}

exports.ap = obj;//2
module.exports = obj;//3
```

​	require进行导入使用

```js
//在导入使用时
const appendurl = require('./module/index.js');//1
var api = appendurl.appendu(request.url);

const apd = require('./module/index1.js');//2
apd.ap.apd1(request.url)
apd.ap.apd1(request.url)

apd.apd1(request.url)//3
apd.apd2(request.url)
```



require中的参数遵循规则

​	如果是路径，则跟着路径寻找就行，当然后缀.js可以不添加都一样

​	就像核心模块http,他会先去node_modules文件下找到http的文件夹，然后找到index.js作为引用，

​	如果不存在index.js则去找到package.json文件找到main节点的指向文件

​	![](/root/.config/Typora/typora-user-images/image-20201224160018177.png)

## ES6基本语法

```js
/* Ex6基本语法 
let 块级作用域  
const 块级作用域  常量
var 全局作用域
*/
var ma = true;
if (ma) {
    //let name = 'zhangsan';
    //var name = 'zhangsan';

}

const name = 'zhangsan'
//name = 'lisi'   //报错  常量不可修改
console.log(name);

/* 箭头函数 */
let fun = (name) => {
    console.log(name);

}
fun('张三');

/* 模板字符串 */
let age = 20;
let addAge = (age) => {
    return ++age;
}
console.log(`年龄${age},增加一岁的年龄:${addAge(age)}`);

/* 对象  属性  简写 */
let ids = 1;
let pwd = '123456';
var obj = {
    ids,
    pwd,
    run(name) {
        console.log(`${name}在跑步`);

    }
}
console.log(`ids是${obj.ids}---密码是:${obj.pwd}`);
obj.run('zhangsan ')

/* 异步调用 */
//传统回调函数
let getpwdbyname = (name, callback) => {
    setTimeout(() => {
        console.log(`回调函数传入参数 ${name}`);
        var pwd = '这是密码';
        callback(pwd)
    }, 1000);
}
getpwdbyname('李斯', (pwd) => {
    console.log(`这是获取到的密码:${pwd}`);

})
//es6的promise
if (true) {
    let getpwd = (resovle, reject) => {
        setTimeout(() => {
            let name = 'zhangsan';
            if (name == '張三') {
                reject(`${name}这是异常信息`)
            } else {
                resovle(`${name}成功了`)
            }
        }, 1000);
    }

    let p = new Promise(getpwd);
    let p1 = new Promise(getpwd);


    let pAll = Promise.all([p,p1])

    pAll.catch((error) => {
        console.log(`捕獲到了異常信息${error}`);
    })

    pAll.then(result=>{
        const [data, moreData] = result;
        console.log(`${result}`);
    })


    /* p.then(
        (pwd) => {
            console.log(pwd);
        },
        (err) => {
            console.log(`${err}`);

        }
    ); */

}



```

## async / await / promise 使用递归调用目录中的文件

```js
/* 
async 将方法定义为异步
await 等待方法执行完成并获取其返回的promise
*/

var fs = require('fs');
var filedir = [];

let isDir = path => {
    return new Promise(
        (resolve, reject) => {
            fs.stat(path, (error, stats) => {
                if (error) {
                    console.log(error);
                    reject(error);
                }

                if (stats.isDirectory()) {
                    resolve(true);
                } else {
                    resolve(false);
                }
            })
        }
    )
}

const getfile =  (path) => {
    return new Promise((resolve, reject) => {
        fs.readdir(path, async (err, data) => {
            if (err) {
                console.log(`异常信息`);
                reject(err);
            } else {
                for (let index = 0; index < data.length; index++) {
                    //console.log(path+'/'+data[index]);
                    filedir.push(path+'/'+data[index]);
                    if (await isDir(path+'/'+data[index])) {
                        await getfile(path+'/'+data[index]);
                    }
                    
                }
                resolve(filedir)
            }
        })
    })
}

const main = async () => {
    console.log(`开始`);
    var data = await getfile('/home/ysh/gitwork')
    console.log(data);
    console.log(`结束`);

}

main();


```



