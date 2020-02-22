**Vue框架的学习**

[TOC]

# 1、学习目标

第一阶段：Vue基本语法和概念、路由、状态管理、网络请求、Webpack打包工具等。

第二阶段：SpringBoot+Vue项目实战（Crud）

# 2、Vue基本语法

## 2.1什么是Vue.JS

Vue是目前最火（在国内）的一个前端框架，与Angular.js、React.js并成为前端三大框架。MVVM

跨平台：React->ReactNative(React Native使你只使用JavaScript也能编写原生移动应用) -> taro(一处代码，多出运行)

Vue  ->Weex   ->uniapp(一套代码编写到8个平台) 

Weex框架能够完美兼顾性能与动态性，让移动开发者通过简捷的前端语法写出Native级别的性能体验，并支持iOS、安卓、YunOS及Web等多端部署

***\*在“百度指数” 网址网页可以查看最近的使用情况排名\****



## 2.2为什么要学习Vue框架

什么是框架？框架的作用？

框架：半成品的项目。提高开发效率。

前端提高开发效率的发展历程：

DOM：原生JS -> JQuery -> 前端模板引擎  （DOM操作，降低了ajax请求函数的复用性）

MVVM：Angular、Vue、React。能够帮助我们减少不必要的DOM操作；提高渲染效率；数据双向绑定（通过框架提供的指令，我们只需要关心数据的业务逻辑，不需要关心DOM是如何渲染的）。在vue中，一个核心的概念就是不让用户去操作dom元素

## 2.3什么是MVVM

Mvc：后端的分层开发概念 Model、View、Controller

MVVM：是前端视图层的分层开发概念。MVVM将前端视图层分成了三个部分：Model、View、ViewModel

传统的前端开发思想，html与js耦合性过大，导致js代码复用性降低

![1.MVVM和MVC](img\1.MVVM和MVC.png)

## 2.4入门程序

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <!-- 1.导入vue的包 -->
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<body>
  
  <div id="app">
    {{msg}}
    {{name}}
  </div>

<script>
  // 2.我们导入了vue的包之后，在浏览器的内存中，我们就多了一个Vue的构造函数。

  var vue = new Vue({
    el: '#app', // 表示我们new的这个vue实例，控制页面上的哪个区域
    data: { // data是MVVM中的m，专门存放每个页面的数据。
      msg: 'hello vue',
      name: '稽哥'
    }
  })

</script>

</body>
</html>
```

# 3、Vue常用指令

## 3.1v-text、v-html

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>v-text,v-html，v-clock指令</title>
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<style>
  [v-clock] {
    display: none;
  }
</style>
<body>

  <div id="app">
    <!-- v-text指令。会覆盖标签中原有的内容。而占位符（插值表达式）只会替换自己这个占位符，不会把整个元素清空 -->
    <!-- 当直接用占位符时，会有占位符一闪而过的现象。v-text默认没有一闪而过的现象。实际开发中，应结合实际情况选择使用哪一个 -->
    <div>{{msg}} 你好</div>
    <div v-text="msg2">你好</div>
    <!-- v-text和插值表达式会将data中的数据作为字符串去处理。v-html则会将data作为html标签去渲染 -->
    <div v-html="msg3"></div>
  </div>

  <script>

    var vue = new Vue({
      el: '#app',
      data: {
        msg: '稽哥',
        msg2: '哈哈哈',
        msg3: '<h1>我是v-html啊啊啊</h1>'
      }
    })

  </script>
</body>
</html>
```

## 3.2、v-bind、v-on

v-on：绑定事件，简写成：@click="方法名"

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<body>

  <div id="app">
    <!-- 
      v-bind: 字面意思是绑定。是Vue中提供的用户绑定属性的指令。v-bind: 可以缩写成: 
      v-bind可以出现在任何地方。
        每个html标签都有一个属性，直接写属性时，传入的是文本
        加上v-bind时，传入的是vue实例中的data
    -->
    <button :title="msg">这是一个按钮</button>

    <!-- 
      v-on：vue提供的事件绑定指令。可以简写成 @
     -->
     <button @click="show">点我</button>
  </div>

  <script>

    var vue = new Vue({
      el: '#app',
      data: {
        msg: '我是一个按钮'
      },
      methods: { // methods属性用于定义当前vue实例中所有可以调用的方法
        show() {
          alert('点一下晚一年，装备不花一分钱')
        }
      }
    })
  </script>
