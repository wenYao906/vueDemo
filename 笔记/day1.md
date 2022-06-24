#  第一天

## 一、要求

1.1：每一天老师书写代码务必三遍
1.2:node + webpack + VScode + 谷歌浏览器 + git
1.3:数组的方法 + promise + await + async + 模块化........



## 二、脚手架使用

2: vue init webpack 项目的名字
3|4：vue create 项目名称
脚手架目录:public + assets文件夹区别
node_modules:放置项目依赖的地方
public:一般放置一些共用的静态资源，打包上线的时候，public文件夹里面资源原封不动打包到dist文件夹里面
src：程序员源代码文件夹
  -----assets文件夹：经常放置一些静态资源（图片），assets文件夹里面资源webpack会进行打包为一个模块（js文件夹里面）
  -----components文件夹:一般放置非路由组件（或者项目共用的组件）
        App.vue 唯一的根组件
        main.js 入口文件【程序最先执行的文件】
        babel.config.js:babel配置文件
        package.json：看到项目描述、项目依赖、项目运行指令
        README.md:项目说明文件



## 三、脚手架下载下来的项目稍微配置一下

3.1:浏览器自动打开
        在package.json文件中

      ```vue
      "scripts": {
         "serve": "vue-cli-service serve --open",
          "build": "vue-cli-service build",
          "lint": "vue-cli-service lint"
      },
      ```

3.2关闭eslint校验工具
创建vue.config.js文件：需要对外暴露

```vue
module.exports = {
   lintOnSave:false,
}
```



3.3 src文件夹的别名的设置
因为项目大的时候src（源代码文件夹）：里面目录会很多，找文件不方便，设置src文件夹的别名的好处，找文件会方便一些
创建jsconfig.json文件

```vue
 {
    "compilerOptions": {
        "baseUrl": "./",
        "paths": {
            "@/*": [
                "src/*"
            ]
        }
    },
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```







##  四、创建非路由组件

######  （2个：Header、Footer）

* 在这个项目中，不以CSS+HTML 为主，直接使用辉宏老师的代码。主要搞业务和逻辑

在开发项目的时候：
1. 书写静态页面（HTML+CSS）
2. 拆分组件
3. 获取服务器的数据动态展示
4. 完成相应的动态业务逻辑

注意1： 创建组件的时候， 组件结构 + 组件的样式 + 图片资源

注意2：这个项目采用的less样式,浏览器不识别less语法（样式），需要通过less、less-loader[安装第五版本的]进行处理less，把less语法转换为CSS语法，浏览器才可以识别。
1：安装less less-loader@5
切记less-loader安装5版本的，不要安装在最新版本，安装最新版本less-loader会报错，报的错误setOption函数未定义
安装命令为： cnpm install --save less less-loader@5  (mac 报错 改成 npm install less less-loader --save-dev 成功)
安装完成后，重启服务 ：npm run serve

注意3: 如果想让组件识别less样式，需要在style标签的身上加上lang="less",不添加样式不生效
<style scoped lang="less">
4.1 使用组件的步骤（非路由组件）:
第一步：创建或者定义
第二步：引入
第三步：注册
第四步:使用

4.2 代码放在了 src - components - footer\header 文件中
4.3 引入的代码、注册的代码
src - components - app.vue 文件中的 <script> 中写 

```vue
// 引入的代码
import Header from './components/Header'
export default {
    name:'',
    components:{ // 注册的代码
        Header
    }
}
```

4.4 使用的代码
src - components - app.vue 文件中的 <template> 中写

```vue
<div>
    <Header/>
</div>
```



## 五、去掉 Header 中的默认样式

复制 静态页面 - 辉洪 - css - reset.css
放到自己项目中 public 文件夹中
在 public - index.html 中写上 

```vue
 <!-- 引入清除默认的样式 -->
<link rel="stylesheet" href="./reset.css">
```



5. Footer 同 header 一样

注意1：在终端暂停 vue的服务，可以使用 ctrl+c 





## 六、路由的配置

vue-router

本项目的路由组件应该有四个：Home、Search、Login、Register

使用以下命令安装路由

```vue
// MAC 不可用
cnpm install --save vue-router 
// MAC  可用
npm install vue-router@3.5.2 --save
 
--save:可以让你安装的依赖，在package.json文件当中进行记录

```

