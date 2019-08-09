# Vue.js Day01
+ Vue.js的引入方式
```html
    <!--1. 外引方式 通过 使用URL的方式-->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <!--2.本地文件方式引入-->
    <script src="./lib/vue.js"></script>
```
## Vue.js代码结构
```js
 //2.创建 Vue 实例对象
          var vm=new Vue({
              el:'#app', // 选择器名称  表示当前new的vue实例，要控制页面上的哪一个区域
              data:{  //存放el 中要用到的数据
                msg:'欢迎学习Vue'  //指令

              }
          })
```
### 指令

指令 (Directives) 是带有 v- 前缀的特殊特性。
#### `v-cloak` 解决页面加载时闪烁问题
> 案例：
> 01-v-cloak的学习.html
``` html
<div id="app">
       <!-- 插值{{ }} 会因为加载顺序的问题造成 闪烁问题
        1.通过使用v-cloak来解决
        -->
       <h3 v-cloak>{{ msg }}</h3>
       <!--2.使用v-text 也可以解决闪烁问题-->
       <h2 v-text="msg"></h2>
       <!--插值{{}} 表达式和v-text区别-->
       <p>++++++++{{ msg }}++++++++++++</p>
       <p v-text="msg">===============</p>
       <!--v-text默认是没有闪烁问题表达式只会替换自己的这个占位符，不会把整个元素的内容清空
        v-text会覆盖元素中原本的内容，但是插值
        -->
        <!---->
       

   </div> 
```
```js
 var vm=new Vue({
        el:'#app',
        data:{
            msg:'v-cloak是用来解决常见的一个问题',
            msg2:'<h1>这是带有元素标签的字符串</h1>'

        }
    })
```
#### `v-text` 和 `v-html`
```html
 <!-- v-html是用来解析带有元素标签的字符串-->
        <div v-text="msg2"></div>
        <div v-html="msg2"></div>
```

#### `v-bind` 指令
1\.在vue中v-bind是用来绑定属性的指令  使用**v-bind**有3种方式：
+ 直接使用v-bind指令
```html
  <input type="button" value="按钮2" v-bind:title="mytitle">
```
+ 使用简化指令
```html
 <input type="button" value="按钮2" :title="mytitle">
 ```
