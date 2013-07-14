---
layout: post
title: "Python Module Hijacking"
date: 2013-04-28 23:03
comments: true
categories: 
---
Reading through documentation, I noticed a peculiarity about python: it's susceptible to library hijacking attacks. 

Open your python interpreter and type in:
```
import sys
print sys.path
```
The result should be a list of paths that python will look in to find a module, in the order that python looks for them in. In other words, a pythonpath is the python equivalent of a DLL search path in windows.

You'll note that the very first item on the list is '', which means your current working directory. So if you import io, the first place python will look for the io module is your current working directory.

Now, let's say that someone places an io.py file in the same directory as your script. Now, the next time your script is run, it will load their version of io instead of yours. A user can essentially hijack the intended functionality of a module and replace it with their own, and all they need are write permissions to the same directory as your python script. For servers that offer shells to a variety of users, this type of attack could be used as a local privelege escalation on that box. 

For example, here we have a script poc.py that opens a file (I know, not very useful, but it's an example, so shhhh)
```
import io

io.open('spam.txt', 'w')
```
If you place this script in the same directory as the following io.py file:
```
def open(foo,bar):
	print "Pining for the fjords?!"
```
Then when you go to run poc.py, you will get the output "Pining for the fjords?!"

I sent this along to the python security team with a set of suggestions for fixing it. For one thing, I think the current working directory should be the last thing listed in the pythonpath by default. For another, python could provide a way for developers to specify the static path to their modules. 

The python security team let me know that this attack is already well known to them, and they have something called isolation mode planned for a future version of python.

The good news is that this doesn't work for protected modules! The bad news is that most modules aren't protected modules.

In the meantime, I'm told that you can configure virtualenv to only allow specific modules to be used by your python applications. 

And sysadmins, please, *never* allow people to write to the same directories that your python scripts run out of.
