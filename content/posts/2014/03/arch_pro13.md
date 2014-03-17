Title: Arch on Vaio Pro 13
Date: 2014-03-17

# (SVP1321BPXB) -- In Progress

I am very much a newbie in Arch and have only some experience with Linux. This guide only serves as a log so I can repeat later if needed. If it helps anybody then that's even more awesome.

# System

## Systemd user

Follow the instruction [here](https://wiki.archlinux.org/index.php/Systemd/User#Using_.2Fusr.2Flib.2Fsystemd.2Fsystemd_--user_To_Manage_Your_Session) to set up systemd for user that is later needed to start dropbox daemon when login to certain user.

### Create a systemd service for current user

Follow the instruction [here](https://wiki.archlinux.org/index.php/Dropbox#Run_as_daemon_with_systemd)

I modified "After=xorg.target" to "After=local-fs.target network.target"
"ExecStart=/home/your_user/.dropbox-dist/dropbox" to "ExecStart=/home/andy/bin/dropbox start". I think this will eliminate the need for starting graphical interface before starting dropbox.

## Automount



## Mounting NTFS

I got some error when trying to mount My Passport formated in NTFS. Apparently, Arch does not support NTFS out of the box

    sudo mount /dev/sdb /mnt/Passport
    mount: wrong fs type, bad option, bad superblock on /dev/sdb,
	   missing codepage or helper program, or other error

	   In some cases useful info is found in syslog - try
	   dmesg | tail or so.

I got the same error trying to force type with -t option. lsblk -f recognize the filesystem as NTFS. Of course I made the silly mistake of putting /dev/sdb instead of /dev/sdb1 too.

### Install ntfs-3g

     sudo pacman -S ntfs-3g

Now the mounting with command is successful.Apparently I did not safely close the passport on Windows.

    sudo mount /dev/sdb1 /mnt/Passport

    The disk contains an unclean file system (0, 0).
    The file system wasn't safely closed on Windows. Fixing.

## Audio

I have to install alsa-util, pulseaudio, pavucontrol. After fiddling with pavucontrol to select the right output, got audio to work.

## udev(automatic mounting, battery)

Built to systemctl by default in Arch

### Automount

Install gvfs and polkit with authentication agent as mentioned [here](https://wiki.archlinux.org/index.php/Polkit#Authentication_agents) (for mount without password)

# Appearance

## Console font

The console font was way too small for me. So I followed the instruction on arch wiki. This is a summary

    sudo pacman -S terminus-font

Then change the font by change FONT variable in /etc/vconsole.conf(create if not exist)
     FONT=ter-128b
More terminus font can be found at /usr/share/fonts/local/ 
    

# Application

I am very nicely surprised that installing emacs24 go much smoother than in Debian. No adding third-party repo. And pacman rocks

## dmenu-xft(from AUR)

Can also install dmenu in Official Repository but does not support xft font

Create a file in PATH named launch_dmenu with contents

       #!/bin/sh
       exec $(dmenu_path | dmenu_run -b -nb '#202930' -sb '#285481' -sf '#FFFFFF' -fn 'Droid Sans Mono-16')

Using Droid Sans Georgian makes dmenu shows all boxes for some reasons

## Openbox or xfce

### openbox

#### obconf, obmenu, conky, lxappearance

In lxapperance I set default font to Droid Sans Georgian 16

In obconf set Theme to Artwiz-boxed; set font under Appearance to Droid Sans Georgian (bold/Italic depending item; window title 14, the rest 16)

#### Desktop panel - tin2

Start by running tin2

Add to ~/.config/openbox/autostart

    tint2 &

Create a keyboard shortcut entry in openbox ~/.config/openbox/rc.xml. If file does not exist, initialize the file with /etc/xdg/openbox. Add this entry under <keyboard> <!-- Keybindings for running applications -->. Google openbox keyboard for detail about binding.

    <keybind key="W-space">
      <action name="execute">
	<execute>/home/andy/bin/launch_dmenu</execute>
      </action>
    </keybind>

### xfce

Keyboard shortcut can be bound  through xfce keyboard app.

## rsync, p7zip(for handling archive file) emacs thunar

    
## flashplugin chromium firefox

Even after installing flashplugin(need to login to openbox again), youtube plays in chromium but not firefox(give me error after 1-2 second of playing), google music stucks at 0 and pandora switches song continuously. I am guessing it has something to do with flash


## hal

Installing hal-flash(smaller) only enable playing google play movie



## Dropbox

I installed dropbox by downloading tar ball from here [link](https://aur.archlinux.org/packages/dropbox/)

Then follow the isntruction for [installing package from AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages)

Do the same thing for thunar-dropbox for thunar support

I donwload the cli from [dropbox page](https://www.dropbox.com/download?dl=packages/dropbox.py) since AUR is out-of-date. I put dropbox.py in my path. Then run to enable execute perm:

  chmod +x dropbox.py

Arch apparently comes with python3 by default so I have to edit the shebang line in dropbox.py from "#!/usr/bin/python" to "#!/usr/bin/python2" and rename the file to dropbox for convenience. type dropbox will run ok at this point.

After this, i tried to type dropboxd in the terminal but nothing happens. I tried relogin to openbox but still nothing. So I try dropbox start and the login window finally popped up.

## git and openssh

Use this [guide](https://help.github.com/articles/set-up-git) to set up git

    git config --global user.name "Your Name Here"
    # Sets the default name for git to use when you commit
    git config --global user.email "your_email@example.com"
    # Sets the default email for git to use when you commit
    git config --global credential.helper 'cache --timeout=3600'
    # Set the cache to timeout after 1 hour (setting is in seconds)

Install openssh to set up [ssh-key](https://help.github.com/articles/generating-ssh-keys)

## Installing lyx livetex-most 

## Java Intellij android-sdk android-skd-build-tools android-sdk-platform-tools

All android stuffs can be found in AUR(this way it will take care of all 32bit dependency comparing to getting from Google directly)

Oracle java can be found in AUR (jdk)

After installing by following AUR wiki instruction(with makepkg -s and pacman -U), set java path by putting the following in .bashrc.

      export PATH=$PATH:$JAVA_HOME/bin
      ## export JAVA_HOME JDK ##
      export JAVA_HOME="/opt/java/"

Intellij is available in community repository(woo-hoo) and can be installed by installing. Somehow running idea.sh in dmenu does not do anything so I created a bash file named "idea" in my path.

## pdf reader

I choose okular - hefty with KDE but still the best. It is kdegraphics-okular package in arch repository.

## libreoffice

## yaourt (for easy access to AUR)

## python3 pip pelican

Install python-pip then follow instruction from here to [install pelican](http://docs.getpelican.com/en/3.1.1/getting_started.html)

Use pip to install markdown and virtualenvwrapper