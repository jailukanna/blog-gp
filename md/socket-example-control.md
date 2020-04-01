---
title: socket example control (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'control'

Functions in program: 
* `def socketErr():`
* `def stateErr():`
* `def socket2(state):`
* `def socket1(state):`

Modules used in program: 
* `import time`
* `import RPi.GPIO as GPIO`
* `import sys`

## python control

Python socket example: control

```python
#!/usr/bin/python

#import the required modules
import sys
import RPi.GPIO as GPIO
import time

# Check arguments
# (note 3 includes arg 0 which is this script!)
if len(sys.argv) != 3:
    print("\n***",sys.argv[0], "***\n")
    print('Incorrect number of arguments, please run script as follows:')
    print('\n'+str(sys.argv[0])+' <socket number> <on or off>')
    sys.exit(0)

# set the pins numbering mode
GPIO.setmode(GPIO.BOARD)

# Select the GPIO pins used for the encoder K0-K3 data inputs
GPIO.setup(11, GPIO.OUT)
GPIO.setup(15, GPIO.OUT)
GPIO.setup(16, GPIO.OUT)
GPIO.setup(13, GPIO.OUT)

# Select the signal used to select ASK/FSK
GPIO.setup(18, GPIO.OUT)

# Select the signal used to enable/disable the modulator
GPIO.setup(22, GPIO.OUT)

# Disable the modulator by setting CE pin lo
GPIO.output (22, False)

# Set the modulator to ASK for On Off Keying
# by setting MODSEL pin lo
GPIO.output (18, False)

# Initialise K0-K3 inputs of the encoder to 0000
GPIO.output (11, False)
GPIO.output (15, False)
GPIO.output (16, False)
GPIO.output (13, False)

# The On/Off code pairs correspond to the hand controller codes.
# True = '1', False ='0'
# To clear the socket programming, press the green button"
# for 5 seconds or more until the red light flashes slowly"
# The socket is now in its learning mode and listening for"
# a control code to be sent. It will accept the following"
# code pairs"
# 0011 and 1011 all ON and OFF"
# 1111 and 0111 socket 1"
# 1110 and 0110 socket 2"
# 1101 and 0101 socket 3"
# 1100 and 0100 socket 4"

# Socket 1 control
def socket1(state):
    if state == 'on':
        GPIO.output (11, True)
        GPIO.output (15, True)
        GPIO.output (16, True)
        GPIO.output (13, True)
        # let it settle, encoder requires this
        time.sleep(0.1)
        # Enable the modulator
        GPIO.output (22, True)
        # keep enabled for a period
        time.sleep(0.25)
        # Disable the modulator
        GPIO.output (22, False)
    else:
        GPIO.output (11, True)
        GPIO.output (15, True)
        GPIO.output (16, True)
        GPIO.output (13, False)
        # let it settle, encoder requires this
        time.sleep(0.1)
        # Enable the modulator
        GPIO.output (22, True)
        # keep enabled for a period
        time.sleep(0.25)
        # Disable the modulator
        GPIO.output (22, False)


# Socket 2 control
def socket2(state):
    if state == 'on':
        GPIO.output (11, False)
        GPIO.output (15, True)
        GPIO.output (16, True)
        GPIO.output (13, True)
        # let it settle, encoder requires this
        time.sleep(0.1)
        # Enable the modulator
        GPIO.output (22, True)
        # keep enabled for a period
        time.sleep(0.25)
        # Disable the modulator
        GPIO.output (22, False)
    else:
        GPIO.output (11, False)
        GPIO.output (15, True)
        GPIO.output (16, True)
        GPIO.output (13, False)
        # let it settle, encoder requires this
        time.sleep(0.1)
        # Enable the modulator
        GPIO.output (22, True)
        # keep enabled for a period
        time.sleep(0.25)
        # Disable the modulator
        GPIO.output (22, False)

def stateErr():
    print('Please select on or off for state')

def socketErr():
    print('Please select 1 or 2 for socket')

socket = sys.argv[1]
state = sys.argv[2]

if socket == '1':
    if state == 'on' or state == 'off':
        socket1(state)
    else:
        stateErr()
elif socket == '2':
    if state == 'on' or state == 'off':
        socket2(state)
    else:
        stateErr()
else:
    socketErr()

# Clean up the GPIOs for next time
GPIO.cleanup()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
