---
title: "Create Simple Chat App Using UDP Protocol In Python"
toc: true
toc_label: "Content"
tags:
  - Python
  - Arth-Tasks
  - Networking
date: April 10, 2021
categories: Python
header:
  # image: assets/images/banner_without_name.png
  # overlay_image: assets/images/banner_without_name.png
  teaser: assets/images/python/chat-app-udp/header.png
permalink: /:categories/:title/
excerpt: "🔅 Create your own Chat Servers, and establish a network to transfer data using Socket Programing by creating both Server and Client machines as Sender and Receiver both. Do this program using UDP data transfer protocol.

🔅 Use multi-threading concept to get and receive data parallelly from both the Server Sides. Observe the challenges that you face to achieve this using UDP.
"
comments: true
---
![header.png]({{ site.url }}{{ site.baseurl }}/assets/images/python/chat-app-udp/header.png)

In this blog we are going to create a chat server based on UDP protocol in Python.

Here's the problem statement:

🔅 Create your own Chat Servers, and establish a network to transfer data using Socket Programing by creating both Server and Client machines as Sender and Receiver both. Do this program using UDP data transfer protocol.

🔅 Use multi-threading concept to get and receive data parallelly from both the Server Sides. Observe the challenges that you face to achieve this using UDP.

* * *

Before jump into code, let's understand

## What is UDP?

In computer networking, the User Datagram Protocol is one of the core members of the Internet protocol suite. With UDP, computer applications can send messages, in this case referred to as datagrams, to other hosts on an Internet Protocol network.

It is a connectionless protocol and not reliable. It is not used in other chatting apps like facebook and whatsapp. The communication speed is comparatively fast than TCP protocol. It is used in online video surfing and gaming.

* * *

## Creating Chat server\[One sided connection\]

In this setup, Server only receives message and client can only send messages. We can utilize the socket library in the python.

In server, we have to write below code, which will continuously  in receiving mode using while true.

```python
import socket      

#AF_INET used for IPv4
#SOCK_DGRAM used for UDP protocol
s = socket.socket(socket.AF_INET , socket.SOCK_DGRAM )
#binding IP and port 

s.bind(("192.168.225..242",2222))
print("Server started ...192.168.225..242:2222")
print("Waiting for Client response...") 
#recieving data from client
while True:
       print(s.recvfrom(1024))
```

In client program:

```python
import socket

#client program

s = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

while True:
       ip ,port = input("Enter server ip address and port number :\n").split()
       m = input("Enter data to send server: ")
       res = s.sendto(m.encode(),("192.168.225.242",2222)) 
       if res:
          print("\nsuccessfully send")
```

we can also see the working of it using below image

![ae90c373a8fd310eeb818419e8f989bb.png]({{ site.url }}{{ site.baseurl }}/assets/images/python/chat-app-udp/1db32f3068774002a10515d9f6eb9b92.png)

You can see on client end we really don't need to establish connection with client. We can directly send data to server.

But problem here is server can not respond back and client can not receive any message at the same time.

Here's we can take help of multi threading, by which we can receive as well send messages simultaneous.

## Both side communication using multithreading

In python we have threading module, with help we can add many threads to run our program. we can also pass different functions to different threads to run.

Let's understand with one help. consider human body is a process and our parts are different threads of it. they are performing their tasks simultaneously. Liver, heart, lungs, brain are the organs involved in the process and each process is running simultaneously.

*\# chatapp.py*

```python
import socket
import threading
import os

s = socket.socket(socket.AF_INET , socket.SOCK_DGRAM )
s.bind(("192.168.225.34",2222))
print("\t\t\t====>  UDP CHAT APP  <=====")
print("==============================================")
nm = input("ENTER YOUR NAME : ")
print("\nType 'quit' to exit.")

ip,port = input("Enter IP address and Port number: ").split()

def send():
    while True:
        ms = input(">> ")
        if ms == "quit":
            os._exit(1)
        sm = "{}  : {}".format(nm,ms)
        s.sendto(sm.encode() , (ip,int(port)))

def rec():
    while True:
        msg = s.recvfrom(1024)
        print("\t\t\t\t >> " +  msg[0].decode()  )
        print(">> ")
x1 = threading.Thread( target = send )
x2 = threading.Thread( target = rec )

x1.start()
x2.start()
```

Here we are leveraging the power of threads to simultaneously send and receive the messages.

In the above code I have two functions send() and rec() and we are starting them at same time. Theading module gives us Thread class and we can use that to make and start at the same time.

![fc8986688da5ff980e04271d96e5c1da.png]({{ site.url }}{{ site.baseurl }}/assets/images/python/chat-app-udp/384250a770154ff28825c4d3cc78db04.png)

Type quite to exit the program.

UDP is not reliable protocol which does not check that other system is running or not because it is connection less and does not have any acknowledgement.

## Conclusion

We have successfully create simple chat application using UDP protocol in python. 

I would love to hear your thoughts and ideas about this topics. Don't hesitate to share in comment section below. 

Also you can always message me over linkedIn as well. 

More Ideas:

You can enhance this setup to send OS commands and get the output back, instead of logging in systems.

# Thank you…
