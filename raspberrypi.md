Raspberry Pi Setup
=============
###By Tara Furstenau
----------------------  
1. Configure Keyboard
---------------------------
```bash
$ sudo nano /etc/default/keyboard
```  
or  
```
$ sudo dpkg-reconfigure keyboard-configuration
```  

2. Configure Locale
-----------------------
```bash
$ sudo dpkg-reconfigure locales
```  
Use ```en_US.UTF-8``` for English

3. Configure Timezone
-----------------------------
```
$ sudo dpkg-reconfigure tzdata
```  

4. Raspberry Pi Configure
-------------------------------
```
$ sudo raspi-config 
```  
Reboot to apply changes  

5. Shutdown and Reboot
----------------------
```
$ sudo shutdown now %now is a time argument
```  
or    
```
$ sudo reboot
```  
or  
```
$ sudo shutdown -r now %reboot with time argument
```  

6. Automatic Login
----------------------
```
$ sudo nano /etc/inittab
```  
Scroll down to:  
```
1:2345:respawn:/sbin/getty --noclear 38400 tty1
```  
Comment this line out by adding a # sign at the beginning of the line  
Add a new line:  
```
1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2&1
```  
Assuming that pi is the username for the account that you want to automatically log into.  

7. Auto StartX
-----------------
```
$ sudo nano /etc/rc.local 
```  
above ```exit 0``` add ```su -l pi -c startx```  

8. Listing Connected USB Devices
---------------------------------------
```$ lsusb```  

9. Running Command-Line Script on Startup
----------------------------------------------------
Create a new init file:  
```$ sudo nano /etc/init.d/file_name```  
Paste the following code into the editor window and save the file:  
```
	###BEGIN INIT INFO
	# Provides: file_name
	# Required-Start: $remote_fs $syslog $network
	# Required-Stop: $remote_fs $syslog $network
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Simple Web Server
	# Description: Simple Web Server
	### END INIT INFO
	
	#! /bin/shutdown#
	# /etc/init.d/file_name
	
	export HOME
	case "$1" in 
		start)
			echo "Starting My Server"
			sudo /usr/bin/python /home/pi/myserver.py 2>&1 &
		;;
	stop)
		echo "Stopping My Server"
		PID=`ps auxwww | grep myserver.py | head -1 | awk '{print $2}'`
		kill -9 $PID
		;;
	*)
		echo "Usage: /etc/init.d/file_name {start|stop}"
		exit 1
	;;
	esac
	exit 0
```  

Make file executable for the owner:  
```$ sudo chmod +x /etc/init.d/file_name```  
Test with:  
```
$ /etc/init.d/file_name start
```  