</body>
</html>
```



## 3.3v-model

**v-model:model中数据的双向绑定**

**注意：v-model只能用于表单元素中。**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<body>

  <div id="app">

    <h1>{{msg}}</h1>
    <!-- 
      v-model指令可以实现表单元素和model中数据的双向绑定。其中一方被修改时，另一方会同步展示。
      注意：v-model只能用于表单元素中。
      v-bind只能用于单向绑定。当model中的数据被修改时，
      v-bind绑定的位置会实时修改。而v-bind绑定的元素被修改时，model中的数据并不会被修改
     -->
    <input type="text" name="" id="" :value="msg">
    
    <button @click="changeMsg">点我</button>
  </div>

  <script>
    var vue = new Vue({
      el: '#app',
      data: {
        msg: '我是一个按钮'
      },
      methods: {
        changeMsg() {
          this.changeMsg2()
        },
        changeMsg2() {
          this.msg = '我现在不是按钮了'
        }
      }
    })
  </script>
</body>
</html>
```

## 3.4简单的计算器实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue指令</title>
    <!--导入Vue的包-->
    <script src="../lib/vue-2.4.0.js"></script>
</head>
<body>
    <div id="app">
        <input type="text" v-model="number1">

        <select v-model="opt">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>

        <input type="text"  v-model="number2">

        <button @click="calcu">计算</button>

        <input type="text" v-model="sum">
    </div>
    <script>
        var vue = new Vue({
            el:'#app',
            data:{
                number1 : 0,
                number2 : 0,
                sum : 0,
                opt : '+'
            },
            methods:{
                calcu(){
                    var a = this.number1;
                    var b = this.number2;
                    if (this.opt==='+'){
                        this.sum = parseInt(a) + parseInt(b)
                    }
                    if (this.opt==='-'){
                        this.sum = parseInt(a) - parseInt(b)
                    }

                    if (this.opt==='*'){
                        this.sum = parseInt(a) * parseInt(b)
                    }

                    if (this.opt==='/'){
                        this.sum = parseInt(a) / parseInt(b)
                    }
                }
            }
        })
    </script>
</body>
</html>
```

## 3.5Vue中写样式

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<style>
.red {
  color: red;
}
.size {
  font-size: 20px;
}
.back {
  background-color: green;
}
</style>
<body>

  <div id="app">
    <!-- 1.行内样式，直接用style指定样式，和传统的html一样。vue不建议写行内样式，会打破mvvm思想，耦合性变高 -->
    <div style="color: red;">我是第1个div</div>
    <!-- 2.用class、id指定，在style中编写样式。推荐 -->
    <div class="red">我是第2个div</div>
    <!-- 3.使用v-bind制定一个数组，数组是class数组 -->
    <div :class="['red', 'size']">我是第3个div</div>
    <!-- 4.使用三元运算符 -->
    <div :class="flag?'size':'red'">我是第4个div</div>
    <!-- 5.使用对象来代替三元表达式，提高代码可读性 -->
    <div :class="{'red': true}">我是第5个div</div>
    <!-- 6.通过绑定data中数据来控制style -->
    <div :class="classObj" id="sixDiv">我是第6个div</div>
    <!-- 2和6用的最多。2就是经典的css写法，耦合性也很低。6是基于vue的v-bind的写法，可以动态的改变样式 -->
    <button @click="changeStyle">改变样式</button>
  </div>

  <script>

    var vue = new Vue({
      el: '#app',
      data: {
        flag: true,
        classObj: {'red':true,'size':true,'back': true}
      },
      methods: {
        changeStyle() {
          this.classObj.red = !this.classObj.red
        }
      }
    })
  </script>
</body>
</html>
```

**\*注意：一般采取2和6两种方式，2就是经典的css写法，耦合性也很低。6是基于vue的v-bind的写法，可以动态的改变样式**

## 3.6v-for

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<style>
  .red {
    color: red;
  }

  .size {
    font-size: 20px;
  }

  .back {
    background-color: green;
  }
</style>

<body>

  <div id="app">
    <!-- v-for是vue中的遍历（循环）指令。(item index) in data，item是遍历的每一项名称，index是每一项的索引值 -->
    <!-- <p v-for="(item, index) in simpleList">索引值：{{index}}---数据：{{item}}</p> -->
    <!-- <p v-for="(obj, i) in objectList">索引值：{{i}}--id：{{obj.id}}--name：{{obj.name}}</p> -->
    <!-- <p v-for="(count, index) in 10">这是第{{count}}次循环，索引为{{index}}</p> -->

    <div>
      <input type="text" v-model="id">
      <input type="text" v-model="name">
      <button @click="add">添加</button>

    </div>

    <!-- key是在v-for中表示唯一标识，在任何的v-for中建议都加上key。key一定要保证唯一性 -->
    <p v-for="item in objectList" :key="item.id">
      <input type="checkbox" name="" id=""> {{item.id}} --- {{item.name}}
    </p>
  </div>

  <script>

    var vue = new Vue({
      el: '#app',
      data: {
        simpleList: [1, 2, 3, 4, 5, 6],
        objectList: [
          { id: 1, name: '张三' },
          { id: 2, name: '李四' },
          { id: 3, name: '王五' },
          { id: 4, name: '赵六' }
        ],
        id: '',
        name: '',
      },
      methods: {
        add() {
          const obj = {id: this.id, name: this.name}
          this.objectList.unshift(obj)
        }
      }
    })
  </script>
