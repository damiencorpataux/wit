wit *
=

**Track the meaningful from your bash history.**

When fiddling with experimental software on your linux box, <br>
you would like to track the commandline process that led you to a working setup. <br>

You would like to have a file where all the important commands are stored, for the record. <br>

#### Usage
Usage is very simple, issue a *wit command* ```#:``` after the commands you want to track, <br>
and the last command will be saved to a file <br>
```
#:[count][:jump] [tag] [comment]
```
**With:** <br>
the line ```count``` you want to save as a wit *(default: 1)*<br>
The number of lines you want to ```jump``` *(default: 0)*, <br>
    this will ignore the first n lines which will not be counted <br>
The ```tag``` to associate with this wit, for later sorting *(default: 'default')*<br>
A ```comment``` you would like to associate *(default: null)*<br>
*All arguments are optional*

**Example:** <br>
**Wit saves your last command line, with a 'default' tag name**
```sh
apt-get install python python-bottle python-paste lighttpd
#:
```

**Save multiple lines at once**
```sh
apt-get install motion
echo start_motion_daemon=yes > /etc/default/motion
service motion start
#:3
````

**You can ignore unmeaningful trailing lines**
```bash
apt-get install python python-bottle python-paste lighttpd
ls
pwd
#::2
````

**This will save the 4 last lines of your history, jumping the last command:**
Specifying a ```tag``` makes wit write contents into a separate file named ater the tag. <br>
A comment will make wit write separators
```sh
apt-get install python python-bottle python-paste lighttpd
scp agent@receipts.example.com:`uname -n`/lighttpd/100-squareeye-web.conf /etc/lighttpd/conf-available/.
ln -s /etc/lighttpd/conf-available/100-square-eye.conf /etc/lighttpd/conf-enabled/.
service lighttpd restart
curl localhost
#:4:1 sqeye paste+lighty setup and site config
```

**You can comment without tagging:**
```sh
echo "4" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio4/direction
#:2 #setting up gpio port
```

#### Viewing
Wit saves command lines into plain files names after their tag.


Also, this software can be used together with [canonical-bash-history](https://github.com/damiencorpataux/bash-canonical-history).
Especially if you're using *screen* and experience the *multiple histories hassles*.

*Note that: this software is at idea incubation stage*
