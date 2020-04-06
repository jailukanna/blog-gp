---
title: mysql example jarvis (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'jarvis'

Functions in program: 
* `def phonecall():`
* `def message():`
* `def is_valid_google_search(phrase):`
* `def myCommand():`
* `def play_sound(mp3_list):`
* `def greetMe():`
* `def speak(audio):`

Modules used in program: 
* `import sys`
* `import os`
* `import mysql.connector as myc`
* `import wolframalpha`
* `import datetime`
* `import wikipedia`
* `import speech_recognition as sr`
* `import random`
* `import smtplib`
* `import ctypes`

## python jarvis

Python mysql example: jarvis

```python
import ctypes
from pyttsx3 import init
from webbrowser import open
import smtplib

import random
import speech_recognition as sr
import wikipedia
import datetime
import wolframalpha

from playsound import playsound
from twilio.rest import Client
import mysql.connector as myc
import os
import sys

engine = init('sapi5')

account_sid ='AC638968186ce37229dc43fae9207eb57e'
auth_token = 'bdea460acb8e983869ba55ac0c38ab26'

client = wolframalpha.Client('YYLPXJ-XKYUJAE7JJ')
client = Client(account_sid, auth_token)

voices = engine.getProperty('voices')  # getting details of current voice
engine.setProperty('voice', voices[1].id)  # changing index, changes voices. 1 for female


rate = engine.getProperty('rate')
engine.setProperty('rate', 175)

voices = engine.getProperty('voices')
engine.setProperty('voice', voices[len(voices) - 1].id)

volume = engine.getProperty('volume')
engine.setProperty('volume', 1.0)

mydb = myc.connect(host='localhost', user='root', passwd='', database='test')

google_searches_dict = {'what': 'what', 'why': 'why', 'who': 'who', 'which': 'which'}


def speak(audio):
    print('Computer: ' + audio)
    engine.say(audio)
    engine.runAndWait()


def greetMe():
    currentH = int(datetime.datetime.now().hour)
    if currentH >= 0 and currentH < 12:
        speak('Good Morning!')

    if currentH >= 12 and currentH < 18:
        speak('Good Afternoon!')

    if currentH >= 18 and currentH != 0:
        speak('Good Evening!')


greetMe()



speak('Hello Sir, this is your artificial inteligence friday')
speak('How may I help you?')

def play_sound(mp3_list):
    mp3 = random.choice(mp3_list)
    playsound(mp3)


def myCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        query = r.recognize_google(audio, language='en-in')
        print('User: ' + query + '\n')

    except sr.UnknownValueError:
        speak('Sorry sir! I didn\'t get that! Try typing the command!')
        query = str(input('Command: '))

    except sr.RequestError as e :
        print('NETWORK ERROR...')

    return query

def is_valid_google_search(phrase):
    if google_searches_dict.get(phrase.split(' ')[0]) == phrase.split(' ')[0]:
        return True

def message():
    message = client.messages.create(
        body='hello sir this is artificial integence sir',
        to='+919423246403',
        from_='+12178614810',
    )
    return message.sid

def phonecall():
    call = client.calls.create(
        to='+919423246403',
        from_='+12178614810',
        url="http://demo.twilio.com/docs/voices.xml"
    )
    return call.sid



if __name__ == '__main__':

    while True:

        query = myCommand()
        query = query.lower()

        mycursor = mydb.cursor()

        if 'hello'in query or 'hi' in query :
            playsound('mp3/voices/greeting_accept_1.mp3', 'mp3/voices/greeting_accept_2.mp3')
            print('IN GREETING...')
            continue

        elif 'open' in query:
            speak('okay')
            playsound('mp3/voices/open_1.mp3', 'mp3/voices/open_2.mp3')
            print('IN OPEN...')
            os.system('explorer C:\\"{}"'.format(query.replace('open ', '')))
            continue

        elif 'call' in query :
            speak('okay')
            playsound('mp3/voices/phonecall.mp3')
            print('IN CALLING...')
            a = phonecall()
            print(a)
            continue

        elif 'message' in query:
            speak('okay')
            speak('sending your message.')
            print('IN MESSAGE...')
            b = message()
            print(b)
            continue

        elif 'youtube' in query:
            speak('okay')
            playsound('mp3/voices/google_search_1.mp3', 'mp3/voices/google_search_2.mp3')
            print('IN YOUTUBE...')
            open('www.youtube.com')
            continue

        elif 'facebook' in query:
            speak('okay')
            playsound('mp3/voices/social_media.mp3')
            print('IN FACEBOOK...')
            open('www.facebook.com')
            continue

        elif 'twitter' in query:
            speak('okay')
            playsound('mp3/voices/social_media.mp3')
            print('IN TWITTER...')
            open('www.twitter.com')
            continue

        elif 'google' in query:
            speak('okay')
            playsound('mp3/voices/google_search_1.mp3', 'mp3/voices/google_search_2.mp3')
            print('IN GOOGLE...')
            open('www.google.co.in')
            continue

        elif 'gmail' in query:
            speak('okay')
            playsound('mp3/voices/google_search_1.mp3', 'mp3/voices/google_search_2.mp3')
            print('IN GMAIL...')
            open('www.gmail.com')
            continue


        elif 'contact list' in query :
            speak('okay')
            playsound('mp3/voices/contact.mp3')
            mycursor.execute('select * from contactlist')

            for x in mycursor:
                print(x)
            continue

        elif "what\'s up" in query or 'how are you' in query:
            stMsgs = ['Just doing my thing!', 'I am fine!', 'Nice!', 'I am nice and full of energy']
            speak(random.choice(stMsgs))
            continue

        elif 'email' in query:
            speak('Who is the recipient? ')
            recipient = myCommand()

            if 'me' in recipient:
                try:
                    speak('What should I say? ')
                    content = myCommand()

                    server = smtplib.SMTP('smtp.gmail.com', 587)
                    server.ehlo()
                    server.starttls()
                    server.login("Your_Username", 'Your_Password')
                    server.sendmail('Your_Username', "Recipient_Username", content)
                    server.close()
                    speak('Email sent!')

                except:
                    speak('Sorry Sir! I am unable to send your message at this moment!')

        elif 'stop' in query:
            speak('okay')
            speak('Bye Sir, have a good day.')
            sys.exit()

        elif 'thank you' in query:
            playsound('mp3/voices/thankyou_1.mp3', 'mp3/voices/thankyou_2.mp3')
            continue

        elif 'play music' in query:
            music_folder = 'C:\\Users\\KV2\\Desktop\\python_songs\\'
            music = ['Amplifier', 'Despacito', 'she-move-like-it', 'shape-of-you']
            random_music = music_folder + random.choice(music) + '.mp3'
            os.system(random_music)

            speak('Okay, here is your music! Enjoy!')
            continue

        elif is_valid_google_search(query):
            open('https://www.google.co.in/search?q={}'.format(query))
            continue

        elif 'lock' in query:
            playsound('mp3/voices/lock.mp3')
            for value in ['pc', 'system', 'windows']:
                ctypes.windll.user32.LockWorkStation()


        else:
            query = query
            speak('Searching...')
            try:
                try:
                    res = client.query(query)
                    results = next(res.results).text
                    speak('WOLFRAM-ALPHA says - ')
                    speak('Got it.')
                    speak(results)

                except:
                    results = wikipedia.summary(query, sentences=2)
                    speak('Got it.')
                    speak('WIKIPEDIA says - ')
                    speak(results)

            except:
                open('www.google.com')

        speak('Next Command! Sir!')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
