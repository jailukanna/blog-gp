---
title: turtle example matt oop  (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'matt oop '


## python matt oop 

Python turtle example: matt oop 

```python
from turtle import Turtle

class Character:

    turtles = ["Leonardo", "Donatello", "Raphael", "Michaelangelo"]
    color = {"Leonardo" : "blue" ,  "Donatello" : "purple", "Raphael" : "red" , "Michaelangelo": "orange"}
    weapon = {"Leonardo" : "ninjato" ,  "Donatello" : "bo", "Raphael" : "sai" , "Michaelangelo": "nunchuck"}

    def __init__(self, name = None):
        self.name = None

    def get_turtle(self):
        """
        User selects turtle from class variable turtle
        user selecton checks color and weapon variables to match with turtle selected
        turtle deleted from index so that map class can create the other turtles
        """
        while self.name == None:
            select = input("Please select your Ninja Turtle: Leonardo, Donatello, Raphael, or Michaelangelo.\n")
            if  select in Character.turtles:
                self.name = select
                index = Character.turtles.index(self.name)
                del Character.turtles[index]
            else:
                print("Please select one of the Turtles")
        self.weapon = Character.weapon[self.name]
        self.color = Character.color[self.name]
        print("You selected: " + self.name + ".")
        print("You are the " + self.color + " turtle.")
        print("Your weapon is a " + self.weapon + ".\n")


class Room:
    def __init__(self, room_name, description):
        """
        sets room name, description
        creates a dictionary to capture room names and directions to guide user movement
        """
        self.room_name = room_name
        self.description = description
        self.linked_rooms = {}

    def map_to(self, room_link, direction):
        """
        adds rooms and directions to dictionary created in __init__ method
        """
        self.linked_rooms[direction] = room_link
        
    def room_details(self):
        """
        prints out current room and description
        prints out adjoining rooms and their directions to guide users movement
        """
        print(self.room_name)
        print("")
        print(self.description)
        for direction in self.linked_rooms:
          room = self.linked_rooms[direction]
          print("The " + room.room_name + " is " + direction)
        
    def movement(self, direction):
        """
        allows user movement between rooms
        """
        if direction in self.linked_rooms:
            return self.linked_rooms[direction]
        else:
            print("There is no exit that way")
            return self

class Map:
    def __init__(self, name):
        self.name = name
    
    def startmap(self):
        """
        uses turtle module to create the map
        sets color of turtle to match the color of the user's selection
        """
        combos = [(-100,-100), (-40,-100), (-40,-40), (20, -100), (20,-40), (20,20), (80, -100), (80,-40), (140, -40)] 
        mapp = False
        while mapp == False:
            zone.shape("turtle")
            zone.color("black")
            zone.pensize(4)
            zone.speed(0)
            zone.penup()
            for combo in combos:
                zone.goto(combo)
                zone.pendown()
                for combo in range(4):
                    zone.forward(60)
                    zone.left(90)
                zone.penup()
            zone.goto(-70,-70)
            zone.color(Character.color[player1.name])
            mapp = True
            
    def movement(self, x, y):
        """
        moves the turtle through the map based on coordinates in the overall grid
        """
        self.x = x
        self.y = y
        zone.goto(self.x, self.y)

    def endmap(self):
        """
        creates three more turtles and matches them to the colors of the character not selected by the user
        creats an additional turtle and shapes it as an orange circle to represent the pizza
        moves all the new creations to the final room
        """
        turt2 = Turtle()
        turt3 = Turtle()
        turt4 = Turtle()
        turt5 = Turtle()
        two = Character.turtles[0]
        color2 = Character.color[two]
        three = Character.turtles[1]
        color3 = Character.color[three]
        four = Character.turtles[2]
        color4 = Character.color[four]
        turt2.shape("turtle")
        turt3.shape("turtle")
        turt4.shape("turtle")
        turt2.color(color2)
        turt2.penup()
        turt2.right(180)
        turt2.goto(190,10)
        turt3.color(color3)
        turt3.penup()
        turt3.right(180)
        turt3.goto(190,-10)
        turt4.color(color4)
        turt4.penup()
        turt4.right(180)
        turt4.goto(190,-30)
        turt5.shape("circle")
        turt5.penup()
        turt5.right(180)
        turt5.goto(170,-10)
        turt5.color("dark orange")
        

class Game:
    def __init__(self):
        print("""_______________________________
            TMNT
The Legend of the Greatest Pizza!
________________________________ \n""")

    def start(self):
        print("You hear the Turtle emergency phone ring and answer it.\n")
        player1.get_turtle()
        print("""On the other end of the line you hear Splinter,
"Oh {} it's you. Fantastic!"
"We need you to get to GoodCorp Labs as soon as possible.
They were in the middle of developing a solution to World Peace...
and the worlds greatest pizza ...
but Shredder showed up and is trying to steal the technology.
We need you to join the other turtles and get there before Shreddar
takes the tech.
Take the map I left on the refrigerator and go.
The map should automatically load and show your location at the Labs."
You say: 'the world's greatest pizza, I've got to get there fast.'\n""".format(player1.name))

        print("________________________________________________________\n")
        current_room = lobby
        mapmaker = Map("A")
        mapmaker.startmap()
        x_var = {lobby : -70, main : -10, break_room : -10, sales : 50, file : 50, bathroom : 50, production : 110, lab : 110, research : 150}
        y_var = {lobby : -70, main : -70, break_room : -10, sales : -70, file : -10, bathroom : 50, production : -70, lab : -10, research : -10}
        end = None
        while current_room != research and end != "q":
            current_room.room_details()
            action = input("""\nThe available movements are north, east, south and west.
You may be limited in which direction you can move by the room you are in.
Refer to the info right below room description to see available movement directions
Input your action here-->""").lower()
            if action in ["north", "south", "east", "west"]:
                current_room = current_room.movement(action)
                mapmaker.movement(x_var[current_room], y_var[current_room])
            if current_room == research:
               mapmaker.endmap()
               print("Research and Development")
               print(research.description)
               print("You join the other turtles in victory over Shredder, but know he will be back.\n")
               print("Now is time to enjoy the best pizza ever!")
               end = input("Type q to quit.")
        

# The items below are the room names and descriptions for each each instance of the Room class.
lobby = Room("Lobby", """You arrive in the lobby of the Lab.
The door was broken and there is office equipment scattered around the room.
the emergency lights are flashing all around you.\n""")
main = Room("Main Office", """The main office area.
Most of the desks and computers have been broken.
Injured police, scientists, and businessmen are lying on the floor.
One of the police begs you to reach the tech before Shredder\n""")            
break_room = Room("Break Room", """A room with a few small tables a refrigerator and a microwave.
You can tell that your fellow turtles have been here.
You smell microwaved pizza.\n""")
sales = Room("Sales Office", """The room is mostly dark with just a single flourescent bulb providing light.
You see a foot soldier.
You sneak up behind him and grab him.
He begs to be let go for information.
You let him go and he tells you Shredder went towards the lab.\n""")
file = Room("File Room", """The room has long rows of file cabinets.
A few cabinets have been opened and their files are thrown across the floor.\n""")
bathroom = Room("Bathroom", """A standard bathroom.
There is nothing in here.\n""")
production = Room("Production Room", """The room contains multiple manuals and engineering equipment.
Most of it has been smashed.
You see a foot soldier on the ground.
The other turtles must be near.\n""")
lab = Room("Lab", """A standard chemistry lab with lots of vials, scales and bunsen burners.
Most of the equipment has been smashed.
You see Shredder, he is trying to get to the Research and Development room.
He sees you and knows that with you there there's no way he can win the fight.
He jumps through the window without the tech.\n""")
research = Room("Research and Development", """The room where final products are tested and approved.
The room smells of the most delicious pizza ever.""")

# this takes the room instance and links them to adjacent rooms and their directions
lobby.map_to(main, "east")
main.map_to(lobby, "west")
main.map_to(break_room, "north")
main.map_to(sales, "east")
break_room.map_to(main, "south")
sales.map_to(main, "west")
sales.map_to(file, "north")
sales.map_to(production, "east")
file.map_to(sales, "south")
file.map_to(bathroom, "north")
bathroom.map_to(file, "south")
production.map_to(sales, "west")
production.map_to(lab, "north")
lab.map_to(production, "south")
lab.map_to(research, "east")

player1 = Character()
zone = Turtle()
turt2 = Turtle()
turt3 = Turtle()
turt4 = Turtle()
turt4 = Turtle()
game = Game()
game.start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
