# chromium_GUI
Installing chromium in ubuntu with GUI
we want to run chromium on a vps using XFCE GUI panel and XRDP connection and windows remote desktop.

1. Update and Upgrade Your VPS

```
sudo apt update
sudo apt upgrade -y
```

2. Install XFCE Desktop Environment
```
sudo apt install xfce4 xfce4-goodies -y
```

3. Install XRDP for remote desktop connection
```
sudo apt install xrdp -y
```

4. Enable XRDP To ensure XRDP starts automatically on boot
```
sudo systemctl enable xrdp
```

Now we need to switch to Xorg as the default display server 

5. Create an XRDP startup script
```
sudo nano /etc/xrdp/startwm.sh 
```

6. Add the following content to the end of startwm.sh file
```
#!/bin/sh

if [ -x /etc/X11/xinit/xserverrc ]; then
        exec /etc/X11/xinit/xserverrc
fi

unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4
```

7. Make the script executable
```
sudo chmod +x /etc/xrdp/startwm.sh
```

8. Restart XRDP
```
sudo systemctl restart xrdp
```

9. (optional) Ensure that your VPS firewall (mine was not active) is allowing connections on port 3389 (the default XRDP port)
```
sudo ufw allow 3389
sudo ufw reload
```
Now we want to install chromium on the vps
When you login form windows remote desktop connection as root user and trying to install chromium, you get an error, because it goes against Chromium's security measures, so we should switch to a regular user. You can add a regular user in putty
1. Create a new ordinary username
```
useradd yourusername
passwd yourusername
```
2. Add the new user to the sudo group, you should use the new username you created in the previous step instead of yourusername in the all following steps
```
sudo usermod -aG sudo yourusername 
```

next you should switch to the new user, but before that you should do the following

3. Create a home directory for the new user
```
sudo mkdir /home/yourusername
```

4. Set the correct ownership and permissions
```
sudo chown yourusername:yourusername /home/yourusername
```
```
sudo chmod 700 /home/yourusername
```

5. Switch to the new user
```
su - yourusername
```

6. Install Chromium
```
sudo apt install chromium-browser -y
```

Now connect using windows remote desktop connection and your new user name credentials
7. Open terminal in the graphical interface and run Chromium
```
chromium-browser
```

Also you could run chromium using application in the upper left of the screen and in the internet you could find chromium

