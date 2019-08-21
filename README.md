# -

一. computed、methods和watch
    1.区别：
        computed: computed是基于他的依赖缓存，当他相关依赖的值发生改变时，才会重新取值。
        methods: methods是重新渲染页面时，总会执行。
        watch: watch更多的是‘观察’的作用，类似于数据的监听，每当所监听的数据发生改变时，才会执行
    2.运用场景及优势：
        computed相比较于methods，computed更倾向于计算方面性能较好，可利用computed的缓存特性，避免每次去重新计算去取值，并且computed只有（getter）属性，在需要时可添加一个（setter）属性
        watch当我们需要在数据变化是执行异步或开销较大的操作时，应该使用watch，使用watch选项允许我们执行异步操作（访问一个API）,限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。computed则无法做到这一点

二. v-for遍历必须为item添加key,且避免同时使用v-if
    1.在使用v-for时，在每一项设置唯一key值，方便vue.js内部机制准确找到该条列表数据。当state(状态)更新时，新的状态值和旧的状态值对比，较快地定位到diff。
    2.在使用v-for时避免使用v-if
        v-for的优先级比v-if优先级高，如果每次需要遍历整个数组，将会影响速度，可使用computed属性。

三.图片资源懒加载
    1.插件安装 npm install vue-lazyload --save dev
    2.在入口文件main.js中引入并使用 import VueLazyload from 'vue-lazyload'
    3.在vue中直接使用 Vue.use(VueLazyload)
        也可自定义 Vue.use(VueLazyload,{
            preLoad: 1.3,
            error: 'dist/error.png',
            loading: 'dist/loading-gif',
            attempt: 1
        })
    4.在vue文件中将img标签的src属性改为v-lazy,从而将图片显示方式改为懒加载显示
        <img v-lazy='https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566295251751&di=b8283f34b769d85ab5164046174223b2&imgtype=0&src=http%3A%2F%2Fpic16.nipic.com%2F20110914%2F8262611_122312165309_2.jpg'>
四.路由懒加载
    const Foo = () => import('./Foo.vue')
    const router = new VueRouter({
      routes: [
        { path: '/foo', component: Foo }
      ]
    })
五.服务端渲染SSR or 预渲染
    服务端渲染是指Vue在客户端将标签渲染成整个html片段的工作在服务端完成服务端形成html片段直接返回给客户端整个过程就叫做服务端渲染
    1.服务端渲染的优点：
        *   更好的SEO：因为SPA页面的内容时通过Ajax获取，而搜索引擎爬取工具并不会等待Ajax异步完成后在抓取页面内容，所以在SPA中是抓取不到页面通过Ajax获取到的内容；而SSR是直接由服务端返回已经渲染好的页面（数据已经包含在页面中），所以搜索引擎爬取工具可以抓取渲染好的页面
        *   更快的内容到达时间（首屏加载更快）: SPA会等待所有Vue编译后的js文件都下载完成之后，才开始进行页面的渲染，文件下载等需要一定的时间等，所以首屏渲染需要一定的时间；SSR直接由服务端渲染好页面直接返回显示，无需等待下载js文件及再去渲染等，所以SSR有更快的内容到达时间；
    2.服务端渲染的缺点：
        *   更多的开发条件限制：例如服务端渲染只支持beforeCreate和created两个钩子函数，这回导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行；并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序SPA不同，服务端渲染应用程序，需要处理Node.js server运行环境；
        *   更多的服务器负载：在Node.js中渲染完整的应用程序，显然会比仅仅提供静态文件的server更加大量占用CPU资源，因此如果你预料在高流量环境下使用，请准备相应的服务器负载，并明智地采用缓存策略
