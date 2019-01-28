### 解题步骤的静态展示

针对**整数**的四则运算，提供竖式计算的图片展示

图片的特点：
- 竖式中每个数字的格子是36*36的框
- 字体的大小是36
- 竖式中，进位为红色，退位点是蓝色
- 竖式中，横线的宽度为0像素
- 图片的大小，字体的圆滑度，数字的间距，都可以慢慢修改

下面针对每种运算，给出若干例子。

#### 加法

有进位加法  
![47+874](https://upload-images.jianshu.io/upload_images/12134479-7dcff4a949277c3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

无进位加法  
![447+211](https://upload-images.jianshu.io/upload_images/12134479-108f4315dcf630c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 减法

有退位减法  
![447-274](https://upload-images.jianshu.io/upload_images/12134479-f2cee258ff0eff30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

无退位减法  
![447-211](https://upload-images.jianshu.io/upload_images/12134479-cb386eaf70ca4bfa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 乘法

一位数乘法  
![447*2](https://upload-images.jianshu.io/upload_images/12134479-f74e68ba18ec212b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有进位乘法  
![447*26](https://upload-images.jianshu.io/upload_images/12134479-fc1b18c80c512284.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

乘数中间有0乘法  
![447*206](https://upload-images.jianshu.io/upload_images/12134479-009285a0ce63888d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

乘数末尾有0乘法（暂未实现，需提供标准模版）  
![447*260](https://upload-images.jianshu.io/upload_images/12134479-8d2a63e7ebba49c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 除法

整除除法  
![836/4](https://upload-images.jianshu.io/upload_images/12134479-078b51fad184e5fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有余除法  
![836/40](https://upload-images.jianshu.io/upload_images/12134479-0e641e0a588d9936.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

被除数和除数末尾有0  
![83600/400](https://upload-images.jianshu.io/upload_images/12134479-2b8aae4c9ce429e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更多详情，可以去[网站](http://megrez-service.17zuoye.net/expression/)查询，或者联系开发人员rui.cheng@17zuoye.com。

### 技术方面

静态展示竖式运算过程一直是智能诊断Megrez项目的一个想法，现在基本把**整数**四则运算的过程实现了。

代码仓库：megrez/megrez-bot  
代码目录：app/img_generator/  
代码文件：brush.py, calculator.py  

下面举个例子说一下设计思路。  

例子：  
![447*26](https://upload-images.jianshu.io/upload_images/12134479-286c0da67f23b52c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

计算思路是：学生每一步运算是取两个数（两个方格）进行运算，那么只要对格子进行一定的组合运算，就可以将所有格子的数据都计算出来。

画图思路是：
- 构造框架（将数字画在格子中）   
- 画运算符   
- 画分界线

调用方法：
```python
import re
from app.img_generator.brush import Painter

expression = input("请输入你要作图的算式 >")
operator = re.split(r"\d+", expression)[1]
OPERATION_OPERATOR_MAP = {"add": "+", "times": "*", "minus": "-", "divide": "/"}
operation = [key for key, value in OPERATION_OPERATOR_MAP.items() if value == operator][0]
getattr(Painter(), operation)(expression).show()
```
