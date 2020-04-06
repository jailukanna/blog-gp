---
title: mysql example question (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'question'


## python question

Python mysql example: question

```python
class Question(object):
    """
    This class represents a Question
    """
    
    def __init__(self, question, answer):
        """
        Accepts string objects for question and answer. They Will be set them in this question object.
        They can be accessed using the _question and _answer properties of the Question class.

        Both the 'question' and 'answer' arguments must of of type 'str'. If they are of any other type
        an exception will be raised
        """
        
        #Validate input arguments
        str_type = type("")
        if(type(question) != str_type):
            msg = "Incorrect argument for 'question' - It is of type %r but it must be of type %r " % (type(question), str_type)
            raise(BaseException(msg))

        if(type(answer) != str_type):
            msg = "Incorrect argument for 'answer' - It is of type %r but it must be of type %r " % (type(answer), str_type)
            raise(BaseException(msg))
            
            
        self._question = question
        self._answer = answer


if __name__ == "__main__":
    """ Test code for this module """
    
    q1 = Question("q1", "a1")

    try:
        q2 = Question("q2", 2)
        print("fail: Should have thrown an Exception")
    except:
        pass

    try:
        q3 = Question(3, "q3")
        print("fail: Should have thrown an Exception")
    except:
        pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
