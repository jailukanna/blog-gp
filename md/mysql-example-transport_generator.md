---
title: mysql example transport generator (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'transport generator'


Modules used in program: 
* `import mysql.connector`

## python transport generator

Python mysql example: transport generator

```python
#!/usr/bin/env python


import mysql.connector

cnx = mysql.connector.connect(user='psa_readonly', password='securepassword', host='127.0.0.1', database='psa')

target = "smtp:[plesk_beckend_server.xxxx.xxx]:25"

buffer = ""

cursor = cnx.cursor()

# Get all mailboxes
mb_query = "select mail_name, name from mail left join domains on mail.dom_id=domains.id;"
cursor.execute(mb_query)
for (mail_name, name) in cursor.fetchall():
	buffer += mail_name + "@" + name + " " + target + "\n"

# Get all aliases
alias_query = "select alias, name from mail_aliases left join mail on mail_aliases.mn_id = mail.id left join domains on mail.dom_id=domains.id;"
cursor.execute(alias_query)
for (alias, name) in cursor.fetchall():
	buffer += alias + "@" + name + " " + target + "\n"

# Get all mailboxes alias domains
mb_ad_query = "select mail_name, domain_aliases.name from mail left join domains on mail.dom_id=domains.id LEFT JOIN domain_aliases on domain_aliases.dom_id=domains.id WHERE domain_aliases.mail = 'true';"
cursor.execute(mb_ad_query)
for (mail_name, name) in cursor.fetchall():
	buffer += mail_name + "@" + name + " " + target + "\n"



# Get all aliases for alias domains
alias_ad_query = "select alias, domain_aliases.name from mail_aliases left join mail on mail_aliases.mn_id = mail.id left join domains on mail.dom_id=domains.id LEFT JOIN domain_aliases on domain_aliases.dom_id=domains.id WHERE domain_aliases.mail = 'true';"
cursor.execute(alias_ad_query)
for (alias, name) in cursor.fetchall():
	buffer += alias + "@" + name + " " + target + "\n"

### NEU: Mailman
mailman_query ="select MailLists.name as alias, domains.name as domain from MailLists left join domains on MailLists.dom_id = domains.id;"
cursor.execute(mailman_query)
for (alias, domain) in cursor.fetchall():
	buffer += alias + "@" + domain + " " + target + "\n"

# Check for catchall
catch_query = "select id, value from Parameters where parameter='nonexist_mail';"
cursor.execute(catch_query)
for (dom_id, value) in cursor.fetchall():
	if value == "catch":
		# Domains
		get_catch_query = "select name, value from Parameters left join domains on domains.id=Parameters.id where parameter='catch_addr' and Parameters.id=%s;" % int(dom_id)
		cursor.execute(get_catch_query)
		for (name, value) in cursor.fetchall():
			buffer += name + " " + target + "\n"
		# Alias Domains
		get_alias_catch_query = "select domain_aliases.name, value from Parameters left join domain_aliases on domain_aliases.dom_id=Parameters.id where parameter='catch_addr' and Parameters.id=%s;" % int(dom_id)
		cursor.execute(get_alias_catch_query)
		for (name, value) in cursor.fetchall():
			buffer += name + " " + target + "\n"

print(buffer)

with open("/root/mx-in/transport","w") as f:
	f.writelines(buffer)
	f.close()

cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
