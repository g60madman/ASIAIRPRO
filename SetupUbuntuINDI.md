WHO IS THIS FOR: While this is written for the ASI Air Pro user base these same steps will work for any Rasberry Pi board 3 or 4. You simply need to skip the installation of the indi-asi-power that KenS from our very own CN wrote.



PREFACE: So I wanted to try and run Ubuntu 20.04.2 LTS Server 64bit on the ASI Air Pro (Ubuntu 20.10 Server will not work and is not supported by INDI currently) and build all the modules by hand instead of doing the 32bit install of AstroBerry and run this all from the ASIAir Pro



PRO/CON: The pro for this over Astroberry is the AAP/Pi (ASI Air Pro / Raspberry Pi) now has more horsepower as you are not bloating it up with a full GUI desktop and can use 100% of the RAM as it's a full 64bit OS and not 32bit. Also, you do not need VNC or RDP or any nonsense to connect to the AAP/Pi instead you simply use SSH to talk to the AAP/Pi to run updates from time to time, a web browser to update drivers on the INDI Web Manager for your Astro rig hardware (telescope, camera, focuser, filter wheel, etc), and install KStars on a PC for Windows, Mac, or Linux so you can control your Astro rig from the comfort of your couch. Or you can use the amazing Android application Telescope.Touch which is almost like using the ASIAir Pro mobile application!! https://play.google.com/store/apps/details?id=io.github.marcocipriani01.telescopetouch&hl=en_US&gl=US



The con to this tutorial is you need to be a little tech-savvy.



TUTORIAL FYI: This is strictly a building tutorial. I will create a video suite using the Telescope.Touch application in another post. I set this up from a Ubuntu 20.04.2 Desktop and did not have access to a keyboard/mouse/monitor for the ASI Air Pro. This probably can be done from a Windows PC or Mac as well as using Putty for Windows or Terminal for Mac but I prefer using Linux.  



Use the following steps to install 20.04.2 LTS Server 64bit (RPI 3/4/400) 64-bit server OS for arm64 architectures.



https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview



As the instructions state, I needed to reboot two times before the AAP/Pi connected to my WiFi make sure to wait 2 minutes on the first boot before restarting. Once I had access through ssh, I changed the ubuntu password. Then I ran the following commands:



sudo apt update

sudo apt upgrade



If you run into an error:

Waiting for cache lock: Could not get lock /var/lib/dpkg/lock-frontend. Simple reboot the unit and try the following command again



sudo apt upgrade

sudo reboot now



Once the unit reboots it's time to install packages through ssh



Install INDI Drivers, GSC, and INDI Web Manager

sudo apt-add-repository ppa:mutlaqja/ppa

sudo apt update

sudo apt-get install indi-full gsc python3-pip python-is-python3

sudo -H pip3 install indiweb



Install PigPiod / ASI Power (skip this step if you do not have the ASI Air Pro) KenS from CN created this package. It's still in beta but does work!

sudo apt -y install pigpio-tools

sudo apt-get update

sudo apt-get install apt indi-asi-power

sudo systemctl enable pigpiod.service

sudo systemctl start pigpiod.service



Configure INDI Web Manager to start on boot

wget https://raw.githubusercontent.com/knro/indiwebmanager/master/indiwebmanager.service

sed -i 's/pi/ubuntu/' indiwebmanager.service

sudo cp indiwebmanager.service /etc/systemd/system/

sudo chmod 644 /etc/systemd/system/indiwebmanager.service

sudo systemctl daemon-reload

sudo systemctl enable indiwebmanager.service

sudo reboot



Configure INDI Web Manager

Once rebooted configure your driver profile for all your hardware

http://ipaddress:8624/



INSTALL KStars

At this point, we simply need to install Kstars on your PC, Mac, Linux or Android device and get cooking. I would recommend checking out YouTube or other online documentation to configure it

https://edu.kde.org/kstars/



Keeping your AAP/Pi Update

From time to time, you will need to update your AAP/Pi. Simply run the following commands to do that. This will keep the OS, INDI and PHD2 updated. It's also recommended once you have everything working to backup your AAP/Pi



sudo apt-get update

sudo apt upgrade

sudo reboot now



EXTRA

Install PHD2 with multi-star alignment on AAP/Pi

sudo add-apt-repository ppa:pch/phd2

sudo apt-get update

sudo apt-get install phd2 phdlogview



Configure PHD2 to work through terminal

sudo apt-get install xorg openbox

sudo apt-get upgrade

sudo reboot



Configure PHD2

Once the unit reboots log in with SSH using the following command to allow X11 to work through terminal



ssh -X ubuntu@<IPaddress>

phd2



Configure the server profile and your hardware. Once it connects to the camera make sure to go to Tools and check "Enable Server"



Normal Startup

Boot up your AAP/Pi and SSH into the unit and start PHD2



ssh -X ubuntu@<IPaddress>

phd2



When you are done with your session simple shut down the unit



sudo shutdown now
