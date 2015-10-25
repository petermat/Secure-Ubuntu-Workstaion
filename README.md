# Secure-Ubuntu-Workstaion
How to secure Ubuntu linux workstation.
This is a list of tips and tricks you might apply to your laptop setup in order to make it more secure. 
Tested on Thinkpad T440s with Ubuntu 15.04.

# Software

## BIOS
* Disable boot from CD
* Disable boot from Pendrive
* Some laptops provide a utility to lock a hard disk with a password. These passwords are not the same as BIOS passwords. Set this password to prevent access to the hard-disk.
* Set a password for BIOS access

## Boot loader
* TPM (requires a laptop with TPM support): TrustedGRUB (requires laptop with TPM)
https://web.archive.org/web/20141221071438/http://www.grounation.org/index.php?post/2008/07/04/8-how-to-use-a-tpm-with-linux
~~http://www.grounation.org/index.php?post/2008/07/04/8-how-to-use-a-tpm-with-linux~~

## AppArmor
* Enable the default configuration
* Dropbox https://bugs.launchpad.net/ubuntu/+source/apparmor/+bug/811885
* https://bazaar.launchpad.net/~apparmor-dev/apparmor-profiles/master/files/head:/ubuntu/
* https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/AppArmorProfiles

## Full disk encryption
* (or) encrypted home directory if you’re worried about performance

## Firefox / Chromium
* Lastpass
 * Make sure to set the password generator to more than 14 alnum+special chars.
 * If you're using Google's two factor authentication you might be interested to know that LastPass can use that in the authentication process.
* HTTPS Everywhere from EFF
* Ghostery
* Disconnect
* User-Agent Switcher
 * Choose Firefox in Windows, which is the most used browser according to https://panopticlick.eff.org 
* Run the process under a different user as explained in https://grepular.com/Protecting_a_Laptop_from_Simple_and_Sophisticated_Attacks (Securing the Web browser section)
* Setup startpage (https://startpage.com/) as your default search engine
* **Run chrome as different user**


  My normal user account is called "mike". For Firefox I created a new user account called "mike.firefox". "/usr/bin/firefox" was merely a symlink to /usr/lib/firefox-6.0/firefox.sh so I replaced it with a shell script which runs:


  ```sudo -u mike.firefox -H /usr/lib/firefox-6.0/firefox.sh```


  I didn't want to be prompted for a password every time I tried to run firefox though, so I configured sudo to allow me to run that command without entering my password by adding this to the end of my /etc/sudoers (use the visudo command to do this)


  ```mike ALL=(mike.firefox) NOPASSWD: /usr/lib/firefox-6.0/firefox.sh```


  The "mike.firefox" user doesn't have access to the X display though when I'm logged in as "mike". To give it access I went to "System->Preferences->Startup Applications" and told it to run the command "xhost +SI:localuser:mike.firefox" when I log in. Now, when I run firefox, it runs as user mike.firefox instead. Something to look out for when you do this: Any command that firefox spawns, will it's self run as user mike.firefox. I noticed that when playing flash, there was no audio. This is because the mike.firefox user that I created did not have access to the audio device. To give it permission, I ran the command "adduser mike.firefox audio". I also set up permissions so that user "mike" could access "/home/mike.firefox/Downloads" as that is where Firefox will now download to. I symlinked /home/mike/Downloads/firefox to this directory for simplicity.


## Pidgin
* OTR for encrypted chats

## Truecrypt
In all cases truecrypt hidden volumes are recommended. Use the main volume for storing private pictures and the hidden volume for the real information you want to keep.
* If you decide to use dropbox, create a truecrypt disk and sync that file only. Real content should be hidden from dropbox.
* Backups need to be encrypted

## Sandboxing
* Disposable virtual OS (VMware, Qemu or similar) to run applications from “low reputation sources”

## GPG
* Generate the longest (bits) keys available
* Recommended tools are Thunderbird and Enigmail. In order to make your Thunderbird secure follow these steps:
 * “Tools > Options > Advanced > Encryption > Security Devices > Enable FIPS”
 * “Tools > Options > Privacy > Passwords”, choose “Use a master password to encrypt stored passwords”. You will then be prompted to choose a Master Password.
 * There are some methods to make your profile secure which are documented in mozilla's knowledge base article [Protecting the contents of the profile](http://kb.mozillazine.org/Protecting_the_contents_of_the_profile_-_mail)

## Hardware you might not use:
* Disable bluetooth
* Disable firewire
* Only enable USB after successful login (need confirmation on how this is done).

## Network
* Change IP address network range of your local network to something uncommon like 10.9.4.0/24
* Use a wired network
* Set static ARP entries for the router/access point you've got at home and work (prevents ARP spoofing)
 * arp -i eth0 -s 10.9.4.1 00:11:22:33:44:55
* Regularly change your public IP address. Easily done with a script that performs a couple of HTTP requests to your modem before your desktop computer is shutdown.
* [dnscrypt](https://github.com/opendns/dnscrypt-proxy) in combination with a DNS cache like unbound
* IPTables firewall
* Use macchanger to change the mac addresses for all interfaces on each boot
* Use arpwatch (apt-get install arpwatch) to detect ARP spoofing attacks. TODO: Create a script that parses the arpwatch messages in syslog and shows them via libnotify.

## Lock desktop
* Screen saver that locks desktop after 5 minutes of idle time
* [Disable guest account](http://www.ubuntugeek.com/ubuntu-tiphow-to-disable-guest-account-in-ubuntu-12-04precise.html)

## Antivirus and related software:
* rkhunter
* http://afick.sourceforge.net/
* ESET Antivirus

# Services
* Google two factor authentication
* Paypal two factor authentication

# Privacy
* Remove Amazon desktop search

# Habits
* Re-Install your workstation once every 8 months (useful?)
* Change your passwords every month
 * Ideally you’ll have to remember the LastPass and Linux user passwords only, so this shouldn’t be so terrible. 
* If you’re supposed to access a specific IP address only through a proxy / tor; create an iptables rule to DENY all traffic to that host.
* Use different users for each service and web application
* During your laptop installation, choose a generic hostname and username. Your shell prompt should look as generic as possible (ie. user@laptop:~$). This will partially avoid tracking based on those variables.

# Hardware

* 3M Privacy Filter for 14.1 Inch Standard Laptop (PF14.1)

# Paranoid
* Tor Browser Bundle
* Tor IM Browser Bundle
 * For encrypted and private (won’t know your IP) chat
 
 ----------------------------------------
 #Sources
 https://github.com/andresriancho/secure-ubuntu-desktop/wiki/Secure-Ubuntu-Desktop
 https://grepular.com/Protecting_a_Laptop_from_Simple_and_Sophisticated_Attacks