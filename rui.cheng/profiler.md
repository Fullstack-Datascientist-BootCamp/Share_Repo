### 诊断python代码

> A good code is a fast code, not a short code.    ---- HackerRank.coderator

#### Problem itself

工作中，BOT智能诊断时间过长，FLASK的MVC架构无法满足QPS，以及最近发现协程web框架也难以支持稍显复杂的业务。能够看到，逻辑和服务都会碰到性能上的问题，为了优化，我们可以用递归替代循环，用并发计算替代单进程单线程，用迭代替代枚举等等。不同的场景，不同的方案，所以，为了更好的选择方案，首先要准确判断问题，诊断python代码就是这次分享的主题。

#### Tool itself

Python内置了一些模块来分析代码，`profile.py`，`cProfile.py`，`pstats.py`。

- Profile => Profile是一系列统计数据，用来描述当一个程序执行时各个部分被调用的次数以及花销的时间  
Python有两个模块做这件事情，`profile.py`和`cProfile.py`。
    - 相同点：
        - 完全相同的接口
        - 核心返回数据：ncalls, cumtime
            - Call Count, 被调用次数。通常被用来发现代码中的bug（surprising count，过大or过小）, 和嵌套for-loop和recursive（high count）
            - Cumulative time，总共花销时间。通常被用来诊断高层次的算法错误  

    - 不同点：
        - `cProfile.py`使用了C语言扩展，在处理大程序时更快更准
        - `profile.py`是纯Python包，在自定义扩展时更简单


- Format => 对Profile进行格式整理，形成报告的过程  
`pstats.py`模块提供了一些strip，filter和sort的方法，可以让我们得到想要的信息。

- Visualize => 对整理过后的文件进行可视化展示  
有大量的工具可以对`.prof`文件做可视化，这里用snakeviz做简单的示范。

#### Demo itself

首先有一段用于诊断的代码，用常见的迭代器作为例子。  
```
# demo.py

def for_square(n):
    new_list = []
    for i in range(0, n):
        if i % 2 == 0:
            new_list.append(n**2)
    return new_list


def list_comp_square(n):
    return [i**2 for i in range(0, n) if i % 2 == 0]
```
接着用`timeit`模块先看一下两个函数的性能区别  
```
import timeit

print("Time taken by For Loop: {}".format(timeit.timeit('for_square(50)', 'from demo import for_square')))
print("Time taken by List Comprehension: {}".format(timeit.timeit('list_comp_square(50)', 'from demo import list_comp_square')))
```
然后使用`cProfile`模块来看一下具体的差别  
```
python -m cProfile demo.py
```
![cProfile结果](https://upload-images.jianshu.io/upload_images/12134479-4fe1276d0a583958.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为`profile`模块的接口是相同的，所以我们用这样的command也是可以的  
```
python -m profile demo.py
```
最后为了可视化demo，我们把profile存成文件  
```
python -m cProfile -o demo.prof demo.py
```
就可以用`snakeviz`命令就可以打开看一看了
```
snakeviz demo.prof
```
![可视化图，中间紫色部分是list.append方法执行花销的时间](https://upload-images.jianshu.io/upload_images/12134479-925f64591698de89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### Example itself

在简单了解了profile之后，我们可以来看一下实际的例子。

1. Flask  
Flask是一款基于werkzeug的web框架，werkzeug有一个自带的ProfilerMiddleware，可以让我们非常简单的做性能诊断。
```
# 假设我们已经拥有了一个app实例,那我们只需要对这个app进行一个简单的包裹,并传入profile_dir参数
from werkzeug.contrib.profiler import ProfilerMiddleware
app = ProfilerMiddleware(app, profile_dir="demo.prof")
```

2. AioHttp  
AioHttp是我组项目中选用的concurrent web框架，拥有还不错的开源生态环境。面对一个较新的开源框架，当碰到一个问题时，我们有两个选择：寻找合适的组件||自己开发解决。
```
# 自行开发
import cProfile
def profile_middleware(request, handler):
    pr = cProfile.Profile()
    pr.enable()
    resp = handler(request)
    pr.disable()
    pr.dump_stats("demo.prof")
    return resp
# 合适的组件，aiohttp_debugtoolbar，这个插件中自带一个ProfilePanel
import aiohttp_debugtoolbar
aiohttp_debugtoolbar.setup(_app)
```

#### Ghost in Heart

- 拿到了prof文件，看不懂？典型的例子，在协程框架中，开销最高的是一个selector对象，但是我甚至没见过这个对象。
- 发现了问题，那解决问题的最佳实践是什么？计算机领域的先贤把我目前所能遇到的工程问题全都解决了，我只需要捡到合适的牙慧就OK了。

#### Reference itself

- [Profiler Python Doc](https://docs.python.org/3/library/profile.html#module-pstats)
- [Python Performance Optimization](https://stackabuse.com/python-performance-optimization/)
- [Python Source Code](https://github.com/python/cpython/tree/3.7/Lib)
- [Parallel Computing](http://composingprograms.com/pages/48-parallel-computing.html)
- [Werkzeug Profiler](https://werkzeug.palletsprojects.com/en/0.14.x/contrib/profiler/)