</body>
</html>
```

## 3.7v-show和v-if

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
</head>
<body>

  <div id="app">
    <!-- 
      结论：v-show和v-if都可以去动态的控制元素显示与隐藏
      v-if：每次都会重新删除或者创建元素
      v-show：每次不会进行DOM的删除和创建，但是会给元素加上display:none样式
      v-if有较高的切换性能消耗
      v-show有较高的渲染性能消耗

      如果涉及到元素的频繁切换，最好不要使用v-if
      如果元素可能永远也不会显示出来给用户看到，最好用v-if

     -->
    <div v-show="content1">我是内容1</div>
    <button @click="handlerContent1">控制内容1</button>
    <div v-if="content2">我是内容2</div>
    <button @click="handlerContent2">控制内容2</button>
  </div>

  <script>

    var vue = new Vue({
      el: '#app',
      data: {
        content1: false,
        content2: this.content1
      },
      methods: {
        handlerContent1() {
          this.content1= !this.content1
        },
        handlerContent2() {
          this.content2 = !this.content2
        }
      }
    })
  </script>
</body>
</html>
```

## 3.8用户列表案例(CRUD模拟)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue案例</title>
    <!--导入Vue的包-->
    <script src="../lib/vue-2.4.0.js"></script>
    <link rel="stylesheet" href="../lib/layui.css">
</head>
<body>
    <div id="app">
        <form class="layui-form" action="">
            <div class="layui-form-item">
                <label class="layui-form-label">ID</label>
                <div class="layui-input-block">
                    <input type="text"  placeholder="ID" class="layui-input" v-model="id">
                </div>

                <label class="layui-form-label">姓名</label>
                <div class="layui-input-block">
                    <input type="text"  placeholder="姓名" class="layui-input" v-model="name">
                </div>

                <label class="layui-form-label">年龄</label>
                <div class="layui-input-block">
                    <input type="text"  placeholder="年龄" class="layui-input" v-model="age">
                </div>

                <div class="layui-input-block">
                    <button type="button" class="layui-btn" @click="add()">添加</button>
                </div>

                <label class="layui-form-label">搜索</label>
                <div class="layui-input-block">
                    <input type="text"  placeholder="搜索" class="layui-input" v-model="search">
                </div>

                <div class="layui-input-block">
                    <button type="button" class="layui-btn" @click="searchData()">搜索</button>
                </div>
            </div>
        </form>

        <table class="layui-table">
            <colgroup>
                <col width="150">
                <col width="200">
                <col>
            </colgroup>
            <thead>
            <tr>
                <th>ID</th>
                <th>名称</th>
                <th>年龄</th>
                <th>操作</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="item in list" :key="item.id">
                <td>{{item.id}}</td>
                <td v-text="item.name"></td>
                <td v-html="item.age"></td>
                <td><button type="button" class="layui-btn layui-btn-danger" @click="del(item.id)">删除</button></td>
            </tr>
            </tbody>
        </table>
    </div>
    <script>
        var vue = new Vue({
            el:'#app',
            data:{
                list : [
                    {id:1,name:'张三',age:20},
                    {id:2,name:'李四',age:19},
                    {id:3,name:'雷哥',age:10},
                    {id:4,name:'姬哥',age:30}
                ],
                id:'',
                name:'',
                age:'',
                search:''
            },
            methods:{
                // 删除一条数据
                del(id){
                    let index = this.list.findIndex(item=>{
                        if (item.id === id){
                            return true;
                        }
                    });
                    //console.log(res);
                    this.list.splice(index,1);
                },

                // 添加数据
                add(){
                    if(this.id && this.name && this.age){
                        const newData = {id:this.id,name:this.name,age:this.age};
                        this.list.push(newData);
                        // alert("添加成功");
                    }
                },

                // 搜索数据
                searchData(){
                    if(this.search === ''){
                        this.list = [
                            {id:1,name:'张三',age:20},
                            {id:2,name:'李四',age:19},
                            {id:3,name:'雷哥',age:10},
                            {id:4,name:'姬哥',age:30}
                        ];
                        return;
                    }
                    let obj = this.list.filter(item=>{
                        if(item.name.includes(this.search)){
                            return item;
                        }
                    });
                    //console.log(obj);
                    this.list = obj;
                }
            }
        })
    </script>
