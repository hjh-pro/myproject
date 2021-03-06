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

**{{}}的补充：**

1、只要符合javascript的语法规范都能写在{{}}中

2、{{}}里面只能写表达式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tittle</title>
    <style>
        *{margin:0;padding:0;}
        li{list-style:none;}
        a{text-decoration: none;}
        body{user-select: none;}
    </style>
</head>
<body>
    <div id="wrap">
        <p>{{msg + content + number1 }}</p>

        <!--<p>{{msg + , + number1 }}</p>  vue会把,当作变量解析-->
        <p>{{msg + ',' + number1 }}</p>

        <p>{{isShow?msg:content}}</p>

        <p v-html="isShow?msg:content"></p>

        <p v-html=`${msg}abc${isShow}`></p>

        <p v-html="isShow + 1"></p>

    </div>
    <script src="../lib/vue-2.4.0.js"></script>

    <script>
        new Vue({
            el : "#wrap",
            data : {
                msg : '标题',
                content : '内容',
                number1: 111,
                isShow : false
            }
        });
    </script>
</body>
</html>
```

![image-20200229175600426](C:\Users\hjh\AppData\Roaming\Typora\typora-user-images\image-20200229175600426.png)

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

**v-on高级用法**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tittle</title>
    <style>
        *{margin:0;padding:0;}
        li{list-style:none;}
        a{text-decoration: none;}
        body{user-select: none;}
    </style>
</head>
<body>
    <div id="wrap">

        <!--按任意键，按下时都会触发-->
        <!--<input type="text" @keydown="add">
        <p>{{number}}</p>-->

        <!--只有按enter时才会触发 -- 方式1-->
        <input type="text" @keydown.13="add">
        <p>{{number}}</p>

        <!--只有按enter时才会触发 -- 方式2-->
        <input type="text" @keydown.enter="add">
        <p>{{number}}</p>
        
        <!--  .是修饰符，可以多个 -->
        <input type="text" @keydown.enter.space="add">
        <p>{{number}}</p>

    </div>
    <script src="../lib/vue-2.4.0.js"></script>
    <script>
        new Vue({
            el : "#wrap",
            data : {
                number : 0
            },
            methods : {
               add(){
                   this.number++;
               }
            }
        });
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tittle</title>
    <style>
        *{margin:0;padding:0;}
        li{list-style:none;}
        a{text-decoration: none;}
        body{user-select: none;}
    </style>
</head>
<body>
    <div id="wrap">
        <button @click.once="add">一次事件</button>
        <p>{{num}}</p>

        <button @click.left="add">只有左键点击触发事件</button>
        <button @click.middle="add">只有中键点击触发事件</button>
        <button @click.right="add">只有右键点击触发事件</button>

    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>

    <script>
        new Vue({
            el : "#wrap",
            data : {
                num : 0
            },
            methods : {
                add(){
                    this.num ++ ;
                }
            }
        });
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tittle</title>
    <style>
        *{margin:0;padding:0;}
        li{list-style:none;}
        a{text-decoration: none;}
        body{user-select: none;}
    </style>
</head>
<body>
    <div id="wrap">
        <!--事件冒泡-->
        <div @click="add">
            <button @click="add">一次事件</button>
        </div>
        {{num}}
        <!--阻止冒泡-->
        <div @click="add">
            <button @click.stop="add">一次事件</button>
        </div>

        <!--阻止默认事件-->
        <button @click.right.prevent="add">右键 触发，阻止默认事件</button>
        <a href="https://www.baidu.com" @click.prevent="add">百度</a>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>

    <script>
        new Vue({
            el : "#wrap",
            data : {
                num : 0
            },
            methods : {
                add(){
                    this.num ++ ;
                }
            }
        });
    </script>
</body>
</html>
```

**多个事件的绑定**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>tittle</title>
    <style>
        *{margin:0;padding:0;}
        li{list-style:none;}
        a{text-decoration: none;}
        body{user-select: none;}
        .goudan{
            width: 100px;
            height: 100px;
            background-color: blue;
        }
    </style>
