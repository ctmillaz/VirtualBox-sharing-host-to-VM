### How to tell if GuestAdditions are installed:
``` lsmod | grep vboxguest ```

### Sometimes it doesn't show that it is mounted on the Desktop:
### You can just rerun the VBoxLinuxAdditions.run which is located in the VBox_GAs_5.2.12 (Or which ever version of VBox Guest Additions you have)

### While in the Virtual Machine, open a terminal and run the command echo $(whoami)
### We want to find out the username in the virtual.
### We need to add our username to the virtual box shared folder group with:
``` sudo usermod -aG vboxsf $(whoami) ```

### We then want to create a share folder on our host machine and in virtual machine.
### I created a directory on the host machine called C:\share and a directory in /home/<username>/share/.
### My username is dev-user so I created a share directory in /home/dev-user/ called share, which looks like this /home/dev-user/share

### While logged into the virtual machine, in the top left corner, I clicked on Devices, Shared Folders, Shared Folder Settings.
### Click on Machine Folders, then click the plus symbol which is to the right of the word Access.

### There are 2 lines Folder Path and Folder Name.  On Folder Path click the drop down and click Other.
### Now you need to find the share folder on your host computer (which is likely windows) and click select folder.

### At this point the virtual machine knows where our shared folder is for windows.

### We can mount share which is now known from adding it to the Machine Folder, to the share folder on our virtual machine with this command:

sudo mount -t vboxsf share /home/dev-user/share

### We need to link the host and shared folder with the command:
``` ln -s /media/sf_share /home/dev-user/share  not C:/share ```


### I don't know if this is needed:

### add to /etc/fstab:
``` share /home/dev-user/share                      vboxsf  defaults        0 0 ```

### example of what the above should look like:
``` <name_of_share> /path/to/mountpoint vboxsf default 0 0 ```


----------------------------------------------------------------------------------------------------

# How to test if port works on host if everything is firewalled
### Open Windows powershell
Test-NetConnection -Computername localhost -Port 9300

###Compare against a port that you know is blocked.



# Other ways to test if guest is firewalled, these are commands to run while in guest VM.
### Verify status of FirewallD and Iptables and disable them as well.

`systemctl status firewalld`
`systemctl disable firewalld`
`systemctl stop iptables`

### Also check services name and port:

`nmap -sT -o localhost`
