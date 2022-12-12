---
layout: "../../layouts/BlogPost.astro"
title: "Cyberpatriot Linux Checklist"
description: "This is a very cool linux checklist that you can use for Cyberpatriot, keep in mind that it is not completed"
image: "/penguin.jpg"
---

# Linux Checklist

Welcome to this very cool Linux checklist!

This is more of an updated version of Ardy’s checklist, some things do not work, or are not very clear on that checklist, so I thought of making this one for clarity.

This checklist will have bulletin points with things that you need to do, each block that looks like `this` is a command, which you can run on your system. There may or may not be a comment under each command, explaining the command, if there is no comment, then the command should be self explanatory, for example: `sudo apt install something` , I don’t have to explain that, you should know what it does 😁.

When a bulletin mark tells you to edit a file, either run `gedit [filepath]` or `nano [filepath]` to edit the file.

So!

Here are some checklist stuff!

- Secure Firewall (UFW)
  - `sudo apt install ufw`
  - `sudo ufw enable`
  - `sudo ufw logging full`
    - This will turn on the highest level of logging for UFW
  - Add or remove ports which should not be accepting traffic, do this via
    - `sudo ufw allow [port]`
    - or `sudo ufw deny [port]`
- Lightdm config
  - Edit the lightdm config file, the path is **”/usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf”**, and add these settings to the file:
    - allow-guest=false
    - greeter-hide-users=true
    - greeter-show-manual-login=true
    - autologin-user=none
- Userconf
  - Configure users either via GUI or CLI - Read the README file for info on users, such as which user should have admin, shouldn’t and etc.
    - For CLI:
      - `sudo adduser -m [username] -g [groupname]`
        - -m creates a home dir, and -g specifies what group the user would go into, if you do not want to put the user into a group, then remove -g, if the user is supposed to be an administrator, then add the sudo group to the [groupname] list
      - `sudo userdel [username]`
- Password stuff
  - Change insecure passwords for users
  - PAM - _be very careful with PAM, it can lock you out of your account/other accounts._
    - `sudo apt install libpam-cracklib`
    - Edit \***\*\*\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*\*\***\*\*\***\*\*\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*\*\***“/etc/pam.d/common-password”\***\*\*\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*\*\***\*\*\***\*\*\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*\*\*** and paste the contents of [this](https://github.com/maytees/cypat-ubuntu-script/blob/master/preset_files/common-password) file into it
    - Edit \***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***“/etc/pam.d/common-auth”\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\*** and paste the contents of [this](https://github.com/maytees/cypat-ubuntu-script/blob/master/preset_files/common-auth) file into it
  - `chage -l`
    - Changes password expiry data
  - Edit **\*\*\*\***\*\***\*\*\*\***\*\***\*\*\*\***\*\***\*\*\*\***“/etc/login.defs”**\*\*\*\***\*\***\*\*\*\***\*\***\*\*\*\***\*\***\*\*\*\*** and paste [this](https://github.com/maytees/cypat-ubuntu-script/blob/master/preset_files/login.defs) file into it
- Updates
  - `sudo apt update`
  - `sudo apt dist-upgrade`
  - `sudo apt upgrade`
  - Put in the most strict password settings on the updates GUI
- Audits
  - `sudo auditctl -e 1`
  - TODO: /etc/audit/auditd.conf
- Services
  - Find services which you do not think belong there via bum
    - `sudo apt install bum`
    - To remove a service completely, including its files, run `sudo apt --purge autoremove [servicename]`
- SSH Config
  - Do this stuff if the README specifies that the system should have SSH, if it doesn’t allow it, then do the opposite of this stuff!
  - Edit \***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\***“/etc/ssh/sshd_config”\***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\***\*\*\***\*\*\*\*\*** and paste the contents of [this](https://github.com/maytees/cypat-ubuntu-script/blob/master/preset_files/sshd_config) file into it
  - Allow port 22
    - `ufw allow 22`
    - `ufw allow ssh`
- Desktop Remote Sharing
  - If the README does not allow this, then disable it via the GUI, just search for “remote desktop”
- VSFTPD
  - Same thing with SSH, if the README does not allow this stuff, don’t do it, and reverse it.
  - Edit “**\*\*\*\***\*\*\*\***\*\*\*\***/etc/vsftpd.conf”**\*\*\*\***\*\*\*\***\*\*\*\*** and paste the contents of [this](https://github.com/maytees/cypat-ubuntu-script/blob/master/preset_files/vsftpd.conf) file into it.
- Apparmor
  - I’m not sure what this does, probably just hardens apps, but run these two commands:
    - `sudo apt install apparmor-utils`
    - `sudo aa-enforce /etc/apparmor.d/user.bin.*`
- Firefox
  - Go to the Firefox settings and put on the strictest settings. (do stuff like HTTPS only mode, and safety with cookies, and etc)
- Global permissions
  - `chmod 640 /etc/shadow`
  - `chmod 640 /etc/group`
  - `chmod 640 /etc/passwd`
  - Check other important files
- Remove bad apps
  - To find bad apps, look at your installed apps in your Gnome store, and look at what they’re about. Do not remove any apps that the README tells you not to remove, make sure that you read the README thoroughly to understand what your boss wants.
  - Remove games
  - Here are a list of known bad apps, run `sudo apt remove [name]` make sure to press tab to check for the completed name _(Thanks Gido)_
    | ophcrack
    medusa
    Minetest
    Wireshark
    Netcat
    Freciv
    WorldForge
    hunt
    dsniff
    Endless Sky
    ettercap
    Kismet
    nmap
    p0f
    TCSpray
    dnsniff
    NBTScan
    John
    hydra-gtk
    hydra |
    | --- |
- Check Crontab for suspicious activity
  - `sudo ls /etc/cron. *`
- Samba
  - `sudo apt remove samba-common samba-common-bin && sudo apt purge samba samba-common samba-common-bin`
  - `sudo rm -r /var/lib/samba/printers/x64`
  - `sudo rm -r /var/lib/samba/printers/W32X86`

This is a work in progress, meaning that this is not completed, check [`https://nas.seahawkcyber.com/linux_master.txt`](https://nas.seahawkcyber.com/linux_master.txt) for a completed list of stuff (though don’t rely on it to get you 100 points, that won’t work, maybe on the first round).
