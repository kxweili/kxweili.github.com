---
layout: post
title: python3.x socket初试
category:coding
---
1.TCP服务端  

步骤：建立socket——>绑定地址端口——>开始监听——>循环，等待接受连接——>接受应答——>关闭该连接——>循环，等待接受连接。  
代码如下：

    import socket
    import sys
    if "__main__" == __name__:
        try:
            HOST = '127.0.0.1'
            PORT = 31500
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) #定义socket类型，网络通信，TCP
            print("creat socket")
            s.bind((HOST, PORT))
            print("bind socket")
            s.listen(5)
            print("begin listening")
        except:
            print("socket error!!")
        while True:
            print('listren for client...')
            conn, addr = s.accept() #接受TCP连接，并返回新的套接字与IP地址
            print('connected by', addr)
            conn.settimeout(5)
            szBuf = conn.recv(1024)
            byt = 'recv:'+szBuf.decode('UTF-8')#解码
            print(byt)
            if '0' == szBuf:
                conn.send('exit'.encode('UTF-8'))#编码
            else:
                conn.send('welcome client'.encode('UTF-8'))
        conn.close()
        print('end of the service）
注意，开始在发送和接收部分出现问题，好像是缓冲区不接受字符串，所以对发送和接受部分的内容进行了编码和解码
2.TCP客户端  

步骤：建立socket——>与地址端口建立连接请求——>发送内容——>关闭连接。


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

上面是本机的连接，如果要建立远程的连接，调用函数socket.gethostbyname()。如下： 


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

3.Socketsocket  

模块常见的异常有：  
Socket.error 与一般I/O和通信问题有关的   
Socket.gaierror 与查询地址有关的   
Socket.herror 与其他地址错误有关   
Socket.timeout 与一个socket上调用settimeout()后，超时处理有关   
