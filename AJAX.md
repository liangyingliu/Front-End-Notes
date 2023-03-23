# AJAX

## 方案

- XMLHTTPRuequest
- Fetch
- Axios

## XMLHTTPRequest

### 示例

```javascript
 //创建一个新的xhr对象，xhr表示请求信息
const xhr =new XMLHttpRequest()

//xhr.responseType="json"    //直接表示拿到的xhr.response数据为Json格式，再调用console.log时会直接将其格式转换为JS对象

//可以为xhr对象绑定一个load事件
xhr.onload=function(){
    //xhr.status表示响应状态码
    if(xhr.status===200){          //只有页面正常被访问才会响应数据
        const stuList=JSON.parse(xhr.response)    //xhr.response返回的数据为字符串，无法直接读取数据，需将其转换为JS对象    
        console.log(stuList)       //数据加载完毕之后再打印响应信息，这样才能拿到数据
    }  

}
//设置请求的信息
xhr.open("get","http://localhost:3000/students")

//发送请求
xhr.send() 

//读取响应信息
//console.log(xhr.response)  不能这么读数据，因为xhr是异步的，不会等待上一句执行结束就会执行该句，而发完请求到响应需要一定的时间，响应速度没那么快，因此							读不到数据。需确保响应信息回来之后才可以进行读操作 
```

### 问题

#### CORS（跨域资源共享）

- 跨域请求
  - 两个网站的完整的域名不相同
    - a网站：http://haha.com
    - b网站：http://heihei.com
  - 跨域需要检查三个东西：协议、域名、端口号。只要有个一不同，就算跨域。
  - 当通过AJAX去发送跨域请求时，**浏览器**为了服务器的安全，会阻止 JS 读取到服务器的数据。

