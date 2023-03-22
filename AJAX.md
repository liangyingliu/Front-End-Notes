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
//console.log(xhr.response)  不能这么读数据，因为xhr是异步的，不会等待上一句执行结束就会执行该句，而发完请求到响应需要一定的时间，响应速度没那么快，因此读不到数据。需确保响应信息回来之后才可以进行读操作 
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

### Fetch简介

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