-components 文件夹：经常放置的是非路由组件（共用的全局组件）

-pages|views 文件夹：经常放置路由组件

* 在src 文件夹中创建 pages文件夹，在这个文件夹中创建4个文件夹（Home、Search、Login、Register），每个文件夹有一个index.vue



### 6 .1 配置路由，配置完四个路由组件

* 在 src 文件夹中创建 router文件夹，在router文件夹中创建一个 index.js文件

* 在index.js 文件中写上

* ```vue
  // 配置路由的地方
  import Vue from 'vue';
  import VueRouter from 'vue-router';
  // 使用插件
  Vue.use(VueRouter);
  // 引入路由组件(只举例1个，剩下3个也需要写上)
  import Home from '@/pages/Home'
  // 配置路由
  export default new VueRouter({
  	 // 配置路由 
    routes:[
  		{
  			path::"/home",
  			component:Home
      }
    ]
  })
  ```



### 6.2 引入路由

* main.js 文件中写

```vue
// 引入路由
import router from '@/router';

new Vue({
  render:h=> h(App),
  // 注册路由,这个是key,value 一致，所以可以省略 value。
	// router 的 首字母一定要小写，不要大写
	// 注册路由信息：当这里书写 router的时候，组件身上都拥有$route，$router属性
  router
})
```



### 6.3 路由组件出口的地方

在App.vue 中的`<template>`中写上`<router-view>`



### 6.4 总结

#### 6.4.1 路由组件与非路由组件的区别？

1. 路由组件一般放置在 pages|views文件夹，非路由组件一般放置在 components 文件夹中。
2. 路由组件一般需要在router文件夹中进行注册（使用的即为组件的名字），非路由组件在使用的时候，一般都是以标签的形式使用。
3. 注册完路由，不管是路由组件、还是非路由组件身上都有$route\$router属性



#### 6.4.2  $route 和 ￥router的含义

* $route: 一般获取路由信息【比如：路径、query、params等等】
* $router: 一般进行编程式导航进行路由跳转的【如：push 和 replace】



#### 6.4.3 $router中的 push 和 replace 的区别是什么？

?????



### 6.5 重定向

当你访问一个项目地址，点击了这个项目中的很多页面后，在地址栏中修改这个地址时，让他默认跳转到一个页面。

在scr - router - index.js 中写上 

```vue
export default new VueRouter ({
	routes:[
    ...
    // 重定向，在项目跑起来的时候，访问/时，立马让他定向到首页
    {
      path:'*',
      redirect: “/home”
    }
	],
})
```

 

### 6.6 路由的跳转

路由的跳转有两种形式：

* 声明式导航router-link, 可以进行路由的跳转
* 编程式导航 push|replace, 可以进行路由跳转

这两个的区别：

* 编程式导航:声明式导航能做的，编程式导航都能干，但编程式导航除了可以进行路由跳转外，还可以做一些其他的业务逻辑。

* 声明式导航：务必要有to属性



### 6.7 修改登录、注册、首页的logo图标的跳转

在src - components - Header - index.vue 中

```vue
<template>
	<header class="header">
		<router-link to="/login"> 登录 </router-link>
    <router-link class="register" to="/register">免费注册</router-link>
    <router-linke class="logo" to="/home"> logo </router-linke>
    <button class="sui-btn btn-xlarge btn-danger" type="button" @class="goSearch">
      搜索
  </button>
	</header>
</template>


<script>
  export default {
    name:"",
    methods:{
      // 搜索按钮的回调函数，需要向search路由进行跳转
      goSearch(){
        this.$router.push('/search')
      }
    }
  }
</script>
```



## 七、Footer 组件显示与隐藏

### 7.1 前言

Footer 组件：在Home、Search 中显示Footer 组件

Footer 组件：在登录、注册的时候隐藏

#### 7.1.1 显示或者隐藏组件

可以使用 `v-if` 或者是 `v-show`

`v-if` 是操作DOM在节点是真有，还是真没有。它会频繁的操作DOM,损耗性能

`v-show` 通过样式让元素进行显示或者隐藏。

从性能出发，`v-show` 更好一些



