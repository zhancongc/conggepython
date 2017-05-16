# while循环

> 当我们需要重复执行某种操作的时候，就需要使用循环语句。上期我们讲到了，可以依托一个序列来实现for循环。可是，我们未必总是能找到一个序列来辅助实现循环。譬如，当我们只知道一个模糊的循环终止条件的时候，就只能使用while循环语句。

##while循环语句

在while循环中，while是关键词，表示一个循环的开始；表达式是循环执行的前提条件，当这个表达式的逻辑值为True的时候，代码块会继续执行。while循环的一般形式是如下：

```python
# 基本形式
while 表达式:
    代码块

# 当前n项和大于100时，打印此时的n
summation, index, numbers = 0, 0, range(0,100)
while summation <= 100:
    summation = summation + numbers[index]
    index = index + 1
print(index-1)
```
上面的例子实现的是：计算1,2,3...n的和(n <= 100)，当这个和大于100，打印对应的n。

>python支持多个参数同时赋值，`summation, index, numbers = 0, 0, range(0,100)`，可以看作是把元组`(0, 0, range(0,100))`赋值给列表`[summation, index, numbers]`。相应地，summation被赋值为0，index被赋值为0，numbers被赋值为range(0,100)。

while循环既可以在有序列的情况下使用，也可以在没有序列的情况下使用。并且while语句中，根据表达式的布尔值来决定代码块是否执行，相当于自带了一个if语句。由此看出，while循环语句 >= for循环语句 + if条件语句。

如果有序列的话，尽量使用for循环，它依托一个确定的序列，不容易出错。

## 拓展

循环终止的条件只有两个，一是：表达式的逻辑值变为False；二是：执行到break语句；三是：在函数中执行到return语句。return语句用于函数，它给出了函数的返回结果，讲到函数时聪哥会具体介绍。

> 永远无法终止的循环叫做死循环，死循环无益于问题的解决，还会占用大量计算机资源，应该尽量避免。代码执行过程中，使用快捷键Ctrl+C可以终止死循环。

有一种while True + if + break的用法，就是利用了break语句跳出死循环。这种用法的好处是，将判定与循环分离，while专门用作循环，if来做判断，使得逻辑结构更加清晰，更容易理解。如果while语句十分熟悉，也建议直接使用while语句。

```python
# 打印第一个不及格的科目的分数
scores = [89, 75, 83, 66, 92, 97,78, 57, 84]
index = 0
while True:
    if scores[index] < 60:
        print(scores[index])
        break
    index = index + 1
```

类似于for-else语句，while语句也有else用法。在while循环中的表达式的布尔值为False时，执行else部分的代码块。

```python
# 基本形式
while 表达式:
    代码块
else:
    代码块
# 打印第一个不及格的科目的分数
scores = [89, 75, 83, 66, 92, 97,78, 57, 84]
index = 0
while scores[index] >= 60:
    index = index + 1
else:
    print(scores[index])
```

## 总结

* while循环语句，不仅仅是一个for循环与一个if语句的合体
* 尽量避免死循环，遇到死循环使用Ctrl+C终止
* while True + if + break的用法，使逻辑结构更加清晰
* while-else语句，可以包含循环加后处理

## 思考

本期思考题：

```python

# 自然数集中，由小到大，
# 2是第一个质数，3是第二个质数，5是第三个质数，
# 依此类推，11是第五个质数。
# 请编写一个程序，找出第100个质数

```

上期思考题：

![img](http://mmbiz.qpic.cn/mmbiz_png/yQOGaRouhVp0HzEtA6r7wULK3UCPWViaoykQGRdQAb1gdTNGcxeYCtdavOWaSN6UcelyjURXCyibyRIMoiaKhC3Zg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

上期思考题解答：

```python
# 判定质数
# 思路：以35为例，只要找到一个数可以被35整除，
#      并且这个数不是1也不是35，那么35就不是质数
number = 35
for index in range(2,number):
    if number%index == 0 and index != number:
        print("It is not a prime number.")
        break
    index = index + 1
else:
    print("It is a prime number.")
```

```python
# 找出1~100之间的所有质数
# 思路：2~100之间，每个数都判定一下是否是质数
#      如果是质数则输出
for number in range(2,100):
    # num统计了除1和本身外，能被整除的数的个数
    # 如果个数为0，就是质数
    num = 0
    for index in range(2,number):
        if number%index == 0:
            num = num + 1
    if num == 0:
        print(number)
```

































# python的while循环语句



