</head>
<body>
    <div id="wrap">
        <!--绑定多个事件必须用v-on=""-->
        <!--""里面是对象-->
        <div
            class="goudan"
            v-on="{
                click:add,
                mouseenter:reduce
            }"
        >
        </div>
        {{num}}

        <!--多个事件的绑定-->
        <a href="https://www.baidu.com"
            @click.left.prevent="add"
            @click.right.prevent="add"
        >百度</a>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>

    <script>
        new Vue({
            el : "#wrap",
            data : {
                num : 0
            },
            methods : {
                add(){
                    this.num ++ ;
                },
                reduce(){
                    this.num--;
                }
            }
        });
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

	1. findIndex():根据条件可以返回匹配的对象的索引，参数是一个函数
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

# 7、组件

## 7.1创建组件

**Vue.component('组件的名称',{template:'模板的选择器'})**

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
    <my-component></my-component>
    <my-component2></my-component2>
    <my-component3></my-component3>
    <my-component4></my-component4>
  </div>

  <template id="temp1">
    <div>
      <div>哈哈哈哈</div>
      <p>我是p标签</p>
      <h1>我是个大大的h1</h1>
    </div>
  </template>

  <script>
    var num = 3
    // 1 使用Vue.extend来组件组件
    // 按照java的开发思想，变量名往往是驼峰规则。
    // vue定义组件时可以使用驼峰规则，但是，使用组件时如果存在驼峰，应当全部改为小写，并将每个单词用 - 连接
    Vue.component('myComponent', Vue.extend({
      template: '<h3>这是用extend组件的组件</h3>' // template就是组件要展示的内容，可以是html标签
    }))

    // 不使用extend去组件组件
    Vue.component('myComponent2', {
      template: '<h4>不使用extend去组件组件</h4>'
    })

    // 不论用哪种方式去组件组件，组件template属性只能有一个。并且有且只能有一个根节点
    // ES6语法：反引号 ` ES6中用来解决字符串拼接烦恼的新引号。在两个反引号之间可以写任何内容，不需要使用 + 拼接
    // 如果在`中需要使用到变量，直接使用模板语法${}
    Vue.component('myComponent3', {
      template: `
            <div><h4>不使用extend去组件组件</h4>
                  <div>${num}</div></div>
              `
    })

    // 3.使用template
    Vue.component('myComponent4', {
      template: '#temp1'
    })

    var vue = new Vue({
      el: '#app',
      data: {
      }
    })

  </script>
</body>
</html>
```



## 7.2组件中的data和methods

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
    <my-component></my-component>
  </div>

  <template id="temp1">
    <div>
      <div>哈哈哈哈 {{count}}</div>
      <button @click="add">点我</button>
    </div>
  </template>

  <script>
    // 3.使用template
    Vue.component('myComponent', {
      template: '#temp1',
      // Vue 组件中的data必须是一个方法，并且返回一个对象
      // 组件的存在是为了复用性，定义了一个组件后，可能会有多个地方使用到该组件。
      // 如果按照data: {}的写法，多个组件会复用同一个data，降低组件的复用性，而定义为function则不会。
      data() {
        return {
          count: 0
        }
      },
      methods: {
        add() {
          this.count++
        }
      }
    })

    var vue = new Vue({
      el: '#app',
      data: {
      }
    })

  </script>
</body>
</html>
```



