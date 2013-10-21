wit *
=

**Track the meaningful from your bash history.**

When fiddling with experimental software on your linux box, <br>
you would like to save the commandline process that led you to a working setup. <br>

You would like to have a file where all the important commands are stored, for the record. <br>

## Usage
A ```wit``` is a saved set of command line(s), optionally tagged and commented.

Issue a *wit command* ```#:``` after the commands you want to save, <br>
and the last command will be stacked to a file <br>
```
#:[count][:jump] [tag] [comment]
```
**With:** <br>
the line ```count``` you want to stack as a wit *(default: 1)*<br>
The number of lines you want to ```jump``` *(default: 0)*, <br>
    this will ignore the first n lines which will not be counted <br>
The ```tag``` to associate with this wit, for later sorting *(default: 'default')*<br>
A ```comment``` you would like to associate *(default: null)*<br>
*All arguments are optional*

## Example <br>
* **Wit stacks your last command line, with a 'default' tag name**:
```sh
apt-get install python python-bottle python-paste lighttpd
#:
```

* **Stack multiple lines at once**:
```sh
apt-get install motion
echo start_motion_daemon=yes > /etc/default/motion
service motion start
#:3
````

* **You can ignore unmeaningful trailing lines**:
```bash
apt-get install gpio
ls
pwd
#::2
````

* **This will stack the 4 last lines of your history, jumping the last command**: <br>
Specifying a ```tag``` makes wit write contents into a separate file named ater the tag. <br>
A comment will make wit write separators around the commandlines set.
```sh
apt-get install python python-bottle python-paste lighttpd
scp agent@receipts.example.com:`uname -n`/lighttpd/100-squareeye-web.conf /etc/lighttpd/conf-available/.
ln -s /etc/lighttpd/conf-available/100-square-eye.conf /etc/lighttpd/conf-enabled/.
service lighttpd restart
curl localhost
#:4:1 sqeye paste+lighty setup and site config
```

* **You can comment without tagging**:
```sh
echo "4" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio4/direction
#:2  setting up gpio port
```

* **You easily stack the command output**: <br>
Wit will remove it's own pipe
```sh
apt-get install screen | wit -c "Installing screen, yay"
```

## Viewing
Wit stacks command lines into plain files names after their tag.
```
~/.wit/default.sh.log
~/.wit/sqeye.sh.log
```

Containing (```cat ~/wit/*```):
```
#
# Tag: default
#

apt-get install python python-bottle python-paste lighttpd
apt-get install motion
echo start_motion_daemon=yes > /etc/default/motion
service motion start

apt-get install gpio

# setting up gpio port
echo "4" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio4/direction

# Installing screen, yay
apt-get install --yes screen
# Lecture des listes de paquets... Fait
# Construction de l'arbre des dépendances       
# Lecture des informations d'état... Fait
# Paquets suggérés :
#   iselect screenie byobu
# Les NOUVEAUX paquets suivants seront installés :
#   screen
# 0 mis à jour, 1 nouvellement installés, 0 à enlever et 86 non mis à jour.
# Il est nécessaire de prendre 0 o/650 ko dans les archives.
# Après cette opération, 923 ko d'espace disque supplémentaires seront utilisés.
# Sélection du paquet screen précédemment désélectionné.
# (Lecture de la base de données... 69108 fichiers et répertoires déjà installés.)
# Dépaquetage de screen (à partir de .../screen_4.1.0~20120320gitdb59704-7_armhf.deb) ...
# Traitement des actions différées (« triggers ») pour « install-info »...
# Traitement des actions différées (« triggers ») pour « man-db »...
# Paramétrage de screen (4.1.0~20120320gitdb59704-7) ...

#
# Tag: sqeye
#

# sqeye paste+lighty setup and site config
apt-get install python python-bottle python-paste lighttpd
scp agent@receipts.example.com:`uname -n`/lighttpd/100-squareeye-web.conf /etc/lighttpd/conf-available/.
ln -s /etc/lighttpd/conf-available/100-square-eye.conf /etc/lighttpd/conf-enabled/.
service lighttpd restart
```

## Additional fun
* **Comment, optionally tagged, can be stacked without a commandline**: <br>
Note the double space between *count* and *comment*
```sh
#:0  You sould tune your ~/.screerc
```

* **Undo wits and amend stuff easily**:
Updates the tag and comment of stacked wits
```sh
Undo the last wit
#:u  # or,
#:undo
Undo the last 2 wits
#:u:2
Undo the last 2 with juping the last wit
#:u:2:3
Post-tag the last wit
#:-1 gpio  # or,
#:- gpio
Post-comment the last wit
#:-1  setting up gpio port 4 with out direction
Post-tag the last 3 wits
#:: default
Post-tag the penultimate wit
#:-1:-1 default  # or,
#:-:-1  # or,
#::-1
Post-tag the 3 wits 5 lines above
#:-3:-5 default
```

* **Reviewing and digging wit stack**:
```sh
Show the last wit
#:-1  # or,
#:-
Show the last 2 wits
#:-2
Show the last 2 wits, skipping the last wit
#:-2:-1
```

* **Plain command line usage**:
```sh
# Actual wit command (without the #: shortcut)
# wit [count] [jump] [tag] [comment]
wit --count 1 --jump 0 --tag "my-tag" --comment "my comment"
```

Also, this software can be used together with [canonical-bash-history](https://github.com/damiencorpataux/bash-canonical-history).
Especially if you're using *screen* and experience the *multiple histories hassles*.

*Note that: this software is at idea incubation stage*

## fixme
* Invoke command using a plain ```wit``` command, instead of the spicy ```#:``` <br>
   * This makes implementation simply plain
   * Using ```#:``` lets you don't worry about ()"!? chars interpretation, which is very handy
