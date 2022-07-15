# moxabuildnewfirmware
Setup for Moxa UC-8200 to make base configuration
1. SSH into device via ethernet using LAN 1 on device
   - Use Putty software
     - LAN 1 Default IP is `192.168.3.127`
     - Use Port `22`

   - ![putty](https://user-images.githubusercontent.com/108074645/175321878-c8aefea5-4a4e-4410-8502-54d3b70611ac.png)

2. Enter Username and Password
   - default username is `moxa`
   - default password is `moxa`

3. Create `edgeadmin` user
   1. Type `sudo useradd -m -G sudo -s /bin/bash edgeadmin` to add new user
   2. Enter moxa password `moxa'
   3. Type `sudo passwd edgeadmin`
     - Set password
     - Get password from Secret Server

4. Log out, and log back in with `edgeadmin` user

5. Disable Moxa user
   1. Type `sudo passwd -l moxa`

6. Update LAN 2 IP Address to be DHCP so it can be connected to the internet for updates and licensing 
   1. Type `cd /etc/network/`
   2. Type `sudo vi interfaces`
      - To update file, press 'Insert' on keyboard to insert text rather than replace.
      - edit file and once done press 'ESC' key and then ':wq' to save and exit file
   4. PUT PIC OF IP SETTINGS HERE ***************
   5. Type `sudo reboot` to reboot device
   6. Connect to device on DHCP network via LAN 2
      - This LAN needs internet if preforming upgrades
      
7. Set NTP Time Server Config 
   1. Type `sudo nano /etc/systemd/timesyncd.conf`
   2. Type `sudo vi interfaces`
      - edit file and once done press `ctrl + x` to save
      - Primary is `10.8.0.250`
      - Secondary is `10.65.0.250`
   4. PUT PIC OF NTP SETTINGS HERE ***************
   5. Type `sudo reboot`

8. Install Ignition Edge
   1. Type `sudo mkdir /usr/local/Ignition-Gateway` to create the Ignition directory
   2. Using a FTP/SSH software, copy Ignition Edge install from PC to device
      - Copy Ignition Edge install to `/home/edgeadmin`
   3. Type `sudo apt-get update` to get and apply updates to device
   4. Type `sudo apt-get install unzip` to install WinZip on device
   5. Type `sudo unzip Ignition-Edge-linux-armhf-32-8.1.17.zip -d /usr/local/Ignition-Gateway` to unzip and install
   6. Type `sudo rm Ignition-Edge-linux-armhf-32-8.1.17.zip` and press `Enter` to remove file
   7. Type `cd /usr/local/Ignition-Gateway`
   8. Type `sudo chmod +x *.sh` to make all ignition scripts executable
   9. Type `sudo ./ignition.sh install` To install as a system service that auto-starts Ignition on system boo
   10. Type `sudo ./ignition.sh start` to start the ignition gateway
   
9. Igntion Edge initial commission and modules
   1. Open a web browser and navigate to `http://192.168.3.127:8088`
   2. Create new user `edgesupport`
      - Password is in secret server
      - Leave ports default and start gateway.  The gateway take 5 or more min to start
   3. Log into Ignition
   4. Go to `Config -> Module`
   5. Click on Install or Upgrade Module at bottom of page
     - Install `MQTT-Transmission-signed_4_0_11.modl`
     - Install `EFM-Emerson-ROC-Driver-signed_4_0_11.modl`
     
   
  

6. Update Hostname
   1. Type `hostnamectl set-hostname <Enter HostName Here>` and press `Enter`
      - Host Name is `KR<Pad LDS ID>-Edge01`
      - Example 03-29 is `KR0329-Edge`
   2. Type `hostnamectl` and press `Enter` to view update
   3. Type `Sudo vi /etc/hosts` and press `Enter`

7. Update LAN IP Address'
   1. Type `cd /etc/network/` and press `Enter`
   2. Type `sudo vi interfaces` and press `Enter`
   3. PUT PIC OF IP SETTINGS HERE ***************
   4. Type `Sudo Reboot` and press `Enter` to reboot device
   5. Connect to device on DHCP network via LAN 2
      - This LAN needs internet if preforming upgrades







   