## 7.3Vue私有组件

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
    <my-component></my-component>
  </div>

  <div id="app2">
    <my-component></my-component>
    <my-component2></my-component2>
  </div>

  <template id="temp1">
    <div>
      <div>哈哈哈哈 {{count}}</div>
      <button @click="add">点我</button>
    </div>
  </template>

  <template id="temp2">
    <div>
      <div>我是私有组件</div>
    </div>
  </template>

  <script>
    // 3.使用template
    Vue.component('myComponent', {
      template: '#temp1',
      // Vue 组件中的data必须是一个方法，并且返回一个对象
      // 组件的存在是为了复用性，定义了一个组件后，可能会有多个地方使用到该组件。
      // 如果按照data: {}的写法，多个组件会复用同一个data，降低组件的复用性，而定义为function则不会。
      data() {
        return {
          count: 0
        }
      },
      methods: {
        add() {
          // Vue中，建议js代码结尾不要有分号。建议字符串用单引号包起来。
          // 在严格语法中，分号和双引号是不符合语法规范的，会报错
          this.count++
        }
      }
    })

    var vue = new Vue({
      el: '#app',
      data: {
      }
    })
    var vue2 = new Vue({
      el: '#app2',
      data: {
      },
      components: { // 定义私有组件的方式：
        // 组件名称建议用引号包起来。如果不包起来，在严格语法情况下会报警告，需要改成-。而方法、变量中是不允许有横线的，就会报错、
        'myComponent2': {
          template: '#temp2'
        }
      }
    })
  </script>
</body>
</html>
```



## 7.4组件切换的两种方式

方式一：

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
    <button @click="flag=true">登录</button>
    <button @click="flag=false">注册</button>
    <login v-if="flag"></login>
    <register v-else></register>
  </div>

  <script>
    Vue.component('login', {
      template: '<p>我是登录组件</p>'
    })
    Vue.component('register', {
      template: '<p>我是注册组件</p>'
    })

    var vue = new Vue({
      el: '#app',
      data: {
        flag: true
      }
    })

  </script>

</body>

</html>
```

方式二：

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
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>

  <div id="app">
    <button @click="componentName='login'">登录</button>
    <button @click="componentName='register'">注册</button>

    <!-- transition提供了mode属性，设置组件切换时的模式 -->
    <transition name="component" mode="out-in">
      <!-- 
        Vue提供了component，来展示对应名称的组件。
        这是个占位符，使用:is来指定要展示的组件名称
       -->
      <component :is="componentName"></component>
    </transition>

  </div>

  <script>
    Vue.component('login', {
      template: '<p>我是登录组件</p>'
    })
    Vue.component('register', {
      template: '<p>我是注册组件</p>'
    })

    var vue = new Vue({
      el: '#app',
      data: {
        componentName: 'login'
      }
    })

  </script>
</body>
</html>
```



## 7.5父组件向子组件传值

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
    <!-- 父组件向子组件传值。属性名称，也需要使用- -->
    <my-component :parent-msg="msg"></my-component>
  </div>

  <template id="temp1">
    <div>
      <div>{{title}}</div>
      <!-- 经过测试，子组件无法直接使用到父组件中的data -->
      <!-- <div>{{msg}}</div> -->
      <div>{{parentMsg}}</div>
      <button @click="changeMsg">修改</button>
    </div>
  </template>

  <script>
    var vue = new Vue({
      el: '#app',
      data: {
        msg: '我是父组件中的msg'
      },
      components: {
        'my-component': {
          template: '#temp1',
          data() {
            return {
              title: '我是标题'
            }
          },
          methods: {
            changeMsg() {
              this.parentMsg = '我被修改了'
            }
          },
          // props中的数据，都是通过父组件传递给子组件的。
          // props中的数据都是只读的，无法去赋值
          props: {
            parentMsg: { // props使用数组是非常不标准的写法。建议使用对象，并给每一个prop声明类型和默认值
              type: String,
              default: null
            }
          }
        }
      }
    })
  </script>
</body>
</html>
```



## 7.6父组件向子组件传递方法

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
    {{msg}}<br/>
    {{title}}
    <!-- 
      父组件向子组件传递方法，需要使用v-on。 
      v-on属性表示子组件调用方法的名称。值表示父组件传递的方法。
      这里也不能使用驼峰规则。建议也不要使用 - ，这里的名称就使用单个单词
    -->
    <my-component @change="changeMsg"></my-component>
  </div>

  <template id="temp1">
    <div>
      <!-- 通过点击修改按钮，修改父组件中的msg -->
      <button @click="changeMsg">修改</button>
    </div>
  </template>

  <script>
    var vue = new Vue({
      el: '#app',
      data: {
        msg: '我是父组件中的msg',
        title: '我是父组件的标题'
      },
      methods: {
        changeMsg(msg, title) {
          this.msg = msg
          this.title = title
        }
      },
      components: {
        'my-component': {
          template: '#temp1',
          methods: {
            changeMsg() {
              // emit 第一个参数是要调用的方法名称。第二个参数以后都表示这个方法需要传递的参数
              this.$emit('change', '修改了父组件中的msg', '修改了父组件的title')
            }
          }
        }
      }
    })

  </script>
