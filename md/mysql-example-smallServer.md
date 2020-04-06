---
title: mysql example smallServer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'smallServer'

Functions in program: 
* `def navBar():`
* `def textPage():`
* `def aboutUs():`
* `def videoMain():`
* `def workers():`
* `def norm():`
* `def index():`

Modules used in program: 
* `import sys`

## python smallServer

Python mysql example: smallServer

```python
from flask import Flask, flash, redirect, render_template, request, url_for
from flaskext.mysql import MySQL
import sys

mysql = MySQL()

app = Flask(__name__)
app.secret_key = 'many random bytes'

app.config['MYSQL_DATABASE_USER'] = 'root'
app.config['MYSQL_DATABASE_PASSWORD'] = '1234'
app.config['MYSQL_DATABASE_DB'] = 'dbname'
app.config['MYSQL_DATABASE_HOST'] = 'localhost'
mysql.init_app(app)

testVarialble = "hello testing testing..."

@app.route("/")
def index():
    connection = mysql.connect()
    cursor = connection.cursor()

    queryFind = "SELECT * from aboutUs"
    cursor.execute(queryFind)
    aboutUs = cursor.fetchall()

    queryFind = "SELECT * from aboutUs_extras"
    cursor.execute(queryFind)
    aboutUs_extra = cursor.fetchall()

    queryFind = "SELECT * from navBar"
    cursor.execute(queryFind)
    navBar = cursor.fetchall()

    queryFind = "SELECT * from textPage"
    cursor.execute(queryFind)
    textPage = cursor.fetchall()[0]

    queryFind = "SELECT * from videoMain WHERE v_name='constant'"
    cursor.execute(queryFind)
    videoMain = cursor.fetchall()

    queryFind = "SELECT * from Workers"
    cursor.execute(queryFind)
    workers = cursor.fetchall()
   
    return render_template('index.html', extras=aboutUs_extra, dataAboutUs=aboutUs, dataSelected="", dataNavBar=navBar,dataTextPage=textPage, dataVideoMain=videoMain, dataWorkers=workers)

@app.route("/admin", methods=['GET', 'POST'])
def norm():
    # get All the tables data. And send it to 
    connection = mysql.connect()
    cursor = connection.cursor()

    queryFind = "SELECT * from aboutUs"
    cursor.execute(queryFind)
    aboutUs = cursor.fetchall()

    queryFind = "SELECT * from aboutUs_extras"
    cursor.execute(queryFind)
    aboutUs_extra = cursor.fetchall()

    queryFind = "SELECT * from navBar"
    cursor.execute(queryFind)
    navBar = cursor.fetchall()

    queryFind = "SELECT * from textPage"
    cursor.execute(queryFind)
    textPage = cursor.fetchall()

    queryFind = "SELECT * from videoMain WHERE v_name='constant'"
    cursor.execute(queryFind)
    videoMain = cursor.fetchall()

    queryFind = "SELECT * from Workers"
    cursor.execute(queryFind)
    workers = cursor.fetchall()

    if request.method == "POST":
        aboutUsB = request.form.get('aboutUsButton')
        navBarB = request.form.get('navBarButton')
        textPageB = request.form.get('textPageButton')
        videoMainB = request.form.get('videoMainButton')
        workersB = request.form.get('workersButton')

        if aboutUsB == "val" :
            return redirect(url_for('aboutUs'), extras=aboutUs_extra[0], data=aboutUs[0], dataSelected="")

        if navBarB == "val" :
            return redirect(url_for('navBar'), data=navBar)
        
        if textPageB == "val" :
            return redirect(url_for('textPage'), data=textPage)
        
        if videoMainB == "val" : 
            return redirect(url_for('videoMain'), data=videoMain)

        if workersB == "val" :
            return redirect(url_for('Workers'), data=workers)
        

    return render_template('indexAdmin.html', extras=aboutUs_extra, dataAboutUs=aboutUs, dataSelected="", dataNavBar=navBar,dataTextPage=textPage, dataVideoMain=videoMain, dataWorkers=workers)
@app.route("/workers", methods=['GET','POST'])
def workers():
    data = ""
    connection = mysql.connect()
    cursor = connection.cursor()
    if request.method == "POST":
            #variables read BEGIN
            name = request.form.get('name')
            position = request.form.get('position')
            facebook_link = request.form.get('facebook_link')
            google_link = request.form.get('google_link')
            twitter = request.form.get('twitter')
            email = request.form.get('email')
            about = request.form.get('about')
            image_path = request.form.get('image_path')
            #variables read END
            searchButton = request.form.get('searchEnter')
            searchName = request.form.get('searchName')
            if searchButton == "enterSearch":
                queryFind = "SELECT * from Workers WHERE name='" + searchName + "'"
                cursor.execute(queryFind)
                data = cursor.fetchone()
                queryFind = "SELECT * from Workers ORDER BY name"
                cursor.execute(queryFind)
                people = cursor.fetchall()
                return render_template('workers.html', people=people,data=data)
            queryFind = "SELECT * from Workers WHERE name='" + name + "'"
            cursor.execute(queryFind)
            data = cursor.fetchone()
            if data is not None:
                queryDetele = "DELETE FROM Workers WHERE name='" + name + "'"
                cursor.execute(queryDetele)
                connection.commit()       
            queryInsert = "INSERT INTO Workers (name, position, facebook_link, google_link, twitter, email, about, image_path) VALUES ('" + name + "','" + position + "','" + facebook_link + "','" + google_link + "','" + twitter + "','" + email + "','" + about + "','" + image_path + "')"
            cursor.execute(queryInsert)
            connection.commit()
            data = [name, position, facebook_link, google_link, twitter, about, image_path]
        #except:
         #   return str(e)
    queryFind = "SELECT * from Workers ORDER BY name"
    cursor.execute(queryFind)
    people = cursor.fetchall()
    return render_template('workers.html', people=people,data=data)

@app.route("/videoMain", methods=['GET','POST'])
def videoMain():
    data = ""
    connection = mysql.connect()
    cursor = connection.cursor()
    queryFind = "SELECT * from videoMain WHERE v_name='constant'"
    cursor.execute(queryFind)
    data = cursor.fetchone()

    if request.method == "POST":

            searchButton = request.form.get('enterButton')

            #variables read BEGIN
            if searchButton == "enterValue":
                name = "nm"
                v_imgLink = request.form.get('v_imgLink')
                v_webmLink = request.form.get('v_webmLink')
                v_mp4Link = request.form.get('v_mp4Link')
                hSmall = request.form.get('hSmall')
                hBig = request.form.get('hBig')
                hLast = request.form.get('hLast')
                if data is not None:
                    queryDetele = "DELETE FROM videoMain WHERE v_name='constant'"
                    cursor.execute(queryDetele)
                    connection.commit()       
                queryInsert = "INSERT INTO videoMain (v_imgLink, v_webmLink, v_mp4Link, hSmall, hBig, hLast, v_name) VALUES ('" + v_imgLink + "','" + v_webmLink + "','" + v_mp4Link + "','" + hSmall + "','" + hBig + "','" + hLast + "', 'constant')"
                cursor.execute(queryInsert)
                connection.commit()
                data = [v_imgLink, v_webmLink, v_mp4Link, hSmall, hBig, hLast]
        #except:
         #   return str(e)

    return render_template('videoMain.html', data=data)





@app.route("/aboutUs", methods=['GET','POST'])
def aboutUs():
    data = ""
    connection = mysql.connect()
    cursor = connection.cursor()
    if request.method == "POST":
            #variables read BEGIN
            titleMain = request.form.get('titleMain')
            mainHeader = request.form.get('mainHeader')
            mainBody = request.form.get('mainBody')
            header = request.form.get('header')
            body = request.form.get('body')

            #variables read END
            searchButton = request.form.get('searchEnter')
            searchName = request.form.get('searchName')
            if header is not None :
                #littleTiles
                queryFind = "SELECT * from aboutUs_extras WHERE header='" + header + "'"
                cursor.execute(queryFind)
                finding = cursor.fetchone()

                if finding : 
                    queryDelete = "DELETE FROM aboutUs_extras WHERE header='" + header + "'"
                    cursor.execute(queryDelete)
                    connection.commit()
                
                queryInsert = "INSERT INTO aboutUs_extras (header, body) VALUES ('" + header + "','" + body + "')"
                cursor.execute(queryInsert)
                connection.commit()

                queryFind = "SELECT * from aboutUs WHERE title='" + titleMain + "'"
                cursor.execute(queryFind)
                data = cursor.fetchone()

                if searchButton == "enterSearch":
                    queryFind = "SELECT * from aboutUs_extras WHERE header='" + searchName + "'"
                    cursor.execute(queryFind)
                    dataSelected = cursor.fetchone()
                    queryFind = "SELECT * from aboutUs_extras ORDER BY header"
                    cursor.execute(queryFind)
                    extras = cursor.fetchall()
                    return render_template('aboutUs.html', extras=extras,data=data, dataSelected=dataSelected)

                queryDetele = "DELETE FROM aboutUs"
                cursor.execute(queryDetele)
                connection.commit()       
                queryInsert = "INSERT INTO aboutUs (title, mainHeader, mainBody) VALUES ('" + titleMain + "','" + mainHeader + "','" + mainBody + "')"
                cursor.execute(queryInsert)
                connection.commit()
                data = [titleMain, mainHeader, mainBody]
        #except:
         #   return str(e)
    queryFind = "SELECT * from aboutUs_extras ORDER BY header"
    cursor.execute(queryFind)
    extras = cursor.fetchall()

    searchName = request.form.get('searchName')
    if searchName and searchName is not None:
        queryFind = "SELECT * from aboutUs_extras WHERE header='" + searchName + "'"
        cursor.execute(queryFind)
        dataSelected = cursor.fetchone()
    else :
        dataSelected = ["",""]

    queryFind = "SELECT * from aboutUs"
    cursor.execute(queryFind)
    data = cursor.fetchone()
    return render_template('aboutUs.html', extras=extras,data=data, dataSelected=dataSelected)




@app.route("/textPage", methods=['GET','POST'])
def textPage():
    data = ""
    connection = mysql.connect()
    cursor = connection.cursor()
    queryFind = "SELECT * from textPage"
    cursor.execute(queryFind)
    data = cursor.fetchone()

    if request.method == "POST":
            #variables read BEGIN
            title = request.form.get('title')
            leftTop = request.form.get('leftTop')
            leftBot = request.form.get('leftBot')
            rightTop = request.form.get('rightTop')
            rightBot = request.form.get('rightBot')
            if data:
                queryDetele = "DELETE FROM textPage"
                cursor.execute(queryDetele)
                connection.commit()       
            queryInsert = "INSERT INTO textPage (title, leftTop, leftBot, rightTop, rightBot) VALUES ('" + title + "','" + leftTop + "','" + leftBot + "','" + rightTop + "','" + rightBot + "')"
            cursor.execute(queryInsert)
            connection.commit()
            data = [title, leftTop, leftBot, rightTop, rightBot]
        #except:
         #   return str(e)

    return render_template('textPage.html', data=data)


@app.route("/navBar", methods=['GET','POST'])
def navBar():
    data = ""
    connection = mysql.connect()
    cursor = connection.cursor()

    if request.method == "POST":
            #variables read BEGIN
            title = request.form.get('title')
            deleteButton = request.form.get('deleteButton')
            insertButton = request.form.get('insertButton')
            if deleteButton == "delete":
                queryDelete = "DELETE FROM navBar WHERE title='" + title + "'"
                cursor.execute(queryDelete)
                connection.commit()
            if insertButton == "insert":
                queryInsert = "INSERT INTO navBar (title) VALUES ('" + title + "')"
                cursor.execute(queryInsert)
                connection.commit()
        #except:
         #   return str(e)
    queryFind = "SELECT * from navBar"
    cursor.execute(queryFind)
    data = cursor.fetchall()
    return render_template('navBar.html', data=data)


if __name__ == "__main__":
    app.run(debug = True)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
