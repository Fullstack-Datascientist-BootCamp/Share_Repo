##### 1 运维能力

说到运维，就是一种管理的能力，管理计算机系统，管理数据库，管理应用配置，让一切都流程化、标准化并自动化。

- 计算系统。基于kubernetes写dockerfile，基于成熟的docker系统创建、销毁容器。DOCKER让系统自动化
任何系统问题都能用“重启”的方式解决，如果不行，那就重启两次。云是大势，所以只要我们能弄出最基本的系统image，比如macOS、阿里云，那么作为程序员的我们就能够一展身手。
当然，理解操作系统是很有必要的，不要浪费或过度使用机器性能，注重sql语句的优化，进程的使用
- 数据库。DBA让数据库自动化
- 应用配置。CMDB让应用自动化
- 生产环境。我们要学会利用以上，必要时候开发以上，学会起docker，学会和DBA沟通，学会起CMDB，以及学会管理自己的生产环境！
  - 基本cmd命令，比如yum，mkdir，touch，cd，rm，>，grep，ps，top，w，nohup，set，crontab等等；
  - 基本编辑器vim，会配置使用vim（用vundle来管理插件），或者基本ide atom；
  - 基本解释器，python环境管理pipenv，`pipenv install`，`pipenv shell`；
  - 基本代码管理git，pull，push，checkout，rebase
  - 基本数据库管理，mongo，mysql, els的启用，migrate，export，import
  - 基本业务管理，能够预估开发feature的时间，能够管理开发的轻重缓急，能够对业务细节反复推敲
  - 基本个人管理，能够健康，能够“有爱”

就个人或小组开发而言，掌握以上应该能让你非常舒服地做一个开发工程师，所谓工欲善其事，必先利其器。

---
##### 2 PYTHON能力

