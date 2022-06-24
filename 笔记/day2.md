# 第二天

##### 1.编程式导航路由跳转到当前路由(参数不变), 多次执行会抛出NavigationDuplicated的警告错误?

-- 路由跳转有两种形式：声明式导航、编程式导航

-- 声明式导航没有这类问题，因为 vue-router底层已经处理好了。

##### 1.1 为什么编程式导航进行路由跳转的时候，会有这种警告错误？

“vue-router”:"^3.5.3"  // 最新的vue-router引入了promise

解决办法：通过给push方法传递相应的成功回调和失败的回调，可以捕获到当前错误，来解决这种警告错误。（`这种方法治标不治本。`）

```vue
let result = this.$router.push({
	name:"search",
	params:{keyword:this.keyword},
	query:{k:this.keyword.toUppcase()}
},
	// 成功回调和失败回调
	()=>{},
	(error)=>{console.log(error);},
)
```

* 这种方法治标不治本。将来在别的组件当中使用 push ||  replace , 编程式导航还会有类似的错误。



```vue
function push(){

​	return new Promise((resolve,reject)=>{

​	})

}
```



解释：

1. this: 当前组件实例（search）
2. `this.$router`属性：当前的这个属性，属性值VueRouter类的一个实例。他是在入口文件注册路由的时候，给组件实例添加`$router` 和 `$route`属性。
3. push : 是VueRouter类的一个原形方法 
4. `$router`是VueRouter类的实例，类的实例可以直接调用类的原形方法。原形方法push 进行修改，修改结果就会作用于组件实例的`$rout`



一次性解决

在src -> router -> index.js 中

```vue
// 先把 VueRouter 原形对象的 push 保存一份
let originPush = VueRouter.prototype.push;
```







## 二、home首页模块组件拆分

-- 先把相应的静态页面完成

-- 拆分出静态组件

-- 获取服务器的数据进行展示

-- 动态业务

1. 把三级联动拆分成一个组件

2. 把轮播图和尚品汇快报裁成一个整体，也可以裁成2个。

3. 今日推荐 是一个组件

   ... ...



## 三、三级联动组件完成

当一个组件在很多页面使用的时候，需要注册成为一个全局组件。

全局组件：只需要注册一次，就可以在项目的任意地方使用。 

-- 由于三级联动，在Home,search，Detail 中使用，可以把三级联动注册陈伟一个全剧组件。



#### 1.获取html、css内容

#####1.1 获取Html

从前台项目_student 中 的 谷粒商城-> home.html 页面中拿到 “商品分类导航” 的代码。

在自己写的项目中写：

	* app -> src -> pages -> Home -> typeNav -> index.vue

##### 1.2 获取css代码

从前台项目_student 中 的 谷粒商城-> css ->  home.less 页面中拿到 “type-nav” 的css代码。

放到自己项目中

* app -> src -> pages -> Home -> typeNav -> index.vue -> `<style scoped lang="less">`

```vue
<script>
	export default {
    name:"TypeNav",
  }
</script>
```

##### 1.3 注册成全局组件

scr -> main.js 

```vue
// 三级联动组件 --- 全局组件
import TypeNav from '@/pages/Home/TypeNav'
// 第一个参数：全局组件的名字，第二个参数：哪一个组件
Vue.component(TypeNav.name, TypeNav);

一定要注意大消息
```



##### 1.4 使用三级联动组件

src -> pages -> Home -> index.vue

```vue
<template>
	<div>
    // 三级联动已经是全局组件了，因此不需要再次引入。直接使用即可
    <TypeNav/>
  </div>
</template>
```



## 四、完成其余静态组件

4.1 拆分组件的时候需要注意的三个点：

样式 + 结构 + 图片资源

HTML + CSS + 图片资源



4.2 app -> src -> pages -> ListContainer 

跟 "三" 中的一样.

1. 图片资源
2. 修改图片的路径

./images/home/icons.png 把 中间的/home 删掉即可



4.3 引入其余的组件

src -> pages -> Home -> index.vue

