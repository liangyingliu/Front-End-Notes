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

## vue 组件化

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

## 代码分析

main.js ：

```js
import { createApp } from 'vue'
import App from './App.vue'


/* 
    App.vue是根组件
        - createApp(App) 将根组件关联到应用上
            - 会返回一个应用的实例
        - app.mount("#app") 将应用挂载到页面中
            - 会返回一个根组件的实例，组件的实例通常可以命名为vm
            - 组件实例是一个Proxy对象（代理对象）
*/


const app = createApp(App)

const vm = app.mount("#app")

// 将vm设置为全局变量
window.vm = vm
window.app = app

// createApp(App).mount('#app')

// console.log(vm)
```

App.vue：

```vue
<script>
export default {
    //data是一个函数（方法）
    //在data中，this就是当前的组件实例（vm），如果使用箭头函数，则无法通过this来访问组件实例
    //使用vue时，尽量减少使用箭头函数
  data() {
      
        // 直接向组件实例中添加的属性不会被vue所代理，不是响应数据，修改后页面不会发生变化
        this.name = "孙悟空"

    
        //data会返回一个对象，vue会对该对象进行代理，从而将其转换为响应式数据（网页随数据变而变）
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

## 响应式原理

### 代理

我们创建一个对象，如果直接修改对象的属性，则仅仅是修改了属性，没有做其他事情，这种操作只会影响对象自身，并不会导致元素的重新渲染。而我们希望在修改一个属性的同时，可以进行一些其他的操作，比如触发元素重新渲染等。要实现这个目的，我们就必须对对象进行改造，Vue 3 中使用的是代理模式来完成对象的改造。

```js
//创建一个对象
const obj = {
    name: "Vue",
    age: 4,
}
/*
    如果直接修改对象的属性，则仅仅是修改了属性，没有做其他事情，这种操作只会影响对象自身，不会导致元素的重新渲染。

*/

//为对象创建一个代理
const handler = {
    //get用来指定读取数据时的行为，它的返回值就是最终读取到的值
    //指定get后，再通过代理读取对象数据时，就会调用get方法来获取值
    get(target, prop, receiver) {
        /*
            三个参数：
                - target：被代理的对象。
                - prop：读取的属性。
                - receiver：代理对象。
        
        */
            //可在返回值之前做其他操作...
            return target[prop]
    },
    
    //set会在通过修改代理时调用
    Set(target,prop,value,receiver) {
        target[prop] = value;
        
        //可在修改值之后做其他操作...
    }

    
}      //handler用来指定代理的行为

//创建代理
const proxy = new Proxy(obj, handler)

//console.log(proxy)

//console.log(proxy.name)

//修改代理的属性可以间接的修改对象的属性(调用handler里的set方法)
proxy.age = 8;

