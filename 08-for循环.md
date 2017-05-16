## for循环

> 当我们需要反复做一种操作的时候，为了节省时间，提高效率，需要用到循环语句来简化编程。

## for循环基本形式

当循环的条件和某个序列有关时，就适合使用for循环。for循环中，for和in都是python保留的关键字，for是循环的标识，in是成员运算符。代码块中的内容是需要循环执行的。

```python
# for循环的基本形式
for 索引 in 序列:
    代码块

# 示例
fruits = ['apple', 'banana', 'grape', 'orange', 'pear']
for index in fruits:
    print(index)
```

上面的例子，实现的功能是：按顺序打印列表fruits中的元素。其中的index是自定义的一个变量，将index改成其它的名字，也不会影响程序的输出（python保留的关键词除外）。循环时依次将fruits中的元素赋值给index，每赋值一次执行一次代码块。

字符串之所以是序列，是因为它的每个字符都可以看作是一个元素，并且和列表一样自带索引。实际上，字符串可以看作是一个列表，这个列表的元素都是一个字符。对于字典而言，它的键是也是一个序列。因此，字符串和字典都可以实现for循环。

```python
# 字符串
sign = "Welcome to congge python!"
for index in sign:
    print(index)
# 字典
scores = {'Chinese':90, 'Mathematics':85, 'English':93}
for index in scores:
    print(scores[index])
```

## 高级应用

> 函数range()，可以返回一个特定的序列，描述这个序列的是三个参数：start，stop， step，分别表示开始序号、结束序号和步长。
>
> 函数len()，可以获取一个字符串、元组、列表和字典的长度。比如，列表名为fruits，则长度是len(fruits)。

配合函数range()和len()，生成一个索引元组，便于自定义循环的范围。

```python
fruits = ['apple', 'banana', 'grape', 'orange', 'pear']
# 输出序号1到3的所有fruits元素
for index in range(1,4):
    print(index)
# 输出序号2开始的所有fruits元素
for index in range(2,len(fruits)):
    print(index)
```

上面两个循环语句的输出结果分别是：banana, grape, orange 和 grape, orange, pear。看懂了吗？

配合if语句，实现更高级的操作。比如：统计"Welcome to congge python!"中的字母"o"的个数。

```python
sign = "Welcome to congge python!"
number = 0
for index in sign:
    if index == "o":
        number = number + 1
    print(number)
```

> break语句，用于跳出本级循环。循环中一旦执行到break语句，循环立刻中止。
>
> continue语句，用于跳过本次操作，继续循环。

配合break和continue语句，实现循环的高级控制。比如：统计各门科目的平均分，如果没有分数则循环中止。

```python
scores = {'Chinese':90, 'Mathematics':None, 'English':93}
# 总分
total = 0
for index in scores:
    if scores[index] is None:
        break
    total = total + scores[index]
# 平均分 = 总分/科目数
average = total/len(scores)
print(scores[index])
```

此时，total的值应该是90，而average的值是错的。大家可以尝试将`scores['Mathematics']`的值改成85，这样就能正常地计算出平均分。

对于此例，还有一种for-else循环可以使用。这时的for循环和正常的for循环没有任何区别，区别在于：当循环没有没有异常地执行完毕时，再执行else部分的代码块。到目前为止，这种异常仅限于break语句。

```python
scores = {'Chinese':90, 'Mathematics':None, 'English':93}
# 总分
total = 0
for index in scores:
    # 如果这门课没有分，则循环终止
    if scores[index] is None:
        break
    total = total + scores[index]
else:
	average = total/len(scores)
	print(scores[index])
```

一般来说，else部分用于与循环相关的后处理。

## 总结

1. for循环，依托某个序列，重复地实现某个操作
2. 使用函数range()，自定义循环范围
3. break语句和continue语句，控制循环的执行
4. for-else语句，组合了循环和后续操作

## 思考

本期思考：

```python
# 数学中我们学过质数的概念
# 质数的特征是：只能被1和它本身整除
# 例如：2是质数，它只能被1和2整除
# 4不是质数，它可以被1、2和4整除
# 1.请编写一个程序判定一个数是否是质数
# 2.请编写一个程序找出1~100之间的所有质数

# 1.判定质数
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

# 2.找出1~100之间的所有质数
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

**扫一扫这个二维码，关注公众号：聪哥python，获取最新python3基础教程**
![聪哥python](http://opa63tcx6.bkt.clouddn.com/qrcode%E8%81%AA%E5%93%A5python.jpg)