</body>
</html>
```



## 7.7父组件调用子组件的方法

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
    <button @click="getElement">获取元素</button>
    <button @click="handlerChildFunction">调用子组件方法</button>
    <div ref="div1">我是内容</div>
    <my-component ref='com1'></my-component>
  </div>

  <template id="temp1">
    <div>
    </div>
  </template>

  <script>
    var vue = new Vue({
      el: '#app',
      methods: {
        getElement() {
          // 1. 获取dom元素。就用传统的js去获取。但是vue不推荐这种用法
          // let text = document.getElementById('div1').innerText
          // 2. 获取dom元素，使用ref （reference）。这个是vue提供的写法。建议使用这个。
          let text = this.$refs.div1.innerText
          console.log(text)
        },
        handlerChildFunction() {
          // 父组件调用子组件方法。，直接使用ref去调用方法。方法的用法和子组件中一模一样
          this.$refs.com1.childFunction()
        }
      },
      components: {
        'my-component': {
          template: '#temp1',
          methods: {
            childFunction() {
              console.log('子组件方法被调用了')
            }
          }
        }
      }
    })
  </script>
</body>
</html>
```

# 8、Vue路由

## 8.1路由的基本使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-router-3.0.1.js"></script>
</head>
<style>
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>

  <div id="app">
    <!-- router-link控制组件的切换。router-link默认会被渲染成a标签 -->
    <!-- 如果不想被渲染成a标签，可以使用tag属性，指定router-link被渲染成哪种标签 -->
    <router-link to="/login" tag="button">登录</router-link>
    <router-link to="/register">注册</router-link>

    <transition name="component" mode="out-in">
      <!-- 这是个坑，专门作为一个占位符。当路由进行切换的时候，在路由中匹配到对应的组件，就会展示在这个坑里  -->
      <router-view></router-view>
    </transition>

  </div>

  <script>
    var login = {
      template: '<p>我是登录组件</p>'
    }
    var register = {
      template: '<p>我是注册组件</p>'
    }

    // 当引入了vue-router之后，在window全局对象中，就有了一个VueRouter构造函数
    let routerObj = new VueRouter({
      routes: [ // routes中定义了路由的规则
        {path: '/', redirect: '/login'}, // redirect就是重定向，访问path时，会被重定向到redirect指定的路由。这里是指路由的重定向，而非请求重定向，和后端的redirect是两码事
        {path: '/login', component: login}, // 对象中有两个属性：path：表示路由的url，component：表示路由跳转到的组件
        {path: '/register', component: register}
      ]
    })

    var vue = new Vue({
      el: '#app',
      data: {
      },
      router: routerObj
    })

  </script>
</body>
</html>
```

## 8.2路由参数

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-router-3.0.1.js"></script>
</head>
<style>
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>
  <!-- 
    以往的非前后端分离项目，如果需要根据id获取某条数据，在另一个页面去展示？
      页面A点击，传递id给后端，后端查出，转发到页面B，页面B展示。
    前后端分离项目？
      页面A点击，传递id给页面B，页面B 在created中根据id将数据查出，接着，在页面B展示。
   -->
  <div id="app">
    <!-- 只需要在url后面用?去传参即可，不需要对路由进行任何修改 -->
    <router-link to="/login?id=123&name=张三" tag="button">登录</router-link>
    <router-link to="/register/12312/李四">注册</router-link>

    <transition name="component" mode="out-in">
      <router-view></router-view>
    </transition>

  </div>

  <script>
    var login = {
      template: '<p>我是登录组件 --- {{$route.query.id}} --- {{$route.query.name}}</p>',
      created() {
        // this.$route获取路由对象
        // route.query只适用于 ？ 后面拼接的参数
        console.log(this.$route.query.id, this.$route.query.name)
      }
    }
    var register = {
      template: '<p>我是注册组件 --- {{$route.params.id}} --- {{$route.params.name}}</p>',
      created() {
        // this.$route获取路由对象
        console.log(this.$route.params.id, this.$route.params.name)
      }
    }

    let routerObj = new VueRouter({
      routes: [
        {path: '/login', component: login},
        {path: '/register/:id/:name', component: register}
      ]
    })

    var vue = new Vue({
      el: '#app',
      data: {
      },
      router: routerObj
    })
  </script>
</body>
</html>
```

