---
title: mysql example stripe reconciliation (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'stripe reconciliation'

Functions in program: 
* `def unix_to_readable(ts):`
* `def cents_to_dollars(cents):`

Modules used in program: 
* `import mysql.connector`
* `import json`
* `import stripe`

## python stripe reconciliation

Python mysql example: stripe reconciliation

```python
import stripe
import json
import mysql.connector
from datetime import datetime

stripe.api_key = ""


def cents_to_dollars(cents):
    if cents is not None:
        return cents / 100


def unix_to_readable(ts):
    if ts is not None:
        return datetime.utcfromtimestamp(ts).strftime('%m/%d/%Y')


# Get a list of successful payouts from Stripe
payouts = stripe.Payout.list(limit=20)
payout_count = 0
for p in payouts:
    payout_count += 1
    charge_count = 0
    payout_id = p['id']
    payout_amount = cents_to_dollars(p['amount'])
    expected_by = unix_to_readable(p['arrival_date'])
    # print("({})".format(payout_count))
    print('Date: {}\nPayout amount: {:.2f}'.format(
        expected_by, payout_amount))
    print('Transactions in payout:')
    charges = stripe.BalanceTransaction.list(
        limit=30, payout=payout_id)
    for charge in charges:
        if charge['amount'] > 0:
            charge_id = charge['source']
            charge_count += 1
            amount_billed = cents_to_dollars(charge['amount'])
            fee_meta = charge['fee_details']
            for fm in fee_meta:
                fee = cents_to_dollars(fm['amount'])
            # Reconstruct client data based on charge id in database
            # Prabaly should use JOINs
            cnx = mysql.connector.connect(
                user='',
                password='',
                host='localhost',
                port=3306,
                database='')
            cursor = cnx.cursor()
            client_id_query = (
                "SELECT client_id FROM invoices WHERE charge_id = %s")
            cursor.execute(client_id_query, (charge_id,))
            invoice_data = cursor.fetchone()
            client_id = invoice_data[0]
            # note = invoice_data[1]
            client_address_query = (
                "SELECT country_code, administrative_area, "
                "locality, postal_code, thoroughfare, premise "
                "FROM client_addresses WHERE client_id = %s")
            client_query = (
                "SELECT first_name, last_name, email "
                "FROM clients WHERE id = %s")

            # Get client name, email
            cursor.execute(client_query, (client_id))
            print(client_id)
            client = cursor.fetchone()
            first_name = client[0]
            last_name = client[1]
            email = client[2]
            if not email:
                email = ''
            if not first_name:
                first_name = ''
            if not last_name:
                last_name = ''
            # Get client address
            cursor.execute(client_address_query, (client_id))
            client_address = cursor.fetchone()
            country_code = client_address[0]
            administrative_area = client_address[1]
            locality = client_address[2]
            postal_code = client_address[3]
            premise = client_address[5]
            if not premise:
                premise = ''
            thoroughfare = client_address[4] + premise

            # print(" "*4 + "({})".format(charge_count))
            print(" "*4 + "Amount billed: {:.2f}".format(amount_billed))
            print(" "*4 + "Fee: {:.2f}".format(fee))
            print(" "*4 + "Type of Service: ")
            print(" "*4 + "Name: {} {}".format(first_name, last_name))
            print(" "*4 + "Address:")
            print(" "*4 + "{} {}".format(thoroughfare, premise))
            print(" "*4 + "{} {}".format(locality, administrative_area))
            print(" "*4 + "{}".format(postal_code))
            print("\n")
    # Clear database connection
    close_cursor = cursor.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
