---
title: socket example assignment packet (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'assignment packet'


Modules used in program: 
* `import struct`

## python assignment packet

Python socket example: assignment packet

```python
"""Assignment 1: Packet classes
    Written by: Ryan McLaughlin
    Date: 21/8/2018
    Description: This library contains classes used in assingment_server and
    assingment_client, the classes of interst are DT_Request and DT_Response
    which are used for packing and unpacking their respective packets while
    maintaining consistency between clien and server
"""

import struct
from enum import Enum
from datetime import datetime

class RequestType(Enum):
    """Enum for request types"""
    DATE = 1
    TIME = 2

class Language(Enum):
    """Enum for languages"""
    ENGLISH = 1
    MAORI = 2
    GERMAN = 3

class PacketsError(Exception):
    def __init__(self, msg):
        self.msg = msg

class DT_Request():
    """DT_Request packet, each instance will contain all required info and all
    necessary functions to be performed upon itself. Magic number, format string,
    and packet code are made to be static class elements for consistency between
    systems and ease of modification"""
    MAGICNO = 0x497E
    FORMAT = "!HHH"
    PACKET_CODE = 0x0001

    def __init__(self, request_type = RequestType.DATE, language = Language.ENGLISH):
        """Initialise class instance with relevant properties"""
        self.request_type = request_type
        self.language = language

    def pack(self):
        """Returns a packed bytearray to be sent"""
        r_type = RequestType(self.request_type)
        return struct.pack(self.FORMAT, self.MAGICNO, self.PACKET_CODE, r_type.value)

    def unpack(message, lang):
        """Unpackes a bytearray into the properties of the packet class"""
        self = DT_Request()
        magic_no, packet_code, dt_type = struct.unpack('!HHH', message)
        #Checking packet for errors:
        if len(bytearray(message)) != 6:
            print("Oversize packet")
        elif magic_no != DT_Response.MAGICNO:
            print("Magic number mismatch")
        elif packet_code != self.PACKET_CODE:
            print("Not a DT-Request packet")
        elif dt_type not in [0x0001, 0x0002]:
            print("Request type value not recognised")
        #set properties of packet:
        self.request_type = RequestType(dt_type)
        self.language = Language(lang)
        return self

class DT_Response():
    """DT_Response packet, each instance will contain all required info and all
    necessary functions to be performed upon itself. Magic number, format string,
    and packet code are made to be static class elements for consistency between
    systems and ease of modification"""

    MAGICNO = 0x497E
    FORMAT = "!HHHHBBBBB"
    PACKET_CODE = 0x0002
    MONTHS = {
        Language.ENGLISH: ['January', 'Febuarry', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        Language.MAORI: ['Kohitatea','Hui-tanguru','Poutu-te-rangi','Paenga-whawha','Haratua','Pipiri','Hongongoi','Here-turi-koka','Mahuru','Whiringa-a-nuku','Whiringa-a-rangi','Hakihea'],
        Language.GERMAN: ['Januar', 'Februar', 'Marz', 'April', 'Mai', 'Juni', 'Juli', 'August', 'September', 'Oktober', 'November', 'Dezember']
    }
    FORMAT_STRINGS = {
        RequestType.DATE: {
            Language.ENGLISH: "Today's date is {month} {day}, {year}",
            Language.MAORI: "Ko te ra o tenei ra ko {month} {day}, {year}",
            Language.GERMAN: "Heute ist der {day}. {month} {year}",
        },
        RequestType.TIME: {
            Language.ENGLISH: "The current time is {hour}:{minute}",
            Language.MAORI: "Ko te wa o tenei wa {hour}:{minute}",
            Language.GERMAN: "Die Uhrzeit ist {hour}:{minute}",
        }
    }

    def __init__(self, request_type = RequestType.DATE, language = Language.ENGLISH, dt = datetime.now()):
        """Initialise class instance with relevant properties"""
        self.request_type = request_type
        self.language = language
        self.dt = dt
        self.str = None

    def __str__(self):
        """Return the string, formatted as required by packet type and language"""
        dt = self.dt
        #if the packet already has a string:
        if self.str:
            return self.str
        #if not make and return string:
        return DT_Response.FORMAT_STRINGS[self.request_type][self.language].format(
            year = dt.year,
            month = DT_Response.MONTHS[self.language][dt.month],
            day = dt.day,
            hour = dt.hour,
            minute = dt.minute
        )

    def pack(self):
        """Returns a packed bytearray to be sent"""
        dt = self.dt
        #make header:
        header = struct.pack(DT_Response.FORMAT, DT_Response.MAGICNO,
            DT_Response.PACKET_CODE, self.language.value, dt.year, dt.month,
            dt.day, dt.hour, dt.minute, len(str(self)))
        #combine header and encoded string:
        return header + str(self).encode("UTF-8")

    def unpack(bs):
        """Unpackes a bytearray into the properties of the packet class"""

        self = DT_Response()
        #unpack bytearray:
        mg, pt, ln, yr, mn, day, hr, min, sl = struct.unpack(DT_Response.FORMAT, bs[:13])
        #error checking, terminate packet handling with mesage on error:
        if mg !=  DT_Response.MAGICNO:
            raise PacketsError("Invalid packet: Unknown magic number")
        if pt != DT_Response.PACKET_CODE:
            raise PacketsError("Invalid packet: Packet not a DT-Response packet")
        elif ln < 1 or ln >3:
            raise PacketsError("Invalid packet: Unknown language code")
        elif yr >= 2100:
            raise PacketsError("Invalid packet: Year too high")
        elif mn < 1 or mn > 12:
            raise PacketsError("Invalid packet: Incorrect month (1-12)")
        elif day < 1 or day > 31:
            raise PacketsError("Invalid packet: Incorrect day (1-31)")
        elif hr < 0 or hr > 23:
            raise PacketsError("Invalid packet:  Incorrect hour (0-23)")
        elif min < 0 or min > 59:
            raise PacketsError("Invalid packet: Incorrect minute (0-59)")
        elif sl != len(bs[13:]):
            raise PacketsError("Invalid packet: Length incorrect")
        else:
            #set properties of packet:
            self.language = Language(ln)
            self.dt = datetime(yr, mn, day, hr, min)
            self.str = bs[13:].decode("UTF-8")
            return self


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