## 8.3路由的嵌套

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-router-3.0.1.js"></script>
</head>
<style>
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>

  <div id="app">
    <!-- router-link控制组件的切换。router-link默认会被渲染成a标签 -->
    <!-- 如果不想被渲染成a标签，可以使用tag属性，指定router-link被渲染成哪种标签 -->
    <router-link to="/login" tag="button">登录</router-link>
    <router-link to="/register">注册</router-link>

    <transition name="component" mode="out-in">
      <!-- 这是个坑，专门作为一个占位符。当路由进行切换的时候，在路由中匹配到对应的组件，就会展示在这个坑里  -->
      <router-view></router-view>
    </transition>

  </div>

  <template id="login">
    <div>
      <p>我是登录组件</p>
      <router-link to="/login/loginByPassword" tag="button">账密登录</router-link>
      <!-- 不建议这么写，建议像上面，把父级路由也加上 -->
      <router-link to="loginBySms">验证码登录</router-link>
      <router-view></router-view>
    </div>
  </template>

  <script>
    var login = {
      template: '#login'
    }
    var register = {
      template: '<p>我是注册组件</p>'
    }
    var passwordLogin = {
      template: '<p>密码登录</p>'
    }
    var smsLogin = {
      template: '<p>验证码登录</p>'
    }

    // 当引入了vue-router之后，在window全局对象中，就有了一个VueRouter构造函数
    let routerObj = new VueRouter({
      routes: [ // routes中定义了路由的规则
        { path: '/', redirect: '/login' }, // redirect就是重定向，访问path时，会被重定向到redirect指定的路由。这里是指路由的重定向，而非请求重定向，和后端的redirect是两码事
        {
          path: '/login',
          component: login,
          children: [ // children属性用于指定子路由，通过子路由可以实现路由的嵌套
            // children里的属性和路由是一毛一样的，path和component
            { path: 'loginByPassword', component: passwordLogin },
            { path: 'loginBySms', component: smsLogin }
          ]
        },
        { path: '/register', component: register } // 对象中有两个属性：path：表示路由的url，component：表示路由跳转到的组件
      ]
    })

    var vue = new Vue({
      el: '#app',
      data: {
      },
      router: routerObj
    })

  </script>
</body>
</html>
```

## 8.4名称拼接

### 8.4.1方式一

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
    <!-- 
      分析：如果使firstName和lastName被修改时，同步修改fullName？
     -->
    <input type="text" v-model="firstName" @keyup="handlerName">
    <input type="text" v-model="lastName" @keyup="handlerName">
    <input type="text" v-model="fullName">

  </div>

<script>
  var vue = new Vue({
    el: '#app',
    data: {
      firstName: '',
      lastName: '',
      fullName: ''
    },
    methods: {
      // 第一种方式：给前两个表单加上键盘抬起事件。当触发事件时，拼接两个name，为fullName赋值。
      handlerName() {
        this.fullName = this.firstName + this.lastName
      }
    }
  })
</script>
</body>
</html>
```

### 8.4.2方式二(Watch)

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
    <!-- 
      分析：如果使firstName和lastName被修改时，同步修改fullName？
     -->
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <input type="text" v-model="fullName">

  </div>

