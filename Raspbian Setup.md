# Raspbian Setup

## Write Rasbian image on SD card

1. Find the name of the sd card in **System Information** app. It's in Card Reader section.
2. Open **Disk Utility**' and Unmount the card.
3. Download .img file.
4. Run in **Terminal**:
	```bash
	sudo dd bs=4m if=imageFilePathAndName.img  of=/dev/rdiskn
    ```
	> The last `n` is the n in name found in step 1.
5. To see the progress, in **Terminal** press `Ctrl + t`.

## BackUp Rasbian disk
	
1. Change dir to your image file.
2. Run in **Terminal**:
	```bash
	sudo dd bs=4m if=/dev/rdiskn | gzip > rasbianBackUp.img.gz
	```
	> The `n` in rdiskn is as in step above.

### Restore from BackUp file

1. Open **Disk Utility** and Unmount the card.
2. Run in **Terminal**:
	```bash
	gunzip --stdout rasbianBackUp.img.gz | sudo dd bs=4m of=/dev/rdiskn
	```
	> The `n` in rdiskn is as in step above.




## Display Raspberry Pi’s desktop on a Mac
1. Enable **Internet Sharing** in **System Preferences**.
2. Connect to PI:
	```bash
	ssh pi@192.168.2.2
	```
	* The pass is: **raspberry**
	* If it says **WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!**, change the ssh key: 
		```bash
		ssh-keygen -R 192.168.2.2
		```

3. Install and setup VNC server:
	```bash
	sudo apt-get install tightvncserver
	vncserver :1 -geometry 1200x700 -depth 24
	vncserver
	```

4. Change cursor from X to a normal one:
	* Open file: `sudo nano  ~/.vnc/xstartup`
	* Change *xsetroot* parameter to: 
		> xsetroot -solid grey -cursor_name left_ptr

	* To restart vncserver run:
		```bash
       	vncserver -kill :1
        vncserver
        ```
5. On Mac, in **Safari**: 
	`vnc://192.168.2.2:5901`
	* set a password. *I set it as mdcel nr*.



## AUTOMATION AND RUN of vncserver AT BOOT

1. Log into a terminal on the Pi as root
	```
	sudo su
	```
2. Navigate to the directory:
	```
    cd /etc/init.d/
    ```
3. Create a new file(`sudo nano vncboot`) here, containing the following script:
	```bash
	#! /bin/sh
	# /etc/init.d/vncboot
	
	### BEGIN INIT INFO
	# Provides: vncboot
	# Required-Start: $remote_fs $syslog
	# Required-Stop: $remote_fs $syslog
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Start VNC Server at boot time
	# Description: Start VNC Server at boot time.
	### END INIT INFO
	
	USER=pi
	HOME=/home/pi
	
	export USER HOME
	
	case "$1" in
		start)
			echo "Starting VNC Server"
        	#Insert your favoured settings for a VNC session
			su - $USER -c "/usr/bin/vncserver :1 -geometry 1200x700 -depth 24"
			;;
	
		stop)
			echo "Stopping VNC Server"
			/usr/bin/vncserver -kill :1
			;;
	
		*)
			echo "Usage: /etc/init.d/vncboot {start|stop}"
			exit 1
			;;
	
    esac
       
	exit 0
       ```

4. Save this file as vncboot
5. Make the file executable:
	```bash
    chmod 755 vncboot
    ```

6. Enable dependency-based boot sequencing:
	```bash
	update-rc.d -f lightdm remove
	update-rc.d vncboot defaults
    ```

7. Reboot your Raspberry Pi and you should find a VNC server already started.



## Configure Pi:

1. Run in **Terminal**:
	```bash
    sudo raspi-config
    ```
	* Select Expand Filesystem.
	* Boot Option -> Desktop Autologin (may differ depending on Raspbian revision)
		* When Confidential information present:	Boot Option -> Desktop GUI, requiring user to login

2. Update the PI:
	```bash
    sudo apt-get update
	sudo apt-get dist-upgrade
	sudo apt-get clean
    ```

3. Security configuration:
	1. Setting Up a Firewall:
		* Insall Firewall:
			```bash
			sudo apt-get install ufw
            ```
		* Check the Status of UFW:
			```bash
            sudo ufw status verbose
            ```
		* Default rules for allowing all outgoing and denying all incoming connections.
			```bash
			sudo ufw default deny incoming
			sudo ufw default allow outgoing
            ```
		* Allow connections:
			```bash
            sudo ufw allow 22/tcp
            # or: sudo ufw allow ssh
            sudo ufw allow 5901/tcp
			# or: sudo ufw allow vnc

	2. Restart the Firewall:
		```bash
		sudo ufw disable
		sudo ufw enable
        ```

	3. **Help**: Finding some defined programs like ssh(in `sudo ufw allow ssh`):
		```bash
		ls etc/ufw/applications.d/
        ```
		In **ufw-loginserver** is stored ssh rule with **ports=22/tcp**
        
4. SSH settings:
	* Disable remote root login:
		1. `sudo nano /etc/ssh/sshd_config`
		2. Change the line **PermitRootLogin** to `no`


## Install Display Driver: 
* Read instructions from:
	http://www.waveshare.com/wiki/3.5inch_RPi_LCD_(A)
    
    
## Check **GPU** and **ARM CPU** temperature:
1. Display Raspberry Pi GPU temperature:
	```bash
    vcgencmd measure_temp
    ```
2. Display Raspberry Pi ARM CPU temperature:
	```bash
	cpu=$(</sys/class/thermal/thermal_zone0/temp)
	echo "$((cpu/1000)) c"
    ```