```vue
<template>
	<div>
    <TypeNav/>
    <ListContainer></ListContainer>
    <Recomend/>
    <Rank/>
    <Like/>
    <Floor/>
  </div>
</template>

<script>
  // 引入其余的组件
  import ListContainer from '@/pages/Home/ListContainer';
  import Recomend from '@/pages/Home/Recommend';
    import Rank from '@/pages/Home/Rank';
    import Like from '@/pages/Home/Like';
    import Floor from '@/pages/Home/Floor';
      import  from '@/pages/Home/';
    import  from '@/pages/Home/';
	export default {
    name:'',
    components:{
      ListContainer,
      Recomend,
      Rank,
      Like,
      Floor,
    }
  }
</script>
```

 

##五、POSTMAN 测试接口

-- 给的接口，经过 postman工具测试，是没有问题的

-- 如果服务器返回的数据code为200，代表服务器返回数据成功

-- 整个项目，接口的前缀都有/api字样





## 六、axios 二次封装

XMLHTTPRequest 、fetch、JQ、axios 都可以网络请求

6.1 为什么需要进行二次封装 axios？

请求拦截器、响应拦截器

请求拦截器：可以在发送请求之前处理一些业务；

响应拦截器：当服务器数据返回后，可以处理一些事情。



6.2 代码

src -> api -> request.js 

```vue
// 对于 axios 进行二次封装
import axios from "axios";

// 1. 利用axios 对象方法create， 去创建一个axios实例
// 2. request 就是 axios， 只不过稍微配置一下
const requests = axios.create({
	// 配置对象
	// 基础路径：发送请求的时候，路径当中会出现api
	baseURL:"/api",
	// 代表请求超时的时间5s
	timeout:5000,
})

// 请求拦截器：在发送请求之前，请求拦截器可以检测到。可以在请求发出去之前做一些事情
requests.interceptors.request.use((config)=>{
	// config: 配置对象，对象里面有一个属性很重要，headers 请求头
	return config;
})

// 响应拦截器
requests.interceptors.response.use((res)=>{
	// 成功的回调函数：服务器的数据回来以后，响应拦截器可以检测到，可以做一些事情
	return res.data;
},(error)=>{
	// 响应失败的回调函数
	return Promise.reject(new Error('faile'));
});


// 对外暴露
export default axios;
```



6.3 在项目当中经常API文件夹【axios】

接口当中：路径都带有/api

baseURL:"/api"



6.4 有的同学 axios 基础不好，可以参考 git | NPM 关于 axios 文档 



## 七、接口统一管理

项目很小：完全可以在组件的生命周期函数中发送请求

src -> api -> index.js 



```vue
// 当前这个模块： API 进行统一管理
import requests from './request';

// 三级联动接口
/// api/product/getBaseCategoryList get 无参数

export const reqCategoryList = () => {
	// 发送请求: axios 发请求返回的结果是 Promise 对象
	return requests({   
    url:'/product/getBaseCategoryList',
		method:'get'
	});
}
```



src -> main.js 

```vue
// 测试一下上面写的接口，看看能否返回数据

import {reqCategoryList} from '@/api';

reqCategoryList();
```

请求失败：因为跨域问题



7.1 跨域问题

什么是跨域：请求的时候，协议、域名、端口号跟本地的不一样。称为跨域问题。

例如：

```vue
// 前端项目本地服务器
https://localhost:8080/#/home
// 后台服务器
https://39.98.123.211
```

因为这两个服务器不是在一个域名上，所以出现请求失败。

![2022060901跨域请求失败](/Users/study/Desktop/vueDemo/笔记/images/2022060901跨域请求失败.png)



如何解决：

JSONP、CROS、代理服务器解决跨域问题



本项目用代理服务器解决跨域问题

1. webpack 这个api -> 官网 -> devServer -> 在 webpack.config.js 中去配置（实际上是vue.config.js）





编写 vue.config.js 

