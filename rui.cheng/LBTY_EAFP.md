- Python generally recommends the EAFP policy, but must then proliferate utility functions like dic.get(key, None) to enable this.

- I don't think that dict.get() would be unnecessary once we have except expressions, and I disagree with the position that EAFP is better than LBYL, or "generally recommended" by Python.

在PEP463中，Python推出了try-catch的方式来捕捉错误，于是核心开发者和Python创造者有了上述的讨论，那问题的焦点在哪呢？

`if … else …` is used for checking a condition and choosing which code to execute  
`try … catch …` is for running code and handling an error/exception occuring

LBYL ( look before you leap), when you choose an `if … else …`, this style is referred to as defensive coding  
EAFP (easier to ask forgiveness than permission), is the alternative way

在目前代码的flow control中，我们都使用`if else`的方式， 而其实`try catch`也是非常常见的方式，甚至更能避免race-conditions，也能够在简化一些代码的层次结构（where the ability to handle an issue is far removed from where the issue arose）。

> One of the biggest misconceptions about exceptions is that they are for “exceptional conditions.” The reality is that they are for communicating error conditions. From a framework design perspective, there is no such thing as an “exceptional condition”. Whether a condition is exceptional or not depends on the context of usage, --- but reusable libraries rarely know how they will be used. For example, OutOfMemoryException might be exceptional for a simple data entry application; it’s not so exceptional for applications doing their own memory management (e.g. SQL server). In other words, one man’s exceptional condition is another man’s chronic condition.

举个例子：
- 我们的主站代码中，有一个first_procedure的属性，return None，如果换成raise InvalidDocError，会有效改善阅读体验

References：
- [Good practice to use try-except-else in Python](https://stackoverflow.com/questions/16138232/is-it-a-good-practice-to-use-try-except-else-in-python/16138864#16138864)

- [PEP 463](https://www.python.org/dev/peps/pep-0463/)