</body>
</html>
```

**重点：**

	1. findIndex():根据条件可以返回匹配的对象，参数是一个函数
 	2. splice() : 删除对应索引位置的数据，第一个参数为索引，第二个参数为从当前索引位置删除的个数
 	3. filter() : 根据条件可以返回匹配的数组，里面用到字符串的方法是includes()：包含某个字符串，参数是一个函数，**第一参数与findIndex（）中一样都是遍历数组中的每个对象**

# 4、过滤器的简单使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <link rel="stylesheet" href="./lib/layui.css">
</head>

<body>
  <div id="app">

    <table class="layui-table">
      <colgroup>
        <col width="150">
        <col width="200">
        <col>
      </colgroup>
      <thead>
        <tr>
          <th>ID</th>
          <th>姓名</th>
          <th>性别</th>
          <th>备注</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in userList" :key="item.id">
          <td>{{item.id}}</td>
          <td v-text="item.name"></td>
          <!-- 过滤器用法：值 | 过滤器 -->
          <td>{{item.sex | handlerSex}}</td>
          <td>{{item.comment | handlerComment}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</body>


<script>
  var vue = new Vue({
    el: '#app',
    data: {
      id: '',
      name: '',
      sex: '',
      comment: '',
      searchName: '',
      userList: [
        { id: 1, name: '雷哥', sex: '1', comment: '地表最帅男人' },
        { id: 2, name: '稽哥', sex: '1', comment: '地表最最帅男人' },
        { id: 3, name: '刘备', sex: '2', comment: '刘知兵' },
        { id: 4, name: '关羽', sex: '2', comment: '关过江' },
        { id: 5, name: '张飞', sex: '3', comment: '张爱兵' }
      ]
    },
    methods: {
    },
    filters: {
      // 过滤器的参数就是调用方，谁调用过滤器，参数就是谁
      handlerSex(sex) {
        if (sex === '1') {
          // 过滤器直接return，就作为过滤后的值
          return '男'
        } else if (sex === '2') {
          return '女'
        } else {
          return '咱也不知道，咱也不敢问'
        }
      },
      handlerComment(comment) {
        return comment.replace(/帅/g,  '最帅')
      }
    }
  })
</script>

</html>
```

# 5、Vue的生命周期

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <link rel="stylesheet" href="./lib/layui.css">
</head>

<body>
  <div id="app">
    <div id="divId">页面还没有渲染 --- {{msg}}</div>
  </div>
</body>


<script>
  var vue = new Vue({
    el: '#app',
    data: {
      msg: '页面渲染完毕'
    },
    methods: {
    },
    filters: {
    },
    beforeCreate() {
      // 这是我们第一个遇到的生命周期函数，实例被创建出来，还没有data、methods等时，会执行它
      let content = document.getElementById('divId')
      console.log('beforeCreate：', content.innerText)
      // 在js中，null和undefined是不同的含义。null表示有这个对象，但是这个对象的值是null。undefined表示压根就没有这个对象
      console.log('beforeCreate', this.msg)
    },
    created() {
      // Vue实例创建完毕，methods、data、filters等已经挂载到vue实例上。如果需要调用methods，使用data，最早只能在这里操作
      let content = document.getElementById('divId')
      console.log('created', content.innerText)
      console.log('created', this.msg)
    },
    beforeMount() {
      // Vue实例创建完毕，页面尚未重新渲染
      let content = document.getElementById('divId')
      console.log('beforeMounte', content.innerText)
      console.log('beforeMounte', this.msg)
    },
    mounted() {
      // 页面渲染完毕
      let content = document.getElementById('divId')
      console.log('mounted', content.innerText)
      console.log('mounted', this.msg)
    }
  })
</script>

</html>
```

生命周期分析如图：

![2.Vue生命周期钩子函数](img\2.Vue生命周期钩子函数.png)

# 6、动画过渡特效

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <style>
    /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
    /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
    .div1-enter,
    .div1-leave-to {
      opacity: 0;
      transform: translateX(50px);
    }

    /* v-enter-active 入场动画的时间段 */
    /* v-leave-active 离场动画的时间段 */
    .div1-enter-active,
    .div1-leave-active {
      transition: all 0.8s ease;
    }

    /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
    /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
    .div2-enter,
    .div2-leave-to {
      opacity: 0;
      transform: translateX(50px);
    }

    /* v-enter-active 入场动画的时间段 */
    /* v-leave-active 离场动画的时间段 */
    .div2-enter-active,
    .div2-leave-active {
      transition: all 0.2s ease;
    }
  </style>

</head>

<body>
  <div id="app">

    <button @click="flag=!flag">点击</button>

    <transition name="div1">
      <div v-show="flag">这是一个div</div>
    </transition>

    <button @click="flag=!flag">点击2</button>

    <transition name="div2">
      <div v-show="flag">这是一个div2</div>
    </transition>

  </div>
</body>


<script>
  var vue = new Vue({
    el: '#app',
    data: {
      flag: false,
      flag2: false
    },
    methods: {
    },
    filters: {
    }
  })
</script>

</html>
```

