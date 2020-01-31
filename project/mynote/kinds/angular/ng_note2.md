**目录**

***

[TOC]

#ng1

## 了解AngularJS

1、AngularJS 诞生于2009年，由Misko Hevery 等人创建，后为Google所收购。是一款优秀的前端JS框架，已经被用于Google的多款产品当中。AngularJS有着诸多特性，最为核心的是：MVC（Model–view–controller）、模块化、自动化双向数据绑定、语义化标签、依赖注入等等。

2、AngularJS 是一个 [**JavaScript**](https://baike.baidu.com/item/JavaScript)**框架**。它是一个以 JavaScript 编写的库。它可通过 \<script\> 标签添加到[HTML](https://baike.baidu.com/item/HTML) 页面。

3、AngularJS 通过 **指令** 扩展了 HTML，且通过 **表达式** 绑定数据到 HTML。

4、AngularJS 是以一个 JavaScript 文件形式发布的，可通过 script 标签添加到网页中。

5、AngularJS 这套Js框架针对于SPA(Single-Page-Application)应用。



## CLI

CLI是一个工具，可以将Angular项目中的ts代码编译成浏览器可以解析的js代码。

##查看电脑上Angular CLI的版本信息

通过命令：ng --version

##创建第一个Angular项目

ng new my-first-angular-app

如果安装过慢可以Ctrl+c打断，

然后cd 进入新建的项目：cd my-first-angular-app，

再通过cnpm install 进行安装。



通过ng serve  来启动项目

设置指定端口启动项目：

​		ng serve --port 8888



## Angular项目目录结构详解

**首层目录：**

- 【node_modules  】      第三方依赖包存放目录
- 【e2e   】              端到端的测试目录  用来做自动测试的
- 【src  】               应用源代码目录
- 【.angular-cli.json  】 Angular命令行工具的配置文件。后期可能会去修改它，引一些其他的第三方的包  比如jquery等
   -【 karma.conf.js】       karma是单元测试的执行器，karma.conf.js是karma的配置文件
- 【package.json  】      这是一个标准的npm工具的配置文件，这个文件里面列出了该应用程序所使用的第三方依赖包。实际上我们在新建项目的时候，等了半天就是在下载第三方依赖包。下载完成后会放在node_modules这个目录中，后期我们可能会修改这个文件。
- 【protractor.conf.js】  也是一个做自动化测试的配置文件
- 【README.md】           说明文件
- 【tslint.json】         是tslint的配置文件，用来定义TypeScript代码质量检查的规则，不用管它



**src目录：**

- **app目录**             包含应用的组件和模块，我们要写的代码都在这个目录

- **assets目录**            资源目录，存储静态资源的  比如图片

- **environments目录**      环境配置。Angular是支持多环境开发的，我们可以在不同的环境下（开发环境，测试环境，生产环境）共用一套代码，主要用来配置环境的

- **index.html**          整个应用的根html，程序启动就是访问这个页面

- **main.ts**             整个项目的入口点，Angular通过这个文件来启动项目

- **polyfills.ts**        主要是用来导入一些必要库，为了让Angular能正常运行在老版本下

- **styles.css**          主要是放一些全局的样式

- **tsconfig.app.json**   TypeScript编译器的配置,添加第三方依赖的时候会修改这个文件

- **tsconfig.spec.json**  不用管

- **test.ts**             也是自动化测试用的

- **typings.d.ts**        系统模块定义文件

  

**app目录（重点）**

app.component.css：样式文件

app.component.html：模板文件

app.component.spec.ts：单元测试文件

app.component.ts：是typescript脚本文件

app.module.ts：数据模型定义文件



## ngModel指令的操作

在页面中的代码：![image-20200130205259725](img\image-20200130205259725.png)

```html
<div style="text-align: center;">
  <h1>
    Welcom to {{title}}
  </h1>
  <input type="text" [(ngModel)]="name">
  <hr>
  {{name}}
</div>
```



![image-20200130205348921](img\image-20200130205348921.png)

此时运行项目不起作用，而且控制台报错：

还需要引入forms模块：

![image-20200130205528681](img\image-20200130205528681.png)

运行结果：

![image-20200130205625951](img\image-20200130205625951.png)

![image-20200130205657729](img\image-20200130205657729.png)

# ng2

## 引入BootStrap框架

1、安装bootStrap

```
通过命令：cnpm i bootstrap -s
```

2、

![Alt text](./img/a.png)

3、在app组件的模板页面添加内容

![Alt text](./img/b.png)

启动项目，页面效果：

![c](img\c.png)

```
<body>
  <app-root>Loading...</app-root>
</body>
```

**\*: angualr项目由webpack打包，在未打包完成前，页面显示Loading...，打包完成后， \<app-root\>标签生效替换里面的内容**



## 创建组件(在app根组件下创建)

步骤：

1. 在app文件夹下新建serve文件夹；

2. 在serve里面新建文件：serve.component.ts：

   ```
   import { Component } from '@angular/core';
   
   
   @Component({
       selector:"serve-app",
       templateUrl:"./serve.component.html",
       styleUrls: ['./serve.component.css']
   })
   
   export class ServeComponent {
   
     }
   ```

3. 新建文件：serve.component.html、serve.component.css

**在app.module.ts中声明ServeComponent组件**

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ServeComponent } from './serve/serve.component';

