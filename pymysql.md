# python操作MySQL数据库

前面大家虽然学了很多python的基础知识，但是并没有什么实际可用的技能。现在聪哥为大家带来一篇技术干货，python操作mysql数据库，希望大家能够喜欢。看懂本文，需要一定的SQL基础。

## 准备

首先你要确定你有一个可以连接的数据库，以及数据库的一个账户和密码。

然后命令行安装pymysql包，它是python操作数据库的核心工具。

```shell
pip install pymysql 
```

然后就可以编写代码连接数据库。

```python
import pymysql

# 这些信息一般要从配置文件中导入，写在此处是不妥的
# 此处的信息仅供参考，要根据实际情况来定
host = 'localhost'
username = 'root'
password = '12345678'
# 这是database的名称，MySQL中有多个database
db_name = 'test'

# 创建一个连接对象
connection = pymysql.connect(
	host=host, user=username, password=password, charset='utf8mb4', db=db_name
)
```

## 编写SQL语句

在test中创建一个名叫person的表

```python
# 创建person表
create_table_sql = """
CREATE TABLE person(
id INT AUTO_INCREMENT PRIMARY KEY,
username VARCHAR(255) UNIQUE ,
nickname VARCHAR(255) NOT NULL ,
birthday DATE
)
"""

# 删除表用drop语句
drop_table_sql = """
DROP TABLE person
"""
```

编写SQL语句，实现增删查改的功能。

```python
insert_table_sql = """
INSERT INTO person(username,nickname,birthday)
 VALUES('{username}','{nickname}','{birthday}')
"""

query_table_sql = """
SELECT id,username,nickname,birthday
FROM person
"""

update_table_sql = """
UPDATE person SET nickname = "Jack" WHERE id = 1
"""

delete_table_sql = """
DELETE FROM person WHERE id = 1
"""
```

## 执行SQL

下面来介绍如何执行这些SQL语句

```python
# 这是一个日期的库
import datetime
# 操作数据库容易出问题，使用try可以捕捉异常
try:
    # 创建一个游标对象，该对象有execute()方法支持SQL
    with connection.cursor() as cursor:
        print('--------------新建表--------------')
        cursor.execute(create_table_sql)
        # 每个操作完成后必须要执行commit()方法，否则不会生效
        connection.commit()

        print('--------------插入数据--------------')
        cursor.execute(
            insert_table_sql.format(
                username='杰克', nickname='jackson', birthday=datetime.date.today()
            )
        )
        cursor.execute(
            insert_table_sql.format(
                username='zhang3', nickname='张三', birthday=datetime.date.today()
            )
        )
        connection.commit()

        print('--------------查询数据--------------')
        cursor.execute(query_table_sql)
        results = cursor.fetchall()
        # 按列打印出查询结果
        print(f'id\tname\tnickname\tbirthday')
        for row in results:
            print(row[0], row[1], row[2], row[3], sep='\t')

        print('--------------删除数据--------------')
        cursor.execute(delete_table_sql)
        connection.commit()

        print('---------------删除表---------------')
        cursor.execute(drop_table_sql)
        connection.commit()
finally:
    # 最后别忘了关闭这个连接
    connection.close()
```

更多操作请参考pymysql的官方文档。

## 总结

1. 使用pymysql连接数据库
2. 最好提前编写好SQL语句
3. 使用`connection.cursor.execute()`方法执行SQL语句，别忘了用try语句捕捉异常

## 思考

尝试连接数据库，实现数据的增删查改

