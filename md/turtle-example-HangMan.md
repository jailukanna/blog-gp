---
title: turtle example HangMan (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'HangMan'

Functions in program: 
* `def hangMan():`

## python HangMan

Python turtle example: HangMan

```python
def hangMan():
    numberOfLives=7
    Answer=input("please enter the name of your movie: ")
    finalAnswer=""
    print(50*"\n")
    print("You Have ",numberOfLives," lives left")
    AnswerList=list(Answer)
    for i in range (len(AnswerList)):
        if AnswerList[i]=='a' or AnswerList[i]=='e' or AnswerList[i]=='i' or AnswerList[i]=='o' or AnswerList[i]=='u':
            AnswerList[i]="O"
        elif 48<=ord(AnswerList[i])<=57:
            AnswerList[i]="#"
        elif AnswerList[i]==' ':
            AnswerList[i]="/"
        else:
            AnswerList[i]="_"        
    print(' '.join(AnswerList))
    
   
    while (numberOfLives!=0 and (finalAnswer!=Answer)):
        YourGuess=input("enter your guess: ")
        if YourGuess in Answer:
            for char in range(len(AnswerList)):
                if Answer[char]==YourGuess:
                    AnswerList[char]=YourGuess
            finalAnswer=''.join(AnswerList)
            print(finalAnswer)
        else:
            numberOfLives-=1
            print("sorry you only have ",numberOfLives," remaining") 
           
            
      
        
    
            

          

    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
