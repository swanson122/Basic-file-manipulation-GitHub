## Lab-5
This is a guide to Lab 5 in HCS 7194. In this lab we will practice the inspection of files. We will use the commands ```ls```, ```wc```, ```head```, ```tail```, and the use of piping (|), as well as the use of ```less```, ```grep```, and practice with ```tar``` and ```gunzip```.

# Preliminaries
For this Lab you should have in your computer already installed the following software
* GitHub Desktop: https://desktop.github.com
* Atom: https://flight-manual.atom.io/getting-started/sections/installing-atom/
  * In Atom, go to the menu Preferences, Install and type: Atom IDE Terminal by Qicrosoft, install the package and restart Atom 

# Let's start simple: connect to OSC using ssh and your terminal
```
ssh username@owens.osc.edu
```
Then, let's go to our working directory and make a new directory called ```Lab_5```
```
# Cheking in which directory you are located
pwd
# Moving to the correct working directory
cd /fs/scratch/PAS####/username
# Creating the new directory
mkdir Lab_5
# Listing your directories
ls
# Entering to the directory Lab_5
cd Lab_5
```
Now, let's get a basic text file by using wget:
```
wget cdn.learnenough.com/sonnets.txt
```
A file called sonnets.txt should be in your directory, let's start to inspect it.
