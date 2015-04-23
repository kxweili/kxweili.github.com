---
layout: post
title: python3.x socket初试
category:  coding
---
参考和摘抄：
[1]http://www.oschina.net/question/12_76126

1.TCP 服务端
步骤：建立socket——>开始监听——>循环，接受连接——>接收数据，并应答——>接收完毕，关闭连接——>循环，接受连接。
代码：
import socket
import sys
if "__main__" == __name__:
    try:
        HOST = '127.0.0.1'
        PORT = 31500
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) #定义socket类型，网络通信，TCP
        print("creat socket")
        s.bind((HOST, PORT))#绑定地址端口
        print("bind socket")
        s.listen(5)#监听
        print("begin listening")
    except:
        print("socket error!!")
    while True:
        print('listren for client...')
        conn, addr = s.accept() #接受TCP连接，并返回新的套接字与IP地址
        print('connected by', addr)
        conn.settimeout(5)#设定时限
        szBuf = conn.recv(1024)
        byt = 'recv:'+szBuf.decode('UTF-8')#不能直接发送字符串，接收的数据需要编码
        print(byt)
        if '0' == szBuf:
            conn.send('exit'.encode('UTF-8'))#发送的数据需要编码
        else:
            conn.send('welcome client'.encode('UTF-8'))

        conn.close()
        print('end of the service')

起初碰到的问题就在，发送数据和接收数据上。

2.TCP客户端
步骤：建立——>与服务端的地址端口绑定——>发送与接收——>关闭连接

代码如下：
import socket
if "__main__" == __name__:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    HOST = '127.0.0.1'
    PORT = 31500
    s.connect((HOST, PORT))
    s.send("hi,nice to meet you.\nI'm sock1".encode('UTF-8'))
    szBuf = s.recv(1024)
    byt = 'recv:' + szBuf.decode('UTF-8')
    print(byt)
    s.close()
    print('end of the connecct')

3.异常
Socket模块常见的异常有：
Socket.error 与一般I/O和通信问题有关的
Socket.gaierror 与查询地址有关的
Socket.herror 与其他地址错误有关
Socket.timeout 与一个socket上调用settimeout()后，超时处理有关

4.远程主机的连接
用到远程地址解析socket.gethostbyname(host)
代码：
import socket
import sys
if "__main__" == __name__:
    host= 'www.baidu.com'
    port = 443
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)   #定义socket类型，网络通信，TCP
    print("creat socket")
    try:
        remote_ip = socket.gethostbyname(host)     #远程地址解析
    except socket.gaierror:
        print('Hostname could not be resolved. Exiting...')
        sys.exit()
    print('IP address of  ' + host + ' is  ' + remote_ip)
    s.connect((remote_ip, port))
    message = "GET / HTTP/1.1\r\n\r\n".encode('UTF-8')    #一个 HTTP 协议的命令，用来获取网站首页的内容。
    try:
        s.sendall(message)
    except socket.error:
        print('Send failed')
        sys.exit()
    print('Message send successfully')

    reply = s.recv(4096)
    print(reply.decode('UTF-8'))




