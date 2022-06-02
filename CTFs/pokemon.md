---
layout: default
---

## TryHackMe | Gotta Catch Em All

### Step 1: Recon:

Start with an nmap scan against the target machine: 

```
nmap -sV -sC [Target IP]
```
We use -sV for a more verbose response, and -sC for standard scripts. Additionally, if you'd like to put the output into a file, you can add -oN [filename].

Our scan shows us that ports 22 (SSH) and 80 (http) are open. Lets go check the webpage first.

First thing to do is to check the page source to see if there's any hints/clues. You can right click and select view page source, or ctrl+U.

At the bottom of the page we'll find a strange entry above a line that says to check the console for a surprise. Maybe these could be SSH credentials? Let's try it!

```
ssh [username]@[targetIP]
```

Boom! We're in! 

### Flag 1

Let's first check the users Desktop to see if we can find any of the flags there. We find a zip file named "P0kEm0n.zip" Lets unzip the file and see what's inside

```
unzip P0kEm0n.zip
```
Now go into the new directory and we'll find our first grass type flag, but it just looks like a bunch of random numbers. It's actually just encoded with hexadecimal! Go check out [cyberchef](https://gchq.github.io/CyberChef/) and decode the output for your first flag.

### Flag 2

Our first flag was formatted with the syntax "grass-type.txt" so let's see if our other flags happen to have the same format by using the find command.

```
find / -name water-type.txt 2>/dev/null
```
We got the location of our next flag! Let's go check it out.

If you cat out the file, our flag looks like garbled nonsense. That's because it's encoded! This file happens to be encoded with Caesar Cipher. You can just google a decoder for this and paste the output of the file. We now have our 2nd flag!


### Flag 3

We can use the same find command to find our fire-type flag!

```
find / -name fire-type.txt 2>/dev/null
```

This one points to the /etc/why_am_i_here? directory. Lets check out the file.

This one is also encoded! Looks like base64. You can either use cyberchef to decode this one, or you can use the command line tool to decode it for your flag:

```
cat [base64 encoded info] | base64 -d [ > file(if you want the output to be put into a new file)]
```

### Flag 4

This one requires a bit more searching. If we go back to the /home/ directory, we'll see there's a file for "roots-pokemon.txt". This might be what we're looking for, but we don't have access to read it. If we try to access the ash directory, we also don't have the ability to enter. Lets go back to the pokemon directory and see if there's anything we can find. 

After scouting around, in the Videos directory there's a chain of directories:
```
pokemon@root:~/Videos/Gotta/Catch/Them/ALL!$ 
```
Inside is a file called: Could_this_be_what_Im_looking_for?.cplusplus; Let's see what's inside.

Looks like it could be potentially credentials for the ash account! You can either SSH back into the machine as ash with the password you found, or you can just use
```
su ash
```
Now go get that last flag from roots-pokemon.txt.

## Congratulations! You've completed the Gotta Catch'em All Room!

