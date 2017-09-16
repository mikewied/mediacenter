
#Preparing the software

###Prepare Raspian
1. Download the Raspian Stretch (with Desktop) from the Raspberry Pi website
2. Add a file named "ssh" to the root directory of the Raspbian image. This will allow you to ssh into the Raspberry Pi once booted.
3. Edit the config.txt file in the root directory of the Raspian image and add or uncomment the following lines:

config.txt settings:

	dtoverlay=hifiberry-digi
	dtoverlay=lirc-rpi
    dtoverlay=lirc-rpi,gpio\_in\_pin=23

Copy the Raspian OS to a micro SD card. The easiest way to do this is to use software called Apple Pi Baker. Place the card into the Raspberry Pi and boot it for the first time.

### Updating OS Settings

1. SSH into the Raspberry Pi as the pi user and change the password. This can be done by running raspi-config.
2. Run "sudo reboot" to restart the Raspberry pi to allow all changes to take affect.

### Setup Bonjour

Bonkour allows your media center to be discovered by other computers. This is nice because you can use a Domain Name instead of typing in a (potentially changing) IP Address each time you want to connect to your media server.


### Add ability to use the IR receiver

To install software to allow the Raspberry Pi to use the McIntosh remote that came with the McIntosh D100 run the following commands:

1. sudo mod probe lirc_rpi
2. sudo apt-get install lirc
3. Edit the /etc/lirc/lirc_options.conf file and change the "driver" setting from "devinput" to "default".
4. Overwrite the contents of the /etc/lirc/lircd.conf file with the contents of the mcintosh_d100.conf file.

### Install mopidy

Mopidy is the service that is used to stream music and also provides the web interface that allows you to easily play music. Run the commands below to install and configure it. Note that you need a Spotify Premium account and also need to register as an application developer in order to use the spotify service. There are a few blank fields in the mopidy.conf file that need can be filled in once you do these things. If you don't want to use Spotify then you can set the enabled field to false in the mopidy.conf file.

1. wget -q -O - https://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
2. sudo wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/stretch.list
3. sudo apt-get update
4. sudo apt-get install mopidy-spotify
5. sudo apt-get install qt5-default qt5-qmake emacs mpd mopidy mopidy-local-sqlite
6. sudo pip install Mopidy-Iris
7. sudo pip install --upgrade Mopidy-Iris
8. sudo pip install spotipy
9. Copy the mopidy.conf file to /etc/mopidy.conf
10. sudo systemctl disable mpd
11. sudo systemctl enable mopidy

### Auto mount the hard disk

To mount an external hard disk you can run the command below. Note that the PARTUUID is unique to each hard disk. You should replace this value with the PARTUUID of the hard drive you are using. Also, if you screw these step up your Raspberry Pi may not be able to boot.

1. sudo mkdir /media
2. echo "PARTUUID=cf34d76f-1531-4b95-8bce-63431083405e /media vfat defaults 0 0" | sudo tee -a /etc/fstab


### Setup nginex

TODO

### Scan the music in the media directory

Add your music the the /media directory and ensure that all files have the correct permissions so that they can be read by mopidy. Setting the owner and group to root and the permisisons for each file to 755 should work. The run the command below to scan your files so that mopidy knows about them.

    sudo mopidyctl local scan

### Setup the TV Display

TODO