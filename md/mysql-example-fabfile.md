---
title: mysql example fabfile (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'fabfile'

Functions in program: 
* `def django_collect_static():`
* `def django_migrate():`
* `def create_superuser():`
* `def setup_mysql():`
* `def setup_gunicorn():`
* `def setup_nginx():`
* `def install_venv_requirements():`
* `def create_virtualenv():`
* `def set_permissions():`
* `def sync_files():`
* `def make_dirs():`
* `def add_grp_to_user():`
* `def install_os_packages():`
* `def update_server():`
* `def print_success(message):`
* `def print_error(message):`
* `def print_title(title):`
* `def services_status():`
* `def restart_db_services():`
* `def restart_web_services():`
* `def setup_website():`
* `def install_webserver():`
* `def redeploy():`
* `def add_ssh_key(path=None):`

Modules used in program: 
* `import socket`

## python fabfile

Python mysql example: fabfile

```python
#encoding:UTF-8

# =============================================================================
# This fabfile will turn a Raspberry Pi into a webserver,
# See http://www.kittley.com/2018/04/04/blog-sdstore-and-pi/ for more details.
# =============================================================================

import socket
from os import sep, remove
from fabric.api import cd, lcd, task
from fabric.operations import run, local, prompt, put, sudo
from fabric.network import needs_host
from fabric.state import env, output
from fabric.contrib import files
from fabric.contrib.project import rsync_project
from fabtools import mysql
from fabtools import user, group, require, deb
from fabtools.python import virtualenv, install_requirements, install
from termcolor import colored
from unipath import Path, DIRS

# =============================================================================
# SETTINGS 
# =============================================================================

class Settings:
    DEPLOY_USER = "pi"                      # Username of owner
    DEPLOY_GRP = "www-data"                 # Usergroup for webservices
    ROOT_NAME = "sdstore-demo"              # A system friendly name for the project
    DIR_PROJ = "/srv/" + ROOT_NAME + "/"    # The root of this project folder
    DIR_CODE = DIR_PROJ + 'src/'            # Where the website source for this project will live
    DIR_LOGS = DIR_PROJ + 'logs/'           # Where the log files for this project will live
    DIR_ENVS = DIR_PROJ + 'envs/'           # Where the Virtual environments for this project will live
    DIR_VENV = DIR_ENVS + ROOT_NAME + "/"   # The name of the virtual environment to create
    DIR_SOCK = DIR_PROJ + 'sockets/'        # Where the sockets will be stored
    LOCAL_DIR_CODE = "./"                   # Where the local website source is stored
    # Python Version
    PYVERSION = (3,5,3)
    PYVFULL   = ".".join([str(x) for x in PYVERSION])
    PYVMM     = ".".join([str(x) for x in PYVERSION[:2]])
    # Requirements
    REQUIRMENTS_FILES = [
        DIR_CODE + 'requirements.txt',
        DIR_CODE + 'sd_store/requirements.txt'
    ]
    APP_ENTRY_POINT = "sdstore-demo.wsgi"   # You will need to change the sdstore-demo part to match the name of the django project you created.
    # Database
    DB_NAME      = "pidatabase"             # Database name to create
    DB_USER_NAME = "sdstore"                # Database username to create
    DB_PASSWORD  = "secret-password"        # Database password for newly created user
    

# =============================================================================
# END OF SETTINGS 
# =============================================================================

env.user = Settings.DEPLOY_USER

@task
def add_ssh_key(path=None):
    if path is None:
        print_error("You must specify a path e.g. fab add_ssh_key:/user/name/.ssh/id_rsa.pub")
        return
    elif '.pub' not in path:
        print_error("Are you sure this is a public key? It doesn't end in .pub")
        return
    print_title('Adding SSH Key to remote')
    user.add_ssh_public_key(env.user, path)

@task
def redeploy():
    sync_files()
    set_permissions()
    install_venv_requirements()
    django_migrate()
    django_collect_static()
    restart_web_services()

@task
def install_webserver():
    # OS
    update_server()
    install_os_packages()
    add_grp_to_user()

@task
def setup_website():
    make_dirs()
    sync_files()
    set_permissions()
    create_virtualenv()
    install_venv_requirements()
    set_permissions()
    # Web server
    setup_nginx()
    setup_gunicorn()
    restart_web_services()
    # Database & Requirements
    setup_mysql()
    restart_db_services()
    # Django Tasks
    django_migrate()
    django_collect_static()


# =============================================================================
# SUB TASKS
# =============================================================================

# Restart webservices
@task
def restart_web_services():
    print_title('Restarting Web Service - nginx and gunicorn')
    sudo('systemctl daemon-reload')
    sudo('systemctl restart nginx')
    sudo('systemctl restart gunicorn')

# Restart background services
@task
def restart_db_services():
    print_title('Restarting database services')
    sudo('service mysqld restart')


# Restart background services
@task
def services_status():
    print_title('journalctl since yeterday')
    sudo('journalctl --since yesterday')
    print_title('Systemctl status nginx')
    sudo('systemctl status nginx')
    print_title('Systemctl status gunicorn')
    sudo('systemctl status gunicorn')


# ----------------------------------------------------------------------------------------
# Helper functions below
# ----------------------------------------------------------------------------------------

def print_title(title):
    pad = "-" * (80 - len(title) - 4)
    print((colored("-- {} {}".format(title,pad), 'blue', 'on_yellow')))

def print_error(message):
    print((colored(message, 'red')))

def print_success(message):
    print((colored(message, 'green')))

  
# ----------------------------------------------------------------------------------------
# Sub Tasks - OS
# ----------------------------------------------------------------------------------------

def update_server():
    print_title('Updating server')
    sudo('apt-get update -y')
    sudo('apt-get upgrade -y')

def install_os_packages():
    print_title('Installing OS packages')
    sudo('apt-get install -y nginx python3-pip python3-dev python3-psycopg2')

#  Users and Groups
def add_grp_to_user():
    print_title('Adding {} to {}'.format(Settings.DEPLOY_GRP, env.user))
    if not group.exists(Settings.DEPLOY_GRP):
        group.create(Settings.DEPLOY_GRP)
    sudo('adduser {username} {group}'.format(username=env.user, group=Settings.DEPLOY_GRP))
    # user.modify(env.user, group=DEPLOY_GRP)

# # Install Python 3.5
# def install_python_35():
#     print_title('Installing Python 3.5')
#     sudo('apt install -y build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev')
#     with cd('/srv'):
#         pyvstr = ".".join([ str(x) for x in Settings.PYVERSION ])
#         sudo('wget https://www.python.org/ftp/python/{0}/Python-{0}.tgz'.format(pyvstr))
#         sudo('tar -xvf Python-{0}.tgz'.format(pyvstr))
#         with cd('/srv/Python-{0}'.format(pyvstr)):
#             sudo('./configure')
#             sudo('make')
#             sudo('make altinstall')


# ----------------------------------------------------------------------------------------
# Sub Tasks - Project
# ----------------------------------------------------------------------------------------

# Make project folders
def make_dirs():
    print_title('Making folders')
    for d in [Settings.DIR_PROJ, Settings.DIR_CODE, Settings.DIR_LOGS, Settings.DIR_ENVS, Settings.DIR_SOCK]:
        exists = files.exists(d)
        print("File", d, "exists?", exists)
        if not exists:
            sudo('mkdir -p {}'.format(d))
            sudo('chown -R %s %s' % (env.user, d))
            sudo('chgrp -R %s %s' % (Settings.DEPLOY_GRP, d))
    set_permissions()

# Sync project fioles to server
def sync_files():
    print_title('Synchronising project code')
    rsync_project(   
        remote_dir=Settings.DIR_CODE,
        local_dir=Settings.LOCAL_DIR_CODE,
        exclude=("fabfile.py","*.pyc",".git","*.db", "*.log", "*.csv" '__pychache__', '*.md','*.DS_Store'),
        extra_opts="--filter 'protect *.csv' --filter 'protect *.json' --filter 'protect *.db'",
        delete=True
    )

# Set folder permissions
def set_permissions():
    print_title('Setting folder and file permissions')
    sudo('chmod -R %s %s' % ("u=rwx,g=rwx,o=r", Settings.DIR_CODE))
    sudo('chmod -R %s %s' % ("u=rwx,g=rw,o=r", Settings.DIR_LOGS))
    sudo('chmod -R %s %s' % ("u=rwx,g=rwx,o=r", Settings.DIR_ENVS))

# Create a new environments
def create_virtualenv():
    print_title('Creating Python {} virtual environment'.format(Settings.PYVMM))
    sudo('pip3 install virtualenv')
    if files.exists(Settings.DIR_VENV):
        print("Virtual Environment already exists")
        return
    run('virtualenv -p python{0} {1}'.format(Settings.PYVMM, Settings.DIR_VENV))
    sudo('chgrp -R %s %s' % (Settings.DEPLOY_GRP, Settings.DIR_VENV))

# Install Python requirments
def install_venv_requirements():
    print_title('Installing remote virtual env requirements')
    with virtualenv(Settings.DIR_VENV):
        for path in Settings.REQUIRMENTS_FILES:
            if files.exists(path):
                install_requirements(path, use_sudo=False)
                print_success("Installed: {}".format(path))
            else:
                print_error("File missing: {}".format(path))
                return

# ----------------------------------------------------------------------------------------
# Sub Tasks - Web Server
# ----------------------------------------------------------------------------------------

# Seup Nginx web service routing
def setup_nginx():
    print_title('Installing Nginx')
    deb.install('nginx')

    server_hosts = [env.hosts[0], "raspberrypi.local", "{}.local".format(Settings.ROOT_NAME)]
    server_hosts.append( socket.gethostbyname(env.host) )
    server_hosts = set(server_hosts)

    nginx_conf = '''
        # the upstream component nginx needs to connect to
        upstream django {{
            server unix:/tmp/{PROJECT_NAME}.sock;
        }}

        # configuration of the server
        server {{

            # Block all names not in list i.e. prevent HTTP_HOST errors
            if ($host !~* ^({SERVER_NAMES})$) {{
               return 444;
            }}

            listen      80;
            server_name {SERVER_NAMES};
            charset     utf-8;

            # max upload size
            client_max_body_size 75M;   # adjust to taste

            # Static files
            location /static {{
               alias {PROJECT_PATH}static; 
            }}

            location = /favicon.ico {{ access_log off; log_not_found off; }}

            # Finally, send all non-media requests to the server.
            location / {{
                include proxy_params;
                proxy_pass http://unix:{SOCKET_FILES_PATH}{PROJECT_NAME}.sock;
        }}
    }}'''.format(
        SERVER_NAMES="|".join(server_hosts),
        PROJECT_NAME=Settings.ROOT_NAME,
        PROJECT_PATH=Settings.DIR_CODE,
        STATIC_FILES_PATH=Settings.DIR_CODE,
        VIRTUALENV_PATH=Settings.DIR_VENV,
        SOCKET_FILES_PATH=Settings.DIR_SOCK
    )  

    sites_available = "/etc/nginx/sites-available/%s" % Settings.ROOT_NAME
    sites_enabled = "/etc/nginx/sites-enabled/%s" % Settings.ROOT_NAME
    files.append(sites_available, nginx_conf, use_sudo=True)
    # Link to sites enabled
    if not files.exists(sites_enabled):
        sudo('ln -s %s %s' % (sites_available, sites_enabled))
    # This removes the default configuration profile for Nginx
    if files.exists('/etc/nginx/sites-enabled/default'):
        sudo('rm -v /etc/nginx/sites-enabled/default')
    # Firewall settings
    # sudo("ufw allow 'Nginx Full'")


# Setup Gunicorn to serve web application
def setup_gunicorn():
    print_title('Installing Gunicorn')
    with virtualenv(Settings.DIR_VENV):
        install('gunicorn', use_sudo=False)

    gunicorn_conf = '''[Unit]
        Description=gunicorn daemon
        After=network.target

        [Service]
        User={USER}
        Group={GRP}
        WorkingDirectory={PATH}
        Restart=always
        ExecStart={VIRTUALENV_PATH}/bin/gunicorn --workers 3 --bind unix:{SOCKET_FILES_PATH}{PROJECT_NAME}.sock {APP_ENTRY_POINT}

        [Install]
        WantedBy=multi-user.target
        '''.format(
            APP_NAME=Settings.ROOT_NAME,
            PROJECT_NAME=Settings.ROOT_NAME,
            PATH=Settings.DIR_CODE,
            USER=env.user,
            GRP=Settings.DEPLOY_GRP,
            VIRTUALENV_PATH=Settings.DIR_VENV,
            SOCKET_FILES_PATH=Settings.DIR_SOCK,
            APP_ENTRY_POINT=Settings.APP_ENTRY_POINT
        )
    
    gunicorn_service = "/etc/systemd/system/gunicorn.service"
    files.append(gunicorn_service, gunicorn_conf, use_sudo=True)
    sudo('systemctl enable gunicorn')
    sudo('systemctl start gunicorn')
   



# ----------------------------------------------------------------------------------------
# Sub Tasks - Database
# ----------------------------------------------------------------------------------------

@task
def setup_mysql():
    sudo('apt-get install -y mysql-server python-mysqldb')
    with virtualenv(Settings.DIR_VENV):
        run('pip install mysqlclient')
    if not mysql.user_exists(Settings.DB_USER_NAME):
        mysql.create_user(Settings.DB_USER_NAME, password=Settings.DB_PASSWORD)
    if not mysql.database_exists(Settings.DB_NAME):
        mysql.create_database(Settings.DB_NAME, Settings.DB_USER_NAME)
    
@task
def create_superuser():
    with virtualenv(Settings.DIR_VENV):
        run("python {}manage.py createsuperuser".format(Settings.DIR_CODE))

# ----------------------------------------------------------------------------------------
# Sub Tasks - Django Specific
# ----------------------------------------------------------------------------------------

def django_migrate():
    print_title('Collecting Static files')
    with virtualenv(Settings.DIR_VENV):
        run('python {}manage.py migrate'.format(Settings.DIR_CODE))

def django_collect_static():
    print_title('Collecting Static files')
    with virtualenv(Settings.DIR_VENV):
        run('python {}manage.py collectstatic --noinput'.format(Settings.DIR_CODE))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
