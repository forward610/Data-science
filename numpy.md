| DATA：2021-04-15     |
| -------------------- |
| **Author**：Fahaxiki |

---

[TOC]

## 1 什么是numpy

- numpy是python中基于数组对象的科学计算库。提炼关键字，numpy的三大特点：拥有n维数组对象，拥有广播功能，拥有各种科学计算API，任你调用

## 2 什么是n维数组对象

- n维数组（ndarray）对象，是一系列同类数据的集合，可以进行索引、切片、迭代操作。numpy中可以使用array函数创建数组

## 3 如何区分一维、二维、多维

- 判断一个数组是几维，主要是看它有几个轴（axis）

## 4 ndarray注意事项

① 直接操作数组的方法定义和属性在数组上面

- shape、size  数组的形状和个数
- astype          修改数组的类型
- reshape        重置形状以后的大小必须要和重置之前答大小一致
- resize          修改自身
- 这些方法和属性都只能使用ndarray对象调用
- m.shape  / m.size  / m.astype  / m.reshape  / resize((2,6,6),refcheck=False)强制修改

② 统计相关的方法可以使用数组对象调用，也可以使用numpy模块调用

- m.max()  / np.max(m)   np.max()  范围广  数组中最大值
- m.min()   /  np.min(m)
- 求百分位 ndarray对象中没有这个方法，找numpy模块

## 5 PIL处理图片、生成验证码

```python
from PIL import Image
import numpy as np

img = Image.open('test.jpg') # 打开图片
a = np.array(img)  # 将团片转为数组
# Image.fromarray(a[:,:,::-1])
# Image.fromarray(a[::-1,:,:])
# Image.fromarray(a[:,::-1,:])
# Image.fromarray(a[::-1,::-1,::-1])
Image.fromarray(a[:,::-1,::-1]) # 翻转换色
```

```python
# 生成验证码
from PIL import Image,ImageDraw,ImageFont
import numpy as np
import random.randint

img = Image.new('RGB',(160,40),color=(255,255,255))
canvas = ImageDraw.Draw(img)
img.size  # (160,40)
def random_color():
    r = random.randint(0,255)
    g = random.randint(0,255)
    b = random.randint(0,255)
    return (r,g,b)
def random_xy():
    x = random.randint(0,img.size[0])
    y = random.randint(0,img.size[1])
    return (x,y)
font = ImageFont.FreeTypeFont('DASKVILL.TTF',size=32)
for x in range(img.size[0]):
    for y in range(img.size[1]):
        canvas.point((x,y),fill=random_color())
for i in range(4):
  canvas.text((i*40,0),random.choice(string.ascii_letters+string.digits),font=font,fill=random_color())
img.save('code.png')
```

## 6 广播

- 当两个数组的形状不同时，做运算会尝试进行广播对齐，如果能对齐，就可以运算，如果不能对齐就报错
    1. 如果两个数组的后元维度相等，可以广播
    2. 如果有一个轴长，也可以广播

```python
import numpy as np

a = np.random.randint(3,10,size=(2,5))  # array([[9, 6, 5, 7, 9],[3, 4, 5, 9, 9]])
b = np.random.randint(10,15,size=(2,5))  # array([[13, 12, 13, 11, 14],[12, 14, 11, 13, 10]])
a + b  # 形状相同的两个数组可以直接相加  array([[22, 18, 18, 18, 23],[15, 18, 16, 22, 19]])
c = np.random.randint(5,10,size=6)   # array([5, 8, 9, 9, 8, 8])
a + c 
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-136-e81e582b6fa9> in <module>
----> 1 a + c
ValueError: operands could not be broadcast together with shapes (2,5) (6,) 
```

## 7 数组的级联和分割

1. concatenate 方法
    + 需要将多个数组拼接成一个元组以参数的形式转入
    + concatenate可以指定数组的拼接方向，默认是眼着0轴，也就是垂直方向拼接，可以通过修改axis参数，让数组水平方向拼接
    + concatenate对拼接的数组的形状有一定的要求。如果水平方向拼接，行数需要相等，如果是垂直方向拼接，列数需要相等
2. stack 方法
    - 会提升维度
    - 也可以指定轴进行拼接
3. hstack 方法
    - 不会提升维度
    - 水平方向级联
    - 等价于 concatenate((x,y)，axis=1)
4. vstack 方法
    - 不一定会提升维度，如果数组是一维数组会提升维二维数组
    - 如果不提升维度，等价于 concatenate((a,b))

