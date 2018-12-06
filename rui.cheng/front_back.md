### When talking about Front-Back End, what exactly is that?

#### 目的

- web开发的演化，这至少是我们组的工作之一，希望大家有同样的知识背景
    - 当我们在讨论前后端分离的时候，关注点应该是什么？
    - 手头的一些工具，我们该怎么选择？
- 针对具体的代码，分享一些想法

#### web演化之路

什么是web开发？  
Web开发，一种基于B/S架构（BROWSER/SERVER）的应用软件开发技术，分为前端（用户接口）和后端（业务逻辑和数据）。  
简单介绍一下技术：
- 前端：HTML（负责框架），CSS（负责样式），JavaScript（负责交互）【载体：浏览器。HTML、CSS渲染引擎，JS解释器，还有一些别的，比如客户端数据库】
- 后端：PHP，JAVA，PYTHON等等。【载体：python解释器，java编译器，Apache】
- OSI模型：物理层，数据链路层【SDLC】，网络层【IP】，传输层【TCP、UDP】，会话层，表示层，应用层【HTTP】（共七层）

在整个互联网的发展过程中，web的演化是非常漫长的，也伴随着技术的迭代。而在我们小组，这个过程是简单并且清晰的，我从一年众多的工作中找到这样一条web演化的路线，希望能够帮助大家更加理解团队的技术架构。

##### v1.0

背景故事：  
为了验证因错施教的有效性，我们启动了过程还原项目，第一个阶段是build一个能够让实习生标记错题错因的平台。这就是小组WEB开发的beginning，我们选择python为基础的技术栈，进行flask框架的培训，然后就做出了第一个页面。

代码工具：  
Jinja2。Flask自带的页面渲染模块，它被最常使用的就是变量传递和控制结构。
```python
# hello.py
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    user = {"Xing": "Ming", "Nian": "Ling"}
    cool_here = "Thanks for reading!"
    return render_template('index.html', user=user, cool_here=cool_here)


app.run()

# index.html
<h1>Hello World!</h1>

{% for key, value in user.items() %}
  <h1>{{ key }}</h1>
  <h2>{{ value }}</h2>
{% endfor %}

{{ cool_here }}
```

看代码，很容易就能发现Jinja2模版引擎的优势，它可以让你像写python一样写HTML页面，通过python解释器转化出整个HTML的DOM树，从而实现web开发。  
在这个阶段，没有什么前后端的区分，大家都在写后端代码，所有的控制都在服务端完成，浏览器只用渲染后端提供的HTML数据就可以了。

再补充一点关于数据传递（POST， GET）的代码。

```html
<form id='user_form' name='user_form' method='post' action='{{ url_for('user.add_user') }}'>
  <tr id='hidden-tr'>
    <td><input type='text' name='username'>
    <td><input pattern='[0, 1, 2, 4]' name='permission' value=1>  <!--数据校验-->
    <td><input type='submit' class='like' value='增加&更新'>
  </tr>
</form>

<a href="{{ url_for(link_url, sid=sid) }}" name='card-link'>{{ link_name }}</a>
```

明显优势：  
- 小巧的Flask以一己之力从后到前的实现了当时业务的需求，过程还原平台有条不紊的被搭建起来;
- 技术栈简单轻巧，团队能够在开发时有余力做模型建立和教研探索。

典型问题：

1. 业务变得复杂。在一个页面内点击一个按钮，发出一个request但不reload页面，这套简单的开发模式根本无法实现类似的需求；
2. 代码难维护。随着前端业务变得复杂，为了响应变动的前端业务，后端要付出的成本急剧增大，代码职责也变得不清晰起来；
3. 交互体验。改善用户每次校验数据需要把数据传到服务器端的窘境。

##### v2.0

背景故事：  
为了响应复杂的业务需求，我们采用了ajax。JavaScript的加入，让前后端变得分明，也让代码职责重新变得清晰。页面的局部交互全部由ajax完成，部分数据校验也由ajax完成，这时候是前后端分庭抗礼的阶段。

