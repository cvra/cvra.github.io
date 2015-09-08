---
layout: post
title: My notes on installing Arch Linux on my Lenovo T420s
tags: linux
author: Antoine
crosspost_site: Antoine's blog
crosspost_url: http://antoinealb.net/2014/12/05/archlinux-install.html
---
I recently decided I wanted to reinstall my Arch Linux system.
I have been running Arch on my Lenovo T420s for a few years now, and it has been working flawlessly.
However, I started working a lot with virtual machines, I was slowly running out of disk space.
Since I wanted to document my installation for future reference, and I thought it may be useful to someone, I decided to make a blog post about it.

## Why reinstall ? You could have migrated instead!
I wanted to split my data between a spinning hard drive for my data and virtual machines and a SSD for my host OS.
I could probably have migrated the data to the new drive, changed partition size and changed config just to make it work;
It would have been very complex and error prone.
Remember the first point of the Arch Way:

> Simplicity is absolutely the principal objective behind Arch development.

# Pre-install planning
Before wiping the hard drive, I took a few days to note what I was really using on my existing installation.
I researched those topics, mostly using the ArchWiki, to make sure the install would be painless.
After a few days of research, here is the list of what should work on my system in the end :

* Desktop environment. I am currently using GNOME3 with XMonad.
* Wifi. Wpa supplicant is not a lot of fun: I want NetworkManager to work.
* CUPS. At home I have a printer that I know is not working with CUPS on Linux, but at least at the university it should.
* Sound multiplexing between different applications.
* Web browsing.
* Usual programming environment.
* TRIM support for the SSD.
* Powersaving should be optimal-ish.
* My laptop has two different graphic cards. I should be able to disable the Nvidia one to avoid eating all the power.
* The webcam should work, even if I use it twice a year. It worked out of the box so I won't talk about it.
* VirtualBox and Vagrant.
* Music player. I decided to go with Clementine (a fork of Amarok 2.X) since a friend advised it to me.
* LibreOffice
* Latex
* Dual boot with Windows. I am not entierly sure this is really needed. Maybe I will just try working with more virtual machines.

I decided not to implement the use of the fingerprint reader for two reasons.
First it is a bit of a pain to support for Linux, and is prone to breaking.
Then because fingerprint are not a correct way to authenticate yourself on a system.
Since you leave them on almost everything you touch, they do not provide additional security.

Don't forget to do a full backup of your home partition before reinstalling.
I almost forgot to copy my SSH keys, which would have left me locked out of my servers.

# Installation
For the installation part of the process, I highly suggest reading Arch's beginner guide.
It is one of the most complete installation walkthrough I saw and it explains your different options very well.
I won't go into details that are in there, and focus on what is specific to my system.

## Partionning
After booting the installer USB key, the first step before installing is the partionning of the system.
I decided to stay on an MBR partition scheme, even if my system supports EFI and GPT, because I did not need more than 4 partitions per disk.

From my previous installation of Arch Linux, I learn a few things about partition sizing:

* 25G for `/` is not enough, at least for my usage.
* Swap partition is useless.
    Do a swapfile if you really need suspend or swap ram.
* 80G for a Windows partition seems enough.
* I used to put the pacman cache in a tmpfs that would get wiped at each boot.
    This is a bad idea;
    most problems are only seen after a reboot, when you cannot downgrade your packages.
* Having an empty partition or two that you can use to temporarily install a Linux/BSD to is useful.
    This problem can be mitigated by using virtual machines, but for sustained use they are less comfortable.

For SSDs it is recommended aligning your partitions on 1024kb blocks, but I think partition tools will do it for you anyways.

Finally I went with the following partition scheme for my main drive, which is a 160G Intel SSD:

1. Windows partition, 80G, NTFS
2. `/boot`, 200M, ext4
3. `/` on the leftover space (about 70G), ext4

My second disk is only made of `/home` right now, but I plan to put other testing partitions on it when I get a bigger drive.

## Bootloader installation
I decided to keep Syslinux as on my previous install.
I like it because it is quite powerful and way easier to setup than GRUB2.
However, it only supports MBR and BIOS booting;
if you use EFI you'll have to use another bootloader.

The installation was quite difficult, and it took me quite a while to understand what was wrong:local/rvm/log/1417816661_ruby-2.1.2/update_system.log’ for reading: No such file or directory
Requirements installation failed with status:
The BIOS would just loop on the boot device selection screen.
I tried redoing the installation and the formatting a few times, thinking it had something to do with the MBR.
This was by far the most time consuming part of the reinstall.
After a few tries, I realized my BIOS was set to try EFI boot first.
After changing it to BIOS emulation, it worked perfectly.
This is a bit weird, as it worked flawlessly and the settings haven't changed since.
I did not investigate it further and went on with my install.