```python

```

### 7.1 数组的分割

1. split 方法
    - 可以传入一个数字，表示切割成多少份
    - 还可以传入一维数组，用来表示分割时的下标
    - 得到的结果是一个列表，列表里的每一个元素都是一个数组
2. hsplit 方法
    - 等价于 split(axis=1)，vsplit 等价与 split(axis=0)，也等价于split

## 8 位运算相关函数

1. 按位与  bitwise_and 等价于 &
2. 按位或  bitwise_or  等将于  |
3. 按位取反   bitwise_not   等价于 ~
4. 按位异或    bitwise_xor   等价于  ^

### 8.1 算术运算

1. add将数组相加，等价于 +
2. subtract  数组相减
3. multiply   数组相乘
4. divide     数组项除
5. power     幂运算
6. mod / remainder       取余
7. reciprocal     取倒数

### 8.2 字符串相关的方法

数组通常情况下存储的都是数字类型的数据

## 9 统计相关方法

1. max / amax  / min  / amin : 获取最大值/最小值
2. argmax  /  argmin  ： 获取最大值/最小值的下标
3. unique : 用来去除重复的数据
    - axis  用来指定去重的轴，默认是None表示将数组平铺以后去重
    - return_index 返回数组第一次出现的下标
    - return_inverse  返回去重前数字在去重后的下标
    - return_counts  返回元素出现的次数
4. sum  用来求和.指定那个维度求和，就是在那个维度上降维
5. ptp  用来求最大值与最小值之间的差值
6.  percentile  求平均值
7. median  求中位数，其实就是第50个百分位
8. mean  用来求平均值
9. average  用来求加权平均值
10. var  方差
11. std   标准差

```python
import numpy as np
b = np.random.randint(5,10,size=15)
np.unique(b) # 得到的结果是一个去重以后的数组
np.unique(b,return_index=True,return_inverse=True)
# 得到的结果是一个元组，元组里有三个元素
# 第0个元素是去重以后的数组
# 第1个元素是元素首次出现的下标
# 第2个元素是去重元素在去重以后数组里对应的下标
# 第3个元素是 对应输出线的次数

m = random.randint(4,10,size=(3,5))
np.sum(m)
```

## 10 排序与条件过滤函数

1. sort 用来排序 ，默认将最内层的数据排序
2. argsort  表示排序以后数据对应的下标
3. partition  用来取n个最大或者最小值
    - 如果 n > 0表示取n个最小值放在数组的最前面
    - 如果n < 0 表示取n个最大值放在数组的最后面
4. argpartition 用来获取n个最大值或最小值的下标
5. lexsort  根据多个维度进行排序，如果总成绩排序，总程序成绩相同按照数学排序

### 10.1 过滤与查询相关方法

1. where  指定判断条件，可以获取满足条件的数据下标
    - np.where(a<10)  获取到a里所有小于10的数字的下标
    - np.where(a<10,a,a+1) 获取a里大于10的数字加1
2. argwhere  获取到满足条件的下标，会升维
3. extract  获取到满足条件所有数据

## 11 线性代数

1. transpose 矩阵转置  等价于 a.T
2. dot   矩阵的点积，对参与运算的数组形状有要求，a@b a的列要等与b的行
3. vdot   向量的点积   对参与运算的数组形状没有要求，对个数有要求
4. solve   解方程

numpy里内置的linalg库

```python
a = np.random.randint(3,10,size=(3,4))
np.transpose(a)  # 矩阵转置
a.T
b = np.random.randint(2,5,size=(4,3))
np.dot(a,b)

x = np.random.randint(3,10,size=(5,5))
np.linalg.inv(x)

-- 解方程
x + y + z = 6
2x + 3y + z = 11
5x + 3y + 4z = 11
x = np.array([
    [1,1,1],
    [2,3,1],
    [5,-3,4]
])
y = np.array([6,11,11])
np.linalg.solve(x,y)
```

## 12 IO操作

可以将一个ndarray对象写入到一个文件里，也可以加载一个文件里的ndarray对象

```python
import numpy as np

a = np.random.randint(3,10,size=(3,5))
np.save('a',a)  # 会将数组以二进制的形式存入到以  .npy  结尾的文件里
np.load('a.npy')  # 加载一个文件里的ndarray
np.savetxt('a.txt',a,fmt='%d',delimiter=',') # 以普通文本的形式保存数组
```