<script>
  var vue = new Vue({
    el: '#app',
    data: {
      firstName: '',
      lastName: '',
      fullName: ''
    },
    watch: { // 监听器，可以监听某个属性
        'firstName': function(newVal, oldVal) { // 格式：要监听的属性:function(newVal, oldVal)
          this.fullName = newVal + this.lastName
        },
        // 使用监听器，可以直接对属性进行监听，不需要去监听用户的操作。使开发者只关注Model，不关注View
        'lastName': function(newVal, oldVal) {
          this.fullName = this.firstName + newVal
        }
    }
  })
</script>
</body>
</html>
```

### 8.4.3方式三(computed)

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
    <button @click="handlerSumName">点我</button>
    <!-- 
      分析：如果使firstName和lastName被修改时，同步修改fullName？
     -->
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <input type="text" v-model="sumName">

  </div>

  <script>
    var vue = new Vue({
      el: '#app',
      data: {
        firstName: '',
        lastName: '',
        fullName: ''
      },
      methods: {
        handlerSumName() {
          console.log(this.sumName)
        }
      },
      computed: {
        // 计算属性。在computed中，可以定义一些 属性 ，这些属性叫做计算属性、计算属性的本质就是一个方法，但是，当我们使用的时候，是将其视为属性去用的。
        // 简单说就是一句话，按照method写，按照data去用。this.name，this.name()

        // 注意：计算属性调用的时候，一定不要加()，它就是个属性，用法和data一模一样。
        // 注意：只要计算属性中使用到的所有data中的属性发生了变化，就会立即去重新计算属性的值。
        // 注意：计算属性的结果会被缓存起来，方便下次直接调用，如果计算属性中用到的所有的data都没有发生变化，就不会对计算属性重新求值。
        sumName() {
          console.log('计算属性执行了')
          return this.firstName + this.lastName
        }
      }
    })
  </script>
</body>
</html>
```

## 8.5计算属性中的get/set方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue拼接-方法3</title>
    <!--导入Vue的包-->
    <script src="../lib/vue-2.4.0.js"></script>
</head>
<body>
    <div id="app">
        <input type="text" v-model="a" >
        <input type="text" v-model="b" >
        <input type="text" v-model="sum">
        <button @click="handler">点击</button>
    </div>

    <script>

        var vue = new Vue({
            el:'#app',
            data:{
                a:1,
                b:2
            },
            methods : {
              handler(){
                  console.log(this.sum);
              }
            },
            computed : {
                sum:{
                    // get() : 是用于获取属性的值
                    get(){
                        console.log("computed方法执行了");
                        return this.a + this.b;
                    },
                    // set() : 用于设置，当sum属性发生变化时，执行的操作
                    set(){
                        this.a = 666;
                    }
                }
            }
        })
    </script>
</body>
</html>
```

## 8.6监听器的使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-router-3.0.1.js"></script>
</head>
<style>
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>

  <div id="app">
    <!-- router-link控制组件的切换。router-link默认会被渲染成a标签 -->
    <!-- 如果不想被渲染成a标签，可以使用tag属性，指定router-link被渲染成哪种标签 -->
    <router-link to="/login" tag="button">登录</router-link>
    <router-link to="/register">注册</router-link>

    <transition name="component" mode="out-in">
      <!-- 这是个坑，专门作为一个占位符。当路由进行切换的时候，在路由中匹配到对应的组件，就会展示在这个坑里  -->
      <router-view></router-view>
    </transition>

  </div>

  <script>
    var login = {
      template: '<p>我是登录组件</p>'
    }
    var register = {
      template: '<p>我是注册组件</p>'
    }

    // 当引入了vue-router之后，在window全局对象中，就有了一个VueRouter构造函数
    let routerObj = new VueRouter({
      routes: [ // routes中定义了路由的规则
        {path: '/', redirect: '/login'}, // redirect就是重定向，访问path时，会被重定向到redirect指定的路由。这里是指路由的重定向，而非请求重定向，和后端的redirect是两码事
        {path: '/login', component: login}, // 对象中有两个属性：path：表示路由的url，component：表示路由跳转到的组件
        {path: '/register', component: register}
      ]
    })

    var vue = new Vue({
      el: '#app',
      data: {
      },
      router: routerObj,
      watch: {
        '$route.fullPath': function(newVal, oldVal) {
          // 监听路由去执行对应的逻辑
          // 应用场景:监听路由的url是否为登录url.如果不是,校验用户是否登录,未登录的情况下跳转到登录页
          console.log('用户路由跳转，url为：',  newVal)
        }
      }
    })
  </script>
</body>
</html>
```

