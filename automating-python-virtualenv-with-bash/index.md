# Automating Python Virtualenv With Bash



I always forget how to use Python's virtual environment. It's simple. You'd think I wouldn't forget but I do. I don't use it enough, and I should. 

## What is Python Virtual Environment

According to [Real Python]()
> At its core, the main purpose of Python virtual environments is to create an isolated environment for Python projects.
> This means that each project can have its own dependencies, regardless of what dependencies every other project has.  


This is useful in one very important way. There are times when python packages can interfere with each other and they dependencies can get In the way of your script or program, causing all sorts of various bugs. 

---

## How to make a Python Virtual Environment

Usually, this is how you create a virtual environment using python.

Create Project Directory.  
```$ mkdir Project```

Change into directory.  
```$ cd Project``` 
 
Create the virtual environment.  
```$ python3 -m venv venv```  

Activate virtual environment.  
```$ source venv/bin/activate``` 


This is pretty simple but I wanted to make it easier, quicker. I may have a bit of a tendency to be lazy at times.  

![Kevin Malone from the office saves time](https://thumbs.gfycat.com/CoarseAgileEyelashpitviper-size_restricted.gif)


## Creating the Bash Script

For this script, I want to be able to call the command and enter the folder I want to make and put the python virtual environment folder in. So it'll need: 

+ To take an argument of folder path
+ Check if folder exists
+ if it doesn't create the folder, if does generate the virtual environment
+ and finally, activate the virtual environment

Let's create the bash script and call it _createVenv.sh_

_createVenv.sh_
```bash
#!/bin/bash
YELLOW='\033[1;33m'
RED='\033[0;31m'
GREEN='\033[0;32m'
CYAN='\033[0.36m'

# Check if argument is used
if [ $# -ge 1 ]
then
  # if directory doesn't exist
  # make directory
  if [ ! -d $1 ]
  then
    printf "${YELLOW}[-]${YELLOW} ${CYAN}Creating directory for $1${CYAN}\n"
    mkdir $1
  fi

  # create & activate virtual environment
  printf "${YELLOW}[-]${YELLOW} ${CYAN}Building virtual environment${CYAN}\n"

  /usr/bin/python3 -m venv $1/venv
  printf "${GREEN}[+] Successfully created virtual environment${GREEN}\n"
  source $1/venv/bin/activate

else
  printf "${RED}[!] Usage: createVenv.sh [ folder ] to create virtual environment${RED}\n"
	
  printf "${YELLOW}[-] Usage: source createVenv.sh [ folder ] to create & use virtual environment${YELLOW}\n"
fi
``` 

Next, before we can run it, we first need to make it executable   
```$ sudo chmod +x createVenv.sh``` 

And finally, create a symbolic link so we can run it like a normal bash command.  
```$ ln -s createVenv.sh /usr/local/bin/createVenv``` 
 
> If you aren't sure how to handle symbolic links properly, I wrote a quick article [here](http://virbuntu.com/using-symbolic-links-in-linux/).  

---

You can run this command two different ways. 

1. ```$ createVenv /path/to/Project``` : running the command this way will create the directory if needed and create the virtual environment.  
2. ```$ source createVenv /path/to/Project``` : everything that the top does AND it activates the virtual environment so you can get started right away.  


> <span style="color: yellow;">**WARNING**: It is important you don't end the path to project with the closing `/`. This will confuse your activation and make a path like this `//`.</span> 

## Closing Thoughts
Hopefully, this is as useful to you as it is for me. I thought it would be fun to start tinkering with bash scripting and I figured this would be my first shot at it. Keep looking out for more notes that could be useful. Thanks for reading.

