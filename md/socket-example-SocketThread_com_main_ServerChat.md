---
title: socket example SocketThread com main ServerChat (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SocketThread com main ServerChat'

Functions in program: 
* `def main():`
* `def send_msg(name,msg):`
* `def notice_other_usr(usr):`
* `def hand_user_con(usr):`

Modules used in program: 
* `import sys,socket,threading`

## python SocketThread com main ServerChat

Python socket example: SocketThread com main ServerChat

```python
import sys,socket,threading
sys.path.append("D:\\python_code\\SocketThread\\com\\bean")
from time import ctime,sleep
from User import User




#global variable
userList = []

def hand_user_con(usr):
    try:
        isNormal = True
        print('创建用户线程  %s'%usr.name)
        while isNormal:
            data = usr.soc.recv(1024)
            data = str(data,'utf-8')
            msg = data.split("|")   #msg  0 登录指令  1  登录名   2
            if msg[0] == "login":
                print('user %s is login' %msg[1])
                usr.name = msg[1]
                notice_other_usr(usr)
            elif msg[0] =="talk":
                print("user[%s] said: [%s] " %(msg[1],msg[2]))
                send_msg(msg[1], msg[2])  # 发送消息给目标用户，参数1：目标用户，参数2：消息内容
            elif msg[0] =='exit' or msg[0] =='quit':
                print('user exit %s '%usr.name)
                isNormal = False
                usr.close()
                userList.remove(usr)

    except:
        print('出现异常......')
        isNormal = False

#通知其他用户以上的好友
def notice_other_usr(usr):
    if(len(userList)>1):
        print('The two users')
        userList[0].soc.send(("login|%s" % userList[1].name))
        userList[1].soc.send(("login|%s" % userList[0].name))
    else:
        print('The one users')

def send_msg(name,msg):
    for usr in userList:
        if(usr.name==name):
            usr.soc.send(bytes(msg.encode()))

#程序入口
def main():
    s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.bind(('127.0.0.1',9999))
    s.listen(5)
    print('服务端已创建，等待用户接入...')
    while True:
        conn,addr=s.accept()#等待用户连接
        user=User(conn,'guo')
        print('开始添加用户 %s'%user)
        userList.append(user)
        t=threading.Thread(target=hand_user_con,args=(user,));
        t.start()
    s.close()

if(__name__=="__main__"):
    main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
