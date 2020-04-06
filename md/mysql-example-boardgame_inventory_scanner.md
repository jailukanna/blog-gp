---
title: mysql example boardgame inventory scanner (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'boardgame inventory scanner'

Functions in program: 
* `def numcheckedout(bggid):`
* `def checkout(bggid, playerid):`
* `def checkin(bggid, playerid, checkingid):`
* `def checking(c, upc):`
* `def playercommands(upc):`
* `def commands(upc):`
* `def main_menu():`
* `def inventory(upc):`
* `def player_search(upc):`
* `def gameoutput(result, col2):`
* `def undo(bggid, name):`
* `def alter_name(name):`
* `def user_search(name):`
* `def get_name(bggid):`
* `def get_bggid_from_publisher(c, bggid):`
* `def add_game(c, bggid, name, upc):`
* `def get_bggid_from_name(name):`
* `def scan_upcitemdb(upc):`
* `def scan_upcindex(upc):`
* `def upc_search(upc):`

Modules used in program: 
* `import urllib2, sys, re`
* `import mysql.connector`
* `import requests`

## python boardgame inventory scanner

Python mysql example: boardgame inventory scanner

```python
import requests
import mysql.connector
from xml.dom import minidom
import urllib2, sys, re
from BeautifulSoup import BeautifulSoup as bs

menu_select = "1"

# to do: check to see if inventory table exists.  If not, create it.
# to do: check to see if replica bgg database exists.  If not, don't use it.
# to do: add help
# to do: add ability to some how read and/or manipulate database?
# to do: bgg database doesn't have collection table.  The conversion errored out with: ERROR 1064 (42000) at line 857795: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'current_date, owned INTEGER NOT NULL, rank INTEGER, weight REAL, trading INTEGER' at line 1
# to do: add menu 'above' this that lets the user select catalog or check in/out
# to do: add exit function to go back to the menu 'above' this
# to do: update the date when you increment a game on inventory


def upc_search(upc):
        # Checks to see if the UPC is already in the inventory database
        # If it is, it outputs the BGGID of the game associated with it
        
        query = "SELECT bggid FROM inventory WHERE upc = {upc}".format(upc = upc)
        c.execute(query)
        bggid = c.fetchone()
        
        return bggid

def scan_upcindex(upc):
        # Searches UPCIndex for the UPC
        # Returns the game names that it found, if any 
        
        url = 'http://www.upcindex.com/'
        url += str(upc)
        
        hdr = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11',
                   'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
                   'Accept-Charset': 'ISO-8859-1,utf-8;q=0.7,*;q=0.3',
                   'Accept-Encoding': 'none',
                   'Accept-Language': 'en-US,en;q=0.8',
                   'Connection': 'keep-alive'}
        
        req = urllib2.Request(url, headers=hdr)
        
        page = urllib2.urlopen(req)
        soup = bs(page)
        
        
        name = soup.find("ol", {"class":"no-slide"})
        title = []
        for li in name:
                title.append(li.text.encode("utf-8"))
        
        return title

def scan_upcitemdb(upc):
        # Searches UPCItemdb for the UPC and returns the game names, if any
        print("Unable to find in UPCindex.  Searching UPCitemdb.")
        url = 'https://api.upcitemdb.com/prod/trial/lookup?upc='
        #upc = 609456647229
     
        url += str(upc)

        response = requests.get(url)
        #       print(response.text)
        if response.status_code != 200:
                print("Item not found!")
                title = []
                return
        # print(response.json())

       
        offers= response.json()["items"][0]["offers"]
#       print(offers[0])
        title = []
        for index,item in enumerate(response.json()["items"][0]["offers"]):
                title.append(item["title"])
#               print(response.json()["items"][0]["offers"][index]["title"])

        return title
        
def get_bggid_from_name(name):
        # searches the database for the game name and returns the bggid, if it found it.
        # if multiple games were found with the same name, it goes to get_bggid_from_publisher function
        #    which will ultimately display the games to the user and allows them to select which one they want
        
        
        # to do: don't add % at the end or beginning of the name (it might lead to multiples)

        # Remove :, &, and leading spaces
        name = name.replace(':', '%')
        name = name.replace('&', '%')
        name = name.rstrip(' ')         # remove trailing spaces
        name = name.lstrip(' ')         # remove leading spaces
        name = name.replace('and', '%') # to do: consider making this better
        query = "SELECT bggid FROM games WHERE name LIKE '{nm}'".format(nm = name)
        
        
        c.execute(query)
        
        result = c.fetchall()
        if not result:
                return
        elif len(result) == 1:
                bggid = str(result[0][0])
        else: # more than one bggid for that game name 817054010226         
                bggid = []
                string = ""
                for game in result:
                        string += str(game[0]) + ", "
                        bggid.append(game[0])
                # string = string.rstrip(", ")
                # print("more than one game with that name: " + string)

                bggid = get_bggid_from_publisher(c, bggid)
        
        return bggid
        
def add_game(c, bggid, name, upc):
        # adds game to the inventory table of the database (or increments its ownership value if it is already there)
        query = "SELECT owned FROM inventory WHERE bggid = {bggid}".format(bggid = bggid)
        c.execute(query)
        owned = c.fetchone()

        if owned: # This game is already in the inventory, see if we want to increment it
                owned = owned[0]
                
                response = str(raw_input( "There are {num} copies of {gm} in your inventory.  Do you want to add another? y/N ".format(num=owned, gm = name))) or "n"
                if response == 'y' or response == 'Y':
                        query = "UPDATE inventory SET owned = {num} WHERE bggid = {bggid}".format(num=owned+1, bggid=bggid)
                        c.execute(query)
                        print("Updated " + name + " in your collection.")
                elif response == 'n' or response == "N":
                        return
                else:
                        print("Invalid response.  Please scan again.")
        else:                    
                query = "INSERT INTO inventory (bggid, upc) VALUES ({bggid}, {upc})".format(bggid = bggid, upc = upc)
                c.execute(query)
                print("Added " + name + " to your collection.")
        
        conn.commit()
             
def get_bggid_from_publisher(c, bggid):
        # multiple BGGIDs found, this function will sort them by publisher and the user can pick from that
        pub_query = """ SELECT publishers.name
                FROM publishers
                INNER JOIN gamepub ON gamepub.pubid = publishers.pubid
                WHERE gamepub.bggid = {bggid}"""
                
        year_query = """SELECT games.year
        FROM games
        WHERE games.bggid = {bggid}"""
        
        name_query = "SELECT games.name FROM games WHERE games.bggid = {bggid}"
        
        i = 1
        print("Multiple games with that name were found:")
        choices = []
                        
        name_string = ""
        for game in bggid:
                choices.append(game)
                query = name_query.format(bggid=game)
                c.execute(query)
                name = c.fetchall()[0][0].encode('utf-8')
                
                query = year_query.format(bggid=game)
                c.execute(query)
                year = c.fetchall()[0][0]
                
                print(str(i) + " : " + "("+str(year)+") "+ name)
                i += 1
         # TO DO: add the more info capability 
        more_info = i
        print(str(more_info) + " : More Info")
        print(str(more_info+1) + " : Exit")
        
        user_choice = input("Input number of the game you would like to select: ")
        
       
        
        
        i = 1
        if user_choice == more_info: # more info was selected
                for game in bggid:
                        pub_string = ""
                        query = pub_query.format(bggid=game)
                        c.execute(query)
                        result = c.fetchall()
                        for pub in result:
                                pub_string += pub[0].encode('utf-8') + ", "
                        pub_string = pub_string.rstrip(", ")
                        
                        print(str(i) + " : " + pub_string)
                        i += 1
                        
                print(str(more_info) + " : Input BGGID")
                print(str(more_info+1) + " : Exit")
                user_choice = input("Input number of the game you would like to select: ")
        elif user_choice == more_info+1: # user wants to exit
                return
        else: # game chosen
                bggid = choices[user_choice-1]
                        
        
        
        if user_choice == more_info: # user inputs BGGID
                bggid = input("Input BGGID: ")
                query = name_query.format(bggid=bggid)
                c.execute(query)
                name = c.fetchall()[0][0].encode('utf-8')
                user_input = input("Add {nm}? Y/n: ".format(nm = name)) or "Y"
                if user_input != 'y' or user_input != "Y":
                        return # otherwise, bggid stays as user input
                else:
                        bggid = choices[user_choice-1]
        elif user_choice == more_info + 1: # user wants to exit
                return
        else:
                bggid = choices[user_choice-1]
                
                
        
        return bggid
        
def get_name(bggid):
        # gets the name from the db, so that we don't enter a new game name in
        query = """SELECT name FROM games WHERE bggid = {bggid}""".format(bggid = bggid)
        c.execute(query)
        return c.fetchall()[0][0]

def user_search(name):
        # searches the database for a game name given by the user
        # first searches exactly what the user typed.  Then, it adds wildcards to the beginning and end
        bggid = get_bggid_from_name(name)
        
        if not bggid: # no game found.  Add wild cards
                print("No game with name '{nm}' found.  Adding wildcards".format(nm = name))
                bggid = get_bggid_from_name("%" + name + "%")
                # name = get_name(bggid)
                # add_game(c, bggid, name, upc)
        
        return bggid
 
def alter_name(name):
        # alters the game name to remove things like 'deluxe edition', which may come up in a UPC search
        name = re.sub('deluxe edition', '', name, flags=re.IGNORECASE)
                        
        name = re.sub('(brand new)', '', name, flags=re.IGNORECASE)
        name = name.lstrip('Kosmos - ')
        name = re.sub('board game', '', name, flags = re.IGNORECASE)
        name = re.sub('Brand', '', name, flags = re.IGNORECASE)
        name = re.sub(': N/a', '', name, flags = re.IGNORECASE)
        name = re.sub('game', '', name, flags = re.IGNORECASE)
        name = name.replace('&#039;', '')
        name = name.rstrip(' ')
        name = name.rstrip('-')
        name = name.replace('()', '')
        
        return name

def undo(bggid, name):
        # removes the previous game that was added to the database (or reduces the ownership value if > 1)
        query = "SELECT owned FROM inventory WHERE bggid = {bggid}".format(bggid = bggid)
        c.execute(query)
        owned = c.fetchone()[0]
        if owned == 1:
                # need to remove the game from the database
                user_choice = raw_input("Remove '{gm}' from inventory? y/N: ".format(gm = name)) or "N"
                if user_choice == 'y' or user_choice == 'Y':
                        query = "DELETE FROM inventory WHERE bggid = {bggid}".format(bggid = bggid)
                        c.execute(query)
                else:
                        return
        elif owned > 1:
                user_choice = raw_input("Remove 1 copy of '{gm}' from inventory (there are current {x} copies in the inventory)? y/N: ".format(gm = name, x = owned)) or "n"
                if user_choice == 'y' or user_choice == 'Y':
                        query = "UPDATE inventory SET owned = {num} WHERE bggid = {bggid}".format(num=owned-1, bggid=bggid)
                        c.execute(query)
                        print("Updated " + name + " in your collection.")
                else:
                        return
                
        
        conn.commit() 

def gameoutput(result, col2):
        # prints game and number in inventory in a tabular form
        # result needs to be the result of a query with SELECT games.name, inventory.owned
        print("{0:<75s} {1}".format('Game', col2))
        print("{0:<75s} {1}".format('----', '----'))
        for game, owned in result:
                print("{0:<75s} {1}".format(game.encode('utf-8'), owned))
        print("{0:<75s} {1}".format('----', '----'))
        if isinstance(result[0][1], int):
                print("{0:<75s} {1}".format('Total', sum([pair[1] for pair in result])))
        
def player_search(upc):
        query = "SElECT playerid FROM players WHERE upc = {upc}".format(upc = upc)
        c.execute(query)
        playerid = c.fetchone()[0]
        
        return playerid
  
def inventory(upc):
        global menu_select
        if upc == "": # allows the user to press enter without any command or UPC
                True
        
        elif upc[0] == '/': # signifies user is going to use a function
                commands(upc)
                
                        
        else:
              
                bggid = []
                name_upcindex = [] 
                name_upcitemdb = []
                
                bggid = upc_search(upc) # see if UPC is already in the db
                if bggid:               # if it is, get the name of the game and go to add_game function
                        bggid = bggid[0]
                        name = get_name(bggid)
                        add_game(c, bggid, name, upc)
                else:                   # if the UPC isn't in the database, search online for the UPC
                
                
                
                        # # Search upcindex
                        # try:
                        #         name_upcindex = scan_upcindex(upc)
                        #         
                        #         for game in name_upcindex:
                        #                 bggid = get_bggid_from_name(game)
                        #                 print(game)
                        #                 print(bggid)
                        #                 if bggid:
                        #                         bgname = game
                        #                         break
                        # except TypeError:
                        #         print("Game not found on UPCindex")
                        #         name_upcindex = []
                        # except urllib2.HTTPError:
                        #         print("UPCIndex HTTP Error 403: Forbidden")
                        #         name_upcindex = []
                        
                        # If doesn't find it on upcindex, search UPCitemdb
                        try:
                                name_upcitemdb = scan_upcitemdb(upc)
                                if not bggid and name_upcitemdb:
                                        for game in name_upcitemdb:
                                                bggid = get_bggid_from_name(game)
                                                print(game)
                                                print(bggid)
                                                if bggid:
                                                        bgname = game
                                                        break
                        except IndexError:
                                print("Does not exist in UPCitemdb")
                                name_upcitemdb = []
                        except urllib2.HTTPError:
                                print("UPCitemdb HTTP Error")
                                name_upcitemdb = []
                                        
                        # to do time n space doesnt work
                        
                        ## If still doesn't find it, start removing:
                        
                        if not bggid and (name_upcindex or name_upcitemdb):
                                print("Unable to find in UPCIndex and UPCitemdb.  Altering Name.")
                                for name in name_upcindex + name_upcitemdb:
                                        alter_name(name)
                                        bggid = get_bggid_from_name(name)
                                        
                                        if bggid: # found game
                                                bgname = name
                                                break
                        
                        if not bggid:
                                user_input = str(raw_input( "Unable to find this game.  Please type name: "))
                                bggid = user_search(user_input)
                                        
                                        
                                
                        print("bggid: " + str(bggid))
                        
                        if bggid:
                                bgname = get_name(bggid)
                                add_game(c, bggid, bgname, upc)

def main_menu():
        global menu_select
        print("Menu Options: ")
        i = 1
        for items in menu:
                print(str(i) + " : " + items)
                i += 1
        
        menu_select = raw_input("Select The Number of The Menu Option You Want [Default: 1]: ") or "1"
        return menu_select

def commands(upc):
        # to do: add command to add player
        if upc == '/undo':
                if bggid:
                        undo(bggid, name)
                        bggid = []
                        name = ""
                else:
                        print("No game to undo. Note: You can only undo the previous (1) game.")
        
        elif upc == '/showall':
                query = """SELECT games.name, inventory.owned
                                FROM games
                                INNER JOIN inventory ON games.bggid = inventory.bggid
                                ORDER BY games.name"""
                c.execute(query)
                result = c.fetchall()

                gameoutput(result, "Number Owned")
        
        elif upc[0:7] == '/search':
                upc = upc.lstrip('/search ')
                
                query = """SELECT games.name, inventory.owned
                                FROM games
                                INNER JOIN inventory ON games.bggid = inventory.bggid
                                WHERE games.name LIKE '%{nm}%'
                                ORDER BY games.name""".format(nm = upc)
                c.execute(query)
                result = c.fetchall()
                if result:
                        gameoutput(result, "Number Onwed")
                else:
                        print("'{gm}' was not found in your inventory.".format(gm = upc))
                        
        elif upc[0:5] == '/last':
                upc = upc.lstrip('/last ')
                query = """SELECT games.name, inventory.date
                                FROM games
                                INNER JOIN inventory ON games.bggid = inventory.bggid
                                ORDER BY inventory.date desc LIMIT {num}""".format(num=upc)
                c.execute(query)
                result = c.fetchall()
                if result:
                        gameoutput(result, "Date Updated")
                else:
                        print("Wrong syntax.  To see the last X games added, use: /last 5")
        elif upc[0:5] == "/exit":
                main_menu()
                        
        elif upc[0:10] == "/addplayer":
                upc = raw_input("Enter UPC: ")
                firstname = raw_input("First Name: ")
                lastname = raw_input("Last Name: ")
                
                query = "INSERT INTO players (firstname, lastname, upc) VALUES ('{fn}', '{ln}', '{upc}')".format(fn = firstname, ln = lastname, upc = upc)
                c.execute(query)
                
        else:
                print("Invalid syntax.  Type '/help'.")

def playercommands(upc):
        if upc[0:7] == "/search":
                upc = upc.lstrip("/search ")
                query = "SELECT games.name FROM games INNER JOIN checking ON games.bggid = checking.bggid INNER JOIN players ON players.playerid = checking.playerid WHERE checking.checkin = 0 AND players.upc = {upc}".format(upc = upc)
                c.execute(query)
                result = c.fetchall()
                
                query = "SELECT firstname FROM players WHERE upc = {upc}".format(upc = upc)
                c.execute(query)
                name = c.fetchone()[0]
                
                print("{name} has the following checked out: ".format(name = name))
                
                for game in result:
                        print(game[0].encode('utf-8'))
        else:
                print("Invalid function.")
                

def checking(c, upc):
        # to do: add undo function
        
        if upc == "":
                True
        if upc[0] == '/':
                playercommands(upc)
        else:
                # search UPC in inventory table.
                bggid = upc_search(upc)
                if bggid: # game is in inventory.
                        bggid = bggid[0]
                
                        upc = raw_input("Scan Player Badge: ")
                        playerid = player_search(upc)
                        if playerid: # player is in the player table
                                # check to see if the player has that game checked out
                                print(playerid)
                                print(bggid)
                                query = "SELECT checkingid FROM checking WHERE checkin = 0 AND playerid = {pi} AND bggid = {bi}".format(pi = playerid, bi = bggid)
                                print(query)
                                c.execute(query)
                                
                                checkingid = c.fetchone()
                                if checkingid: # the user has that game checked out
                                        checkin(bggid, playerid, checkingid[0])
                                else: # that user doesn't have that game checked out
                                        checkout(bggid, playerid)
                else:
                        playerid = player_search(upc)
                        if playerid:
                                upc = raw_input('Scan Board Game: ')
                                bggid = upc_search(upc)
                                if bggid:
                                        bggid = bggid[0]
                                        query = "SELECT checkingid FROM checking WHERE checkin = 0 AND playerid = {pi} AND bggid = {bi}".format(pi = playerid, bi = bggid)
                                        c.execute(query)
                                        checkingid = c.fetchone()
                                        if checkingid: # the user has that game checked out
                                                checkingid = checkingid[0]
                                                checkin(bggid, playerid, checkingid)
                                        else: # that user doesn't have that game checked out
                                                checkout(bggid, playerid)
                                        
                
                        
                        # to do: check to make sure user doesn't already have the game checked out before checking in
                                
                                
                                
                
                
                
                
                        
        conn.commit()

def checkin(bggid, playerid, checkingid):
        # to do: make sure the game is checked out before checking it in
        num = numcheckedout(bggid)
        
        
        
        query = "UPDATE checking SET checkin = now() WHERE checkingid = {ci}".format(ci = checkingid)
        c.execute(query)
        

        query = "UPDATE inventory SET numcheckedout = {num} WHERE bggid = {bggid}".format(num = int(num)-1, bggid = bggid)
        c.execute(query)
        
        # conn.commit()
        
        gamename = get_name(bggid)
        
        print("Checked in {gamename}".format(gamename = gamename))
        
def checkout(bggid, playerid):
        # to do: make sure player doesn'thave a game checked out already (select checkingid where checkin = 0 and playerid =)
        # to do: add override command to allow a player to have multiple games checked out

        num = numcheckedout(bggid)
        
        query = "UPDATE inventory SET numcheckedout = {num} WHERE bggid = {bggid}".format(num = int(num)+1, bggid = bggid)
        c.execute(query)
        
        query = "INSERT INTO checking (checkout, playerid, bggid) VALUES (NOW(), {pi}, {bi})".format(pi = playerid, bi = bggid)
        c.execute(query)
        
        gamename = get_name(bggid)
        
        print("Checked out {name} to player".format(name = gamename))
        
        # conn.commit()
        
def numcheckedout(bggid):
        query = "SELECT numcheckedout FROM inventory WHERE bggid = {bi}".format(bi = bggid)
        c.execute(query)
        numcheckedout = c.fetchone()[0]
        
        return numcheckedout

# Connect to the database        
conn = mysql.connector.connect(user='<user>', password='<password>', host='<ipaddress>', database='<databasename>')
c = conn.cursor()
bggid = []
name = ""

menu = ["Inventory", "Check In/Out"] # Inventory = 1, Check In = 2

main_menu()
while True:
        try:
                upc = raw_input('[{menu}] Scan UPC or enter command: '.format(menu = menu[int(menu_select)-1]))
                upc = upc.lstrip('0')
                
        except SyntaxError:
                print("Null UPC")
        
        if menu_select == "1":
                inventory(upc)
        elif menu_select == "2":
                checking(c,upc)
        
                
           
           
                
        conn.commit()






conn.close()


# TO DO some games might have more than one UPC.  Update database and code to be able to handle this.
# to do: when listing games, after more info add exit
''' To do: Games that don't work
ascension chronlic of the godslayer (lots come up, not sure which)




'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
