---
title: mysql example from remote to local (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'from remote to local'

Functions in program: 
* `def main(config,database):`
* `def delete_old_backups(backup_dir_daily,backup_dir_weekly,backup_dir_monthly,days_expire_daily,days_expire_weekly,days_expire_monthly,remote_ip,remote_port,remote_user):`
* `def touch_dirs(remote_ip,remote_port,remote_user,backup_dir_daily,backup_dir_weekly,backup_dir_monthly):`
* `def monthly_backup_procedure(backup_dir_monthly,days_expire_monthly,db_name,backup_name,db_user,pg_dump_cmd):`
* `def weekly_backup_procedure(backup_dir_weekly,days_expire_weekly,db_name,backup_name,db_user,pg_dump_cmd):`
* `def daily_backup_procedure(backup_dir_daily,backup_name,pg_dump_cmd):`
* `def type_of_backup(weekly_backup_day,monthly_backup_date):`
* `def check_dependencies():`
* `def check_connection(remote_ip,remote_port,remote_user):`

Modules used in program: 
* `import boyans_ssh`
* `import datetime`
* `import psutil`
* `import sys`
* `import os`
* `import subprocess`

## python from remote to local

Python mysql example: from remote to local

```python
import subprocess
import os
import sys
import psutil
import datetime
import boyans_ssh


def check_connection(remote_ip,remote_port,remote_user):
    """Checks if ssh exists on local machine, if an ssh server exists on the
       remote machine and if a connection could be made"""    
    if(boyans_ssh.check_conn_using_key(remote_ip,remote_port,remote_user,5))!=0:
        sys.exit(1)


        
def check_dependencies():
    """Checks for dependencies
       Local dependencies are - pg_dump,psql
       Remote dependencies are - find,mkdir,ssh(server)
       Non-mandatory - gzip. Remote dependencies are not checked."""
    try:
        if subprocess.call('/usr/bin/pg_dump > /dev/null 2>&1', shell=True) == 127:
            sys.stderr.write("Failed: ERROR:Missing dependancy: pg_dump")
            sys.exit(1)
            
        if subprocess.call('/usr/bin/psql > /dev/null 2>&1', shell=True) == 127:
            sys.stderr.write("Failed: ERROR:Missing dependancy: psql")
            sys.exit(1)         
    except subprocess.CalledProcessError:
        sys.stderr.write("ERROR:Unable to check for dependencies")
        sys.exit(1)        


            
def type_of_backup(weekly_backup_day,monthly_backup_date):
    """returns if backup should be daily/weekly/monthly by checking the config dates and the real date"""
    time_now = datetime.datetime.now()
    current_day = time_now.strftime("%A")
    if current_day == weekly_backup_day:
        return "weekly"
    current_date = time_now.day    
    if monthly_backup_date == str(current_date):
        return "monthly"   
    return "daily"



def daily_backup_procedure(backup_dir_daily,backup_name,pg_dump_cmd):
    try:
       if not gzip_enabled!="yes":    
            pg_dump_cmd+="| /bin/gzip > "+backup_dir_daily+"/"+backup_name+"\""
       else:
            pg_dump_cmd+=" > "+backup_dir_daily+""+backup_name+"\""
                           
       output = subprocess.check_output([pg_dump_cmd],shell=True).decode('utf-8')
    except subprocess.CalledProcessError:
        sys.stderr.write("ERROR:Failed to do daily backup \n")
        sys.exit(1)              
    print("Successfull daily backup \n")
    return 0



def weekly_backup_procedure(backup_dir_weekly,days_expire_weekly,db_name,backup_name,db_user,pg_dump_cmd):

    try:         
       if not gzip_enabled!="yes":    
            pg_dump_cmd+="| /bin/gzip > "+backup_dir_weekly+"/"+backup_name+"\""
       else:
            pg_dump_cmd+=" > "+backup_dir_weekly+""+backup_name+"\""
                           
       output = subprocess.check_output([pg_dump_cmd],shell=True).decode('utf-8')         
    except subprocess.CalledProcessError:
        sys.stderr.write("ERROR: Failed to do weekly backup\n")
        sys.exit(1) 
    return 0



def monthly_backup_procedure(backup_dir_monthly,days_expire_monthly,db_name,backup_name,db_user,pg_dump_cmd):

    try:         
       if not gzip_enabled!="yes":    
            pg_dump_cmd+="| /bin/gzip > "+backup_dir_monthly+"/"+backup_name+"\""
       else:
            pg_dump_cmd+=" > "+backup_dir_monthly+""+backup_name+"\""
                           
       output = subprocess.check_output([pg_dump_cmd],shell=True).decode('utf-8')         
    except subprocess.CalledProcessError:
        sys.stderr.write("ERROR: Failed to do monthly backup\n")
        sys.exit(1) 
    return 0




def touch_dirs(remote_ip,remote_port,remote_user,backup_dir_daily,backup_dir_weekly,backup_dir_monthly):
    """Executs mkdir -p for daily/weekly/monthly directories"""
    cmd="\"/bin/mkdir -p "+backup_dir_daily+"| /bin/mkdir -p "+backup_dir_weekly+"/bin/mkdir -p "+backup_dir_monthly+"\""
    if(boyans_ssh.exec_single_command(remote_ip,remote_port,remote_user,cmd,1))!=0:
        sys.exit(1)




def delete_old_backups(backup_dir_daily,backup_dir_weekly,backup_dir_monthly,days_expire_daily,days_expire_weekly,days_expire_monthly,remote_ip,remote_port,remote_user):
    """uses rmdir with maxdepth of 2, that specifically searches for files named *.sql* with -f option"""
    ########################################################################################################################
    cmd="\"/usr/bin/find "+backup_dir_daily+" -maxdepth 1 -type f -name \'*.sql*\' -mtime +"+days_expire_daily+" -delete ; /usr/bin/find "+backup_dir_weekly+" -maxdepth 1 -type f -name \'*.sql*\' -mtime +"+days_expire_weekly+" -delete ; /usr/bin/find "+backup_dir_monthly+" -maxdepth 1 -type f -name \'*.sql*\' -mtime +"+days_expire_monthly+" -delete \""
    boyans_ssh.exec_single_command(remote_ip,remote_port,remote_user,cmd,1)


def main(config,database):
    #try:       
        local_hostname  = os.uname()[1]
        
        check_connection(config["DEFAULT"]["remote_ip"],config["DEFAULT"]["remote_port"],config["DEFAULT"]["remote_user"])        

        backup_dir_daily = config["DEFAULT"]["backup_dir"]+local_hostname+"/"+database+"/daily/"
        backup_dir_weekly = config["DEFAULT"]["backup_dir"]+local_hostname+"/"+database+"/weekly/"
        backup_dir_monthly = config["DEFAULT"]["backup_dir"]+local_hostname+"/"+database+"/monthly/"
                    
        global gzip_enabled
        gzip_enabled = config["DEFAULT"]["gzip_enabled"]

        check_dependencies()        
        
        if(config["DEFAULT"]["backup_name"]=="~date~.sql"):
            backup_name = datetime.datetime.now().strftime("%d")+"-"+datetime.datetime.now().strftime("%m")+".sql"

        elif(config["DEFAULT"]["backup_name"]=="~dbname-date~.sql"):
            backup_name = database+"-"
            backup_name += datetime.datetime.now().strftime("%d")+"-"+datetime.datetime.now().strftime("%m")+".sql"


        else:
            backup_name = config["DEFAULT"]["backup_name"]
            if not (backup_name.endswith(".sql")):
                    backup_name += ".sql"

        if not gzip_enabled != "yes":
          if not (backup_name.endswith(".gz")):
            backup_name += ".gz"
                                   
        backup_type = type_of_backup(config["DEFAULT"]["weekly_backup_day"],config["DEFAULT"]["monthly_backup_date"])

        if(config["DEFAULT"]["interactive"]=="yes"):
            if not gzip_enabled != "yes":
                pg_dump_cmd = "/usr/bin/pg_dump -Fp -W -U "+config["DEFAULT"]["db_user"]+" "+database +" | /usr/bin/ssh "+config["DEFAULT"]["remote_ip"]+" -p"+config["DEFAULT"]["remote_port"]+" -o NumberOfPasswordPrompts=0 \"/bin/cat - "

            else:
                pg_dump_cmd = "/usr/bin/pg_dump -Fp -W -U "+config["DEFAULT"]["db_user"]+" "+database +" | /usr/bin/ssh "+config["DEFAULT"]["remote_ip"]+" -p"+config["DEFAULT"]["remote_port"]+" -o NumberOfPasswordPrompts=0 \"/bin/cat -  "
        else:
            if not gzip_enabled != "yes":
                pg_dump_cmd = "/usr/bin/pg_dump -Fp -w -U "+config["DEFAULT"]["db_user"]+" "+database +" | /usr/bin/ssh "+config["DEFAULT"]["remote_user"]+"@"+config["DEFAULT"]["remote_ip"]+" -p"+config["DEFAULT"]["remote_port"]+" -o NumberOfPasswordPrompts=0 \"/bin/cat - "
               
            else:
                pg_dump_cmd = "/usr/bin/pg_dump -Fp -w -U "+config["DEFAULT"]["db_user"]+" "+database +" | /usr/bin/ssh "+config["DEFAULT"]["remote_user"]+"@"+config["DEFAULT"]["remote_ip"]+" -p"+config["DEFAULT"]["remote_port"]+" -o NumberOfPasswordPrompts=0 \"/bin/cat - "

        daily_pg_dump_cmd=pg_dump_cmd+backup_dir_daily +"/"+backup_name+"\""
        weekly_pg_dump_cmd=pg_dump_cmd+backup_dir_weekly+"/"+backup_name+"\""
        monthly_pg_dump_cmd=pg_dump_cmd+backup_dir_weekly+"/"+backup_name+"\""

        touch_dirs(config["DEFAULT"]["remote_ip"],config["DEFAULT"]["remote_port"],config["DEFAULT"]["remote_user"],backup_dir_daily,backup_dir_weekly,backup_dir_monthly)

        if backup_type == "daily":
            daily_backup_procedure(backup_dir_daily,backup_name,pg_dump_cmd)

        if backup_type == "weekly":
            weekly_backup_procedure(backup_dir_weekly,config["DEFAULT"]["days_expire_weekly"],database,backup_name,config["DEFAULT"]["db_user"],pg_dump_cmd)
            daily_backup_procedure(backup_dir_daily,backup_name,pg_dump_cmd)
            
        else:
            monthly_backup_procedure(backup_dir_monthly,config["DEFAULT"]["days_expire_monthly"],database,backup_name,config["DEFAULT"]["db_user"],pg_dump_cmd)
            daily_backup_procedure(backup_dir_daily,backup_name,pg_dump_cmd)

        delete_old_backups(backup_dir_daily,backup_dir_weekly,backup_dir_monthly,config["DEFAULT"]["days_expire_daily"],config["DEFAULT"]["days_expire_weekly"],config["DEFAULT"]["days_expire_monthly"],config["DEFAULT"]["remote_ip"],config["DEFAULT"]["remote_port"],config["DEFAULT"]["remote_user"])
        return 0
    #except:
     #   sys.stderr.write("CRITICAL:Main exception")
        
            
#try:
if __name__ == '__main__':
    main()
#except:
#   sys.stderr.write(" Main exception")
#   sys.exit(-1)
                       
   


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
