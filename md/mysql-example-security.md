---
title: mysql example security (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'security'

Functions in program: 
* `def main():`
* `def oracle(s, infile1, outfile1, outfile2):`
* `def modcbc(infile, iv):`
* `def verify(key, aes, iv, infile, outfile):`
* `def submit(s, key, aes, iv, infile, outfile):`
* `def url_encode(s):`
* `def cbc(file, outfile):`
* `def ecb(file, outfile):`
* `def ttp(file1, file2, outfile):`
* `def otp(filename, outfile):`
* `def xor(string1, string2):`

## python security

Python mysql example: security

```python
#Lab 2
from os import urandom
from Crypto.Cipher import AES

def xor(string1, string2):
    newString = ""
    if len(string1) == len(string2):
        newString = ''.join(chr(ord(s1)^ord(s2)) for s1,s2 in zip(string1, string2))
    else:
        print("strings are not equal lengths")

    return newString

'''
one time pad
'''
def otp(filename, outfile):
    #get header from bmp
    f2 = open(filename)
    header = f2.read(54)

    #grab text from file
    f = open(filename, 'r')
    plaintext = ""
    for line in f.readlines():
        for c in line:
            plaintext += c

    #generate key of same length as plaintext
    key = urandom(len(plaintext))
    #create cipher by xor
    cipher = xor(plaintext, key)

    #write to output file
    output = open(outfile, 'w')
    for h in header:
        output.write(h)

    for c in cipher:
        output.write(c)
'''
two time pad
'''
def ttp(file1, file2, outfile):
    #get header from bmp
    f3 = open(file1)
    header = f3.read(54)

    #grab text from file1
    f = open(file1, 'r')
    plaintext = ""
    for line in f.readlines():
        for c in line:
            plaintext += c

    #grab text from file2
    f2 = open(file2, 'r')
    plaintext2 = ""
    for line in f2.readlines():
        for c in line:
            plaintext2 += c


    #generate key of same length as plaintext
    key = urandom(len(plaintext))
    #create cipher by xor
    cipher = xor(plaintext, key)
    cipher2 = xor(plaintext2, key)

    cipher3 = xor(cipher, cipher2)

    #write to output file
    output = open(outfile, 'w')
    for h in header:
        output.write(h)

    for c3 in cipher3:
        output.write(c3)

    f.close()
    f2.close()
    f3.close()
    output.close()

def ecb(file, outfile):
    f = open(file, 'r')
    header = f.read(54)

    output = open(outfile, 'w')

    for h in header:
        output.write(h)

    key = urandom(16)
    aes = AES.new(bytes(key))

    while True:
        pt = f.read(16)
        if not pt:
            break
        if len(pt) < 16:
            pad = 16 - (len(pt) % 16)
            for i in range(pad):
                pt += chr(pad)
        ct = aes.encrypt(pt)
        output.write(ct)

    f.close()
    output.close()

def cbc(file, outfile):
    f = open(file, 'r')
    header = f.read(54)

    output = open(outfile, 'w')

    for h in header:
        output.write(h)

    key = urandom(16)
    aes = AES.new(bytes(key))
    iv = urandom(16)
    
    index = 0
    while True:
        pt = f.read(16)
        if not pt:
            break
        if len(pt) < 16:
            pad = 16 - (len(pt) % 16)
            for i in range(pad):
                pt += chr(pad)
        if index == 0:
            ct = aes.encrypt(xor(pt, iv))
            index = index + 1
        else:
            ct = aes.encrypt(xor(pt, ct))
            index = index + 1
        output.write(ct)

    f.close()
    output.close()

def url_encode(s):
    while ';' in s:
        ndx = s.index(';')
        s = s[:ndx] + "%3B" + s[ndx+1:]
    
    while '=' in s:
        ndx = s.index('=')
        s = s[:ndx] + "%3D" + s[ndx+1:]

    return s



def submit(s, key, aes, iv, infile, outfile):
    #create user string
    prefix = "userid=456;userdata="
    suffix = ";session-id=31337"
    newString = prefix + s + suffix

    newString = url_encode(newString)

    #write string to file
    f = open(infile, 'w')
    for c in newString:
        f.write(c)
    f.close()

    #read string from file
    f2 = open(infile, 'r')
    output = open(outfile, 'w')
    
    #encrypt using cbc method
    index = 0
    while True:
        pt = f2.read(16)
        if not pt:
            break
        if len(pt) < 16:
            pad = 16 - (len(pt) % 16)
            for i in range(pad):
                pt += chr(pad)
        if index == 0:
            ct = aes.encrypt(xor(pt, iv))
            index = index + 1
        else:
            ct = aes.encrypt(xor(pt, ct))
            index = index + 1
        output.write(ct)
    f2.close()
    output.close()

def verify(key, aes, iv, infile, outfile):
    #read string from file
    f2 = open(infile, 'r')
    output = open(outfile, 'w')

    modcbc(infile, iv)
    
    #decrypt cbc
    fullpt = ""
    index = 0
    while True:
        ct = f2.read(16)
        if not ct:
            break
        if index == 0:
            pt = xor(aes.decrypt(ct), iv)
            prevCT = ct
            index = index + 1
        else:
            pt = xor(aes.decrypt(ct), prevCT)
            prevCT = ct
            index = index + 1
        output.write(pt)
        fullpt += pt
    f2.close()
    output.close()
    
    if ";admin=true;" in fullpt:
        result = True
    else:
        result = False

    return result

def modcbc(infile, iv):
    f = open(infile, 'r')
    firstblock = f.read(16)
    secondblock = f.read(16)
    ct = ""
    for line in f.readlines():
        for c in line:
            ct += c
    
    newCT = ""
    for c in firstblock:
        newCT += chr(ord(c)^7)
    
    ct = newCT + ct

    #clear file
    f = open(infile, 'w')
    f.close()

    output = open(infile, 'w')
    for c in ct:
        output.write(c)
    output.close()


def oracle(s, infile1, outfile1, outfile2):

    key = urandom(16)
    aes = AES.new(bytes(key))
    iv = urandom(16)

    submit(s, key, aes, iv, infile1, outfile1)
    verify(key, aes, iv, outfile1, outfile2)


def main():

    s1 = "Darlin dont you go"
    s2 = "and cut your hair!"
    r = xor(s1,s2)
    print(r.encode("hex"))
    r2 = xor(s1, r)
    print(r2)

    #otp("test.txt", "result1.txt")

    '''
    otp("mustang.bmp", "mcipher.bmp")
    otp("cp-logo.bmp", "cpcipher.bmp")
    '''

    #ttp("cp-logo.bmp", "mustang.bmp", "ttp.bmp")

    #ecb("cp-logo.bmp", "cp-ecb.bmp")
    #cbc("cp-logo.bmp", "cp-cbc.bmp")

    oracle("You're the man now, dog", "sIn.txt", "sOut.txt", "vOut.txt")

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
