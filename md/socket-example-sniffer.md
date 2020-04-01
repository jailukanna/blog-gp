---
title: socket example sniffer (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sniffer'


Modules used in program: 
* `import socket `

## python sniffer

Python socket example: sniffer

```python
import socket 
from struct import *


s = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_TCP)

while True:
	pack = s.recvfrom(20000)
	
	#Get the single element from the tuple
	packet = pack[0]

	#Extract the first 20 bytes 

	data = packet[0:20]

	# Now we have to unpack each element from this raw data 

	ip_header_data = unpack('!BBHHHBBH4s4s', data) 

	#To the the ip version we have to shift 
	#the first element 4 bits right. Because in the first element
	#is stored the ip version and the header lenght in this way
	#first four bits are ip version and the last 4 bites are
	#the header lenght  
	ip_version = ip_header_data[0] >> 4

	#Now to get the header lenght we use "and" operation to make the
	#Ip versional bits equal to zero, in order to the the desired data
	IHL = ip_header_data[0] & 0x0F

	#Diferentiated services doesn't need any magic opperations,
	#so we jus grab it from the tuple
	diff_services = ip_header_data[1]

	#Total lenght is also easy to extract
	total_length = ip_header_data[2]

	#The same goes for identification 
	id_ = ip_header_data[3]

	#The "Flags" and Fragment Offset are situated in a sinle
	#element from the forth element of the tuple.
	#Flag is 3 bits (Most significant), so we make "and" with 1110 0000 0000 0000(=0xE000)
	#to leave 3 most significant bits and then shift them right 13 positions
	flags = ip_header_data[4] & 0xE000 >> 13

	#The next elements are easy to get
	TTL 	 = ip_header_data[5]
	protocol = ip_header_data[6]
	checksum = ip_header_data[7]
	source   = ip_header_data[8]
	destinat = ip_header_data[9]

	#and the rest data from the "packet" variable is the payload so we
	#extract it also
	payload = packet[20:]


	print("___________NEW_PACKET__________________________")
	print("Version: %s  \n\rHeader lenght: %s"  %(ip_version,IHL))
	print("Diferentiated services: %s \n\rID: %s" %(diff_services, id_))
	print("Flags: %s \n\rTTL: %s \n\rProtocol: %s" %(flags,TTL,protocol))
	print("Checksum: %s \n\rSource: %s \n\rDestination: %s" %(checksum, socket.inet_ntoa(source),socket.inet_ntoa(destinat)))
	print("Payload: %s" %(payload))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
