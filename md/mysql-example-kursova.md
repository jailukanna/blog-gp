---
title: mysql example kursova (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'kursova'

Functions in program: 
* `def main():`
* `def wordpressinstall():`
* `def worpress():`
* `def rewrite():`
* `def apache2mysqlconfigurations():`
* `def apache2configuration():`
* `def mysqlconfiguration():`
* `def php():`
* `def apache2():`
* `def mysql():`

Modules used in program: 
* `import os`
* `import subprocess`

## python kursova

Python mysql example: kursova

```python
#!/usr/bin/python
import subprocess
from subprocess import PIPE
import os

try:
    import mysql.connector
except Exception:
    print("Important files aren't installed")
    req = input("""\n Press 1 to install files and continue
    press other button for exit:""")
    if req == "1":

        subprocess.run(["sudo", "apt-get", "install", "python3-pip"], stdout=PIPE, stderr=PIPE, shell=False)
        subprocess.run(["pip3", "install", "mysql-connector-python-rf"], stdout=PIPE, stderr=PIPE, shell=False)
        os.system("clear")
        import mysql.connector
    else:
        print("Bye :(")
        exit()

def mysql():
    subprocess.run(["sudo", "apt-get", "update"], stdout=PIPE, stderr=PIPE)
    subprocess.run(["sudo", "apt-get", "install", "mysql-server"], stdout=PIPE, stderr=PIPE, shell=False)
    subprocess.run(["sudo", "systemctl", "start", "mysql"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
def apache2():
    subprocess.run(["sudo", "apt-get", "update"], stdout=PIPE, stderr=PIPE)
    subprocess.run(["sudo", "apt-get", "install", "apache2"], stdout=PIPE, stderr=PIPE, shell=False)
    subprocess.run(["sudo", "systemctl", "restart", "apache2"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
def php():
    subprocess.run(["sudo", "apt-get", "update"], stdout=PIPE, stderr=PIPE)
    subprocess.run(["sudo", "apt-get", "install", "php-curl", "php-gd", "php-mbstring", "php-mcrypt", "php-xml", "php-xmlrpc"],   stdout=PIPE, stderr=PIPE, shell=False)
def mysqlconfiguration():
    logw = input('Enter name for your wordpress user')
    passw = input('Enter password for your wordpress user')
    db = connection.MySQLConnection(user='root', password='vick2506',
                                    host='localhost')
    mycursor = db.cursor()
    mycursor.execute("CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci")
    mycursor.execute("GRANT ALL ON wordpress.* TO" + logw + "@'localhost' IDENTIFIED BY" + passw)
def apache2configuration():
    path_to_output_file = "/etc/apache2/apache2.conf"
    with open(path_to_output_file, 'r') as f:
        data = f.readlines()
    data[169] = '<Directory /var/www/>\n'
    data[170] = '      Options Indexes FollowSymLinks\n'
    data[171] = '      AllowOverride All\n'
    data[172] = '      Require all granted\n'
    data[173] = '</Directory>\n'
    with open(path_to_output_file, 'w') as f:
        f.writelines(data)
def apache2mysqlconfigurations():
    rows = os.system("echo $USER")
    os.system("sudo wget -c https://api.wordpress.org/secret-key/1.1/salt/ >> 10.txt")
    logw = input('Enter name for your wordpress user again')
    passw = input('Enter password for your wordpress user again')
    path_to_output_file1 = '/var/www/wordpress/wp-config.php'
    path_to_output_file2 = "/home//"+rows+"/10.txt"
    with open(path_to_output_file2, 'r') as f:
        data = [row.strip() for row in f]
    with open(path_to_output_file1, 'r') as f:
        text = f.readlines()
    text[22] = "define('DB_NAME', 'wordpress');" + '\n'
    text[25] = "define('DB_USER'," + logw + ");" + "\n"
    text[28] = "define('DB_PASSWORD'," + passw + ");" + "\n"
    text[29] = "define('FS_METHOD', 'direct');" + "\n"
    text[48] = data[0] + '\n'
    text[49] = data[1] + '\n'
    text[50] = data[2] + '\n'
    text[51] = data[3] + '\n'
    text[52] = data[4] + '\n'
    text[53] = data[5] + '\n'
    text[54] = data[6] + '\n'
    text[55] = data[7] + '\n'
    with open(path_to_output_file1, 'w') as f:
        f.writelines(text)
def rewrite():
    subprocess.run(['sudo', 'a2enmod', 'rewrite'], stdout=PIPE)
    subprocess.run(["sudo", "systemctl", "restart", "apache2"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
def worpress():
    rows = os.system("echo $USER")
    os.system("wget -c https://wordpress.org/latest.tar.gz")
    os.system("tar xzvf latest.tar.gz")
    os.system("mv /home/"+rows+"/wordpress /tmp")
def wordpressinstall():
    subprocess.run(['touch', '/tmp/wordpress/.htaccess'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['chmod', '660', '/tmp/wordpress/.htaccess'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['cp', '/tmp/wordpress/wp-config-sample.php', '/tmp/wordpress/wp-config.php'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['mkdir', '/tmp/wordpress/wp-content/upgrade'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['sudo', 'cp', '-a', '/tmp/wordpress/.', '/var/www/wordpress'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['sudo', 'chown', '-R', 'www-data:www-data', '/var/www/wordpress'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['sudo', 'find', '/var/www/wordpress/', '-type', 'd', '-exec', 'chmod', '750', '{}', '\;'], stdout=PIPE, stderr=PIPE)
    subprocess.run(['sudo', 'find', '/var/www/wordpress/', '-type', 'f', '-exec', 'chmod', '640', '{}', '\;'], stdout=PIPE, stderr=PIPE)
    print("Succesful!!! Continue set up in the installation through the Web Interface")


def main():
    os.system('clear')
    while True:
        print("""\n     INSTALLING WORDPRESS FOR UBUNTU 18.04
        1. INSTALL AMP
        2. INSTALL WORDPRESS
        3. SET UP CONFIGURATION
        4. EXIT""")
        check = input("Write your choice:")
        if check == "1":
            os.system('clear')
            print("""\n    Please, choice what you want to install
            1. INSTALL MYSQL-SERVER
            2. INSTALL APACHE2
            3. INSTALL PHP-packages
            4. EXIT""")
            choice = input("Write your choice:")
            if choice == "1":
                mysql()
                print("Installing MYSQL-server...")
                print("Set up configurations")
            elif choice == "2":
                apache2()
                print("Installing apache2...")
            elif choice == "3":
                php()
                print("Installing PHP-packages")
            else:
                print("Unknown option!!!")
        if check == "2":
            os.system('clear')
            worpress()
            wordpressinstall()
        if check == "3":
            try:
                import mysql.connector
                from mysql.connector import (connection)
                os.system('clear')
                mysqlconfiguration()
                apache2configuration()
                apache2mysqlconfigurations()
                rewrite()
            except ImportError:
                print("MySQLconnection doesn't install")
        if check == "4":
            os.system('clear')
            print("Please, use this app again :)")
            break
        else:
            print("Unknown iption selected")

if __name__ == '__main__':
        main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