说到PYTHON，很多人都会写，语言很简单，但工程师之间工资又各有不同，这是为什么呢？为了回答这个问题，不妨上拉勾网搜索了一下“金主”们对工程师的要求。
<sub>搜索词为'python'，筛选为工资‘25k-50k’以及‘50k以上’,我随机选取了第一页的两个公司</sub>
![初级玩家](https://upload-images.jianshu.io/upload_images/12134479-f64712c6ffac53f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![中级玩家](https://upload-images.jianshu.io/upload_images/12134479-617815c44aae4a68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可是看出，对于‘初级玩家’公司主要看你“会不会”，也就是0和1的区别；而对于‘中级玩家’公司主要看中你“会不会分析，能不能优化”，也就是1和10的区别；至于‘高级玩家’，我想只会PYTHON是不行的。那么我的这篇文章，一方面总结一下我半年的学习成果，希望能够把知识从0到1真正落到实处，便于以后从1到10不断进步；另一方面，借着分享交流的互联网，希望各位同行能够指出我工作学习中的短板，帮助我不断改进。

##### 2.1 Clean Code

我在上篇文章中推荐在HackerRank做完全部python习题，并且在LeetCode上继续训练。不断刷题能极大的增强我们写单个函数、算法的能力，训练我们的思维能力，但对我们写"工程项目"却鲜有帮助。因为每一题的context都不一样，容易养成重复造轮子、代码不整洁的坏习惯，在我入职的第一个月，因为坏习惯被反复批评，之后我仔细读了《Clean Code》一书，情况有所好转。所以，请务必注意代码的测试、抽象层、命名规范、参数、Exception等。
训练方法：《Clean Code》，要有好代码的smell
训练成果：厉害的程序员写出机器能读懂的代码，好的程序员写出人能读懂的代码。
```python3
# 保龄球积分，从第0轮到第10轮，已知每一轮的击倒情况，求总分
def score_frame(frame=10):
    score = 0
    ordinal_ball = 0
    for current_frame in range(frame):
        if is_strike(ordinal_ball):
            score += 10 + next_two_balls_for_strike()
            ordinal_ball += 1
        elif is_spare(ordinal_ball):
            score += 10 + next_ball_for_spare()
            ordinal_ball += 2
        else:
            score += two_balls_in_frame()
            ordinal_ball += 2
    return score
```



![1](https://upload-images.jianshu.io/upload_images/12134479-c9f6fac31a6cc038.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)
*Clean code is simple and direct. Clean code reads like well-written prose. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control.*

##### 2.2 Flask

整洁代码是基础中的基础，接下来掌握的是web框架。要想熟练使用Flask，知识上要求对网络会话层、表示层和应用层有了解，从Client端输入URI并发送请求的一瞬间，网络通过自上而下的OSI结构，传输层（TCP）--- 网络层（IP） --- 数据链路层 --- 物理层， 路由（识别IP），然后，Server端与之建立TCP连接，拿到request对象，梳理业务逻辑，返回response对象，经过同样的一系列过程到Client端接受数据（如果是浏览器则会渲染页面）。这背后所有的事情，就是一个web工程师的工作所在。
训练方法：《Flask Web开发，基于python的web开发实战》，上篇文章已经推荐过了
训练成果：能够独立编写并部署一个满足业务需求的web应用
![FLASK框架可以简单至此，但面对更大的需求，还望多思量。](https://upload-images.jianshu.io/upload_images/12134479-409b3b1a48a87ac9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```python3
from flask import Flask, request, jsonify
from models import get_question_analysis

app = Flask(__name__)

@app.route('/', methods=['GET'])
def index():
    return 'hello world'

@app.route('/api', methods=['POST'])
def api():
    data = request.get_json()
    question = data['question']
    question_analysis = get_question_analysis(question)
    return jsonify(question_analysis)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=False)
```

##### 2.3 AioHttp

一步通，则步步通，能够学会一个框架，其他的框架就不难了。Flask的底层是werkzeuk，基于CGI（通用网关接口，Common Gateway Interface）的WSGI在业务受到了异步的挑战，ASGI应运而生。AioHttp的框架，支持python asycio，能极大的提高API的性能（与flask框架下API性能的差异，请移步我的[测试文章](https://www.jianshu.com/p/eda9fd632764)）。
训练方法：[AioHttp Git](https://github.com/aio-libs/aiohttp/)，[Asyncio Doc](https://docs.python.org/3/library/asyncio.html)，永远不要忘记源码和doc
训练成果：能够独立编写并部署一个满足业务需求的web应用，理解并掌握python的异步语法
```python3
from aiohttp import web
from models import get_question_analysis

async def index(request):
    return web.Response(text='Hello World')

async def api(request):
    data = await request.json()
    question = data['question']
    question_analysis = await get_question_analysis(question)
    return web.json_response(question_analysis)

app = web.Application()
app.add_routes([web.get('/', index),
                web.post('/api', api)])

if __name__ == '__main__':
    web.run_app(app, host='0.0.0.0', port=5000)
```
> Parallelism introduces new challenges in writing correct code, particularly in the presence of shared, mutable state.

##### 2.4 MONGO

对于一个助理工程师，不懂框架，你也能形成生产力，可以做一个写数据库增删改查的cool boy。如果一个工程师不会操作数据库，那就连“删库跑路”的“黑暗森林威慑”都建立不起来...所以无论怎样，这个得会。我们有两种操作mongodb的方式，一种是ODM（Object-Document Mapper），如mongoengine；另一种则是超有名的pymongo。
训练方法：MongoEngine文档，PyMongo文档
训练成果：熟练对mongo表进行增删改查，能够对mongo表查询进行一定程度的优化
```python3
# 更改user_info这张表中的user_id为‘test’的年龄为22
# mongoengine
from mongoengine import Document, connect
from config import MONGO_URI

connect(host=MONGO_URI, alias='default')

class UserInfo(Document):
    user_id = StringField(required=True)
    age = IntField()

UserInfo.objects.get(user_id='test').update(age=22)


# pymongo
from config import MONGO_URI, DB
from pymongo import MongoClient

client = MongoClient(MONGO_URI)
collection = client[DB]['user_info']

collection.update({'user_id': 'test'}, {'age': 22})
```
简单分析一下二者的优缺点：
- mongoengine允许我们定义一套模式，然后把所有的值都匹配到我们定义的schema上，这种schema更加清楚和明确，个人感觉比字典更容易操作，用dot notation更像OOP，而pymong返回的是一个dict，操作时困难的多，很多时候有点丑
- mongoengine相当于在你和pymongo之间加了map层，所以会对性能有微乎其微的影响，在进行很简单的查询时显得有些没有必要

##### 2.5 MYSQL

noSQL和SQL的取舍总是要不断斟酌。操作mysql我们也有两种方式，ORM的sqlalchemy，以及pymysql
训练方法：SqlAlchemy文档，PyMySql文档
训练成果：熟练对mysql表进行增删改查，能够对mysql表查询进行一定程度的优化
```python3
# 向user_info这张表中存入一条姓名为‘test’，年龄为22的数据
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from config import MYSQL_URI

Base = declarative_base()
engine = create_engine(MYSQL_URI, echo=False)
Session = sessionmaker(bind=engine)
session = Session()

class UserInfo(Base):
    __tablename__ = 'user_info'
    user_id = Column(String(80), nullable=False)
    age = Column(Integer(), nullable=True)

session.add(UserInfo('test', 22))
session.commit()
```
ORM的用途非常非常广，使用起来很优雅，但如同flask框架一般，深入探究它的使用方法和场景需要大量的篇幅，会在以后的文章中继续分享，希望感兴趣的同学自行探索，也可以在评论区有更多交流。

##### 2.6 Abstraction

> Computer programs consist of instructions to either: Compute some value Or Carry out some action

有了框架和数据库，剩下的就是“操作”了，也就是业务逻辑的编写。如果使用命令式编程，实现业务不是一件困难的事情，那么代码的区别就彰显了工程师们的抽象水平。百多行的函数；没有层次感的代码；命名不规范的代码；概率编程；google编程，等等，不一而足。优秀的工程师，对抽象一定有自己的理解！
训练方法：多想，多看
训练成果：至少能够从“只看到了”函数，到“看到了”项目，从细节到系统
```python3
# 表示有理数，并计算其平方
# 表示方法
def rational(n, d):
    return [n, d]

def numerator(x):
    return x[0]

def denominator(x):
    return x[1]

# 操作方法
def square_rational(x):
    return mul_rational(x, x)

def square_rational_violation_once(x):
    return rational(numerator(x) * numerator(x), denominator(x) * denominator(x))

def square_rational_violation_twice(x):
    return [x[0] * x[0], x[1] * x[1]]

# 表示方法 2
def rational(n, d):
    def get(index):
        if index == 0:
            return n
        elif index == 1:
            return d
    return get

def numerator(x):
    return x(0)

def denominator(x):
    return x(1)
```
![Isolate the parts of a program that deal with how data are represendted from the parts that deal with how data are manipulated](https://upload-images.jianshu.io/upload_images/12134479-172d72c886138949.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1600)
用了一个简单的例子来解释表示层和操作层（完全不一样的表示层，但是操作层却完全一样），庞大的系统必然存在着细小的抽象。我们只要能够掌握抽象的方法，再配合leetcode中的算法知识，那么所有的业务逻辑必然迎刃而解。

##### 2.7 lib/site-packages

框架 -> 数据库 -> 抽象，我把技术内容分成了这三块，那其实还有一些边角料。python为人称道的就是大量好用的包，所以python工程师经常被说成调包的。在工作中当然也会直接调包，比如smtp和beautifulsoup，这两个太常见了。
```python3
import requests
from bs4 import BeautifulSoup

url_head = 'http://www.jianshu.com/u/1f167239855b'
HTML = requests.get(url_head).text
soup = BeautifulSoup(HTML, 'lxml')
soup.find_all()
```

##### 2.8 widgets

最后的最后，在项目开发的过程中，一定会和公司业务有联系，也一定会发生很多bug，更会有一些奇奇怪怪的需求。为了应对各种各样的事情，python工程师集成了大量好用的工具，比如elastic-search，sentry，kafka， memcache等等，这种Client/Server的构架很好用，也是微服务的雏形。
![The client/server model is appropriate for service-oriented situations.](https://upload-images.jianshu.io/upload_images/12134479-ca96f2ae376e2466.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2.9 behave && selenium

OK，做完所有工作，临近上线，我的PM一定会在旁边大喊三声：测试！测试！测试！自动化测试是所有稳定系统的标配，unittests和doc tests不必多提，这里介绍一种BDD工具，behave和selenium。
```python3
功能: 自动测试
    场景: 页面点击
        假如 我在登陆页面
        当 输入账号、密码并点击登陆
        那么 我在个人主页

from behave import given, when, then
from selenium import webdiver

driver = webdrivear.Chrome(driver_path, chrome_options=chrome_options)

@given('我在登陆页面')
def login_in(context):
    driver.get(login_url)
    assert is_at_page(LOGIN_PAGE)

@when('输入账号、密码并点击登陆')
def send_key(context):
    driver.find_element_by_id(ACCOUNT).send_keys(KEYS)
    dirver.find_element_by_id(CONFIRM).click()

@then('我在个人主页')
    assert is_at_page(HOME_PAGE)
```
如果你觉得测试没有必要，那你一定没有写过测试！

##### 2.10 CI && CD

OK，顺利通过测试，合并到master，此时应该有一套能够自动集成、自动部署的机制，用python起一个监控master分支的服务，一出现合并操作，就使用ssh协议重启原服务。

至此，整套python web开发后端流程的技术要点都清晰了，希望以后能够继续成长、分享，也希望阅读到这里的伙伴多多交流、给予指导。

---
##### 3 前端能力

互联网行业注重T型人才的培养，所谓的T型人才就是横向了解，纵向发展，所以web开发往往看重全栈的能力。

- HTML。标记性语言，就用jinja模版吧。
```python3
{% import "bootstrap/wtf.html" as wtf %}
{{ wtf.quick_form(form) }}
```

- CSS。样式语言，就用bootstrap吧。
`{% extends 'bootstrap/base.html %}`

- JS。动态交互，就用ajax吧。
```
$("#botton").on("click",function(){
    $.ajax({
            url: 'http://www.baidu.com',
            type: 'GET',
            success: function(data){
            };
        })
    })
```