####7.1.2 使用`v-show`显示隐藏Footer

##### 第一种方法：

在 src -> App.vue -> <template> 中写:

```vue
<Footer v-show="$route.path='/home' || $route.path=='/search'">
</Footer>
```

注意1：我们可以根据组件身上的$route获取当前路由的信息，通过路由路径判断Footer显示与隐藏

##### 第二种写法：

利用路由元信息 meta

在vue官网 -> Vue Router -> 进阶 -> 路由元信息

src -> router -> index.js  中写

```vue
export default new VueRouter({
	routes:[
		{
      path:"/home",
      component:Home,
      meta:{
        show:true
      },

			...
			...
    }
  ]
})
```

src -> App.vue -> template 中改成 

```vue
<template>
	<Footer v-show="$route.meta.show">
</template>
```

配置路由的时候，可以给路由添加路由元信息【meta】，路由需要配置对象，它的key不能瞎写、胡写、乱写



## 八、路由传参

### 8.1 路由的跳转有几种方式？

比如：A -> B 

* 声明式导航：router-link （务必要有to属性），可以实现路由的跳转
* 编程式导航：利用的是组件实例的$router.push|replace方法，可以实现路由的跳转。（编程式导航比声明式导航好一些，因为在路由跳转之前，可以写一些自己的业务，然后在进行跳转）

### 8.2 路由传参，参数有几种写法？

* params参数：属于路径当中的一部分，需要注意，在配置路由的时候，需要占位。
* query参数：不属于路径当中的一部分，类似于ajax中的queryString。
  * 例如：/home?k=v&kv=, 它不需要占位



8.3 代码

src -> components -> Header -> index.vue 

```vue
<template>
	// 搜索框中写一个
	<input type="text" ... v-model="keyword">
</template>

<script>
	export default {
    name:"",
    data(){
      return {
        keyword:''
      }
    },
    methods:{
      goSearch(){
        // 路由传递参数：
        // 第一种写法：字符串形式
        this.$router.push("/search/"+this.keyword+"?k="+this.keyword.toUpperCase());
        // 第二种写法：模板字符串
      this.$router.push('/search/${this.keyword}?k=${this.keyword.toUpperCase()}');
        // 第三种写法：对象写法
        this.$router.push({
          name:"search",
          params:{
            keyword:this.keyword
          },
          query:{
            k:this.keyword.toUpperCase()
          }
        })
      }
    }
  }
</script>

10010079s
```



第一种路由参数，接收参数的写法：

src -> pages -> Search -> index.vue

```vue
<template>
	<div>
    <h1>
      params参数{{$route.params.keyword}}</h1>
    <h1>
      query 参数 {{$route.query.k }}
  </h1>
  </div>
</template>
```



第一种写法：字符串形式传参数

* 我们需要2中参数都传递数据

1. params 参数传递数据
   * src -> router -> index.js 

需要在 path：中写入内容，因为params参数需要占位，所以写成

```vue
{
    path:"/search/:keyword",
    component:Search,
    meta:{
        showFooter:true
    },
		// 如果不写 name，params找不到地址，当前页面什么都不显示
		// 给路由起一个名称
		name: "search"
},
```

2. query传递数据
   * 需要在字符串的后面写上` + "?k="+this.keyword.toUpperCase()`

完整写法如下：

```vue
      this.$router.push("/search/"+this.keyword+"?k="+this.keyword.toUpperCase());
```





## 九、路由传递参数先关面试题

​    

 #### 1:路由传递参数（对象写法）path是否可以结合params参数一起使用?

* 不可以：不能这样书写，程序会崩掉
* 路由跳转传参的时候，对象的写法可以是name、path形式，但需要注意的是，path这种写法不能与params参数一起使用     

#### 2:如何指定params参数可传可不传? 

比如：配置路由的时候，占位了（params参数），但是路由跳转的时候就不传递。    

路径会出现问题

* 正常的路径为 http://localhost:8080/#/search
* 当不传递params的时候，变成了 http://localhost:8080/#?k=111 缺少了 search

答案：如果路由要求传递params参数，但是你就不传递params参数，会发现一件事情，URL会有问题。