+ 拼接绑定内容
```html
<input type="button" value="按钮2" :title="mytitle + '123'">
```
#### `v-on` 指令
> Vue中提供`v-on`事件绑定
```html
<div id="app">
        <!--原生JS实现时间响应-->
        <input type="button" value="原生JS按钮" onclick="clickHandler()">
        <!--使用Vue实现-->
        <input type="button" value="Vue按钮" v-on:click="sayHello">
       <!--v-on的简写@event-->
        <input type="button" value="Vue按钮" @click="sayHello">
        <input type="button" value="Vue按钮" @mouseover="sayHello">
    </div>
```
```js
  var vm=new Vue({
            el:'#app',
            data:{

            },
            methods: {
                sayHello:function(){
                    alert('hello')
                }
            }
        })
```
#### `跑马灯` 实例
```html
 <div id="app">
        <input type="button" value="浪起来" @click="lang">
        <input type="button" value="停止" @click="stop">
        <h4 v-text="msg"> </h4>
    </div>
```
```js
<script>
    var vm=new Vue({
        el:'#app',
        data:{
            msg:'小花猫，别浪了~~~~~',
            intervalID:null
        },
        methods: {
            lang(){
               /* 
                //获取第一个字母
                var start=this.msg.substring(0,1);
                //获取后面的字符
                var end=this.msg.substring(1);
                //拼接
                this.msg=end+start;
                */
               //如果定时器还在运行中就不要再开启
               if(this.intervalID!=null) return 
               this.intervalID=setInterval(() => {
                var start=this.msg.substring(0,1);
                var end=this.msg.substring(1);
                this.msg=end+start;
               }, 400);

            },
            stop(){
                //停止定时器
                clearInterval(this.intervalID);
                //还原定时器
                this.intervalID=null
            }
        },
    })
    //分析
    //1.实现 按钮 让字符串 走起来 v-on事件  来触发
    //2.在按钮的事件中，写相关 逻辑代码：先拿到msg,然后调用字符串函数substring进行字符串截取
    //把第一个字符串截取出来，放到最后一个位置即可

    </script>
    
```
#### `事件修饰符` 
```html
<!--事件修饰符格式：@event.modifier
    .stop阻止冒泡
    .prevent阻止默认事件
    .capture实现捕获
    .self只当事件应用在元素本身  才会触发事件
    .once事件只触发一次
    -->
    <div id="app">
         <!--1.使用.stop阻止冒泡-->
        <div class="inner" v-on:click="divHandle">
            <input type="button" value="戳它" @click.stop="btnHandle">
        </div>
        <!--2.使用.prevent阻止默认事件-->
        <a href="http://www.baidu.com"  @click.prevent="linkHandle">百度</a>
        <!--3.capture-->
        <div class="inner" v-on:click.capture="divHandle">
            <input type="button" value="戳它" @click="btnHandle">
        </div>
        <!--4.self-->
        <div class="inner" v-on:click.self="divHandle">
                <input type="button" value="戳它" @click="btnHandle">
        </div>
        <div class="outer" @click="outerHandle">
                <div class="inner" v-on:click.self="divHandle">
                        <input type="button" value="戳它" @click="btnHandle">
                </div>
        </div>
        <!--5.once-->
        <a href="http://www.baidu.com"  @click.prevent.once="linkHandle">百度</a>
        
    </div>
```
```js
 var vm=new Vue({
            el:'#app',
            data:{

            },
            methods: {
                btnHandle(){
                    console.log("这是触发了btn的点击事件")
                },
                divHandle(){
                    console.log("这是触发了div的点击事件")
                },
                linkHandle(){
                    console.log("这是触发了a的点击事件")
                },
                outerHandle(){
                    console.log("这是触发了outer的点击事件")
                }
            },
        })

```
#### `v-model` 数据双向绑定
```html
    <div id="app">
        <h4 v-text="msg"></h4>
        <input type="text"  :value="msg">
        <input type="text"  v-model="msg">
    </div>
```
```js
 var vm=new Vue({
            el:'#app',
            data:{
                msg:'大家都是好学生，爱敲代码，爱学习，简直完美'
            }

        })
```
####    `v-for` 指令基于一个数组来渲染一个列表
```html
    <div id="app">
        <!--1.对普通的 数组列表轮询-->
        <p v-for="(item,key) in list">第{{key}}项 值为：{{ item }}</p>
        <ul>
            <!--对对象数组进行轮询-->
            <li v-for="item in obList">{{item.name}}</li>
            <li v-for="(item,index) in obList">{{index}} {{item.name}}</li>
        </ul>
        <ul>
            <!--3.对对象轮询-->
            <li v-for="(item,key,i) in userInfo">{{key}} {{i}} {{item}}</li>
        </ul>
        <ul>
            <!--4.对数字进行轮询-->
            <li v-for="count in 10">{{count}}</li>
        </ul>
    </div>
```
```js
  var vm=new Vue({
            el:'#app',
            data:{
                list:[
                    1,2,3,4,5,6
                ],
                obList:[
                    {id:1,name:'zhangsan'},
                    {id:2,name:'zhangsan2'},
                    {id:3,name:'zhangsan3'},
                    {id:4,name:'zhangsan4'},
                    {id:5,name:'zhangsan5'},
                ],
                userInfo:{
                    id:1,
                    name:'超人',
                    sex:'M'
                }
            }
        })
```
# Vue.js Day02
#### `v-for优化`
```html
<div id="app">
        <div>
           id: <input type="text" v-model="userId" >
           name: <input type="text" v-model="userName">
           <input type="button" value="添加" @click="add">
        </div>
        <!--为了给 Vue 一个提示，以便它能跟踪每个节点的身份，
            从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性：,其中数据的id不能重复-->
        <p v-for="(item,index) in listArray" v-bind:key="item.id">
            <input type="checkbox" >
            {{item.id}}---{{item.name}}
        </p>
    </div>
```
```js
 var vm=new Vue({
            el:'#app',
            data:{
                userId:0,
                userName:'',
                listArray:[
                    {id:1,name:'李斯'},
                    {id:2,name:'王五'},
                    {id:3,name:'赵六'},
                    {id:4,name:'张三'},
                    {id:5,name:'李四'}
                ]
            },
            methods: {
                add(){
                //    this. listArray.push({
                //        id:this.userId,
                //        name:this.userName
                //    })
                    this. listArray.unshift({
                       id:this.userId,
                       name:this.userName
                   })
                }
            },
        })
```
#### `v-if` 和`v-show`区别
```html
<div id="app">
        <p >{{flag}}</p>
        <input type="button" value="切换" @click="toggle">
        <h2 v-if="flag">这是通过v-if的方式来控制元素的状态</h2>
        <h2 v-else>else通过v-if的方式来控制元素的状态</h2>
        <h2 v-show="flag">通过v-show的方式控制元素的状态</h2>
        <!--v-if-->
        <!--特点：每次都会重新创建或者删除元素-->
        <!--v-show-->
        <!--特点：每次不会重新进行DOM的创建和删除，
            只是会切换元素的display样式
        -->
        <!--v-if有较高的切换性能消耗-->
        <!--v-show有较高的初始渲染消耗-->
        <!--如果频繁的切换元素，最好使用v-show 而不是使用v-if-->
        <!--如果元素可能永远不会被显示出来，则推荐使用v-if-->
    </div>
```

```js
var vm=new Vue({
            el:'#app',
            data:{
                flag:false
            },
            methods: {
                toggle(){
                    this.flag=!this.flag
                }
            },
        })
```











