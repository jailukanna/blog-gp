---
title: mysql example gulf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gulf'

Functions in program: 
* `def get_db():`
* `def connect_to_database():`

Modules used in program: 
* `import mysql.connector`
* `import logging`
* `import time`
* `import boto3`

## python gulf

Python mysql example: gulf

```python
import boto3
import time
import logging
import mysql.connector
from datetime import datetime, timedelta
from operator import itemgetter

'''

database config

'''
db_config = {'user': 'ece1779',
         'password': 'secret',
         'host': '54.165.124.234',
         'database': 'mydb'}

'''

Functions for Database

'''

def connect_to_database():
    return mysql.connector.connect(user=db_config['user'],
                                   password=db_config['password'],
                                   host=db_config['host'],
                                   database=db_config['database'])

def get_db():
    db = connect_to_database()
    return db

'''

boto3 config

'''
ami_id = 'ami-b368c3a5'
ec2 = boto3.setup_default_session(region_name='us-east-1')
client = boto3.client('cloudwatch')
ec2 = boto3.resource('ec2')
metric_name = 'CPUUtilization'
namespace = 'AWS/EC2'
statistic = 'Average'

'''

Create Load Balancer

'''
elb = boto3.client('elb')

lb = elb.create_load_balancer(LoadBalancerName='myelb',
                               Listeners=[
                                       {
                                           'Protocol':'TCP',
                                           'LoadBalancerPort':5000,
                                           'InstanceProtocol':'TCP',
                                           'InstancePort':5000
                                       },
                                   ],
                               AvailabilityZones=['us-east-1a','us-east-1b','us-east-1c','us-east-1e']
                            )

# add worker security groups to load balancer
elb.apply_security_groups_to_load_balancer(LoadBalancerName='myelb',
                                           SecurityGroups=['sg-95387fe9'])

# configure health check
health_check = elb.configure_health_check(LoadBalancerName='myelb',
                                          HealthCheck={
                                              'Target':'TCP:5000',  #'HTTP:80/index.html
                                              'Timeout':2,
                                              'Interval':5,
                                              'UnhealthyThreshold':10,
                                              'HealthyThreshold':2
                                            }
                                          )

# # create the database
# new_instance=ec2.create_instances(ImageId=database_ami_id_pre_created,
#                                   KeyName='ece1779_project1',
#                                   MinCount=1,
#                                   MaxCount=1,
#                                   InstanceType='t2.small',
#                                   Monitoring={'Enabled': True},
#                                   SecurityGroupIds=[
#                                         'sg-705b940f',
#                                     ]
#                                   )
# while list(ec2.instances.filter(InstanceIds=[new_instance[0].id]))[0].state.get('Name') != 'running':
#     i=1
# sql_host = list(ec2.instances.filter(InstanceIds=[new_instance[0].id]))[0].public_dns_name
# userdata = '''#cloud-config
#     runcmd:
#      - cd /home/ubuntu/Desktop/ece1779/
#      - ./venv/bin/python run.py {}
#     '''.format(sql_host)

# create the first worker
new_instance=ec2.create_instances(ImageId=ami_id,
                                  KeyName='ece1779_a1',
                                  MinCount=1,
                                  MaxCount=1,
                                  InstanceType='t2.small',
                                  Monitoring={'Enabled': True},
                                  # UserData =
                                  SecurityGroupIds=[
                                        'sg-95387fe9',
                                    ]
                                  )

# attach the first worker instance to the lb
attachinst = elb.register_instances_with_load_balancer(LoadBalancerName='myelb',
                                                      Instances=[
                                                          {
                                                              'InstanceId':new_instance[0].id
                                                          }
                                                       ])
#flag for booting up
flag = 1
num_of_instance_last_time = 0

while(1):
    time.sleep(10)
    #wait for instance to boot up
    #check is there any pending worker instances
    #flag = 0
    #instances = ec2.instances.all()
    #for instance in instances:
    #    if(instance.state['Name']=='pending'):
    #        print(('Waiting for new instance to boot up......'))
    #        # update the instance state
    #        flag = 1
    #        time.sleep(10)
    #for status in ec2.meta.client.describe_instance_status()['InstanceStatuses']:
    #    if ((status['InstanceStatus']['Status']) != 'ok' or (status['SystemStatus']['Status']) != 'ok'):
    #        print('Waiting for new instance to register on the load balancer and pass the status check......')
    #        # update the instance state
    #        flag = 1
    #        time.sleep(10)
    #if (flag==1):
    #   continue

    # compute the average cpu utilization now
    instances = ec2.instances.filter(
        Filters=[{'Name': 'instance.group-name', 'Values': ['a1_flask']}])
    sum = 0.0
    num_of_instance = 0
    num_of_new_instance = 0
    num_of_old_instance = 0
    for instance in instances:
        cpu = client.get_metric_statistics(
            Period=1 * 60,
            StartTime=datetime.utcnow() - timedelta(seconds=60 * 60),
            EndTime=datetime.utcnow() - timedelta(seconds=0 * 60),
            MetricName=metric_name,
            Namespace=namespace,  # Unit='Percent',
            Statistics=[statistic],
            Dimensions=[{'Name': 'InstanceId', 'Value': instance.id}]
        )

        datapoints = cpu['Datapoints']
        if(datapoints!=[]):
            last_datapoint = sorted(datapoints, key=itemgetter('Timestamp'))[-1]
            utilization = last_datapoint['Average']
            print(("One Instance's CPU utilization is {}".format(utilization)))
            sum = sum + utilization
            num_of_instance = num_of_instance + 1
        else:
            continue
    if(num_of_instance!=0):
        print(("There are {} Instances are working".format(num_of_instance)))
        utilization = sum / num_of_instance
        print(("All Instances average CPU utilization is {}".format(utilization)))
    else:
        print(('Waiting for instance to boot up......'))
        continue
    if (num_of_instance_last_time == num_of_instance and flag==0):
        print(('Waiting for instance to boot up......'))
        continue
    else:
        flag = 1
        num_of_instance_last_time = num_of_instance

    # fetch the parameters from the database

    cnx = get_db()
    cursor = cnx.cursor()
    query = '''SELECT * FROM config_parameters WHERE id =1
    '''
    cursor.execute(query)
    parameters=cursor.fetchone()

    if (parameters[3] != 1):
        num_of_new_instance = num_of_instance * (parameters[3]-1)
        # set back the database
        query = '''UPDATE config_parameters
                   SET expandratio = 1
                   WHERE id = 1
                '''
        cursor.execute(query)
        cnx.commit()

    if (parameters[4] != 1):
        num_of_old_instance = num_of_instance * (parameters[4]-1)/(parameters[4])
        # set back the database
        query = '''UPDATE config_parameters
                   SET shrinkratio = 1
                   WHERE id = 1
                '''
        cursor.execute(query)
        cnx.commit()

    if(utilization>parameters[1]):
        num_of_new_instance = num_of_new_instance + 1
        flag = 0

    if(utilization<parameters[2]):
        num_of_old_instance = num_of_old_instance + 1

    if (num_of_new_instance!=0):
        for i in range(num_of_new_instance):
            new_instance = ec2.create_instances(ImageId=ami_id,
                                                KeyName='ece1779_a1',
                                                MinCount=1,
                                                MaxCount=1,
                                                InstanceType='t2.small',
                                                Monitoring={'Enabled': True},
                                                SecurityGroupIds=[
                                                    'sg-95387fe9',
                                                ]
                                                )
            attachinst = elb.register_instances_with_load_balancer(LoadBalancerName='myelb',
                                                                   Instances=[
                                                                       {
                                                                           'InstanceId': new_instance[0].id
                                                                       }
                                                                   ])

    if (num_of_old_instance!=0):
        instances = ec2.instances.filter(
            Filters=[{'Name': 'instance-state-name', 'Values': ['running']}])

        instances = ec2.instances.filter(
            Filters=[{'Name':'instance.group-name', 'Values':['a1_flask']}]
        )
        i=0
        for instance in instances:
            elb.deregister_instances_from_load_balancer(LoadBalancerName='myelb',
                                                        Instances=[
                                                            {
                                                                'InstanceId': instance.id
                                                            }
                                                        ])
            instance.terminate()
            i=i+1
            if (i >= num_of_old_instance):
                break

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
