# music-daw
The porpuse of this project is to create a shell script to facilitate the process of configuring Ubuntu or Mint operating systems to use the low latency and real-time kernel, transforming your computer into a digital audio workstation (DAW).

You just need to download the .deb file that will probably be saved in the Downloads directory.
Open your terminal, go to where .deb is and run:
```
sudo dpkg -i music-daw.0.1.0.deb
```
After installing all you need to do is run the main script, with your own user, not root.
```
music-daw
```

# The “music-daw” Script
  

1- The The first part of the script includes the user in the audio group,in a way that the user has access to the permissions of this group

2- The script reduces /etc/sysctl.conf  the parameter vm.swappiness value, which determines the moment when the system starts using its swap partition. This ensures that audio processes will preferably use RAM.

3- Another alteration takes place on the  max_user_watches, which defines the maximum amount of files that the system can monitor with the inotify (which is part of the kernel), altering the command listed bellow: 
```
sed -i '$afs.inotify.max_user_watches = 524288' /etc/sysctl.conf
```
4- The script checks if there is a low latency kernel installed. If it does, it alters the /boot/config-`uname -r` in order to ad the RT mode. This is achieved by modifying the following parameters:
```
CONFIG_HZ_1000=y
CONFIG_HZ=1000
CONFIG_PREEMPT_RT_FULL=y
CONFIG_PREEMPT=y
```
If no low latency kernel has not been located, the script proceeds with its installation.

5-Then the script includes the KX Studio’s repositories in your system. This procedure will allow the user to install both software and plugins available at the repository. This portion of the installation does not alter any function of the OS. If at any moment, errors occur due to any modification in the KX Studio’s repository, the script will inform the user but continues the remaining portion of the installation normally. 

6- In turn, the  cpufrequtils indicator-cpufreq package is installed. This will allow the user to make use of the CPU at its maximum performance.

7- One of the last tasks performed is the download and installation of the realtimeconfigquickscan Perl script that allows the user to check if all the parameters are ready to work in RT mode.

sudo git clone git://github.com/raboof/realtimeconfigquickscan.git

8- The installation of the jackd qjacktl packages are the last steps of the whole process. During this time, it is given the user the choice to warrant priority access and access to memory for the audio group. 
