---
title: mysql example ex43 game (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ex43 game'


Modules used in program: 
* `import random`

## python ex43 game

Python mysql example: ex43 game

```python
# LPTHW Exercise 43 
import random

""" This module contains classes which denote the Game and individual rooms of the game."""

class Game(object):
    """
    This class represents the Game. It is what you would expect in a game 
    engine.
    
    This game consists of rooms. You can do different things in each room.
    The Game class holds references to various room objects and also
    provides a base set of game commands to query the current room, navigate
    among rooms, etc. Each room is expected to have it's own set of commands
    and a help command which will list all other commands.
    """
        
    def __init__(self):
        # Add commands and their help text to a dict
        self.cmds = {}
        self.cmds["rooms"] = "Prints the list of available rooms"
        self.cmds["room"] = "Print the name of the current room. If followed by an argument then user will be taken to that room"
        self.cmds["help"] = "Lists all the commands"
        self.cmds["quit"] = "Quit the game"

        # set room names along with their object instances in a dict
        self._rooms = {}
        self._rooms['SpacedRepitition'] = SpacedRepitition(self)
        
        # set the current room
        self.current_room = None
        
        #A placeholder list of commands representing that no command has been issued
        self._nocmd = ["nocmd"]


    def start(self):
        """
        Starts the game and provides an input prompt to the user to enter commands
        
        """
        
        print("Welcome to this wonderful game of self development!")
        print("Please enter your command at the prompt below. Type 'help' to get assistance.")
        
        #The main loop of the game
        while(True):
            cmd = raw_input("> ")
            args = cmd.split(' ')
            while args != None and len(args) > 0 and args[0] in self.cmds:
                cmd_func = getattr(self, args[0])
                # A command could return a list of arguments which may signify
                # the next command to execute
                args = cmd_func(args)
            
            if args == None:
                print("You did not enter a command")
            elif len(args) == 0:
                #Control will usually come here because it came out of the inner while loop
                print("You did not enter a command")
            elif args == self._nocmd:
                pass
            else:
                print("Unknown command '" + args[0] + "'"                )


    def help(self, args):
        """
        A function for the 'help' command
        """
        
        for cmd in self.cmds:
            print(cmd + " - " + self.cmds[cmd])

        return self._nocmd
         

    def rooms(self, args):
        """
        A function for the 'rooms' command
        """
        
        print("Rooms available in this game are:")
        count = 0
        for room in self._rooms:
            count += 1
            print("%d. %s" % (count, room))
            
        return self._nocmd


    def room(self, args):
        """
        A function for the 'room' command
        """
        
        if(len(args) > 1):
            # User seems to be indicating that they want to enter a room
            room_name = args[1]
            if room_name in self._rooms:
                room_obj = self._rooms[room_name]
                self.current_room = room_name
                args = room_obj.enter()
                return args
            else:
                print("Sorry, we could not find a room by that name '%s'" % room_name)
                
        else:
            # User wants to know which room they are in
            #we need to provide hallway as a default room
            if self.current_room == None:
                print("You are in the hallway, and have not yet entered a room.")
            else:
                print("You are in '%s'" % self.current_room)
                
        return self._nocmd


    def quit(self, args):
        """
        A function for the 'quit' command
        """

        print("Thank you for playing with us, have a wonderful day !")
        exit(0)


    def has_cmd(self, the_cmd):
        """
        Determines if this class supports the specified command.
        
        Returns True if it does and False otherwise
        """
        
        if(the_cmd in self.cmds):
            return True
        else:
            return False



class SpacedRepitition(object):
    """
    Class to represent the SpacedRepitition room
    """
        
    def __init__(self, _game):
        """
        Requires an object of type Game. This represents the game within which this room is operating.
        """
        
        self.game = _game
        
        # Add commands available in this room along with their help text
        self.cmds = {}
        self.cmds["help"] = "Lists all the commands"
        self.cmds["topic"] = "Prints the current topic. If followed by an argument then the user will be switched to that topic"
        self.cmds["topics"] = "Prints the list of available topics"
        self.cmds["question"] = "Request to ask a question"

        # Import all the files in which we have questions for various topics
        import java_questions
        import version_control_questions
        import javascript_questions
        import linux_questions
        import mysql_questions
        import misc_questions
        import php_questions
        import vim_questions        
        from question import Question

        #Adds topics to this room. Right now the questions for each topic come 
        #from a separate python file.
        self._topics = {
            "version_control":version_control_questions.version_control_questions,
            "java":java_questions.java_questions,
            "javascript":javascript_questions.javascript_questions,
            "linux":linux_questions.linux_questions,
            "mysql":mysql_questions.mysql_questions,
            "misc":misc_questions.misc_questions,
            "php":php_questions.php_questions,
            "vim":vim_questions.vim_questions
        }
        
        # Set the current topic
        self._current_topic = None


    def enter(self):
        """
        Signifies that a user has entered this room. All the commands of this
        room will now be available to the user
        """
        
        # TODO: The room name should come from a static property which should also be accessed from the Game class
        print("Welcome to the '%s' room" % "SpacedRepitition")
        
        while(True):
            # The main loop of this room
            cmd = raw_input("SpacedRepitition > ")
            args = cmd.split(' ')
                        
            if(args[0] in self.cmds):
                # We first see if the command is available in this room. If it is
                # then we will invoke the function which represents the command
                cmd_func = getattr(self, args[0])
                cmd_func(args)
            elif(self.game.has_cmd(args[0])):
                # Next we check if it is available in the game. If it is then
                # we will return the arguments to the game, so it may invoke
                # the command as it sees fit
                return args
            else:
                print("Unknown command '" + args[0] + "'")


    def topics(self, args):
        """
        A function for the 'topics' command
        """
        
        print("The following topics are available")
        count = 0
        for topic in self._topics:
            count += 1
            print("%d. %s" % (count, topic))
    

    def topic(self, args):
        """
        A function for the 'topic' command
        """
        
        if(len(args) > 1):
            topic_name = args[1]
            if topic_name in self._topics:
                self._current_topic = topic_name
                print("You will now be asked questions from the following topic %s." % self._current_topic)
                print("Please use the 'question' command to request a question.")
            else:
                print("Sorry, we could not find a topic by that name '%s'" % topic_name)
                
        else:
            #we need to provide hallway as a default room
            if self._current_topic == None:
                print("You have not selected any topic.")
            else:
                print("You will be asked questions from the following topic '%s'" % self._current_topic)
            
    
    def question(self, args):
        """
        A function for the 'question' command
        """
        
        if self._current_topic != None:
            current_topic = self._topics[self._current_topic]
            question_index = random.randint(0, len(self._topics[self._current_topic])-1)
            the_question = current_topic[question_index]
            print(the_question._question + "\n")
            answer = raw_input("Please type your answer >")
            print("\nThe correct answer is printed on the line below. Please compare it with what you typed.....")
            print(the_question._answer)
            print("\n\n")
        else:
            print("Please select a topic first.")
    

    def help(self, args):
        """
        A function for the 'help' command
        """
        
        print("Commands available from all rooms")
        self.game.help(args)

        print("\nCommands available in this room")
        for cmd in self.cmds:
            print("%s - %s" % (cmd, self.cmds[cmd]))

#=================== main ============================
if(__name__ == "__main__"):
    game = Game()
    game.start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