```vue
module.exports = {

	// 关闭 eslint
	lintOnSave: false,
	devServer: {
		proxy:{
			'/api':{
				// 代理跨域
				target: 'http://39.98.123.211',
				// pathRewrite:{'^/api':''},
      },
    },
  },
}
```

上面的代码直接在 webpack.docschina.org -> 中文文档 -> DevServer -> webpack.config.js 中的代码.

* 写完代码后，需要在终端重启服务

`npm run serve`





## 八、nprogress 进度条的使用

1. 在终端写 `npm install --save nprogress`

2. 安装完成后，可以在 package.json 中的 `dependencies` 中看是否有 `nprogress`。

3. 在 src -> api -> request.js 中写

   ```vue
   
   // 引入进度条
   // start：进度条开始  done：进度条结束
   import nprogress from 'nprogress';
   
   // 引入进度条样式
   import "nprogress/nprogress.css";
   
   requests.interceptors.request.use((config)=>{
   
   	// 进度条开始动
   	nprogress.start();
   	return config;
   })
   
   // 响应拦截器
   requests.interceptors.response.use((res)=>{
   
   	// 进度条结束
   	nprogress.done();
   	return res.data;
   },(error)=>{
   	
   	return Promise.reject(new 	 Error('faile'));
   }); 
   ```

4. 进度条颜色可以修改的，直接修改nprogress 代码里面的样式。



## 九、vuex 状态管理库

9.1 vuex 是什么？

vuex 是官方提供的一个插件，状态管理库，集中式管理项目中组件共用的数据。

切记，并不是全部项目都需要Vuex，如果项目很小，完全不需要vuex。如果项目很大，组件很多，数据很多，数据维护很费劲，使用Vuex.

它里面有几个方法

```vue
state

mutations

actions

getters

modules
```



代码编写：

app -> src -> store -> index.js 

```vue
import Vue from 'vue';
import Vuex from 'vuex';

// 需要使用插件一次
Vue.use(Vuex);
// state:仓库存储数据的地方
const state = {};
// mutations: 修改 state 的唯一手段
const mutations ={};
// action：处理 action， 可以书写自己的业务逻辑，也可以处理异步
const actions = {};
// getters: 理解为计算属性，用于简化仓库数据，让组件获取仓库的数据更加方便
const getters = {};

// 对外暴露store类的一个实例
export default new Vuex.Store({
	state,
	mutations,
	actions,
	getters
})
```



app -> store -> main.js 

```vue
// 三级联动组件 --- 全局组件
import TypeNav from '@/pages/Home/TypeNav';
// 第一个参数：全局组件的名字 第二个参数： 哪一个组件
Vue.component(TypeNav.name, TypeNav);
// 引入路由
import router from '@/router';
// 引入仓库
import store from '@/store';

// 可以先测试一下
import {reqCategoryList} from '@/api';
reqCategoryList();

new Vue({
	render: h => h(App),
	router,
	// 注册仓库：组件实例的身上会多一个属性￥store 属性
	store
}).$mount('#app')

```



可以测试一下，代码如下：

```vue
import Vue from 'vue';
import Vuex from 'vuex';

// 需要使用插件一次
Vue.use(Vuex);

// state:仓库存储数据的地方
const state = {
	count:1
};

// action：处理 action， 可以书写自己的业务逻辑，也可以处理异步
const actions = {
  // 这里可以书写业务逻辑，但是不能修改 state
  add({commit}){
    commit('ADD');
  }
};

// mutations: 修改 state 的唯一手段
const mutations ={
	ADD(state,count){
    state.count++;
  }
};

// getters: 理解为计算属性，用于简化仓库数据，让组件获取仓库的数据更加方便
const getters = {};

// 对外暴露store类的一个实例
export default new Vuex.Store({
	state,
	mutations,
	actions,
	getters
})
```

































————————————————————————————



