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
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>	//在网页中引入vue
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

template 是模板，它决定了组件的最终样子。定义模板的方式有三种：

- 在组件中通过 template 属性去指定

  ```js
  const Root = {
  	data() {                     //data 是一个函数，需要一个对象作为返回值
  		return {
  			count: 0             //data 方法返回的对象，其中的属性会自动添加到组件实例中
  		}
  	},
  		//在模板中可以直接访问组件实例中属性   
  		//在模板中可以通过 {{属性名}} 来访问组件实例中的属性                      
  		template: "<button @click='count++'>点我一下</button>-已点{{count}}次"  //我们希望组件在页面中呈现的样子,@click 为其绑定点击事件
  }
  ```

- 直接在网页的根元素中指定（基本不会使用这种方式 ）

  ```js
  <div id="Root">
      <button @click='count++'>点我一下</button>-已点{{count}}次
  </div>
  ```

  注意：

  - 如果直接将模板定义到网页中，此时模板必须符合 html 的规范。 
  - 如果组件中定义了 template ，则会优先使用 template 作为模板，同时跟元素中的所有内容，都会被替换。
  - 如果组件中没有定义 template ，则会使用根元素的 innerHTML 作为模板。

- 组件中通过 render() 直接渲染。（只在特殊场景使用，一般不使用）

## 使用 Vite

1. 安装 vite ：`yarn add vite -D`
2. 安装 vue ：`yarn add vue`

3. 创建 html 页面：

   ```html
   <!DOCTYPE html>
   <html lang="zh">
   
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Hello vue</title>
       <script type="module" src="./src/index.js"></script>
   </head>
   
   <body>
       <div id="app"></div>
   </body>
   
   </html>
   ```

4. 创建 js 文件并使用 vue：

   ```js
   //引入Vue
   //这里引入的Vue默认不支持通过template属性来设置模板
   import { createApp } from "vue/dist/vue.esm-bundler.js"		//这里直接使用vue的话会显示不出来，因为网页的vue与js里的vue版本不同
   
   //创建一个根组件
   const App = {
       data() {
           return {
               message: "Vue!"
           }
       },
       template: "<h1>{{message}}</h1>"
   }
   
   //创建应用挂载到页面
   createApp(App).mount("#app")
   ```

## vue组件化

我们可以根据需要来自定义所想要的组件，如：

```js
// 封装一个自定义的按钮组件
export default {
    data() {
        return {
            count:0
        }
    },
    template: `
        <div>
            <h2>子组件</h2>
            <button @click='count++'> 点我一下</button> <span>{{ count }}</span>
        </div>
    `
}
```

然后在应用此组件的地方先引用组件：

```js
//引入组件
import Mybutton from "./conponents/Mybutton"
//创建一个根组件
export default {
    data() {
        return {
            message: "Vue!"
        }
    },

    //在组件中注册子组件
    components: {
        //若是该组件直接在网页HTML中使用，则名字应该符合HTML的规范，即需改成小写
        Mybutton:Mybutton		//自定义名：自定义组件的名字（即上面引入时的名字）
    },
    template: `
        <h1> {{ message }}</h1> 
        <Mybutton></Mybutton>
  	 `
}
```

## 单文件组件（SFC）

上面所写的代码中的 template 都是用字符串的形式在编写模板，会有以下缺点：

- 这些字符串会在项目运行时，在浏览器中被编译为 js 的函数，性能不太好。
- 在字符串中编写代码，体验很差。

为了解决以上问题，Vue 为我们提供了一种单文件组件。

- 单文件组件的格式为 vue（在 VScode 需要安装插件 Vue Language Features (Volar)）。
- vue 文件用来编写单文件组件，vue 文件本身并不能被浏览器所识别，所以它必须要被构建工具打包后，才可使用。
- 同时 vue 文件在打包时，构建工具会直接将 template 转换成函数，无需在浏览器中去编译，这样一来性能会有所提升。 

使用示例：

1. 新建 vue 文件（这里定义为 App.vue）

   ```vue
   <script>
       // 编写组件的代码
       export default {
           data() {
                   return {
                       message:"Vue!"
                   }
               }
           }
   </script>
   
   <template>
       <h1>{{ message }}</h1>
   </template>
   ```

2. 引入组件

   ```js
   import App from "./conponents/App.vue"
   ```

3. 安装开发依赖：`yarn add -D  @vitejs/plugin-vue`

4. 创建 vite 配置文件：`vite.config.js`

   ```js
   import vue from "@vitejs/plugin-vue"
   export default {
       plugins:[vue()]
   }
   ```

   这里配置完后前面使用的 `import { createApp } from "vue/dist/vue.esm-bundler.js"`就可以直接引入vue了：`import { createApp } from "vue"`

## 自动创建项目

1. 执行代码：`yarn create vue`

2. 安装依赖：`yarn`
