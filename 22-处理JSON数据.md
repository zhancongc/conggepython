# python处理JSON数据

> 相同的数据，json格式相比与xml格式的占用空间小，解析速度更快。因此，json数据应用越来越广泛。python内置的字典格式，和json格式基本相同，可以看出，python天生就对json数据比较友好。今天我们来介绍python对json数据的处理。

## JSON数据与python的字典

对于一个字典，我们可以用`json.dumps()`方法来将其转化为字符串。

```python
>>> import json
>>> data = {
···     'Chinese': 90,
···     'Mathematics': 85,
···     'English': 94
··· }
>>> string = json.dumps(data)
>>> print(string)
'{"Chinese": 90, "Mathematics": 85, "English": 94}'

```

不难发现，这里的string不就是json数据嘛。所以，字典转json数据用`json.dumps()`方法实现。相反，json转字典用`json.loads()`方法。

```python
>>> import json
>>> string = '{"Chinese": 90, "Mathematics": 85, "English": 94}'
>>> data = json.loads(string)
>>> print(data)
{"Chinese": 90, "Mathematics": 85, "English": 94}	
```

json数据和python的字典语法几乎完全相同，差别仅仅在：python中的True, False, None，转化为json之后会变成：true, false, null。

```python
>>> import json
>>> characteristic = {
···     'friendly': False,
···     'honset': True,
···     'wealthy': None
··· }
>>> characteristic_json = json.dumps(characteristic)
>>> print(characteristic_json)
'{"friendly": false, "honset": true, "wealthy": null}'

```

直接打印characteristic_json，会得到没有缩进的一行字符串，并不是很美观。`json.dumps()`方法还提供了一个indent参数，来控制缩进，实现美观的输出。其中，`indent=4`表示缩进长度为4个空格。

```python
# 添加参数indent，实现带缩进的清晰输出
>>> print(json.dumps(characteristic, indent=4))
{
    "friendly": false,
    "honset": true,
    "wealthy": null
}
```

##  JSON文件

```python
city_data = {
    'code': 320000,
    'name': 'Jiangsu',
    'subordinate': [
        {
            'code': 320100,
            'name': 'Nanjing'
        },
        {
            'code': 320200,
            'name': 'wuxi'
        },
        {
            'code': 320300,
            'name': 'Xuzhou'
        }
    ]
}
```

对于上面的python字典，也可以用`json.dump()`方法将其写入到文件中。同样地，使用`json.load()`方法也可以从类文件对象中读取数据。

```python
# 处理json数据需要用到json库
import json
# 将字典city_data写入到文件city.json中
with open('city.json', 'w') as f:
    json.dump(city_data, f)
# 从city.json中读取json数据并转化为python字典
with open('city.json', 'r') as f:
	data = json.load(f)
# 数据其实没有变
>>> city_data == data
True
```

注意：`json.dump()`和`json.dumps()`方法有着细微但重要的区别，前者将字典转化为类文件对象，因此可以直接写入文件；后者将字典转化为字符串，可以读取其中的数据，但是不能直接写入文件。

## 总结

1. `json.dumps()`方法将字典转化为字符串（json数据），`json.loads()`则相反
2. `json.dump()`方法将字典转化为类文件对象，`json.load()`方法将类文件对象转化为字典

