---
title: mysql example seed jobs (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'seed jobs'


Modules used in program: 
* `import datetime`
* `import uuid`
* `import mysql.connector`

## python seed jobs

Python mysql example: seed jobs

```python
import mysql.connector
import uuid
import datetime

BATCH_SIZE      = 2000
JOBS_NUMBER     = 10000

mydb = mysql.connector.connect(
    host="localhost",               
    port=3306,
    user="commander",
    passwd="commander",
    db="commander"
)
mycursor = mydb.cursor()

# Get Default project id
mycursor.execute("SELECT hex(id) FROM ec_project WHERE name='Default'")
PROJECT_ID = uuid.UUID(mycursor.fetchone()[0])

mycursor.execute("SELECT hex(proc.id) FROM  ec_procedure proc JOIN ec_project proj WHERE proj.name = 'Default' and proc.name = 'echo'")
PROCEDURE_ID = uuid.UUID(mycursor.fetchone()[0])

mycursor.execute("SELECT hex(ps.id) from ec_procedure_step ps JOIN ec_procedure p where p.name = 'echo' and ps.procedure_id = p.id")
PROCEDURE_STEP_ID = uuid.UUID(mycursor.fetchone()[0])

acl_sql             = "INSERT INTO ec_acl (id,version,inheriting,nonce,owner_type,tracked) VALUES (%s,%s,%s,%s,%s,%s);"
job_sql             = "INSERT INTO ec_job (id,version,name,created,created_millis,last_modified_by,modified,modified_millis,owner,directory_name,launched_by_user,priority,procedure_name,schedule_fired,archived,acl_id,procedure_id,project_id) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s);"
job_step_sql        = "INSERT INTO ec_job_step (id,version,name,created,created_millis,last_modified_by,modified,modified_millis,owner,exclusive_mode,release_mode,always_run,broadcast,condition_expanded,error_handling,exit_code,is_external,finish_count,finish_time,finish_millis,finish_max,license_wait_time,outcome,parallel,post_exit_code,procedure_name,resource_wait_time,retries,run_time,start_count,start_time,start_millis,start_max,status,subprocedure,subproject,time_limit,user_abort,workspace_wait_time,assigned_resource_name,command,step_condition,host_name,runnable_time,runnable_millis,acl_id,job_id,parent_id,procedure_id,procedure_step_id,step_index) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s);"
update_job_sql      = "UPDATE ec_job SET root_step_id = %s WHERE id = %s;"    

totas_sql = acl_sql + job_sql + job_step_sql + job_step_sql + update_job_sql

now = datetime.datetime.now()

for j in xrange(0,JOBS_NUMBER / BATCH_SIZE):
    acl_vals             = []
    job_vals             = []
    root_job_step_vals   = []
    job_step_vals        = []
    update_job_vals      = []

    for i in xrange(0,BATCH_SIZE):
        job_id                  = uuid.uuid4().bytes
        job_step_id             = uuid.uuid4().bytes
        root_job_step_id        = uuid.uuid4().bytes
        job_created_at          = now - datetime.timedelta(seconds = (j * BATCH_SIZE +i))
        job_created_at_str      = job_created_at.strftime("%Y-%m-%d %H:%M:%S")
        job_created_at_millis   = job_created_at.strftime('%s') + "000"
        job_finish_time         = job_created_at + datetime.timedelta(seconds=1)
        job_finish_time_str     = job_finish_time.strftime("%Y-%m-%d %H:%M:%S")
        job_finish_time_millis  = job_finish_time.strftime('%s') + "000"
        job_name                = "seeded_job_%d_%s" % (j * BATCH_SIZE + i, job_created_at.strftime("%Y%m%d%H%M%S"))

        acl_vals.append(
            (job_id, 0, 1, 2, 'job', 0)
            )
        job_vals.append(
            (job_id, 5, job_name, job_created_at_str, job_created_at_millis, 'admin', job_created_at_str, job_created_at_millis, 'admin', job_name, 'admin', 500, 'echo', '\0', 0, job_id, PROCEDURE_ID.bytes, PROJECT_ID.bytes)
            )
        root_job_step_vals.append(
            (root_job_step_id, 4, job_name + '-root', job_created_at_str, job_created_at_millis, 'project: Default', job_created_at_str, job_created_at_millis, 'admin','none','none', 0, 0, 0, 'failProcedure', 0, 0, 1, job_finish_time_str, job_finish_time_millis, 1, 0, 'success', 0, 0, 'echo', 0, 0, 0, 0, job_created_at, job_created_at_millis, 0, 'completed','echo', 'Default', 0, 0, 0, None, None, None, None, None, None, job_id, job_id, None, PROCEDURE_ID.bytes, None, 0)
            )
        job_step_vals.append(
            (job_step_id, 8, 'echo', job_created_at_str, job_created_at_millis, 'project: Default', job_created_at_str, job_created_at_millis, 'project: Default','none','none', 0, 0, 1, 'failProcedure', 0, 0, 0, job_finish_time_str, job_finish_time_millis, 0, 0, 'success', 0, 0, 'echo', 0, 0, 10, 0, job_created_at, job_created_at_millis, 0, 'completed', None, None, 0, 0, 0, 'local', 'echo hello', 1, 'localhost', job_finish_time_str, job_finish_time_millis, job_id, job_id, root_job_step_id, PROCEDURE_ID.bytes, PROCEDURE_STEP_ID.bytes, 0)
            )
        update_job_vals.append(
            (root_job_step_id, job_id)
        )

    vals = acl_vals + job_vals + root_job_step_vals + job_step_vals + update_job_vals

    mycursor.executemany(acl_sql, acl_vals)
    mydb.commit()
    print("ACL records inserted: %d" % mycursor.rowcount)

    mycursor.executemany(job_sql, job_vals)
    mydb.commit()
    print("Jobs records inserted: %d" % mycursor.rowcount)

    mycursor.executemany(job_step_sql, root_job_step_vals)
    mydb.commit()
    print("Root Job steps records inserted: %d" % mycursor.rowcount)

    mycursor.executemany(job_step_sql, job_step_vals)
    mydb.commit()
    print("Job steps records inserted: %d" % mycursor.rowcount)

    mycursor.executemany(update_job_sql, update_job_vals)
    mydb.commit()
    print("jobs Updated: %d" % mycursor.rowcount)

    # mycursor.executemany(totas_sql, vals)
    # mydb.commit()
    # print("performed: %d" % mycursor.rowcount)

mycursor.close()
mydb.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