代码工具：  
Jquery，脱颖而出的JavaScript库，依赖简单的CDN就能完美使用，它被最常使用的是事件响应和ajax交互。
```JavaScript
<script src="{{ url_for('static',filename='jquery-3.2.1.min.js') }}"></script>

<script>
$("#btn_input_form").on("click",function(){
    console.info("show a form");
    $("#div_input").show();
})

$.ajax({
    url: postUrl,
    data: formData,
    processData: false,
    contentType: "application/json",
    type: 'POST',
    success: function (data) {
        // do something
    }
})
</script>
```

明显优势:
- 满足了需求，页面的功能逐渐饱满；
- 前后分明，在某种程度上简化了后端代码的复杂度，让代码维护变得轻松。

典型问题：  
1. 有Jinja2模版的支持，jquery操作DOM起来得心应手，但是jquery以功能交互为主，前端代码很快变得十分庞大，文件变得冗长，阅读困难倍增，维护亦然；
2. 业务不断的迭代，对web页面的需求也不断拔高，jinja页面和ajax的配合逐渐不能满足日益增长的交互需求，新的技术扩展需求迫在眉睫；
3. 后端业务的不断扩展，前端相同的交互功能代码，抽象和复用都很难进行，这几乎成为了制约前端进步的主要原因。

##### v3.0

背景故事：  
为了响应复杂的业务需求，团队进一步践行SPA（single-page application），把大量的业务需求放在前端实现，进一步增加前端交互功能并且优化用户体验，推行了Vue框架。至此，组内的web开发已向前端倾斜。

代码工具：  
Vue，构建用户界面的渐进式框架，它的核心库只关注页面本身，易于上手，方便集成。
```JavaScript
<div id="demoPage">
  <h4> 请选择：你更喜欢哪一个还原过程呢？</h4>
  <form>
    <textarea v-model="comment" v-if='showOrHide' v-bind:placeholder="warnMsg"></textarea>
    <button type="button" @click="makeDecision()">我选好了！</button>
  </form>
</div>

var app = new Vue({
    el: "#demoPage",
    data: {
        showOrHide: true,
        comment: '',
        warnMsg: '请输入理由！',
    },
    methods: {
      makeDecision: function() {
          let that = this;
          if (that.chosenValue === null && that.comment === "") {
            var msgArray = ["要输入理由哦~", "请务必告知原因~"];
            that.warnMsg = msgArray[Math.floor(Math.random() * msgArray.length)];
            return
          };
          $.ajax({
              // do something
          })
      },
    },
})
```
看代码，很容易知道Vue不仅在script层做交互，它绑定了DOM，实现了数据的双向绑定。各种语法和组件也易于使用，还有简单方便的打包工具，也给一段组件化的示例。

```JavaScript
<template>
</template>

<script>
</script>

<style scoped>
</style>
```

明显优势：  
- 虚拟的DOM，双向绑定的数据，组件化的视图，简单的语法，Vue极大的满足了团队的前端需求，几乎对接了企业级的前端应用；
- 彻底分开的前后端架构，职责更清晰，开发更容易，维护更简单；
- 部署相对独立，产品体验可以快速改进。

典型问题：  
1. API不清楚。随着前后端的分离，所有链接变成API，那API的稳定和清晰就至关重要了。前端静态化、后端数据化、构架分离化，三大转变演进缓慢，导致团队配合失调。【前后端分离最困难、最关键的环节，API的制定和编写。】
    - 设计阶段：架构负责人对项目整体进行分析，讨论并确定API风格，制定API接口；
    - 开发阶段：前后端分离各自分工，后端提供数据API，并撰写文档，前端获取数据；
    - 测试阶段：（mock测试）前后端拟定测试时间，迅速调整接口。
2. SPA并不能满足所有需求，依旧存在大量多页面应用，某些design仍然需要后端配合。

##### Future

团队的web开发演化到了今天，几乎发生了沧海桑田的变化。但是，演化并没有走到尽头...