## Basic setup: User, vim and sudo
After my first succesful boot, the first thing I did was installing a non priviledged user.
Doing your work as root is dangerous: even if you are not the target of hackers, any mistake can be a disaster.
I installed `sudo` and setup it to accept my user as temporary superuser, which was super easy thanks to the wiki.
The most important trick here is to never edit the config files directly but instead to use `visudo`.
Not doing it can result in you being locked out of your computer.

I also directly installed Vim, as I wanted to be able to edit my config files in my usual editor.
Since I store my vimrc and other configs on Github I just had to clone them to the appropriate location.


## Optimizing for SSDs
For now I simply replaced the `relatime` option with `noatime` on my SSD filesystems (in `/dev/fstab`).
This reduces write wear by not writing the access date each time the file is opened for reading.
I also added the `discard` option, which enables TRIM support on SSDs.

The next step would be to replace the kernel IO scheduler.
It should improve latency, but I am happy as is, so I prefer to stay on tried and true schedulers.

## Keyboard
For virtual consoles, `/etc/vconsole.conf`:
{% highlight bash %}
KEYMAP=fr_CH
{% endhighlight %}

## Trackpoint (TBD)
Put the following in `/etc/X11/xorg.conf.d/20-thinkpad.conf`

    Section "InputClass"
            Identifier      "Trackpoint Wheel Emulation"
            MatchProduct    "TPPS/2 IBM TrackPoint|DualPoint Stick|Synaptics Inc. Composite TouchPad / TrackPoint|ThinkPad USB Keyboard with TrackPoint|USB Trackpoint pointing device|Composite TouchPad / TrackPoint"
            MatchDevicePath "/dev/input/event*"
            Option          "EmulateWheel"          "true"
            Option          "EmulateWheelButton"    "2"
            Option          "Emulate3Buttons"       "false"
            Option          "XAxisMapping"          "6 7"
            Option          "YAxisMapping"          "4 5"
    EndSection

# AUR and Yaourt
For those of you who don't know, the Arch User Repository (AUR) is a collection of non-official packages that are built from source.
You can build them using an official tool called `makepkg` but it is not easy and forces you to track dependencies and updates by hand.
To avoid this, you can use what is called an AUR wrapper, and Yaourt is my favourite.

For security reasons the Arch official repositories won't contain such wrappers.
This means that to install one, we must either find a binary package somewhere or build it from the AUR.
Last time, I decided to use the binary package which can be found on the ArchLinuxFr repository.
But for this install, I went a little bit integrist and decided that adding a repository for a single package was overkill and dangerous.
Fortunately, Yaourt only has a few dependencies, and only one must be installed by hand from the AUR: `package-query`.
The command to install it is really close to the one used to install yaourt :

{% highlight bash %}
wget https://aur.archlinux.org/packages/ya/yaourt/PKGBUILD
makepkg
sudo pacman -U yaourt.xz # replace with name of the ouput file
{% endhighlight %}

For the software updates, don't worry: Yaourt will now update itself from source.


# Programming environment
This one does not require much explanation.
I will just list the few software I installed in case I need to do it again.

* `base-devel`. It contains a lot of useful tools such as `make` or `gcc`
* Vim. Actually I use `gvim` to have support for the X clipboard.
* Git
* Ag. It is in package `the_silver_searcher` package.
* Mercurial
* Screen
* Vagrant + VirtualBox

Virtualbox required a bit of configuration because it uses some kernel modules.
You need to put the following in `/etc/modules-load.d/virtualbox.conf`:

{% highlight bash %}
# VirtualBox modules
vboxdrv
vboxnetadp
vboxnetflt
vboxpci
{% endhighlight %}

# Wifi
Currently I am using NetworkManager;
it is pretty well integrated into GNOME, so there is not much to say about it.
I also still have to install `dnsmasq` to handle connection sharing.

# Powersaving
First of all, if you want to optimize your power saving, `powertop` is a real life saver.
Install it with `yaourt -S powertop`.

## Disabling Bluetooth
To save some power I disabled Bluetooth completely since I was not using it.

Put the following in `/etc/modprobe/modprobe.conf` :

{% highlight bash %}
# disable bluetooth to save power
blacklist btusb
blacklist bluetooth
{% endhighlight %}

## Kernel parameters
There are some kernel parameters which can be incredibly efficient at saving power.
Powertop can help you finding those.