- 解决方案

  - 在服务器中设置一个允许跨域的头，告诉浏览器不需要保护。

    - Access-Control-Allow-Origin：允许那些客户端访问我们的服务器。（浏览器会有提示）

      ```javascript
      res.setHeader("Access-Control-Allow-Origin","*")   //*号表示全部的请求都允许
      res.setHeader("Access-Control-Allow-Origin","http://127.0.0.1:5500")  //表示只允许该路径的请求，其他的请求还是不会允许
      ```

      Access-Control-Allow-Origin 只能设置一个允许值，如果需要设置多个，需将被允许的地址放入一个数组，请求进来时去数组中检查是否有符合条件的路径，以此来实现对多个地址的请求许可。如：

      ```javascript
      var origins = ['a.com', 'b.com', 'c.com', 'boobies.com'];
      for(var i=0;i<origins.length;i++){
          var origin = origins[i];
          if(req.headers.origin.indexOf(origin) > -1){ 
               res.setHeader('Access-Control-Allow-Origin', req.headers.origin);
               return;
          }
          //else...
      }
      ```

    - Access-Control-Allow-Methods：允许的请求方式

      ```javascript
      res.setHeader("Access-Control-Allow-Methods","GET,POSt")  //允许的请求方式
      ```

    - Access-Control-Allow-Headers：允许传递的请求头

      ```javascript
      res.setHeader("Access-Control-Allow-Hesders","Content-type") 
      ```

  - 更多参考资料：[Cross-Origin Resource Sharing (CORS) - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## Fetch

### Fetch 简介

- fetch 是 xhr 的升级版，采用的是 Promise API。
- 作用和 AJAX 是一样的，但使用起来更加友好。
- fetch 是原生 JS 就支持的一种 AJAX 请求的方式。

### 示例

```javascript
fetch("http://localhost:3000/students")	//默认发get请求，fetch可以传第二个参数，该参数为配置对象，更多可参考：https://developer.mozilla.org/en-US/docs/Web/API/fetch 注意：通过body去发送数据时，必须通过请求头来指定数据的类型
fetch("http://localhost:3000/students",{
    method:"post"
    headers:{
    	"Content-type":"application/json"
    	"authorization":'Bearer ${token}'					//请求头向服务器发送token,需在前面加上Bearer。设置请求头时需要在服务器中设置允许																的请求头，不然会产生跨域问题。
	},
    body:JSON.stringify({
      name:"老王",
      age:28,
      gender:"男",
      address:"隔壁"
      })
})
    .then(res=>{
    if(res.status===200){
        //res.json 可以用来读取json格式的数据
        //console.log(res.json())
        return res.json()
    }else{
        throw new Error("加载失败！");  //状态码不对时抛出错误，由最后的catch解决。
    }
})
    .then((res)=>{
    console.log(res)
})
    .catch(err=>{
    console.log("出错了",err)
})
```

### sessionStorage 和 localStorage

- sessionStorgae 是会话存储。它的有效期是页面会话持续，如果页面会话结束（关闭会话窗口或标签页），sessionStorage 就会销毁。

- localStorage 是本地存储。它的生命周期是永久的，除非被清除，不然会永久保存。

- 使用：

  ```js
  sessionStorgae.setItem()   		//用来存储数据
  sessionStorgae.getItem()		//用来读取数据
  sessionStorgae.removeItem()		//删除数据
  sessionStorgae.clear()			//清空数据
  //localStorage 的用法和 sessionStorgae 一样
  ```

### token

在我们写项目时，经常有需要用户登录的情况，而如何告诉服务器客户端当前的登录状态是很重要的问题。rest 风格的服务器是无状态的服务器，所以注意不要再服务器中存储用户的数据，我们可以将用户信息发送给客户端保存。客户端每次访问服务器时，直接将用户信息返回，这样服务器就可以根据客户端带回来的用户信息来识别用户的身份。但将数据直接发送给客户端同样会有数据安全问题，所以我们需对数据进行加密，加密以后再发送给客户端保存，这样即可避免数据的泄露。

在 node 中可以直接使用 jsonwebtoken 这个包来对数据进行加密。[详细说明可看：jsonwebtoken - npm search (npmjs.com)](https://www.npmjs.com/search?q=jsonwebtoken&csrftoken=N7P3NzzFM9K93bIkd6y0uVJBdHENuta3WGFFjDpqLDL)

jsonwebtoken (jwt)：通过对 json 加密后，生成一个 web 使用的令牌。使用方法如下：

```js
1.安装
yarn add jsonwebtoken
2.引入
const jwt = require("jsonwebtoken")

//假设对如下对象进行加密
const obj={
      name:"老王",
      age:28,
      gender:"男",
      address:"隔壁"
}
//使用jwt对json数据加密
const token = jwt.sign(obj,"hh",{
    expiresIn:"1h"		//该配置对象表示加密有效时间为1小时
})						//sign方法的三个参数分别为（要加密的内容，密钥，配置对象）

//服务器收到token后需对其解密
const decodeData = jwt.verify(token,"hh")		//使用方法veritf对加密的内容进行解密，verify的参数为（解密的内容，密钥（该密钥需要和前面加密的密													钥相同）），需注意：应该在加密有效时间内解密才有效。

//可加上对token有效与否的判断：
try{
    //token有效
	const decodeData = jwt.verify(token,"hh")
    console.log (decodeData)
}catch(e){
    //token无效，解码失败
    console.log ("无效的token")
}
```

console.log (token) 执行结果：

![image-20230323130010499](https://typora-liang.oss-cn-shenzhen.aliyuncs.com/image-20230323130010499.png)

console.log (decodeData) 执行结果：

![image-20230323130200082](https://typora-liang.oss-cn-shenzhen.aliyuncs.com/image-20230323130200082.png)

### AbortController

由于 AJAX 是异步的，当客户端向服务器发送请求但未能得到响应时，并不会影响下一次向服务器发送请求，这是我们不希望看到的，我们希望向服务器发送的请求在长时间未能得到处理后会自动取消，这样就不会影响到下一次的请求。AbortController 能帮助我们实现这个功能。

```js
const controller = new AbortController()	//创建一个AbortController
setTimeout(()=>{
    controller.abort()		//设置一个定时器，3秒以后执行abort操作
},3000)
frtch("http://localhost:3000/students",{
    signal:controller.signal					//在请求中设置信号为controller的信号，有controller控制请求的取消与否，在此代码中在发送请求后的				           							3秒后会自动取消。也可以根据需要将取消请求设置为手动取消。
})
```

## Axios

[详细文档 (axios-http.cn)](https://www.axios-http.cn/docs/intro)

```js
//安装：
npm	install axios

//发送一个"post"请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});

// 发起一个 GET 请求 (默认请求方式)
axios('/user/12345'); 
```

### 请求配置

```js

axios({
    baseurl:"http://localhost:3000",				//使用baseurl（指定服务器根目录）时会将baseurl和url的地址合并起来再发送请求
    url:"12345",									//请求地址,这是唯一的必须项。
    method:"post",									//请求方法，默认时get
	headers:{"Content-type":"application/json"},	//指定请求头（会自动设置）
	data：{},									   //请求体，axios会将数据自动转换成json格式
    transformRequest:[function(data,headers){},function(data,headers){},],	//transformRequest可以用来处理请求数据（data），它需要一个数组作为参数，数组可以接受多个函数，请求发送时多个函数会参照顺序执行。函数在执行时会接受两个参数，分别时data和headers。我们可以在函数中对data和headers进行修改等操作。最后一个函数必须返回一个字符串，才能使数据有效。
    params:{},										//指定查询字符串，相当于在请求地址后面加上(？...&...)
	timeout:{}										//表示过期时间，若长时间没有响应返回，则在此时间后将请求取消掉。
})
```

### Axios 实例

axios 实例相当于时 axios 的一个副本，它的功能和 axios 一样，axios 的默认配置在实例中同样会生效，但 axios 实例中的配置参数可以单独修改。所以当我们要设置请求配置中的其中一些跟默认配置不一样的参数时，可以使用 axios 实例来实现，只对当前的 axios 实例有效。

```js
const instance = axios.create({
    baseurl:"http://localhost:5000"
})
```

### Axios 拦截器

axios 拦截器可以对请求或者响应进行拦截，在请求发送前和响应读取前处理数据。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么									//config表示axios里面的配置对象
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });xxxxxxxxxx axios int// 添加请求拦截器axios.interceptors.request.use(function (config) {    // 在发送请求之前做些什么    return config;  }, function (error) {    // 对请求错误做些什么    return Promise.reject(error);  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

移除拦截器

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

