# Python with Ubuntu on Windows

http://timmyreilly.azurewebsites.net/python-with-ubuntu-on-windows/

---

**1. Install Bash on Windows**

**2. Check for updates**

```
$ sudo apt-get update
```

**3. Check out the REPL** 

```
$ python
Python 2.7.6 .....................

# or
$ python3
Pythone 3.5.0 .....................
```

**4. Install Pip** 

```
sudo apt-get install python3-pip

# if run into permission issue
$ sudo -i 
$ apt-get install python-pip
$ exit

# see what comes with pip
pip list
```

**5. Install VirtualEnv**

ref: http://docs.python-guide.org/en/latest/dev/virtualenvs/

```
$ sudo -i
$ pip3 install virtualenv
$ exit
$ cd my_project_folder
$ virtualenv venv

# to use the VirtualEnvironment
$ source venv/bin/activate

# then (venv) will appear indicating u r in virtual env
(venv)username@...........$

# to deactivate
$ deactivate
```

**6. Install VirtualEnvWrapper** 

ref: http://virtualenvwrapper.readthedocs.io/en/latest/index.html 

```
$ sudo -i
$ pip3 install virtualenvwrapper
$ exit
$ export WORKON_HOME=~/Envs
$ source /usr/local/bin/virtualenvwrapper.sh

# if it cannot find virtualenvwrapper.sh, sth is wrong with the installation, do it again
# or try this:
$ cd ~
$ sudo nano .bashrc
# add this to the bottom, save
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
```

**7. Create virtualenv using virtualenvwrapper** 

```
# in working folder
$ mkvirtualenv venv
$ workon venv
$ deactivate
```

**8. Configure bashrc to keep it working** 

```
# might not happen but may need to re-source virtualenvwrapper.sh and WORKON_HOME in new bash,
# so do this:
$ sudo nano ~/.bashrc
-- type in password --

export WORKON_HOME=~/Envs
source /usr/local/bin/virtualwrapper.sh
# save it, then:
Source ~/.bashrc
# or open a new bash and try
$ workon
# or 
$ lsvirtualenv
```

**9. Install some packages** 

```
$ pip3 install thonny
```

---

# Thonny

**1. Pip install thonny** 

or pip3 install thonny

**2. Install Xming**

ref: https://stackoverflow.com/questions/48254530/tkinter-in-unbuntu-inside-windows-10-error-no-display-name-and-no-display-en

ref: https://sourceforge.net/projects/xming/?source=typ_redirect

since bash on ubuntu on win10 does not have a GUI , we need Xming to connect Windows GUI to ubuntu so that ubuntu can display more than command line

**3. modify ~/.bashrc**

add this to the bottom

```
export DISPLAY=:0
```

**4. run thonny**

```
run Xming on windows

$ thonny
```

