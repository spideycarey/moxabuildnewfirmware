# moxabuildnewfirmware
Setup for Moxa UC-8200 to make base configuration
1. SSH into device via ethernet using LAN 2 on device
   - Use Putty software
     - LAN 2 Default IP is `192.168.4.127`
     - Use Port `22`
   
   ![lan2](https://user-images.githubusercontent.com/109390971/182858043-242a11da-11f1-40d3-b667-8326291d9cc2.png)

2. Enter Username and Password
   - default username is `moxa`
   - default password is `moxa`

3. Create `edgeadmin` user
   1. Type `sudo useradd -m -G sudo -s /bin/bash edgeadmin` to add new user
   2. Type `sudo passwd edgeadmin`
     - Set password
     - Get password from Secret Server

4. Log out, and log back in with `edgeadmin` user
   - To do this, you must close putty and reopen the connection in putty

5. Disable Moxa user
   1. Type `sudo passwd -l moxa`

6. Update LAN 2 IP Address to be DHCP so it can be connected to the internet for updates and licensing 
   1. Type `sudo vi /etc/network/interfaces`
      - To update file, press 'Insert' on keyboard to insert text rather than replace.
   2. ![net](https://user-images.githubusercontent.com/109390971/182859623-0f017c34-1347-422e-bc2c-52115882aadc.png)
   3. Edit file to look like above and once done press 'ESC' key and then ':wq' to save and exit file
   4. Type `sudo reboot` to reboot device
   5. LAN 2 can stay connected to PC, but also plug LAN1 into internet to obtain updates to device.
      
7. Set NTP Time Server Config 
   1. Type `sudo nano /etc/systemd/timesyncd.conf`
   2. Type `sudo vi interfaces`
      - edit file and once done press `ctrl + x` to save
      - Primary is `10.8.0.250`
      - Secondary is `10.65.0.250`
   4. ![NTP](https://user-images.githubusercontent.com/109390971/182207266-fd71b35f-93e4-4d0a-93f8-514b0a5a5589.png)
   5. Type `sudo reboot`
   
8. Install Moxa Ignition DEB packages (Must be in the order below)
   These packages are installed so when the edge device reboots, the license key is not lost
   1. Type `sudo dpkg -i /home/edgeadmin/uc8200-modules_4.4.176+1.0.11+tpmi1_armhf.deb`
   2. Type `sudo dpkg -i /home/edgeadmin/uc8200-kernel_4.4.176+1.0.11+tpmi1_armhf.deb`

9. Install Ignition Edge
   These instructions are based of the Readme file included in the install
   1. Type `sudo mkdir /usr/local/bin/ignition` to create the Ignition directory
   2. Type `sudo chown edgeadmin /usr/local/bin/ignition` to set the owner of the directory
   2. Using a FTP/SSH software, copy Ignition Edge install from PC to device
      - Copy Ignition Edge install to `/home/edgeadmin`
   3. Type `sudo apt-get update` to get and apply updates to device
   4. Type `sudo apt-get install unzip` to install WinZip on device
   5. Type `sudo unzip /home/edgeadmin/Ignition-Edge-linux-armhf-32-8.1.18.zip -d /usr/local/bin/ignition` to unzip and install
   6. Type `sudo rm /home/edgeadmin/Ignition-Edge-linux-armhf-32-8.1.18.zip` to remove file
   7. Type `cd /usr/local/bin/ignition`
   8. Type `sudo chmod +x *.sh` to make all ignition scripts executable
   9. Type `sudo ./ignition.sh install` To install as a system service that auto-starts Ignition on system boo
   10. Type `sudo ./ignition.sh start` to start the ignition gateway
   
10. Ignition Edge initial commission and modules
   1. Open a web browser and navigate to `http://192.168.4.127:8088`
   2. Create new user `edgesupport`
      - Password is in secret server
      - Leave ports default and start gateway.  The gateway take 5 or more min to start
   3. Log into Ignition
   4. Go to `Config -> Module`
   5. Click on Install or Upgrade Module at bottom of page
     - Install `MQTT-Transmission-signed_4_0_11.modl`
     - Install `EFM-Emerson-ROC-Driver-signed_4_0_11.modl`

** These next steps are from a Moxa git**The intructions below are based off that**
https://github.com/Moxa-Linux/resize-image

11. Setup another Linux PC to close to - I used raspberry pi
   1. Need to get GIT package
      - `sudo apt install git`
   2. Then need moxa resize-image package
      - `sudo git clone https://github.com/Moxa-Linux/resize-image`
      
11. Create clone of device
   1. Create clone to another linux (See Moxa Git for details)
      - `sudo dd if=/dev/mmcblk2 | ssh pi@192.168.112.146 dd of=/tmp/uc8100_dump.img`
      - To run the above command, you need to be logged in with sudo or else it wants two passwords at once and that created errors.  To prevent this, i ran the ignition status command with sudo first, that way credentials were updated
      - This is done from moxa
   2. Then log into your linux pc and
      - `sudo git clone https://github.com/Moxa-Linux/resize-image`
      - `cd /resize-image`
   3. `sudo ./resize-img C /tmp/uc8100_dump.img` to resize the image
   4. Use filezilla to transfer 'resize.img' to pc
      - File will be at `/resize-image`
  
  
  
  

6. Update Hostname
   1. Type `hostnamectl set-hostname <Enter HostName Here>` and press `Enter`
      - Host Name is `KR<Pad LDS ID>-EC01`
      - Example 03-29 is `KR0329064-EC01`
   2. Type `hostnamectl` and press `Enter` to view update
   3. Type `Sudo vi /etc/hosts` and press `Enter`

7. Update LAN IP Address'
   1. Type `cd /etc/network/` and press `Enter`
   2. Type `sudo vi interfaces` and press `Enter`
   3. PUT PIC OF IP SETTINGS HERE ***************
   4. Type `Sudo Reboot` and press `Enter` to reboot device
   5. Connect to device on DHCP network via LAN 2
      - This LAN needs internet if preforming upgrades







   
