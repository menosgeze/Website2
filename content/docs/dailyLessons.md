# The life of a beginner - Still more Arch!

## Listing Partitions 
To see a tree of the partitions in your
hard drives and solid state drives, even
usb and other mounted devices, just type:

    lsblk
 
## Watching protected content from Streaming Websites.
(Aug-10-2019) Since I use Arch-linux 
(new to it) and Chromium (also new to it) 
in my desktop pc,  I was not able to play 
Netflix or Amazon. Luckily, 
I found that rather than installing
desktop applications for each of them, 
I could install WideVine from the AUR.

    yay WideVine

Not only, I did not have to set it up, 
it works perfectly, and I was able to
watch Netflix and Amazon movies again.

## GRUB for what?
(Aug-10-2019) For arch, it is recommended to use GRUB. 
At least my PACMAN, 
or teacher friend who introduced me
to Arch told me to look into. Since it is a 
core package, I did

    sudo pacman -S grub

which is currently in version 2.05. Then look at the Arch-wiki
[here](https://wiki.archlinux.org/index.php/GRUB)

The files to edit in order will be:
    /etc/default/grub
    /etc/grub.d

## Mounting Windows to Linux
While I was transitioning from Windows 
to Linux, I wanted to retrieve my
Windows personal files. One suggestion from 
my PACMAN was to use the package **ntfs-3g**
installed with:

    sudo pacman -S ntfs-3g

or with:
    
    yay ntfs-3g

because it is a core package. Then one
can just mount as usual:

    mount /dev/Windows_partition /dev/home_partition

In the tutorial
[here](<BS>https://www.tecmint.com/how-do-i-access-or-mount-windows-ntfs-partition-in-linux/)
Ravi Saive suggests to use the option **-t ntfs-3g** which by putting it before
the source Windows_partition limits the set of filesystem types to be used. 

<!--
## Fast History Search in the Z-shell
Look at [here](https://unix.stackexchange.com/questions/97843/how-can-i-search-history-with-text-already-entered-at-the-prompt-in-zsh)

Since I am using kitty per suggestion of my ZEN, rather than going to
**.zshrc** which is in my user directory **/home/username**, I will write these
commands in **.config/kitty/kitty.conf**
-->




