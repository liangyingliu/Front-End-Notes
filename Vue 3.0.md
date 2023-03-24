#  Vue 3.0

## Vue

- vue 是一个前端框架，主要负责帮助我们构建用户界面。
- vue 负责 vm 的工作（视图模型），通过 vue 可以将视图和模型相关联。
  - 当模型变化时，视图会自动更新。
  - 也可以通过视图去操作模型。
- 特点：
  - 组件化开发：组件化开发就是将一个臃肿的、单一的项目，根据功能/业务/技术等等进行拆分，形成一个个独立的功能组件，再将这些组件集合成一个完整的项目。
  - 声明式编程：声明式编程指的是只需描述目标的性质，让计算机明白目标，而非流程，具体代码由 vue 实现。声明式编程不用告诉计算机问题领域，从而避免随之而来的副作用。而命令式编程则需要用算法来明确的指出每一步该怎么做。

## HelloVue

Vue 在网页中的简单使用：

```html
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 引入 Vue -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>

<body>
    <div id="Root"></div>

    <script>
        //编写 Vue 代码
        //创建一个根组件（最外层组件），在 Vue3 中，组件就是一个普通的 js 对象
        //组件用来创建组件实例，组件是组件实例的模板
        //组件 --> 组件生成组件实例 --> 虚拟 DOM --> DOM（在页面中呈现）
        const Root = {
            data(){                                 //data 是一个函数，需要一个对象作为返回值
                return{
                    message:"Vue好棒！"             //data 方法返回的对象，其中的属性会自动添加到组件实例中
                }
            },    
            //在模板中可以直接访问组件实例中属性   
            //在模板中可以通过 {{属性名}} 来访问组件实例中的属性                      
            template: "<h1>Hello Vue3，{{message}}</h1>"     //我们希望组件在页面中呈现的样子
        }

        

        //创建 app 实例
        const app = Vue.createApp(Root)         //创建一个 app 实例，其根组件为上面代码所定义的 Root

        //将实例在页面中挂载（调用实例方法）
        app.mount("#Root")      //将 app 实例挂载在 id 为 Root 的 div 中

        //上述的代码可写成如下一行代码
        Vue.createApp(Root).mount("#Root")
    </script>
</body>

</html>
```

为元素绑定事件：

```js
const Root = {
	data() {                     //data 是一个函数，需要一个对象作为返回值
		return {
			count: 0             //data 方法返回的对象，其中的属性会自动添加到组件实例中
		}
	},
//在模板中可以直接访问组件实例中属性   
//在模板中可以通过 {{属性名}} 来访问组件实例中的属性                      
template: "<button @click='count++'>点我一下</button>-已点{{count}}次"     //我们希望组件在页面中呈现的样子,@click 为其绑定点击事件
}
```