在配置路由的时候，在扎内后面加上一个问号？。即在 path 的后面添加一个问好，代表这个参数可传可不传

```vue
{
// keyword 后面添加了一个?号，代表这个参数可以传递也可以不传递，并且URL地址不会出现错误
    path:"/search/:keyword?",
    component:Search,
    meta:{
        showFooter:true
    },
		name: "search"
},
```



 #### 3:params参数可以传递也可以不传递，但是如果传递是空串，如何解决？

```vue
goSearch(){
	this.$router.push({
		name:'search',
		params:{keyword:''},
		query:{k:this.keyword.toUpperCase()}
	});
}
```

传空串，路径有问题，跟上一道题一样的问题。

可以使用 undefined 解决：params 参数可以传递、可以不传递。也可以传递空的字符串。

改成：

```
goSearch(){
	this.$router.push({
		name:'search',
		params:{keyword:''||undefined},
		query:{k:this.keyword.toUpperCase()}
	});
}
```



#### 4:如果指定name与params配置, 但params中数据是一个"", 无法跳转，路径会出问题

​     







#### 5: 路由组件能不能传递props数据?

可以的。有三种写法

第一种：布尔值的写法，只能传递 params

src -> router -> index.js 

```vue
{
    path:"/search/:keyword?",
    component:Search,
    meta:{
        showFooter:true
    },
		name: "search",
		// 布尔值写法：可以把 params传递给路由参数
		props:true,
},
```

src -> pages -> search -> index.vue

```vue
<template>
	<div>
    // 直接写 props 里面的参数名称即可
    <h1>{{keyword}}</h1>
  </div>
</template>

<script>
	export default {
    name:'',
    // 路由组件可以传递 props
    props:['keyword']
  }
</script>
```



第二种写法：对象写法

```vue
{
    path:"/search/:keyword?",
    component:Search,
    meta:{
        showFooter:true
    },
		name: "search",
		// 对象写法:额外给路由组件传递一些 props
		props:{a:1,b:2},
},
```

src -> pages -> search -> index.vue

```vue
<template>
	<div>
    // 直接写 props 里面的参数名称即可
    <h1>{{a}}</h1>
    <h1>{{b}}</h1>
  </div>
</template>

<script>
	export default {
    name:'',
    // 路由组件可以传递 props
    props:['a','b']
  }
</script>
```



第三种写法：函数写法：

* 可以把params参数、query参数，通过props传递给路由组件

```vue
{
    path:"/search/:keyword?",
    component:Search,
    meta:{
        showFooter:true
    },
		name: "search",
		// 对象写法:额外给路由组件传递一些 props
		props:(route)=>{
				return {
					keyword:route.params.keyword,
					k:route.query.k
				};
		}
},
```

src -> pages -> search -> index.vue

```vue
<template>
	<div>
    // 直接写 props 里面的参数名称即可
    <h1>{{keyword}}</h1>
    <h1>{{k}}</h1>
  </div>
</template>

<script>
	export default {
    name:'',
    // 路由组件可以传递 props
    props:['keyword','k']
  }
</script>
```

简化写法：

```vue
// 对象写法:额外给路由组件传递一些 props
props:(route)=>{
    return {
      keyword:route.params.keyword,
      k:route.query.k
    };
}
改成
props:(route)=>({
      keyword:route.params.keyword,
      k:route.query.k
    });
```





​     



路由分为KV

node平台（并非语言）
对于后台而言:K即为URL地址   V即为相应的中间件
http://localhost:8080/0607
app.get("/0607",(res,req)=>{
   res.send('我是祖国的老花骨朵');
});

前端路由:
K即为URL（网络资源定位符）
V即为相应的路由组件







面试题：v-show与v-if区别?
v-show:通过样式display控制
v-if：通过元素上树与下树进行操作
面试题:开发项目的时候，优化手段有哪些?
1:v-show|v-if
2:按需加载
8)首页|搜索底部是有Footer组件，而登录注册是没有Footer组件
Footer组件显示|隐藏，选择v-show|v-if
路由元信息























4:项目上传GIT
微信小程序实战课的时候，会带着大家玩耍的
注意:前面基础课程当中，创建分支、处理冲突等等
https://gitee.com/jch1011/shangpinhui_0607.git




​     
​    