犯的错误:
1:项目阶段，左侧菜单目录，只能有项目文件夹
2:联想电脑安装node_modules依赖包的时候，经常丢包。npm install --save axios --force
3：单词错误
4：路由理解
KV：K--->URL  V---->相应的组件
配置路由：
     ------路由组件
     -----router--->index.js
                  import Vue  from 'vue';
                  import VueRouter from 'vue-router';
                  //使用插件
                  Vue.use(VueRouter);
                  //对外暴露VueRouter类的实例
                  export default new VueRouter({
                       routes:[
                            {
                                 path:'/home',
                                 component:Home
                            }
                       ]
                  })
    ------main.js   配置项不能瞎写


$router:进行编程式导航的路由跳转
this.$router.push|this.$router.replace
$route:可以获取路由的信息|参数
this.$route.path
this.$route.params|query
this.$route.meta

编程式导航路由跳转到当前路由(参数不变), 多次执行会抛出NavigationDuplicated的警告错误?

注意:编程式导航（push|replace）才会有这种情况的异常，声明式导航是没有这种问题，因为声明式导航内部已经解决这种问题。
这种异常，对于程序没有任何影响的。
为什么会出现这种现象:
由于vue-router最新版本3.5.2，引入了promise，当传递参数多次且重复，会抛出异常，因此出现上面现象,
第一种解决方案：是给push函数，传入相应的成功的回调与失败的回调
第一种解决方案可以暂时解决当前问题，但是以后再用push|replace还是会出现类似现象，因此我们需要从‘根’治病；



2)将Home组件的静态组件拆分
2.1静态页面（样式）
2.2拆分静态组件
2.3发请求获取服务器数据进行展示
2.4开发动态业务
拆分组件：结构+样式+图片资源
一共要拆分为七个组件








3)axios二次封装
AJAX:客户端可以'敲敲的'向服务器端发请求，在页面没有刷新的情况下，实现页面的局部更新。
XMLHttpRequest、$、fetch、axios
跨域:如果多次请求协议、域名、端口号有不同的地方，称之为跨域
JSONP、CROS、代理
2.1:工作的时候src目录下的API文件夹，一般关于axios二次封装的文件
2.2进度条：nprogress模块实现进度条功能
工作的时候，修改进度条的颜色，修改源码样式.bar类名的




4)vuex:今晚务必vuex复习一下
vuex:Vue官方提供的一个插件，插件可以管理项目共用数据。
vuex：书写任何项目都需要vuex？
项目大的时候，需要有一个地方‘统一管理数据’即为仓库store
Vuex基本使用:

​     



















弹幕:

一般编程式导航 都会带参数走 ，意味着可能要调用接口  防止用户重复点击 多次提交信息



说白了就是利用promise函数异常穿透的特性 让他报错的时候不执行任何操作呗



看不懂的先去把Promise好好学一下,在把模块化学一下,在把axios学一下,这些内容都不是很多,不然后面更难学下去



不是说了吗，proprise,会有两个参数一个成功，一个失败，一旦执行就不可逆，基础都没有你学什么框架？



就是VueRouter才是push的本质，哪里调用了VueRouter就可以在哪里重写push



const originPush = VueRouter.prototype.push



这块是原型链那块的知识



老师讲错了啊，$router是vueRouter类的实例



老师讲的也太绕了两句话就解决了 $router是VueRouter的一个实例 push是VueRouter原型上的方法



不是push是vuerouter原型对象上的方法



看不懂的去看js基础,弄懂原型



这里最好有一点ES6基础才听的懂



push是VueRouter类的一个原型方法

$router是VueRouter类的实例

类的实例可以直接调用类的原型方法

所以对原型方法push进行修改，修改结果就会作用于组件实例的$router实例。



很简单push replace方法是`$router`隐式原型中的方法，`$router`的隐式原型就是veurouter的显示原型

为什么push函数的执行上下文是VueRouter的实例啊？





因为router是vuerouter所创建的实例 所以它身上可以通过原型链找到 vuerouter身上的属实方法



push是vuerouter原型对象上的的方法 而vueRouter等同于vue-router



构造函数VueRouter和它的实例对象$route都指向同一个原型对象，然后修改原型对象上的push方法
