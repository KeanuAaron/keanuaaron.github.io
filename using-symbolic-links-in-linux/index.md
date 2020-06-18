# Using Symbolic Links In Linux


# Using Symbolic Links In Linux
Recently I learned about symbolic links and what they were. I had used them once time without really understanding what I was doing.  

Symbolic links are in a way like a road sign telling you what direction to go to. They point the computer towards the actual file so it knows where to get it from. Without a road sign you wouldn't know where to go and it's the same for a computer.  

---
## How to Create a Symbolic Link in Linux

To create a symbolic link you would follow the syntax of this command:  

`ln -s [source file] [target file]`  

The `-s` is what makes it a symbolic link over a hard link. You can learn more about hard links [here](https://blog.usejournal.com/what-is-the-difference-between-a-hard-link-and-a-symbolic-link-8c0493041b62).  

The source file represents what file you want to lead your computer to, and the target file represents the road sign pointing the way to the source file.  

## Symbolic Links Use Cases  
One way I found a symbolic link useful to me was when I needed to use a python class for a file that I couldn't move from it's place, and making the new script I was running needed to stay in its location. Neither could move. So there was a problem.  

We are going to recreate something similar.  

Create a new file called `sayHello.py` in your Documents folder and copy this code into it.

_~/Documents/sayHello.py_
```python
class SayHello:
  def __init__(self):
    self.greeting = 'Hello, World!'

  def say_hello(self):
    print(self.greeting)
```
This is a simple script that just prints `Hello, World!`.

Now move into your Desktop and create a new file called `grabHello.py` and copy this code into it.

_~/Desktop/grabHello.py_
```python
from sayHello import SayHello

s = SayHello()
s.say_hello()
```

If you run this command right now with `$ python3 grabHello.py` you will get this error:  

```
Traceback (most recent call last):
  File "grabHello.py", line 1, in <module>
    from sayHello import SayHello
ModuleNotFoundError: No module named 'sayHello'
```  

This is because you haven't given the computer a road sign where to go. This is where the symbolic link will come to play.  

Let's create a road sign, or symbolic link.  

`$ ln -s ~/Documents/sayHello.py ~/Desktop/sayHello.py`  

You can verify that you created a symbolic link using the `ls -l` command.  

```
$ ls -l
total 2
-rw-r--r--   1 virbuntu  staff    60 Jun 16 19:13 grabHello.py
lrwxr-xr-x   1 virbuntu  staff    21 Jun 16 19:18 sayHello.py -> /home/$USER/Documents/sayHello.py
```  

If you created the symbolic link successfully, you should see an arrow `->` pointing towards the location of the original file.  

Now run your `grabHello.py` script and see the output.  

```
$ python3 grabHello.py
Hello, World!
```  

## Deleting Symbolic Links In Linux
Whenever you want to get rid of symbolic links in linux you can do so without issue, but you **must be careful** about the way you do it.  

To give you an example of what can go wrong I will guide you through a **do not do** example.  

First, make a directory called test in your home directory.  
`$ mkdir ~/test`  

Then copy files over from your `/etc` directory. **[ Note: We are making copys of the files, we won't let any of your important files get lost. ]**  
`$ cp /etc/* ~/test`  

> You will get some permission denied errors, but if you `ls ~/test` you'll see that we still managed to copy files over successfully.  

Now, create a symbolic link called `link`  
`$ ln -s ~/test link`  

You can verify the symbolic link with the `ls -l` command.  

**PAY CLOSE ATTENTION HERE**  
There are times when we all use tab completion on the linux terminal to get the rest of the word spelled out for us. This is a very helpful feature. **However**, when it comes to symbolic links. It could ruin your day if you aren't careful!  

Run this command to delete the symbolic link we created:  
`$ rm -rf link/`  

After you run this command, use `ls -l` and you will see that the symbolic link still exists but the `test` directory does not.  

```
$ ls -l
total 1
lrwxr-xr-x   1 virbuntu  staff    21 Jun 16 19:18 link -> /home/$USER/test
```  

In this example, by adding the `/` at the end, linux interpreted it as the source location causing it to assume you wanted to delete the source of the symbolic link.  

#### When Deleting Symbolic Links, NEVER use `-rf` and avoid a trailing slash `/`.  

The proper way to delete the symbolic link we created earlier is with:  
`$ rm link`  

This will delete ONLY the symbolic link and keep the source file(s) in tact.  

---   
In this guide, you were able to get a good understanding of how symbolic links work in Linux and how you can utilize them to your advantage. You also learned how to properly delete symbolic links in a safe way that will keep all your important files safe from deletion.