//读取属性的行为实则是在调用handler里的get方法
console.log(proxy.age);
```

在 vue 中，data () 返回的对象会被vue所代理，vue代理后，当我们通过代理去读取属性时，返回值之前，它会先做一个跟踪的操作`track() 追踪谁用了我这个属性`， 当我们通过代理去修改属性时，修改后，会通知之前所有用到该值的位置进行更新`trigger() 触发所有的使用该值的位置进行更新`。

### 代理对象

#### data

`vm.$data` 是实际的代理对象，通过 vm 可以直接访问到 $data 中的属性。

`vm.$data.msg ` 等价于 `vm.msg`

vue 在构建响应式对象时，会同时将该对象中的属性也设置成响应式属性，即我们也可以响应式的修改响应式对象中的属性。该对象也叫深层响应式对象。

在有些场景下，我们不希望用户改掉对象里面的深层属性，这时我们可以通过 `shallowReactive()` 来创建一个浅层的响应式对象。、

```js
import { shallowReactive } from "vue"
export default{
	data({
            return shallowReactive({
       			 msg: "Hello Vue!",
       			 stu: {
           				name: "小王",
            			age: 18,
            			gender: "男",
            			friend: {
                			name: "老王"
           				 }
        			}
    			})
	})
}					//通过使用shallowReactive()，这个对象就只能响应式地修改第一层属性，即mag和stu，但不能单独的修改stu里面的属性
```

如果我们有暂时不需要使用，但后面有可能使用的属性，我们也添加到 data 返回的对象中，将其值设置为 null ，这样后期修改属性就会很方便。

#### methods

data 用来指定实例对象中的响应式属性，而 methods 是用来指定实例对象中的方法。

-  它是一个对象，可以在它里边定义多个方法。
- 这些方法最终都会被挂载到组件实例上。
- 可以直接通过组件实例来调用这些方法。

- 所有组件实例上的属性都可以直接在模板中访问。
- methods 中函数的 this 会被自动绑定为组件实例。

## 计算属性

computed 用来指定计算属性。当我们访问计算属性是，会将其中的 return 的内容显示出来。因为这是通过函数来返回结果，所有可以通过编写逻辑来返回比较复杂的内容。计算属性只在其依赖的数据发送改变时才会重新执行（会对数据进行缓存），而 methods 中的方法每次组件重新渲染都会调用，且 methods 调用时需话括号（( )）。computed 不需要。

```js
comuted:{
    info:function(){
        return:"哈哈"
    }
}
```

在计算属性的 getter 中，尽量只做读取相关的逻辑，不要执行那些会产生（副）作用的代码。可以为计算书属性设置 setter ，使得计算属性可写，但是不建议这么做。

```js
data(){
    return{
        firstName:"王"
        lastName："老"
    }
},
name:{
    get(){
        return this.lastName + this.firstName;
    },
    set(value){							//set在计算属性被修改时调用
        this.lastName = value[0];
        this.firstName = value.slice(1);
    }
}
```

## 安装调试工具

浏览器中安装拓展插件 `Vue.js devtools`

## 组合式 API

之前的内容都是选项式 API ，但我们更常用的是组合式 API 。组合式 API 使用的是函数，我们可以在里面定义变量等内容，而选项式 API 使用的对象，里面不能定义变量之类的内容。

```vue
<script>
import { reactive } from "vue"
export default {
    setup() {
        // 定义变量
        // 在组合式api中直接声明的变量，就是一个普通的变量，不是响应式属性
        //      修改这些属性时，不会在视图中产生效果
        let msg = "今天天气真不错!"
        let count = 0
        //可以通过 reactive()来创建一个响应式的对象
        const stu = reactive({
            name: "孙悟空",
            age: 18,
            gender: "男"
        })
        function changeAge(){
            stu.age = 44
        }
        // 在setup()中可以通过返回值来指定那些内容要暴露给外部
        // 暴露后的内容，可以在模板中直接使用
        return {
            msg,
            count,
            stu,
            changeAge
        }
    }
}
</script>
<template>
    <h1>演示组合式API</h1>
    <h2>{{ msg }}</h2>
    <h3>{{ count }}</h3>
    <button @click="changeAge">点我一下</button>
    <h2>{{ stu.name }} -- {{ stu.age }} -- {{ stu.gender }}</h2>
</template>
```

 下面这种写法更方便：

```vue
<script setup>
import { reactive } from "vue"
const msg = "我爱Vue"
const count = 0
const stu = reactive({
    name: "孙悟空"
})
function fn() {
    alert("哈哈哈，好快乐！")
}
</script>
<template>
    <h1 @click="fn">组合式的API</h1>
    <h2>{{ msg }} -- {{ count }}</h2>
    <h3>{{ stu.name }}</h3>
</template>
```

## reactive ( ) 与 ref ( )

reactive ( ) 返回一个对象的响应式代理，返回的是个深层响应式对象。我们可以使用 shallowactive ( ) 创建一个浅层响应式代理。但有个缺点：只能返回对象的响应式代理（只能是对象），不能处理原始值。 而 ref ( ) 与 reactive ( ) 用法基本一样，但其可以接受任意值，并返回它的响应式代理。ref ( ) 在生成响应式代理时，是将值包装成了一个对象，将传入的值放在 value 属性里面。如`0 --> {value:0}`、`{name:"老王",age:28} --> {value:{name:"老王",age:28}} `。但在模板中， ref ( ) 对象会自动解包，不用再写 value 。

```vue
<script setup>
import { reactive, ref } from "vue"
import { $ref } from "vue/macros"
/* 
    reactive()
        - 返回一个对象的响应式代理
        - 返回的是一个深层响应式对象
        - 也可以使用shallowReactive()创建一个浅层响应式对象
        - 缺点：
            - 只能返回对象的响应式代理！不能处理原始值
    ref()
        - 接收一个任意值，并返回它的响应式代理
*/
const stu = reactive({
    name: "老王"
})
// ref在生成响应式代理时，它是将值包装为了一个对象 0  --> {value:0}
// 访问ref对象时，必须通过 对象.value 来访问其中的值
// 在模板中，ref对象会被自动解包
let count = $ref(0) // 生成一个0的响应式代理
// count = 10 // 改变量只会影响到变量自己，在js中，无法实现对一个变量的代理
console.log(count)
// vue给我们提供了一个语法糖，使得ref对象在script标签中也可以自动解包
// $是一个实验性的，需要在vite插件中做一些配置 reactivityTransform:true
function fn() {
    // count自增
    count++
}
</script>
<template>
    <h1>组合式的API</h1>
    <h2 @click="fn">{{ count }}</h2>
