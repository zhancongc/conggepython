# socket 编程

> socket 

```python
import socket

server = socket.socket()
server.bind(('127.0.0.1', 8000))
while True:
    connection, address = server.accept()
    while True:
        recv_data = connection.recv(1024)
        if recv_data:
            print(recv_data)
            connection.send(recv_data)
        else:
            connection.close()
        break
```

![img](https://images2018.cnblogs.com/blog/1327694/201803/1327694-20180331170957832-405086904.png)



## 非阻塞socket客户端

```python
# -*- coding: UTF-8 -*-
# python使用select进行非阻塞模式编程，客户端程序
import socket
import select

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)    # 生成socket
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  # 不经过WAIT_TIME，直接关闭
sock.setblocking(False)                                     # 设置非阻塞编程

try:
    # sock.connect(("google.com", 80))
    sock.connect(("127.0.0.1", 8000))
except Exception as e:
    print(e)

r_inputs = set()
r_inputs.add(sock)
w_inputs = set()
w_inputs.add(sock)
e_inputs = set()
e_inputs.add(sock)

while True:
    try:
        r_list, w_list, e_list = select.select(r_inputs, w_inputs, e_inputs, 1)
        print("r")              # 产生了可读事件，即服务端发送信息
        for event in r_list:
            try:
                data = event.recv(1024)
            except Exception as e:
                print(e)
            if data:
                print(data)
                print("收到信息")
            else:
                print("远程断开连接")
                r_inputs.clear()
        print("w")
        if len(w_list) > 0:     # 产生了可写的事件，即连接完成
            print(w_list)
            w_inputs.clear()    # 当连接完成之后，清除掉完成连接的socket
        print("e")
        if len(e_list) > 0:     # 产生了错误的事件，即连接错误
            print(e_list)
            e_inputs.clear()    # 当连接有错误发生时，清除掉发生错误的socket
    except OSError as e:
        print(e)
```

## 非阻塞socket服务端

```python
# -*- coding: UTF-8 -*-
import socket
import select

sock = socket.socket()
sock.bind(('127.0.0.1', 8000))
sock.setblocking(False)
sock.listen()


inputs = [sock, ]

while True:
    r_list, w_list, e_list = select.select(inputs, [], [], 1)
    for event in r_list:
        if event == sock:
            print("新的客户端连接")
            new_sock, addr = event.accept()
            inputs.append(new_sock)
        else:
            data = event.recv(1024)
            if data:
                print("接收到客户端信息")
                print(data)
                event.send(b'\x31')
            else:
                print("客户端断开连接")
                inputs.remove(event)
```