## 8.7编程式导航

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="./lib/vue-2.4.0.js"></script>
  <script src="./lib/vue-router-3.0.1.js"></script>
</head>
<style>
  /* v-enter 这是个时间点，是进入之前，元素的起始状态，此时还没有进入 */
  /* v-leave-to 这是个时间点，是动画离开之后，元素的终止状态，此时，元素 动画已经结束了 */
  .component-enter,
  .component-leave-to {
    opacity: 0;
    transform: translateX(50px);
  }

  /* v-enter-active 入场动画的时间段 */
  /* v-leave-active 离场动画的时间段 */
  .component-enter-active,
  .component-leave-active {
    transition: all 0.5s ease;
  }
</style>

<body>

  <div id="app">
    <!-- router-link控制组件的切换。router-link默认会被渲染成a标签 -->
    <!-- 如果不想被渲染成a标签，可以使用tag属性，指定router-link被渲染成哪种标签 -->
    <router-link to="/login" tag="button">登录</router-link>
    <router-link to="/register">注册</router-link>
    <button @click="toLogin">跳转到登录</button>

    <transition name="component" mode="out-in">
      <!-- 这是个坑，专门作为一个占位符。当路由进行切换的时候，在路由中匹配到对应的组件，就会展示在这个坑里  -->
      <router-view></router-view>
    </transition>

  </div>

  <script>
    var login = {
      template: '<p>我是登录组件</p>',
      created() {
        console.log(this.$route)
      }
    }
    var register = {
      template: '<p>我是注册组件</p>'
    }

    // 当引入了vue-router之后，在window全局对象中，就有了一个VueRouter构造函数
    let routerObj = new VueRouter({
      routes: [ // routes中定义了路由的规则
        {path: '/', redirect: '/login'}, // redirect就是重定向，访问path时，会被重定向到redirect指定的路由。这里是指路由的重定向，而非请求重定向，和后端的redirect是两码事
        {path: '/login', component: login}, // 对象中有两个属性：path：表示路由的url，component：表示路由跳转到的组件
        {path: '/register', component: register}
      ]
    })

    var vue = new Vue({
      el: '#app',
      data: {
      },
      methods: {
        toLogin() {
          // 编程式导航：通过代码去操作路由
          // this.$router.push('/login')
          // 编程式路由如何传递参数
          this.$router.push({
            path: '/login', 
            params: {id: '2121', name: '呵呵'}
          })
        }
      },
      router: routerObj,
      watch: {
        '$route.fullPath': function(newVal, oldVal) {
          // 监听路由去执行对应的逻辑
          // 应用场景:监听路由的url是否为登录url.如果不是,校验用户是否登录,未登录的情况下跳转到登录页
          console.log('用户路由跳转，url为：',  newVal)
        }
      }
    })

  </script>
</body>
</html>
```

# 9、webpack

***\*网页中静态文件过多会有什么问题？\****

静态文件需要发起二次请求去引入，过多的时候会导致网页比较慢、卡。

***\*网页中静态文件有哪些？\****

Js：

.js，.jsx，.ts

Css：

.css，.less，.sass

图片：

.jpg，.png，.gif等等

 

***\*如何解决上述问题？\****

合并、压缩、精灵图、base64、webpack

**什么是webpack？**

WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

**为什要使用WebPack**

今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

a:模块化，让我们可以把复杂的程序细化为小的文件;

b:类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能能装换为JavaScript文件使浏览器可以识别；

c:scss，less等CSS预处理器

.........

这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别,而手动处理又是非常繁琐的，这就为WebPack类的工具的出现提供了需求。