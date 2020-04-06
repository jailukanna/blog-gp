---
title: mysql example v09script (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'v09script'

Functions in program: 
* `def drawcircle(canv,x,y,rad):`
* `def getPostsOfUser(userId):`
* `def deleteUser(userId):`
* `def changePassword(password, id):`
* `def addUser(username, password):`

Modules used in program: 
* `import mysql.connector`
* `import mysql.connector`

## python v09script

Python mysql example: v09script

```python
# 01 => MySQL i Python
#
# MySQL i Python funkcionisu veoma dobro i rad sa njima je 
# poprilicno jednostavan i fleksibilan. Za rad je potrebno 
# preuzeti MySQL kontektor sa interneta (ili univerzitetske
# mreze). Podrazumevano je da je XAMPP vec podesen i da je
# aktiv.
#
# Mysql kontektor omogucava komunikaciju izmedju RDBMS i 
# Python jezika.

# PROBLEM:
#
# Ono sto se moze ispostaviti kao problem jeste koriscenje 
# Python 3.~ verzije iz razloga jer MySQL ne podrzava verzije
# iznad 3.0. 

console> python --version
console> python -v 
3.5.1

# RESENJE:
#
# Postoje 2 resenja za taj problem:
# 1) Downgrade-ovati Python verziju na 2.7
# 2) Pronaci neku nezvanicnu biblioteku sa interneta.
#
# Prvo resenje predstavlja znatno lakse resenje pa se u ovim
# vezbama vracamo na verziju 2.7.
#
# U slucaju rada na odredjenom projektu morali biste da pronadjete
# adekvatno resenje za mysql-python-connector (2. resenje).
#
# Mysql-connector preuzeti sa:
https://dev.mysql.com/downloads/connector/python/

# Odabrati:
MSI Installer za x86/32-bit

# 02 => IMPORT MYSQL CONNECTOR 
#
# Sledeci korak jeste importovanje mysql modula. To se moze
# uraditi koristeci (kao sa proteklih vezbi) import keyword:

import mysql.connector

# ili:

from mysql import *

# U trenutku kada se pokrene program, ukoliko ne dodje do 
# Ukoliko prilikom pokretanja programa on ne bude zaustavljen
# zbog bilo kakve greske, mysql connector je uspesno importovan.

# 03 => ESTABLISHING A CONNECTION WITH MYSQL
#
# Uspostavljanje konekcije izmedju Python-a i Mysql-a je 
# krajnje jednostavno. Ono se realizuje pomocu dva objekta:
#
# cnx    -> connection string zasnovan na spec. parametrima
# cursor -> neophodan za izvrsavanje operacija poput SQL 
#					  izjava. Ovaj objekat interaguje sa MySQL serverom
#						tako sto koristi mysql konektor kao objekat.
#				    
# Primer uspostavljanja konekcije:

cnx = mysql.connector.connect(
				user     = 'root',
				password = '',
				host     = '127.0.0.1',
				database = 'cs324'
	)

cursor = cnx.cursor()

# Kako da znam da li je sve ok? - Nije doslo do greske.
# Drugi nacin uz pomocu kojeg se mozemo osigurati da je 
# konekcija na MySQL prosla uspesno (ili nije) jeste 
# koriscenje izuzetaka:

import mysql.connector
from mysql.connector import errorcode

try:
	cnx = mysql.connector.connect(
					user     = 'root',
					password = '',
					host     = '127.0.0.1',
					database = 'cs324'
		)

	cursor = cnx.cursor()
except mysql.connector.Error as err:
	if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
		print("Oops, username/pass/host may be wrong.")
	elif err.errno == errorcode.ER_BAD_DB_ERROR:
		print("Oops, database not exist.")
	else:
		print(err) # host wrong
else:
	print("Connection established!")

# Ukoliko je konekcija uspostavljena, rezultat je:
# Connection established!

# Ukoliko se unese pogresan user/pass parametar:
# Oops, username/pass/host may be wrong." + error msg

# Ukoliko se unese pogresna baza:
# Oops, database not exist + error msg

# 04 => CURSOR IN ACTION
#
# Kao sto je ranije receno, koriscenjem cursora se moze
# izvrsiti odredjeni upit nad bazom koja vraca rezultat.
#
# Sledeci primer ilustruje prikaz svih tabela u bazi

...

cursor.execute("SHOW TABLES") # priprema/izvrsava upit
tables = cursor.fetchall()		# dovlacenje rezultata iz db
print(tables)

# 05 => DATABASE DATA MANIPULATION
#
# Sledeci primeri ilustruju nacin na koji se moze manipulisati
# nad podacima jedne ili vise tabela:
#
#
# SELECT (selektovanja svih zapisa iz tabele):

cursor.execute("SELECT * FROM users")
users = cursor.fetchall()
print(users) # ret list of tupples

# Izvrsavanjem prethodnog primear moze se uociti format 
# rezultata upita. Format je predstavljen kao lista tupplova
# gde svaki tupple predstavlja jedan objekat, odnosno zapis.

[
 (1, u'miksi', u'cs324'), 
 (2, u'fifi', u'redcolor'), 
 (3, u'harry', u'poter321'), 
 (4, u'instaqueen', u'iliveforinstagram')
]

# Ovo predstavlja jos jedan primer gde su tupplovi izuzetno
# korisni.
#
# Dakle, s obzirom da je to lista, elementima se moze pristupiti
# na sledeci nacin:

print(users[0]) # ret 1st row: (1, u'miksi', u'cs324')

# Nakon ovoga, moze se pristupiti tacno odredjenoj vrednosti 
# elementa u okviru tuppla:

print(users[0][1]) # ret username: miksi
print(("UserId: %d | UserName: %s | UserPass: %s" % (users[1][0], users[1][1], users[1][2])))


# Dakle kroz prethodne primere se moze uociti na koji nacin 
# se dalje mogu obradjivati podaci.
#
# Sta ce se desiti ukoliko pokusate da promenite vrednosti
# users[0][1]?


# INSERT (Upisivanje podataka u bazu)
#
# Upisivanje u bazu je takodje dosta jednostavno s tim sto se 
# u okviru upisivanja pojavljuju koncepti poput commit/rollback
#
# commit()   - potvrdi i pokreni akciju prema bazi
# rollback() - u slucaju neuspeha, vrati prethodno stanje db

cursor.execute(
	"INSERT INTO users VALUES (%s, %s, %s)", 
	(None, "mici", "foksi")
	)
# cnx.commit()

# Ukoliko pokrenemo ovaj deo koda bez naknadnog commit-ovanja
# u bazi se nece desiti nikakve promene, program prolazi kroz
# ceo kod i ne nailazi na gresku, all works fine.
# U trenutku kada odkomentarisemo cnx.commit() python ce 
# potvrditi i aktivirati akciju upisivanja u tabelu users.

cursor.execute(
	"INSERT INTO users VALUES (%s, %s, %s)", 
	(None, "mici", "foksi")
	)
cnx.commit()

# Sta ce se desiti ukoliko dodje do neke grseke i cnx.commit
# ne bude mogao da uspesno izvrsi akciju? - Exception thrown
#
# Nprm. pokusajte da dodate primary key umesto None.

try:
	cursor.execute(
		"INSERT INTO users VALUES (%s, %s, %s)", 
		(None, "mici", "foksi")
		)
	cnx.commit()
except: # mysql.connector.errors.ProgrammingError
	print("Operation aborted.")
	cnx.rollback()
else:
	print("Success!")

# Bolji primer bi bio smestanje ovog bloka u metod addUser:

def addUser(username, password):
	try:
		cursor.execute(
			"INSERT INTO users VALUES (%s, %s, %s)", 
			(None, username, password)
		)
		cnx.commit()
	except:
		print("Operation aborted.")
		# Vraca na prethodno sigurno stanje (savepoint)
		# Vraca na poslednji commit
		# Omogucava da ne dodje do gresaka u bazi
		cnx.rollback()
	else:
		print("Success! Inserted.")

addUser("markonkr", "vanja")


# UPDATE (Azuriranje podataka iz baze)
#
# Azuriranje se vrsi na isti nacin kao prethodni upiti s tim
# sto su u ovom slucaju neophodni parametri u vidu novih 
# vrednosti.

cursor.execute(
	"UPDATE users SET user_name = 'Miksi' WHERE user_id = '1'"
	)

# Metoda koja omogucava promenu vrednosti lozinke, odnosno 
# koja ilustruje nacin prosledjivanja parametara u upit

def changePassword(password, id):
	try:
		cursor.execute(
			"UPDATE users SET user_password = %s WHERE user_id = %s",
			(password, id)
		)
		cnx.commit()
	except:
		print("Operation aborted.")
		cnx.rollback()
	else:
		print("Success! Updated.")

changePassword("newPass", 1)


# DELETE (Brisanje zapisa iz tabele)
#
# Kod brisanja zapisa potrebno je obratiti paznju na koji nacin
# se moraju prosledjivati parametri (koriscenje % nakon upita):

def deleteUser(userId):
	try:
		cursor.execute(
			"DELETE FROM users WHERE user_id = %s" % userId
		)
		cnx.commit()
	except:
		print("Operation aborted.")
		cnx.rollback()
	else:
		print("Success! Deleted.")


# DODAVANJE TABELE POSTS
#
# post_id (primary, ai) 
# user_id (foreign key)
# post_content (varchar(1024))

# JOIN TWO TABLES (Spajanje rezultata dveju table)
#
# Izvrsavanjem sledeceg upita nad bazom rezultovace spajanjem 
# rezultata iz dveju tabela (users i posts) tako da svaki od 
# zapisa (tupple) u listi odgovara jednom korisniku.
cursor.execute('''
		SELECT users.user_id, posts.post_id, users.user_name, posts.post_content 
		FROM users 
		JOIN posts 
		ON posts.user_id = users.user_id
		WHERE posts.user_id = 1
	''')
result = cursor.fetchall()
print(result)

# Rezultati su grupisani prema user_id atributu:

[
 (2, 1, u'fifi', u'I love being here with you!'), 
 (2, 2, u'fifi', u'I just fall in love'), 
 (1, 3, u'Miksi', u'I love milena glisic'), 
 (1, 4, u'Miksi', u'I love sneshka stankovic'), 
 (19, 5, u'mici', u'I am so sexy.')]
[Finished in 0.2s]

# Slanje upita za konkretnog korisnika (sa user_id = 1):

cursor.execute('''
		SELECT users.user_id, posts.post_id, users.user_name, posts.post_content 
		FROM users 
		JOIN posts 
		ON posts.user_id = users.user_id
		WHERE posts.user_id = 1
	''')
result = cursor.fetchall()
print(result)

# Rezultat jesu postovi korisnika pod korisnickim imenom 
# "Miksi":

[
 (1, 3, u'Miksi', u'I love milena glisic'), 
 (1, 4, u'Miksi', u'I love sneshka stankovic')
]

# Kreiranje metode getPostsOfUser:

def getPostsOfUser(userId):
	try:
		cursor.execute('''
			SELECT users.user_id, posts.post_id, users.user_name, posts.post_content 
			FROM users 
			JOIN posts 
			ON posts.user_id = users.user_id
			WHERE posts.user_id = %s
		''' % userId)

		result = cursor.fetchall()

		for row in result:
			print(row[2] + ": " + row[3])

	except:
		print("Operation aborted.")
	else:
		print("Success!")	

getPostsOfUser(2)


# 06 => Python GUI
#
# Ukoliko se ikada odlucite da koristite Python GUI verovatno
# cete koristiti Tkinter GUI biblioteku. S obzirom da je okvir
# ovih vezbi python i mysql ne zadrzavamo se toliko na ovome.
#
# OSNOVNI GUI POJMOVI:
#
# Window         - celokupna aplikacija
# Top-Lvl Window - prozor bez roditelja, default os stil
# Widget 				 - gui element
# Frame          - osnovna jedinica za GUI (parent svih elemenata)
#
# Importovanje:

from Tkinter import * # za 2.~ verzije
from tkinter import * # za 3.~ verzije

# 1) Kreiranje osnovnog prozora
#
# - Klasa App nasledjuje klasu Frame, komponentu Tkinter 
#   modula
# - Parent se jos uvek ne koristi kroz ovaj primer.
# - Kroz konstruktor se vrsi nasledjivanje (self je bitan)
# - createWidgets() predstavlja praznu metodu u okviru koje
#   bi se eventualno dodavale gui komponente koje bi se 
#   ispisivale na glavnom panelu.
# - mainloop() sluzi za pokretanje samog gui programa.

class App(Frame):

    def __init__(self, parent = None):
        Frame.__init__(self, parent)

app = App()
app.master.title("hello")
app.mainloop()

# 2) Dodavanje gui elemenata
#
# Sledeci primer prikazuje dodavanje novih gui elemenata
# kroz createWidgets metod.
#
# - self.grid() omogucava kreiranje custom grida.

from Tkinter import *

class App(Frame):

    def __init__(self, parent=None):
        Frame.__init__(self, parent)
        self.grid() 
        self.createWidgets()

    def createWidgets(self):
	    self.myButton = Button(self, text='Quit')
	    self.myButton.grid()

	    self.myLabel = Label(self, text='Labela')
	    self.myLabel.grid()

	    self.myInput = Entry(self, text='Input')
	    self.myInput.grid()


app = App()
app.master.title('Simple app')
app.mainloop()

# 3) Events & callbacks
#
# Tkinter je event-based gui biblioteka. To znaci da se za
# GUI elemente mogu vezivati listeneri koji bivaju pozvani 
# u odredjenoj situaciji (npr. na klik).

from Tkinter import *

class App(Frame):

    def __init__(self, parent=None):
        Frame.__init__(self, parent)
        self.grid()
        self.createWidgets()

    def createWidgets(self):
        self.myInput = Entry(self, text='Entry')
        self.myInput.grid()
        
        self.myButton = Button(self, text='Enter', 
        														 command=self.getInput)
        self.myButton.grid()

    def getInput(self):
        print("Val is: ", self.myInput.get())

app = App()
app.master.title('Simple app')
app.mainloop()

# 4) Primer Python GUI programa koji omogucava unos itema u 
# listu kao i brisanje istih iz nje (obratiti paznju na nacin
# pisanja koda):

from Tkinter import *

class Application(Frame):

    def __init__(self, parent=None):
        Frame.__init__(self, parent)
        self.grid()
        self.widgets(parent)

    # Metoda u kojoj su setovani gui elementi
    def widgets(self, master):
        # label
        self.label = Label(master, text="Enter some text:")
        self.label.grid(column=0, row=0)
        
        # input_field
        self.input = Entry(master)
        self.input.grid(column=0, row=1)

        # button insert
        self.button = Button(master, text="Insert", command=self.add_to_list)
        self.button.grid(column=0, row=2)

        # button delete
        self.button = Button(master, text="Delete", command=self.delete_from_list)
        self.button.grid(column=0, row=3)

        # listbox
        self.listbox = Listbox(master)
        self.listbox.grid(column=0, row=4)

    # Metoda koja dodaje element u listbox
    def add_to_list(self):
        if(self.input.get() == ""):
            print("Error message", "Cant insert empty space.")
        else:
            self.listbox.insert(END, self.input.get())

    # Metoda koja brise element iz listbox
    def delete_from_list(self):
        self.listbox.delete(END, END)

app = Application()
app.master.title("My first GUI")
app.mainloop()

# 5) Grid primer
#
# Tkinter grid funkcionise i formatira gui elemente na osnovu
# kolona, odnosno rovova. Svaki prozor GUI programa podeljen
# je na kolone, odnosno rovove u okviru kojih se smestaju gui
# elementi prema tacno odredjenim pozicijama

from tkinter import *

class Application(Frame):
  def __init__(self, master=None):
    Frame.__init__(self, master)
    self.grid()
    self.createWidgets()

  def createWidgets(self):
    b = Button(text="1")
    b.grid(column=0, row=0)
    b = Button(text="2")
    b.grid(column=1, row=0)
    b = Button(text="3")
    b.grid(column=2, row=0)

    b = Button(text="4")
    b.grid(column=0, row=1)
    b = Button(text="5")
    b.grid(column=1, row=1)
    b = Button(text="6")
    b.grid(column=2, row=1)

    b = Button(text="7")
    b.grid(column=0, row=2)
    b = Button(text="8")
    b.grid(column=1, row=2)
    b = Button(text="9")
    b.grid(column=2, row=2)

    b = Button(text="0")
    b.grid(column=0, row=3, columnspan=2, sticky=E+W)
    b = Button(text=".")
    b.grid(column=2, row=3, sticky=E+W)

app = Application()
app.master.title('Sample application')
app.mainloop()

# 6) Python i Kanvas
#
# Iz prethodnih primera se moze videti da je Python Tkinter
# biblioteka dosta jednostavna za koriscenje. Sledeci primer
# prikazuje koriscenje Kanvasa koji iscrtava element na 
# prozoru

from Tkinter import *
root = Tk()

def drawcircle(canv,x,y,rad):
    canv.create_oval(x-rad,y-rad,x+rad,y+rad,width=0,fill='blue')
    
canvas = Canvas(width=200, height=200,N bg='yellow')  
canvas.pack(expand=YES, fill=BOTH) 
text = canvas.create_text(50,10, text="tk circle text")

circ1=drawcircle(canvas,100,100,20)          
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