For now I have put the following in `/etc/sysctl.d/powersaving.conf` :

{% highlight python %}
# Non maskable watchdog creates a lot of interrupts, disable it
kernel.nmi_watchdog = 0

# Reduces disk access
vm.dirty_writeback_centisecs = 6000

# Seems to be a bit magic but recommended
vm.laptop_mode = 5
{% endhighlight %}


## Laptop mode
We can install laptop mode tools by doing `yaourt -S laptop-mode-tools`.
We then enable it by doing `systemctl enable laptop-mode`.
Don't forget about blacklisting usb autosuspend on a few devices.
You should really do it on the mouse and keyboard, otherwise it just drives you insane.

To do this, first use `lsusb` to find the device ID.
For example for my WASD keyboard the relevant line is :

> Bus 001 Device 006: ID 04d9:0169 Holtek Semiconductor, Inc.

and the device ID here is `04d9:0169`.
Then put this in `/etc/laptop-mode/conf.d/runtime-pm.conf` :

{% highlight python %}
AUTOSUSPEND_USBID_BLACKLIST="04d9:0169"
{% endhighlight %}

Multiple blacklisted IDs are separated by spaces.
Finaly run `systemctl restart latop-mode`.


## PCI Power management
Here again, to enable it we simply follow the Arch Wiki:

`/etc/udev/rules.d/pci_powersave.rules` :

{% highlight python %}
ACTION=="add", SUBSYSTEM=="pci", ATTR{power/control}="auto"
{% endhighlight %}


# Sound
Sound is supported in two places:
First the kernel is responsible for providing an interface to the audio hardware.
This is done through something called ALSA.
The main issue with ALSA is that it can only listen to a single audio stream.
We use a userland daemon called PulseAudio (some will hate me) to do this.
To install it, simply run `yaourt -S pulseaudio` and it works.
It might even be installed by your desktop.

I had to install a few Gstreamers plugins for MP3 support, and honestly, I don't really know which one enabled it.
Just install everything in the base and ugly group.

# Web tools
I did not need anything fancy.
I just wanted Firefox and Chrome, in case one of them stops working.

The installation was simply `yaourt -S chromium firefox`

# Network tools
To put it in a Cyberpunk way : "All I needed was a few tools to make my way through the Net"
I wanted SSH and Mosh, to connect to servers.
I also wanted Irssi to chat on IRC.
Transmission is really the best client out there to download your favorite Linux distribution or Creative Commons music (wink wink).
Nmap is really useful for network diagnosis, so let's install it as well.

# Desktop environment

## Gnome 3
Very easy, just do `pacman -S gnome gnome-extra`.
I decided to use GDM because it is very well integrated and lightweight enough.
It is already in `gnome-extra`, so you only need to run `systemctl enable gdm.service`.

## XMonad
{% highlight bash %}
yaourt -S xmonad xmonad-contrib xmonad-gnome3 dmenu
{% endhighlight %}
The config files are hosted in Git so they will be easy to retrieve.
Just don't forget to run `xmonad --recompile` before starting the xmonad session.

## Media Transfer Protocol
The media transfer protocol is a USB device class used by my Nexus 7 to access its memory.
It was pretty easy to install : `pacman -S gvfs-mtp libmtp`.


# Optimus
Optimus support so far is simply disabling the Nvidia card.
I don't use it a lot (it may change if I do some OpenGL).

We simply install `bbswitch` and then put the following in `/etc/modprobe/modprobe.conf` :

{% highlight bash %}
# disable NVidia card at boot
options bbswitch load_state=0 unload_state=0
{% endhighlight %}

# To be done
Those parts are the one which I still need to do.
They look more like notes, because they are.

## CUPS
TBD

## Backup solution
Still to be done, but something in the spirit of :
{% highlight bash %}
rsync -aAXv /home/antoine/ /path/to/backup/
{% endhighlight %}

It also needs to handle different backup locations, because one backup disk will stay at home and the second will stay at EPFL.

Also, don't forget to exclude `~/.cache`, `~/.config` and maybe other (hidden) folders.

Should the script be unit tested ?

## Hard drive protection sensor
Still to be done, but here are my notes.

https://wiki.archlinux.org/index.php/Hard_Drive_Active_Protection_System

Looks a little bit outdate (rc.conf what?)

Apparently the current trend is to protect it via making uber resistant hard drives.

I should ask Joseph if he did anything to protect his drives.

# Non DKMS modules
Those modules are the one that needs to be updated after each major kernel update.
* `bbswitch`
* `vboxdrv`
