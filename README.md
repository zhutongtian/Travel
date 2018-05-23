# 前言
根据慕课网实战课程——[Vue2.0实战带你开发去哪儿APP](https://coding.imooc.com/class/203.html)开发出来的微型项目，通过这个项目，进一步巩固自己的Vue实践能力。在这之前，还用大白话整理一篇关于[Vue基础知识的整理](https://github.com/CruxF/Vue-base/issues/1)，真的是大白话啊，因此描述的不会是很官方和标准，可能也有些是理解错了（轻点喷....），唯一的亮点就是看一遍真的就能get到Vue大部分的知识了，在这里看完一遍并且理解的话，马上撸一遍[Vue.js的官方文档](https://cn.vuejs.org/)，你会受益匪浅的！



### 多页应用和单页应用的区别
- 多页应用：它是页面跳转时返回一个HTML文件。所具备的优点是：首屏时间快，SEO效果好；缺点是：页面之间切换慢。
- 单页应用：它是页面跳转时利用JavaScript渲染出一个页面。所具备的优点是：页面切换时间短；缺点是：首屏时间稍慢，SEO效果差，因为搜素引擎只识别HTML页面的内容，但是不识别JavaScript渲染出来的页面。



### 如何全局使用一个CSS文件
这个CSS样式是所有组件公用的，使用的方式是在main.js中导入，比如：<br>
`import './assets/styles/reset.css'`<br>

在这个项目中全局还使用了一个解决移动端1px问题的css文件，需要使用的话十分简单，比如要在该元素的下面添加一个1px的边框，那么使用以下类名即可：<br>
`<div class="border-bottom">当前城市</div> `<br>

假如要修改1px边框的颜色或者其他样式，我们可以这么来做：<br>
```
.border-bottom:before {
  border-color: #777;
}
.border-bottom:after {
  border-color: #777;
}
```



### 使用stylus编写样式代码
使用stylus语法编写样式代码能够很好进行代码管理和提高开发速度，首先我们需要利用npm下载相关的依赖包，下载方式如下：<br>
`npm install stylus --save`<br>
`npm install stylus-loader --save`<br>
下载好之后我们就可以使用它了，使用方式可以来看看相关的[中文文档](http://www.zhangxinxu.com/jq/stylus/)



### 项目单位的运算方式
拿首页头部来说，设计稿的尺寸为736x86，即设计稿头部区域高为86px（物理像素）。由于设计师给的是2倍的设计稿，于是CSS样式中这个头部区域的高得为43px（逻辑像素）。在这个项目开发中，是使用rem这个单位，因为rem是相对于根元素的字体大小的单位，该怎么理解呢？比如在全局样式reset.css中，我们设置了html的font-size为50px，则1rem=50px。那么此时逻辑像素为43px转化为rem单位值为多少呢？下面来看一看运算过程：<br>
由1rem=50px  ==>  50x?=43  ==>  ?=0.86  ==>  则此时的43px=0.86rem<br>

更多的移动端知识请转到这里————[移动端基础知识整理](https://github.com/CruxF/IMOOC/issues/4)<br>



### 提高项目的维护性
我们可以将一些经常通用的属性值放在一个样式表中，然后组件导入，直接传变量值就好了，这样就能实现修改一处而改变多处。比如我们可以定义一个公共样式文件，在里面用一个变量存放经常要用到的单独样式，示例代码如下：
```
variables.styl文件中定义
$bgColor = #00bcd4
$darkTextColor = #333

Header.vue文件中使用
<style lang="stylus" scoped="scoped">
@import '~styles/variables.styl'
.header {
  display: flex;
  height: .86rem;
  line-height: .86rem;
  background: $bgColor;
  color: #fff;
}
```

除了定义变量来存放经常要用到的单独样式，我们还能定义方法来存放一组要经常用到的样式，示例代码如下：
```
variables.styl文件中定义
ellipsis(){
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

Header.vue文件中使用
<style lang="stylus" scoped="scoped">
@import '~styles/variables.styl'
.icon-desc {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  height: .44rem;
  line-height: .44rem;
  text-align: center;
  color: $darkTextColor;
  ellipsis();
}
```
<br>



### vue-cli项目中文件路径的别名
在项目开发之中，可能我们会写很多又长又臭的路径名，这是十分不优雅且麻烦的。比如这样的：`import '../../../assets/styles/iconfont.css'`，还有这样的：`@import '../../../assets/styles/variables.styl'`，那么在vue-cli项目中我们就可以使用一些别名来代替，比如能这么写：`import '@/assets/styles/iconfont.css'`，还能这么写：`@import '~@/assets/styles/variables.styl'`，需要注意的是在导入一个css文件到另一个css文件中，@符号前面要加个波浪线，此时这里的@代表的是整个src目录。<br>

我们还能自己来定义各种文件名的别名，修改地址是build——>webpack.base.conf.js，具体改动地方看下面的代码：
```
resolve: {
  extensions: ['.js', '.vue', '.json'],
  alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': resolve('src'),
    'styles':resolve('src/assets/styles'),
  }
},
```
然后我们就能把一开始的导入文件地址这么写：`import 'styles/iconfont.css'`，还有这么写的：`@import '~/styles/variables.styl'`。这样修改之后导入文件路径的书写是不是方便了很多？不过需要注意的是修改了webpack.base.conf.js文件记得重新npm run dev运行项目。<br>



### 使用稳定版本的vue-awesome-swiper插件
这是一个移动端轮播插件，使用步骤为：

- 下载相关jar包`npm install vue-awesome-swiper@2.6.7 --save`
- 使用方式以及相关配置，请到[官方网站](https://github.com/surmon-china/vue-awesome-swiper)进行查看



### 使用Chrome浏览器插件vue devtools
这款插件的作用是能帮助我们更方便的调试vue程序、发现bug和数据传输的过程，说白了就是vue程序调试工具。[这是下载地址](https://github.com/vuejs/vue-devtools)<br>



### 为什么使用axios这个工具来发送ajax获取后台数据？
目前知道发送ajax的手段有以下几种：
- 原生ajax请求
- jQuery中封装好的ajax请求
- 浏览器自带的fetch函数<br>

在vue项目中发送ajax请求的工具有以下两种：
- vue-resource
- axios
- 那么为什么最后官方推荐使用axios来作为发送ajax请求的工具呢？因为axios十分的强大，可以实现跨平台的数据请求，比如axios在浏览器端可以发送XHR的请求，在node服务端上又可以发送http请求。<br>



**使用axios开发步骤：** 
- 安装aixos：`npm install axios --save`
- 在单个组件中导入它：`import axios from 'axios'`
- 来看看一个简单的代码实例：
```
import axios from 'axios'
export default {
  name: 'Home',
  methods: {
    getHomeInfo () {
      axios.get('static/mock/index.json').then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc (res) {
      console.log(res)
    }
  },
  mounted () {
    this.getHomeInfo()
  }
}
```

不过以上代码有个问题就是ajax请求的只是本地的数据，如果要请求服务器的数据，也就是项目需要上线前，那么该做哪些工作呢？
- 首先请求地址修改为后台接口地址，比如`axios.get('/api/index.json').then(this.getHomeInfoSucc)`；
- 转发地址，即我们请求后台接口地址的时候，成功后会将该地址转换成本地的地址进行测试，具体代码如下：
```
proxyTable: {
   '/api': {
     target: 'http://localhost:8080',
     pathRewrite: {
       '^/api': '/static/mock'
     }
   }
 },
```
以上代码的位置在vue-cli项目中的config文件夹下的index.js文件中。它的含义是：当页面请求后台地址是api目录的下面时，那么就将请求转移到本地端口为8080的本地服务器，并且请求地址是以api为开头时，就将地址转换成/statick/mock。这个转发地址的功能是由webpack中webpack-dev-server这个工具提供的。
- 最后当我以下面这段代码去请求数据的时候也能够成功
```
import axios from 'axios'
export default {
  name: 'Home',
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  methods: {
    getHomeInfo () {
      axios.get('/api/index.json').then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc (res) {
      console.log(res)
    }
  },
  mounted () {
    this.getHomeInfo()
  }
}
</script>
```



### Better-scroll的使用及字母表布局
有时候我们开发项目的时候，会有类似与手机联系人浏览模式的需求，也就是向下滚动内容的需求。开发这种需求，我们往往会使用一个插件，叫做————[better-scroll](https://github.com/ustbhuangyi/better-scroll)，国人自主开发移动端（现已支持 PC 端）各种滚动场景需求的插件，文档十分全面，很容易就能入手，下面我们开始正式使用它：
- 下载安装：`npm install better-scroll --save`
- 根据官网的“起步”介绍，我们知道首先得需要一个符合标准的HTML结构（具体去看[官方介绍](https://github.com/ustbhuangyi/better-scroll)），我们需要做的就是做灵活的变动，比如获取DOM数据那块，我们开发该项目是基于Vue的，于是可以根据关键字ref获取到html结构的DOM，具体变动看下面代码：
```
import Bscroll from 'better-scroll'
export default {
  name: 'CityList',
  mounted () {
    this.scroll = new Bscroll(this.$refs.wrapper)
  }
}
```


### 使用Vuex实现数据共享
一般情况下，我们开发一个vue项目，都是使用组件化的方式来进行开发，使得项目更加容易管理和维护。虽然组件化开发有其优点，但是缺点也存在，比如组件与组件之间的数据联动以及管理，父子组件传值还好说，组件与组件的传值那就麻烦了，而且不容易进行数据开发和管理。于是在这种情况下，vuex出来了，我们可以先到[官方网站](https://vuex.vuejs.org/zh/)看vuex的具体介绍。<br>

用大白话说就是vuex是一个公共数据存放仓库，其中的数据是和N个组件共享的，当在某个组件内改变了该数据，那么另一些与之关联的组件数据也会发生变化。下面来看基础的使用步骤：
- 1、下载安装：npm install vuex --save
- 2、创建一个存储数据的仓库，为了方便管理我们可以在src目录下创建一个store目录，然后在里面创建数据仓库文件index.js，具体代码如下：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    city: '北京'
  }
})

```
- 3、我们需要将该数据仓库import到main.js中使其成为全局能使用的API，具体代码如下：
```
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store/index'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})

```
- 4、由于我们在main.js将store这个玩意加载到了new Vue实例中，因此我们要在组件中调用数据仓库中存储的数据那就十分简单，下面看示例代码：
```
<div class="header-right">
   {{this.$store.state.city}}
   <span class="iconfont arrow-icon">&#xe64a;</span>
</div>
```

以上就是基础的vuex使用方式，下面我们需要实现的触发某事件，然后数据发生变化的操作，在开始看具体之前，先来琢磨一下官方给出的这张数据流向图：


结合这张图我们可知在组件中的数据是通过`dispatch`这个方法传递出去，核心代码实现如下：
```
//HTML代码，定义一个点击事件，并传递参数
<div class="title border-bottom">热门城市</div>
<div class="button-list">
  <div class="button-wrapper" v-for="item of hot" :key="item.id" @click="handleCityClick(item.name)">
    <div class="button">{{item.name}}</div>
  </div>
</div>

//Vue实例代码，通过dispatch派发成一个事件，并接收传值然后再传出去
methods: {
  handleCityClick (city) {
    this.$store.dispatch('changeCity', city)
    this.$router.push({
      path: '/'
    })
  }
}
```

继续看官网的数据流向图，可以知道此时数据来到了存储数据的仓库，此时的代码如下：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    city: '上海'
  },
  actions: {
    //ctx参数代表上下文，与mutations进行通信，city是组件传递过来的数据
    //commit（）方法的作用是将city数据传递给MchangeCity这个方法
    changeCity (ctx, city) {
      ctx.commit('MchangeCity', city)
    }
  }
})
```

继续看官网的数据流向图，可以知道数据即将要来到最初始的位置，只要在这个位置将变动的代码传递给state即走完了整个流程，下面看具体代码：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    city: '上海'
  },
  actions: {
    changeCity (ctx, city) {
      ctx.commit('MchangeCity', city)
    }
  },
  mutations: {
    //state代表了最开始存放的区域
    MchangeCity (state, city) {
      state.city = city
    }
  }
})

```

虽然以上就是完整的实现了组件之间数据联动的功能，但是事情还没结束呢，因为刷新页面时，数据还是会变回state中最初始的值，那么该怎么办呢？此时，就到了HTML5中的localstorage发挥作用的时候了！下面请看具体代码，简直太简单了：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    city: localStorage.city || '上海'
  },
  actions: {
    changeCity (ctx, city) {
      ctx.commit('MchangeCity', city)
    }
  },
  mutations: {
    MchangeCity (state, city) {
      state.city = city
      localStorage.city = city
    }
  }
})
```

为了避免用户将浏览器的本地存储关闭而导致的错误使网站奔溃，我们应该编写以下更加合理的代码：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

let defaultcity = '上海'
try {
  if (localStorage.city) {
    defaultcity = localStorage.city
  }
} catch (e) {}

export default new Vuex.Store({
  state: {
    city: defaultcity
  },
  actions: {
    changeCity (ctx, city) {
      ctx.commit('MchangeCity', city)
    }
  },
  mutations: {
    MchangeCity (state, city) {
      state.city = city
      try {
        localStorage.city = city
      } catch (e) {}
    }
  }
})

```

其实对于vuex数据的传递，我们还有一个简洁的方法，就是定义一个数组项来映射vuex中的数据，使HTML代码中调用数据的代码量减少，同时方便管理，下面看具体代码：
```
//{{this.$store.state.city}}变成了this.city
<div class="header-right">
   {{this.city}}
   <span class="iconfont arrow-icon">&#xe64a;</span>
</div>

<script>
import { mspState } from 'vuex'
export default {
  name: 'HomeHeader',
  computed: {
    ...mapState(['city'])
  }
}
</script>

//我们也能传递个对象过去
//此时this.city就要变成this.currentCity了
<script>
import { mspState } from 'vuex'
export default {
  name: 'HomeHeader',
  computed: {
    ...mapState({
      currentCity: 'city'
    })
  }
}
</script>
```

以上是单个数据的传递简洁写法，下面是传递数据方法的简洁写法：
```
<script>
import Bscroll from 'better-scroll'
//import进来mapActions这个玩意
import { mapState, mapActions } from 'vuex'
export default {
  name: 'CityList',
  props: {
    hot: Array,
    cities: Object,
    letter: String
  },
  computed: {
    ...mapState({
      currentCity: 'city'
    })
  },
  methods: {
    handleCityClick (city) {
      //直接使用这个方法传值，而不是this.$store.dispatch('changeCity', city)
      this.changeCity(city)
      this.$router.push({
        path: '/'
      })
    },
    //映射changeCity这个方法
    ...mapActions(['changeCity'])
  }
}
</script>
```



### 项目难点
1、在城市列表实现点击右侧A-Z字母，右侧内容滚动到相应的位置。<br>

【分析】<br>
在这一整个页面，总共由三个组件组成，那么首先需要考虑到的是如何去传值？该传什么值？接收到值后应该做什么？按照这个思路，我们做一下具体的代码实现。<br>

【实现】<br>
由于组件之间的关系并不复杂，层次也不是很深，因此我们直接使用父子组件传值的方式即可。首先我们在数据最开始流出的地方(Alphabet.vue)定义一个点击字母事件触发发布订阅模式，然后发布一个事件，并且将数据传递出去：
```
handleLetterClick (e) {
   this.$emit('change', e.target.innerText)
},
```
其中e.target.innerText代表的是目标元素的内容，也就是item的值，而item是从cities对象中遍历出来的A-Z字母。<br>

接着我们在父组件(City.vue)监听/订阅这个事件，接收传递过来的值，并将值传给最终的一个子组件(List.vue)：
```
HTML
<city-list :cities="cities" :hot="hotCities" :letter="letter"></city-list>
<city-alphabet :cities="cities" @change="handleLetterChange"></city-alphabet>

JS
handleLetterChange (letterValue) {
  this.letter = letterValue
}
```
其中letterValue代表的是传递过来的 e.target.innerText的值，然后将这个值以父组件向子组件传递的方式传给List.vue这个子组件。<br>

最后我们在List.vue这个子组件中使用better-scroll这个插件实现内容定位功能，具体代码如下：
```
import Bscroll from 'better-scroll'
export default {
  name: 'CityList',
  props: {
    hot: Array,
    cities: Object,
    letter: String
  },
  watch: {
    letter () {
      if (this.letter) {
        // 无法获取到this.$refs[this.letter]的值，和视频中以及自己的假想完全不同啊！
        var Element = this.$refs[this.letter][0]
        console.log(Element)
        this.scroll.scrollToElement(Element)
      }
    }
  },
  mounted () {
    this.scroll = new Bscroll(this.$refs.wrapper)
  }
}
```
this.letter代表的是A-Z，也可以把this.$refs[this.letter]当做是获取到id为A-Z区域的所有内容，这是一个数组，要将其转化为一个div区域的很简单，只要这么写this.$refs[this.letter][0]即可，之后再把这个div区域传到this.scroll.scrollToElement中则可完成滚动需求。以上的值各代表什么可以使用console.log进行测试，最后要注意的一点是在HTML代码中要有个值和this.letter对应才行，我们这么来定义：`<div class="area" v-for="(item,key) of cities" :key="key" :ref="key">`<br>

【bug问题解决】<br>
以上“无法获取到this.$refs[this.letter]的值”的bug是由于在数据最开始传递的地方HTML代码多了一层div，下面是错误的HTML结构代码：
```
<template>
<div class="alphabet">
  <ul class="list">
    <li
      class="item"
      v-for="item of letters"
      :key="item"
      :ref="item"
      @click="handleLetterClick"
      @touchstart.prevent="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      {{item}}
    </li>
  </ul>
</div>
</template>
```
下面是正确的HTML结构代码，至于是什么原因，我也是丈二和尚摸不着头脑：
```
<template>
  <ul class="list">
    <li
      class="item"
      v-for="item of letters"
      :key="item"
      :ref="item"
      @touchstart.prevent="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
      @click="handleLetterClick"
    >
      {{item}}
    </li>
  </ul>
</template>
```


2、在城市列表实现滚动右侧的字母表，左侧区域能够到达指定位置。<br>

【分析】<br>
谈到移动端的滚动事件，那么肯定和touch事件逃脱不了关系。我们现在需要明白的如何获取到滚动的值并且将它传递过去，下面看代码实现<br>

【实现】<br>
首先，我们需要将A-Z的数据遍历出来并存储到某个数组中：
```
computed: {
  letters () {
    const letters = []
    for (let i in this.cities) {
      letters.push(i)
    }
    // console.log(letters)
    return letters
  }
}
```
接着我们在遍历的时候也需要将对象改为数组,同时也要设置各种手指触摸事件和ref，设置reg目的是为了获取A这个区域距离顶部的距离，具体请看最后的代码：
```
<li
  class="item"
   v-for="item of letters"
   :key="item"
   :ref="item"
   @click="handleLetterClick"
   @touchstart.prevent="handleTouchStart"
   @touchmove="handleTouchMove"
   @touchend="handleTouchEnd"
 >
  {{item}}
</li>
```
最后就是核心代码实现，其中的原理请看注释：
```
methods: {
  handleLetterClick (e) {
    this.$emit('change', e.target.innerText)
  },
  handleTouchStart () {
    this.touchStatus = true
  },
  handleTouchMove (e) {
    if (this.touchStatus) {
      // 计算A到顶部的距离，这是个固定值
      const startY = this.$refs['A'][0].offsetTop
      // console.log(startY)
      // 计算手指距离顶部的距离，这是个不固定值。79代表的是城市选择和搜索框区域的高度
      const touchY = e.touches[0].clientY - 79
      // console.log(touchY)
      // 计算对应数组的下标值，每个字母的高度为20px
      const index = Math.floor((touchY - startY) / 20)
      if (index >= 0 && index < this.letters.length) {
        this.$emit('change', this.letters[index])
      }
    }
  },
  handleTouchEnd () {
    this.touchStatus = false
  }
}
```

以上代码性能偏低，现在需要提升一下。首先使用updated()生命周期函数缓存固定不变的量，比如：
```
updated () {
  // startY在data中已经定义
  this.startY = this.$refs['A'][0].offsetTop
}
```
由于在滚动的时候的，handleTouchMove方法执行的频率非常高，现在我们需要使用一个定时器来限制该方法的执行频率，提高性能，俗称函数节流，具体看以下代码：
```
handleTouchMove(e) {
  if(this.touchStatus) {
    if(this.timer) {
      clearTimeout(this.timer)
    }
    this.timer = setTimeout(() => {
      const touchY = e.touches[0].clientY - 79
      const index = Math.floor((touchY - this.startY) / 20)
      if(index >= 0 && index < this.letters.length) {
        this.$emit('change', this.letters[index])
      }
    }, 16)
  }
}
```
好了，这块的功能完美实现了！


3、实现搜索数据并显示结果的功能<br>

【分析】<br>
首先第一步我们肯定是要先把HTML结构代码和CSS布局样式写好，之后我们需要使用v-model实现双向绑定数据，为了能够实时的展示出搜索结果，那么毫无疑问，这种单独数据的变化使用watch是最佳的选择，接着我们就要考虑下，该如何将得到数据遍历出来，是通过一个数组？还是说得通过两个数组？如果是需要通过两个数组，那么原因是什么？以及为什么需要遍历数组，难道结果不是唯一的吗？带着这些疑问，我们来看代码实现<br>

【实现】<br>
首先我们需要定义一个变量实现双向数据绑定以及wath函数需要监听的变量函数，同时定义一个数据用来存放搜素结果：
```
data () {
  return {
    keyword: '',
    list: [],
    timer: null
  }
}
```

接着我们使用watch监听keyword这个变量的变化，在监听器中我们需要使用定时器来使函数节流，提高程序的性能。我们一起看以下来进一步分析：
```
watch: {
  keyword() {
    if(this.timer) {
      clearTimeout(this.timer)
    }
    if(!this.keyword) {
      this.list = []
      return
    }
    this.timer = setTimeout(() => {
      const result = []
      // 查看json文件可知i代表的是A-Z字母
      // value代表的是每一个字母中的对象
      for(let i in this.cities) {
        this.cities[i].forEach((value) => {
          if(value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
            result.push(value)
          }
        })
      }
      this.list = result
    }, 100)
  }
}
```
通过以上代码我们可知在监听器中又定义一个数组存放搜索结果，首先使用for-in将i值遍历了出来，接着又使用forEach遍历搜索每个i值对应的spell值和name值，如果这两个值存在的话，那么就将一整个value对象加入到result这个数组中，然后再让list等于result这个数组，从而达到了提高性能并且方便了其他功能的实现，如果没有resutl这个中间数组的话，那么当搜索框的数据在变化的时候就会发生错误，但是通过中间数组，这种情况就不会发生，因为Vue中的数据是双向绑定的。<br>

最后就是一些HTML结构代码中的一些合理的判断显示，很简单，看源码就能明白，在这不说了。<br>



### 有用的网站
1、能够定制和收藏属于自己的icon网站，[传送门](http://www.iconfont.cn/home/index?spm=a313x.7781069.1998910419.2)在此。我们可以在每次开发一个项目的时候都在里面收集一些icon，并为这些icon创建一个相应的仓库。<br>

**使用方式** <br>
icon新建一个项目，在官方图标库中找到相应的icon，加入购物车，再选择购物车添加到创建的项目之中，然后下载到本地。我们需要的是把iconfont.css、iconfont.eot、iconfont.svg、iconfont.ttf和iconfont.woff文件添加加到我们的开发项目中，唯一需要注意的是我们需要在iconfont.css更改下调用文件的路径，如果iconfont.eot、iconfont.svg、iconfont.ttf和iconfont.woff这些文件的位置法伤变化的话。<br>

iconfont.css是全局样式，如何去使用前面有提到。在项目中是这么来使用的，首先在相应的区域添加一个iconfont的类，然后来那个区域使用在iconfont官网复制下来的代码，下面请看案例：<br>
`<span class="iconfont">&#xe624;</span>`


### 如何设置忽略文件不上传到云端？
有个.gitignore文件配置，只要将该文件目录或者具体的文件名配置进去即可，不过目前我在项目中还没找到，估计改位置了，有空再来好好找一下。


# 项目下载和运行

```
下载：git clone git@github.com:CruxF/Travel.git
```
