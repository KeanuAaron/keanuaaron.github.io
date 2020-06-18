# Linux I/O Redirection Explained


# Linux I/O Redirection Explained

By default when a command is executed, the result is shown on your monitor. This is known as the standard output, also known as **STDOUT** for short.  

When you input a command from a keyboard, the computer is taking input. This is known as **STDIN**, or standard input.  

Finally, when an error is thrown, it is known as, you guessed it. **STDERR**.  

---

Each one of these are represented by a number the terminal can understand (though some numbers aren't entirely necessary).

**Standard Input, Output, and Error Overview**  

| **Name** | **Default Destination** | **Use in Redirection** | **File Description Number** |  
|----------|:-----------------------:|:----------------------:|:---------------------------:|
| STDIN    | Computer keyboard       | < (same as 0<)         | 0 |
| STDOUT   | Computer monitor        | > (same as 1>)         | 1 |
| STDERR   | Computer monitor        | 2>                     | 2 |  

---

## STDOUT Made Simple
One of the most common examples you see for **STDOUT** is:  
`echo "hello, world" > hello.txt`  

In this common example, we are sending the text `hello world` into a text file called `hello.txt`. Now, you don't have to send text like that only. If you have a script running, or run any command that would normally output text to the terminal you could send to a file to review at a later time. An example of this would be a python script that outputs numbers.  

_numbers_out.py_
```python
for x in range(10):
  print(x)

# Normal Output
# 0
# 1
# 2
# ...
# 9
```  
Lets say you didn't want to have all that normal output on the bash terminal. An easy fix would be to send the normal output of the script into a log file you could read later.  

`$ python numbers_out.py > numbers.log`  

If you already had contents inside of numbers.log and wanted to append, or add more to the file without deleting what is already there, you could use `>>`  

`$ python numbers_out.py >> numbers.log`

---
## STDIN Made Simple
As mentioned earlier **STDIN** is input, this can be from the keyboard, or it can be from sending a file to a script or command to use as it needs. For this example we are going to create a simple bash script that outputs the contents of the file we created earlier. `numbers.log`.

_print_numbers.sh_
```bash
#!/bin/bash
while read line
do
  echo "$line"
done
```
This script can be used in two ways:  

**1.** `$ bash print_numbers.sh` : which will prompt us to type something and hit enter. It will then print out whatever we typed.  
**2.** `$ bash print_numbers.sh < numbers.log` : instead of asking us for input we are sending the contents of the earlier file. The script will then print out each line of the file.  

Both of these are examples of **STDIN**. The first one is input from the keyboard whereas the second example shows how you can give input from a file to a command or script.

---
## STDERR Made Simple
First off I'm going to start by saying this: I wish I new this one the moment I jumped on the terminal. Learning about **STDERR** redirection, I think, is a very handy thing to have in your tool belt.

There are times when you might have some programs or scripts running and you either don't need to worry about minor errors that are being thrown, or, you'd feel much better having all those errors thrown into a log file instead of having to scroll over tons of text trying to find where the error was.

In this example we are going to write a script that will give us an error. For the first example we will discard the error to `/dev/null`, you can read more about that [here](https://en.wikipedia.org/wiki/Null_device). In the second example, we will discard the errors into a log file.

_stderr_example.py_
```python
arr_list = [ 1, 2, 3, 'a' ]

for i in arr_list:
  print( int(i) )

# Output
# 1
# 2
# 3
# Traceback (most recent call last):
#   File "number_out.py", line 4, in <module>
#     print( int(i) )
# ValueError: invalid literal for int() with base 10: 'a'
```  
In the table earlier, you saw that to use the **STDERR** redirection, you have to use `2>` to send the errors where you wish.  
`$ python stderr_example.py 2> /dev/null`  

By sending the errors to `/dev/null` we will only see this output:
```
1
2
3
```  
There is no log file or any file created where we could see what went wrong. There are cases when this works fine. However, there are also tons more cases where you may want to review what went wrong. This can be done by sending the error contents to a log file.  

`$ python stderr_example.py 2> errout.log`  

Finally, if you wanted to send **BOTH** the output and the error to a log file you would combine both **STDERR** and **STDOUT** using their file descriptor numbers like this:  

`$ python stderr_example.py 2>&1 errout.log`


Hopefully this was simple and clear explanation of using I/O Redirection in Linux. I strongly urge you to start practicing and implementing these tactics in your daily usage in both Linux and Unix. In my opinion these are very great tactics to know and I wish I knew them earlier in my Linux journey to save me from some headaches. Thanks for reading through.


