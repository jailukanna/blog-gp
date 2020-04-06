---
title: mysql example Escapeto80s (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Escapeto80s'

Functions in program: 
* `def unlock(loc):`
* `def take(target):`
* `def move(loc,newloc):`
* `def show_odetails(target,loc):`
* `def door_view(loc):`
* `def object_view(loc):`
* `def room_view(loc):`

Modules used in program: 
* `import mysql.connector`

## python Escapeto80s

Python mysql example: Escapeto80s

```python
import mysql.connector
#Tee extrs lista fannypackil muista ja osaat kyl moi tulevaisuuden rasmus t rasmus
 #Defining functions
#Shows overall picture of room 
def room_view(loc):
    cur = db.cursor()
    sql = "SELECT details,description FROM Location WHERE locationid ='" + loc + "'"

    cur.execute(sql)
    for i in cur:
        print(i[0])
        if(i[1]!=""):
            print(i[1])
    
    return

#Descriptions of  objects in room

def object_view(loc):
    cur = db.cursor()
    sql = "SELECT Item FROM OBJECT WHERE Hidden=FALSE and locationId ='" + loc + "'"
    cur.execute(sql)
    print('You see these awesome things: ')
    for i in cur:
        
        print(i[0])
    
    return


#Showing locations of the doors light my fire
def door_view(loc):
    print("The door is ")
    cur=db.cursor()
    sql = "SELECT note, jotain FROM Door WHERE startingpoint ='" + loc + "' and locked != TRUE"
    
    cur.execute(sql)
    for i in cur:
        print(i[0])
        if(i[1]!=""):
            print(i[1])

    return

# Details from object when player examines them
def show_odetails(target,loc):
    cur = db.cursor()
    sql =" SELECT Odetails FROM Object WHERE name = '" + target + "' and  locationId ='" + loc + "'"
    cur.execute(sql)
    for i in cur:
        print(i[0])

    return

 #Player moving from room to another   
def move(loc,newloc):
    finishpoint = loc
    cur = db.cursor()
    sql = "SELECT finishpoint FROM Door WHERE startingpoint ='" + loc + "' and locked = False" #Room2
    cur.execute(sql)
    if cur.rowcount >= 1:
        for row in cur.fetchall():
            finishpoint = row[0]
    else:
        finishpoint = loc
    return finishpoint
'''
    for row in cur:
        newloc =str(row[0])
        print("newloc",newloc)
        #newloc = finishpoint
        
        print("location",loc)
'''





#Player taking objects to inventory
def take(target):
    
    cur = db.cursor()
    #sql = "UPTATE SET locationId player WHERE Takeable = True and  locationId ='" + loc + "'"
    sql = "SELECT name  From Object WHERE Takeable = True and  locationId ='" + loc + "'"
   # name = "SELECT name FROM Object"
    cur.execute(sql)
    for row in cur:
        listu.append(str(row[0]))
       
    x = target
   # print(x)
    if x == 'fannypack':
        print("You took the fannypack")
        fannypack.append('fannypack')
    elif x in listu and 'fannypack' in fannypack :
            print("You took the item")
            fannypack.append(x)
    else:
            print("You can't do that")

    return listu, target, fannypack


#unlocking doors
def unlock(loc):
    cur=db.cursor()
    #sql = "SELECT locationId FROM Location"
    sql= "UPDATE Door SET Locked = False WHERE startingpoint ='" + loc + "' and Locked=True"
    cur.execute(sql)
    return 










    

#Log in to the database 
db = mysql.connector.connect(host="localhost",
                      user="hoffa",
                      passwd="salasana",
                      db="peli",
                      buffered=True)




#Making some room to console
print("\n"*1000)

#Variables
loc = 'ROOM1'
newloc=""
action =""
fannypack = []
listu= []

#functions
room_view(loc)
object_view(loc)
door_view(loc)

#Gameplay
while action != "quit":
    
    input_command = input("Its your move: ").split()
    if len(input_command)>=1:
        action = input_command[0].lower()
    else:
            action =""
    if len(input_command)>=2:
            target = input_command[len(input_command)-1].lower()
            
    else:
     target = ""

    #print(action)
    #print(target)

   #examine
    if action == "examine" or action == "e" or  action == "look" or action == "l":
        
        show_odetails(target,loc)

#dance
    if action == "dance":
        print("You are dancing")

 #open door       
    elif action == "open" and target == "door":
         newloc = move(loc,action)
         if loc == newloc:
            print("Door is locked")
         else:
                print("You opened the door" ,loc)
                print("newloc",newloc)
                loc = newloc
                print("location",loc)
                room_view(loc)


                
#Inventory
    if action == "i" or action == "inventory":
        print("You have following items on your fannypack ",fannypack)



#command to take objects into inventory
    elif action == "take":
        take(target)
        print(fannypack)

#command use key, unlocks the door
    if action=="use" and target=="key":
        
        if target in fannypack:
            unlock(loc)
            print("Door is now unlocked" )
        else:
            print("You need to find the key to unlock the door")
            
#opening the first codelock
    elif action=="use" and target=="codelock":
        y = input("Give the code: ")
        if y == "fresh":
            unlock(loc)
            print("Door is unlocked")
        else:
            print("Wrong code")            
        


db.rollback()
db.close



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