@NgModule({
  declarations: [
    AppComponent,
    ServeComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

**调用serve组件**

![e](img\e.png)



## 自动创建组件

通过命令创建serves组件

```
PS F:\month\angular\ng2\my-app> ng generate componet serves
```

或者通过缩写模式

```
PS F:\month\angular\ng2\my-app> ng g c reserve
```



通过template引入其他的组件：

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-serves',
  //templateUrl: './serves.component.html',
  template:`
      <serve-app></serve-app>
      <serve-app></serve-app>
  `,
  styleUrls: ['./serves.component.css']
})
export class ServesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

app.component.css中写css样式：

```
p{
    color:red;
}
```

**或者直接在app.component.ts的@Component修饰器中写css样式：**

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  //styleUrls: ['./app.component.css']
  styles:[
    `
      p{
        font-size:30px;
        color:pink;
        text-align:center;
      }
    `
  ]
})
export class AppComponent {
  title = 'my-app';
}

```

**selector中的写法以及html的使用方式**

通过标签

```
selector: 'serve-app'
html中的使用方式：<serve-app></serve-app>
```

## 数据绑定

插值表达式：{{index}}

属性绑定：[属性名]="变量名"

组件的html页面内容：

```
<h1>
    我是serve组件
    编号是：{{serveId}}   状态是1：{{serveStatus}} - 状态是2：{{getStatus()}}
    字符串：{{"我是字符串"}}
</h1>
```

ts中的内容：

```
export class ServeComponent {
    serveId:number = 10;
    serveStatus:string = '离线';

    getStatus(){
      return this.serveStatus;
    }
  }
```

属性绑定：

html中：

```
<button style="background-color: aquamarine;" [disabled]="allow">按钮</button>
```

ts中：

```
export class ReserveComponent implements OnInit {
  allow = true;
  constructor() {
      setTimeout(() => {
        this.allow = false;
      }, 2000);
   }

  ngOnInit() {
  }

}
```

**插值表达式与属性绑定的区别**

如果插值表达式处理的是字符串时与属性绑定没有区别，插值表达式处理的是判断，bool，叠加等等，我们只能使用属性绑定的方式。

## 模板页向业务逻辑进行事件触发

html中添加点击事件：

```
<button 
    style="background-color: aquamarine;" 
    (click)="onClick()"
    [disabled]="allow"
    >按钮</button>

<p>{{allow}}</p>
<p [innerText]="allow"></p>

<p>创建状态：{{createStatus}}</p>
```

ts中：

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-reserve',
  templateUrl: './reserve.component.html',
  styleUrls: ['./reserve.component.css']
})
export class ReserveComponent implements OnInit {
  allow = true;
  createStatus = "还没有被创建";

  constructor() {
      setTimeout(() => {
        this.allow = false;
      }, 2000);
   }

  ngOnInit() {
  }

  onClick(){
    this.createStatus = "已经创建";
  }

}
```

## 数据绑定到事件

html中：

```
<input type="text" (input)="onUpdate($event)">

{{serveName}}
```

ts中：

```
serveName = '';

onUpdate(event:any){
    //console.log(event);
    this.serveName = event.target.value;
  }
```

## 双向数据绑定

html中：

```
<input type="text" [(ngModel)]="serveName">

{{serveName}}
```

**再到app.module.ts中引入FormsModule模块**![f](img\f.png)

与数据绑定到事件的区别：

```
<input type="text" (input)="onUpdate($event)">
<input type="text" [(ngModel)]="serveName">

{{serveName}}
```

![g](img\g.png)