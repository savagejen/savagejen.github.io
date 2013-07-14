---
layout: post
title: "Python boolean *features*"
date: 2013-05-16 10:12
comments: true
categories: 
---
([Amatus](https://github.com/amatus)) brought to my attention the fact that True can be set to False in python.

So, for instance, if you open up your python interpreter and do:
```
>>> True=False
>>> print True
False
```
So, True can be set to False. True can be set to None. False can be set to True. False can be set to None. Luckily setting None to anything will give you an assignment error. Why on earth setting True to False doesn't give you an assignment error, I will never understand. I certainly don't consider this to be "expected behavior"or anything.

After you get over the shock of realizing that python's boolean values are not immutable, you might think "Ok, but why would I ever set True=False?"
I have no idea! But check this out:
```
>>> something=True=False
>>> myVariable=something
>>> myOtherVariable=True
>>> print myVariable
False
>>> print myOtherVariable
False
>>> print True
False
```
So you can change the value of True or False on the same line as you set a variable and that redefinition will affect setting every boolean value following it.

([The PEP for bool is also really a bizarre and interesting read.](http://www.python.org/dev/peps/pep-0285/))