</template>
```

## 模板

模板，即`<template> </template>` 。

- 在模板中，可以直接访问到组件中声明的变量。
- 除了组件中的变量外，vue也为我们提供了一些全局对象可以访问：比如：Date、Math、RegExp ...等，除此之外，也可以通过app对象来向vue中添加一些全局变量 `app.config.globalProperties`。
- 使用插值(双大括号)，只能使用表达式，即有返回值的语句。
- 插值实际上就是在修改元素的 `textContent`，如果内容中含有标签，标签会被转义显示，不会作为标签生效。

### 指令

我们要使内容中的标签生效，可以使用指令。

- 指令模板中为标签设置的一些特殊属性，它可以用来设置标签如何显示内容。
- 指令使用 `v-` 开头。
  - `v-text` 将表达式的值作为元素的 `textContent` 插入，作用同 `{{ }}`。
  - `v-html` 将表达式的值作为元素的 `innerHTML` 插入，有 `xss` 攻击的风险。
  - `v-bind` 当我们需要为标签动态的设置属性时使用，可以简写为 `:` 。注意，如果为一个布尔值设置属性时，若该值为：`""`，即空串时，在这里会被当成真值。
- 使用指令时，不需要通过 `{{ }}` 来指定表达式。

## Style

在组件中，我们可以直接通过 `style` 标签来编写样式，如果直接通过 ``style 标签写样式，此时编写的样式是全局样式， 会影响到所有的组件。

```vue
<style>
h1 {
    background-color: #bfa;
}
.box1 {
    width: 200px;
    height: 200px;
    background-color: yellowgreen;
}
</style>
```

### Style-scoped

若我们需要为单独的组件设置样式，可以为 `style` 标签添加一个 `scoped` 属性，这样样式将成为局部样式，只对当前组件生效 。

```vue
<style scoped>
h1 {
    background-color: orange;
}
.box1 {
    width: 200px;
    height: 200px;
    background-color: #bfa;
}
```

实现原理：

当我们在组件中使用 `scoped` 样式时，vue 会自动为组件中的所有元素生成一个随机的属性，形如：`data-v-7a7a37b1`， 生成后，所有的选择器都会在最后添加一个 ` [data-v-7a7a37b1]` 如：` [data-v-7a7a37b1]` 、`.box1 -> .box1[data-v-7a7a37b1]`。

**注意： 随机生成的属性，除了会添加到当前组件内的所有元素上，也会添加到当前组件引入的其他组件的根元素上，这样设计是为了，可以通过父组件来为子组件设置一些样式。**

如果有多个根元素，则不会设置在根元素上。若我们要设置的元素不是根元素，我们仍希望能设置其样式，可以使用 `deep()` 来实现。

```vue
<style sscoped>
/* 
将组件中所有的 h2的字体颜色设置为黄色 
.app h2 --> .app h2[xxxxx]
.app h2[data-v-7a7a37b1] 没用deep
.app[data-v-7a7a37b1] h2 用了deep
*/
.app :deep(h2) {
    color: yellow;
}
</style>
```

`:global()` 全局选择器：

```vue
<style sscoped>
:global(div) {
    border: 1px red solid;
}
</style>
```

### Style-module

```vue
<script setup>
import MyBox from "./components/MyBox.vue"
</script>

<template>
    <div class="app">
        <h1>今天天气真不错！</h1>
        <div :class="classes.box1">App中的box1</div>
        <MyBox></MyBox>
    </div>
</template>
<!-- 
  css 模块
    - 自动的对模块中的类名进行hash化来确保类名的唯一性
    - 在模板中可以通过 $style.类名 使用
    - 也可以通过module的属性值来指定变量名
-->
<!--
    <style module="classes">
    .box1 {
        background-color: #bfa;
    }
-->
<style module>
.box1 {
    background-color: #bfa;
}
</style>
```

## 类和内联样式

```vue
<script setup>
const arr = ["box1", "box2", "box3"]
const arr2 = [{ box1: true }, { box2: false }, { box3: true }]
const style = {
    color: "red",
    backgroundColor: "#bfa"
}
</script>
<template>
    <h1 class="header">我爱Vue</h1>
    <div :class="arr2" :style="style">我是div</div>
</template>
<style scoped>
.header {
    background-color: orange;
}
</style>
```

